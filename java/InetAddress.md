# InetAddress 클래스
- 자바에서 IP 주소를 표현할 때 사용하는 클래스이다.
- InetAddress의 주요 메서드
   - getAddress() - InetAddress 객체의 IP 주소를 반환
   - getHostAddress() - IP 주소를 반환
   - getHostName() - 호스트 이름을 문자열로 반환

## Example
```
import java.net.*;

class ex1 {
  public static void main(String args[]) throws UnknownHostException {
    InetAddress address = InetAddress.getLocalHost();
    System.out.println("로컬컴퓨터 이름 : " + address.getHostName());
    System.out.println("로컬컴퓨터 IP 주소 : " + address.getHostAddress());
    
    address = InetAddress.getByName("www.naver.com");
    System.out.println("www.naver.com의 이름과 IP 주소 : " + address);
    
    InetAddress sw[] = InetAddress.getAllByName("www.naver.com");
    
    for (int i = 0; i <sw.length; i++) {
      System.out.println(sw[i]);
    }
  }
}
```
결과  
```
로컬컴퓨터 이름 : KDH-PC
로컬컴퓨터 IP주소 : 192.168.0.8
www.naver.com의 이름과 IP주소 : www.naver.com/202.131.30.11
www.naver.com/202.131.30.11
www.naver.com/125.209.222.141
```

## Example 2
```
import java.net.*;

class ex2 {
  public static void main(String args[]) { 
    InetAddress ipAddress = null;
    
    try {
      ipAddress = InetAddress.getByName("202.131.30.11");
      System.out.println("getHostName : " + ipAddress.getHostName());
      System.out.println("getHostAddr : " + ipAddress.getHostAddress());
      System.out.println("toString : " + ipAddress.toString());
    } catch(UnknownHostException e) {
      System.out.println(e);
    }
    System.out.println("-------------------------------------");
    
    try {
      ipAddress = InetAddress.getByName("www.naver.com");
      System.out.println("getHostName : " + ipAddress.getHostName());
      System.out.println("getHostAddr : " + ipAddress.getHostAddress());
      System.out.println("toString : " + ipAddress.toString());
    } catch(UnknownHostException e) {
      System.out.println(e);
    }
  }
}
```
결과  
```
getHostName : 202.131.30.11
getHostAddr : 202.131.30.11
toString : 202.131.30.11/202.131.30.11
------------------------------
getHostName : www.naver.com
getHostAddr : 125.209.222.141
toString : www.naver.com/125.209.222.141
```






















