# Machine-Learning Fundamentals



Referenced From : [Click](https://towardsdatascience.com/machine-learning-basics-part-1-a36d38c7916#:~:text=Machine%20Learning%20is%20an%20application,be%20at%20least%20human%20level.&text=In%20order%20to%20perform%20the,from%20the%20data%2Dset%20provided.)



위 링크에 있는 내용을 그대로 내용 간략하게 요약하였습니다.



### What is Machine Learning?

* Arthur Samuel, AI 전문가(명시적 프로그래밍 없이 컴퓨터에게 배울 능력을 갖추게 하는 연구 분야)
* Alan Turing : Machine Learning 표준 도입
* Def. 과거의 데이터로 부터 미래의 예측값을 도출하도록 컴퓨터에게 학습을 시키고, 컴퓨터의 성능은 human level 과 비슷해야 한다.



### Machine Learning Categories



Supervised Learning

* 예시 데이터에 라벨링을 추가한다.
* classification : 컴퓨터가 구분된 값을 배우도록 만듬. 
* regression : 연속적인 변수 값을 예측하도록 함.
* Tech.
  * Linear Regression 
    * y = wx + b
      * x : vector
      * w : weight(예측 가중치)
      * b : bias
    * MAE(Mean Absolute Error) = (예측값 - 실제값) 들의 평균
    * MSE(Mean Squared Error) = (예측값 - 실제값) 제곱들의 평균
    * Goal : MAE or MSE 를 줄이기 위해서 w 값을 잘 설정한다.
    * w 는 비용 함수와 같음(cost function)
  * Logistic Regression
    * 반응 변수가 정규화되지 않은 상태로 퍼짐.
    * 값이 0~1 사이에 오도록 공식을 적용
    * g(z) = 1 / ( 1 + e^-Z )
  * Gradient-Descent Algorithm
    * cost 를 줄이기 위한 방법 찾기
    * minimum cost 를 위한 repeat (2차형 곡선)
    * Tech
      * Batch gradient descent : 매 반복마다 모델 파라미터 업데이트하기 위해 모든 트레이닝 데이터 사용
      * Mini-batch gradient descent : 모든 예시 샘플 대신, 훈련용 데이터를 작게
      * Stochastic gradent descent(SGD) : 한 반복마다 하나의 훈련 데이터만 사용하여 파라미터 업데이트 - cost function 최적화에 많이 사용됨.
  * Over-fit/Under-fit
    * 아직 잘 모르는 내용이니, 더 상세히 찾도록
    * over-fitting 해결 방법
      * features 수를 줄이기
      * regularization(정규화) - W 를 줄여라
      * early stopping : 새로 예측값을 만드려는 모델의 능력이 약해짐
  * Error analysis
  * Regularization
  * Hyper-parameters
  * Cross-validation
  * 



Unsupervised Learning

* 분류되지 않은(unclassified), 라벨링되지 않은(unlabeled)



Reinforcement Learning

* 목표 지향적 알고리즘 사용.
* 컴퓨터에게 스스로 이상적인 행동하도록 돕는 기법
* 