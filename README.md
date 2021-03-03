# separate garbage collection project

## 프로젝트 진행 계기
대중 매체에서도 많이 언급되듯이 오늘날의 분리수거는 사회의 큰 이슈거리이자 문제점이다. 
그저 "귀찮아서"라는 무책임한 이기심이 원인이 되기도 하지만 분리수거에 대한 비전문적인 지식 역시 큰 원인이 된다. 나 역시 쓰레기가 어떤 종류인지 몰라 일반쓰레기에 같이 끼워두고 간 적이 있었다. 
이러한 문제로 쓰레기장에서 겪는 곤란함과 환경 문제를 최소화하고자 쓰레기의 종류을 구별해주는 인공지능을 만들면 어떨까 라는 생각을 하게 되었다.

## 관련 자료 수집 과정
### 인터넷 조사
#### 정의와 필요성
분리수거란 폐기물의 소각 및 재활용 등 처분을 용이하게 하기 위해, 그 재질마다 폐기물을 분류하고 그것을 수집하는 것을 말한다.
다시 말해 분리수거는 쓰레기를 재활용하기 위함인데, 분리수거를 하지 않으면 여러 종류가 섞여 버려진 쓰레기를 매립하거나 소각해야 하고 이로 인해 우리는 쓰레기를 배출할 때마다 돈을 내야 하는 상황이 오게 된다.
#### 분리수거의 핵심 4원칙
 1. 비운다
 (용기 안에 담겨있는 내용물은 깨끗이 비우고 배출한다.)
 2. 헹군다
 (재활용품에 묻어있는 이물질, 음식물 등은 닦거나 한 번 헹궈서 배출한다.)
 3. 분리한다
 (라벨 등의 다른 재질 부분은 제거하여 배출한다.)
 4. 섞지 않는다
 (종류별, 재질별로 구분하여 분리수거함으로 배출한다.)
![0](https://user-images.githubusercontent.com/76692294/108505492-d5d79d00-72fa-11eb-858f-ded74eef611b.jpg)
위는 한눈에 구분할 수 있도록 새겨놓는 분리수거 표시이다.
### 설문 조사
학교 친구들과 선생님, 아는 지인 등 다양한 연령대를 대상으로 하여 설문 조사를 진행해본 결과, 오답률이 작지 않았다는 것을 알 수 있었다.
이를 통해 청소년 뿐만 아니라 한 가정을 책임지는 성인까지도 분리수거의 중요성을 인지하고는 있으나 종류의 다양성으로 불편함을 겪고 있다는 것이 입증되었다.

설문조사['https://www.kaggle.com/datasets?search=garbage']

## 일지

|Date|Title|Supplement|
|:---:|:---|:---|
|02.01|사전 자료 조사|참고 자료['https://namu.wiki/w/%EB%B6%84%EB%A6%AC%EC%88%98%EA%B1%B0']|
|02.02|설문조사||
|02.04|전처리 및 모델링||
|02.09|학습|tensorflow 모델 저장['https://sequencedata.tistory.com/14']|
|02.15|데이터 보충|정확도를 높이기 위해 추가적으로 데이터를 모았다.|
|02.17|조건 수정(1)|배치사이즈 :32->16, 이미지 크기 : 512x384|
|02.19|조건 수정(2)|Dropout : 0.1 ->0.3, Maxpooling(이미지 크기) : 32 -> 64 (overfitting 방지)|
|02.22|데이터 조정|이미지를 무작위로 수집했더니 오히려 정확도가 떨어졌다. 이의 원인을 이미지의 배경 색깔 차이로 판단하고 최대한 유사한 배경의 데이터로만 다시 모아보았다.|
|02.23|학습|재수집한 데이터들로 다시 돌려본 결과, 아래의 첫번째 자료와 같이 test 데이터의 결과가 약 40%밖에 나오지 않는 overfitting의 결과를 초래했다.|
|||
Epoch 95/100
72/72 [==============================] - 54s 743ms/step - loss: 0.5321 - accuracy: 0.7905 - val_loss: 1.0030 - val_accuracy: 0.7188
Epoch 96/100
72/72 [==============================] - 54s 747ms/step - loss: 0.5325 - accuracy: 0.8104 - val_loss: 1.0800 - val_accuracy: 0.7500
Epoch 97/100
72/72 [==============================] - 54s 746ms/step - loss: 0.5375 - accuracy: 0.8056 - val_loss: 0.9837 - val_accuracy: 0.7366
Epoch 98/100
72/72 [==============================] - 53s 737ms/step - loss: 0.5338 - accuracy: 0.8061 - val_loss: 1.0764 - val_accuracy: 0.7098
Epoch 99/100
72/72 [==============================] - 53s 733ms/step - loss: 0.5664 - accuracy: 0.7989 - val_loss: 1.0902 - val_accuracy: 0.7143
Epoch 100/100
72/72 [==============================] - 54s 742ms/step - loss: 0.5894 - accuracy: 0.7703 - val_loss: 1.1430 - val_accuracy: 0.7098|
|03.02|parameter 조정|epochs 100일 때의 값에 비해 epochs 95~99 사이의 val 정확도가 평균 3% 정도 높았다. 그래서 epochs 값을 95로 줄여 다시 돌려보았고 최종적으로 73.75%의 정확도를 나타냈다.|
## 모델 정확도
```
최종 saved 된 모델 : garbage_model95.h5

model.fit :
Epoch 95/95
144/144 [==============================] - 54s 378ms/step - loss: 0.3275 - accuracy: 0.8829 - val_loss: 1.3736 - val_accuracy: 0.7375

Evaluate Model score : [1.4881970882415771, 0.7250000238418579] (loss/accuracy)
```
![val](https://user-images.githubusercontent.com/76692294/109809452-a6fee680-7c6b-11eb-9bf4-44cfa92af936.png)
![val2](https://user-images.githubusercontent.com/76692294/109809457-a8301380-7c6b-11eb-98db-099b851f15c7.png)