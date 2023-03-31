# 如何使用 NLP(自然语言处理)查找天气数据

> 原文：<https://medium.com/analytics-vidhya/how-to-find-weather-data-using-nlp-natural-language-processing-3093f7544a5f?source=collection_archive---------5----------------------->

[自然语言处理(NLP)](https://en.wikipedia.org/wiki/Natural_language_processing) 是一种人工智能过程，由计算机系统用来寻找文本内容的含义。这允许人类用户使用用正常句子以自然方式书写的句子来提问。成功的 NLP 系统将允许用户以不同的方式提问，并且仍然能够理解所提问题的含义。

![](img/abdaa912106582d7c60209c2703a1364.png)

在本文中，我们使用自然语言处理(NLP)来创建一个示例 Java 应用程序，它可以使用“自然语言”文本问题来查找天气数据。这允许用户使用查询来查找天气数据，例如:

*   *告诉我 1975 年 1 月 6 日日本东京的天气。*
*   *下周三英国伦敦的天气会怎么样？*
*   去年 7 月 4 日 DC 华盛顿州的天气如何？

*关于这个例子的完整源代码，请从 GitHub 资源库下载:*[https://GitHub . com/visual crossing/weather API/tree/master/Java/com/visual crossing/weather/samples](https://github.com/visualcrossing/WeatherApi/tree/master/Java/com/visualcrossing/weather/samples)

# 创建一个简单的 NLP 天气回答机器人的步骤

在我们的示例中，我们将把我们的 NLP 处理限制为识别用户是否在询问关于天气数据的问题。支持的问题将是找出即将到来的天气预报或查找历史天气观测。

为此，我们需要识别两条信息:

1.  用户试图查找天气信息的位置。
2.  天气数据的日期或日期范围。

一旦我们找到这些信息，我们就可以查找天气数据。为此，我们将使用[可视交叉天气 API](https://www.visualcrossing.com/resources/documentation/weather-api/timeline-weather-api/) 。这个时间线天气 API 可以很容易地查找过去和未来的天气数据，因为它会自动调整到我们要求的日期范围。如果我们询问未来的日期，它会给我们天气预报。如果我们要求过去的数据，那么它会给我们历史天气观测。

*要运行示例代码，您需要一个免费的 Weather API 密匙。如果您没有 API 密钥，请前往* [*免费注册页面*](https://www.visualcrossing.com/weather/weather-data-services) *创建您的密钥。*

# 第一步—设置 NLP 处理

有许多优秀的开源 Java 库可用于 NLP 处理。其中包括由[标准 NLP 小组](https://nlp.stanford.edu/)创建的 [Apache OpenNLP](https://opennlp.apache.org/) 和 [CoreNLP](https://stanfordnlp.github.io/CoreNLP/) 。在这个示例中，我们将使用 CoreNLP，因为它提供了一种简单的入门方式，并且包括针对我们需要 API 来回答的许多问题的预训练模型。

## 确定位置和日期

我们将使用 NLP 处理从用户问题中找到两条信息，这样我们就可以找到合适的天气数据——位置和日期。因此，我们将关注适当地使用命名实体识别来查找文本中的实体提及。实体提及表示文本中特别感兴趣的项目，例如人、位置、日期或时间。

我们正在寻找实体提及，确定天气数据的目标位置(如城市，州或省或国家)和我们应该找到天气数据的日期。

因此，我们需要 NLP 处理器能够从文本中定位。让我们考虑几个例子。

在这种情况下，处理器应该识别位置是日本东京，并且用户已经提供了确切的日期——1975 年 1 月 6 日。CoreNLP 包括一个演示 sit e，提供文本的图形解释:

我们可以看到，库正确地将位置识别为城市和国家的组合，然后正确地识别和解析预期日期。

我们将能够把“日本东京”的位置和日期传递给天气 API 来查找天气信息。

*英国伦敦******下周三*** *天气如何？***

**这种情况下的地点是英国伦敦。这个日期有点棘手，因为它不是一个简单的日期(尽管用户可能有许多方式要求日期，所以即使这也不是一个微不足道的练习！)我们希望 NLP 能够将“下周三”解释为询问日期，然后将其转换为确切的日期。**

**如果我们通过在线文本运行上面的文本，我们可以看到图书馆再次正确地找到了位置和预定日期:**

**我们可以将 NLP 处理视为正确理解了“下周三”的概念。从那里，图书馆找到当前日期之后的下一个星期三。**

******华盛顿州*** ***去年 7 月 4 日*** *？****

**如果我们看最后一个例子，我们再次看到 NLP 库正确地处理了位置和目标日期:**

**图书馆再次能够识别 DC 的华盛顿州，并解释“最后”是指 7 月 4 日的最后一次事件。**

# **第二步-创建 NLP Java 代码**

**我们现在准备创建 Java 样本的第一部分。我们的 simple 将是一个 Java 应用程序，它提示用户输入文本，处理所请求的位置和日期的文本，然后查找天气数据。为了简单起见，这将通过 Java system.out 来处理。**

**在进入代码之前，首先从 CoreNLP 下载所需的库。这些示例是使用版本 4.2.2 编写的，但是任何最新的版本都应该可以工作。**

**如果您使用 maven 配置您的 Java 项目，您可以在这里找到所需的 maven 依赖项。我们添加的 maven 依赖项有:**

```
**<dependency> 
 <groupId>edu.stanford.nlp</groupId> 
 <artifactId>stanford-corenlp</artifactId> 
 <version>4.2.2</version> 
</dependency> 
<dependency> 
 <groupId>edu.stanford.nlp</groupId>
 <artifactId>stanford-corenlp</artifactId>
 <version>4.2.2</version>
 <classifier>models</classifier>
 </dependency>**
```

## **设置 NLP 管道**

**我们代码的第一部分是主要的 Java 方法。在这个方法中，我们首先设置 CoreNLP 管道，并为用户输入文本创建一个基本循环。有关 CoreNLP 设置和配置的更多信息，请参见 [CoreNLP API 示例。](https://stanfordnlp.github.io/CoreNLP/api.html)**

```
**public static void main(String[] args) {
 // set up pipeline properties
 Properties props = new Properties();
 // set the list of annotators to run
 props.setProperty("annotators","tokenize,ssplit,pos,lemma,ner");

 // set to use the neural algorithm
 props.setProperty("coref.algorithm", "neural");
        props.setProperty("ner.docdate.usePresent", "true");
        props.setProperty("sutime.includeRange", "true");
        props.setProperty("sutime.markTimeRanges", "true");

 // build pipeline
 System.out.printf("Starting pipeline...%n");
 StanfordCoreNLP pipeline = new StanfordCoreNLP(props);
 System.out.printf("Pipeline ready...%n%n");

 }
}**
```

**我们减少了示例中注释器的数量，因为我们主要关注‘ner’注释器的输出。“ner”是“NERCombinerAnnotator”的缩写，它负责命名实体处理，我们需要找到位置和日期。其余的注释器是 ner 正常工作所必需的。**

**我们现在向上面的 main 方法添加一个循环，以允许用户输入文本并让示例处理数据。**

```
**public static void main(String[] args) {
 // set up pipeline properties
 Properties props = new Properties();
 // set the list of annotators to run
 props.setProperty("annotators","tokenize,ssplit,pos,lemma,ner");

 // set to use the neural algorithm
 props.setProperty("coref.algorithm", "neural");
        props.setProperty("ner.docdate.usePresent", "true");
        props.setProperty("sutime.includeRange", "true");
        props.setProperty("sutime.markTimeRanges", "true");

 // build pipeline
 System.out.printf("Starting pipeline...%n");
 StanfordCoreNLP pipeline = new StanfordCoreNLP(props);
 System.out.printf("Pipeline ready...%n%n");

 //loop for ever asking the user for text to process
 try (Scanner in = new Scanner(System.in)) {
  while (true) {
  System.out.printf("Enter text:%n%n");

  String text = in.nextLine();
  try {
   processText(pipeline, text);
  } catch (Throwable e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

  }
 }
}**
```

**用户输入文本后，该示例将配置的管道和文本传递给第二个方法“processText”来处理文本。**

## **处理位置和日期的文本**

```
**private static void processText(StanfordCoreNLP pipeline, String text) throws Exception {
 CoreDocument document = new CoreDocument(text);
 // annnotate the document
 pipeline.annotate(document);

 if (document.entityMentions()==null || document.entityMentions().isEmpty()) {
  System.out.println("no entities found");
  return;
 }

 String city=null;
 String state=null;
 String country=null;
 LocalDate startDate=null;
 LocalDate endDate=null;
     for (CoreEntityMention em : document.entityMentions()) {

      if (DATE.equals(em.entityType())) {
      Timex timex=em.coreMap().get(TimeAnnotations.TimexAnnotation.class);
      Calendar t1=timex.getRange().first;
      if (t1!=null) {
       startDate=LocalDateTime.ofInstant(t1.toInstant(), ZoneId.systemDefault()).toLocalDate();
      }
      Calendar t2=timex.getRange().second;
      if (t2!=null) {
       endDate=LocalDateTime.ofInstant(t2.toInstant(), ZoneId.systemDefault()).toLocalDate();
      }
      } else if (LOCATION_CITY.equals(em.entityType())) {
      city=em.text();
      } else if (LOCATION_STATE_OR_PROVINCE.equals(em.entityType())) {
      state=em.text();
      } else if (LOCATION_COUNTRY.equals(em.entityType())) {
      country=em.text();
      }
     }
     String location="";
     if (city!=null) location+=location.isEmpty()?city:(","+city);
     if (state!=null) location+=location.isEmpty()?state:(","+state);
     if (country!=null) location+=location.isEmpty()?country:(","+country);
     System.out.printf("Location=%s; fromDate=%s, toDate=%s%n", location,
      startDate!=null?startDate.format(DateTimeFormatter.ISO_LOCAL_DATE):"[Null]",
       endDate!=null?endDate.format(DateTimeFormatter.ISO_LOCAL_DATE):"[Null]"
       );

     if (location==null || location.isEmpty()) {
      System.out.println("no location information found");
      return;
     }
     timelineRequestHttpClient(location, startDate, endDate);
 }**
```

**首先，代码通过创建一个文档来处理文本，然后使用我们上面配置的管道对其进行注释。注释完成后，我们可以检查结果，看是否有任何请求的实体被提及。**

**为了找到可能提到的实体，我们在处理过程中循环遍历找到的列表。我们使用实体类型来帮助识别我们接收的信息。如果我们发现我们有一个日期，我们使用 Timex 实例来查找开始和结束日期。**

**之后，我们寻找与位置相关的注释(城市、州或省名或国家)。如果我们找到他们中的任何一个，我们会记住他们。**

**检查完所有实体后，我们构建一个位置字符串，它代表我们找到的位置实体的组合(就像地址是城市、州、国家等的组合一样)。我们需要一个字符串将这个位置信息传递给 Weather API，以帮助 API 识别所请求的确切位置。**

## **从天气 API 中检索信息**

**我们现在有了请求的位置和日期或日期范围，所以我们可以继续从 Weather API 读取天气数据。有关如何在 Java 中使用天气 API 以及通过网络读取数据的不同方式的更多信息，请查看[如何在 Java 中使用时间轴天气 API 检索历史天气数据和天气预报数据](https://www.visualcrossing.com/resources/documentation/weather-api/how-to-use-timeline-weather-api-to-retrieve-historical-weather-data-and-weather-forecast-data-in-java/)。**

**在我们的例子中，我们还需要一些 maven 依赖项来处理网络请求和 JSON 解析:**

```
**<dependency> 
 <groupId>org.apache.httpcomponents</groupId>
 <artifactId>httpclient</artifactId>
 <version>4.5.12</version>
 </dependency>
<dependency>
 <groupId>org.json</groupId>
 <artifactId>json</artifactId>
 <version>20200518</version>
</dependency>**
```

**天气 API 用于请求 JSON 格式的天气数据:**

```
**public static void timelineRequestHttpClient(String location, LocalDate startDate, LocalDate endDate ) throws Exception {
 //set up the end point
 String apiEndPoint="[https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/](https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/)";

 String unitGroup="us";

 StringBuilder requestBuilder=new StringBuilder(apiEndPoint);
 requestBuilder.append(URLEncoder.encode(location, StandardCharsets.UTF_8.toString()));

 if (startDate!=null) {
  requestBuilder.append("/").append(startDate.format(DateTimeFormatter.ISO_DATE));
  if (endDate!=null && !startDate.equals(endDate)) {
  requestBuilder.append("/").append(endDate.format(DateTimeFormatter.ISO_DATE));
  }
 }

 URIBuilder builder = new URIBuilder(requestBuilder.toString());

 builder.setParameter("unitGroup", unitGroup)
  .setParameter("key", API_KEY)
  .setParameter("include", "days")
  .setParameter("elements", "datetimeEpoch,tempmax,tempmin,precip");HttpGet get = new HttpGet(builder.build());

 CloseableHttpClient httpclient = HttpClients.createDefault();

 CloseableHttpResponse response = httpclient.execute(get);    

 String rawResult=null;
 try {
  if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
  System.out.printf("Bad response status code:%d%n", response.getStatusLine().getStatusCode());
  return;
  }

  HttpEntity entity = response.getEntity();
     if (entity != null) {
      rawResult=EntityUtils.toString(entity, Charset.forName("utf-8"));
     }

 } finally {
  response.close();
 }

 parseTimelineJson(rawResult);

 }**
```

**代码的第一部分创建时间线天气 API 请求 URL。首先，位置被添加到请求中。如有必要，可添加日期。如果只要求一个日期，则该日期会自动添加，否则会添加两个日期。如果请求中没有添加日期，将返回 15 天的预测。**

**单个日期请求将如下所示:**

```
**[https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/Washington,DC/2020-07-04?unitGroup=us&key=YOUR_API_KEY&include=dates&elements=datetimeEpoch,tempmax,tempmin,precip](https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/Washington,DC/2020-07-04?unitGroup=us&key=YOUR_API_KEY&include=dates&elements=datetimeEpoch,tempmax,tempmin,precip)**
```

**日期范围请求将如下所示:**

```
**[https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/Washington,DC/2020-07-04/2020-08-04?unitGroup=us&key=YOUR_API_KEY&include=dates&elements=datetimeEpoch,tempmax,tempmin,precip](https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/Washington,DC/2020-07-04/2020-08-04?unitGroup=us&key=YOUR_API_KEY&include=dates&elements=datetimeEpoch,tempmax,tempmin,precip)**
```

**还要注意可选的“元素”和“包含”参数的使用。这些参数用于过滤从请求中返回的数据量。**

***include=dates* 告诉 API 只给我们日期级别的信息(否则也会包括每小时的信息)。**

***elements=datetimeEpoch，tempmax，tempmin，precip* 指示 API 只返回这四条天气数据。风、压力等其他信息将不包括在内。**

## **显示天气数据**

**代码的最后一部分向命令行显示了一个非常简单的天气数据表:**

```
**private static void parseTimelineJson(String rawResult) {

 if (rawResult==null || rawResult.isEmpty()) {
  System.out.printf("No raw data%n");
  return;
 }

 JSONObject timelineResponse = new JSONObject(rawResult);

 ZoneId zoneId=ZoneId.of(timelineResponse.getString("timezone"));

 System.out.printf("Weather data for: %s%n", timelineResponse.getString("resolvedAddress"));

 JSONArray values=timelineResponse.getJSONArray("days");

 System.out.printf("Date\tMaxTemp\tMinTemp\tPrecip\tSource%n");
 for (int i = 0; i < values.length(); i++) {
  JSONObject dayValue = values.getJSONObject(i);
        ZonedDateTime datetime=ZonedDateTime.ofInstant(Instant.ofEpochSecond(dayValue.getLong("datetimeEpoch")), zoneId);

        double maxtemp=dayValue.getDouble("tempmax");
        double mintemp=dayValue.getDouble("tempmin");
        double precip=dayValue.getDouble("precip");

        System.out.printf("%s\t%.1f\t%.1f\t%.1f%n", datetime.format(DateTimeFormatter.ISO_LOCAL_DATE), maxtemp, mintemp, precip );
    }
}**
```

**这段代码解析从 Weather API 传入的 JSON 数据。从那里，代码遍历 days 数组中的值，并从每一天中提取 datetime、tempmax、tempmin 和 precip 值。**

**通过使用自纪元以来的秒数读取日期并提供位置时区 ID 来处理时间。这将创建一个 ZonedDateTime 实例。**

**最终结果是原始文本中请求的地点和日期的简单天气数据。**

**感谢您的阅读。如果您有任何意见或问题，请告诉我！**

***原载于 2021 年 6 月 11 日*[*【https://www.visualcrossing.com】*](https://www.visualcrossing.com/resources/documentation/weather-api/how-to-find-weather-data-using-nlp/)*。***