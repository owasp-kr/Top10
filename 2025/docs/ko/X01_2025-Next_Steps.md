# 다음 단계

OWASP Top 10은 이름 그대로 가장 중요한 10가지 위험으로만 선정한다. 각 버전의 OWASP Top 10에는 포함 여부를 두고 충분히 검토되었으나, 다른 위험들이 더 빈번하게 발생하고 영향도도 더 컸기 때문에 최종 목록에 포함되지 않은 "경계선상(on the cusp)" 위험들이 존재한다.

아래의 세 가지 이슈는 발견 및 조치에 투자할 만한 가치가 크며, 성숙한 애플리케이션 보안 프로그램을 목표로 하는 조직, 보안 자문 회사, 또는 제품의 커버리지를 확장하려는 보안 도구 벤더에 특히 유용하게 사용될 수 있다.


## X01:2025 애플리케이션 복원력 부족

### 배경. 

이 카테고리는 2021년의 서비스 거부(Denial of Service)를 재명명한 것이다. 기존 명칭은 근본 원인보다는 발생 현상을 설명하는 성격이 강해, 이를 보완하기 위해 재명명되었다. 이 카테고리는 복원력과 관련된 약점을 설명하는 CWE에 초점을 둔다. 점수 산정은 A10:2025-잘못된 예외 처리와 매우 근접했다. 관련된 CWE로는 *CWE-400 통제되지 않은 자원 소비, CWE-409 고압축 데이터의 부적절한 처리(데이터 증폭), CWE-674 통제되지 않은 재귀*, 그리고 *CWE-835 종료 조건에 도달할 수 없는 루프(무한루프).*가 있다.

### 점수표.


<table>
  <tr>
   <td>해당되는 CWE 개수
   </td>
   <td>최대 취약점 발생률
   </td>
   <td>평균 취약점 발생률
   </td>
   <td>최대 테스트 커버리지
   </td>
   <td>평균 테스트 커버리지
   </td>
   <td>평균 가중 악용도
   </td>
   <td>평균 가중 영향도
   </td>
   <td>총 발생 건수
   </td>
   <td>총 CVE 건수
   </td>
  </tr>
  <tr>
   <td>16
   </td>
   <td>20.05%
   </td>
   <td>4.55%
   </td>
   <td>86.01%
   </td>
   <td>41.47%
   </td>
   <td>7.92
   </td>
   <td>3.49
   </td>
   <td>865,066
   </td>
   <td>4,423
   </td>
  </tr>
</table>



### 설명. 

이 카테고리는 애플리케이션이 스트레스, 장애 및 예외 케이스에 대응하는 방식 전반에 존재하는 시스템적 약점을 의미하며, 그 결과 장애 상황에서 애플리케이션이 정상 상태로 복구하지 못할 수 있다. 애플리케이션이 예기치 않은 조건, 리소스 제약 및 기타 불리한 이벤트를 원활하게(gracefully) 처리하지 못하거나, 견디지 못하거나, 또는 복구하지 못할 경우, 가용성 문제(일반적으로)로 이어지며, 그 외에도 데이터 손상, 민감 데이터 노출, 연쇄 장애, 및/또는 보안 통제 우회를 유발할 수 있다.

또한 [X02:2025 메모리 관리 실패](#x022025-memory-management-failures) 역시 애플리케이션, 또는 심지어 전체 시스템의 장애로 이어질 수 있다.

### 대응방안. 

이 유형의 취약점을 예방하기 위해서는 시스템의 장애와 복구를 기본 전제로 설계해야 한다.

* 제한, 할당량 및 장애 극복 기능(failover) 기능을 추가하되, 특히 자원을 가장 많이 소모하는 작업에 주의를 기울인다.
* 자원 소모가 큰 페이지를 식별하고 사전에 대비하라: 공격 표면을 줄이되, 특히 불명의 또는 신뢰할 수 없는 사용자에게 불필요한 '가젯(gadgets)'과 많은 자원(예: CPU, 메모리)을 요구하는 기능을 노출하지 않도록 한다.
* 입력값은 크기 제한을 적용하고 허용 리스트 기반으로 엄격히 검증한뒤, 철저히 테스트한다.
* 응답 크기를 제한하고, 가공되지 않은(raw) 응답을 클라이언트에 그대로 반환하지 않는다(서버 측에서 우선 처리한다).
* 기본값으로 안전/차단(절대로 오픈을 사용하지 않는다)로 설정하고, 우선적으로 차단(deny by default)하며, 오류가 발생하면 롤백한다.
* 리퀘스트 스레드에서 동기식 차단 호출(blocking synchronous call)을 피한다(비동기/논블로킹 사용, 타임아웃 설정, 동시성 제한 등).
* 에러 처리 기능을 신중하게 테스트한다.
* 서킷 브레이커, 격벽(bulkhead), 다시 시도(retry logic), 우아한 성능 저하(graceful degradation)와 같은 복원력 패턴을 구현한다.
* 성능 및 부하 테스트를 수행한다. 조직의 위험 수용 범위 내에서 카오스 엔지니어링을 도입한다.
* 합리적이고 비용적으로 감당 가능한 범위에서 이중화(redundancy)을 구현하고, 이를 전제로 아키텍처를 설계한다.
* 모니터링, 옵저버빌리티, 알림을 구현한다.
* RFC 2267을 준수해 잘못된 발신자 주소를 필터링한다.
* 핑거프린트, IP 또는 행위 기반 동적 탐지로 알려진 봇넷을 차단한다.
* 작업 증명(Proof-of-Work): 정상 사용자에게는 큰 영향이 없으면서 대량 요청을 보내려는 봇에는 영향을 주도록, 자원 소모 작업을 *공격자* 측에서 수행하도록 시작한다. 시스템의 전체 부하가 증가하면 특히 신뢰도가 낮거나 봇으로 보이는 시스템에 대해 작업증명 난이도를 높인다.
* 작업 증명(Proof-of-Work) 적용. 자격 증명을 적용하여 자원 소모 작업을 서버가 아니라 *공격자* 측에 부과한다. 정상 사용자 경험에 미치는 영향은 최소화하고, 시스템 부하가 상승할수록 자격증명 난이도를 노이고, 특히 신뢰도가 낮거나 봇으로 판단되는 트래픽에는 더 높은 난이도를 적용한다.
* 비활성 시간과 최종 타임아웃을 기준으로 서버 측 세션 시간을 제한한다.
* 세션에 저장되는 상태 정보는 최소화한다.


### 공격 시나리오 예시.  

**시나리오 1:** 공격자가 리소스 소모를 유도해 시스템 장애를 유발하고, 결과적으로 서비스 거부(DoS) 상태를 만든다. 예로 메모리 고갈, 디스크 용량 소진, CPU 사용량 포화, 커넥션 무제한 연결 등이 있다.

**시나리오 2:** 입력값 퍼징을 통해 비정상 입력을 대량 주입하고, 그 결과 비즈니스 로직을 오동작시키는 형태의 응답이 유도되도록 만든다.

**시나리오 3:** 공격자가 애플리케이션의 의존성을 공격하여 API 또는 기타 외부 서비스를 다운시키며, 애플리케이션이 동작할 수 없게 한다.


### 참조.

* [OWASP Cheat Sheet: Denial of Service](https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html)
* [OWASP MASVS‑RESILIENCE](https://mas.owasp.org/MASVS/11-MASVS-RESILIENCE/)
* [ASP.NET Core Best Practices (Microsoft)](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/best-practices?view=aspnetcore-9.0)
* [Resilience in Microservices: Bulkhead vs Circuit Breaker (Parser)](https://medium.com/@parserdigital/resilience-in-microservices-bulkhead-vs-circuit-breaker-54364c1f9d53)
* [Bulkhead Pattern (Geeks for Geeks)](https://www.geeksforgeeks.org/system-design/bulkhead-pattern/)
* [NIST Cybersecurity Framework (CSF)](https://www.nist.gov/cyberframework)
* [Avoid Blocking Calls: Go Async in Java (Devlane)](https://www.devlane.com/blog/avoid-blocking-calls-go-async-in-java)

### 해당되는 CWE 목록.

* [CWE-73  External Control of File Name or Path](https://cwe.mitre.org/data/definitions/73.html)
* [CWE-183 Permissive List of Allowed Inputs](https://cwe.mitre.org/data/definitions/183.html)
* [CWE-256 Plaintext Storage of a Password](https://cwe.mitre.org/data/definitions/256.html)
* [CWE-266 Incorrect Privilege Assignment](https://cwe.mitre.org/data/definitions/266.html)
* [CWE-269 Improper Privilege Management](https://cwe.mitre.org/data/definitions/269.html)
* [CWE-286 Incorrect User Management](https://cwe.mitre.org/data/definitions/286.html)
* [CWE-311 Missing Encryption of Sensitive Data](https://cwe.mitre.org/data/definitions/311.html)
* [CWE-312 Cleartext Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/312.html)
* [CWE-313 Cleartext Storage in a File or on Disk](https://cwe.mitre.org/data/definitions/313.html)
* [CWE-316 Cleartext Storage of Sensitive Information in Memory](https://cwe.mitre.org/data/definitions/316.html)
* [CWE-362 Concurrent Execution using Shared Resource with Improper Synchronization ('Race Condition')](https://cwe.mitre.org/data/definitions/362.html)
* [CWE-382 J2EE Bad Practices: Use of System.exit()](https://cwe.mitre.org/data/definitions/382.html)
* [CWE-419 Unprotected Primary Channel](https://cwe.mitre.org/data/definitions/419.html)
* [CWE-434 Unrestricted Upload of File with Dangerous Type](https://cwe.mitre.org/data/definitions/434.html)
* [CWE-436 Interpretation Conflict](https://cwe.mitre.org/data/definitions/436.html)
* [CWE-444 Inconsistent Interpretation of HTTP Requests ('HTTP Request/Response Smuggling')](https://cwe.mitre.org/data/definitions/444.html)
* [CWE-451 User Interface (UI) Misrepresentation of Critical Information](https://cwe.mitre.org/data/definitions/451.html)
* [CWE-454 External Initialization of Trusted Variables or Data Stores](https://cwe.mitre.org/data/definitions/454.html)
* [CWE-472 External Control of Assumed-Immutable Web Parameter](https://cwe.mitre.org/data/definitions/472.html)
* [CWE-501 Trust Boundary Violation](https://cwe.mitre.org/data/definitions/501.html)
* [CWE-522 Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)
* [CWE-525 Use of Web Browser Cache Containing Sensitive Information](https://cwe.mitre.org/data/definitions/525.html)
* [CWE-539 Use of Persistent Cookies Containing Sensitive Information](https://cwe.mitre.org/data/definitions/539.html)
* [CWE-598 Use of GET Request Method With Sensitive Query Strings](https://cwe.mitre.org/data/definitions/598.html)
* [CWE-602 Client-Side Enforcement of Server-Side Security](https://cwe.mitre.org/data/definitions/602.html)
* [CWE-628 Function Call with Incorrectly Specified Arguments](https://cwe.mitre.org/data/definitions/628.html)
* [CWE-642 External Control of Critical State Data](https://cwe.mitre.org/data/definitions/642.html)
* [CWE-646 Reliance on File Name or Extension of Externally-Supplied File](https://cwe.mitre.org/data/definitions/646.html)
* [CWE-653 Improper Isolation or Compartmentalization](https://cwe.mitre.org/data/definitions/653.html)
* [CWE-656 Reliance on Security Through Obscurity](https://cwe.mitre.org/data/definitions/656.html)
* [CWE-657 Violation of Secure Design Principles](https://cwe.mitre.org/data/definitions/657.html)
* [CWE-676 Use of Potentially Dangerous Function](https://cwe.mitre.org/data/definitions/676.html)
* [CWE-693 Protection Mechanism Failure](https://cwe.mitre.org/data/definitions/693.html)
* [CWE-799 Improper Control of Interaction Frequency](https://cwe.mitre.org/data/definitions/799.html)
* [CWE-807 Reliance on Untrusted Inputs in a Security Decision](https://cwe.mitre.org/data/definitions/807.html)
* [CWE-841 Improper Enforcement of Behavioral Workflow](https://cwe.mitre.org/data/definitions/841.html)
* [CWE-1021 Improper Restriction of Rendered UI Layers or Frames](https://cwe.mitre.org/data/definitions/1021.html)
* [CWE-1022 Use of Web Link to Untrusted Target with window.opener Access](https://cwe.mitre.org/data/definitions/1022.html)
* [CWE-1125 Excessive Attack Surface](https://cwe.mitre.org/data/definitions/1125.html)


## X02:2025 Memory Management Failures

### 배경. 

Languagess like Java, C#, JavaScript/TypeScript (node.js), Go, and "safe" Rust are memory safe. Memory management problems tend to happen in non-memory safe languages such as C and C++. This category scored the lowest on the community survey and low in the data despite having the third most related CVEs. We believe this is due to the predominance of web applications over more traditional desktop applications. Memory management vulnerabilities frequently have the highest CVSS scores. 


### 점수표.


<table>
  <tr>
   <td>해당되는 CWE 개수
   </td>
   <td>최대 취약점 발생률
   </td>
   <td>평균 취약점 발생률
   </td>
   <td>최대 테스트 커버리지
   </td>
   <td>평균 테스트 커버리지
   </td>
   <td>평균 가중 악용도
   </td>
   <td>평균 가중 영향도
   </td>
   <td>총 발생 건수
   </td>
   <td>총 CVE 건수
   </td>
  </tr>
  <tr>
   <td>24
   </td>
   <td>2.96%
   </td>
   <td>1.13%
   </td>
   <td>55.62%
   </td>
   <td>28.45%
   </td>
   <td>6.75
   </td>
   <td>4.82
   </td>
   <td>220,414
   </td>
   <td>30,978
   </td>
  </tr>
</table>



### 설명. 

When an application is forced to manage memory itself, it is very easy to make mistakes. Memory safe languages are being used more often, but there are still many legacy systems in production worldwide, new low-level systems that require the use of non-memory safe languages, and web applications that interact with mainframes, IoT devices, firmware, and other systems that may be forced to manage their own memory. Representative CWEs are *CWE-120 Buffer Copy without Checking Size of Input ('Classic Buffer Overflow')* and *CWE-121 Stack-based Buffer Overflow*.

Memory management failures can happen when:

* You do not allocate enough memory for a variable
* You do not validate input, causing an overflow of the heap, the stack, a buffer
* You store a data value that is larger than the type of the variable can hold 
* You attempt to use unallocated memory or address spaces
* You create off-by-one errors (counting from 1 instead of zero)
* You try to access an object after its been freed
* You use uninitialized variables
* You leak memory or otherwise use up all available memory in error until our application fails

Memory management failures can lead to failure of the application or even the entire system, see also [X01:2025 Lack of Application Resilience](#x012025-lack-of-application-resilience)


### 대응 방안. 

The best way to prevent memory management failures is to use a memory-safe language. Examples include Rust, Java, Go, C#, Python, Swift, Kotlin, JavaScript, etc. When creating new applications, try hard to convince your organization that it is worth the learning curve to switch to a memory-safe language. If performing a full refactor, push for a rewrite in a memory-safe language when it is possible and feasible.

If you are unable to use a memory-safe language, perform the following:

* Enable the following server features that make memory management errors harder to exploit: address space layout randomization (ASLR), Data Execution Protection (DEP), and Structured Exception Handling Overwrite Protection (SEHOP).
* Monitor your application for memory leaks.
* Validate all input to your system very carefully, and reject all input that does not meet expectations.
* Study the language you are using and make a list of unsafe and more-safe functions, then share that list with your entire team. If possible, add it to your secure coding guideline or standard. For example, in C, prefer strncpy() over strcpy() and strncat() over strcat().
* If your language or framework offers memory safety libraries, use them. For example: Safestringlib or SafeStr.
* Use managed buffers and strings rather than raw arrays and pointers whenever possible.
* Take secure coding training that focuses on memory issues and/or your language of choice. Inform your trainer that you are concerned about memory management failures.
* Perform code reviews and/or static analyses.
* Use compiler tools that help with memory management such as StackShield, StackGuard, and Libsafe.
* Perform fuzzing on every input to your system.
* If you have a penetration test performed, inform your tester that you are concerned about memory management failures and that you would like them to pay special attention to this while testing.
*  Fix all compiler errors *and* warnings. Do not ignore warnings because your program compiles.
* Ensure your underlying infrastructure is regularly patched, scanned, and hardened.
* Monitor your underlying infrastructure specifically for potential memory vulnerabilities and other failures.
* Consider using [canaries](https://en.wikipedia.org/wiki/Buffer_overflow_protection#Canaries) to protect your address stack from overflow attacks.

### 공격 시나리오 예시. 

**Scenario #1:** Buffer overflows are the most famous memory vulnerability, a situation where an attacker submits more information into a field than it can accept, such that it overflows the buffer created for the underlying variable. In a successful attack, the overflow characters overwrite the stack pointer, allowing the attacker to insert malicious instructions into your program.

**Scenario #2:** Use-After-Free (UAF) happens often enough that it’s a semi-common browser bug bounty submission. Imagine a web browser processing JavaScript that manipulates DOM elements. The attacker crafts a JavaScript payload that creates an object (such as a DOM element) and obtains references to it. Through careful manipulation, they trigger the browser to free the object's memory while keeping a dangling pointer to it. Before the browser realizes the memory has been freed, the attacker allocates a new object that occupies the *same* memory space. When the browser tries to use the original pointer, it now points to attacker-controlled data. If this pointer was for a virtual function table, the attacker can redirect code execution to their payload. 

**Scenario #3:** A network service that accepts user input, doesn’t properly validate or sanitize it, then passes it directly to the logging function. The input from the user is passed to the logging function as syslog(user_input) instead of syslog("%s", user_input), which doesn’t specify the format. The attacker sends malicious payloads containing format specifiers such as %x to read stack memory (sensitive data disclosure) or %n to write to memory addresses. By chaining together multiple format specifiers they could map out the stack, locate important addresses, and then overwrite them. This would be a Format string vulnerability (uncontrolled string format). 

Note: modern browsers use many levels of defenses to defend against such attacks, including [browser sandboxing](https://www.geeksforgeeks.org/ethical-hacking/what-is-browser-sandboxing/#types-of-browser-sandboxing) ASLR, DEP/NX, RELRO, and PIE. A memory management failure attack on a browser is not a simple attack to carry out.

### 참조.

* [OWASP community pages: Memory leak,](https://owasp.org/www-community/vulnerabilities/Memory_leak) [Doubly freeing memory,](https://owasp.org/www-community/vulnerabilities/Doubly_freeing_memory) [& Buffer Overflow](https://owasp.org/www-community/vulnerabilities/Buffer_Overflow)
* [Awesome Fuzzing: a list of fuzzing resources](https://github.com/secfigo/Awesome-Fuzzing) 
* [Project Zero Blog](https://googleprojectzero.blogspot.com)
* [Microsoft MSRC Blog](https://www.microsoft.com/en-us/msrc/blog)

### 해당되는 CWE 목록.
* [CWE-14 Compiler Removal of Code to Clear Buffers](https://cwe.mitre.org/data/definitions/14.html)
* [CWE-119 Improper Restriction of Operations within the Bounds of a Memory Buffer](https://cwe.mitre.org/data/definitions/119.html)
* [CWE-120 Buffer Copy without Checking Size of Input ('Classic Buffer Overflow')](https://cwe.mitre.org/data/definitions/120.html)
* [CWE-121 Stack-based Buffer Overflow](https://cwe.mitre.org/data/definitions/121.html)
* [CWE-122 Heap-based Buffer Overflow](https://cwe.mitre.org/data/definitions/122.html)
* [CWE-124 Buffer Underwrite ('Buffer Underflow')](https://cwe.mitre.org/data/definitions/124.html)
* [CWE-125 Out-of-bounds Read](https://cwe.mitre.org/data/definitions/125.html)
* [CWE-126 Buffer Over-read](https://cwe.mitre.org/data/definitions/126.html)
* [CWE-190 Integer Overflow or Wraparound](https://cwe.mitre.org/data/definitions/190.html)
* [CWE-191 Integer Underflow (Wrap or Wraparound)](https://cwe.mitre.org/data/definitions/191.html)
* [CWE-196 Unsigned to Signed Conversion Error](https://cwe.mitre.org/data/definitions/196.html)
* [CWE-367 Time-of-check Time-of-use (TOCTOU) Race Condition](https://cwe.mitre.org/data/definitions/367.html)
* [CWE-415 Double Free](https://cwe.mitre.org/data/definitions/415.html)
* [CWE-416 Use After Free](https://cwe.mitre.org/data/definitions/416.html)
* [CWE-457 Use of Uninitialized Variable](https://cwe.mitre.org/data/definitions/457.html)
* [CWE-459 Incomplete Cleanup](https://cwe.mitre.org/data/definitions/459.html)
* [CWE-467 Use of sizeof() on a Pointer Type](https://cwe.mitre.org/data/definitions/467.html)
* [CWE-787 Out-of-bounds Write](https://cwe.mitre.org/data/definitions/787.html)
* [CWE-788 Access of Memory Location After End of Buffer](https://cwe.mitre.org/data/definitions/788.html)
* [CWE-824 Access of Uninitialized Pointer](https://cwe.mitre.org/data/definitions/824.html)



## X03:2025 Inappropriate Trust in AI Generated Code ('Vibe Coding')

### 배경.

Currently the entire world is talking about and using AI, and this includes software developers. Although there are currently no CVEs or CWEs related to AI generated code, it is well known and documented that AI generated code often contains more vulnerabilities than code written by human beings.


### 설명.

We are seeing software development practices change to include not only code written with the assistance of AI, but code written and committed almost entirely without human oversight (often referred to as vibe coding). Just as it was never a good idea to copy code snippets from blogs or websites without thinking twice, the problem is exacerbated in this case. Good, secure code snippets were and are rare and might be statistically neglected by AI due to system constraints.


### 대응 방안.
We urge all people who write code to consider the following when using AI:

* You should be able to read and fully understand all code you submit, even if it is written by an AI or copied from an online forum. You are responsible for all code that you commit.
* You should review all AI-assisted code thoroughly for vulnerabilities, ideally with your own eyes and also with security tooling made for this purpose (such as static analysis). Consider using classic code review techniques as described in [OWASP Cheat Sheet Series: Secure Code Review](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html).
* Ideally, write your own code, let the AI suggest improvements, check the AI's code, and let the AI make corrections until you are satisfied with the result.
* Consider using a Retrieval Augmented Generation (RAG) server with your own collected  and reviewed secure code samples and documentation, such as your organization’s security coding guideline, standard, or policy, and have the RAG server enforce any policies or standards.
* Consider purchasing tooling that implements guardrails for privacy and security for use with your AI(s) of choice.
* Consider purchasing a private AI, ideally with a contract agreement (including a privacy agreement) that the AI is not to be trained on your organization’s data, queries, code or any other sensitive information.
* Consider implementing an Model Context Protocol (MCP) server in-between your IDE and AI, then set it up to enforce the use of your security tooling of choice.
* Implement policies and processes as part of your SDLC to inform developers (and all employees) of how they should and should not use AI within your organization.
* Create a list of good and effective prompts, that take IT security best practices into account. Ideally they should also consider your internal secure coding guidelines. Developers can use this prompts as a starting point for their programs.
* AI is likely to become part of each phase of your system development life cycle, both how to use it effectively and safely. Use it wisely.
* Actually it is **<u>not</u>** recommended to use vibe coding for complex functions, business critical programs, or programs that are used for a long time.
* Implement technical checks and safeguards against the use of Shadow AI.
* Train your developers on your policies, as well as safe AI usage and best practices for using AI in software development.


### 참조.

* [OWASP Cheat Sheet: Secure Code Review](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html)


###  해당되는 CWE 목록.
-none-
