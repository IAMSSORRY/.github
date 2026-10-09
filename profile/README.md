# IAMSSORRY · 암쏘사과

> 판정 근거를 제시하는 사과 선별 시스템, **SSORRY**

PAC 2026 Physical AI Challenge · 분과① 로보틱스 미션 · 안동청과 기업문제

<img width="4174" height="2000" alt="image" src="https://github.com/user-attachments/assets/333f7c93-dee9-48cb-a4c3-0ed139ea888f" />

---

## 무엇을 만드나

로봇팔이 트레이의 사과를 하나씩 집어 색과 멍으로 **상 · 중 · 하**를 판정하고, 등급별 상자 칸에 놓습니다.

기계가 등급을 매겨도 근거를 보여주지 못하면 현장은 결국 사람이 다시 검수합니다.
SSORRY는 등급과 함께 판정에 쓴 수치(빨강 비율, 임계값, 신뢰도)를 대시보드에 그대로 보여줍니다.

## 원칙

| | |
|---|---|
| **조건을 고정한다** | 회색 카드로 색을 보정하고, 같은 자세에서 사과를 돌려 가며 멍을 검사합니다 |
| **근거를 함께 낸다** | 등급과 판정 수치를 같이 기록하고 화면에 표시합니다 |
| **실패를 스스로 안다** | 놓은 사과가 구르면 다음 놓기의 하강 속도와 높이를 자동으로 낮춥니다 |

## 저장소

| 저장소 | 역할 | 스택 |
|---|---|---|
| [Piper](https://github.com/IAMSSORRY/Piper) | 로봇팔 제어 · 사과 검출 · 등급 판정 | Python · piper_sdk · OpenCV |
| [Server](https://github.com/IAMSSORRY/Server) | 영상 중계 · 판정 수집 · 원격 제어 · AI 조언 | FastAPI · SQLite · Docker |
| [Web](https://github.com/IAMSSORRY/Web) | 관제 대시보드 | React 19 · Vite · Tailwind CSS 4 |

## 시스템 구성

```
[ 카메라 2대 ]  위: Sony A7C (고정)  ·  손목: 글로벌 셔터
[ AgileX PIPER 6축 로봇팔 ]  USB-CAN
        │
        ▼
[ Ubuntu 로봇 PC ]
  PIPER Studio ── 카메라 MJPEG 스트림
  Piper (mission.py) ── 집기 · 검사 · 판정 · 적재 · 비상정지
        │  POST /ingest/*         ▲ /control/*
        ▼                         │
  Server (FastAPI, Docker) ── SQLite 기록 · AI 조언
        │  WebSocket · REST
        ▼
[ Web 대시보드 ]  실시간 영상 · 판정 근거 · 통계 · CSV · 비상정지
```

## 판정 기준

| 등급 | 조건 |
|---|---|
| 상 | 빨강 비율 0.5 이상, 멍 1.5% 미만 |
| 중 | 빨강이지만 회전 검사에서 멍 1.5% 이상 |
| 하 | 빨강 비율 0.5 미만 (노랑) |

## 팀

**암쏘사과** · 대구소프트웨어마이스터고등학교

| 이름 | 담당 |
|---|---|
| 이도건 | 대시보드, 로봇팔 제어 |
| 김준현 | 비전, 데이터 처리 |

협업 규칙은 [Git 협업 가이드](https://github.com/IAMSSORRY/.github/blob/main/README.md)를 따릅니다.
