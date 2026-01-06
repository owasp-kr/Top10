# A03:2025 소프트웨어 공급망 실패![icon](../assets/TOP_10_Icons_Final_Vulnerable_Outdated_Components.png){: style="height:80px;width:80px" align="right"}


## 배경.

소프트웨어 공급망 실패는 TOP 10 커뮤니티 조사에서 정확히 50%의 응답자가 1위로 선정하였다. 2013년 TOP 10에 "A9 - 사용 중인 컴포넌트 내 알려진 취약점" 으로 처음 등장한 이후, 해당 카테고리는 "알려진 취약점" 외에도 "모든 공급망 실패"를 포함하도록 범위가 확장되었다. 범위가 확대됨에도 불구하고, 공급망 실패는 여전히 식별이 어려우며, 관련 CWE와 매핑된 CVE가 11개에 불과하다. 그러나, 수집한 데이터를 테스트하고 보고한 결과에 따르면 이번 카테고리는 5.19%라는 높은 평균 사고 발생률을 보였고, 관련된 CWE로는 *CWE-477: 더 이상 사용되지 않는 기능 사용*, *CWE-1104: 관리되지 않은 외부 컴포넌트 사용*, *CWE-1329: 업데이트할 수 없는 컴포넌트에 대한 의존*, 그리고 *CWE-1395: 취약한 외부 컴포넌트 의존* 이 있다.

## 점수표.


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
   <td>가중 평균 악용도
   </td>
   <td>가중 평균 영향도
   </td>
   <td>총 발생 건수
   </td>
   <td>총 CVE 건수
   </td>
  </tr>
  <tr>
   <td>6
   </td>
   <td>9.56%
   </td>
   <td>5.72%
   </td>
   <td>65.42%
   </td>
   <td>27.47%
   </td>
   <td>8.17
   </td>
   <td>5.23
   </td>
   <td>215,248
   </td>
   <td>11
   </td>
  </tr>
</table>



## 설명.

소프트웨어 공급망 실패는 소프트웨어 개발, 배포, 업데이트하는 과정에서 발생하는 장애 또는 침해를 의미한다. 이는 주로 외부 코드, 도구, 시스템이 신뢰하는 의존성들에서의 악의적인 변조나 취약점이 원인이 된다.

다음과 같은 경우 취약할 가능성이 있다.

* 사용하는 모든 컴포넌트 버전을 추적하지 않는 경우.(클라이언트, 서버 사이드 모두 포함) 여기서의 컴포넌트는 직접 사용하는 컴포넌트 뿐만 아니라 중첩되거나 전이 의존성(Transitive Dependencies)도 포함한다.
* 소프트웨어가 취약하거나, 더 이상 지원하지 않거나, 오래된 버전인 경우. 이는 OS, 웹/애플리케이션 서버, 데이터베이스 관리 시스템(DBMS), 애플리케이션, API, 모든 컴포넌트, 런타임 환경 그리고 모든 라이브러리를 포함한다.
* 주기적으로 취약점을 점검하지 않거나 사용 중인 컴포넌트에 대한 보안 공지를 확인하지 않는 경우.
* 변경 관리 프로세스나 공급망 내 변경 추적 기능이 없는 경우. 이는 IDE 추적, IDE 확장 기능 또는 업데이트, 조직 내 코드 레포지터리, 샌드박스, 이미지와 라이브러리 레포지터리, 생성되거나 저장된 아티팩트 등을 포함한다. 모든 공급망에 대한 영역은 문서화되어야 하며, 특히 변경에 관한 사항은 문서화에 꼭 포함하여야 한다.
* 공급망에 대한 모든 영역에 보안 하드닝이 없는 경우. 특히 접근 제어, 애플리케이션 최소 권한에 관한 사항.
* 공급망 시스템에 직무 분리 사항이 없는 경우. 다른 사람의 감독 없이 공급망 내 코드를 작성하거나 운영 환경에 배포되는 전 과정을 단독으로 수행해선 안 된다.
* 신뢰할 수 없는 출처의 컴포넌트나 기술 스택이 운영 환경에 영향을 미치는 경우.
* 기반 플랫폼, 프레임워크, 의존성을 위험 기반 또는 적시에 고치거나 업그레이드하지 않는 경우. 이는 변경 관리를 월, 분기 단위로 작업할 때 흔히 발생하며, 취약점 조치 전 조직에 불필요한 위험을 수일 또는 수개월 간 노출시킬 수 있다.
* 소프트웨어 개발자가 업데이트, 업그레이드, 패치된 라이브러리의 호환성을 테스트하지 않는 경우.
* 시스템 내 모든 영역에 보안 설정이 미비하거나 없는 경우. (참고. [A02:2025-보안 설정 오류](https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/))
* CI/CD 파이프라인이 빌드, 배포 대상 시스템보다 보안성이 약한 경우. 특히 파이프라인이 복잡할수록 취약해질 수 있다.

## 대응 방안.

패치 관리 프로세스 내 다음과 같은 사항이 포함되어야 한다.



* 전체 소프트웨어에 대한 소프트웨어 구성 목록(SBOM)을 중앙 생성 및 관리한다.
* 직접적 의존성, 전이 의존성(Transitive Dependency) 등을 추적한다.
* 불필요 의존성, 기능, 컴포넌트, 파일 그리고 문서를 지움으로써 공격 가능 범위를 축소한다.
* OWASP Dependency Track, OWASP Dependency Check, retire.js 등과 같은 도구를 사용함으로써 클라이언트, 서버 사이드 컴포넌트(예: 프레임워크, 라이브러리) 버전 지속 목록화 한다.
* 사용 중인 컴포넌트에 대한 취약점들을 CVE(Common Vulnerability and Exposures), NVD(National Vulnerability Database), OSV(Open Source Vulnerabilities) 를 통해 지속 모니터링한다. 이를 위해 소프트웨어 구성 분석, 소프트웨어 공급망, 프로세스 자동화를 위한 소프트웨어 구성 목록(SBOM) 도구 사용 또는 사용 중인 컴포넌트에 대한 보안 취약점 공지 구독을 통해서도 관리할 수 있다.
* 보안 링크를 통해 공식 소스로부터 컴포넌트 다운로드를 하여야 하며, 조작되거나 악성 컴포넌트로 변경되는 경우를 방지하기 위해 서명된 패키지 사용과 체크를 권고한다. (참고. [A08:2025-소프트웨어, 데이터 무결성 실패](https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/))
* 사용할 의존성 버전 선택이 가능하고 필요할 때 업그레이드 가능해야 한다.
* 관리되지 않거나 보안 패치가 이루어지지 않는 오래된 버전의 라이브러리, 컴포넌트 사용 여부를 모니터링한다. 만약 패치가 가능하지 않다면 대안을 선택해야 하는 것을 고려해야 하며, 이 또한 가능하지 않다면 가상 패치를 적용해 발견된 문제에 대해 모니터링, 탐지, 방어하는 방안을 고려한다.
* CI/CD, IDE, 기타 다른 개발 도구를 주기적으로 업데이트한다.
* 모든 시스템은 동시 업데이트가 되지 않아야 한다. 신뢰된 공급업체가 침해된 경우를 대비하여 노출을 제한하기 위해 점진적 배포나 카나리 배포를 한다.

다음과 같은 변경 사항을 추적하기 위해 변경 관리 프로세스 또는 추적 시스템을 마련한다.

* CI/CD 설정 (모든 빌드 도구와 파이프라인)
* 코드 레포지터리
* 샌드박스 영역
* 개발자 IDE
* 소프트웨어 구성 목록 도구(SBOM tooling)와 생성된 아티팩트
* 로깅 시스템과 로그
* 소프트웨어 서비스(SaaS) 같은 통합된 외부 서비스
* 아티팩트 레포지터리
* 컨테이너 레지스트리


다음과 같은 시스템의 경우 보안 하드닝을 해야하며, 이는 다중 인증(MFA)과 IAM 관리를 포함한다.

* 코드 레포지터리: 백업, 보호된 브런치, 민감 데이터 커밋 방지
* 개발자 워크스테이션: 주기적 패치, 다중 인증(MFA), 모니터링 등
* 빌드 서버와 CI/CD 서버: 직무 분리, 접근 제어, 서명된 빌드, 환경변수 내 민감 데이터, 로그 조작 방지 등
* 아티팩트: 출처(Provenance), 서명, 타임스탬프를 통해 무결성을 보장한다. 환경마다 재빌드하지 않고 동일한 아티팩트를 배포하며, 빌드의 불변성을 검증한다.
* 코드형 인프라(Infrastructure as Code): 모든 코드 관리, PR 사용과 버전 관리 포함

모든 조직은 애플리케이션 또는 포트폴리오의 전체 생애주기 동안 업데이트나 설정 변경 사항을 모니터링하고, 분류(Triage)하며, 적용하기 위한 지속적인 계획을 수립, 유지해야 한다.

## 공격 시나리오 예시.

**시나리오 1:** 신뢰된 공급업체가 멀웨어에 감염되어, 해당 소프트웨어를 업그레이드하는 과정에서 사용자 시스템까지 침해되는 사례이다. 가장 유명한 예시는 다음과 같다.

* 2019년 솔라윈즈(Solarwinds) 침해 사고로 약 18,000개의 조직이 침해되었다.[https://www.npr.org/2021/04/16/985439655/a-worst-nightmare-cyberattack-the-untold-story-of-the-solarwinds-hack](https://www.npr.org/2021/04/16/985439655/a-worst-nightmare-cyberattack-the-untold-story-of-the-solarwinds-hack)

**시나리오 2:** 신뢰된 공급업체가 침해되어 특정 조건에서만 악성 행위가 동작하는 사례다.

* 2025년에 바이빗(Bybit)은 [월렛 소프트웨어에서의 공급망 공격](https://www.sygnia.co/blog/sygnia-investigation-bybit-hack/)으로15억 달러 가량을 탈취당했다. 해당 악성 코드는 대상 월렛 사용될 때만 실행되었다.

**시나리오 3:** 2025년 발생한 [`Shai-Hulud` 공급망 공격](https://www.cisa.gov/news-events/alerts/2025/09/23/widespread-supply-chain-compromise-impacting-npm-ecosystem)은 최초로 성공한 자기 전파형 npm 웜이다. 공격자는 인기 패키지에 악성코드를 주입해 새 버전으로 배포했으며, post-install 스크립트를 통해 민감 정보를 수집하고 퍼블릭 GitHub 레포지터리로 유출했다. 해당 멀웨어는 피해자 환경에서 npm 토큰을 탐지하면 이를 이용해 접근 가능한 모든 패키지에 악성 버전을 자동으로 푸시했다. 이 웜은 npm에 의해 차단되기 전까지 500개 이상의 패키지 버전에 영향을 미쳤다. 이번 공격은 고도화되고 빠르게 전파되어 심각한 피해를 입혔으며, 개발자 환경을 직접 표적으로 삼아 개발자 자신이 공급망 공격의 주요 대상이 될 수 있음을 보여주었다.

**시나리오 4:** 컴포넌트는 일반적으로 애플리케이션과 동일한 권한으로 실행되므로, 컴포넌트의 취약점이 있으면 심각한 영향을 미칠 수 있다. 이러한 취약점은 우발적(예: 코딩 에러)이거나, 의도적(예: 컴포넌트 내 백도어)일 수 있다. 악용 가능한 컴포넌트 취약점 대표적 사례는 다음과 같다. 

* CVE-2017-5638: Struts 2의 원격 코드 실행 취약점으로, 서버에서 임의 코드 실행 가능하여 다수의 심각한 침해 사고의 원인이 되었다.
* [CVE-2021-44228](https://github.com/advisories/GHSA-jfh8-c2jp-5v3q "CVE-2021-44228")(Log4Shell): Apache Log4j의 제로데이 원격 코드 실행 취약점으로, 랜섬웨어, 암호화폐 채굴 등 다양한 공격 캠페인에 악용되었다.


## 참조.

* [OWASP Application Security Verification Standard: V15 Secure Coding and Architecture](https://owasp.org/www-project-application-security-verification-standard/)
* [OWASP Cheat Sheet Series: Dependency Graph SBOM](https://cheatsheetseries.owasp.org/cheatsheets/Dependency_Graph_SBOM_Cheat_Sheet.html)
* [OWASP Cheat Sheet Series: Vulnerable Dependency Management](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html)
* [OWASP Dependency-Track](https://owasp.org/www-project-dependency-track/)
* [OWASP CycloneDX](https://owasp.org/www-project-cyclonedx/)
* [OWASP Application Security Verification Standard: V1 Architecture, design and threat modelling](https://owasp-aasvs.readthedocs.io/en/latest/v1.html)
* [OWASP Dependency Check (for Java and .NET libraries)](https://owasp.org/www-project-dependency-check/)
* OWASP Testing Guide - Map Application Architecture (OTG-INFO-010)
* [OWASP Virtual Patching Best Practices](https://owasp.org/www-community/Virtual_Patching_Best_Practices)
* [The Unfortunate Reality of Insecure Libraries](https://www.scribd.com/document/105692739/JeffWilliamsPreso-Sm)
* [MITRE Common Vulnerabilities and Exposures (CVE) search](https://www.cve.org)
* [National Vulnerability Database (NVD)](https://nvd.nist.gov)
* [Retire.js for detecting known vulnerable JavaScript libraries](https://retirejs.github.io/retire.js/)
* [GitHub Advisory Database](https://github.com/advisories)
* Ruby Libraries Security Advisory Database and Tools
* [SAFECode Software Integrity Controls (PDF)](https://safecode.org/publication/SAFECode_Software_Integrity_Controls0610.pdf)
* [Glassworm supply chain attack](https://thehackernews.com/2025/10/self-spreading-glassworm-infects-vs.html)
* [PhantomRaven supply chain attack campaign](https://thehackernews.com/2025/10/phantomraven-malware-found-in-126-npm.html)


## 해당되는 CWE.

* [CWE-447 Use of Obsolete Function](https://cwe.mitre.org/data/definitions/447.html)

* [CWE-1035 2017 Top 10 A9: Using Components with Known Vulnerabilities](https://cwe.mitre.org/data/definitions/1035.html)

* [CWE-1104 Use of Unmaintained Third Party Components](https://cwe.mitre.org/data/definitions/1104.html)

* [CWE-1329 Reliance on Component That is Not Updateable](https://cwe.mitre.org/data/definitions/1329.html)

* [CWE-1357 Reliance on Insufficiently Trustworthy Component](https://cwe.mitre.org/data/definitions/1357.html)

* [CWE-1395 Dependency on Vulnerable Third-Party Component](https://cwe.mitre.org/data/definitions/1395.html)
