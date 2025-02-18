# FIGMA : Fine-tuning a Fashion Domain Image Generation Model Using Self-Instruct

### How to collaborate git

- 각 작업자가 매 작업마다 working 전용 브렌치를 만들고, 이 브렌치를 매번 main에 merge하는 형태로 협업이 진행.
- 즉 main은 가장 최신의 프로젝트 코드로 두고, 각 작업자들은 개별 브렌치에서 작업 후 main에 merge하는 방식.
- 작업자의 브렌치명은 `영문 이름`. eg. gayeon

- 아래 git 커맨드를 참고.

```bash
# 작업용 git 커맨드. 중괄호로 된 부분에 내용을 채워넣은 뒤 사용

# 1) 작업 전 main 브렌치 pull & 기존 작업용 브렌치 삭제
# 반드시 작업/수정하기 전에 이 스크립트를 실행. 최신 버전의 코드를 내려받아 이 위에서 작업 필요.
git checkout main  # main 브렌치로 이동 (현재 위치한 브렌치에서 수정사항이 있을 경우 커밋 또는 되돌린 다음 이동해야 합니다)
git pull
git branch -d working_{이니셜}  # 기존브렌치 삭제


# 2) 작업 전 새 브렌치 생성
git checkout -b working_{이니셜}  # 브렌치 새로 생성
git status  # status 확인
git branch  # 현재 브렌치 확인


# 3) 작업 후 add & commit & push (vscode의 git 탭을 활용하는 것도 가능)
git add .  # 전체 한번에 add
git commit -m "{커밋 메세지, 작업내용을 작성}"
git push origin working_{이니셜}

# 4) github로 이동하여 working_{이니셜} 브렌치를 main에 pull request 생성
# merge 된 다음에 1) 로 돌아가 작업 시작
```

- 기본적으로는 각자 맡은 파트에 대해서만 작업 후 merge하는 것을 권장. (git merge시 충돌을 막기 위함)
- 또한 작업 완료됐으면 가능한 빠르게 commit - push - pull.

### Information of Directory
[Data]
Data/KFashion/Samp_KFashion_Image(230개jpg) : KFashion 데이터 카테고리 별로 샘플링 후 옷 크롭 이미지
Data/KFashion/Samp_KFashion_Label(230개json) : KFashion 데이터 카테고리 별로 샘플링 후 옷 크롭 이미지에 대한 라벨

Data/Seed/Seed_Image(230개jpg) : 샘플링한 KFashion 데이터이미지(Samp_KFashion_Image) 
Data/Seed/EN_Seed_Label(230개json) : 영문 캡션 라벨( Prompt, Input, Add_Info, Output)
Data/Seed/Integrated_Seed_data(2,230개) : KFasion Image + EN_Seed_data + Total Image data 

Data/Augment/Augment_KFashion_Image(1,770개jpg) : KFashion 데이터 카테고리 별로 샘플링 후 옷 크롭 이미지(Seed에서 사용 안한 이미지) 
Data/Augment/Augment_CLIP(1,770개json) : 샘플링한 KFashion 데이터 이미지(Augment_KFashion_Image)에 대한 CLIP 증강 캡션 라벨
Data/Augment/Augment_GPT(1,770개json) : 샘플링한 KFashion 데이터 이미지(Augment_KFashion_Image)에 대한 GPT 증강 캡션 라벨

Data/Generate/Generate_Model/Model_Name(jpg): 이미지 생성 모델로 prompt에 맞춰 생성한 이미지
Data/Generate/Answer/Model_Name(jpg+json): 정답 이미지 + 라벨

Data/Total/Total_Image(2,000개jpg) : Seed Image + Augment Image
Data/Total/Total_CLIP(2,000개json) : Seed + CLIP Augment 
Data/Total/Total_GPT(2,000개json) : Seed + GPT Augment

[Prompt]
Prompt/prompt_218.txt : 218.jpg에 대한 이미지 생성 prompt
Prompt/prompt_374.txt : 374.jpg에 대한 이미지 생성 prompt
Prompt/prompt_465.txt : 465.jpg에 대한 이미지 생성 prompt

[Code]
Code/Processing_Data.ipynb : K-Fashion Data 영문으로 변환, K-Fashion Data를 Seed Data로 변환, jpg 데이터와 json 데이터 결합
Code/Augment_Data.ipynb : CLIP, GPT로 이미지 데이터에 대해서 라벨 캡션 증강
Code/Fine-Tuning.ipynb : 생성한 Augment 데이터로 Fine-Tuning
Code/Evaluation.ipynb : CLIP Score와 KID 기반 Metric 평가
Code/Generate.ipynb : 이미지 생성 모델로 캡션에 대해서 생성 
