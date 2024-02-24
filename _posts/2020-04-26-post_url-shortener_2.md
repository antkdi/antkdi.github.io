---
title: "SpringBoot로 완성하는 URL Shortener (2)"
comment: true
date: 2020-04-26
categories: [Programing, Java]
tags: [URL-Shortener, SpringBoot, H2, Base62]

---

## SpringBoot로 완성하는 URL Shortener (2) 

### 1.시작하는 말 

본격적으로 코드를 통한 가벼운 토이 프로젝트를 시작합니다.

우선 요구사항이 뭔지 보고 어떻게 구현할지 고민 해 봅시다.

스압 주의... 

### 2. 요구사항 분석

#### 2.1. 요구사항 

- **URL을 입력받아 짧게 줄여주고, Shortening된 URL을 입력하면 원래 URL로 리다이렉트하는 URL Shortening Service**
  - URL 입력폼 제공 및 결과 출력
  - URL Shortening Key는 8 Character 이내로 생성되어야 합니다.
  - 동일한 URL에 대한 요청은 동일한 Shortening Key로 응답해야 합니다.
  - 동일한 URL에 대한 요청 수 정보를 가져야 한다(화면 제공 필수 아님)
  - Shortening된 URL을 요청받으면 원래 URL로 리다이렉트 합니다.
  - Database 사용은 필수 아님



#### 2.2 구축목표

- 실제 리다이렉트 하는 HTML은 만들지 않고, 결과만 보여주는 간단한 화면 생성 (JSP)
- 저장된 데이터는 지속 가능해야 한다 ( 임베디드 DB 사용 - 재시작 시 정보를 가지고있어야함.)
- 인코딩은 Base62를 사용 (URL에 특화) 
  - [참고1]([https://www.popit.kr/%EC%9E%85-%EA%B0%9C%EB%B0%9C-base64-%EA%B0%80-%EC%9E%88%EB%8A%94%EB%8D%B0-base62-%EA%B0%99%EC%9D%80%EA%B1%B8-%EC%99%9C-%EC%8D%A8%EC%95%BC-%ED%95%98%EB%82%98%EC%9A%94/)([https://www.popit.kr/%EC%9E%85-%EA%B0%9C%EB%B0%9C-base64-%EA%B0%80-%EC%9E%88%EB%8A%94%EB%8D%B0-base62-%EA%B0%99%EC%9D%80%EA%B1%B8-%EC%99%9C-%EC%8D%A8%EC%95%BC-%ED%95%98%EB%82%98%EC%9A%94/](https://www.popit.kr/입-개발-base64-가-있는데-base62-같은걸-왜-써야-하나요/))
  - [참고2](https://metalkin.tistory.com/53)
- 원본 URL과 줄임 URL, 요청횟수를 저장할 수 있는 테이블 설계 (간단히)



#### 2.3 구성

- Spring Boot (StandAlone)
- Gradle
- JPA/Hibernate
- H2 Database
- JSP



### 3. 기본 구조 생성

#### 3.1 build.gradle

```groovy
plugins {
    id 'java'
    id 'idea'
    id 'war'
    id 'org.springframework.boot' version '2.2.2.RELEASE'
    id "io.spring.dependency-management" version "1.0.8.RELEASE"
    id 'java-library'
    id 'application'
}

group 'io.github.antkdi'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.8


bootWar{
    archivesBaseName = 'url-shortner'
    archiveName = 'url-shortner.war'
}

bootJar{
    archivesBaseName = 'url-shortner'
    archiveName = 'url-shortner.jar'
}

repositories {
    mavenCentral()
}

dependencies {
    testCompile group: 'junit', name: 'junit', version: '4.12'

    implementation('org.springframework.boot:spring-boot-starter-web')
    implementation('org.springframework.boot:spring-boot-starter-jdbc')
    implementation('org.springframework.boot:spring-boot-starter-data-jpa')
    implementation('org.springframework.boot:spring-boot-starter-test')
    testCompile group: 'org.assertj', name: 'assertj-core', version: '3.12.2'

    providedRuntime('org.apache.tomcat.embed:tomcat-embed-jasper')
    compile('javax.servlet:jstl:1.2')


    compile group: 'commons-validator', name: 'commons-validator', version: '1.6'
    runtimeOnly 'com.h2database:h2'

    //Lang
    compile group: 'org.apache.commons', name: 'commons-lang3', version: '3.5'

    compileOnly 'org.projectlombok:lombok:1.18.6'
    annotationProcessor 'org.projectlombok:lombok:1.18.6'
}

```



#### 3.2. application.yml 스프링 프로퍼티 정보

- DataSource 정보와 로깅정보등을 세팅

```yaml
spring:
  output:
    ansi:
      enabled: always
  ### MVC 패턴 JSP 리졸버 설정    
  mvc:
    view:
      prefix: /WEB-INF/views/
      suffix: .jsp
    ## 정적 리소스 패스 설정
    static-path-pattern: /static/**

logging:
  level:
    root: INFO
    org.springframework.web: ERROR
    org.hibernate: ERROR

---
### 습관적으로 profile을 사용합니다. 하지 않아도 무방합니다.
spring:
  profiles:
    active: local
  jpa:
    hibernate:
      ## 설정시 Entity 기준으로 생성 및 드랍 //여기선 제외
      ## ddl-auto: create-drop
    generate-ddl: false
    properties:
      hibernate:
        ##SQL 노출 및 포매팅
        show_sql: true
        format_sql: true
  h2:
    console:
      enabled: true
      ## DB명 세팅 해당 이름으로 파일 생성됨
      path: /test_db

  datasource:
    data: classpath:/schema.sql
    driver-class-name: org.h2.Driver
    url: jdbc:h2:file:./test_db;AUTO_SERVER=TRUE
    username: test
    password: 1234

---
```



#### 3.3. /src/resources/schema.sql

클래스 패스에 `schema.sql`이 있으면 H2 데이터베이스가 세팅될때 DDL을 로드 한다.

```sql
-- URL 정보가 저장될 테이블
CREATE TABLE  IF NOT EXISTS SHORT_URL (
  seq INT AUTO_INCREMENT  PRIMARY KEY,
  short_url VARCHAR(100),
  origin_url VARCHAR(2000) NOT NULL,
  req_count INT DEFAULT 1
);

-- 줄임 URL을 만들 시퀀스
CREATE SEQUENCE IF NOT EXISTS url_seq
MINVALUE 100000000
MAXVALUE 999999999
START WITH  100000000
INCREMENT BY 1
CACHE 20;

```

#### 

#### 3.4 ServletInitializer - 웹 어플리케이션 메인 메서드

```java
@SpringBootApplication
@ComponentScan( basePackages = "io.github.antkdi")
public class UrlShortServletInitializer extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(UrlShortServletInitializer.class);
    }

    public static void main(String[] args) {
        SpringApplication.run(UrlShortServletInitializer.class, args);                   
    }
}
```



### 4. 코드 작성

#### 4.1. DataSource 세팅

- SpringBoot 2 버전 부터 `Hikari Datasource`가 default 입니다.
- `Hikari Datasource` 에 대한 성능은 익히 알려져 있으니 찾아보시면 좋을 것 같습니다.

```java
@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(basePackages = {"io.github.antkdi.url_shortner.repository"})
public class DataSourceConfiguration extends HikariConfig {

    //Default DataSource

    @Bean(name = "entityManagerFactory")
    public LocalContainerEntityManagerFactoryBean entityManagerFactoryBean(EntityManagerFactoryBuilder builder, DataSource dataSource){
        return builder.dataSource(dataSource).packages("io.github.antkdi.url_shortner.entity").persistenceUnit("H2DBUnit").build();
    }

    @Bean(name = "transactionManager")
    public PlatformTransactionManager transactionManager(@Qualifier("entityManagerFactory") EntityManagerFactory entityManagerFactory) {
        return new JpaTransactionManager(entityManagerFactory);
    }
}

```



#### 4.2 MainController 

```java
@Controller
public class MainController {

    @GetMapping(value = "/")
    public String home() throws Exception{
        return "home";
    }
}
```

#### 4.3 RestController

- `urlStr` 파라미터를 요청 받아  `ShortUrlResult` 클래스를 리턴. `@ResponseBody`덕에 Json으로 반환

```java
@Slf4j
@CrossOrigin
@RestController
public class ConvertController {

    private final UrlConvertService urlConvertService;

    @Autowired
    public ConvertController(UrlConvertService urlConvertService){
        this.urlConvertService = urlConvertService;
    }

    /**
     * API 컨트롤러
     * @param urlStr
     * @return
     */
    @GetMapping(value = "/rest/convert", produces = {"application/json"})
    @ResponseBody
    ShortUrlResult convert(@RequestParam(defaultValue = "") String urlStr) {
        return urlConvertService.getShortenUrl(urlStr.trim());
    }
}
```

#### 4.4 VO, Enum

```java
/**
 * 줄임 처리된 url 과 원본 url 을 구분하는 열거형 상수
 */
public enum ShortUrlType{
    ORIGIN,SHORT; //반환타입이 원본 URL 인지 축약 URL 인지 
}

@Data
public class ShortUrlResult {

    //url Entity
    private ShortUrl shortUrl;
    //result Data
    private ShortUrlType shortUrlType;
    //flag
    private boolean successFlag;
}
```



#### 4.5 Service 

```java
//인터페이스
public interface UrlConvertService {
    ShortUrlResult getShortenUrl(String url);
}

//구현체
@Slf4j
@Service("urlConvertService")
public class UrlConvertServiceImpl implements UrlConvertService {

    private final CommonUtils commonUtils;
    private final UrlEncoder urlEncoder;
    private final UrlShortDao urlShortDao;

    @Autowired
    public UrlConvertServiceImpl(CommonUtils commonUtils, UrlEncoder urlEncoder, UrlShortDao urlShortDao){
        this.commonUtils = commonUtils;
        this.urlEncoder = urlEncoder;
        this.urlShortDao = urlShortDao;
    }


    /**
     * Url을 입력 받아 신규 데이터인 경우 줄임 처리 / 저장 후 반환
     * 기존의 데이터인 경우 원본 으로 변경 후 반환
     * 카운트++
     * @param url
     * @return
     */
    @Transactional(rollbackFor = Exception.class)
    @Override
    public ShortUrlResult getShortenUrl(String url) {
        ShortUrlResult shortUrlResult = new ShortUrlResult();

        ShortUrl shortUrl = new ShortUrl();
        //입력된 파라미터 유효성 검사
        if(!url.isEmpty() && commonUtils.urlValidationCheck(url)){

            //입력된 url이 저장된 축약Url이거나 원본 URL이 존재하면 
            //기존 저장된 URL 정보를 불러와서 요청 횟수를 증가시키고 반대의 URL을 반환
            if(urlShortDao.exists(url)){
                shortUrl = urlShortDao.findByUrl(url);
                shortUrlResult.setShortUrlType(shortUrl.getShortUrl().equals(url) ? ShortUrlType.ORIGIN:ShortUrlType.SHORT);
                shortUrl.setReqCount(shortUrl.getReqCount()+1);
                shortUrlResult.setShortUrl(shortUrl);
                shortUrlResult.setShortUrlType(ShortUrlType.ORIGIN);

            }else{
				//저장된 URL 정보가 없으면 새로 생성후
                //persist (save)해서 sequence를 먼저 생성하고 sequence를정보를 인코딩해
                //데이터베이스에 저장후 반대의 URL을 리턴
                //save Object
                ShortUrl curShortUrl = new ShortUrl();
                curShortUrl.setOriginUrl(url);
                curShortUrl.setReqCount(1);

                //persist - generate sequence
                shortUrl =  urlShortDao.save(curShortUrl);
                //encoding seq
                String encodeUrl = "";
                try{
                    //시퀀스를 Base62로 인코딩한다.
                    encodeUrl = encodingUrl(String.valueOf(shortUrl.getSeq()));
                }catch(Exception e){
                    e.printStackTrace();
                }finally {
                    shortUrl.setShortUrl(encodeUrl);
                }
                shortUrlResult.setShortUrl(shortUrl);
                shortUrlResult.setShortUrlType(ShortUrlType.SHORT);
            }
            shortUrlResult.setSuccessFlag(true);
        }
        //auto Commit;
        return shortUrlResult;
    }


    // Encode Sequence
    private String encodingUrl(String seqStr) throws Exception{
        return urlEncoder.urlEncoder(seqStr);
    }
}

```

#### 4.6 Module (인코딩)

```java
@Slf4j
@Component
public class UrlEncoder {

    private final String URL_PREFIX = "http://test.com/";
    private final int BASE62 = 62;
    private final String BASE62_CHAR = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

    private String encoding(long param) {
        StringBuffer sb = new StringBuffer();
        while(param > 0) {
            sb.append(BASE62_CHAR.charAt((int) (param % BASE62)));
            param /= BASE62;
        }
        return URL_PREFIX + sb.toString();
    }

    private long decoding(String param) {
        long sum = 0;
        long power = 1;
        for (int i = 0; i < param.length(); i++) {
            sum += BASE62_CHAR.indexOf(param.charAt(i)) * power;
            power *= BASE62;
        }
        return sum;
    }

    //신퀀스를 인코딩
    public String urlEncoder(String seqStr) throws NoSuchAlgorithmException {
        String encodeStr = encoding(Integer.valueOf(seqStr));
        log.info("base62 encode result:" + encodeStr);
        return encodeStr;
    }
   
    //디코딩
    public long urlDecoder(String encodeStr) throws NoSuchAlgorithmException {
        if(encodeStr.trim().startsWith(URL_PREFIX)){
            encodeStr = encodeStr.replace(URL_PREFIX, "");
        }
        long decodeVal = decoding(encodeStr);
        log.info("base62 decode result:" + decodeVal);
        return decodeVal;
    }
}
```



#### 4.7 Entity & Repository

```java
@Data
@Entity
@Table(name = "short_url")
public class ShortUrl {

    @Id
    @SequenceGenerator(name="seq_generator", sequenceName = "url_seq", allocationSize = 1)
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "seq_generator")
    @Column(name = "seq")
    BigInteger seq;
    @Column(name = "short_url")
    private String shortUrl;
    @Column(name="origin_url", nullable = false)
    private String originUrl;
    @Column(name= "req_count")
    private long reqCount;
}


public interface ShortUrlRepository extends JpaRepository<ShortUrl, Long> {

    ShortUrl findFirstByShortUrlOrOriginUrlOrderBySeqDesc(String short_url, String origin_url);
    boolean existsByShortUrlOrOriginUrl(String short_url, String origin_url);
}


```



#### 4.8 DAO

```java
@Repository
public class UrlShortDao {

    private final ShortUrlRepository shortUrlRepository;

    @Autowired
    public UrlShortDao(ShortUrlRepository shortUrlRepository){
        this.shortUrlRepository = shortUrlRepository;
    }

    /**
     * Url을 입력 받아 데이터베이스에 존재 하는지 판단한다.
     * @param url
     * @return
     */
    public boolean exists(String url){
        return shortUrlRepository.existsByShortUrlOrOriginUrl(url, url);
    }

    /**
     * url을 입력받아 저장된 Record 를 가져온다.
     * @param url
     * @return
     */
    public ShortUrl findByUrl(String url){
        return shortUrlRepository.findFirstByShortUrlOrOriginUrlOrderBySeqDesc(url, url);
    }

    /**
     * ShortUrl 저장.
     * @param shortUrl
     * @return
     */
    public ShortUrl save(ShortUrl shortUrl){
        return shortUrlRepository.save(shortUrl);
    }

}
```



#### 4.9 View (JSP) - WEB-INF/views/home.jsp

- 인풋을 과 버튼을 만들어 ajax로 간단하게 작성

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="kr">
<head>
    <meta charset="UTF-8">
    <title>Url Shortner Home</title>
    <script type="text/javascript" src="/static/asset/js/jquery-3.5.0.min.js"></script>
</head>
<body>
    <div>
        변경 할 URL (http:// | https:// 포함) : <input id="url_input" text="" />
        <input type="button" onclick="convert();" value="변경"/>
    </div>
    <div id="result" style="max-width: 1024px;"></div>
</body>
</html>

<script type="text/javascript">

    //단일 데이터 메칭
    function convert(){
        var text = $("#url_input").val();
        if(text != null){

            $.ajax({
                type: "get",
                url: "/rest/convert",
                data: {urlStr: text},
                success: function(data) {
                    console.log(data)
                    if(data.successFlag){
                        if(data.shortUrlType == "ORIGIN"){
                            $("#result").html("<p>원본 URL : " + data.shortUrl.originUrl +"</p><p> 요청 횟수:" + data.shortUrl.reqCount + "</p>");
                        }else{
                            $("#result").html("<p>변경 URL : " + data.shortUrl.shortUrl +"</p><p> 요청 횟수:" + data.shortUrl.reqCount + "</p>");
                        }

                    }else{
                        $("#result").html("<p>"+text + ' 줄이기 실패. </p>');
                    }

                }
            });

        }
    }
</script>
```



### 5. 결과 ( 화면 )



- 요청1

![8bf6fcc0-33a7-46b4-b4fd-035d35713cf8](https://user-images.githubusercontent.com/5414251/80402720-48553780-88f9-11ea-8294-95f4079fa26b.png)

- 요청2

![68d37885-315f-4386-8bce-744d65e57e72](https://user-images.githubusercontent.com/5414251/80402724-4a1efb00-88f9-11ea-8a55-66cd2088ca09.png)



전체 코드를 확인하실 분은 [Github](https://github.com/antkdi/url-shortener)를 참고하세요.
이상으로 포스팅을 마칩니다.