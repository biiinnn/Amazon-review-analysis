# Project-Amazon-review-analysis
2020 Fall - Project in Unstructured Data Analysis Class - SeoulTech, Department of Data Science

## 온라인 상품평 분석

### 분석 목표 - 네트워크 분석, 감성분석을 활용하여 제품에 대한 온라인 상품평을 분석하고자 함
1. 전반적인 상품평에서 나타나는 상품의 특성과 다수의 추천을 받은 상품평에서 나타나는 상품의 특성 비교
2. 각 특성 별로 감성분석을 통해 전체적인 만족도를 사이트에서 제공하는 평균 평점보다 더 세분화하여 표시

### dataset 설명
Amazon review data: "Sunny Health & Fitness SF-B901 Pro Indoor Cycling Excellence Bike" 제품에 대한 리뷰를 크롤링을 통해 수집  
- 선정이유: 다량의 리뷰 데이터 포함, 평점이 고르게 분포되어 있음  
- 데이터 출처: https://www.amazon.com/Sunny-Spin-Bikes-Health-Fitness-Indoor-Cycling/product-reviews/B002CVU2HG/ref=cm_cr_arp_d_paging_btm_next_2?ie=UTF8&reviewerType=all_reviews  
- ```selenium.webdriver``` 와 ```BeautifulSoup``` 사용 -> ```crawler.ipynb```  

  **Crawling feature**
  | Data    | Type |
  | ------- | ---- |
  | title   | str  |
  | text    | str  |
  | star    | int  |
  | helpful | int  |

### 분석 과정
1. 데이터 전처리  
  - 영어가 아닌 문자 공백으로 변환 후 불필요한 공백 제거  
  - 모든 알파벳 소문자화  
  - nltk의 English stopwords를 사용하여 불용어와 구두점 제거  
  - Stemming (SnowballStemmer, LancasterStemmer PorterStemmer 사용)  
  - Lemmatization -> 명사, 형용사, 동사, 부사 추출  
  - 한 줄로 합치기   
  예시) 
    ![preprocessing](https://user-images.githubusercontent.com/46666833/163947613-63347170-2032-4655-a71f-127c12cf1a17.PNG)    
  => ```Preprocessing.ipynb```
  
  - TF-IDF 기반 키워드 매트리스 생성, gephi 입력용 인접행렬 생성 => ```Network.ipynb``` -> ```to_gephi.csv```
2. 네트워크 분석
  - 전체 리뷰에서 나타난 단어   
    ![network1](https://user-images.githubusercontent.com/46666833/163948528-0fccf092-dfe3-4a9c-a53b-5c275d8f554f.PNG)  
  - helpful 1 이상에서 나타난 단어    
    ![network2](https://user-images.githubusercontent.com/46666833/163948592-9d4de901-13c7-4eb2-a90d-5e9b8e3d9cff.PNG)
3. 감성 분석
  - 속성별 단어 사전 구축
    - 네트워크 분석 기반 카테고리를 세분화하고 word2vec을 활용하여 각 카테고리에 맞는 키워드를 선정
      ![word](https://user-images.githubusercontent.com/46666833/163949225-06476baa-0774-4706-bbf8-9c2ac45d0929.PNG)
  - 키워드 기반 ngram 추출
    - 추출된 문장 리스트 별로 감성 분석 시행 (VADER 감성 분석기 사용) => ```all_sentiment.ipynb```, ```helpful_sentiment.ipynb```  
    - 만족도 계산 식을 통해 각 속성별 만족도 도출  
    ![cal](https://user-images.githubusercontent.com/46666833/163949607-758fb572-1804-46ea-a7a9-967f21dd6fde.PNG)

### 분석 결과
 ![result1](https://user-images.githubusercontent.com/46666833/163949960-99c158a5-bcee-4e0e-be64-676d3f9436a4.PNG)  
 ![visual](https://user-images.githubusercontent.com/46666833/163949989-32fccc48-c30c-4733-b8ac-f189114f7dee.png)

### 요약 및 한계점
- 속성별 시각화를 통해 상품에 대한 평가를 보다 자세하게 할 수 있음
- 추천 수가 높은 리뷰에 대한 분석을 통해 기존 시스템의 평균 평점보다 더 객관적이고 유용한 정보를 얻을 수 있음
- 한계) 
  - 특정 상품에 대한 분석으로 더 많은 사례에 적용하는 것이 필요함
  - 속성어 사전 구축 작업이 수작업으로 이루어짐
  - 기존 시스템이 속성별 점수를 부여하고 있지 않기 때문에 본 연구에서 수행한 감성 분석 결과에 대한 정확도 평가가 어려움
  - 좋아요 수를 반영한 리뷰가 전체 리뷰와 차이가 있음을 확인하는데 그침 -> 추후 연구에서 좋아요 수에 가중치를 부여해 분석하는 연구가 기대됨


