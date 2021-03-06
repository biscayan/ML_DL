# Chapter 2. 모델 평가 및 선택

## 2.1. 경험 오차 및 과적합
- 오차율 : 전체 샘플 수와 잘못 분류한 샘플 수의 비율
- 정밀도 : 1 - 오차율
- 오차 : 학습기의 실제 예측 값과 샘플의 실제 값 사이의 차이
- 훈련 오차 : 학습기가 훈련 세트상에서 만들어낸 오차
- 일반화 오차 : 학습기가 새로운 샘플 위에서 만들어낸 오차
- 과적합 : 학습기가 훈련 데이터에서 학습을 과도하게 잘하여 훈련 데이터 중의 일정한 특성을 모든 데이터에서 내재된 일반 성질이라 오해하는 현상. 과적합이 발생하면 일반화 성능이 떨어지게 된다.
- 과소적합 : 학습기가 훈련 데이터의 일반화 성질을 제대로 배우지 못하는 현상
- 과적합은 학습능력이 너무 뛰어나 훈련 데이터들이 가진 일반적이지 않은 특성까지 학습하게 되며, 과소적합은 학습능력이 좋지 못해 발생한다.
- 과소적합은 훈련을 충분히 하여 해결할 수 있지만, 과적합은 피할 수 없다. 따라서 과적합을 완화하고 과적합이 일으키는 위험을 최소화하는 것에 만족해야 한다. 

## 2.2. 평가 방법
훈련 데이터로 학습된 학습기는 테스트 데이터를 활용하여 테스트 오차를 계산하고, 이를 실제 일반화 오차의 근삿값으로 가정한다. 이 때, 테스트 세트는 훈련 세트에서 사용한 것이 아니어야 하며, 훈련 과정에서 사용된 것이면 안된다. 데이터 세트를 훈련 세트와 테스트 세트로 나누는 방법은 다음과 같다.  
### 2.2.1. 홀드아웃
홀드아웃 방법은 데이터 세트를 겹치지 않는 임의의 두 집합으로 나눈다.  
훈련/테스트 세트를 나눌 때 되도록이면 데이터 분포가 같게 나눠야 한다. 그렇지 않으면 데이터 분포의 편향으로 인해 원치 않은 결과를 얻을 수 있다. 훈련/테스트 시트의 비율을 설정하는 것 외에도, 다양한 방식으로 데이터 세트에 대한 검증 세트 분류를 진행해야 한다. 검증 세트 기법의 예측값은 늘 불안정하여, 일반적으로 여러 번에 걸쳐 분류를 진행하고 검증하는 방법을 택한다.  
우리가 평가하고 싶은 것은 데이터 세트를 훈련시킨 모델의 성능이기 때문에, 검증 세트 기법을 사용한다면 일종의 딜레마에 빠질 수밖에 없다. 만약 훈련 세트에 대다수의 샘플이 들어 있다면 만들어진 모델은 데이터 세트 전체를 훈련시킨 것과 비슷하겠지만, 테스트 세트가 적은 탓에 안정적인 평가 결과를 얻지는 못한다. 반대로, 테스트 세트가 많은 샘플을 가져갈수록 훈련 세트와 데이터 세트의 차이는 벌어지고, 이는 데이터 세트 전체를 이용하여 훈련한 모델과 차이를 크게 만든다. 즉, 편과 결과의 신뢰도를 떨어뜨린다.  
어떻게 나눌 것인가에 대한 완벽한 해답은 없지만 일반적으로 60~80% 정도의 데이터를 훈련 세트로 사용하고 나머지를 테스트 세트로 분리하여 사용하는 것이 좋다.
### 2.2.2. 교차 검증
교차 검증은 데이터 세트를 k개의 서로소 집합으로 나누는 것으로 시작한다. 교차검증은 k-1개의 부분집합들을 훈련 세트로 사용하고 나머지 하나의 부분집합을 테스트 세트로 사용한다.  이렇게 하면 k개의 훈련/테스트 세트가 만들어지고, k번의 훈련과 테스트를 거쳐 k개의 테스트 결괏값의 평균을 얻을 수 있다.  
교차 검증법을 통한 평가 결과의 안정성과 정확도는 k의 값에 따라 달라진다. 이러한 점을 강조하기 위해 이러한 교차 검증을 k겹 교차 검증이라 부른다. k는 일반적으로 10으로 두고 검증하며, 10과 더불어 5와 20이라는 값도 자주 쓰인다.  
샘플을 나누는 과정에서 생길 수 있는 차별을 최소화 하기 위해 k겹 교차 검증은 일반적으로 p번을 랜덤하게 반복하여 나누어 진행한다. 즉, 최종 평가 결과는 p번의 k겹 교차 검증을 실행한 값의 평균이다. 주로 '10차 10겹 교차 검증'을 자주 사용한다.  
만약 m개의 샘플이 있는 데이터 세트를 k = m으로 설정하고 교차 검증을 실행한다면, 이러한 교차 검증은 LOOCV(Leave One Out Cross Validation)라고 부른다. LOOCV를 활용한 방법은 편향이 작다는 장점이 있다. 하지만 LOOCV는 데이터 세트의 크기가 매우 클 때, 모델을 m번 적합해야 하므로 계산량이 많아진다는 단점이 있다. 그리고 LOOCV가 다른 평가 방법보다 늘 좋은 성능을 내는 것도 아니다.  
### 2.2.3. 부트스트래핑
부트스트래핑은 부트스트랩 샘플링에 기반을 둔 샘플 추출 기법이다. m개의 샘플이 있는 데이터 세트 D를 가정한다면, 샘플링을 통해 데이터 세트 D-를 만들 수 있다. 매번 D에서 샘플 하나를 꺼내 D'에 복사하여 넣는다. 그리고 다시 원래의 데이터 세트 D로 돌려보낸다. 이렇게 한다면 한 번 뽑혔던 샘플도 다시 뽑힐 가능성이 생기게 된다. 이러한 과정을 m번 반복한 후, m개의 샘플이 들어 있는 데이터 세트 D'를 얻는다. 이것이 부트스트래핑의 결과이다.  
당연히 D의 일부 샘플은 D'에 반복 출현할 수 있고, 어떤 샘플은 아예 뽑히지 않을 수도 있다. 수학적으로 계산해 본다면, m번의 채집 과정 중 샘플이 한 번도 뽑히지 않을 확률은 (1-1/m)m이다. 즉, 부트스트래핑을 사용한다면 데이터 세트 D 중의 36.8%의 샘플은 D-에 들어가지 못한다고 수학적으로 계산할 수 있다. 우리는 D'를 훈련 세트로, D/D'를 테스트 세트로 활용한다. 이렇게 한다면 m개의 샘플을 모두 활용하여 모델 훈련에 사용할 수 있고, 활용하지 못한 1/3에 해당하는 샘플들은 테스트 샘플로 활용할 수 있다. 이러한 테스트를 Out-of-Bag 예측이라고 부른다.  
부트스트래핑은 데이터 세트가 비교적 적거나, 훈련/테스트 세트로 분류하기 힘들 때 사용하면 좋다. 다만, 부트스래핑으로 생성된 데이터 세트들은 초기 데이터의 분포와 다를 수 있으므로 편향을 크게 만들 수 있다. 따라서 초기 데이터량이 충분할 때는 일반적으로 홀드아웃과 교차 검증 기법을 더 자주 활용한다.  
### 2.2.4. 파라미터 튜닝과 최종 모델
대부분의 학습 알고리즘에는 조율해야 하는 파라미터가 있다. 파라미터를 어떻게 설정하는가에 따라서 학습모델의 성능은 큰 차이를 보인다. 따라서 모델 평가 및 선택 시 학습 알고리즘의 선택뿐만 아니라 알고리즘 파라미터에 대한 설정도 고려해야 한다. 이러한 과정을 파라미터 튜닝이라고 한다.  
모든 파라미터를 훈련을 통해 모델에 적합시키기는 어렵다. 그리고 적합된 파라미터가 최적의 파라미터가 아닐 수도 있다. 이는 단지 계산량과 성능 예측을 위한 목표 사이에서 절충한 결과이다. 이러한 절충을 통해서만 학습을 진행할 수 있다. 또한, 모델 선택과 파라미터 조율을 위해, 테스트 데이터를 활용하여 성능을 측정하기 전에 검사할 수도 있는데, 모델 평가 및 선택 과정에서 쓰는 평가 테스트 데이터 집합을 검증 세트라고 부른다.