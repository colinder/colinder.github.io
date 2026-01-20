# 전자(세금)계산서 발급 api 개발기 part.1


​	

# 국세청 전자세금계산서 발급_part.1

> 국세청의 전자세금계산서 발급 기능을 혼자 개발하면서 확인한 프로세스, 문서 및 개인적인 노하우 등을 정리한다. 
> 사실 국가에서 제공하는 여타 api처럼 매우 간단할 것으로 예상했었다. (하지만 복잡하다..)
>
> 또한 이 기능을 개발하는데 https://github.com/ruseel/kr-etax-sample 안내가 도움이 되었다.

​	

## 전체 프로세스

1. 국세청에 부서사용자 신청하기
2. 부서사용자에서 지원하는 표준인증검증 통과하기
3. 실사기술검증 통과하기
4. 등기로 국세청 실서버에 대한 정보 수신하기
5. 실서비스 운영

​	

국세청 전자세금계산서 발급기능의 개발 및 연동을 위해서는 우선 국세청에 해당 기능을 개발 및 사용하겠다는 신청을 해야 한다. 그 시작이 부서사용자 신청이다. 세금계산서 발급을 지원해주는 경우는 **두 가지**가 있다. 

1. 자체 시스템을 보유한 경우 (A)
2. ASP(Application Service Provider, 전자세금계산서 발급 기능을 제공하는 업체)를 사용하는 경우 (B)

이 둘의 흐름을 간략히 표현하자면 아래와 같다. 

- (A) 의 경우

  {{< mermaid >}}
  flowchart TB
      subgraph INTERNAL["내부 시스템"]
          U[내부 사용자] --> ERP[경영정보시스템 / ERP]
          ERP --> A(세금계산서 발급)
          A --> ERP
      end

  %% 스타일 정의
  style A fill:#ffffff,stroke:#ff0000,stroke-width:3px
  {{< /mermaid >}}

  ​	

- (B) 의 경우

  {{< mermaid >}}
  flowchart LR
      U[사용자]
      U --> ASP[ASP 서비스 접속]
      subgraph INTERNAL["외부 시스템"]
          ASP --> A(세금계산서 발급)
          A --> ASP
  		end
      %% 스타일 정의
      style A fill:#ffffff,stroke:#ff0000,stroke-width:3px
  {{< /mermaid >}}


해당 문서는 이 중 (B), 즉 **ASP**의 개발을 설명한다.

​	

### 1. 국세청에 부서사용자 신청하기

> 국세청 전자세금계산서 발급기능을 개발하겠다고 국세청에게 신고하는 단계

(신청에 따른 행정 절차는 생략한다.) 이때 참고해야 하는 문서는 **표준전자세금계산서개발지침(17년8월).pdf**이다. 작성된지 매우 오래된 문서이고 간간히 잘못기재된 내용, 잘못된 내용이 섞여있어, 참과 거짓을 분석하며 봐야 한다. 국세청에서도 현재 기준(25년 12월) 관리되고 있지 않아, 의문점을 문의해도 해답을 받을 수 없었다. 

하지만 전자세금계산서를 발급하기 위한 유일한 가이드 문서라 이를 무시할 수도 없다. 달달 외울 정도로 봐야 한다. 

​	

### 2. 부서사용자에서 지원하는 표준인증검증 통과하기

> 국세청 전자세금계산서 발급기능의 개발 및 연동을 위해서 **단위 기능 테스트**하는 단계

필자는 python이 주 언어라, fastapi를 써서 개발했으며, xml 파일을 다루어야 하는데 이는 java의 라이브러리들을 활용했다. python 라이브러리에도 "signxml" 등을 써봤으나 여러 문제들이 발생했고, 오랬동안 문제없이 사용되었던 java 라이브러리들을 사용해 개발하는게 믿을 수 있고 속도감있게 개발이 가능했다. ("signxml"을 쓰면 안된다 라기보다, 필자가 깊이있게 써보지 않아서 이게 되는지 안되는지 모르겠다.) 하지만, java 라이브러리를 python에 호환해서 사용하려면 배포시 docker 파일이 복잡해지는 등 어려움이 있지만, 이를 감수하고 그냥 사용했다. 

**여기서부터 헬이었다.** 혼자 해당 기능을 개발해야 하는 상황이어서, 국세청 담당자랑도 통화를 많이하고, 혼자 표준지침 문서를 달달 외울 정로도 봤다.

​	

필작 생각하기에 표준인증검증은 크게 4가지 파트인 것 같다. 

1. 전자세금계산서 제작
2. 전자세금계산서 암호화 
3. 전자세금계산서 패키징
4. 국세청에게 전송 및 응답

​	

또한 발급 프로세스를 도식화하면 아래와 같다. 

{{< mermaid >}}
flowchart TB
    subgraph INTERNAL["ASP"]
        res[응답확인]
        xml[XML 전자세금계산서 제작] --> sign[XML 전자서명]
        sign --> decrp[암호화]
        decrp --> soap[soap 패키징]
        soap --> mime[MIME 패키징]
    end
    subgraph NTS["국세청"]
        mime -->|전송|nts[서버]
    end
    nts --> |응답|res
{{< /mermaid >}}

​		

#### 2-1 XML 전자세금계산서 제작

> **XML**(e**X**tensible **M**arkup **L**anguage(확장 가능한 마크업 언어)) 매우 낯선 언어였다. 필자가 이해한 건 그냥 데이터를 주고 받는 과거의 표준?정도의 느낌이다. 현재 테이터 통신을 위한 구조로 JSON이 사실상 표준인 상황에서 xml을 조금 알아두면 표준지침을 이해하는데 도움이 된다.

먼저 세금계산서를 제작하기 위해서 python의 lxml 라이브러리를 사용했다.	

```python
from lxml import etree

TAX_NS = "urn:kr:or:kec:standard:Tax:ReusableAggregateBusinessInformationEntitySchemaModule:1:0"
XSI_NS = "http://www.w3.org/2001/XMLSchema-instance"

# 일부러 둘의 순서를 뒤틈 > 정규화함수가 정상적으로 동작하는지 확인하기 위함.
NSMAP = {
    "xsi": XSI_NS,
    None: TAX_NS,  # 기본 네임스페이스
}

# root 생성
tax_invoice = etree.Element(
    "TaxInvoice",
    nsmap=NSMAP,
    attrib={
        "{%s}schemaLocation"
        % XSI_NS: f"{TAX_NS} http://www.kec.or.kr/standard/Tax/TaxInvoiceSchemaModule_1.0.xsd",
    },
)
### [element 순서 중요]
## <ExchangedDocument>
exchanged_doc = etree.SubElement(tax_invoice, "ExchangedDocument")
```

이후 정규화를 해야 한다. 필자는 함수를 만들어서 사용했다. 정규화 방식은 표준지침에 명시되어있고 그 방법대로 해야 한다.

​	

```python
from lxml import etree

def canonicalize(xml_str, input_type: str) -> bytes:
    parser = etree.XMLParser()
    root = etree.fromstring(xml_str, parser=parser)
    
    c14n_bytes = etree.tostring(
        root,
        method="c14n",  # Canonical XML
    )
  
    return c14n_bytes
```

​	

#### 2-2 XML 전자서명

> 여기서부터는 java 라이브러리들을 사용한다. 

python의 구조를 먼저 보자면, 

```python
# import 
SignXML = jpype.JClass("com.barostudio.eTaxInvoice.SignXML")

# 03. 서명된 XML 생성
signed_bytes = SignXML.sign(
    bytearray(canonicalized_etax),  # XML bytes
    seller_private_key_java,
    seller_x509_certificate_cert_java,
)
```

여기서. seller_private_key_java와 seller_x509_certificate_cert_java는 **공급한 자**의 인증서 정보를 넣으면 된다. 공급한 자의 인증서 정보를 추출함에 있어 front와 back 두 곳에서 추출해야 했기에 javascript와 python 둘 다 인증서를 열어보는 코드를 만들었다. 인증서에서 pem 키들은 상대적으로 쉽게 꺼낼 수 있는데, **Rvalue**는 생각보다 애먹었다. python은 subprocess를 써서, javascript는 node-forge를 써서 찾을 수 있었다. 

이후 암호화를 하는데 서명하는 것은 java를 사용했다. (사실 이 부분은 단일 언어로도 할 수 있었을 것 같다.)

- SignXML.java

  ```java
  package com.barostudio.eTaxInvoice;
  
  import java.io.ByteArrayInputStream;
  import java.io.ByteArrayOutputStream;
  import java.io.FileInputStream;
  import java.io.FileOutputStream;
  import java.io.InputStream;
  import java.io.OutputStream;
  import java.security.KeyStore;
  import java.security.PrivateKey;
  import java.security.Security;
  import java.security.Signature;
  import java.security.cert.X509Certificate;
  import java.util.Enumeration;
  
  import javax.xml.crypto.dsig.DigestMethod;
  import javax.xml.parsers.DocumentBuilder;
  import javax.xml.parsers.DocumentBuilderFactory;
  
  import org.apache.xml.security.signature.XMLSignature;
  import org.apache.xml.security.transforms.Transforms;
  import org.apache.xml.security.utils.Constants;
  import org.apache.xml.security.utils.XMLUtils;
  import org.apache.xpath.XPathAPI;
  
  import org.bouncycastle.jce.provider.BouncyCastleProvider;
  import org.w3c.dom.Document;
  import org.w3c.dom.Element;
  import org.w3c.dom.Node;
  
  //
  import org.apache.xml.security.c14n.Canonicalizer;
  
  public class SignXML {
  	
  	public static byte[] sign(
  		byte[] unsignedXmlBytes, 
  		PrivateKey privateKey,
  		X509Certificate cert
  	) throws Exception {
          // 메모리 기반 처리
          try (InputStream is = new ByteArrayInputStream(unsignedXmlBytes);
               ByteArrayOutputStream os = new ByteArrayOutputStream()) {
              
              // 서명 수행
              signn(privateKey, cert, is, os);
              return os.toByteArray();
          }
      }
  	
  	public static void signn(
  		PrivateKey privateKey, 
  		X509Certificate cert, 
  		InputStream is, 
  		OutputStream os
  	) throws Exception {
  		org.apache.xml.security.Init.init();
  		
  		// Document
  		// 입력 스트림에서 XML을 읽어 DOM(Document Object Model) 객체로 파싱
  		DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
  		dbf.setNamespaceAware(true);
  		DocumentBuilder db = dbf.newDocumentBuilder();
  		Document doc = db.parse(is); // unsign XML 문서
  		
  		// XMLSignature
  		// SHA256을 사용하는 RSA 서명 객체 생성
  		String BaseURI = "";
  		XMLSignature sig = new XMLSignature(doc, BaseURI,
  											XMLSignature.ALGO_ID_SIGNATURE_RSA_SHA256);
  
  		// W3C Element DOM
  		// XPath로 <TaxInvoiceDocument> 요소를 찾음
  		// 그 앞에 서명(Signature) 엘리먼트를 삽입
  		{
  			Element ctx = doc.createElementNS(null, "namespaceContext");
  			ctx.setAttributeNS(Constants.NamespaceSpecNS, "xmlns:tax", "urn:kr:or:kec:standard:Tax:ReusableAggregateBusinessInformationEntitySchemaModule:1:0");
  			Node pivot = XPathAPI.selectSingleNode(doc, "//tax:TaxInvoiceDocument", ctx);
  			pivot.getParentNode().insertBefore(sig.getElement(), pivot);
  		}
  		
  		// 서명 대상 범위(Transforms) 및 XPath 지정
  		// 서명 대상 XML을 주석 없는 정규화(C14N)로 변환
  		// XPath로 서명에서 제외할 노드(예: Signature, TaxInvoice, ExchangedDocument)를 지정
  		{
  			Transforms transforms = new Transforms(doc); // unsign XML 문서 변환 목록 생성
  			transforms.addTransform(Transforms.TRANSFORM_C14N_OMIT_COMMENTS); // c14n 정규화 실행
  
  			Element xpathElement = doc.createElementNS("http://www.w3.org/2000/09/xmldsig#", "ds:XPath"); // XPath 엘라먼트 생성
  			xpathElement.appendChild(doc.createTextNode("not(self::*[name() = 'TaxInvoice'] | ancestor-or-self::*[name() = 'ExchangedDocument'] | ancestor-or-self::ds:Signature)"));
  
  			transforms.addTransform(Transforms.TRANSFORM_XPATH, xpathElement);
  			sig.addDocument("", transforms, DigestMethod.SHA256);			
  		}
  		
  		// XMLSignature
  		sig.addKeyInfo(cert);
  		sig.sign(privateKey);
  		
  		
  		// // OutputStream (정규화, 주석 없음)
  		Canonicalizer canon = Canonicalizer.getInstance(Canonicalizer.ALGO_ID_C14N_OMIT_COMMENTS);
  		canon.canonicalizeSubtree(doc, os);
  		// OutputStream (정규화, 주석 포함)
  		// XMLUtils.outputDOMc14nWithComments(doc, os);
  		
  		}
  }
  ```

  ​	

​	

​	

## 뒤에 계속

혼자 여기까지 오는데 꽤 오랜 시간이 걸렸다. 다양한 사유가 있지만, 인증서 정보 추출, python과 java를 같이 사용하다보니 타입의 변환에도 신경쓸 것이 많았고, 표준지침에 있는 예시의 구조를 세심히 봐야 하는 점.. 등이 어려웠다. 하지만 개발이 그러하듯 성공하고 보면 별거 아닌 것 같다.

​	

​	

​	




