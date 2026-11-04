# week09/tools.py 함수 설명

`week09/tools.py`는 영화진흥위원회(KOBIS) 오픈 API로 **일별 박스오피스** 정보를 조회하는 도구 모음이다.

## 공통 설정

| 이름 | 설명 |
|------|------|
| `requests` | HTTP 요청으로 API를 호출하는 라이브러리 |
| `API_KEY` | `secret.py`에서 가져오는 KOBIS API 키 |
| `BASE_URL` | 일별 박스오피스 조회 API 주소 (`searchDailyBoxOfficeList.json`) |

## 함수 목록

### 1. `get_boxoffice_rank_1(date)`

주어진 날짜의 박스오피스 **1위 영화 제목**을 돌려준다.

- **입력**: `date` (문자열) — 8자리 날짜, 예: `"20260824"`
- **출력**: (문자열) 1위 영화 제목, 예: `"오디세이"`
- **동작**
  1. `key`, `targetDt` 파라미터로 API를 호출한다 (timeout 10초).
  2. 응답 JSON의 `boxOfficeResult` → `dailyBoxOfficeList`에서 영화 목록을 꺼낸다.
  3. 첫 번째 영화(`movies[0]`)의 `movieNm`을 돌려준다.

### 2. `get_movie_audience(date, title)`

주어진 날짜에 특정 영화의 **일일 관객 수**를 돌려준다.

- **입력**
  - `date` (문자열) — 8자리 날짜
  - `title` (문자열) — 영화 제목
- **출력**: (문자열)
  - 찾은 경우: 천 단위 쉼표가 붙은 관객 수, 예: `"123,456명"`
  - 못 찾은 경우: `"'<title>' 은(는) <date> 순위에 없습니다."`
- **동작**
  1. 1번 함수와 같은 방식으로 일별 박스오피스 목록을 가져온다.
  2. 목록을 돌며 `movieNm`이 `title`과 **정확히 일치**하는 영화를 찾는다.
  3. 찾으면 `audiCnt`를 정수로 바꿔 `"N명"` 형식으로 돌려준다.
  4. 끝까지 못 찾으면 순위에 없다는 안내 문장을 돌려준다.

## 참고 사항

- `get_movie_audience`에는 docstring이 없다.
- 두 함수에 API 호출 및 목록 추출 코드가 중복되어 있다.
- 예외 처리가 없어 잘못된 날짜나 API 오류 시 `KeyError` / `IndexError`가 발생할 수 있다.
