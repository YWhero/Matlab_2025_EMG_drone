# Matlab_2025_EMG_drone
2025 MATLAB 대학생 AI 경진대회
팀 옥수수 인턴즈
주제) EMG 기반 한 팔 동작 인식을 이용한 드론 제어 시스템


# EMG_DR_E2E_pipeline_simulink.slk
이 파일은 실시간 EMG 신호 기반으로 딥러닝 분류를 수행하고, 그 결과를 바탕으로 실제 UAV의 제어 명령을 생성하고 시뮬레이션하는 End-to-End 최종 파이프라인을 Simulink로 구현한 모델입니다.
이 문서에서는 전체 파이프라인 중 드론 제어를 담당하는 Drone Execution 서브시스템에 대해 간략히 설명하고, Simulink 파일의 사용을 위한 사전 준비 단계를 안내합니다.

🔍 Overview: End-to-End Pipeline
전체 Simulink 파일 EMG_DR_E2E_pipeline_simulink.slk는 다음의 과정을 통합하여 구현합니다:
- 실시간 EMG 데이터 입력 → 신호 전처리(STFT) → 딥러닝 기반 분류
- 딥러닝 모델 출력(class) → 드론 명령으로 변환
- 드론 제어 로직 (Drone Execution Subsystem)
- 물리 기반 UAV 동역학 시뮬레이션 → Simulation 3D Visualization

🚁 Drone Execution Subsystem (Simulink 내 Subsystem Block)
Drone Execution 서브시스템은 분류된 EMG 결과값(gesture_idx)을 입력받아, UAV의 제어 명령으로 변환하고, 명령에 따라 실제 UAV의 움직임을 구현하기 위한 제어 목표값을 생성하는 로직입니다. 다음과 같은 블록 구조로 구성됩니다:
- ges2drone: EMG 분류 결과(class)를 실제 드론 동작 명령 번호로 변환
- CmdFSMManager: 드론의 상태(속도, 기울기, 회전속도)를 기반으로 명령 전환 시 보정(Cancellation) 여부 판단
- cmdDispatcher: 드론 명령에 따른 목표 자세(roll, pitch, yaw), 목표 고도(z), 추력(T) 생성
- PID Controllers & Angle Converter: 생성된 목표값을 기반으로 실제 드론의 모터 회전수 계산
- Linear Linearization (Dynamics Model): 현실적인 공기 저항, 감쇠력, 중력을 반영하여 드론의 움직임을 실제와 가깝게 시뮬레이션
- Simulation 3D Animation: 위 계산을 토대로 드론의 실제 움직임을 3차원으로 시각화

🚧 사전 준비 사항 (필수)
EMG_DR_E2E_pipeline_simulink.slk를 실행하기 전에 아래 사항을 반드시 수행해야 합니다.

  1️⃣ 학습된 모델 파일 다운로드 및 설정
  먼저 trained_model_3ch_lstm.mat 파일을 다운로드하여 로컬 컴퓨터에 저장합니다.
  Simulink 파일 내의 Classification and Execution 서브시스템 블록 안에 위치한 Prediction 블록을 더블 클릭하여, 모델 파일의 위치 경로를 지정합니다.

  2️⃣ STFT 신호 입력 연결 설정
  Classification and Execution 블록 내의 STFT 블록 출력을 Prediction 블록의 입력으로 반드시 연결해주어야 합니다.
  

▶️ 실행 방법 요약 (Quick Start)
위의 준비사항이 완료되었다면, 아래의 순서대로 파일을 실행할 수 있습니다:
  EMG_DR_E2E_pipeline_simulink.slk 파일을 MATLAB에서 엽니다.
  사전 준비 사항(모델 파일 지정, STFT 출력 연결)을 완료합니다.
  Simulink 메뉴의 ▶️ Run 버튼을 눌러 시뮬레이션을 시작합니다.
  시뮬레이션이 실행되면 Simulation 3D 블록에서 드론의 실제 움직임을 시각적으로 확인할 수 있습니다.

🛠️ Customization
Drone Execution 서브시스템 내부의 파라미터(CmdFSMManager, cmdDispatcher, PID gains 등)는 상황에 따라 최적화가 가능합니다.



본 모델 사용에 관해 추가적인 문의사항이나 피드백이 있다면 언제든지 연락 부탁드립니다. 감사합니다.
