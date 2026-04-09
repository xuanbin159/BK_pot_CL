# BK_pot_CL

Reddit 서브레딧(r/Monitors)에서 유저 목록과 해당 유저들의 전체 리뷰 데이터를 수집하는 파이프라인입니다. Google Colab 환경에서 실행되며, 수집된 데이터는 Google Drive에 저장됩니다.

## 개요

이 노트북은 두 단계로 구성됩니다.

**Step 1. 유저 수집** — 대상 서브레딧의 게시글과 댓글을 순회하며 고유 유저명을 추출하여 텍스트 파일로 저장합니다. 중복 방지 로직과 재실행 시 기존 데이터 유지 기능이 포함되어 있습니다.

**Step 2. 리뷰 수집** — 수집된 유저 목록을 기반으로, 각 유저의 전체 댓글(comment)과 게시글(submission)을 JSONL 형식으로 저장합니다. 서브레딧 필터 없이 유저의 모든 활동 이력을 수집합니다.

## 사전 준비

Reddit API 인증 정보가 필요합니다. https://www.reddit.com/prefs/apps 에서 script 타입 앱을 생성한 뒤 `CLIENT_ID`, `CLIENT_SECRET`, `USER_AGENT` 값을 노트북에 입력하세요.

Google Drive 마운트가 필요하며, 데이터 저장 경로는 `/content/drive/MyDrive/usedata/` 입니다.

## 출력 파일

| 파일 | 설명 |
|---|---|
| `unique_users_mo.txt` | 수집된 고유 유저명 목록 |
| `all_user_reviews.jsonl` | 유저별 전체 댓글 및 게시글 데이터 (JSON Lines) |

## JSONL 레코드 필드

각 레코드에는 `username`, `type`(comment 또는 submission), `id`, `created_utc`, `subreddit`, `body`(댓글) 또는 `title`/`selftext`(게시글), `score`, `permalink` 등의 정보가 포함됩니다.

## 실행 방법

1. 노트북 상단의 "Open in Colab" 배지를 클릭합니다.
2. Google Drive를 마운트합니다.
3. Reddit API 인증 정보를 입력합니다.
4. Step 1 셀을 실행하여 유저를 수집합니다.
5. Step 2 셀을 실행하여 리뷰를 수집합니다.

## 의존성

```
praw
```

## 참고 사항

Reddit API의 과도한 호출을 방지하기 위해 유저 간 1초의 딜레이가 적용되어 있습니다. 대량 수집 시 상당한 시간이 소요될 수 있습니다. 기존 파일에 이어서 쓰려면 `main()` 함수 내 파일 열기 모드를 `"w"`에서 `"a"`로 변경하세요.
