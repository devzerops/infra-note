# Class type
|            Classtype           |                        설명                        | 우선순위 |
|:------------------------------:|:--------------------------------------------------:|:--------:|
| attempted-admin                | 관리자 권한 획득 시도                              | high     |
| attempted-user                 | 사용자 권한 획득 시도                              | high     |
| inappropriate-content          | 부적절한 컨텐츠 감지                               | high     |
| policy-violation               | 잠재적인 개인 정보 침해                            | high     |
| shellcode-detect               | 실행 코드 감지                                     | high     |
| successful-admin               | 관리자 권한 획득 성공                              | high     |
| successful-user                | 사용자 권한 획득 성공                              | high     |
| trojan-activity                | 네트워크 트로이목마 탐지                           | high     |
| unsuccessful-user              | 사용자 권한 획득 실패                              | high     |
| web-application-attack         | 웹 응용 프로그램 공격                              | high     |
| attempted-dos                  | DoS 시도                                           | medium   |
| attempted-recon                | 정보 유출 시도                                     | medium   |
| bad-unknown                    | 잠재적인 나쁜 트래픽                               | medium   |
| default-login-attempt          | 기본 사용자 이름과 암호로 로그인 시도              | medium   |
| denial-of-service              | DoS 탐지                                           | medium   |
| misc-attack                    | 기타 공격                                          | medium   |
| non-standard-protocol          | 비표준 프로토콜 또는 이벤트 감지                   | medium   |
| rpc-portmap-decode             | RPC 쿼리 디코드                                    | medium   |
| successful-dos                 | DoS                                                | medium   |
| successful-recon-largescale    | 대규모 정보 유출                                   | medium   |
| successful-recon-limited       | 정보 유출                                          | medium   |
| suspicious-filename-detect     | 의심스러운 파일 이름 탐지                          | medium   |
| suspicious-login               | 의심스러운 사용자 이름을 사용하여 로그인 시도 탐지 | medium   |
| system-call-detect             | 시스템 호출 탐지                                   | medium   |
| unusual-client-port-connection | 클라이언트가 비정상적인 포트 사용                  | medium   |
| web-application-activity       | 잠재적으로 취약한 웹 응용 프로그램에 대한 액세스   | medium   |
| icmp-event                     | 일반적인 ICMP 이벤트                               | low      |
| misc-activity                  | 기타 활동                                          | low      |
| network-scan                   | 네트워크 스캔 탐지                                 | low      |
| not-suspicious                 | 의심스러운 트래픽 아님                             | low      |
| protocol-command-decode        | 일반 프로토콜 명령어 디코드                        | low      |
| string-detect                  | 의심스러운 문자열 탐지                             | low      |
| unknown                        | 알 수 없는 트래픽                                  | low      |
| tcp-connection                 | TCP 연결 탐지                                      | very low |

# example
``` bash
alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"ET USER_AGENTS Go HTTP Client User-Agent"; flow:established,to_server; http.user_agent; content:"Go-http-client"; nocase; fast_pattern; classtype:misc-activity; sid:2024897; rev:2; metadata:attack_target Client_Endpoint, created_at 2017_10_23, deployment Perimeter, former_category USER_AGENTS, signature_severity Major, updated_at 2020_08_13;)
```