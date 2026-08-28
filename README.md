# 📘 Omx-AI-IL_Robotis

### 1. 모방학습 개요
* [모방학습(Imitation Learning)이란?](https://en.wikipedia.org/wiki/Imitation_learning)
* [IL 핵심 알고리즘 설명](https://ropas.snu.ac.kr/liberation/seminar/2021/Imitation_Learning.pdf)

### 2. omx-서브모터
* [로보티즈(ROBOTIS) e-Manual 바로가기](https://emanual.robotis.com/)
* [DYNAMIXEL 서브모터 제어 가이드](https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_sdk/overview/)

서보모터 ID wizard 2.0세팅
https://www.notion.so/zeta7/1-DXL-Wizard-2-0-29e0db9684a380d6b4e6e3be034ee353

### 2-1. omx ai sw
* **ROBOTIS OMX 초기 환경 설정 자동화 (Orin Nano Super)**
* 아래 파이썬 코드를 `setup_omx.py` 파일로 저장한 뒤, 터미널에서 `python3 setup_omx.py`를 입력하면 설치가 자동으로 진행됩니다.

```python
import subprocess
import os
import sys

def run_command(command, cwd=None):
    print(f"\n[실행] {command}")
    try:
        # check=True를 통해 에러 발생 시 스크립트 중단
        subprocess.run(command, shell=True, executable='/bin/bash', cwd=cwd, check=True)
    except subprocess.CalledProcessError as e:
        print(f"\n[에러] 명령어 실행 중 문제가 발생했습니다. (종료 코드: {e.returncode})")
        sys.exit(1)

def main():
    # 바탕화면 경로 설정
    home = os.path.expanduser("~")
    desktop = os.path.join(home, "Desktop")
    
    print("=== 1. miniconda 설치 및 초기화 ===")
    run_command("rm -rf ~/miniconda3")
    run_command("wget [https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh)")
    run_command("bash Miniconda3-latest-Linux-aarch64.sh -b -p ~/miniconda3")

    # 이후 conda 명령어를 사용하기 위한 환경 활성화 명령어
    conda_base = "source ~/miniconda3/bin/activate && "

    print("\n=== 2. Conda 채널 이용 약관 동의(TOS) ===")
    run_command(conda_base + "conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/main](https://repo.anaconda.com/pkgs/main)")
    run_command(conda_base + "conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/r](https://repo.anaconda.com/pkgs/r)")

    print("\n=== 3. lerobot 가상환경 생성 및 ffmpeg 설치 ===")
    run_command(conda_base + "conda create -y -n lerobot python=3.10")
    run_command(conda_base + "conda activate lerobot && conda install -c conda-forge ffmpeg=6.1.1 -y")

    print("\n=== 4. Github 클론 및 패키지 설치 ===")
    os.makedirs(desktop, exist_ok=True)
    run_command("git clone [https://github.com/ROBOTIS-GIT/lerobot.git](https://github.com/ROBOTIS-GIT/lerobot.git)", cwd=desktop)
    
    lerobot_dir = os.path.join(desktop, "lerobot")
    run_command(conda_base + "conda activate lerobot && pip install -e .", cwd=lerobot_dir)
    run_command(conda_base + "conda activate lerobot && pip install -e \".[dynamixel]\"", cwd=lerobot_dir)

    print("\n=== 5. 리더와 팔로워 포트 확인 ===")
    print("(/dev/ttyACM1이 리더이다)")
    run_command("ls /dev/ttyACM* /dev/ttyUSB* 2>/dev/null")

if __name__ == "__main__":
    main()



### 3. 데이터 수집 및 전처리
* [데이터 사이언스 기초 (Pandas/Numpy)](https://pandas.pydata.org/docs/user_guide/10min.html)
* [머신러닝 데이터 전처리 기법](https://scikit-learn.org/stable/modules/preprocessing.html)

---

### 4. 기타 학습 리소스
* [Python3 공식 문서](https://docs.python.org/ko/3/)
* [Google Colab 시작하기](https://colab.research.google.com/)