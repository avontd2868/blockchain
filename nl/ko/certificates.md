---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-31"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# {{site.data.keyword.blockchainfull_notm}} Platform에서 인증서 관리
{: #certificates}


***[이 페이지가 도움이 되었습니까? 알려주십시오.](https://www.surveygizmo.com/s3/4501493/IBM-Blockchain-Documentation)***


{{site.data.keyword.blockchainfull}} Platform은 Hyperledger Fabric을 기반으로 하고 권한이 부여된 Blockchain 네트워크를 빌드합니다. 참여자는 네트워크 구성원으로 알려져 있으며 해당 활동과 네트워크 리소스에 대한 액세스 권한이 계속 검증됩니다. 참여자의 ID는 신뢰할 수 있는 x509 디지털 인증서의 양식으로 제공됩니다. 검증은 네트워크 내에서의 트랜잭션과 관리에 대해 보안을 제공하는 서명/검증 오퍼레이션과 기본 공개 키 인프라를 통해 제공됩니다.{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform에서는 사용자가 인증서를 처리할 필요 없이 대부분의 인증 오퍼레이션을 관리합니다. 그러나 애플리케이션을 개발하거나 원격 피어와 네트워크를 확장하는 경우와 같이 네트워크와 통신할 수 있도록 인증서를 관리해야 하는 경우도 있습니다. 다음은 인증 기관에 대한 간단한 안내서와 기본 인증 인프라입니다. 이 튜토리얼을 사용하면 작업할 인증서와 수행해야 하는 태스크를 파악할 수 있습니다.

## 인증 기관
{: #network-ca}

인증 기관(CA)에서 네트워크의 ID를 제공합니다. CA는 여러 당사자 간에 신뢰 앵커 역할을 하며 공개적으로 신뢰할 수 있는 공증인으로 간주할 수 있습니다. 디지털 ID를 캡슐화하는 루트 CA에서 서명한 인증서를 네트워크의 모든 엔티티에 제공합니다. 이 인증서는 네트워크에서 수행되는 확인 오퍼레이션과 모든 서명의 신뢰 루트입니다. 인증 기관을 사용하여 ID를 설정하는 방법에 관한 자세한 정보는 [Hyperledger Fabric 문서 ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://hyperledger-fabric.readthedocs.io/en/latest/identity/identity.html)를 참조하십시오.

네트워크의 각 구성원은 자체 CA를 보유하고 있습니다. 이 CA에서 네트워크에 있는 루트 CA의 인증서를 발행합니다. 이와 같이 루트 CA와 연결하면 조직의 신뢰 앵커가 확립됩니다. 조직 CA에서 관리자, 피어 또는 애플리케이션과 같이 조직이 보유한 모든 엔티티에 대한 요청에 서명합니다. 네트워크에 원격 피어 또는 새 애플리케이션을 추가하려면 인증 기관에 새 ID를 등록해야 합니다(등록). 그러면 CA에서 네트워크와 상호작용하는 데 필요한 인증서를 새 엔티티에 제공할 수 있습니다(등록).

### 네트워크 모니터를 사용하여 등록
{: #ca-panel}

{{site.data.keyword.blockchainfull_notm}} Platform Network Monitor를 사용하여 CA에 등록할 ID를 보고 새 ID를 추가할 수 있습니다. 네트워크 모니터의 "인증 기관" 패널로 이동하십시오. 이 패널에는 CA에 등록된 조직 관리자, 피어 및 클라이언트 애플리케이션 등의 모든 ID가 표시됩니다. 조직에 새 ID를 등록하려면 패널 맨 위에서 **사용자 추가** 단추를 클릭하십시오. 팝업 창이 열리고 새 ID를 등록하는 데 필요한 다음 필드가 표시됩니다.
  - **ID:** 새 ID의 이름이며, 경우에 따라 `enroll ID`라고도 합니다. 원격 피어를 구성하거나 새 애플리케이션을 등록하는 경우를 위해 **이 값을 저장**하십시오.
  - **시크릿:** ID의 비밀번호이며, 경우에 따라 `enroll Secret`이라고도 합니다. 원격 피어를 구성하고 새 애플리케이션을 등록하는 경우를 위해 **이 값을 저장**하십시오.  
  - **유형:** 등록할 ID 유형(피어 또는 클라이언트 애플리케이션)을 선택하십시오.
  - **소속:** ID가 속할 조직 내 소속(예: `org1`)을 나타냅니다.
  - **최대 등록 수:** 이 ID를 사용하여 인증서를 등록하거나 생성할 수 있는 횟수를 제한하는 데 이 필드를 사용할 수 있습니다. 필드를 공백으로 두면 기본적으로 무제한 등록 수로 값이 지정됩니다.

[원격 피어](howto/remote_peer.html)를 배치하는 경우 이 패널을 사용하여 새 피어 ID를 등록할 수 있습니다. 또는 네트워크에 트랜잭션을 제출할 수 있는 애플리케이션을 개발하는 경우 클라이언트를 등록할 수 있습니다. [애플리케이션 개발 튜토리얼](v10_application.html)을 방문하여 플랫폼과 Fabric SDK를 사용하는 방법에 관해 알아보십시오.

### 클라이언트 측 인증서 생성(등록)
{: #enrollment}
서드파티 클라이언트를 {{site.data.keyword.blockchainfull_notm}} Platform에 연결하기 전에 먼저 사용자를 인증해야 합니다. 필수 인증서, 개인 키 및 공용 인증서(등록 인증서 또는 signCert라고도 함)를 생성하는 프로세스를 등록이라고 합니다. 해당 인증서는 클라이언트에서 네트워크와 통신할 때마다 필요합니다. 네트워크에 호출을 제출하는 모든 클라이언트에서는 개인 키를 사용하여 페이로드에 서명하고 적절하게 서명된 x509 인증서를 첨부해야 합니다.

[애플리케이션 개발 튜토리얼](v10_application.html)을 방문하여 [Fabric Node SDK를 사용하여 등록](v10_application.html)하는 방법에 관해 알아보십시오. SDK에 등록하면 signCert, signCert를 생성하는 데 사용한 공개 키 및 개인 키가 생성됩니다.

Fabric CA Client를 사용하여 [명령행](#enroll-register-caclient)에서도 인증서를 생성할 수 있습니다. Fabric CA Client에서는 MSP(Membership Service Provider) 폴더에 더 완전한 인증서 세트를 리턴합니다. 이 폴더에는 CA에서 서명한 루트 인증서, 소속에 연결된 중간 인증서, 개인 키 및 signCert가 포함되어 있습니다. MSP와 이 폴더에 포함된 내용에 관한 자세한 정보는 아래 [Membership Service Providers](#msp) 섹션을 참조하십시오.

### {{site.data.keyword.blockchainfull_notm}} Platform에 서명 인증서 업로드
{: #upload-certs}

애플리케이션에는 네트워크에 트랜잭션을 제출하기 위한 올바른 signCert만 필요합니다. 그러나 피어에 체인코드를 설치하거나 채널에 피어를 가입하는 식으로 클라이언트에서 네트워크를 운영하려는 경우 클라이언트가 관리자로 인식되어야 합니다. 각 컴포넌트는 관리자가 소유하는 signCerts 인증서 세트를 인식합니다. 클라이언트에서 네트워크를 운영해야 하는 경우 signCert를 업로드하여 관리 인증서 목록에 추가해야 합니다. 네트워크 모니터 ["개요" 패널](v10_dashboard.html#members)의 **인증서** 탭에서 signCert를 업로드하여 플랫폼에서 이 작업을 수행할 수 있습니다. 업로드 후 표시되는 다시 시작 단추를 누르면 이 인증서가 피어와 동기화됩니다. 그러면 클라이언트를 네트워크 관리자로 인식할 수 있습니다.

<!----
When you add an organization to a channel, this will upload the organization signCert to the channel MSP. Synching the Sign Certs does something.
--->

## TLS 인증서 사용
{: #tls}

[전송 계층 보안 ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm)(TLS)은 Hyperledger Fabric의 신뢰 모델에 임베드됩니다. {{site.data.keyword.blockchainfull_notm}} Platform의 모든 컴포넌트에서는 TLS를 사용하여 서로 인증하고 통신합니다. 따라서 통신의 유효성을 검증하고 암호화하려면 플랫폼에서 발행한 TLS 인증서를 호출에 첨부해야 합니다. 이 튜토리얼에서 설명한 기타 인증서를 통해 네트워크 관리 및 트랜잭션 기능을 보호합니다. TLS 인증서는 네트워크에 대한 호출을 보호하는 데 사용합니다.

TLS 인증서는 플랫폼에서 공개적으로 발행하며 모든 네트워크 컴포넌트에 대해 동일합니다. 멤버십 플랜 및 클라우드 위치에 따라 다음 링크에서 TLS 인증서를 다운로드할 수 있습니다. [인증 정보 프로파일](v10_dashboard.html#enterprise-connection-profile "인증 정보 프로파일")에서도 TLS 인증서를 찾을 수 있습니다. 애플리케이션이나 명령행에서 참조할 수 있는 위치라면 어디든 이 인증서를 둘 수 있습니다.

- 스타터 플랜의 루트 TLS 인증서  
  - 미국: [us01.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/us01.blockchain.ibm.com.cert "us01.blockchain.ibm.com.cert"), [us02.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/us02.blockchain.ibm.com.cert "us02.blockchain.ibm.com.cert")
  - 영국: [uk01.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/uk01.blockchain.ibm.com.cert "uk01.blockchain.ibm.com.cert"), [uk02.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/uk02.blockchain.ibm.com.cert "uk02.blockchain.ibm.com.cert")
  - 시드니: [aus01.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/aus01.blockchain.ibm.com.cert "aus01.blockchain.ibm.com.cert"), [aus02.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/aus02.blockchain.ibm.com.cert "aus02.blockchain.ibm.com.cert")
- [엔터프라이즈 플랜에 대한 루트 TLS 인증서 ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://blockchain-certs.mybluemix.net/3.secure.blockchain.ibm.com.rootcert)

모든 {{site.data.keyword.blockchainfull_notm}} Platform 네트워크에서는 서버 측 TLS를 사용합니다. 해당 TLS는 네트워크가 클라이언트를 인증하는 데 필요합니다. 엔터프라이즈 플랜 네트워크에서는 애플리케이션 보안을 강화하기 위해 클라이언트와 서버에서 서로를 인증하는 상호 TLS를 사용합니다. 클라이언트측 TLS 인증서(상호 TLS용)는 클라이언트 CA에서 발행하고 네트워크마다 고유합니다. 엔터프라이즈 플랜 네트워크를 사용하는 경우 상호 TLS를 사용하는 것이 좋습니다. 상호 TLS에 관한 자세한 정보는 [지시사항](v10_dashboard.html#mutual-tls "상호 TLS 지시사항")을 참조하십시오.

## MSP(Membership Service Provider)
{: #msp}

{{site.data.keyword.blockchainfull_notm}} Platform의 컴포넌트에서는 MSP(Membership Services Provider)를 통해 ID를 이용합니다. MSP는 CA에서 발행하는 인증서를 네트워크 및 채널 역할과 연관시킵니다. MSP에 관한 자세한 정보는 [Hyperledger Fabric 문서의 멤버십 개념 주제 ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://hyperledger-fabric.readthedocs.io/en/latest/membership/membership.html "Hyperledger Fabric 문서의 멤버십 개념 주제")를 참조하십시오.

Fabric의 MSP 폴더에는 구조가 정의되어 있습니다. Fabric CA Client를 사용하여 등록하는 경우 클라이언트에서는 다음 하위 폴더를 사용하여 로컬 파일 시스템의 MSP 폴더에 인증서를 저장합니다.

- **cacerts:** 이 폴더에는 네트워크 루트 CA의 루트 인증서가 포함되어 있습니다.
- **intermediatecerts:** 네트워크 중간 CA의 인증서입니다. 이 중간 CA는 루트 CA에 연결되어 있으며 신뢰 체인을 형성합니다. 각 엔터프라이즈 플랜 조직에는 장애 복구 및 고가용성을 제공하는 두 개의 중간 CA가 있습니다.
- **signcerts:** 이 폴더에는 signCert 또는 등록 인증서라고도 하는 공개 서명 인증서가 들어 있습니다. 이 인증서는 명령행에서 MSP 디렉토리를 참조하거나 SDK에 사용자 컨텍스트 오브젝트를 빌드할 때 네트워크 호출에 첨부됩니다(예: 체인코드 호출). SDK 또는 명령행에서 네트워크를 운영하려는 경우 이 인증서를 플랫폼에 업로드할 수 있습니다.
- **keystore:** 이 폴더에는 개인 키가 포함되어 있습니다. 이 키는 명령행에서 MSP 디렉토리를 참조하거나 SDK에 사용자 컨텍스트 오브젝트를 빌드할 때 네트워크 호출에 서명하는 데 사용됩니다. 네트워크와 데이터를 보호하려면 이 키를 안전하게 보관하십시오.

네트워크 모니터와 Swagger API를 사용하여 Fabric CA Client에서 참조할 수 있는 MSP 폴더도 빌드할 수 있습니다.

- **cacerts** 및 **intermediatecerts**: MSP API에 Get 요청을 발행하여 [Swagger API](howto/swagger_apis.html)로 해당 인증서를 페치할 수 있습니다.
- **signcerts** 및 **keystore**: "인증 기관" 패널에서 **인증서 생성** 단추를 클릭하여 해당 인증서를 생성할 수 있습니다. 두 개의 인증서가 나열된 팝업 창이 열립니다. **인증서**와 **개인 키**를 복사하여 각각 signcert와 키 저장소에 저장하십시오. 해당 인증서는 플랫폼에 저장되지 않으므로 안전한 위치에 보관하십시오.

많은 Fabric 컴포넌트에는 MSP 폴더에 추가 정보가 포함되어 있습니다. 예를 들어 원격 피어를 운영하는 경우 다음 폴더가 표시될 수 있습니다.

- **admincerts:** 이 폴더에는 이 조직이나 컴포넌트의 관리자 목록이 들어 있습니다. 명령행 또는 SDK에서 원격 피어를 운영하는 경우 서명 인증서를 이 폴더에 업로드해야 합니다.
- **tls:** 다른 네트워크 컴포넌트와 통신하는 데 사용하는 TLS 인증서를 저장하는 폴더입니다.

MSP의 구조에 관한 자세한 정보는 Hyperledger Fabric 문서에서 [멤버십 ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://hyperledger-fabric.readthedocs.io/en/latest/membership/membership.html "멤버십") 및 [Membership Service Providers ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://hyperledger-fabric.readthedocs.io/en/latest/msp.html "Membership Service Providers")를 참조하십시오.

## Fabric CA Client를 사용하여 등록
{: #enroll-register-caclient}

또한 Fabric CA Client를 사용하여 인증서를 생성하고 인증 기관에 새 ID를 등록할 수 있습니다. 아래 지시사항에서는 관리자 ID를 사용하여 인증서를 생성한 다음 해당 인증서를 사용하여 새 클라이언트를 등록합니다. Fabric CA Client를 사용하여 등록하면 [Membership Services Providers 섹션](#msp)에 설명된 인증서가 생성됩니다.

### Fabric CA Client를 사용하여 등록
{: #enroll-app-caclient}

1. [fabric-ca 2진 파일](https://nexus.hyperledger.org/content/repositories/releases/org/hyperledger/fabric-ca/hyperledger-fabric-ca/)을 로컬 시스템에 다운로드하고 추출한 다음 `$HOME/fabric-ca-platform/`과 같은 폴더로 이동하십시오. 명령에서 직접 참조할 수 있도록 클라이언트 2진 파일을 이동한 디렉토리로 변경하십시오.
    ```
    cd $HOME/fabric-ca-platform/
    ```

2.  클라이언트에서 키를 작성할 수 있는 경로를 설정하십시오. 과거에 작성한 구성 자료를 제거했는지 확인하십시오. `enroll` 명령을 처음 실행하는 경우 `msp` 폴더와 `.yaml` 파일이 없습니다.
    ```
    export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-platform
    rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
    rm -rf $FABRIC_CA_CLIENT_HOME/msp
    ```

3. 사용하는 서비스 플랜, 위치 및 클러스터에 따라 {{site.data.keyword.cloud_notm}}에서 TLS 인증서를 다운로드하십시오.
  - 스타터 플랜의 루트 TLS 인증서  
    - 미국: [us01.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/us01.blockchain.ibm.com.cert "us01.blockchain.ibm.com.cert"), [us02.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/us02.blockchain.ibm.com.cert "us02.blockchain.ibm.com.cert")
    - 영국: [uk01.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/uk01.blockchain.ibm.com.cert "uk01.blockchain.ibm.com.cert"), [uk02.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/uk02.blockchain.ibm.com.cert "uk02.blockchain.ibm.com.cert")
    - 시드니: [aus01.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/aus01.blockchain.ibm.com.cert "aus01.blockchain.ibm.com.cert"), [aus02.blockchain.ibm.com.cert ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](http://blockchain-certs.mybluemix.net/aus02.blockchain.ibm.com.cert "aus02.blockchain.ibm.com.cert")
  - [엔터프라이즈 플랜에 대한 루트 TLS 인증서 ![외부 링크 아이콘](images/external_link.svg "외부 링크 아이콘")](https://blockchain-certs.mybluemix.net/3.secure.blockchain.ibm.com.rootcert)

  컨텐츠를 폴더(예: ``$HOME/tls``)에 저장하십시오. 이 단계는 데이터 플로우가 연결에서 암호화되도록 허용합니다.

4. 네트워크 모니터의 "개요" 화면에서 **연결 프로파일** JSON 파일을 열고 다음 변수를 찾으십시오.
  * CA의 URL: ``certificateAuthorities`` 아래 ``url``
  * 관리자 ID: ``enrollId``
  * 관리 비밀번호: ``enrollSecret``
  * CA 이름: ``caName``

5. 다음 명령으로 TLS 인증 경로와 위의 네 가지 문자열을 전달하여 `enroll` 호출을 인증 기관에 보내는 데 Fabric CA Client를 사용할 수 있습니다.
  ```
  ./fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name> --tls.certfiles <tls_cert_path>
  ```
  {:codeblock}

  Fabric CA Client가 이동될 디렉토리에서 이 명령을 실행해야 합니다. 실제 호출은 다음 예제 명령과 비슷하게 보일 수 있습니다.
  ```
  ./fabric-ca-client enroll -u https://admin:dda0c53f7b@n7413e3b503174a58b112d30f3af55016-org1-ca.us3.blockchain.ibm.com:31011 --caname org1CA --tls.certfiles $HOME/tls/us2.blockchain.ibm.com.cert
  ```
  {:codeblock}

6. `$FABRIC_CA_CLIENT_HOME/msp/signcerts/cert.pem`에서 관리자 인증서를 찾으십시오. 그런 다음 네트워크 모니터에서 사용자의 블록체인 네트워크에 관리자 인증서를 업로드할 수 있습니다. 인증서 추가에 대한 자세한 정보는 네트워크 모니터에서 ["멤버" 패널의 "인증서" 탭](v10_dashboard.html#members)을 참조하십시오.

  또한, CA 루트 인증서 및 관리자 개인 키를 다음 디렉토리에서 찾을 수 있습니다.
  * CA 루트 인증서: `$FABRIC_CA_CLIENT_HOME/msp/cacerts/--<ca_name>.pem`
  * 관리자 개인 키: `$FABRIC_CA_CLIENT_HOME/msp/keystore/<>_sk file`

Fabric CA Client를 사용하여 등록할 예제 위치를 보고 생성된 인증서를 사용하여 [원격 피어 운영](howto/remote_peer_operate_icp.html#remote-peer-cli-operate) 지시사항의 네트워크 컴포넌트를 운영하십시오.

### Fabric CA Client를 사용하여 등록
{: #register-app-caclient}

1. 다음 명령을 실행하여 Blockchain 네트워크에서 소속과 조직 이름을 찾으십시오.
  ```
  ./fabric-ca-client affiliation list --tls.certfiles pathToPem
  ```
  {:codeblock}

  다음 예제와 비슷한 정보가 표시되어야 합니다.
  ```
  affiliation: ibp
      affiliation: ibp.PeerOrg1
  ```
  {:codeblock}

  두 번째 **소속(affiliation)** 값(예: `ibp.PeerOrg1`)을 기록해 두십시오. 아래 명령에서 이 값을 사용해야 합니다.

2. 다음 명령을 실행하여 피어를 등록하십시오.
  ```
  ./fabric-ca-client register --id.name <name> --id.affiliation <affiliation> --id.secret <password> --tls.certfiles pathToPem
  ```
  {:codeblock}

  피어의 이름과 비밀번호를 지정하고 `name`과 `password`를 사용자의 이름과 비밀번호로 바꾸십시오. 이 정보를 기록하십시오. 이 정보는 피어를 구성할 때 필요합니다. 예를 들어, 다음과 같습니다.
  ```
  ./fabric-ca-client register --id.name user1 --id.affiliation ibp.PeerOrg1 --id.secret userpw  --tls.certfiles --tls.certfiles $HOME/tls/us2.blockchain.ibm.com.cert
  ```
  {:codeblock}

  ID는 한 번만 등록할 수 있습니다. 문제점이 발생하면 새 사용자 이름과 비밀번호를 사용하여 명령을 시도하십시오.

  명령이 완료되면 다음 예제와 비슷한 정보가 표시되어야 합니다.
  ```
  2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
  2018/06/18 16:53:00 [INFO] TLS Enabled
  2018/06/18 16:53:00 [INFO] TLS Enabled
  Password: userpw
  ```
  {:codeblock}
