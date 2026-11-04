# week09/tools.py 함수 정리 (수정 전)

`week09/tools.py`는 영화진흥위원회(KOBIS) 오픈 API에서 **일별 박스오피스** 정보를 가져오는 도구 모음이다.

## 공통 설정

- `requests` 라이브러리로 API를 호출한다.
- API 키는 `secret.py`의 `API_KEY`에서 가져온다.
- `BASE_URL`: KOBIS 일별 박스오피스 조회 API 주소 (`searchDailyBoxOfficeList.json`)

## 함수 목록

### 1. `get_boxoffice_rank_1(date)`

그 날짜의 박스오피스 **1위 영화 제목**을 알려준다.

| 항목 | 내용 |
|------|------|
| 받는 것 | `date` (문자열) - 8자리 날짜. 예: `"20260824"` |
| 돌려주는 것 | (문자열) 1위 영화 제목. 예: `"오디세이"` |

동작 순서:
1. `key`, `targetDt`를 파라미터로 API를 호출한다 (timeout 10초).
2. 응답 JSON에서 `boxOfficeResult` → `dailyBoxOfficeList`를 꺼낸다.
3. 목록의 첫 번째 영화(`movies[0]`)의 `movieNm`을 돌려준다.

### 2. `get_movie_audience(date, title)`

그 날짜에 특정 영화의 **일일 관객 수**를 알려준다.

| 항목 | 내용 |
|------|------|
| 받는 것 | `date` (문자열) - 8자리 날짜, `title` (문자열) - 영화 제목 |
| 돌려주는 것 | (문자열) 관객 수. 예: `"123,456명"` / 순위에 없으면 `"'제목' 은(는) 날짜 순위에 없습니다."` |

동작 순서:
1. `get_boxoffice_rank_1`과 같은 방식으로 일별 박스오피스 목록을 가져온다.
2. 목록을 돌면서 `movieNm`이 `title`과 정확히 같은 영화를 찾는다.
3. 찾으면 `audiCnt`를 정수로 바꾸고 천 단위 쉼표를 붙여 `"N명"` 형태로 돌려준다.
4. 못 찾으면 순위에 없다는 안내 문장을 돌려준다.

## 눈에 띄는 점

- `get_movie_audience`에는 docstring이 없다 (`get_boxoffice_rank_1`에는 있음).
- 두 함수 모두 API 호출 + 목록 추출 코드(4줄)가 똑같이 반복된다.
- 에러 처리가 없다: 잘못된 날짜나 API 오류 응답이면 `KeyError`/`IndexError`가 날 수 있다.
- 영화 제목은 완전히 일치해야만 찾는다 (띄어쓰기 등이 다르면 못 찾음).
