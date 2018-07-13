## tesseractHL

1. ubuntu install for windows10
* Hyper-V 비활성화


* CPU의 가상화 지원 확인


* VirtualBox 설치


* VirtualBox 환경 설정


* 새로운 가상머신 생성


* 가상 머신 설정
  > conda create -n py36 python=3.6<br>
  > activate py36<br>
  > git clone https://github.com/j8lp/atari-py<br>
  > cd atari-py && make && python setup.py install && pip install "gym[atari]"<br>
  > 환경변수 PYTHONPATH 추가<br>
  > PYTHONPATH -> C:\atari-py:$PYTHONPATH<br>

* 게스트 운영체제 설치
