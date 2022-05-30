ln은 link의 약자로 리눅스에서 원본 파일이나 디렉터리를 가리키는 link를 만드는   
명령어이다. 링크의 종류는 하드 링크(hard link)와 심볼릭 링크(symbolic link; 또는   
소프트 링크(soft link))가 있으며 `ln` 명령어로 2가지 유형의 링크를 모두 만들   
수 있다.  
  
# hard link   
하드 링크는 원본 파일에 대한 alias와 비슷한 개념으로 하드 링크를 생성하면 원본과   
동일한 inode를 갖는 파일이 만들어 진다. 하드 링크는 만들고 나면 무엇이 원본이고   
무엇이 링크인지 알기가 어렵다.  
  
하드링크는 ln 명령어로 바로 만들수 있으며 다음은 원본 파일인 origin에 대한 하드   
링크인 hard-link라는 파일을 생성한다.   
  
> 하드 링크는 inode를 공유하므로 다른 파티션이나 파일 시스템에 있는 파일이나  
> 디렉터리에 대해서 하드 링크를 생성할 수 없다.  
  
만들어진 하드 링크는 ls -l로 봐도 구분하기 어려운 문제가 있다.  
```
$ ls -l
total 8 
-rw-r--r--. 2 lesstif lesstif 12 Jan 14 23:00 hard-link
-rw-r--r--. 2 lesstif lesstif 12 Jan 14 23:00 origin
-rw-r--r--. 1 lesstif lesstif  0 Jan 14 23:00 test
```
하드 링크인지 알아보려면 inode 번호를 보는 옵션인 -i를 같이 사용해서 inode 번호의  
일치 여부를 확인하면 된다.   

```
$ ls -l

807407436 -rw-r--r--. 2 lesstif lesstif 12 Jan 14 23:00 hard-link
807407436 -rw-r--r--. 2 lesstif lesstif 12 Jan 14 23:00 origin
807375213 -rw-r--r--. 1 lesstif lesstif  0 Jan 14 23:00 test
```
만약 다른 경로에도 같은 파일에 대한 하드 링크가 있는지 확인하려면 find 명령어에   
-inum 옵션을 주고 실행하면 된다. 다음은 inode number가 807407436인 파일을 찾아서  
자세한 정보를 출력한다.   

```
$ find -inum 807407436 | xargs ls -l
```

# symbolic(soft) link
심볼릭(소프트)링크는 윈도우의 바로가기와 비슷한 개념으로 원본을 가리키는 역할을  
수행하므로 하드 링크와는 달리 다른 파티션이나 파일 시스템에 있어도 생성이 가능하며   
원본과 다른 inode number를 갖게 된다.  
  
만들려면 ln에 symbolic link 생성 옵션인 -s를 추가하며 다음은 $HOME밑에 etc_bashrc  
라는 링크 파일을 생성한다.   
```
$ ln -s /etc/bashrc ${HOME}/etc_bashrc
```
  
심볼릭 링크는 ls -l로 볼 경우 원본을 포인팅하므로 쉽게 알아볼 수 있다.   
```
$ ls -l $HOME/etc_bashrc 

lrwxrwxrwx. 1 lesstif lesstif 11 Jan 14 23:16 /home/lesstif/etc_bashrc -> /etc/bashrc
```
만약 링크 파일이 이미 있을 경우 ln을 실행하면 다음과 같이 "File exists" 에러가   
발생한다.  
```
$ ln -s /etc/bashrc ${HOME}/etc_bashrc

ln: failed to create symbolic link '/home/lesstif/etc_bashrc': File exists
```

기존에 심볼릭 링크가 있을 경우 덮어쓰는 옵션인 -f를 사용하면 기존 링크 파일이   
있어도 정상 동작한다.   
```
$ ln -sf /etc/bashrc ${HOME}/etc_bashrc
```

# 원본 파일을 삭제했을 경우  
하드 링크는 같은 inode number를 공유하므로 원본을 삭제해도 다른 하드 링크가   
있다면 원본 파일 내용을 읽는데 아무 문제가 없다.   
  
소프트 링크는 바로가기이므로 원본이 삭제되면 소프트 링크 파일은 사용 불가하게   
된다.   
  
설정마다 다르지만 첨부처럼 원본이 삭제된 파일은 깜박이는 빨간색으로 표시되므로   
쉽게 알아볼 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158400714-7e560083-6d2d-45d8-914b-229852ef6690.png" width="50%" height="50%"/>  

















---

# 파티션
롤케이크 비유를 해본다.   
여러분들은 롤케이크를 먹을 때 그냥 통째로 먹지는 않을 것이다.   
물리적으로 제한된 크기의 롤케이크를 먹기 좋게 각각 자르고 잘린 조각의 케이크를  
입에서 먹기 좋게 분해하여 소화를 시킨다. 케이크를 한 번에 다 나누어서 총량을  
먹을 수도 있겠지만 대부분 한 번에 먹을 양만 자르고 나머지는 보관할 수도 있다.  
이렇듯 물리적으로 제한된 양을 사람의 성향에 따라서 다 소비하거나 보관이 가능하다.  
리눅스 파티션에 제한된 DISK 공간을 나눌 때도 똑같다.  
사용하는 목적과 용도에 따라서 나누고 남는 공간은 추후에 사용할 목적으로 보관할  
수도 있는 것이다.   



























