---
title: "Spring Boot에서의 테스트 코드 작성"
comment: true
date: 2020-04-26
categories: [Programing, Java]
tags: [SpringBoot, JUnit, SpringBootTest, Test, TDD]
---

## Spring Boot에서의 테스트 코드 작성

InteliJ + SpringBoot Jpa를 사용한 테스트 코드를 살펴보자

여기서는 코드 자체의 테스트에 집중한 TDD 임을 정확히 알고 가자



> 테스트 코드를 작성한다는 것은( 작성 해 놓는다는 것은) 앞으로의 추가 개발과 배포시에 원래 기능에 이상이 없다는 것을 미리 알 수 있게 해주는 효과가 있다 이것만으로도 테스트 코드 작성은 엄청난 생산성을 가져다 준다.



#### 1. 테스트 코드 작성 환경

- `id 'org.springframework.boot' version '2.2.2.RELEASE'`
- `testCompile group: 'junit', name: 'junit', version: '4.12'`
- `testCompile group: 'org.assertj', name: 'assertj-core', version: '3.12.2'`
- `JPA/Hibernate` 
- `RestController`
- `InteliJ` 





#### 2. 코드 작성

- Controller 테스트

``` java 
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class RestControllerTest {

    @Autowired
    private MockMvc mockMvc;

    /**
     * 컨트롤러 테스트
     * @throws Exception
     */
    @Test
    public void rest_test() throws Exception {

        //given
        String urlStr = "www.myCompany.com";
        String ret = "{\"shortUrl\":null,\"shortUrlType\":null,\"successFlag\":false}";

        //when - '/rest/convert/' 엔드포인트를 파라미터 'urlStr'로 수행
        ResultActions actions = mockMvc.perform(get("/rest/convert")
                .param("urlStr", urlStr)
                .content(ret))
                .andDo(print());

        //then
        actions.andExpect(status().isOk())
                .andExpect(jsonPath("$.shortUrl").isEmpty())
                .andExpect(jsonPath("$.shortUrlType").isEmpty())
                .andExpect(jsonPath("$.successFlag").isBoolean())
        ;
    }

}
```

- JPA 테스트

```java
@RunWith(SpringRunner.class)
@DataJpaTest
@ActiveProfiles("local")
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
public class ShortUrlJpaTest {

    @Autowired
    private ShortUrlRepository shortUrlRepository;

    /**
     * Jpa 테스트
     */
    @Test
    public void shortUrlSaveTest(){

        //given
        final String urlStr = "http://www.naver.com";
        final ShortUrl shortUrl = new ShortUrl();
        shortUrl.setReqCount(1);
        shortUrl.setOriginUrl(urlStr);
        shortUrl.setShortUrl("http://test.com/KlkvG");

        //when
        shortUrlRepository.save(shortUrl);
        final ShortUrl savedUrl = shortUrlRepository.findFirstByShortUrlOrOriginUrlOrderBySeqDesc(urlStr, urlStr);

        //then
        Assert.assertThat(shortUrl.getShortUrl(), is(savedUrl.getShortUrl()));
        Assert.assertThat(shortUrl.getOriginUrl(), is(savedUrl.getOriginUrl()));

        //rollback Transaction
    }
}
```

- Module 테스트

``` java
@RunWith(SpringRunner.class)
@SpringBootTest
public class UrlEncoderTest {

    @Autowired
    UrlEncoder urlEncoder;

    /**
     * 모듈 테스트
     * @throws NoSuchAlgorithmException
     */
    @Test
    public void encodingTest() throws NoSuchAlgorithmException {

        //given
        final String url1 = "http://www.gmail.com";
        final String url2 = "https://www.facebook.com";
        final String url3 = "www.korea.com";
        final Map<String, String> keyMap = new HashMap<String, String>(){
            {
                put(url1,"12345");
                put(url2,"23456");
                put(url3,"34567");
            }
        };

        //when
        String result1 = urlEncoder.urlEncoder(keyMap.get(url1));
        String result2 = urlEncoder.urlEncoder(keyMap.get(url2));
        String result3 = urlEncoder.urlEncoder(keyMap.get(url3));

        //then
        Assert.assertEquals(result1, "http://test.com/HND");
        Assert.assertEquals(result2, "http://test.com/UGG");
        Assert.assertEquals(result3, "http://test.com/h9I");

    }
}
```



#### 3. 마치며

먼저 코드를 확인 하실분은 [Github](https://github.com/antkdi/url-shortener) 를 확인 해주세요.
