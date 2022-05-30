## EC2 인스턴스에서 EBS 볼륨 마운트하기
Linux에서는 저장 장치를 사용하려면 마운트라는 과정이 필요하다.  
앞에서 생성한 EBS 볼륨을 Ext4 파일시스템을 포맷했으므로 마운트만하면 바로  
사용할 수 있다.  
먼저 ls /dev/sdf -al 명령을 입력하여 /dev/sdf 장치가 있는지 확인한다.  
![image](https://user-images.githubusercontent.com/33191974/137571644-3f562b67-7ce5-4989-9d52-23540583be08.png)  
/dev/xvdf 장치가 /dev/sdf로 심볼릭 링크(링크를 연결하여 원본 파일을 직접 사용하는 것과 같은 효과를  
내는 링크이다. 윈도우의 바로가기와 비슷한 개념 )가 되어 있다.  
이제 sudo mount /dev/sdf /mnt를 입력하여 저장 장치를 마운트한다.  
/dev/sdf 대신 /dev/xvdf로 지정해도 된다.  
![image](https://user-images.githubusercontent.com/33191974/137571732-ca20e1d9-ff8a-492f-a1fd-d01c2c302f39.png)  
/dev/sdf를 /mnt 디렉터리에 마운트한다는 명령인데 /mnt 디렉터리가 아닌 다른 디렉터리를 지정해도  
상관없다.  
  
df -h 명령을 입력하여 현재 마운트된 저장 장치의 목록을 확인할 수 있다.  
9.8G 용량의 /dev/xvdf 장치가 /mnt에 마운트 되어 있다.  
이제 이곳에 파일을 저장할 수 있다.   
![image](https://user-images.githubusercontent.com/33191974/137571792-412c30b5-b653-4b6c-b320-7971e648074f.png)



