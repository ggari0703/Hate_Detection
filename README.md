# 욕설 탐지 및 분류 ( Hate Detection )

최근 온라인 공간을 중심으로 빠르게 사회 갈등이 확산되고 있다. 정치나 노사갈등과 같은 전통적 이슈를 넘어 성소수자, 세대 갈등 및 젠더 갈등으로까지 나아가고 있다. 그러나 기존에 필터링 방식으로는 욕설이나 혐오 발언을 해결하는 데에는 여러 한계점이 존재한다. 
사용자가 필터링 규칙을 쉽게 유추할 수 있으며, 이를 바탕으로 우회할 수 있다. 또한 관리자 입장에서 차단할 표현을 일일이 추가해야 한다는 비효율성이 존재한다. 이러한 기존 시스템의 한계점을 딥러닝 모델로 극복할 수 있다면 관리의 효율성 측면을 제고할 수 있을 뿐만 아니라 새롭게 등장하는 표현 또한 특정 카테고리의 혐오 표현으로 분류하여 혐오의 확산을 방지할 수 있을 것이다. 

각 데이터셋마다 다른 알고리즘을 사용한 모델 (1D_CNN, KRBERT, KoELECTRA)을 사용해서 성능 비교를 하는 실험을 진행하고자 한다 (총 12 번의 실험). 또한 binary classification, multi-label classification에 적합한 데이터가 따로 존재하므로 실험을 진행해서 multi-label classification을 했을 때의 성능과 binary classification을 했을 때의 성능 및 시간 차이도 비교해 보고자 한다.


## 사용한 데이터

> Korean HateSpeech Dataset (한국어 혐오 표현 데이터)

* 9,381 human-labeled comments
* labeled with gender, others, and none bias labels
* https://github.com/kocohub/korean-hate-speech

 <img src="https://drive.google.com/uc?id=1ZxOGX7Zn95f5zWrfKo12jDsrhwYvKzDi" width="800">


> Curse-detection-data (욕설 감지 데이터)

* 일간베스트(일베), 오늘의 유머와 같은 각종 커뮤니티 사이트의 댓글에 대해 총 5,825문장을 분류했습니다.
* 수직선 기호( | )를 기준으로 좌측에는 댓글 내용, 우측에는 욕설 여부(0,1)가 기록되어 있습니다.
* https://github.com/2runo/Curse-detection-data

 <img src="https://drive.google.com/uc?id=1ZyUoww57a7XcQlAlagPc_6wGHSuma9Pi" width="800">


>  K-MHaS: A Multi-label Hate Speech Detection Dataset in Korean Online News Comment

* consisting of 109,692 utterances from Korean online news comments, labelled with 8 fine-grained hate speech classes.
* ( 0 : 출신차별, 1 : 외모차별, 2 : 정치성향차별, 3 : 혐오욕설, 4 : 연령차별, 5 : 성차별, 6 : 인종차별, 7 : 종교차
별, 8 : 해당사항 없음)
* https://github.com/adlnlp/K-MHaS

 <img src="https://drive.google.com/uc?id=1Zz8H4fa-84Nn-ByfCiDVVvWde1rbeB0f" width="550">


> Korean UnSmile Dataset

* 10,139개의 혐오표현, 3,929개의 악플 및 욕설, 4,674개의 Clean data로 총 18,742개로 구성되어 있다.
* 혐오표현은 여성/가족, 남성, 성소수자, 인종/국적, 연령, 지역, 종교, 기타 등의 mulit-label로 라벨링 되어있다.
* https://github.com/smilegate-ai/korean_unsmile_dataset?tab=readme-ov-file

 <img src="https://drive.google.com/uc?id=1Zz_Qc0O3kk4QPPILu2JBk9-KoFoq_6wF" width="600">

### 데이터 전처리

SoyNLP를 통해 데이터를 간단하게 전처리했다.

## 모델 학습 및 결과(1d-CNN 모델은 기존 이진 분류 문제인 CURSE dataset을 제외한 다른 dataset 또한 이진 분류 문제로 전환하여 진행하였다.)

> 결과 (Accuracy)
<table style="width: 40%;">
  <tr>
   <td> </td> <th style="text-align: center;"> 1D_CNN </th> <th style="text-align: center;"> KoBERT </th> <th style="text-align: center;"> KoELECTRA </th>
  </tr>
 <tr> 
  <td> Curse_data </td> <td> 0.721 </td> <td> 0.873 </td> <td> 0.856 </td>
 </tr>
 <tr> 
  <td> KHS </td> <td> 0.6392 </td> <td> 0.692 </td> <td> 0.705 </td>
 </tr>
 <tr> 
  <td> KmHAS </td> <td> 0.632 </td> <td> 0.854 </td> <td> 0.853 </td>
 </tr>
 <tr> 
  <td> Unsmile </td> <td> 0.677 </td> <td> 0.749 </td> <td> 0.772 </td>
 </tr>
</table>

> 결과 (F1-score)
<table style="width: 40%;">
  <tr>
   <td> </td> <th style="text-align: center;"> 1D_CNN </th> <th style="text-align: center;"> KoBERT </th> <th style="text-align: center;"> KoELECTRA </th>
  </tr>
 <tr> 
  <td> Curse_data </td> <td> 0.570 </td> <td> 0.870 </td> <td> 0.803 </td>
 </tr>
 <tr> 
  <td> KHS </td> <td> 0.701 </td> <td> 0.689 </td> <td> 0.710 </td>
 </tr>
 <tr> 
  <td> KmHAS </td> <td> 0.713 </td> <td> 0.853 </td> <td> 0.853 </td>
 </tr>
 <tr> 
  <td> Unsmile </td> <td> 0.512 </td> <td> 0.747 </td> <td> 0.772 </td>
 </tr>
</table>

## 결론 및 분석

* 이진 분류에서의 성능은 KoBERT가 가장 뛰어난 성능을 가진다

* 욕설이 구체적으로 어떤 카테고리에 속하는지 분류하는 멀티 라벨 분류에서는 KoELECTRA가 타 모델에 비해 뛰어난 성능을 가진다

* 이진 분류를 진행한 1D-CNN 모델의 경우 이진 분류의 경우 나쁘지 않은 성능을 가진다

* 1D-CNN 모델의 학습 시간과 파라미터 개수 (367만)을 고려할 때, 실시간 및 준실시간 이진 분류에서는 타 모델에 비해 강점이 있음 (KoBERT parameter : 1.1억개 , KoELECTRA : 1400만 (small), 1.1억(Base) )

* KoBERT의 경우 성능은 KoELECTRA와 비슷하지만, 연산량 측면에서 차이가 존재한다



