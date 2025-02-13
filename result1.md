# RAG Optimization

## 실험 데이터 

### 액셀데이터 추출기준

* 파일: "대화이력_피드백내역_9월_v0.65_20241015" > GI+RAG_1015 
* 필터
  * Language_cd = en
  * Type = RAG
  * 유형 = 검색
  * Status = InProgress-HQ

### 데이터 선정

* 51건 queries_all.json

* 샘플링: 16건 queries.json

* 임베딩2, 3 모델로 Relevance Score 4점 이하 질문들: queries_low_score.json (10건)
    * 이 데이터를 개선하는 것을 목표로 함.

* 베이스라인 스코어

![베이스라인](image/04.%20Baseline_lowscore.png)

## Embedding 모델 변경

* embedding3 large model로 변경

![임베딩라지3](image/05.%20Embedding%20Large%203.png)

## 제목, 콘텐츠 멀티 벡터

### 스레스홀드 및 가중치

* 예시 멀티벡터 가중치
  * 제목 필드 벡터 가중치: 0.5, 콘텐츠 필드 벡터 가중치: 2
 
* 데이터 수집 파이프라인을 콘텐츠, 제목 필드로 분리되도록 병경할 수 없어서 수행하지 않음.

## 하이브리드(BM25 + Vector) + 시맨틱 리랭킹 w ada2 

![리랭킹](image/03.%20Sementic%20Reranker%20Score.png)

## 하이브리드(BM25 + Vector) + 시맨틱 리랭킹 w embedding large3

![BM25, vector, SM embed3](image/07.%20BM25%20Vector%20Semantic%20embedding3.png)

## 시맨틱 리랭킹 w embedding large 3

![라지3+시맨틱](image/06.%20Embeding%20Large%203%20w%20Semantic.png)
l
## Query Rewrite, 하이브리드, 시맨틱 리랭킹 w embed3

![qrw hybrid semantic emb3](image/08.%20QRW%20Hybrid%20Semantic%20Embed3.png)

* Query Rewrite기능은 North Europe, Southeast Asia만 현재 지원 가능.

>[!Note]
>
>https://learn.microsoft.com/en-us/azure/search/semantic-how-to-query-rewrite#prerequisites


## 결론

다양한 검색기법을 테스트 해 본 결과 `text-embedding-3-large`로 벡터검색만을 사용했을 경우가 가장 품질이 좋게 나타남. 

### 제약사항 및 운영 적용방안
1. 불량질문 데이터의 모수가 크지 않고 실제 쿼리의 답변이 인덱스에 있는 지에 대한 검증은 하지 않고 오직 Relevance Score에만 품질 지표를 의존함. 따라서 결과 데이터의 ROW값을 분석하여 실제 얼마나 적절한지에 대한 검수가 전수 혹은 샘플링 방식으로 이루어져야 함.   
2. TPM제한으로 데이터 셋 샘플 규모가 적음 (전체 16건)
3. 수작업 Query Rewrite는 수행하지 않음.
4. RAG의 대답이 적절하지 않은 답변 세트들을 수집하는 방안에 대한 고민이 필요. 그리고 이렇게 수집된 질문들은 별도의 json으로 축적 관리하여 갱신될 때 마다 Evaluation Pipeline을 구동하게 해애함. 여기서 수집된 Evaluation결과는 Azure AI Project내에서 중앙 집중 관리하여 개선 추이를 지속적으로 모니터링 해야 함. 

### 2차 결과
[데이터셋을 확장하여 수행한 2차 결과는 여기](./result2.md)

