# 1. 🍀 GBIF 데이터 활용 해커톤 - 국립중앙과학관장상(최우수상)

<br />

***대회 준비하면서 구현한 코드 첨부 (단, 대회 제출 코드는 해커톤 특성상 가져오지 못했습니다.)***

***최종 제출한 발표 자료***

<br />
---

**1. 주제선정을 위한 브레인스토밍**

![스크린샷 2023-09-14 09-35-01](https://github.com/jmlee99/GBIF/assets/98507134/7bb54378-68c1-4a46-ae1b-d5e19a99d818)

**1-1. 로드맵 및 데이터 연계 방향**

![스크린샷 2024-01-27 14-46-50](https://github.com/jmlee99/GBIF/assets/98507134/1dde115e-3baa-41c9-9617-520ef7348f63)

![스크린샷 2024-01-27 14-47-00](https://github.com/jmlee99/GBIF/assets/98507134/f13d0bdf-b6dc-436b-85e9-7e185b12b0c3)


<br />

---

<br />

**2. 작물 데이터와 병원균 데이터 수집 및 분포**
- 작물 데이터 필터링 및 전처리 결과

![스크린샷 2023-09-14 09-35-19](https://github.com/jmlee99/GBIF/assets/98507134/d3ab634b-926b-483f-87fd-f61910aeaaa9)

- 2-1. 병원균 데이터 필터링 및 전처리 결과

![스크린샷 2023-09-14 09-35-39](https://github.com/jmlee99/GBIF/assets/98507134/dc55214c-7a7a-4c63-b528-d0ca43640e94)


**2-2. 기후 데이터 분포 검사**

![스크린샷 2023-09-14 09-35-12](https://github.com/jmlee99/GBIF/assets/98507134/a9e0ef3c-13a3-46e9-943f-86b118b8fb4f)


<br />

---

<br />

***3. 기후 예측 모델을 위한 과정***

**3-1 Summary**

![스크린샷 2023-09-14 09-35-51](https://github.com/jmlee99/GBIF/assets/98507134/ab5ab93c-0d02-408b-b408-fa23a7fdd18b)

![스크린샷 2024-01-27 14-52-06](https://github.com/jmlee99/GBIF/assets/98507134/7975714e-3630-433d-833e-6d1bb4a97006)


**3-2 기후 데이터 전처리**

![스크린샷 2024-01-27 14-52-46](https://github.com/jmlee99/GBIF/assets/98507134/9b3e6980-db07-4519-bd91-bdc1b2cb02d3)

**3-3 타임시리즈 설정**

![스크린샷 2024-01-27 15-04-08](https://github.com/jmlee99/GBIF/assets/98507134/55601834-9c27-427c-9f82-dffa8b1b2921)

***대한민국 평균을 생각했을 때는 4계절을 기준으로 Time-Series를 잡는게 맞지만,***
<br />
***최근 기후는 4계절이 뚜렷하다고 볼 수 없으므로 65일을 기준으로 Time-Series를 나누었습니다.***

**4. 시계열 모델 구현**
- 장단기 예측에 유리한 LSTM Layer를 기준으로 다른 Layer들과 함께 사용
![스크린샷 2024-01-27 15-07-35](https://github.com/jmlee99/GBIF/assets/98507134/7bbc13a4-89fa-40c1-8055-e417ef93d2ea)

- LSTM 구조 설명
![스크린샷 2024-01-27 15-07-45](https://github.com/jmlee99/GBIF/assets/98507134/d9f8b8d8-dc6e-4f9a-8f40-17b78887f5fe)

**4-1. LSTM 모델 구축**
- **Activation**, **Optimizer**, **Learning_rate**, **epochs** 등 다양한 파라미터를 수정해가며 여러가지 모델 구현
- **또한 MinMaxScaler를 사용하여 기후관련 데이터들을 음수 값이 없게 0 ~ 1사이로 조절하였습니다.
![스크린샷 2024-01-27 15-10-56](https://github.com/jmlee99/GBIF/assets/98507134/1e43892f-12f6-4945-8340-8725f39c1bf8)

**4-2. 모델 평가 지표**
- **Ensemble모델을 만들기 위해 가장 잘 나온 모델 5개를 선정하고, 그 중 loss율이 가장 높은 모델은 제외하였습니다.**

![스크린샷 2024-01-27 15-19-58](https://github.com/jmlee99/GBIF/assets/98507134/486a89f0-4a8e-45e7-a1f5-aba52e0e12cb)

**4-3 Ensemble모델 평가(시각적)**
- ***최고 기온, 평균 기온, 최저 기온*** 총 3개를 평가
- **최고기온**
![스크린샷 2024-01-27 15-21-13](https://github.com/jmlee99/GBIF/assets/98507134/5f0137ef-9d82-4081-abde-f1c2bd5186e3)
- **평균기온**
![스크린샷 2024-01-27 15-21-18](https://github.com/jmlee99/GBIF/assets/98507134/daa71935-37da-4fcc-8ced-e976461adb2b)
- **최저기온**
![스크린샷 2024-01-27 15-21-26](https://github.com/jmlee99/GBIF/assets/98507134/1f4da1ca-3a64-49c9-bc05-6e01b5214d12)

**4-2. Ensemble 모델 결과**
- **1년을 기준으로 0.3정도 올라가는 것을 알 수 있었습니다.**
- **2년을 예측하면 데이터의 한정으로 인해 일정 주기로 수렴해 버리는 결과를 보였습니다.**
- **따라서 다음 1년을 기준으로 10년 후에는 대략 3도가 상승한다고 생각하고 예측을 진행했습니다.**
![스크린샷 2024-01-27 15-24-13](https://github.com/jmlee99/GBIF/assets/98507134/6c677e19-c602-44f3-8a46-ed9c68b1fea5)

**5. 기후유사지역 작물 데이터 결과**
- 10년 후의 대한민국 기후를 현재 가진 나라를 선별한 결과
![스크린샷 2024-01-27 15-28-40](https://github.com/jmlee99/GBIF/assets/98507134/9b71b3cd-2b12-41b5-95b0-8aa74abd475f)

- 이러한 나라의 공통적 작물
![스크린샷 2024-01-27 15-30-25](https://github.com/jmlee99/GBIF/assets/98507134/629d3f27-fda1-46a6-87ed-7bf95cad8b0b)

**5-1. 위의 작물들에 취약한 병원균**
- 이러한 작물들을 위한 사전 예방이 중요(농약 등)
![스크린샷 2024-01-27 16-08-56](https://github.com/jmlee99/GBIF/assets/98507134/dccfd2c8-97d0-4029-adf7-76c5c4439cdb)
![스크린샷 2024-01-27 16-08-59](https://github.com/jmlee99/GBIF/assets/98507134/766571b1-4db9-4efe-a977-904ee8c131ec)

**5-2. 개선방향**
![스크린샷 2024-01-27 16-10-30](https://github.com/jmlee99/GBIF/assets/98507134/cc411eb6-9c50-487a-a692-e5b37c6d288a)

**5-3. 기대효과**
![스크린샷 2024-01-27 16-11-26](https://github.com/jmlee99/GBIF/assets/98507134/6711c273-cf32-4d57-b4f4-04c486cb7c86)
