# AIFFEL Datatone
## 데이콘 : 전력사용량 예측 AI 경진대회 데이터를 이용하여 분석 [[Link]](https://dacon.io/competitions/official/235736/overview/description)

### code description
cluster.ipynb : 건물 유형과 특성이 예측에 미치는 영향에 대한 분석  
data.ipynb : 태양광과 비전력 냉방기가 전력 사용량에 미치는 영향 분석  
energy : 학습 데이터가 포함된 폴더  

### 1. 프로젝트 개요

    - 전력 수요 예측에 있어 건물의 특징이 예측에 주는 영향과 비전력 냉각, 태양력 발전의 효과 분석  

### 2. 데이터 소개

- train 데이터 : 60개 건물들의 2020년 6월 1일 부터 2020년 8월 24일까지의 데이터
- 1시간 단위로 제공
- 전력사용량(kWh) 포함

### 3. 가설 검증

#### 가설 1 : 건물의 용도와 특징이 수요 예측의 중요한 변수 일 것이다.

##### EDA통한 가설 검증

##### 시각화

목적 : 건물 별로의 특징의 차이를 확인

- 전력 사용 분류
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de75fbb9-bfb7-4a26-a824-1e347d9aa7cd/Untitled.png)
    
- 각 건물 별 target과 feature 사이에 상관 관계
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7dd365a0-cd2b-44b4-97a9-d8f8c3c6fb6e/Untitled.png)
    
- 용도 분류를 위한 요일 및 사용 시간 별 전력 사용량
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb4d6ad5-54ed-4330-9208-7d6df3bd4d78/Untitled.png)
    

##### 군집화

군집 생성 Feature : 

1. 요일 및 사용 시간 3시간 단위 전력 사용량 : 건물의 용도
2. 전력 사용량에 대한 각 건물 별 Feature의 상관 관계 : 건물의 재질, 건물의 특징
3. 태양광 발전과 비전력 냉각 장치의 유무 : 주어진 건물에 대한 정보 

- **Cluster 시각화 : hierarchical clustering**
    - 건물의 경우 용도가 계층적이라고 생각하여 이를 계층적으로 군집화진행
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9b37f86-f5f9-4bb2-9a63-c0e56fd9d80c/Untitled.png)
        
- **시간과 요일에 따른 cluster 별 전기 수요량**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fcabc844-037f-4cce-9a6c-b3108a30fc0e/Untitled.png)
    
    1. 평일과 약간에 새벽에 전력을 쓰는 경향 
    2. 평일에 9~18 ,주말에도 9~18이나 주말보다는 평일에 좀 더 많이 전력을 쓰는 경향  
    3. 저녁 시간과 주말에 더 많은 전기를 사용하는 경향
    4. 평일 9~18 주말에는 쉬는 경향 
    

- **군집 별 기상 요인에 따른 상관 관계**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fba782ee-fe08-4cdb-9f56-34ed126da7a2/Untitled.png)
    

##### 모델을 통한 가설 검증

건물의 용도와 특징이 

1. 요일과 시간에 따른 전력량을 통해 건물 유형을 군집화  → 건물의 유형을 분류
2. 군집화된 유형 별 데이터를 분석 
3. 모델 학습 및 분석
- 군집 유형을 제외한 모델과 군집 유형을 추가한 모델의 학습 결과
    
    군집화를 통한 건물 유형이 어느 정도의 성능 차이를 보이는 지를 통해 건물 유형의 중요성 수치화 및 시각화 
    
    - 성능 비교
        
        
        | 모델 | 성능 |
        | --- | --- |
        | 전체 | 26.15 |
        | 군집 | 21.26 |
        | 건물별 | 4.64 |
    - **결과분석 시각화 2 : 각 건물별 cluster별 변수 중요도**
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed687d84-d82d-4f6b-b895-fba87bb5fb0e/Untitled.png)
        

##### 결론

전력의 수요 예측을 위해서는 시간적 요인과 기후적 요인의 중요성도 높지만 건물에 대한 정보가 중요하다는 것을 확인

### 가설 2 :  태양광 발전은 전기 사용량 감소의 효과가 있다.

#### EDA를 통한 가설 검증

##### 시각화

- **태양광을 보유하고 있는 건물의 일조량(전일 일조량+금일 일조량)과 전력사용량**
    
    특정 건물에서 일조량과 전력사용량의 반비례 관계를 확인함
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f233cd2c-8c73-41f7-86d3-1b369ac92555/Untitled.png)
    
- **일조량의 기준을 2대8(파레토법칙)로 나뉜 후 평균 전력사용량 비교(청색 : 일조량 0.8이상, 황색 0.8미만)**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aadbc4e0-bf3e-4a2b-8dad-cd5472cafbd2/Untitled.png)
    
    → 추측과 다르게 대부분의 건물들이 일조량이 많은 날에 전력을 많이 사용한 것으로 판단됨
    

##### 상관 관계 분석

- **건물 군집간, 태양광 발전기 보유 여부간, 일일 평균 일조량의 상관 관계**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e5c002a1-5c30-4c80-956a-b5d2fc689d6f/Untitled.png)
    
- **분석 : cluster별 → 태양광 발전기 보유 여부 별 → 온도별 전력 사용량의 변화치를 LinearRegression을 사용하여 확인**
    
    → 가설이 맞다면 태양광 발전기를 보유하고 있는 건물의 변화치가 보유하고 있지 않은 건물의 변화치 보다 완만할 것으로 예상
    
    - 평일 주말 데이터를 모두다 사용
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/38431360-6f39-4865-827b-614f78b070f3/Untitled.png)
        
    - 평일의 데이터만 사용(주말과 평일의 전력사용량의 차이가 큰 건물들도 있기에)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b82b023e-b7fa-40eb-a5ac-044048382809/Untitled.png)
        
    - 주말의 데이터만 사용
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2200267c-1c60-4d15-9529-685e21f478e2/Untitled.png)
        
    
    → 가설과 달리 태양광 발전기를 보유하고 있는 건물의 변화치가 더 높았다
    
- 건물 용도별로 나눴으니 전기 사용 추세가 비슷할 것, 대부분의 건물에서 온도가 올라감에 따라 전력사용량이 많아짐을 확인 → 태양광의 유무를 나뉘어 온도에 따른 변화치를 보았을 때 태양광이 효과가 있다면 전력사용량의 변화치가 완만할 것으로 예상

##### 결론

- 분명 태양광의 효과가 있다고 보여지는 건물의 데이터들이 존재하지만 전체적으로 보았을 때는 현재 주어진 데이터로는 판별하기에 부족하다고 판단됨
- 이유 : 태양광의 크기나, 효율, 건물들의 특성 등 가설에 포함되는 여러가지 요소들이 존재하는데 태양광 발전기의 유무, 일조량 정도의 피쳐로는 가설을 검증하기에는 피쳐 수가 부족하다

### 가설 3 :  비전력 냉방기의 사용이 전력 소모 감소의 효과가 있다.

#### EDA를 통한 가설 검증

- 비전력 냉방기가 효과가 있다면 냉방 사용량과 관계있는 온도, 습도 등의 상관 관계가 감소할 것이다.
##### 상관 관계 분석

- 비전력 냉방기 사용이 전기 사용량에 영향을 미칠지 확인
    - 비전력 냉방기 보유여부간 일평균 온도, 전력 사용량간의 상관계수
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/09d2a47e-9593-4ce1-ae71-f00e1a53c60d/Untitled.png)
        
    - **분석 시각화 : 비전력 냉방기 유무에 따른 피쳐별 상관 관계**
        - 클러스터 간 비전력 냉방시 사용 여부에 따른 피쳐 요소
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/913b6feb-6cf4-4526-9c20-fa69ea5840aa/Untitled.png)
            
    - **분석 : 일평균 온도, 습도에 따른 일평균 전력의 상관 관계**
        - 비전력 냉방기 사용 집단과 비사용 집단의 일평균 온도에 따른 일평균 전력에  상관 관계(스피어 계수 값) 확인
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/775c6b00-ab90-4bed-9240-e2da57fb13c5/Untitled.png)
            
            → cluster3과 같이 비전력 냉방기를 보유하고 있는 건물이 없는 경우를 제외
            
            → cluster5를 제외하고는 비전력 냉방기를 보유하고 있는 건물이 아닌 건물들 보다 일평균 온도와 일평균 전력 사용량의 상관계수가  높다.
            
            → cluster3, 5를 제외하고는 가설과 반대되는 양상을 보인다.
            
        - 비전력 냉방기 사용 집단과 비사용 집단의 일평균 습도에 따른 일평균 전력에  상관 관계(스피어 계수 값) 확인
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68158ad9-e069-44b5-8b38-43ea8cf81d43/Untitled.png)
            
            → cluster3과 같이 비전력 냉방기를 보유하고 있는 건물이 없는 경우를 제외
            
            → cluster3을 제외하고는 모두 가설과 반대되는 양상을 보인다.
            
- **환경적인 불쾌지수 지표 사용**
    - 기온, 풍속, 습도, 강수량, 일조의 변수를 모두 사용하여 환경적인 불쾌지수라는 새로운 지표 생성
    - 클러스터마다의 불쾌지수별 전기 사용량
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a30f22fc-f8e1-4f59-b391-0d63e2b7d0e6/Untitled.png)
        
    
    → 아닌 건물들도 존재하지만 대부분의 건물들이 환경적 불쾌지수의 단계가 높아질수록 전력 소비량이 많아짐을 확인할 수 있다.
    
    - 비전력 냉방기 보유여부간 환경적인 불쾌지수와 일평균 전력의 상관계수
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ff362c49-7e44-4fa4-ab56-16bdb74f9c8d/Untitled.png)
        

### 결론

- 태양광 가설보다 어느정도 상관관계가 있다는 것을 확인할 수 있었지만 여전히 태양광 가설때와 같은 아쉬운 점이 있다.  단지 비전력 냉방기의 유무가 아닌 비전력 냉방기의 개수나 성능 전력냉방기의 개수 등 다른 요인의 피쳐가 있어야 더 정확한 분석이 가능할 것이라고 생각된다.
