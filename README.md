# and-fps-
opt
$RemoteHost = "192.168.1.100"

리모트 PowerShell 세션을 만듭니다.
$Session = New-PSSession -ComputerName $RemoteHost

리모트 컴퓨터에서 리셋 명령을 실행합니다.
Invoke-Command -Session $Session -ScriptBlock {
# 운영 체제 리셋 명령
Restart-Computer -Force
}

리모트 PowerShell 세션을 종료합니다.
Remove-PSSession -Session $Session

========================================
#!/bin/bash

# 시스템 정보 수집
echo "시스템 정보:"
echo "- 운영 체제 유형: $(uname -s)"
echo "- CPU: $(lscpu | grep "Model name" | awk -F':' '{print $2}')"
echo "- GPU: $(lspci | grep "VGA" | awk -F':' '{print $3}' | sed 's/\([^(]*\)(.*/\1/')"
echo "- 메모리 용량: $(free -h | awk '/Mem/ {print $2}')"

# NVIDIA 드라이버 확인
if command -v nvidia-smi &> /dev/null; then
    echo "NVIDIA 그래픽 카드가 설치되어 있습니다."
    nvidia_version=$(nvidia-smi --query-gpu=driver_version --format=csv,noheader)
    echo "설치된 NVIDIA 드라이버 버전: $nvidia_version"
else
    echo "NVIDIA 그래픽 카드가 설치되어 있지 않습니다."
fi

# CUDA 툴킷 버전 확인
if command -v nvcc &> /dev/null; then
    cuda_version=$(nvcc --version | grep -o -E 'V[0-9]+\.[0-9]+')
    echo "설치된 CUDA 툴킷 버전: $cuda_version"
else
    echo "CUDA 툴킷이 설치되어 있지 않습니다."
fi

#!/bin/bash

# CUDA 버전 설정
cuda_version="11.8"

# 운영 체제 확인
os_type=$(uname -s)
if [ $os_type == "Linux" ]; then
    os_type="linux"
elif [ $os_type == "Darwin" ]; then
    os_type="mac"
else
    echo "지원되지 않는 운영 체제입니다."
    exit 1
fi

# CUDA 다운로드 및 설치
if [ $os_type == "linux" ]; then
    # Ubuntu 또는 Debian 배포판 확인
    if command -v apt-get &> /dev/null; then
        # Ubuntu 또는 Debian 기반 배포판
        sudo apt-get update
        sudo apt-get install -y software-properties-common
        sudo add-apt-repository -y ppa:graphics-drivers/ppa
        sudo apt-get install -y cuda-$cuda_version-$os_type-x86_64
    elif command -v dnf &> /dev/null; then
        # Fedora 기반 배포판
        sudo dnf install -y https://developer.download.nvidia.com/compute/cuda/repos/rhel8/x86_64/cuda-repo-rhel8-$cuda_version.x86_64.rpm
        sudo dnf module install -y nvidia-driver:latest-dkms
        sudo dnf install -y cuda-$cuda_version
    else
        echo "지원되지 않는 Linux 배포판입니다."
        exit 1
    fi
elif [ $os_type == "mac" ]; then
    # macOS
    curl -O https://developer.download.nvidia.com/compute/cuda/repos/macos/x86_64/cuda-macos.sh
    sudo sh cuda-macos.sh --silent --override
    rm cuda-macos.sh
else
    echo "지원되지 않는 운영 체제입니다."
    exit 1
fi

echo "CUDA 툴킷 $cuda_version이 설치되었습니다."

#!/bin/bash

# 베녹스 환경 설정
echo "베녹스 환경 구성 중..."
sudo apt-get update
sudo apt-get install -y build-essential git cmake
sudo apt-get install -y libopenblas-dev libopenmpi-dev
sudo apt-get install -y python3-dev python3-pip

# 가상 NVIDIA Blackwell 환경 설정
echo "NVIDIA Blackwell 가상 환경 설정 중..."
python3 -m venv nvidia-blackwell-env
source nvidia-blackwell-env/bin/activate
pip install --upgrade pip
pip install nvidia-blackwell

# NVIDIA Blackwell 샘플 코드 실행
echo "NVIDIA Blackwell 샘플 코드 실행 중..."
cd nvidia-blackwell-env/share/nvidia-blackwell/examples
python3 sample_code.py

# 가상 환경 종료
deactivate

egpu의 장점
#!/bin/bash

echo "EGPU의 주요 장점:"

echo "1. 이동성:"
echo "   - EGPU는 외장형으로 설계되어 노트북 등의 이동형 기기와 연결하여 사용할 수 있습니다."
echo "   - 필요에 따라 EGPU를 손쉽게 이동시켜 사용할 수 있습니다."

echo "2. 확장성:"
echo "   - EGPU는 외장형이므로 필요에 따라 교체와 업그레이드가 가능합니다."
echo "   - 새로운 GPU 모델로 쉽게 업그레이드할 수 있습니다."

echo "3. 냉각 효율:"
echo "   - EGPU는 외장형이므로 별도의 냉각 장치를 사용할 수 있어 더 효과적인 냉각이 가능합니다."
echo "   - 내부에 장착된 GPU에 비해 열 발산이 용이합니다."

echo "4. 연결 방식:"
echo "   - EGPU는 Thunderbolt 3 또는 USB-C 포트를 통해 연결됩니다."
echo "   - 이러한 고속 인터페이스를 통해 빠른 데이터 전송이 가능합니다."

echo "5. 호환성:"
echo "   - EGPU는 다양한 운영 체제(Windows, macOS, Linux)와 호환됩니다."
echo "   - 다양한 기기와 연결하여 사용할 수 있습니다."

echo "이러한 장점들로 인해 EGPU는 이동성, 확장성, 냉각 효율 등에서 GPU에 비해 유리합니다."


#!/bin/bash

# ADT-Link 가상 환경 설정
echo "ADT-Link 가상 환경 설정 중..."
python3 -m venv adt-link-env
source adt-link-env/bin/activate

# ADT-Link 라이브러리 설치
echo "ADT-Link 라이브러리 설치 중..."
pip install adt-link

# ADT-Link 테스트 코드 실행
echo "ADT-Link 테스트 코드 실행 중..."
python3 adt-link_test.py

# 결과 확인
echo "테스트 결과:"
cat adt-link_test_result.txt

# 가상 환경 종료
deactivate
echo "ADT-Link 가상 환경 종료."

#!/usr/bin/env python3

import serial
import time

# ADT-Link 하드웨어 연결 설정
ser = serial.Serial(
    port='/dev/ttyUSB0',  # ADT-Link 하드웨어의 포트 번호
    baudrate=115200,      # 통신 속도
    timeout=1             # 응답 대기 시간
)

# ADT-Link 하드웨어와의 통신 함수
def communicate_with_adt_link():
    # 명령어 전송
    ser.write(b'get_status\n')
    
    # 응답 대기 및 수신
    response = ser.readline().decode().strip()
    
    # 응답 처리
    if response == 'OK':
        print("ADT-Link 하드웨어가 정상적으로 작동하고 있습니다.")
    else:
        print("ADT-Link 하드웨어와의 통신에 실패했습니다.")
    
    # 응답 시간 지연
    time.sleep(1)

# 무한 루프로 통신 수행
while True:
    communicate_with_adt_link()
#!/bin/bash

# 가상 NVIDIA RTX 4090 환경 설정
echo "가상 NVIDIA RTX 4090 환경 설정 중..."
python3 -m venv rtx4090-env
source rtx4090-env/bin/activate

# NVIDIA 드라이버 및 CUDA 툴킷 설치
echo "NVIDIA 드라이버 및 CUDA 툴킷 설치 중..."
sudo apt-get update
sudo apt-get install -y nvidia-driver-525 cuda-toolkit-11-8

# RTX 4090 관련 라이브러리 설치
echo "RTX 4090 관련 라이브러리 설치 중..."
pip install numpy tensorflow-gpu opencv-python

# 벤치마크 코드 실행
echo "벤치마크 코드 실행 중..."
python3 rtx4090_benchmark.py

# 실행 결과 확인
echo "실행 결과:"
cat rtx4090_benchmark_result.txt

# 가상 환경 종료
deactivate
echo "가상 NVIDIA RTX 4090 환경 종료."

-=====================
#!/bin/bash

# 디스플레이 설정 최적화
echo "디스플레이 설정 최적화 중..."
xrandr --rate 5700
xrandr --output HDMI-1 --mode 1920x1080

# 운영 체제 설정 최적화
echo "운영 체제 설정 최적화 중..."
sudo sysctl -w vm.swappiness=10
sudo sysctl -w kernel.sched_latency_ns=10000000
sudo sysctl -w kernel.sched_min_granularity_ns=2000000

# 프로세스 스케줄링 최적화
echo "프로세스 스케줄링 최적화 중..."
sudo echo 'GRUB_CMDLINE_LINUX="quiet splash elevator=noop"' >> /etc/default/grub
sudo update-grub

# 시스템 캐시 및 버퍼 최적화
echo "시스템 캐시 및 버퍼 최적화 중..."
sudo echo 'vm.dirty_ratio = 5' >> /etc/sysctl.conf
sudo echo 'vm.dirty_background_ratio = 3' >> /etc/sysctl.conf
sudo sysctl -p

# 파일 시스템 최적화
echo "파일 시스템 최적화 중..."
sudo tune2fs -o user_xattr,acl /dev/sda1

echo "최적화가 완료되었습니다."


