## tesseractHL

1. ubuntu install for windows10
* Hyper-V 비활성화<br>
  > ![pasted image 0](https://user-images.githubusercontent.com/14309034/42666991-717a532c-8683-11e8-861a-2514c024be40.png)

* CPU의 가상화 지원 확인<br>
  > bios Intel VT-x (Virtualization Technology) or AMD-V (AMD Virtualization) - > enable<br>

* VirtualBox 설치<br>
  > VirtualBox platform packages -> Windows hosts -> VirtualBox -> download <br>
  > VirtualBox Oracle VM VirtualBox Extension Pack -> All supported platforms -> VirtualBox extension pack download<br>
  > install<br>
  
* VirtualBox 환경 설정<br>
  > 디폴트 호스트 키인 오른쪽 Ctrl키는 제대로 동작 안하기 때문에 변경 

* 새로운 가상머신 생성
  > Ubuntu lts(Long Term Support) -> download<br>
  > VirtualBox 관리자  상단에 위치한 새로 만들기를 클릭
  > 이름에 생성할 가상 머신의 이름을 적어주고 종류는 Linux, 버전은 Ubuntu (64-bit)를 선택
  > 가상 머신에서 사용할 메모리를 할당 -> 2G 
  > 가상 머신에서 사용할 가상 하드 디스크를 새로 생성하여 사용 -> VDI 
  > 물리적 하드 드라이브에 저장하는 방식은 가상 머신의 성능을 고려하면 고정 크기를 권장 -> 30G

* 가상 머신 설정
  > 클립보드 공유 -> 양방향
  > 드래그 앤 드롭 파일 복사 -> 양방향
  > 부팅 순서 -> 플로피 디스크 체크 해제
  > 메인보드의 칩셋 -> ICH9
  > I/O APIC 사용 가상 머신에서 2개 이상의 CPU 코어를 사용할 경우 체크
  > UEFI 모드 부팅 가능하도록 설치 미디어가 준비된 경우에만 EFI 사용하기를 체크
  > 하드웨어 시각을 UTC로 보고하기 유닉스/리눅스를 게스트 운영체제로 사용할 경우 체크
  > 게스트 운영체제에서 사용하게 될  가상 CPU 코어 개수를 설정 실제 호스트 컴퓨터의 코어  개수의 절반 이하로 설정
  > (Ctrl + Shift + Esc 작업관리자 실행 코어 개수 확인)
  > 실행 제한 90%
  > PAE/NX 사용하기 가상머신에 설치된 운영체제가 4기가 이상의 메모리를 사용하기 위해서 체크
  > VT-x/AMD-V 사용하기 64비트 게스트 운영체제를 사용하려면 활성화
  > 네스티드 페이징 사용하기 하드웨어 가상화를 활성화 했다면 설정
  > 높은 해상도  전체 화면으로 게스트 운영체제를 보려면 충분한 크기를 할당
  > 가상 머신에 설치할 설치용 ISO 이미지를 선택
  > 네트워크 -> NAT

* 게스트 운영체제 설치
  > 오피스나 미디어 플레이어가 필요없다면 최소 설치
  > 기존에 윈도우가 설치된 컴퓨터라면 기타를 선택
  > UEFI 모드로 부팅가능한 컴퓨터라면 용도를 EFI 시스템 파티션으로 변경하고 512메가를 할당
  > 마운트 위치를 /로 변경하고 남은 전체 공간을 루트 파티션으로 할당

* 게스트 확장 설치
  > $ sudo apt-get update
  > $ sudo apt-get install build-essential linux-headers-$(uname -r)
  > 가상 머신 창의 메뉴에서  장치 > 게스트 확장 CD 이미지 삽입을 선택
  > $ sudo reboot
  
* 공유 폴더
  > 가상 머신 창의 메뉴에서  장치 > 공유 폴더 > 공유 폴더 설정을 선택
  > 자동 마운트, 항상 사용하기를 체크
  > sudo adduser 사용자아이디 vboxsf
  
* 게스트에 호스트전용 어뎁터 설정
  > VirtualBox- 파일 - 환경설정 - 네트워크 - 호스트 전용 네트워크 메뉴에서 호스트 전용 어댑터를  버튼으로 신규 호스트 전용 어댑터를 생성
  > 편집을 클릭하여 IP 주소를 확인
  > DHCP 체크 해제
  > VM설정에서 환경설정에서 추가한 어댑터를 추가
  > 어댑터2번에 신규로 추가한 #2번의 호스트 전용 어댑터를 추가
  > guest os terminal 에서 ifconfig -a 아이피 확인
  <pre><code>
  $ sudo vi /etc/network/interfaces
  # This file describes the network interfaces available on your system
  # and how to activate them. For more information, see interfaces(5).

  source /etc/network/interfaces.d/*

  # The loopback network interface
  auto lo  
  iface lo inet loopback

  # The primary network interface
  auto enp0s3  
  iface enp0s3 inet dhcp

  # The host-only network interface 신규로 추가
  auto enp0s8  
  iface enp0s8 inet static  
  address 192.168.253.101  
  netmask 255.255.255.0  
  network 192.168.253.0  
  </code></pre>
  > sudo systemctl restart networking
* guest os 헤드리스 시작
2. Tesseract install
* Leptonica install 
  <pre><code>
  wget http://www.leptonica.com/source/leptonica-1.74.4.tar.gz
  tar -xvzf leptonica-1.74.4.tar.gz
  cd leptonica-1.74.4
  ./configure 
  make
  sudo make install 
  sudo ldconfig
  </code></pre>
* github install
  <pre><code>
  sudo add-apt-repository ppa:git-core/ppa
  sudo apt-get update && sudo apt-get dist-upgrade
  sudo apt-get install git-core

  git version
  </code></pre>
* tesseract install
  <pre><code>
  sudo apt-get install g++ # or clang++ (presumably)
  sudo apt-get install autoconf automake libtool
  sudo apt-get install autoconf-archive
  sudo apt-get install pkg-config
  sudo apt-get install libpng-dev
  sudo apt-get install libjpeg8-dev
  sudo apt-get install libtiff5-dev
  sudo apt-get install zlib1g-dev
  sudo apt-get install libicu-dev
  sudo apt-get install libpango1.0-dev
  sudo apt-get install libcairo2-dev

  # Tesseract LSTM 소스 컴파일
  git clone https://github.com/tesseract-ocr/tesseract.git tesseract-ocr
  cd tesseract-ocr
  ./autogen.sh
  ./configure
  make
  sudo make install
  make training
  sudo make training-install
  sudo ldconfig
  # 학습완료 데이더 다운로드
  wget https://github.com/tesseract-ocr/tessdata_best/raw/master/eng.traineddata
  sudo mv *.traineddata /usr/local/share/tessdata/
  </code></pre>

* test ocr
  <pre><code>
  #ocr text print out include coordinate
  tesseract eng.myfont.exp0.tif out --oem 1 -l eng tsv

  #ocr text print out only text
  tesseract eng.myfont.exp0.tif out --oem 1 -l eng
  </code></pre>
  
# 추가 과제 
* 학습 데이터로 OCR 텍스트 추출
* 이미지에서 텍스트 영역 및 문자 추출


  
