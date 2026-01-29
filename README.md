# DL_tf_flowers


## 1. 개요 
입력 이미지 크기 : 180 X 180
(입력 이미지를 크게 해도 동일한 개수로 모델이 틀린 판정을 해서 점수 변화가 없음)

- EfficientNetV2B0로 전이학습 진행
- 베이스 모델로 설정 후, 분류 레이어 작성 시 다음 사항을 고려
  - `RandomFlip`, `RandomRotation` 등 학습 이미지에 약간의 변형 추가
  - Dropout으로 규제 조건 추가
  - `GlobalAveragePooling2D`로 Flatten을 대체

## 2. 훈련과정

- 컴파일 과정에서 `Adam`옵티마이저를 사용했고, `Sparse_categorical_crossentropy` 손실함수 적용
- 학습후에 TTA(Test Time Augmentation)을 통해 이미지 좌우반전을 학습하여 모델 성능을 높였다.
> TTA : 대충 예측 단계에서도 데이터를 증강해 좌우반전 이미지 각각의 예측 확률의 평균을 사용 
- 시각화를 통해 모델이 틀린 결과만을 모아본 결과, 뚜렷한 특징이 없거나 사람도 헷갈릴만한 잘못된 데이터로 인해 예측이 틀린걸 알 수 있었다.
<Figure size 1200x1200 with 9 Axes><img width="1143" height="1190" alt="image" src="https://github.com/user-attachments/assets/f4aaaaaf-5b43-45bd-bcb0-adab61ee3b14" />
- 파인 튜닝을 통해 베이스 모델의 freeze를 깨고 모델이 지나치지 않을 정도로 보조 학습 느낌으로 진행되었다.

- cut, mix뭐시기를 추천받아 시도하는 중에 colab GPU의 무료 할당량을 다써버려서 시도하지 못했다..

## 3. 결과
<img width="716" height="508" alt="스크린샷 2026-01-30 오전 12 05 45" src="https://github.com/user-attachments/assets/6732926b-29fd-4101-b967-1ec0e6ef5f7c" />



