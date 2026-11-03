# 로컬 RAG 비교 실습 — 2026 교과과정 책자

Ollama + Poetry + Jupyter 로 로컬 RAG 파이프라인의 각 단계(파서·청킹·임베딩·벡터 DB·검색·생성)를
**같은 평가셋으로 비교**하며 장단점을 확인하는 실습 자료입니다.

## 준비

```bash
# 1) Ollama 설치 후 모델 받기
ollama pull bge-m3
ollama pull nomic-embed-text
ollama pull mxbai-embed-large
ollama pull snowflake-arctic-embed2
ollama pull qwen3-embedding:0.6b

# 2) OpenAI API 키 (답변 생성 단계용) — 프로젝트 폴더에 .env 파일 생성
echo 'OPENAI_API_KEY=sk-...' > .env

# 3) 파이썬 환경
poetry config virtualenvs.in-project true --local
poetry install --no-root
poetry run jupyter lab      # local_rag_lab.ipynb 열기 (VSCode 는 .venv 커널 선택)
```

## 구성

| 경로 | 설명 |
|---|---|
| `local_rag_lab.ipynb` | 실습 노트북 — 직접 구현 버전 (1~9장) |
| `local_rag_lab_langchain.ipynb` | 같은 비교를 **LangChain** 으로 구현한 버전. PDF 로더 6종 비교 포함 |
| `data/curriculum_2026.pdf` | 실습 대상 책자 |
| `data/qa_eval.json` | 평가용 QA 25문항 (정답 근거 페이지·키워드 포함) |
| `.cache/` | 임베딩·PDF 파싱 캐시 (삭제하면 처음부터 다시 계산) |

## 참고

- 임베딩·검색·벡터 DB 는 로컬(Ollama 등), **답변 생성만 OpenAI API** 를 씁니다. 7·8장에서 질문과 검색된 책자 문단이 OpenAI 로 전송됩니다.
- 생성 모델은 노트북 1.2 셀의 `GEN_MODELS` 에서 바꿉니다 (`"openai:모델명"` / `"ollama:모델명"`).
- 첫 실행은 임베딩 계산 때문에 오래 걸립니다 (캐시 이후엔 빠름). LangChain 버전은 PDF 로더 6종 파싱(약 4분)이 추가됩니다.
- 사양이 낮으면 노트북 1.2 셀의 `EMBED_MODELS` / `GEN_MODELS` 를 줄여서 사용하세요.
