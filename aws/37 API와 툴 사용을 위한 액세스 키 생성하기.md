## API와 툴 사용을 위한 액세스 키 생성하기

액세스 키(Access Key)와 시크릿 키(Secret Access Key)는 AWS API와 써드파티 툴(아마존  
이외의 회사나 단체에서 만든 툴)을 사용할 때 필요한 인증 수단이다.  
따라서 API와 써드파티 툴을 사용하고 싶을 때 생성한다. AWS의 모든 리소스는 API로 제어가  
가능하기 때문에 이 액세스 키와 시크릿 키는 매우 중요하다.   
따라서 유출이 되지 않도록 각별히 주의한다.  

> 액세스 키와 시크릿 키를 모든 사람이 볼 수 있는 블로그, 트위터, 페이스북에는  
> 절대 올리면 안된다. 특히 AWS API 사용법을 작성하여 블로그에 공개할 때 액세스  
> 키와 시크릿 키가 포함되지 않도록 주의한다. 모르는 사람이 액세스 키와 시크릿 키로  
> EC2 인스턴스를 대량으로 생성하여 몇 천만원의 요금이 청구되는 사고도 자주 일어난다.  

>#### 프리티어 알람 설정하기
>프리티어에서 제공하는 사용량을 넘어서서 요금이 청구되면 알람을 발생시킬 수 있다.  
>CloudWatch에서 요금 알람 설정을 하면 액세스 키와 시크릿 키가 유출되어 나도 모르는 사이에  
>요금이 청구되어도 빠르게 대처할 수 있다.

이제 액세스 키와 시크릿 키를 생성하는 방법에 대해 알아보자.  
AWS 콘솔의 위쪽에 자신의 이름이 표시된 부분을 클릭하면 메뉴가 나온다.  
여기서 Security Credentials를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138227740-085084a8-7024-47f9-8498-03cd58c41779.png)  
AWS 계정은 모든 AWS 리소스에 무제한으로 접근이 가능하므로 AWS 계정의 액세스 키를 사용하는 것은  
아무래도 보안성 위험이 있다. 따라서 AWS에서는 IAM(Identity Access Management)에서 계정을 생성한 뒤  
권한을 제한할 것을 권장하고 있다.  
실무에서는 IAM 계정의 액세스 키를 생성하여 사용하도록 하고, 이번 실습에서는 간단하게 AWS 계정의  
액세스 키를 생성해 보자.  
![image](https://user-images.githubusercontent.com/33191974/138228388-5c8bc42b-a1bc-4485-aa07-631328086f9d.png)  
버튼 클릭 즉시 액세스 키가 생성된다. 생성 창에서 Show Access 링크를 클릭하면 아래 그림과 같이  
액세스 키와 시크릿 키가 표시된다. 여기 표시되는 액세스 키와 시크릿 키를 복사해 따로 보관하거나  
Download Key File 버튼을 클릭한다. Download Key File 버튼을 클릭하면 rootkey.csv 파일을 받게 된다.  
![image](https://user-images.githubusercontent.com/33191974/138228993-bc274987-f00c-48d0-a9e8-134ef64bc1ec.png)  

>여기서 액세스 키와 시크릿 키를 따로 복사하여 보관하지 않거나, Download Key File 버튼을 클릭하여  
>파일을 받지 않고 그냥 Close 버튼을 클릭하여 창을 닫아버리면 두 번 다시 시크릿 키를 확인할 수 없게 된다.  
>(시크릿 키는 일종의 비밀번호이다.) 따라서 생성된 직후 키들을 복사하여 보관하거나 파일을 꼭 받아두어야 한다.  
>그냥 닫아버렸거나 키 파일을 분실했다면 이 액세스 키는 사용할 방법이 없으므로 폐기하고,  
>새로운 액세스 키를 생성해야 한다.  

![image](https://user-images.githubusercontent.com/33191974/138229434-85a87824-43c4-43d3-a1df-c42ccee1307b.png)  
이후 실습에서는 여기서 생성한 액세스 키와 시크릿 키를 사용한다.  



