---
layout: default
title:  "Django error 코드 분석 #001"
date:   2023-10-11 22:55:08 +0900
parent: ERROR_LOG
---
# Django error 코드 분석 #001
Django 프레임워크를 사용하면서 RDS와 S3에 연결되어 있는 이미지를 다운로드하기 위한 코드를 작성 중 다음과 같은 에러 상황에 놓였다.

### 서론 (boto3에 관하여)
먼저 python 과 AWS 서비스를 연결시키기 위해서는 boto3 라이브러리를 import 해야한다.
```python
import boto3
```

이후에 AWS 액세스 키를 사용하여 인증의 절차를 거쳐야하는데, 이때 client를 사용하는 경우와 resource를 사용하는 경우가 있었다. 나는 S3에 있는 이미지를 response만 해주면 되는 상황이었기 때문에 client를 사용했다. 둘의 차이점에 대한 설명은 다른 블로그를 참고하였다. 
[참고 블로그](https://planbs.tistory.com/entry/boto3resource%EC%99%80-boto3client%EC%9D%98-%EC%B0%A8%EC%9D%B4)

```Python
s3 = boto3.client(
        's3',
        aws_access_key_id=settings.AWS_ACCESS_KEY_ID,
        aws_secret_access_key=settings.AWS_SECRET_ACCESS_KEY
    )
```

### 문제 발생
여기서 key의 값은 settings.py에서 받아와서 사용해주었다. 
나는 DB에 저장되어있는 url을 통해 S3에서 해당 이미지를 찾고 그 이미지를 http에 제공하면 된다고 생각했지만,

```Python
response = s3.get_object(Bucket='bucketname', Key=image.filename)
        file_content = response['Body'].read()
response = HttpResponse(file_content, content_type='image/jpeg')
response['Content-Disposition'] = f'attachment; filename={image.filename}'
```
S3에서 가져오는 객체의 Body를 읽는 등의 당시 나에겐 확 와닿지 않는 코드였었다. 또한 어떤 이유에선지 분명 에러 없이 Http 는 응답을 받지만 다운로드가 되지 않는 상황이었다. 여기서 HttpResponse 객체를 FileResponse 로 바꿔보기도 했지만 결과는 잘 나오지 않았다. 

### 문제 해결
오히려 문제의 원인은 저장하는 함수에서가 아니라 응답 시점의 문제였는데, 

```Python
def my_photo_button(request):
    flag = request.POST.__getitem__("flag")
    print(flag)
    if flag == "download":
        my_photo_save(request)
    elif flag == "delete":
        my_photo_delete(request) 
    return redirect('myPhotoApp:myphoto')
```

Html의 Button 중 flag 값을 다르게 하여 요청한 동작이 저장인지 삭제인지 확인하는 함수에서, redirect 했던 것이 문제였다는 것을 알았다. 나는 이렇게 하면 my_photo_save 함수가 return 을 완료한 이후에 my_photo_button 의 return 을 수행할 줄 알았는데, 이 분리된 return 연산이 오히려 씹혔던 것 같다. 결국 계속 redirect return 만 잘 되고 있었던 것이었다.

```Python
# 어떤 버튼이 눌렸는지 확인한다 (삭제 혹은 저장)
def my_photo_button(request):
    flag = request.POST.__getitem__("flag")
    if flag == "download":
        page = "download"
    elif flag == "delete":
        page = "delete"
    return redirect(page)
```

추후에 버튼 값에 따라 동작을 분리해주는 함수를 위와 같이 수정 후에는 다운로드가 잘 되었고, Body를 읽는 방식 대신에 더 쉬운 방식으로 다운로드를 처리했다.

```Python
image = MyPhoto.objects.get(mpnum=mp_nums[0])
        parts = image.image.split(".com/")
        response = HttpResponse(content_type='image/jpg')
        response['Content-Disposition'] = 'attachment; filename={}'.format(image.filename)
        try:
            s3.download_fileobj('bucketname', parts[1], response)
        except Exception as e:
            print(e)
#중략
return response
```

### 소감
사실 간단한 문제였지만 머리에서는 데이터의 흐름이 분명했는데, 더 넓게 보지 못한 내 실수였다. 그래도 이 오류를 해결하는 과정에서 헷갈렸던 함수들에 대해 찾아보는 시간이 매우 유의미했다. 앞으로는 framework 를 더 자유자재로 쓸 수 있으면 좋겠다.