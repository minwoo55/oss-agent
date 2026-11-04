작성:Claude / 확인: (김민우)

# week09/tools.py 함수 설명

- 영화진흥위원회(KOBIS) 오픈 API로 일별 박스오피스 정보를 조회하는 도구 모음임

## 공통 설정

- `requests`: API 호출에 쓰는 HTTP 라이브러리임
- `API_KEY`: `secret.py`에서 가져오는 KOBIS API 키임
- `BASE_URL`: 일별 박스오피스 조회 API 주소임 (`searchDailyBoxOfficeList.json`)

## 함수 목록

### 1. `get_boxoffice_rank_1(date)`

- 역할: 주어진 날짜의 박스오피스 1위 영화 제목을 돌려줌
- 입력: `date` (문자열) — 8자리 날짜, 예: `"20260824"`
- 출력: (문자열) 1위 영화 제목, 예: `"오디세이"`
- 동작
  - `key`, `targetDt` 파라미터로 API 호출함 (timeout 10초)
  - 응답 JSON의 `boxOfficeResult` → `dailyBoxOfficeList`에서 영화 목록 꺼냄
  - 첫 번째 영화(`movies[0]`)의 `movieNm` 돌려줌

### 2. `get_movie_audience(date, title)`

- 역할: 주어진 날짜에 특정 영화의 일일 관객 수를 돌려줌
- 입력
  - `date` (문자열) — 8자리 날짜
  - `title` (문자열) — 영화 제목
- 출력: (문자열)
  - 찾은 경우: 천 단위 쉼표 붙은 관객 수, 예: `"123,456명"`
  - 못 찾은 경우: `"'<title>' 은(는) <date> 순위에 없습니다."`
- 동작
  - 1번 함수와 같은 방식으로 일별 박스오피스 목록 가져옴
  - 목록을 돌며 `movieNm`이 `title`과 정확히 일치하는 영화 찾음
  - 찾으면 `audiCnt`를 정수로 바꿔 `"N명"` 형식으로 돌려줌
  - 끝까지 못 찾으면 순위에 없다는 안내 문장 돌려줌

## 참고 사항

- `get_movie_audience`에는 docstring 없음
- 두 함수에 API 호출·목록 추출 코드가 중복됨
- 예외 처리 없어 잘못된 날짜나 API 오류 시 `KeyError` / `IndexError` 발생 가능함

## 확인할 것

- 예시 관객 수 `"123,456명"`은 형식 설명용 임의 값임 (실제 API 결과 아님)
- 제목 비교가 정확히 일치 방식이라 띄어쓰기·부호 차이 있으면 못 찾음
- tools.py는 규칙상 수정하지 않았으므로 위 문제점은 기록만 함
