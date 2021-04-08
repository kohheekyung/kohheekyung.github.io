---
sort: 1
---

# What is fASL?

## Background
Blood flow(혈류)는 비교적 큰 혈관계에서 혈액의 흐름.
perfusion(관류)는 혈액이 조직내 모세혈관까지 전달되어 양분을 공급하는 개념, 보다 미세한 뇌 활동 지표.

perfusion을 관측하기 위해 주로 PET이나 MRI 촬영을 하는데, 이는 조영제를 체내에 주입하며 특정 조직이나 혈관이 잘보이도록한다.
허나, 주사를 사용한다는 점과 조영제가 체외로 배출되는데 오래걸려 부작용에 예민한 환자가 있으며, 조직에 조영제가 도달하기전에 퍼져버려 오히려 perfusion rate(관류량)을 측정하기 힘들어지는 한계가 있다.

이러한 한계로 ASL(동맥 스핀 라벨링; Arterial spin labeling) fMRI를 사용할 수 있다.  
ASL는 조영제 주입 등의 침습적 행위없이 인체 내의 혈액만을 추적자(tracer)로 이용하여 perfusion을 측정한다.

## How ASL works
ASL은 일반적으로 두번의 영상을 촬영한다.
- 혈액에 특정 펄스를 가하지 않은 영상 (Control image)
- 혈액이 뇌에 도달하기 직전, 주로 목에서 펄스를 가하여 혈액을 지속적으로 자극 및 종축자화하여 라벨한 영상(Labeling image)  
이 두 영상의 신호의 차가 perfusion 정도에 비례에서 나타나서 뇌 활동의 정보를 알 수 있게 된다.
<img src=/assets/images/ASL.PNG width="40%"> <img src=/assets/images/ASL_MAP.PNG width="40%">  




ASL 촬영기법이 다양하지만 최근에는 "pseudo-conditinuous" 기법을 많이 사용하며, 이 문서에서 다루고 있다.

ASL는 일번적으로 두 번의 영상을 촬영하는데, 첫번째 영상에서는 혈액에 특정 라디오 주파수 펄수를 가하지 않고 영상을 얻고 (Control), 두 번째 영상에서는 혈액을 자화시킨후 영상을 얻는다(Labeled). 두 영상의 차로 뇌활동을 측정할 수 있다.
그 중 PCASL(pseudo-conditinuous ASL)은 (주로 목)


## ASL vs. BOLD fMRI
두 방법 모두 뇌 활동을 측정하지만, 몇몇의 차이가 있다. 

간단하게, BOLD fMRI는 뇌영역의 혈액 내 산소량의 변화신호(BOLD)가 뇌 활동(neural firing)과 관련이 있다는 점을 이용한 것이고, 
ASL fMRI는 perfusion rate를 기준으로 뇌활동을 측정한다.

- ASL가 노이즈가 더 많지만 prefusion이 BOLD 변화보다 가변성이 낮아, 자극조건이 비교적 긴 패러다임의 뇌활동을 관측하기 좋음.
- BOLD 변화는 주변 혈관과 정맥에 넓게 확인되는 반면, perfusion의 변화는 보다 더 실질 조직(parenchyma)에서 확인된다.  
- ASL 촬영 기법이 시간소요가 더 크다. 하나의 ASL image 촬영하는데 1.2-2.5s, 여러 BOLD images 촬영하는데 500ms.
  따라서 동일한 시간 내로 ASL와 BOLD 촬영시 ASL의 slice 수가 비교적 적게 얻어지고 더 두껍다. 즉, 공간 해상도가 낮음

<img src=/assets/images/ASLvsBOLD.PNG width="40%">
fingger tapping 태스크(4m) 수행 영상 분석 비교

