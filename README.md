<!--
SPDX-FileCopyrightText: Copyright (c) 2021 LG Electronics Inc.
SPDX-License-Identifier: Apache-2.0
-->


# FOSSLight Scanner CI/CD Pipeline

이 프로젝트는 GitLab CI/CD를 사용하여 FOSSLight Scanner를 자동으로 실행하는 파이프라인을 포함합니다.

## 파이프라인 구조

### 1. Install Stage
- `install_fosslight`: FOSSLight Scanner 설치 및 캐싱

### 2. Scan Stage  
- `run_fosslight_scan`: 프로젝트 코드 스캔 및 라이선스 분석

## 파일 설명

### `.gitlab-ci.yml`
GitLab CI/CD 파이프라인 설정 파일입니다.

**주요 기능:**
- Python 3.11 환경에서 실행
- pip 캐싱으로 빌드 속도 향상
- 스캔 결과를 아티팩트로 저장
- push와 merge request에서만 실행

### `.fosslightrc`
FOSSLight Scanner 설정 파일입니다.

**주요 설정:**
- 출력 디렉토리: `fosslight_report/`
- 출력 형식: JSON
- 제외 패턴: `.git/`, `node_modules/`, `__pycache__/` 등
- 신뢰도 임계값: 0.8

### `requirements.txt`
프로젝트 의존성 파일입니다.

## 사용 방법

### 1. 로컬에서 테스트
```bash
# FOSSLight Scanner 설치
pip install fosslight_scanner

# 스캔 실행
fosslight_scanner -p . -o fosslight_report
```

### 2. GitLab에서 자동 실행
- 코드를 push하거나 merge request를 생성하면 자동으로 실행됩니다
- GitLab > CI/CD > Pipelines에서 진행 상황을 확인할 수 있습니다

## 스캔 결과

스캔이 완료되면 다음 파일들이 생성됩니다:
- `fosslight_report/`: 스캔 결과 디렉토리
- `fosslight_report/*.json`: JSON 형식의 상세 결과
- `fosslight_report/*.xml`: JUnit 형식의 결과 (GitLab에서 테스트 결과로 표시)

## 문제 해결

### 파이프라인 실패 시
1. GitLab > CI/CD > Pipelines에서 실패한 job의 로그 확인
2. `.fosslightrc` 설정 파일 검토
3. 프로젝트 구조 및 파일 권한 확인

### 로컬에서 스캔 실패 시
```bash
# 상세 로그 확인
fosslight_scanner -p . -o fosslight_report --verbose

# 설정 파일 사용
fosslight_scanner --config .fosslightrc
```
