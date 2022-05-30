필요한 AWS 리소스를 생성하고 설정했다. 이제 Node.js로 웹 서버를 작성해보자.   
다음 예제 코드는 필자의 GitHub 저장소에 있는 예제 코드를 받아서 사용한다.   
  
app.js   
```
var express = require('express')
  , Sequelize = require('sequelize')
  , redis = require('redis')
  , EC2Metadata = require('ec2metadata')
  , http = require('http')
  , fs = require('fs')
  , app = express()
  , server = http.createServer(app)
  //express에 socket.io를 연결한다.   
  , io = require('socket.io').listen(server);
  
  
//ElastiCache 캐시 노드 엔드포인트 주소(Redis), RDS DB 인스턴스 엔드포인  
//트 주소(MySQL)와 연결 설정은 여러분들이 생성한 AWS 리소스의 정보를 입력  
//한다.
var redisEndpoint = {
  host: 'redis url',
  port: 6379
};

var rdsEndpoint = {
  host: 'rds url',
  port: 3306
};

// Redis Pub/Sub
//Publiser용 Redis 클라이언트와 Subscriber용 Redis 클라이언트를 따로 생성  
//한다.  
var publisher = redis.createClient(redisEndpoint.port, redisEndpoint.host);
var subscriber = redis.createClient(redisEndpoint.port, redisEndpoint.host);

//MySQL DB 이름, 계정, 암호
//좌석 정보 테이블을 정의하고, 테이블을 생성한다.   
var sequelize = new Sequelize('exampleticket', 'admin', 'adminpassword', {
  host: rdsEndpoint.host,
  port: rdsEndpoint.port,
  maxConcurrentQuries: 1024,
  logging: false
});

//MySQL DB 테이블 정의
var Seat = sequelize.define('Seat', {
  seatId: { type: Sequelize.STRING, allowNull: false, unique: true },
  actionType: { type: Sequelize.STRING, allowNull: false },
  userId: Sequelize.STRING
});

//MySQL DB 테이블 생성
sequelize.sync();

var ipAddress;

app.get(['/', '/index.html'], function (req, res) {
  fs.readFile('./index.html', function (err, data) {
    res.contentType('text/html');
    res.send(data);
  });
});

//좌석 예약, 결제 상태 출력
//Sequelize 모듈로 MySQL에서 취소된 좌석을 제외한 좌석 정보를 가져온 뒤  
//배열 형태로 출력한다.  
//CloudFront에서 좌석 예약, 결제 상태를 캐시하지 않도록 HTTP 헤더에 Cache  
//-Control을 설정한다. 이 부분을 설정하지 않으면 매번 고정된 내용을 가져오  
//게 되므로 주의한다.   
app.get('/seats', function (req, res) {
  Seat.findAll({
    where: { actionType: { ne: 'cancel' }}
  }).success(function (seats) {
    var data = [];
    seats.map(function (seat) { return seat.values; }).forEach(function (e) {
      seat = e.seatId.split('-');
      data.push({
        row: seat[0],
        col: seat[1],
        actionType: e.actionType,
        userId: e.userId
      });
    });
    res.header('Cache-Control', 'max-age=0, s-maxage=0, public');
    res.send(data);
  });
});

//socket.io에 접속할 IP 주소 전달
// /ip에 GET 메서드로 접속했을 때 EC2 인스턴스의 IP주소를 출력한다.  
//웹 브라우저에서 ELB 로드 밸런서를 통하지 않고, socket.io에 직접 접속할   
//수 있도록 한다.  
//ELB 로드 밸런서를 경유하면 WebSocket 프로토콜의 Upgrade Handshake 동작  
//이 실패하기 때문에 EC2 인스턴스의 socket.io에 직접 연결한다.  
//ELB 로드 밸런서를 경유하여 socket.io에 연결하면 WebSocket 프로토콜을   
//사용할 수 없고, xhr-polling 또는 jsonppolling 방식을 사용하게 된다.   
//그리고 60초마다 한 번씩 연결이 끊어진다.  
//ec2metadata 모듈을 사용해 현재 EC2 인스턴스의 IP 주소를 얻어온다. 접속할  
//때마다 매번 IP 주소를 얻지 않도록 처리한다.   
//CloudFront에서 IP 주소를 캐시하지 않도록 HTTP 헤더에 Cache-Control을 설  
//정한다. 이 부분을 설정하지 않으면 모든 사용자가 동일한 EC2 인스턴스에 접  
//속하게 되고, 부하 분산이 되지 않으므로 주의한다.   
app.get('/ip', function (req, res) {
  res.header('Cache-Control', 'max-age=0, s-maxage=0, public');
  if (!ipAddress) {
    EC2Metadata.get(['public-ipv4'], function (err, data) {
      ipAddress = data.publicIpv4;
      res.send(ipAddress);
    });
  }
  else {
    res.send(ipAddress);
  }
});

// 좌석 예약, 결제 처리
//socket.io에서 웹 브라우저가 보내는 action 이벤트를 처리한다.   
//클라이언트에서 요청한 좌석 정보를 MySQL에서 가져온 뒤 정보가 없거나   
//유저 ID가 같거나, 좌석 상태가 취소라면 MySQL에 새로운 좌석 정보를 업  
//데이트(save 함수)한다.  
//메세지 내용은 JSON 형태로 된 새로운 좌석 정보이다.   
io.sockets.on('connection', function (socket) {
  socket.on('action', function (data) {
    Seat.find({
      where: { seatId: data.row + '-' + data.col }
    }).success(function (seat) {
      if (seat == null ||
            seat.userId == data.userId ||
              seat.actionType == 'cancel') {
        if (seat == null) 
          seat = Seat.build();
        seat.seatId = data.row + '-' + data.col;
        seat.userId = data.userId;
        seat.actionType = data.actionType;
        seat.save().success(function () {
          publisher.publish('seat', JSON.stringify(data));
        });
      }
    });
  });
});

//실시간으로 좌석 상태 갱신
//Redis의 seat 채널의 메시지를 받는다(subscribe 함수).
//메시지가 올 때마다 socket.io에 연결된 모든 클라이언트에 새로운 좌석 정보  
//를 보낸다(index.html로).  
subscriber.subscribe('seat');
subscriber.on('message', function (channel, message) {
  io.sockets.emit('result', JSON.parse(message));
});

//Node.js와 console.log 함수  
//Node.js에서 특정 동작때마다 매번 console.log 함수로 로그를 출력하면 성능  
//이 매우 떨어진다. 따라서 동시 접속자 수가 적어지므로 주의해야 한다.   
//console.log 함수는 꼭 필요한 정보나 에러가 발생했을 때만 사용한다.  
server.listen(80);
```
  
다음은 app.js에서 사용한 모듈들의 버전을 정의한 파일이다.  
  
package.json   
```
{
  "name": "ExampleTicketWebServer",
  "version": "0.0.1",
  "description": "ExampleTicketWebServer",
  "dependencies": {
    "express": "4.4.x",
    "ec2metadata": "0.1.x",
    "socket.io": "1.0.x",
    "sequelize": "1.7.x",
    "mysql": "2.3.2",
    "redis": "0.10.x"
  }
}
```

다음 내용을 index.html로 저장한다. 트래픽을 줄이기 위해 CDN의 jQuery, Boo 
tstrap CSS와 JavaScript를 사용한다. socket.io에 접속하기 위해서는 반드시  
/socket.io/socket.io.js를 사용해야 한다.   
```
<!DOCTYPE HTML>
<html>
<head>
  <title>ExampleTicket</title>
  <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
  <script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
  <script src="//netdna.bootstrapcdn.com/bootstrap/3.1.1/js/bootstrap.min.js"></script>
  <script src="/socket.io/socket.io.js"></script>
</head>
<body>
  <div id="stage" class="btn btn-default" style="width:600px; margin-left:265px;">Stage</div>
  <div><span>ID: </span> <span id="userId"></span></div>
  <div id="seats" class="col-xs-12"></div>
  
  <div id="dialog" class="modal fade">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h4 class="modal-title">결제</h4>
        </div>
        <div class="modal-body">
          <p><span id="seatId"></span>좌석을 결제하시겠습니까?</p>
        </div>
        <div class="modal-footer">
          <button id="pay" type="button"
            class="btn btn-primary" data-dismiss="modal">결제</button>
          <button id="cancel" type="button"
            class="btn btn-default" data-dismiss="modal">취소</button>
        </div>
      </div>
    </div>
  </div>
  
  <script>
    $(function () {
      var currentSeat;
      var socket;
      //prompt 함수로 ID를 입력받는다. 시연이 목적이므로 로그인 과정을 간  
      //략화했다.  
      var userId = prompt('ID를 입력하세요', '');
      $('#userId').text(userId);
      
      function action(seat, actionType) {
        socket.emit('action', {
          actionType: actionType,
          userId: userId,
          row: seat.data('row'),
          col: seat.data('col')
        });
      }
      
      function openDialog(seat) {
        $('#seatId').text(seat.data('row') + '-' + seat.data('col'));
        $('#dialog').modal({ keyboard: false, backdrop: 'static' });
      }
      
      function updateSeat(data) {
        var seat = $('div[data-row="' + data.row + '"][data-col="' + data.col + '"]');
        if (data.actionType == 'reserve')
          seat.removeClass('btn-default').addClass('btn-warning');
        else if (data.actionType == 'cancel')
          seat.removeClass('btn-warning').addClass('btn-default');
        else if (data.actionType == 'pay')
          seat.removeClass('btn-warning').addClass('btn-success');
        
        if (data.userId == userId &&
          (data.actionType == 'reserve' || data.actionType == 'pay'))
          seat.addClass('active');
        else
          seat.removeClass('active');
      }
      
      //좌석은 반복되는 코드를 피하기 위해 코드를 작성해 그렸다.  
      function drawSeats() {
        var seat;
        var rowSymbols = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J',
                        'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',  
                        'U', 'V', 'W', 'X', 'Y', 'Z'];
        $('#seats').css({ 'height': '550px', 'width': '1100px' });
        var colDiv = $('<div>');
        colDiv.css({ 'width': '35px', 'height': '550px', 'float': 'left', });
        for (var row = 1; row <= 26; row++) {
          var rowSymbol = $('<div>');
          rowSymbol.css({
            'width': '30px',
            'height': '25px',
            'padding-top': '7px',
            'margin-top': '12px',
            'margin-bottom': '1px' });
          rowSymbol.addClass('badge').text(rowSymbols[row -1]);
          colDiv.append(rowSymbol);
        }
          $('#seats').append(colDiv);
          
          for (var col = 1; col <= 32; col++) {
            var colDiv = $('<div>');
            for (var row = 1; row <= 26; row++) {
              colDiv.css({ 'height': '550px', 'width': '30px', 'float': 'left' });
              
              seat = $('<div>');
              seat.css({'width': '30px',
                        'height': '30px',
                        'padding-left': '5px',
                        'padding-top': '5px',
                        'margin-bottom': '8px'})
            .addClass('btn btn-default seat').text(col);
            seat.attr('data-row', rowSymbols[row - 1]).attr('data-col', col);
            colDiv.append(seat);
          }
          
          if (col <= 8)
            colDiv.css('margin-top', col * 10);
          else if (col <= 24 && col > 8)
            colDiv.css('margin-top', '80px');
          else if (col > 24)
            colDiv.css('margin-top', (33 - col) * 10);
            
          $('#seats').append(colDiv);
          
          if (col % 4 == 0) {
            var way = $('<div>');
            way.css({ 'width': '10px', 'height': '550px', 'float': 'left' });
            $('#seats').append(way);
          }
        }
      }
      
      //처음 페이지가 열렸을 때 /ip에 접속하여 IP 주소를 가져온 뒤 EC2   
      //인스턴스의 socket.io에 직접 접속한다.   
      $.get('/ip', function (ip) {
        socket = io.connect(ip);
        socket.on('connect', function () {
           //좌석을 클릭하면 socket.io로 서버에 action 이벤트를 보내서   
           //예약(reserve) 상태로 만든다.   
          $('.seat').click(function () {
            if ($(this).hasClass('btn-warning') || $(this).hasClass('btn-success'))
              return;
            currentSeat = $(this);
            action($(this), 'reserve');
          });
          
          //결제 버튼을 클릭하면 socket.io로 서버에 action 이벤트를 보내  
          //서 결제(pay) 상태로 만든다.  
          $('#pay').click(function () {
            action(currentSeat, 'pay');
            currentSeat = null;
          });
          
          //취소 버튼을 클릭하면 socket.io로 서버에 action 이벤트를 보내  
          //서 취소(cancel) 상태로 만든다.  
          $('#cancel').click(function () {
            action(currentSeat, 'cancel');
            currentSeat = null;
          });
          
          //서버에서 받은 result 이벤트 내용에서 userId가 같고, 예약   
          //상태일 때 결제 창을 출력한다(좌석을 클릭한 상태에서만 결제  
          //창을 출력해야하기 때문에 currentSeat을 검사한다).  
          //서버에서 result 이벤트를 받은 뒤 좌석의 상태에 따라 색상을 변  
          //경한다.  
          socket.on('result', function (data) {
            if (currentSeat && data.userId == userId && data.actionType == 'reserve') 
              openDialog(currentSeat);
            updateSeat(data);
          });
        });
      });
      
      drawSeats();
      
      //처음 페이지가 열렸을 때 /seats에 접속하여 좌석 정보를 가져온 뒤 좌  
      //석의 색상을 변경한다.   
      $.getJSON('/seats', function (data) {
        $.each(data, function (i, e) {
          updateSeat(e);
        });
      });
    });
  </script>
</body>
</html>
```
앞에서 생성한 `<프로젝트 이름>.src` S3 버킷에 ExampleTicketWebServer라는  
디렉터리를 생성하고 app.js, package.json, index.html 파일을 올린다.   

<img src="https://user-images.githubusercontent.com/33191974/159940154-f6cc6b91-b7b4-467c-8229-f22df9f7586b.png" width="50%" height="50%"/>  




















