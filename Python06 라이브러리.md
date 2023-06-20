---
date: 2021.12.
title: 파이썬 라이브러리
tag: 
---


# 파이썬 라이브러리

```
01-01 textwrap.shorten - 문자열 말줄임
01-02 textwrap.wrap - 문자열 줄바꿈
01-03 re - 정규표현식
02-01 struct - C 구조체 다루기
03-01 datetime.date - 날짜
03-02 datetime.timedelta - 날짜의 차이
03-03 calendar.isleap - 윤년확인
03-04 collections.deque - 데크
03-05 collections.namedtuple - 키값이 있는 튜플
03-06 collections.Counter - 동일한 요소의 갯수
03-07 collections.defaultdict - 딕셔너리의 초깃값 
03-08 heapq - 우선 순위 큐
03-09 pprint - 예쁘게 출력하기
03-10 bisect - 이진 탐색
03-11 enum - 상수 집합
03-12 graphlib.TopologicalSorter - 위상정렬
04-01 math.gcd - 최대공약수
04-02 math.lcm - 최소공배수
04-03 decimal.Decimal - 정확한 소숫점 연산
04-04 fractions - 유리수
04-05 random - 난수생성
04-06 statistics - 평균값과 중앙값
05-01 itertools.cycle - 무한 반복자
05-02 itertools.accumulate - 누적합 계산
05-03 itertools.groupby - 키값으로 분류
05-04 itertools.zip_longest - 사이즈가 큰 것을 기준으로 묶기
05-05 itertools.permutations - 순열
05-06 itertools.combinations - 조합
05-07 functools.cmp_to_key - 함수로 정렬
05-08 functools.lru_cache - 함수의 결과를 캐시
05-09 functools.partial - 인수를 지정하여 함수 재정의
05-10 functools.reduce - 함수 적용하여 단일값으로 줄여나가기
05-11 functools.wraps - 래퍼함수의 속성 유지
05-12 operator.itemgetter - 다중 수준 정렬
06-01 pathlib - 파일 시스템 경로를 객체로 다루기
06-02 os.path - 파일 시스템 경로
06-03 fileinput - 여러파일을 한꺼번에 처리하기
06-04 filecmp - 디렉터리와 파일비교
06-05 tempfile - 임시파일
06-06 glob - 파일 검색
06-07 fnmatch - 파일 매치
06-08 linecache - 파일에서 임의의 줄 가져오기
06-09 shutil - 파일 카피
07-01 pickle - 객체 저장하고 불러오기
07-02 copyreg - 안전하게 pickle 사용하기
07-03 shelve - 딕셔너리를 영구적으로 저장 
07-04 sqlite3 - SQLite 데이터베이스
08-01 zlib - 데이터 압축하기1
08-02 gzip - 압축하여 파일로 저장하기
08-03 bz2 - 데이터 압축하기2
08-04 lzma - 데이터 압축하기3
08-05 zipfile - 파일 합치기1
08-06 tarfile - 파일 합치기2
09-01 csv - CSV 파일 읽고 쓰기
09-02 configparser - ini 파일 처리하기




122189
title="10장 암호화 서비스">
0

10장 암호화 서비스




122201
title="10-01 hashlib - 문자열 해싱">
20

10-01 hashlib - 문자열 해싱




122425
title="10-02 hmac - 메시지 인증을 위한 키 해싱">
20

10-02 hmac - 메시지 인증을 위한 키 해싱




122540
title="10-03 secrets - 편리한 난수 생성기">
20

10-03 secrets - 편리한 난수 생성기




122775
title="11장 일반 운영체제 서비스">
0

11장 일반 운영체제 서비스




122776
title="11-01 io.StringIO - 문자열을 파일처럼 사용하기">
20

11-01 io.StringIO - 문자열을 파일처럼 사용하기




123295
title="11-02 argparse - 명령행 파서">
20

11-02 argparse - 명령행 파서




123324
title="11-03 logging - 로그파일 생성하기">
20

11-03 logging - 로그파일 생성하기




123504
title="11-04 getpass - 에코없이 암호 입력하기">
20

11-04 getpass - 에코없이 암호 입력하기




123586
title="11-05 curses - 터미널 그래픽 애플리케이션">
20

11-05 curses - 터미널 그래픽 애플리케이션




123697
title="11-06 platform - 시스템 정보 확인하기">
20

11-06 platform - 시스템 정보 확인하기




124099
title="11-07 ctypes - C 라이브러리 사용하기">
20

11-07 ctypes - C 라이브러리 사용하기




124142
title="12장 동시실행">
0

12장 동시실행




124143
title="12-01 threading - 스레드 기반의 병렬처리">
20

12-01 threading - 스레드 기반의 병렬처리




124290
title="12-02 multiprocessing - 프로세스 기반의 병렬처리">
20

12-02 multiprocessing - 프로세스 기반의 병렬처리




124348
title="12-03 concurrent.futures - 병렬작업 실행하기">
20

12-03 concurrent.futures - 병렬작업 실행하기




124373
title="12-04 subprocess - 시스템 명령어 실행">
20

12-04 subprocess - 시스템 명령어 실행




124756
title="12-05 sched - 이벤트 스케줄러">
20

12-05 sched - 이벤트 스케줄러




125091
title="13장 네트워킹과 프로세스간 통신">
0

13장 네트워킹과 프로세스간 통신




125092
title="13-01 asyncio - 비동기 I/O">
20

13-01 asyncio - 비동기 I/O




125301
title="13-02 socket - 저수준 네트워킹 인터페이스">
20

13-02 socket - 저수준 네트워킹 인터페이스




125373
title="13-03 ssl - SSL이 적용된 소켓">
20

13-03 ssl - SSL이 적용된 소켓




125626
title="13-04 select - I/O 멀티플렉싱">
20

13-04 select - I/O 멀티플렉싱




125716
title="13-05 selectors - 고수준 I/O 멀티플렉싱">
20

13-05 selectors - 고수준 I/O 멀티플렉싱




126014
title="13-06 signal - 시그널 처리">
20

13-06 signal - 시그널 처리




126087
title="14장 인터넷 데이터 처리">
0

14장 인터넷 데이터 처리




126088
title="14-01 json - JSON 데이터 처리">
20

14-01 json - JSON 데이터 처리




126319
title="14-02 base64 - 바이너리 데이터를 문자열로 변환">
20

14-02 base64 - 바이너리 데이터를 문자열로 변환




126499
title="14-03 binascii - 16진수 문자열 처리">
20

14-03 binascii - 16진수 문자열 처리




126660
title="14-04 quopri - quoted-printable 인코딩">
20

14-04 quopri - quoted-printable 인코딩




127384
title="14-05 uu - 바이너리 파일을 텍스트 파일로 변환">
20

14-05 uu - 바이너리 파일을 텍스트 파일로 변환




127507
title="15장 구조화된 마크업 처리도구">
0

15장 구조화된 마크업 처리도구




127508
title="15-01 html - HTML 문자 이스케이프 처리하기">
20

15-01 html - HTML 문자 이스케이프 처리하기




127665
title="15-02 html.parser - HTML 문서 분석하기">
20

15-02 html.parser - HTML 문서 분석하기




127780
title="15-03 xml.etree.ElementTree - XML 문서 작성하기">
20

15-03 xml.etree.ElementTree - XML 문서 작성하기




127832
title="15-04 xml.etree.ElementTree - XML 문서 분석하기">
20

15-04 xml.etree.ElementTree - XML 문서 분석하기




128226
title="16장 인터넷 프로토콜과 지원">
0

16장 인터넷 프로토콜과 지원




128227
title="16-01 webbrowser - 편리한 웹 브라우저 제어기">
20

16-01 webbrowser - 편리한 웹 브라우저 제어기




129281
title="16-02 cgi - CGI 프로그램 작성하기">
20

16-02 cgi - CGI 프로그램 작성하기




129478
title="16-03 cgitb - CGI 오류 확인하기">
20

16-03 cgitb - CGI 오류 확인하기




129687
title="16-04 wsgiref - WSGI 프로그램 작성하기">
20

16-04 wsgiref - WSGI 프로그램 작성하기




129741
title="16-05 urllib - URL 처리하기">
20

16-05 urllib - URL 처리하기




129903
title="16-06 http.client - HTTP 클라이언트">
20

16-06 http.client - HTTP 클라이언트




130126
title="16-07 ftplib - FTP 클라이언트">
20

16-07 ftplib - FTP 클라이언트




130215
title="16-08 poplib - POP3 이메일 확인하기">
20

16-08 poplib - POP3 이메일 확인하기




130371
title="16-09 imaplib - IMAP4 이메일 확인하기">
20

16-09 imaplib - IMAP4 이메일 확인하기




130611
title="16-10 nntplib - 뉴스 그룹 조회하기">
20

16-10 nntplib - 뉴스 그룹 조회하기




130932
title="16-11 smtplib - 파일 첨부하여 이메일 보내기">
20

16-11 smtplib - 파일 첨부하여 이메일 보내기




131204
title="16-12 telnetlib - 텔넷 클라이언트">
20

16-12 telnetlib - 텔넷 클라이언트




131351
title="16-13 uuid - 유일한 ID 생성">
20

16-13 uuid - 유일한 ID 생성




131489
title="16-14 socketserver - 소켓서버 프레임워크">
20

16-14 socketserver - 소켓서버 프레임워크




131607
title="16-15 http.server - HTTP서버">
20

16-15 http.server - HTTP서버




132031
title="16-16 xmlrpc - XMLRPC 서버와 클라이언트">
20

16-16 xmlrpc - XMLRPC 서버와 클라이언트




132132
title="17장 기타 서비스">
0

17장 기타 서비스




132133
title="17-01 imghdr - 이미지 유형 판단">
20

17-01 imghdr - 이미지 유형 판단




132241
title="17-02 turtle - 터틀 그래픽">
20

17-02 turtle - 터틀 그래픽




132436
title="17-03 cmd - 사용자 친화적인 명령행 프로그램 만들기">
20

17-03 cmd - 사용자 친화적인 명령행 프로그램 만들기




132536
title="17-04 shlex - 간단한 어휘 분석">
20

17-04 shlex - 간단한 어휘 분석




132610
title="17-05 tkinter - 편리한 GUI 툴킷">
20

17-05 tkinter - 편리한 GUI 툴킷




132725
title="17-06 unittest - 단위테스트">
20

17-06 unittest - 단위테스트




132772
title="17-07 doctest - 독스트링을 이용한 테스트">
20

17-07 doctest - 독스트링을 이용한 테스트




133019
title="17-08 timeit - 간단한 코드의 실행시간 측정하기">
20

17-08 timeit - 간단한 코드의 실행시간 측정하기




133085
title="17-09 pdb - 파이썬 디버거">
20

17-09 pdb - 파이썬 디버거




133137
title="17-10 sys.argv - 파이썬 스크립트에 파라미터 전달">
20

17-10 sys.argv - 파이썬 스크립트에 파라미터 전달




133161
title="17-11 dataclasses - 데이터클래스">
20

17-11 dataclasses - 데이터클래스




133139
title="17-12 abc - 추상클래스">
20

17-12 abc - 추상클래스




133281
title="17-13 atexit - 종료 처리기">
20

17-13 atexit - 종료 처리기




133283
title="17-14 traceback - 오류 추적하기">
20

17-14 traceback - 오류 추적하기




134900
title="17-15 typing - 타입 힌트 지원">
20

17-15 typing - 타입 힌트 지원




133199
title="18장 외부 라이브러리">
0

18장 외부 라이브러리




133201
title="18-01 pip - 파이썬 라이브러리 설치">
20

18-01 pip - 파이썬 라이브러리 설치




133287
title="18-02 requests - 간편한 HTTP 클라이언트">
20

18-02 requests - 간편한 HTTP 클라이언트




104599
title="18-03 diff_match_patch - 문자열의 차이">
20

18-03 diff_match_patch - 문자열의 차이




105448
title="18-04 faker - 가짜 데이터 생성기">
20

18-04 faker - 가짜 데이터 생성기




106362
title="18-05 sympy - 방정식 풀이">
20

18-05 sympy - 방정식 풀이




133214
title="18-06 pyinstaller - 파이썬 프로그램을 exe 파일로 배포하기">
20

18-06 pyinstaller - 파이썬 프로그램을 exe 파일로 배포하기




133293
title="마치며">
0

마치며




134566
title="부록">
0

부록




134567
title="01 파이썬과 유니코드">
20

01 파이썬과 유니코드




134789
title="02 클로저와 데코레이터">
20

02 클로저와 데코레이터




134909
title="03 이터레이터와 제너레이터">
20

03 이터레이터와 제너레이터




134883
title="04 파이썬 타입 어노테이션">
20

04 파이썬 타입 어노테이션




134994
title="05 str과 repr">
20

05 str과 repr




</div>
```



### Enum

```python
from enum import Enum, auto
class ABC(Enum):
    A = auto()
    B = auto()
    C = auto()

class AutoName(Enum):
    def _generate_next_value_(name, start, count, last_values):
        return name

class ABC2(AutoName):
    A = auto()
    B = auto()
    C = auto()
    
print(list(ABC))
print(list(ABC2))


def Enum(*sequential):
    enums = dict(zip(sequential, range(len(sequential))))
    t = type('enum', (), enums)
    return t

ABC = Enum('A', 'B', 'C')
print(ABC.A)


from collections import namedtuple
def Enum(*sequential):
    enum = namedtuple('Enum', sequential)
    return enum(*sequential)

ABC = Enum('A', 'B', 'C')
print(ABC.A)
```

