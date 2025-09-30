# Adventure of Jeonrae
전래동화 기반 인터랙티브 콘텐츠

## 프로젝트 소개
전래동화를 바탕으로 한 AI 기반 체험형 스토리·게임 서비스

## 개발기간
### 2025.08.22 ~ 2025.09.26

## 팀원
| [정휘찬](https://github.com/JHC1231) | [배진아](https://github.com/queenjerry614) | [이은아](https://github.com/eunalee) | [이현지](https://github.com/Merongmerongmerona) | [전재용](https://github.com/wodyd0103-byte) | [조찬형](https://github.com/01cho1) |
|:----------------------------------------------:|:------------------------------------:|:---------------------------------------:|:------------------------------------:|:---------------------------------------:|:---------------------------------------:|
| 프로젝트 기획<br>LLM 모델 제작 | UI/UX 디자인<br>미디어 컨텐츠 제작 | 프로젝트 구조 설계<br>전체 웹 서비스 개발 | 프로젝트 매니저<br>이미지 생성 모델 제작 | LLM 모델 제작<br>TTS 기능 구현 | 프로젝트 구조 설계<br>사용자 웹 서비스 개발 |

## 사용 기술 스택
[![My Skills](https://skillicons.dev/icons?i=py,typescript,django,react,docker,nginx,postgresql,redis,azure,git)](https://skillicons.dev)

## 공통
### 데이터 수집
[공유마당](https://gongu.copyright.or.kr/gongu/search/search/list.do?viewType=&collection=gongu_wrt&kwd=%EB%8F%99%ED%99%94&reSrchCheck=&reKwd=&pageIndex=1&wrtTy=10001&licenseCd=97&koreanClList=&srchAll=1&sotrSe=popular&srchName=&dataSe=&koglClSe=&dataSite=&authrNmP=&tagNmList=&d2WrtFileTyChk=&d2WrtTy=&d2LicenseCdTy=&d2LicenseChk=97#moreBtn) 데이터 사용 (자유 이용 가능한 저작권 만료 저작물)

## 기능별 프로세스
### 소셜 로그인
1. Microsoft / Google / Kakao 로그인
   * Microsoft 의 경우, "대한상공회의소 인력개발사업단" 테넌트에 속한 사용자 계정만 로그인 가능 (xxx.onmicrosoft.com 도메인 소유 사용자)

2. 첫 로그인 시, 반드시 닉네임 설정

### 스토리 모드
1. 관리자 스토리 데이터 등록

2. Azure OpenAI 모델의 이야기 창작 - 작업 규칙에 따라 4~5개의 핵심 장면 생성
   | 번호 | 규칙 | 내용 |
   |:----------------------------------------------:|:------------------------------------:|:---------------------------------------:|
   | 1 | 장면 나누기 | 이야기의 전통적인 구조(기승전결)를 참고하여 4~5개의 핵심 장면으로 나누고, 각 장면에 고유한 영어 ID를 붙여주세요. |
   | 2 | 분기 생성 | 플레이어의 선택이 의미 있도록 원작에 없더라도 선택의 결과로 이어질 '새로운 장면'이나 '짧은 엔딩'(좋은/나쁜/재미있는 엔딩 등)을 1~2개 이상 창의적으로 만들어내야 합니다. |
   | 3 | 장면 묘사 원칙 | 선택지가 있는 장면(엔딩이 아닌 장면)의 'description'은 반드시 플레이어가 선택을 내리기 직전의 긴장감 넘치는 상황까지만 묘사해야 합니다. 선택의 결과를 미리 암시하거나 결론을 내리면 절대 안 됩니다. |
   | 4 | 논리적 일관성 검증 | 선택지는 '원인(Cause)', 이어지는 장면의 내용은 '결과(Effect)'입니다. 이 둘은 반드시 명확하고 설득력 있는 인과관계로 이어져야 합니다. |
   | 5 | 완벽한 기술적 연결 | 각 'choices' 배열 안의 모든 선택지는 반드시 'next_moment_id' 키를 통해 이 JSON 파일 내에 실제로 '정의된' 다른 장면 ID로 연결되어야 합니다. |
   | 6 | 엔딩 처리 | 이야기의 끝을 맺는 장면(엔딩)에는 'choices' 키 자체를 포함하지 마세요. 엔딩의 'description'은 최종적인 결과와 이야기가 주는 교훈을 요약해야 합니다. |
   | 7 | JSON 형식 준수 | 최종 결과는 반드시 아래 [출력 JSON 형식]과 똑같은 구조의 JSON 데이터로만 출력해야 합니다. |

3. Dall-e 모델의 핵심 장면별 이미지 생성
   * 8비트 픽셀아트 형식, 레트로 비디오 게임 스타일
   * 과격한 언어, 폭력적, 유혈, 고어 이미지 생성 제재
   * 일본, 중국 요소 제외

4. 이야기 진행에 따라 Azure OpenAI 모델이 장면 설명 및 선택지 실시간 생성
   * 장면
     * 다정한 말투 사용, 아이의 눈높이에 맞춰 설명
     * 의성어, 의태어 사용
   * 선택지
     * 장면에 따른 선택지 N개 생성
     * 주인공이 하려는 행동을 나타내는 짧은 문장 형식
     * 마지막 장면에서는 전래동화 속에서 얻은 교훈이나 감동을 느낄 수 있도록 이야기 마무리

5. 플레이 기록
   * 스토리별 엔딩 갯수를 집계하여 달성한 엔딩에 따라 진행률 반영

### 싱글/멀티 모드 (TRPG)
1. 관리자 시나리오 데이터 등록

2. Azure OpenAI 모델의 캐릭터 생성
   | 항목 | 내용 |
   |:----------------------------------------------:|:------------------------------------:|
   | 이름 | 캐릭터 이름 |
   | 역할 | 캐릭터 역할 (탱커/정찰자/현자/외교가/트릭스터 등) |
   | 설명 | 캐릭터 성격 및 플레이 스타일 |
   | 능력치 | 캐릭터 육체적, 정신적 성능 수치화 (힘/지능/민첩/정신력 등) |
   | 스킬 | 캐릭터 보유 능력 |

3. Dall-e 모델의 캐릭터 이미지 생성 (Azure OpenAI 모델의 캐릭터 요약 바탕)
   * 캐릭터의 시각적인 요소(동작, 표정 등)에 초점
   * 배경은 단순하고 어둡게 설정

4. 게임 진행에 따라 Azure OpenAI 모델이 게임 마스터 역할 및 이미지 생성
   * 게임 마스터
     * 시나리오 정보를 바탕으로 메인 목표와 실패 조건 정의
     * 게임 규칙(장르, 난이도, 진행 횟수 등)에 따라 메인 목표 혹은 실패 조건에 가까워지는 선택지 제시
   * (멀티 모드에서만) 이미지 생성
     * 8비트 픽셀아트 형식, 레트로 비디오 게임 스타일
     * 일본, 중국 요소 제외
     * 이미지 내 텍스트 노출 없음

5. 추가 기능
   * 현재 상황 정보  
     * 다음 장면 생성을 위해 최근 변화에 맞춰 현재 턴의 상황을 요약하여 전달

   * 실시간 채팅
     * Django Channel Layer(Redis) 를 사용하여 채팅 기능 구현

### 업적
1. 기능별 업적
   | 모드 | 업적 |
   |:----------------------------------------------:|:------------------------------------:|
   | 스토리 모드 | 게임 처음 플레이 |
   | 스토리 모드 | 모든 스토리 엔딩 클리어 |
   | 스토리 모드 | 특정 스토리의 숨겨진 엔딩 발견 |
   | 스토리 모드 | 하나의 이야기 완료 |
   | 싱글 모드 | 게임 10회 플레이 |
   | 싱글 모드 | 모든 난이도 마스터 |
   | 싱글 모드 | 상급 난이도 클리어 |
   | 멀티 모드 | 게임 처음 플레이 |
   | 멀티 모드 | 팀원들과 게임 5회 진행 |
   | 멀티 모드 | 게임방 3회 생성 |
   | 멀티 모드 | 6인 이상 게임 참여 완료 |

### TTS
1. Expo Speech 적용
   * 싱글 모드에서만 선택 판정 결과에 대한 음성 인식 서비스 제공

## 웹 서비스
* Django + React Native Expo 기반 웹 서비스
<img width="2560" height="1316" alt="메인 화면" src="https://github.com/user-attachments/assets/d63ad4f0-df68-4446-a196-71b15edab644" />

### 소셜 로그인
<img width="2560" height="1318" alt="로그인" src="https://github.com/user-attachments/assets/516d0b69-dcc5-4f61-a688-6094aeefb7d1" />
<img width="2560" height="1316" alt="닉네임 설정" src="https://github.com/user-attachments/assets/fee765ec-71f4-4af6-b298-75ad253ec7c3" />

### 스토리 모드
* 관리자
<img width="1726" height="1315" alt="스토리 모드 - 스토리 관리" src="https://github.com/user-attachments/assets/6ed2c233-638b-4a3b-be95-30d27851e428" />
<img width="1730" height="1316" alt="스토리 모드 - 이미지 관리" src="https://github.com/user-attachments/assets/dd9f07dc-da47-4144-bffc-ceb6afb076fb" />

* 사용자
<img width="2560" height="1317" alt="스토리 모드 - 이어하기" src="https://github.com/user-attachments/assets/f83bc560-9baa-4a16-890b-a89a1b5746ea" />
<img width="1732" height="1317" alt="스토리 모드 - 진행" src="https://github.com/user-attachments/assets/be9a9406-f6f2-42c7-88e7-5ab80275cc52" />

### 싱글/멀티 모드 (TRPG)
* 관리자
<img width="1730" height="1317" alt="싱글,멀티 모드 - 난이도 관리" src="https://github.com/user-attachments/assets/e376f886-c523-4e5f-8d73-9d266dd39ce1" />
<img width="1730" height="1315" alt="싱글,멀티 모드 - 시나리오 관리" src="https://github.com/user-attachments/assets/bcbfa7ef-ea6d-408e-bdd8-ad33fab22cd2" />
<img width="1731" height="1317" alt="싱글,멀티 모드 - 캐릭터 관리" src="https://github.com/user-attachments/assets/f4ddce12-706d-497b-b12f-b7c0571cef75" />

* 사용자 - 싱글 모드
<img width="2560" height="1316" alt="싱글 모드" src="https://github.com/user-attachments/assets/a47f5ac9-b06a-4971-8c2c-723521987c19" />
<img width="2559" height="1318" alt="싱글 모드 - 이어하기" src="https://github.com/user-attachments/assets/20f6b22a-faa6-4928-928e-33356fa9b888" />
<img width="2560" height="1317" alt="싱글 모드 - 결과" src="https://github.com/user-attachments/assets/adb702d5-982a-4bc9-ad36-2fca850f166c" />
<img width="2560" height="1318" alt="싱글 모드 - 라운드 결과 요약" src="https://github.com/user-attachments/assets/4813a1ed-f5da-4b38-a291-236ee8b4476b" />
<img width="2560" height="1317" alt="싱글 모드 - 현재 상황 정보" src="https://github.com/user-attachments/assets/477a2e70-a187-4bac-a765-0593164ecb1b" />
<img width="2560" height="1316" alt="싱글 모드 - 이야기 끝" src="https://github.com/user-attachments/assets/d5669247-bde2-40cc-8af8-06f28404764d" />

* 사용자 - 멀티 모드
<img width="1731" height="1161" alt="멀티 모드" src="https://github.com/user-attachments/assets/9b821dc3-b226-45bf-9955-72117151b58e" />
<img width="1734" height="1179" alt="멀티 모드 - 방 생성" src="https://github.com/user-attachments/assets/c4e8cea0-aba1-419b-8435-def55a1cab82" />
<img width="1731" height="1318" alt="멀티 모드 - 방장" src="https://github.com/user-attachments/assets/b5985f41-7b4e-4fa5-87d8-7d41e1b74250" />
<img width="1731" height="1316" alt="멀티 모드 - 게임 선택" src="https://github.com/user-attachments/assets/0b05dbde-3ff6-4e97-bb03-403ea5422648" />
<img width="1360" height="998" alt="멀티 모드 - 메신저(보내기)" src="https://github.com/user-attachments/assets/840486f4-65ee-45eb-a914-f3a89d2d49ba" />
<img width="1727" height="1317" alt="멀티 모드 - 캐릭터 선택" src="https://github.com/user-attachments/assets/cf78967b-d208-4af1-95c4-c11f9e8abf62" />
<img width="1732" height="1318" alt="멀티 모드 - 게임" src="https://github.com/user-attachments/assets/052742de-6d01-4307-add7-2bd7c4b3d866" />
<img width="1731" height="1317" alt="멀티 모드 - 게임 결과" src="https://github.com/user-attachments/assets/c6b4cb99-ca3b-43ef-82de-1273380df465" />

### 업적
<img width="2560" height="1316" alt="업적 - 스토리 모드" src="https://github.com/user-attachments/assets/18813e12-9248-4a4c-b244-5768f0928122" />
<img width="2560" height="1318" alt="플레이 기록" src="https://github.com/user-attachments/assets/1467f256-7043-4eb9-b03a-f7fa2a27e46b" />

### 배포
* 사용자 [https://junrae.netlify.app/](https://junrae.netlify.app/)
* 관리자 [https://final-project-admin-frontend--jtxf8mot4g.expo.app/](https://final-project-admin-frontend--jtxf8mot4g.expo.app/)
* 데모 영상 [https://www.youtube.com/watch?v=dbqACVh0raE&feature=youtu.be](https://www.youtube.com/watch?v=dbqACVh0raE&feature=youtu.be)

## 참고
   | 구분 | Repository |
   |:----------------------------------------------:|:------------------------------------:|
   | 관리자 백엔드 | [https://github.com/7ai-final-project/final-project-admin-backend](https://github.com/7ai-final-project/final-project-admin-backend) |
   | 관리자 프론트엔드 | [https://github.com/7ai-final-project/final-project-admin-frontend](https://github.com/7ai-final-project/final-project-admin-frontend) |
   | 사용자 백엔드 | [https://github.com/7ai-final-project/final-project-backend](https://github.com/7ai-final-project/final-project-backend) |
   | 사용자 프론트엔드 | [https://github.com/7ai-final-project/final-project-frontend](https://github.com/7ai-final-project/final-project-frontend) |