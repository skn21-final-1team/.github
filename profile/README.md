## 1. 프로젝트 주제
- "사용자의 브라우저 북마크 및 웹 URL을 지식 베이스로 활용하는 오픈소스 RAG(검색 증강 생성) 서비스"  
- 단순한 링크 저장을 넘어, 저장된 웹 콘텐츠의 내용을 분석하고 사용자의 질문에 근거(Source)를 제시하며 답변하는 지능형 도우미 구축.  



## 2. 문제 정의
- 정보의 파편화: 유익한 아티클을 북마크에 저장해도 나중에 다시 찾아 읽거나 내용을 요약하기 어려움.  
- 기존 서비스의 폐쇄성: NotebookLM 등은 훌륭하지만 데이터 프라이버시나 모델 선택의 자유도가 낮음.  
- 비용 부담: 유료 LLM API를 사용하여 개인 지식 베이스를 구축하기에는 지속적인 비용 발생 우려.  
- 해결책: 무료/오픈소스 모델(Llama 3, Mistral 등)을 활용하여 누구나 무료로 개인 서버나 로컬에서 운영할 수 있는 환경 제공.  




## 3. 시장조사 및 BM
### 3.1 시장 조사
- 타겟: 연구자, 개발자, 대학생 등 방대한 웹 정보를 수집하지만 관리에 어려움을 느끼는 사용자.  
- 유사 서비스: Google NotebookLM, Perplexity, Coral(Cohere).  
- 차별점: '북마크'라는 기존 습관을 데이터 소스로 활용하며, 완전한 오픈소스 및 로컬 실행 옵션 제공.  

### 3.2 BM (Business Model)  
- Open Source Strategy: 핵심 엔진은 Github에 공개하여 커뮤니티 기여 유도.  
- B2B 확장: 기업 내 내부 위키/문서 링크를 기반으로 한 사내 지식 베이스 솔루션으로 커스텀 제공.  
- Managed Service: 인프라 관리가 어려운 사용자를 위해 소액의 구독형 호스팅 서비스(SaaS) 제공 가능.    

  

## 4. 시스템 구성 기획
서비스는 크게 크롤러, 벡터 DB, LLM 엔진으로 구성됩니다.
- Frontend: Next.js (사용자 인터페이스 및 북마크 업로드)  
- Backend: FastAPI (Python 기반의 고속 API 서버)  
- Crawler: Playwright 또는 BeautifulSoup (URL 콘텐츠 추출)  
- Database: * Vector DB: 텍스트 임베딩, 사용자 정보, 
- Metadata DB: PostgreSQL (사용자 정보 및 북마크 메타데이터)  

 

## 5. 모델링 계획 (무료/오픈소스 중심)
비용 제로화를 위해 최신 오픈소스 모델을 활용합니다.  
- LLM (추론): Llama 3 (8B/70B): Groq API(무료 티어) 또는 Ollama를 통한 로컬 실행.  
- Mistral-7B: 한국어 성능이 준수한 튜닝 모델 활용.  
- Embedding (벡터화): * HuggingFace의 BGE-M3 (다국어 지원 및 한국어 성능 우수)  
- Framework: LangChain 또는 LlamaIndex를 사용하여 RAG 파이프라인 구축.  

  

## 6. 사용 데이터
- 사용자 제출 북마크: 브라우저에서 내보낸 HTML/JSON 파일 내 URL 정보.  
- 웹 스크래핑 데이터: 제출된 URL의 텍스트 본문, 메타데이터(제목, 설명).  
- 사용자 쿼리: 지식 베이스에 질문하는 자연어 텍스트.  

## 7. 역할 분담 (R&R)
| 역할 (Role) | 담당 주요 기능 (Key Responsibilities) |
| :--- | :--- |
|박내은, 우재현, 최자슈아주원, 이명준| **Project Manager / 기획** | - 프로젝트 일정 관리 및 요구사항 우선순위 확정<br>- 오픈소스 라이선스 검토 및 BM 기획 |
|박내은, 최자슈아주원| **Frontend Developer** | - Next.js 기반 UI 구현 및 북마크 폴더 트리 시각화<br>- 퀴즈, 플래시 카드, 요약 결과 인터랙션 개발 |
|박내은, 우재현, 이명준| **Backend Developer** | - 계정 관리(OAuth) 및 북마크 데이터 CRUD API 개발<br>- 크롤링 예외 처리 및 서비스 비즈니스 로직 서버 구축 |
|박내은, 우재현, 이명준| **AI/ML Engineer** | - 오픈소스 LLM(Llama 3 등) 연동 및 RAG 파이프라인 설계<br>- 요약, 퀴즈 생성, 표 정리 등을 위한 프롬프트 최적화 |
|박내은, 이명준| **Data/DevOps Engineer** | - 웹 크롤링 엔진 개발 및 벡터 DB(ChromaDB 등) 인프라 구축<br>- Docker 기반 서비스 배포 및 가용성 관리 |
