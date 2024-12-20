# Music-Composition-AI

### LSTM 기반 음악 시퀀스 예측 모델

이 문서는 텍스트 데이터를 기반으로 음악 시퀀스를 예측하는 LSTM 모델의 구현 및 사용 방법을 설명합니다. 이 프로젝트는 텍스트로 표현된 음악 데이터를 숫자로 변환하고, 이를 학습하여 새로운 음악 시퀀스를 생성하는 것이 목적입니다.

---

### **1. 개요**

이 프로젝트의 목표는 텍스트 데이터를 LSTM 기반 신경망으로 학습하여 주어진 음악 시퀀스에서 다음 음을 예측하는 것입니다. 주요 과정은 다음과 같습니다:

1. **데이터 전처리**: 음악 데이터를 고유 숫자(ID)로 변환합니다.
2. **모델 학습**: LSTM 모델을 정의하고 음악 데이터를 학습시킵니다.
3. **예측**: 학습된 모델을 사용해 새로운 음악 시퀀스를 생성합니다.
4. **출력 복원**: 예측된 숫자를 다시 텍스트로 복원하여 음악 시퀀스를 확인합니다.

---

### **2. 프로젝트 목표**

- **음악 시퀀스 학습**: 음악 데이터를 숫자로 변환하여 LSTM 모델로 학습합니다.
- **음악 생성**: 학습된 모델을 이용해 새로운 음악 시퀀스를 생성합니다.
- **확률적 생성**: 모델의 softmax 출력을 활용하여 다양한 음악 시퀀스를 생성합니다.

---

### **3. 데이터 설명**

음악 데이터는 텍스트로 표현된 음표, 코드, 또는 기타 음악적 요소로 구성됩니다.

예를 들어, 음악 데이터는 다음과 같은 형식일 수 있습니다:

```
plaintext
코드 복사
"C D E F G A B C D E"

```

---

### **4. 주요 코드 흐름**

### **1단계: 데이터 전처리**

1. **음악 데이터를 숫자로 변환**:
    - 각 고유 음표(또는 음악 요소)에 숫자 ID를 부여합니다(`text_to_num`, `num_to_text`).
    - 입력 텍스트 데이터를 숫자로 변환하여 `숫자화text` 리스트를 생성합니다.
2. **시퀀스 생성**:
    - 길이가 25인 입력 시퀀스(`X`)와 해당하는 다음 음표(`Y`)를 생성합니다.
    - 이를 통해 모델이 음악 패턴을 학습할 수 있도록 데이터를 준비합니다.

### **2단계: 원-핫 인코딩**

- 모델 학습을 위해 `X`와 `Y` 데이터를 원-핫 인코딩합니다.

### **3단계: 모델 정의 및 학습**

- **모델 구조**:
    - LSTM 레이어(100개 노드)와 Softmax 활성화 함수를 가진 Dense 레이어로 구성됩니다.
- **모델 학습**:
    - 손실 함수로 `categorical_crossentropy`를 사용하며, 옵티마이저는 `adam`을 사용합니다.
    - 배치 크기는 64, 에포크는 3으로 설정합니다.

### **4단계: 음악 시퀀스 예측**

1. **단일 예측**:
    - 주어진 시퀀스(`첫입력값`)를 기반으로 다음 음을 예측합니다.
2. **연속 예측**:
    - 초기 입력 시퀀스를 기반으로 200개의 음을 예측하고, 이를 연결하여 음악 시퀀스를 생성합니다.
3. **확률적 예측**:
    - softmax 출력을 활용해 다음 음을 확률적으로 선택하여 다양한 결과를 생성합니다.

---

### **5. 주요 변수 설명**

| **변수명** | **설명** |
| --- | --- |
| `text_to_num` | 고유 음악 요소를 숫자 ID로 매핑한 딕셔너리. |
| `num_to_text` | 숫자 ID를 음악 요소로 매핑한 딕셔너리. |
| `숫자화text` | 입력 음악 데이터의 숫자 표현. |
| `X` | 입력 시퀀스(원-핫 인코딩된 데이터). |
| `Y` | 타겟 시퀀스(원-핫 인코딩된 데이터). |
| `model` | 음악 시퀀스 예측을 위한 학습된 LSTM 모델. |
| `Pmodel` | 저장된 모델을 불러온 예측 전용 LSTM 모델. |
| `첫입력값` | 예측에 사용되는 초기 입력 시퀀스. |
| `music` | 예측된 숫자 토큰 리스트(음표 ID). |
| `music_text` | 최종 예측된 음악 시퀀스(숫자 토큰을 디코딩한 결과). |

---

### **6. 실행 방법**

1. **데이터 준비**:
    - 고유 음표 리스트(`유니크text`)와 입력 음악 데이터(`text`)를 준비합니다.
    - 예: `유니크text = ['C', 'D', 'E', 'F', 'G', 'A', 'B']`
2. **데이터 전처리**:
    - 제공된 코드를 실행하여 데이터를 `숫자화text`, `X`, `Y` 형식으로 변환합니다.
3. **모델 학습**:
    - 학습 코드를 실행하여 모델을 학습시킵니다.
    - 학습된 모델은 `model.save()`를 통해 저장할 수 있습니다.
4. **예측 수행**:
    - 초기 입력 시퀀스를 설정하고, 단일 또는 연속 예측 코드를 실행하여 음악 시퀀스를 생성합니다.
5. **결과 확인**:
    - 예측된 결과(`music_text`)를 출력하여 새로운 음악 시퀀스를 확인합니다.

---

### **7. 예측 예시**

### **모델 예측 결과**:

```python
python
코드 복사
print(music_text)
# 예시 출력: "C D E F G A B C D E F G A ..."

```

### **결정적 예측 결과**:

- 매번 동일한 결과를 생성합니다.

### **확률적 예측 결과**:

- 다양한 음악 시퀀스를 생성할 수 있습니다.

---

### **8. 주의사항**

- 음악 데이터를 충분히 학습시키기 위해 적절한 데이터셋이 필요합니다.
- 모델의 입력 시퀀스 길이(25)와 어휘 크기(31)는 데이터셋에 맞게 조정해야 합니다.
- 확률적 예측의 경우, softmax 출력의 분포에 따라 결과가 달라질 수 있습니다.

---

### **9. 참고**

이 프로젝트는 간단한 음악 패턴 예측 및 생성 작업을 위한 워크플로를 제공합니다. 이를 기반으로 다양한 음악 생성 프로젝트로 확장할 수 있습니다.
