# wintercoding

## 1. 웹캠으로 동영상 저장
 -> 웹캠저장.ipynb 을 실행시키면 data.avi로 웹캠이 동영상으로 저장된다 . 
 
## 2. 동영상 프레임 별로 짜르기 
  -> 동영상frame별로 짜르기.ipynb 를 실행 시키면 data.avi파일이 20프레임 별 잘라져서  이미지로 저장된다  
  총 300개의 이미지 파일이 images 폴더안에 저장되었다.
  
## 3. 라벨링 하기 
  라벨링을 하기 위해 오픈소스인 OPENLabeling을 사용하였다. 자세한 사용법은 https://github.com/Cartucho/OpenLabeling안에 있다.
  run.py를 실행 시키면 라벨링 할 수 있게 되며 class.txt에 자기가 라벨링 하고싶은 것을 입력하면 된다
  이번 프로젝트에서는 간단하게 pental sharp, jetstream, U-knock 을 구별하기 위해 라벨링을 하였다.
  총 300개의 사진에 대한 라벨링을 수작업으로 수행 하였다. 
  또한 voc 형식으로 학습 시키는 오픈소스를 사용하기 위해선 yolo to voc .ipynb파일을 실행 시키면 
  위에서 라벨링된 yolo 파일 형식의 annotation이 voc format으로 바꿀수 있다.\
  라벨링 된 데이터는 아래의 그림과 같다 . 
![default](https://user-images.githubusercontent.com/33194900/47954303-21e86180-dfcc-11e8-8678-b9e80f06cc04.PNG)

## 4. 학습시키기 
  처음에 keras-yolo3을 사용하고 싶었는데 데이터 형식이 잘 맞지 않고 서버에 있는 gpu버전이 다르므로 학습 시킬 수가 없었다. 
  따라서 가장 유명한 yolo를 사용 하였고 https://github.com/AlexeyAB/darknet.git 를 이용 하였다.
  darknet에서 compile을 시켜야하는데 일단 여기서 local computer의 visual studio가 path형식이 잘 맞지않아 학습 시키려고 수많은       
  노력을 해보았지만 할수가 없었다. 
  따라서 linux서버를 사용하였는데 이 서버는 학교에서 다른 프로젝트를 하기위해 받은 서버를 사용하였다.
  일단 내가 생성 한 이미지 와 거기에 따른 라벨링txt파일을 넣은 후 conf 파일을 새로 생성한 후 여러가지 파일을 수정한 후 학습을 시켜보았다.
  수정한 파일은 Makefile, class_list.txt, obj.data, obj.names, train.txt 이다. 자세한 설명은 https://github.com/AlexeyAB/darknet.git안       에 있다.
  자신의 학습 데이터를 가지고 설정하는 방법은 아래의 그림과 같다 . 
![default](https://user-images.githubusercontent.com/33194900/47954464-9b348400-dfcd-11e8-8b93-8dd0dd1cd50b.PNG)

  또한 설정한후 기존의 yolo network(Download pre-trained weights for the convolutional layers (154 MB): https://pjreddie.com/media/files/darknet53.conv.74)를 다운 받아 
  root folder에 넣은후 학습을 실행을 해 보았다. 
  학습 명령어는 darknet.exe detector train data/obj.data yolo-obj.cfg darknet53.conv.74로 실행을 시키면 된다. 
  모델을 학습 시키는데 아래 그림과 같이 시간이 너무 오래 걸리고 있다. 
![default](https://user-images.githubusercontent.com/33194900/47954507-14cc7200-dfce-11e8-96b1-04e1687052d8.PNG)
![2](https://user-images.githubusercontent.com/33194900/47954508-15650880-dfce-11e8-8bf2-e66ce9cf52f0.PNG)

  gpu를 사용하는데도 너무너무 시간이 오래걸려서 계속 확인을 해보았는데 마침내 결과를 찾아내었다. 서버를 누가 사용하고 있어서 느린것이였다.....
  마침내 서버가 터지는 결과를 나타냈다. 
![image](https://user-images.githubusercontent.com/33194900/47954527-42b1b680-dfce-11e8-80ef-8b439d613682.png)

터지지않으면 학습된 모델이 backup폴더에 저장이 될 것이다.


## 5. 학습된 모델을 웹캠 동영상에 적용시키기. 
   만약 학습된 모델이 있다면 이것은 정말 간단하게 수행할 수 있다.
   이미지파일 실행
   
    ./darknet detector test data/obj.data yolo-obj.cfg backup/ <학습된 모델 이름> data/<image file>
   
   동영상파일 실행
   
   ./darknet detector demo data/obj.data yolo-obj.cfg backup/ <학습된 모델 이름> data/<video file>
   
   Webcam 실행
 
   ./darknet detector demo data/obj.data yolo-obj.cfg backup/ <학습된 모델 이름> data
 
 




