# RAG 최적화 샘플

Azure AI Search의 Capability만을 이용하여 RAG 앱의 품질 지표가 향상되는지 확인

## 데이터셋

* 데이터셋과 환경설정은 포함하지 않음. 다만 Evaluation을 위한 샘플 question set과 환경변수 파일은 다음을 참고

1. queries.json

```json

[
    "query": "Let me know the address of service center", "category": "Service"
]
```

2. .env

```sh
AZURE_SEARCH_SERVICE_ENDPOINT=https://<yours>.search.windows.net
AZURE_SEARCH_INDEX=<yours>
AZURE_SEARCH_ADMIN_KEY=<yours>
AZURE_OPENAI_ENDPOINT=https://<yours>.openai.azure.com/
AZURE_OPENAI_KEY=<yours>
AZURE_OPENAI_ADA002_EMBEDDING_DEPLOYMENT=text-embedding-ada-002
AZURE_OPENAI_3_LARGE_EMBEDDING_DEPLOYMENT=text-embedding-3-large
AZURE_OPENAI_GPT_DEPLOYMENT=gpt-4o
AZURE_AI_PROJECT_CONN_STR=<yours>
AZURE_SUBSCRIPTION_ID=<yours>
AZURE_RESOURCE_GROUP_NAME=<yours>
AZURE_PROJECT_NAME=<yours>
```

## 1차 수행결과

[1차](./result1.md)

## 2차 수행결과

[2차](./result2.md)

## Continuous Improvement 제언

* RAG 및 RAG를 위한 검색 최적화는 1회성으로 달성 할 수 없음. 따라서 피드백 점수 낮은 질문/답변 세트의 지속적 확보와 이를 프롬프트 엔지니어링이나 파인튜닝, 검색 인덱싱등의 작업을 수행해야 하며 LLMOps Pipelining을 통해 지속적 평가, 결과 확인, 개선작업을 수행해야함. 

* LLMOps Pipeline을 구현하기 위한 참조 프로젝트: https://github.com/HakjunMIN/llmops-content
* Evaluation을 위한 합성데이터 생성과 자동화 참조 프로젝트: https://github.com/HakjunMIN/e2e-rag-evaluation