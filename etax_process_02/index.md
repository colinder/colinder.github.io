# 전자(세금)계산서 발급 api 개발기 part.2


​	

# 국세청 전자세금계산서 발급_part.2

> 앞선 문서에서는 국세청에 부서사용자 신청하고, 부서사용자에서 지원하는 표준인증검증 통과하기까지 필요한 개발사항을 정리했다. 지난번에 이어서 단계별로 정리한다.

​	

#### 2-3 암호화

> 추출한 인증서 정보와 암호화할 세금계산서들을 묶어서 하나의 리스트로 만들고 이를 같이 암호화했다. 

여기서는 국세청 공개키를 가져와 사용해야한다. 

```python
# import 
EncryptWithCMS = jpype.JClass("com.barostudio.eTaxInvoice.EncryptWithCMS")
j_xml_list = jpype.JClass("java.util.ArrayList")()

java_signed_xml = bytes(signed_bytes)
j_xml_list.add(JByteArray(java_signed_xml))  
  
# - encoding하지 않은 binary 상태 (표준지침에 따라)
java_cms_encrypted_bytes = EncryptWithCMS.encrypt(
    bytearray(rvalue_bytes),  # rvalue 바이트
    j_xml_list,  # 서명된 XML 바이트 묶음
    homeTax_cert_path, # 국세청 공개키 위치
)
```

- EncryptWithCMS.java

  ```java
  package com.barostudio.eTaxInvoice;
  
  import java.io.BufferedInputStream;
  import java.io.ByteArrayOutputStream;
  import java.io.FileInputStream;
  import java.io.FileNotFoundException;
  import java.io.FileOutputStream;
  import java.io.InputStream;
  import java.security.Security;
  import java.security.cert.CertificateException;
  import java.security.cert.CertificateFactory;
  import java.security.cert.X509Certificate;
  import java.util.List;
  
  // import org.bouncycastle.asn1.DEROutputStream;
  import org.bouncycastle.asn1.ASN1OutputStream;
  import org.bouncycastle.cms.CMSAlgorithm;
  import org.bouncycastle.cms.CMSEnvelopedData;
  import org.bouncycastle.cms.CMSEnvelopedDataGenerator;
  import org.bouncycastle.cms.CMSProcessableByteArray;
  import org.bouncycastle.cms.CMSTypedData;
  import org.bouncycastle.cms.jcajce.JceCMSContentEncryptorBuilder;
  import org.bouncycastle.cms.jcajce.JceKeyTransRecipientInfoGenerator;
  import org.bouncycastle.jce.provider.BouncyCastleProvider;
  
  import com.barostudio.nts.asn1.TaxInvoiceData;
  import com.barostudio.nts.asn1.TaxInvoicePackage;
  //
  import org.bouncycastle.asn1.kisa.KISAObjectIdentifiers;
  //
  public class EncryptWithCMS {
  	/**
       * 여러 건의 rvalue/xml 페어를 국세청 표준에 맞는 ASN.1 구조체로 합친 뒤
       * CMS(EnvelopedData) 포맷으로 암호화한다.
       *
       * @param rvalueBytesList rvalue 바이트 배열 리스트 (서명 vID 검증값, XML 개수와 1:1)
       * @param xmlBytesList 전자세금계산서(서명 포함) XML 바이트 배열 리스트
       * @param homeTaxCertPath 수신자(국세청) 인증서 파일 경로
       * @return 암호화된 CMS DER 바이너리
       * @throws Exception
       */
  	public static byte[] encrypt(
  		byte[] rvalueBytes, 
  		List<byte[]> xmlBytesList, 
  		String homeTaxCertPath
  	) throws Exception {
  		
  		byte[] _package = getTaxInvoicePackageAsBytes(rvalueBytes, xmlBytesList);
  
  		CMSTypedData msg = new CMSProcessableByteArray(_package);
  
  		CMSEnvelopedDataGenerator edGen = new CMSEnvelopedDataGenerator();
  		// 국세청 공개키 import 
  		edGen.addRecipientInfoGenerator(
  			new JceKeyTransRecipientInfoGenerator(
  				kmCert(homeTaxCertPath)
  			)
  		.setProvider("BC"));
  		
  		// 3DES 암호화
  		CMSEnvelopedData ed = edGen.generate(msg, 
  			new JceCMSContentEncryptorBuilder(CMSAlgorithm.DES_EDE3_CBC)
  			.setProvider("BC").build());
  
  		// 암호화된 CMS(EnvelopedData) 객체를 ASN.1 DER 바이너리로 직렬화
  		byte[] cmsEncryptedBytes = ed.getEncoded(); 
  
  		return cmsEncryptedBytes;
  	}
  
  	public static byte[] getTaxInvoicePackageAsBytes(
  		byte[] rvalueBytes,
  		List<byte[]> xmlBytesList
  	) throws Exception {
  		if (xmlBytesList == null || xmlBytesList.isEmpty()) {
              throw new IllegalArgumentException("xmlBytesList must not be empty.");
          }
  
  		int count = xmlBytesList.size();
  		TaxInvoiceData[] dataArr = new TaxInvoiceData[count];
          for (int i = 0; i < count; i++) {
              dataArr[i] = new TaxInvoiceData(rvalueBytes, xmlBytesList.get(i));
          }
  		TaxInvoicePackage pkg = new TaxInvoicePackage(dataArr);
  
  		ByteArrayOutputStream baos = new ByteArrayOutputStream();
  		// ASN.1 DER 형식의 바이너리로 인코딩 (필수 요소)
  		ASN1OutputStream out = ASN1OutputStream.create(baos);
  		out.writeObject(pkg);
  		out.close();
  
  		return baos.toByteArray();
  	}
  
  	// kmCert
  	private static X509Certificate kmCert(
  		String hometaxCertPath
  	) throws FileNotFoundException, CertificateException {
  		Security.addProvider(new BouncyCastleProvider());
  
  		FileInputStream ksfis = new FileInputStream(hometaxCertPath);
  		BufferedInputStream ksbufin = new BufferedInputStream(ksfis);
  		X509Certificate certificate = (X509Certificate)
  		  CertificateFactory.getInstance("X.509").generateCertificate(ksbufin);
  
  		return certificate;
  	}
  }
  ```

​	

#### 2-4 SOAP 패키징

> SOAP도 이번에 처음 들어봤다. SOAP(Simple Object Access Protocol)은 간단하게 정해진 형식의 XML 편지를 주고받는 통신 규약이다. 데이터 형식: **XML**, 전송 방식: 보통 **HTTP/HTTPS**, 규칙이 매우 엄격 (스키마, 네임스페이스, 타입)한 특징을 가지고 있다.

SOAP도 java를 활용해서 만들었다. 이때는 ASP 사업자의 인증서 정보를 추출해서 같이 넣어주어야 한다. 왜냐하면 SOAP로 패키징하면서 ASP 사업자 정보로 서명해서 이 SOAP로 전달된 문서가 ASP가 보냈다고 알려주는 것이다.

또한 표준지침에 따르면 SOAP 1.1 또는 1.2 버전만 사용가능하다고 하는데 필자는 1.1 버전으로 개발했다. 표준 지침이 1.1 버전 기준으로 기술되어 있어 이를 최대한 따라 진행했다.

```python
# - 동적변수 할당
message_id = generate_message_id()
submit_id = generate_submit_id(
    settings.ASP_ETAX_CERTIFICATION_NUMBER
)  # 국세청 인증번호(asp)
soap_version = "1.1"

# - java lib import
buildWithSOAP = jpype.JClass("com.barostudio.eTaxInvoice.buildWithSOAP")

java_init_soap = buildWithSOAP.sign(
    java_cms_encrypted_bytes,
    asp_private_key_java,
    asp_x509_certificate_java,
    SE_endpoint_url,
    message_id,
    submit_id,
    str(1),
    reply_to,
)
```

- buildWithSOAP.java (일부)

  ```java
  package com.barostudio.eTaxInvoice;
  
  import java.io.BufferedReader;
  import java.io.ByteArrayOutputStream;
  import java.io.File;
  import java.io.FileInputStream;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.io.StringWriter;
  import java.nio.charset.Charset;
  import java.security.KeyStore;
  import java.security.PrivateKey;
  import java.security.Security;
  import java.security.cert.X509Certificate;
  import java.util.Enumeration;
  
  import javax.xml.crypto.dsig.DigestMethod;
  import javax.xml.namespace.QName;
  import javax.xml.soap.MessageFactory;
  import javax.xml.soap.SOAPConstants;
  import javax.xml.soap.SOAPBody;
  import javax.xml.soap.SOAPElement;
  import javax.xml.soap.SOAPEnvelope;
  import javax.xml.soap.SOAPException;
  import javax.xml.soap.SOAPHeader;
  import javax.xml.soap.SOAPHeaderElement;
  import javax.xml.soap.SOAPMessage;
  import javax.xml.soap.SOAPPart;
  import javax.xml.transform.OutputKeys;
  import javax.xml.transform.Result;
  import javax.xml.transform.TransformerConfigurationException;
  import javax.xml.transform.TransformerException;
  import javax.xml.transform.TransformerFactory;
  import javax.xml.transform.TransformerFactoryConfigurationError;
  import javax.xml.transform.dom.DOMSource;
  
  import org.apache.xml.security.keys.KeyInfo;
  import org.apache.xml.security.signature.XMLSignature;
  import org.apache.xml.security.transforms.Transforms;
  // import org.apache.xml.security.utils.Base64;
  import java.util.Base64;
  import org.apache.xml.security.utils.Constants;
  import org.apache.xml.security.utils.XMLUtils;
  import org.apache.xml.security.utils.resolver.ResourceResolver;
  import org.apache.xpath.XPathAPI;
  import org.bouncycastle.jce.provider.BouncyCastleProvider;
  import org.w3c.dom.Document;
  import org.w3c.dom.Element;
  import org.w3c.dom.Node;
  
  import com.barostudio.nts.ext.ResolverOwnerDocumentUserData;
  import com.barostudio.nts.ext.TransformAttachementContentSignature;
  
  import java.time.Instant;
  import java.time.format.DateTimeFormatter;
  
  
  public class buildWithSOAP {
  
  	public static String getCurrentIsoUtcTimestamp() {
  		// 밀리초 3자리까지, 항상 'Z'로 끝나게 포맷
  		return DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSX")
  				.withZone(java.time.ZoneOffset.UTC)
  				.format(Instant.now());
  	}
  
  	public static final String wssswa = "http://docs.oasis-open.org/wss/oasis-wss-SwAProfile-1.1#Attachment-Content-Signature-Transform";
  
      static {
          org.apache.xml.security.Init.init();
          try {
              org.apache.xml.security.transforms.Transform.register(
                  wssswa, TransformAttachementContentSignature.class
              );
          } catch (org.apache.xml.security.exceptions.AlgorithmAlreadyRegisteredException e) {
              // 이미 등록된 경우 무시
          } catch (org.apache.xml.security.transforms.InvalidTransformException e) {
              throw new RuntimeException("Transform 등록 실패", e);
          }
          ResourceResolver.register(new ResolverOwnerDocumentUserData(), false);
      }
  
  	public static byte[] sign(
  		byte[] cmsEncrypted, 
          PrivateKey privateKey,
  	    X509Certificate cert,
          String endPoint,
  		String messageID,
  		String submitID,
  		String totalCount,
  		String replyTo
      ) throws Exception {       
  		byte[] taxInvoiceBlob = cmsEncrypted;
  		SOAPMessage message = buildMessage(endPoint, cert, messageID, submitID, totalCount, replyTo);
  		return  signMessage(message, taxInvoiceBlob, privateKey);
  		
  	}
  	
  	public static SOAPMessage buildMessage(
  		String endPoint, 
  		X509Certificate cert,
  		String messageID,
  		String submitID,
  		String totalCount,
  		String replyTo
  	) throws SOAPException, Exception {
  		// SOAP 1.1
  		MessageFactory factory = MessageFactory.newInstance(SOAPConstants.SOAP_1_1_PROTOCOL);
  		// SOAP 1.2
  		// MessageFactory factory = MessageFactory.newInstance(SOAPConstants.SOAP_1_2_PROTOCOL);
  		
  		SOAPMessage message = factory.createMessage();
  
  		SOAPPart part = message.getSOAPPart();
  		SOAPEnvelope envelope = part.getEnvelope();
  		SOAPHeader header = message.getSOAPHeader();
  		SOAPBody body = message.getSOAPBody();
  
  		SOAPHeader soapHeader = envelope.getHeader();
  		if (soapHeader == null) {
  			soapHeader = envelope.addHeader();
  		}
  
  		// SOAP 1.1 ns 등록
  		envelope.removeNamespaceDeclaration(envelope.getPrefix());
  		envelope.setPrefix("SOAP");
  		part.getDocumentElement().setPrefix("SOAP");
  		message.getSOAPHeader().setPrefix("SOAP");
  		message.getSOAPBody().setPrefix("SOAP");
  
  		// SOAP 네임스페이스와 기타 선언 재등록
  		envelope.addNamespaceDeclaration("SOAP", "http://schemas.xmlsoap.org/soap/envelope/");
  		// 이하 기존 네임스페이스 선언 코드 유지
  
  		envelope.addNamespaceDeclaration("ds", "http://www.w3.org/2000/09/xmldsig#");
  		envelope.addNamespaceDeclaration("wsa", "http://www.w3.org/2005/08/addressing");
  		envelope.addNamespaceDeclaration("wsse", "http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd");
  		envelope.addNamespaceDeclaration("wsu", "http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd");
  		envelope.addNamespaceDeclaration("xsd", "http://www.w3.org/2001/XMLSchema");
  		envelope.addNamespaceDeclaration("xsi", "http://www.w3.org/2001/XMLSchema-instance");
  		envelope.addNamespaceDeclaration("kec", "http://www.kec.or.kr/standard/Tax/");
  		// xsi:schemaLocation 추가
  		QName schemaLocName = new QName(
  			"http://www.w3.org/2001/XMLSchema-instance", // namespace URI
  			"schemaLocation",                            // local part
  			"xsi"                                        // prefix
  		);
  		envelope.addAttribute(
  			schemaLocName,
  			"http://schemas.xmlsoap.org/soap/envelope/ http://www.oasis-open.org/committees/ebxml-msg/schema/envelope.xsd"
  		);
  		
  		soapHeader.addChildElement("MessageID", "wsa").addTextNode(
  			messageID);
  		soapHeader.addChildElement("To", "wsa").addTextNode(
  			endPoint);
  		soapHeader.addChildElement("Action", "wsa").addTextNode(
  			"http://www.kec.or.kr/standard/Tax/TaxInvoiceSubmit");
  
  		// MessageHeader
  		SOAPElement kecMessageHeader = soapHeader.addChildElement(
  			"MessageHeader", "kec");
  		kecMessageHeader.addAttribute(
  			new QName("http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd", "Id", "wsu"),
  			"msgHeader"
  		);
  ....
  ....
  ```

​	

#### 2-5 MIME 패키징

> MIME도 처음들어봤다. <b>MIME (Multipurpose Internet Mail Extensions)</b>는  하나의 메시지 안에 여러 종류의 데이터를 담기 위한 포장 규칙 정도로 이해하고 있다. 간단히 “이 메시지 안에 텍스트도 있고, XML도 있고, 파일도 있으니
> 각각이 무엇인지 알려주기 위한 규칙”이다. 구조가 생소한데 눈에 익을때까지 봤다.

여기서는 python 으로 제작했다. 

```python
# - python bytes로 변화
java_soap_bytes = bytes(java_init_soap)

# 06. MIME init
# - 동적 변수 할당
boundary = generate_mime_boundary()
content_id = generate_content_id()

# - build mime
mime_message_bytes = build_mime(
    boundary=boundary,
    content_id=content_id,
    soap_bytes=java_soap_bytes,
    type="standard",
    version=soap_version,
    cms_encrypred_bytes=java_cms_encrypted_bytes,
)
```

- build_mime

  ```python
  def build_mime(
      boundary,
      content_id,
      soap_bytes,
      type,
      version,
      cms_encrypred_bytes=None,
  ):
      if version == "1.2":
          content_type = "Content-Type: application/soap+xml; charset=UTF-8"
      elif version == "1.1":
          content_type = "Content-Type: text/xml; charset=UTF-8"
  
      # mime root 부분 제작. 반드시 bytes로 조립!
      start_boundary = (
          f"--{boundary}\r\n{content_type}\r\nContent-ID: <{content_id}>\r\n\r\n".encode(
              "utf-8"
          )
      )
  
      soap_part = soap_bytes + b"\r\n"
  
      end_boundary = f"--{boundary}--\r\n".encode("utf-8")
  
      if type == "standard" and cms_encrypred_bytes is not None:
          # mime 첨부파일 부분 제작
          attachment_part = (
              (
                  f"--{boundary}\r\n"
                  "Content-Type: application/octet-stream\r\n"
                  "Content-ID: <payload_001>\r\n"
                  "\r\n"
              ).encode("utf-8")
              + cms_encrypred_bytes
              + b"\r\n"
          )
  
          mime_message_bytes = start_boundary + soap_part + attachment_part + end_boundary
  
      elif type == "requestResults":
          # 처리결과 요청 시
          mime_message_bytes = start_boundary + soap_part + end_boundary
  
      return mime_message_bytes
  
  ```

​	


#### 2-6 국세청에게 전송 및 응답 확인

이렇게 MIME로 패키징된 것을 국세청으로 발송하면 된다. 그러면 국세청에서 바로 접수증을 응답해준다. 다만,

여기서 유의하면 좋은 것이, 국세청 부서사용자에게 내 서버(로컬이든, dev서버든)의 callback 주소를 등록해야 하는데, http만 허용된다. 또 개인적으로 ngork은 등록은 되나 응답을 수신하지 못했다. 매우 불편하지만 localXponse라는 기능을 사용해 port로 열어야 응답을 확인할 수 있었다.   

​	

여기까지 숙지한다면, 국세청 표준인증을 통과할 수 있을 것이다. 

​	

​	

## 전체 프로세스

1. ~~국세청에 부서사용자 신청하기~~
2. ~~부서사용자에서 지원하는 표준인증검증 통과하기~~
3. 실사기술검증 통과하기
4. 등기로 국세청 실서버에 대한 정보 수신하기
5. 실서비스 운영

​	

표준인증검사를 마치면 **표준인증서**를 전달받을 수 있다. 이러면 이제 내 회사(혹은 사업자)가 속한 지역 세무서에 가서 ASP 사업자 등록?을 진행한다. 그러면 이후 실사검증 진행일정을 잡게 된다.

​	

이미 표준인증을 통과했다면, 실사검증에 대한 개발사항은 비교적 간단히 진행이 가능하다. 항목이 지역?이나 세무소?마다 다른지는 모르겠지만, 검증사항의 기능들을 몇몇개 소개하자면 다음과 같다. 

- 공급가액, 세액, 합계금액 자동게산 여부
- 지연발생 시 가산세 대상 안내를 제공하는지 여부
- 예약 발행 가능 여부 
- 시스템상 미래일자로 발행 가능하지 여부(법적으로 허용안됨)

...

​	

표준인증은 국세청과의 통신을 위해 최소한이자 모든 기능이 동작하는지 점검하는 느낌이라면, 실사검증은 시스템운영적으로 국세청에 해가되게(무분별하게) 발급이 되지 않는지,  법적 규제 및 안내가 적절히 이루어 지고 있는지 점검하는 느낌이다. 

​	

​	

​	

## 마무리

혼자서 5개월 동안 열심히 개발했다. 진짜 열심히 했다.

​	

​	

​	






