# HOSEO-AI-Competition-2022
2022 호서대학교 1차 AI 프로그래밍 경진대회에 참가한 내용입니다.  
학습 데이터와 테스트 데이터가 주어지며 해당 데이터셋에 대한 문장을 추론하는 task에 대한 대회였습니다.(Kornli)  
해당 대회에서 저희 팀 ACC120은 리더 보드 accuracy score 86.67을 달성했습니다.
<img src="https://user-images.githubusercontent.com/80209977/178156365-d6bf383d-776d-49b9-a338-7d0ccf241b25.png" width="600" height="400">
## Information
- 최종 소스코드 파일 : /notebooks/Stacking ELECTRA+BERT+ALBERT.ipynb  
- KLUE-nli Dataset에 대한 검증 소스코드 : /notebboks/KLUE_NLI.ipynb  
- 리더보드 최종 제출파일 : /submission/submission_stacking_v4.csv  
- 리더보드 정확도 99.47% 달성 제출 파일 : /submission/submission_klue_nli_v1.csv  
## References
- Dataset  
  - kakaobrain/KorNLUDatasets : [GitHub Link](https://github.com/kakaobrain/KorNLUDatasets)  
- Pre-trained models  
  - monologg/KoELECTRA : [GitHub Link](https://github.com/monologg/KoELECTRA)    
  - kiyoungkim1/LMkor : [GitHub Link](https://github.com/kiyoungkim1/LMkor)  
## 대회 주제  
- Dataset에 대한 전처리 과정 및 학습 모델 탐지율 평가
- 데이터 한 행에서 2개의 문장의 일치여부에 따라, 아래 3개 라벨로 분류
  - 참(Entailment)
  - 거짓(Contradiction)
  - 중립(Neutral)
- 테스트 데이터에 대해 예측 결과를 제출(.csv파일)
## Team ACC120 대회 문제 해결 방법
### 1. 데이터
대회에서 제공한 데이터(19,663개) + KorNLUDataset(kornli:5,000개 snli:5,000개) = 총 2,9663개의 데이터 사용  
<img src="https://user-images.githubusercontent.com/80209977/178155656-d12014f1-1fcb-439c-8f8c-cc9073510e51.png" width="700" height="300">  
### 2. 데이터 전처리  
Stacking Ensemble을 적용하기 위해 7:3 비율로 데이터를 Split  
<img src="https://user-images.githubusercontent.com/80209977/178155832-d872f3c4-aca4-400b-883f-bf9b3e85b0dc.png" width="400" height="350">  
데이터 클래스의 불균형, 결측 값 등의 처리 진행  
이후, 개별 모델에 활용할 pre-trained tokenizer을 활용해 적절한 패딩 값을 계산  
### 3. 모델링  
Pre-trained ELECTRA, BERT, ALBERT 모델 3가지를 통해 fine-tuning을 진행하고 validation 데이터의 예측을 통해  
훈련 데이터에 대한 메타 데이터와 최종 예측 데이터에 대한 메타 데이터를 생성  
이 메타 데이터는 각 행에 대해서 예측한 클래스별 확률 값과 최종 판별 클래스로 구성했음  
메타 데이터에 대해 XGBoost 모델을 통해 최종 분류 모델을 생성하고, 최종 예측을 진행함
<br>
<img src="https://user-images.githubusercontent.com/80209977/178156224-8057a02f-d1a1-406a-9376-4bca7824fa5e.png" width="700" height="450">
## 대회 진행 중의 특이사항  
<img src="https://user-images.githubusercontent.com/80209977/178156403-5d2fa570-dfc5-46be-9f5b-98b6a32b7db0.png" width="700" height="80">  
<p>대회 진행 도중 특정 데이터셋을 통해 학습을 진행하면 정확도가 99%가 산출되었습니다.</p>
<p>이에 팀에서는 아무리 최신 모델을 가져다 써도 나올 수 없는 수치라고 판단하여 해당 KLUE-nli 데이터셋을 통해 실험을 진행했습니다.<br>
KLUE-nli 데이터셋을 train 데이터, 대회에서 제공한 데이터셋을 검증 데이터로 사용하여 예측을 진행한 결과 99%의 정확도를 보였습니다.<br>
따라서, 대회에서 제공한 데이터가 KLUE-nli 데이터셋임을 알 수 있었고 실제로 내용도 일치하였을 확인할 수 있었습니다.<br>
저희 팀은 이미 정답을 알고 있는 데이터를 학습해 모델을 구현하고 예측을 진행한다면 대회의 의도와 벗어나며 부정행위라고 판단했습니다.<br>
그래서 해당 데이터셋을 제외한 모델을 구현하여 결과를 제출했습니다.<br></p>
<img src="https://user-images.githubusercontent.com/80209977/178156687-f10fdd35-338a-4894-9d03-3c2ad2e002b0.png" width="" height="">
<p>대회 기간 동안, 리더 보드를 통해 수시로 다른 팀들의 점수를 확인할 수 있었습니다.<br>
저희 팀은 대회의 의도와 맞지 않으며, 부정 행위라 추측되는 점수들이 많이 보여 안타까웠습니다.<br>
대회 주최 측에 이러한 내용을 전달했지만, 제대로 된 답변을 받을 수 없었으며 결과가 나온 후 판단할 수 있을것 같습니다.</p>
