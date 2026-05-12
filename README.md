# FRAGMNT - AI 향수 추천 플랫폼
> ⚠️ 본 레포지토리는 팀 프로젝트 중 제가 담당한 기능을 중심으로 정리한 개인 포트폴리오입니다.  <br/>
AI 기반 향수 추천 플랫폼 **FRAGMNT**의 백엔드 리더로 참여하여  
추천 엔진 설계, AI 안정화, 공유 시스템, 추천 알고리즘 최적화를 담당했습니다.

---

## 🔗 Repository

- Team Repository
- FRAGMNT Backend Repository

---

# 📌 프로젝트 소개

FRAGMNT는 사용자의:

- 설문 데이터
- 키워드
- 이미지
- 챗봇 대화

를 기반으로 AI와 추천 알고리즘을 결합하여  
개인 맞춤 향수를 추천하는 서비스입니다.

---

# 👨‍💻 담당 역할

## Backend Leader

### 담당 영역

- AI 추천 시스템 설계
- 추천 알고리즘 구현
- Gemini API 연동
- AI Retry / Fallback 아키텍처 설계
- OG 공유 시스템 구현
- 추천 성능 최적화
- 백엔드 API 구조 설계
- 프론트엔드 협업
- 인프라 및 배포 협업

---

# 🛠 Tech Stack

## Backend

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white"/>
  <img src="https://img.shields.io/badge/DRF-A30000?style=for-the-badge&logo=django&logoColor=white"/>
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Gunicorn-499848?style=for-the-badge&logo=gunicorn&logoColor=white"/>
</p>

## AI / Infra

<p>
  <img src="https://img.shields.io/badge/Gemini_AI-4285F4?style=for-the-badge&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/AWS_S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white"/>
</p>

---

# 🚀 핵심 기능

## 1. AI 추천 엔진

사용자 설문 데이터를 기반으로  
5차원 취향 벡터를 생성하고 향수 프로필과 매칭하는 추천 엔진을 구현했습니다.

### 추천 흐름

1. 설문 답변 정량화
2. 사용자 취향 벡터 생성
3. 향수 프로필 벡터와 거리 계산
4. 가장 유사한 향수 추천

---

## 2. 유클리드 거리 기반 추천 시스템

사용자 취향 벡터와 향수 프로필 간의  
유클리드 거리(Euclidean Distance)를 계산하여 추천 정확도를 향상시켰습니다.

```python
def distance(p1, p2):
    return math.sqrt(sum((p1[k] - p2[k])**2 for k in p1))
```

### 특징

- 단순 키워드 매칭 제거
- 데이터 기반 추천 강화
- 추천 정확도 개선

---

## 3. 정확도 + 다양성 추천 전략

Top-3 후보군 중 랜덤 추천 방식을 도입하여:

- 추천 신뢰성 유지
- 반복 추천 감소
- 사용자 경험 다양성 확보

```python
return random.choice(sorted_scents[:3])
```

---

# 🧠 AI 안정성 아키텍처

## 4. Retry + Fallback Pipeline

Gemini API 응답 실패 및 Timeout 상황 대응을 위해  
3-Step Retry & Fallback 구조를 설계했습니다.

### 해결한 문제

- AI API Timeout
- 외부 서버 지연
- 503 오류
- 추천 서비스 중단 문제

### 해결 방식

- 최대 3회 Retry
- Main/Sub 모델 Fallback
- 최종 실패 시 베스트셀러 추천

```python
for current_model in model_lineup:
    for attempt in range(max_attempt):
        try:
            ai_result = gemini.analyze()
            break
        except Exception:
            time.sleep(2)
```

### 결과

- 서비스 중단 방지
- AI 장애 대응력 강화
- 사용자 경험 안정화

---

# ⚡ 추천 성능 최적화

## 5. Vector-like Scoring Filter

Gemini AI 응답 속도 개선을 위해  
사용자 데이터를 수치화 후 상위 후보만 AI에 전달하는 구조를 설계했습니다.

### 해결한 문제

- AI 응답 30초 이상 지연
- Timeout 발생
- API 비용 증가

### 개선 방식

- 사전 필터링
- 상위 유사 데이터만 전달
- AI 입력 데이터 최소화

### 결과

- 응답 속도 개선
- API 비용 최적화
- Timeout 감소

---

# 🌐 공유 시스템

## 6. OG Crawler 공유 시스템

카카오톡 및 SNS 공유를 위한  
OG(Open Graph) 메타데이터 제공 시스템을 구현했습니다.

### 주요 기능

- 공유 URL 생성
- Hashids 기반 ID 암호화
- 7일 TTL 만료 관리
- 동적 OG Metadata 제공

```python
question_id = encode_id(result_id)
expires_at = timezone.now() + datetime.timedelta(days=7)
```

### 특징

- 내부 DB ID 보호
- 만료 링크 관리
- 소셜 미리보기 지원

---

# 🏗 아키텍처 개선

## 7. Transaction 최적화

외부 API 통신 완료 이후에만  
`transaction.atomic()`을 실행하도록 구조를 개선했습니다.

### 목적

- DB Lock 최소화
- Connection 점유 시간 감소
- 동시성 안정성 향상

```python
with transaction.atomic():
    resource.save()
```

---

# 🧩 문제 해결 경험

## AI Timeout 문제

### 문제

Gemini AI 응답 생성 과정에서  
30초 이상 소요되어 사용자 경험 저하 발생.

### 해결

- Vector-like Filtering 도입
- 입력 데이터 최소화
- 후보군 선별 구조 설계

### 결과

- 응답 시간 개선
- Timeout 감소

---

## AI API 장애 대응

### 문제

외부 AI 서버 상태에 따라  
간헐적으로 추천 기능 전체 실패 발생.

### 해결

- Retry + Fallback 구조 구현
- 장애 발생 시 베스트셀러 추천 제공

### 결과

- 무중단 추천 서비스 구현

---

# 📚 프로젝트를 통해 배운 점

이번 프로젝트를 통해:

- AI는 항상 예측 가능하게 동작하지 않는다는 점
- 장애 상황을 고려한 설계의 중요성
- 추천 알고리즘 최적화
- 실제 서비스 운영 환경에서의 안정성 확보
- 프론트엔드 / 인프라 협업 경험

을 깊이 경험할 수 있었습니다.

특히 단순 기능 구현보다:

- 장애 대응
- 안정성
- 성능 최적화
- 사용자 경험 유지

를 고려한 백엔드 설계 역량을 키울 수 있었던 프로젝트였습니다.

---

# 🔑 주요 키워드

- Recommendation System
- AI Integration
- Retry / Fallback Architecture
- Recommendation Optimization
- OG Crawler System
- Transaction Optimization
- API Stability
- Share System Design

---

# 📝 프로젝트 회고

초기 설계 단계에서:

- 에러 포맷 규격화
- API 응답 구조 표준화
- 추천 로직 정확도 개선

에 더 많은 시간을 투자했다면  
협업 및 유지보수 효율을 더욱 높일 수 있었을 것이라 생각합니다.

하지만 실제 서비스 수준의:

- AI 장애 대응
- 공유 시스템
- 추천 엔진
- 운영 안정성

을 직접 경험하며  
단순 CRUD를 넘어선 백엔드 아키텍처 경험을 얻을 수 있었습니다.

---

# 🎥 Demo

- Live Demo
- FRAGMNT Demo

---

# 📂 프로젝트 구조 예시

```bash
FRAGMNT
├── apps
│   ├── analysis
│   ├── chatbot
│   ├── core
│   ├── question
│   ├── scent
│   └── users
├── config
├── docker
├── nginx
├── requirements
└── manage.py
```

---

# 📈 핵심 성과

- AI 장애 대응 가능한 추천 시스템 구축
- Timeout 감소 및 응답 속도 개선
- 추천 다양성 향상
- SNS 공유 시스템 구현
- 실제 서비스 운영 수준의 안정성 경험

---

# 🙌 Collaboration

- Frontend 협업
- Infra 협업
- API 설계 및 문서화
- 추천 로직 개선 회의 진행
- 배포 및 운영 안정화 협업

---

# 📄 License

This project is for educational and portfolio purposes.
