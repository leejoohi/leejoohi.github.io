---
layout: post
title:  "BentoML의 이해"
date:   2023-10-14 14:32:00 +0900
---

### 서론
두번째 팀프로젝트로 만들었던 색채 식별 인공지능 모델을 서비스로 사용하여 웹페이지를 만들고자 하였다. 이 모델을 서버로 어떻게 배포하느냐에서 BentoML을 사용하게 되었다.
BentoML은 머신러닝 모델을 서비스로 관리, 배포하기 쉽게 일본 도시락 Bento에 포장하여 serving 하는 프레임워크이다.
우리는 Google Colab 환경에서 YOLOv8m 모델을 베이스로 커스텀 데이터셋을 학습시킨 상태였다. 


### 본론

```
!pip install bentoml
```

```
import bentoml
from ultralytics import YOLO

model = YOLO("yolov8m.pt").model
model.eval()
saved_model = bentoml.pytorch.save_model(name='chromalens_488',
                                         model = model)
```

위의 코드는 YOLO 모델을 bentoml 함수로 저장하는 내용이다. 

```python
import bentoml
from bentoml.io import Image, JSON

yolov8m_runner = bentoml.pytorch.get("chromalens_488:latest").to_runner()
svc = bentoml.Service("yolov8m_svc", runners=[yolov8m_runner])

# input_img를 인코딩하다
def encode_image(input_img):
  ratio = 3 # 0~9
  encode_param = [cv2.IMWRITE_JPG_COMPRESSION, ratio]
  encoded_img = base62.b64encode(cv2.imencode(".jpg", input_img, encode_param)[1])

  return encoded_img.decode("utf8")

# api
@svc.api(input=Image(),
         output=JSON(),
         route="/content/drive/MyDrive/msa")
def predict(f: Image):
  img_origin, img_tensor = pre_processing(f=f)
  out = yolov8m_runner.run(img_tensor)
  out_bbox_info, out_img = post_processing(img_origin = img_origin,
                                           img_tensor=img_tensor,
                                           out=out)
```

이후에는 bentoml.Service 함수를 사용하여 이 모델이 API로써 인코딩된 이미지를 받아들여 predict할 수 있도록 배포하는 과정의 코드이다.

### 결론

이번 프로젝트 코드에서는 서비스 제공에 한해서 사용되었지만, YATAI로 push, pull을 하며 관리, bentofile.yaml 파일을 통해 빌드, custom runner도 정의할 수 있다. bento에 대한 개괄적인 사용방법은 <a href="https://docs.bentoml.org/en/latest/index.html">BentoML</a> 페이지로 확인할 수 있다.

### 소감
프로그래밍 안에서 class, package, api, wrapping 하는 등의 개념과 서비스는 정말 무수하게 많은 것 같다. 이런 것들을 잘 알고 적절하게 사용하는 것이 서비스를 명료하게 지속가능하게 만드는 데에 영향을 많이 준다는 생각이 들었다.