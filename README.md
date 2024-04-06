# 사용자의 MBTI 예측 모델 & 유형별 대화

### Process Record
- https://www.notion.so/YBIGTA-MBTI-b4bccc4df4704ca59c29bcfad937d477

<br>

# Task 1

## Goal
- 랜덤하게 선별한 질문을 출력하면, 사용자가 답변을 입력 (multi-turn)
- 답변들을 바탕으로 모델에 입력하고, 최종적으로 사용자의 MBTI 예측

## Dataset
- https://www.kaggle.com/datasets/zeyadkhalid/mbti-personality-types-500-dataset/data
  > Reddit과 PersonalityCafe Forum에서 수집한 게시물을 전처리한, 유저의 MBTI와 게시글 내용을 담은 데이터

![image](https://github.com/chaehyun1/study/assets/108905986/c04df444-da62-416f-91ab-8703d32c819a)
![image](https://github.com/chaehyun1/study/assets/108905986/2786bf3c-9be2-4c38-941e-efa15e01728e)

* 각 MBTI별로 데이터의 개수에 불균형함이 존재

## 데이터 전처리
<br>

- URL, non-alphanumeric characters 제거

- Data 전체에서 70회 미만으로 사용된 word, nltk library가 정의한 stop word 제거
및 lemmatizing, lower word로 변환

- Torch로 훈련할 수 있도록 LabelEncoder를 이용해 label column추가. 이를 train,
val, test에 label로 사용

- mbti마다 data수가 다르므로 data가 가장 많은 MBTI 기준으로 Upsampling

## Modeling
![image](https://github.com/chaehyun1/study/assets/108905986/72e4bd77-145a-4d3c-8b05-8755ed71afae)
- bert 기반의 pretrained model을 fine-tuning하여 사용  

## Frontend & Backend

#### 전체 코드 구성
```
task1_완성  
├── code
│   ├── ???.json            - firebase에서 할당받은 개인 json 파일 필요 (참고: https://tre2man.tistory.com/196)
│   ├── utility.py          - real_main.py 실행에 필요, 실행할 필요는 없음 
│   ├── save_bert.py        - **첫번째 실행**, **한번만 실행하면 됨**, 시간 단축용, 필요한 pretrained 모델 미리 저장하기 
│   └── real_main.py        - **두번째 실행**, firebase real-time db에 저장된 답변을 바탕으로 예측된 mbti 결과 db에 다시 저장
│  
└── model                          
    └── best_model.pth      - 학습된 weight를 모델에 적용, 용량이 커서 안올립니다.
```
- cd code로 code 파일 안에서 실행해야 함!

![image](https://github.com/chaehyun1/study/assets/108905986/fa958668-8ec6-4b2f-8857-368f3343197a)
![image](https://github.com/chaehyun1/study/assets/108905986/55064238-4e41-4860-96a9-da17ffe62a54)


<br>

# Task2

## Goal
- 내가 대화하고 싶은 상대방의 MBTI를 입력하면, 이에 맞는 답변을 출력하는 챗봇을 만들어보자.

## Dataset
- OpenAI API를 사용한 특정 MBTI Q&A 데이터셋 생성

```
model = "gpt-3.5-turbo"
query =  "You are a close friend who's MBTI is ENFP.You produce two sets of questions and answers, Both questions and answers must be in Korean, reflecting deep insight and thoughtful analysis inherent to ENFP personalities."
input =  "일상생활, 학교생활, 성격, 가치관, 취미, 시사, 스포츠, 상식, 인간관계 등과 관련된 여러 가지 주제로, 내가 질문하고 MBTI가 ENFP인 친구가 대답하는 대화의 예시를 생성해주세요. 반드시 한국어로 생성해야 합니다."
```
![image](https://github.com/chaehyun1/study/assets/108905986/ab60ebbd-ba4f-4273-838d-ce9915024513)

* 데이터셋이 잘 생성되었는지, 예시로 INFJ의 답변 데이터셋을 버블 클라우드와 워드 클라우드로 생성
* 계획, 생각, 문제, 해결 등 INFJ를 설명하는 단어가 가장 많이 사용된 것을 볼 수 있음

## Modeling
![image](https://github.com/chaehyun1/study/assets/108905986/440d02d2-0355-4cb7-bd09-744373ea8c8f)
- Davidkim205의 komt-llama2-7b-v1모델
- https://huggingface.co/ToBeWithYou/MBTI_ENFP
- https://huggingface.co/ToBeWithYou/MBTI_ENTP
- https://huggingface.co/ToBeWithYou/MBTI_ISFJ
- https://huggingface.co/ToBeWithYou/MBTI_ESFP
- https://huggingface.co/ToBeWithYou/MBTI_INTJ

<br>

![image](https://github.com/chaehyun1/ybigta_coding_camp/assets/108905986/408112d7-e440-4823-a7f3-a157e8a67054)

## Frontend & Backend
![image](https://github.com/chaehyun1/study/assets/108905986/6d95630e-7d5a-4d8a-8737-76cf3f359545)
