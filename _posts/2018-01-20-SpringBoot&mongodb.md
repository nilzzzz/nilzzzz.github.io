---
layout: post
title: Spring Boot 数据存储  MongoDB
tag: SpringBoot
---

### 在SpringBoot基础上使用MongoDB

[什么是微服务？][1]在一篇很喜欢的公众号上看到的，漫画讲解很生动的说，另小灰取消了赞赏功能，大概是个有情怀的小灰~

本篇博客，参考原文：https://www.cnblogs.com/cnblog-long/p/7238851.html<br/>
在实际的项目有应用到，感谢分享~<br/>
### 1.1 环境依赖
在maven项目下，修改pom文件，添加spring-boot-starter-data-mongodb依赖<br/>

    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>
    
### 2.2 数据源
**方案一**：使用springboot 默认配置<br/>
MongoDB使用，在SpringBoot中同样提供了自配置功能.<br/>
默认使用localhost:27017的名称叫做test的数据库。<br/>
此外，我们也可以在 src/main/resources/application.properties 中配置数据源信息。<br/>

    spring.data.mongodb.uri=mongodb://localhost:27017/springboot-db
    
如果存在密码，配置改成如下<br/>

    spring.data.mongodb.uri=mongodb://name:pass@localhost:27017/dbname

**方案二**：手动创建
通过 Java Config 创建mongoTemplate<br/>


    @Configuration
    @EnableMongoRepositories
    public class MongoConfig extends AbstractMongoConfiguration {
    
    	private String mongoHost = "localhost";
    	private int mongoPort = 27017;
    	private String dbName = "dbname";
    
    	@Autowired
    	private ApplicationContext appContext;
    
    	@Override
    	protected String getDatabaseName() {
    		return dbName;
    	}
    
    	@Override
    	public Mongo mongo() throws Exception {
    		MongoClient mongoClient = new MongoClient(mongoHost, mongoPort);
    		return mongoClient;
    	}
    
    	@Override
    	@Bean
    	public MongoTemplate mongoTemplate() throws Exception {
    		return new MongoTemplate(mongo(), getDatabaseName());
    	}
    }
    
### 使用mongoTemplate操作
实体对象 bean相关<br/>

    @Document(collection = "author")
    public class Author {
    @Id
    private Long id;
    private String realName;
    private String nickName;
    // SET和GET方法
    }
    
DAO相关<br/>

我们通过mongoTemplate进行数据访问操作。<br/>

    @Repository
    public class AuthorDao {
    @Autowired
    private MongoTemplate mongoTemplate;
     
    public void add(Author author) {
    this.mongoTemplate.insert(author);
    }
    public void update(Author author) {
    this.mongoTemplate.save(author);
    }
    public void delete(Long id) {
    Query query = new Query();
    query.addCriteria(Criteria.where("_id").is(id));
    this.mongoTemplate.remove(query, Author.class);
    }
    public Author findAuthor(Long id) {
    return this.mongoTemplate.findById(id, Author.class);
    }
    public List<Author> findAuthorList() {
    Query query = new Query();
    return this.mongoTemplate.find(query, Author.class);
    }
    }
    
    
Service相关<br/>
Service层调用Dao层的方法.<br/>

    @Service
    public class AuthorService {
    @Autowired
    private AuthorDao authorDao;
     
    public void add(Author author) {
    this.authorDao.add(author);
    }
    public void update(Author author) {
    this.authorDao.update(author);
    }
    public void delete(Long id) {
    this.authorDao.delete(id);
    }
    public Author findAuthor(Long id) {
    return this.authorDao.findAuthor(id);
    }
    public List<Author> findAuthorList() {
    return this.authorDao.findAuthorList();
    }
    }
    
Controller相关<br/>
为了展现效果，我们先定义一组简单的　RESTful API 接口进行测试。<br/>

    @RestController
    @RequestMapping(value="/data/mongodb/author")
    public class AuthorController {
    @Autowired
    private AuthorService authorService;
    /**
    * 查询用户列表
    */
    @RequestMapping(method = RequestMethod.GET)
    public Map<String,Object> getAuthorList(HttpServletRequest request) {
    List<Author> authorList = this.authorService.findAuthorList();
    Map<String,Object> param = new HashMap<String,Object>();
    param.put("total", authorList.size());
    param.put("rows", authorList);
    return param;
    }
    /**
    * 查询用户信息
    */
    @RequestMapping(value = "/{userId:\\d+}", method = RequestMethod.GET)
    public Author getAuthor(@PathVariable Long userId, HttpServletRequest request) {
    Author author = this.authorService.findAuthor(userId);
    if(author == null){
    throw new RuntimeException("查询错误");
    }
    return author;
    }
    /**
    * 新增方法
    */
    @RequestMapping(method = RequestMethod.POST)
    public void add(@RequestBody JSONObject jsonObject) {
    String userId = jsonObject.getString("user_id");
    String realName = jsonObject.getString("real_name");
    String nickName = jsonObject.getString("nick_name");
    Author author = new Author();
    if (author!=null) {
    author.setId(Long.valueOf(userId));
    }
    author.setRealName(realName);
    author.setNickName(nickName);
    try{
    this.authorService.add(author);
    }catch(Exception e){
    e.printStackTrace();
    throw new RuntimeException("新增错误");
    }
    }
    /**
    * 更新方法
    */
    @RequestMapping(value = "/{userId:\\d+}", method = RequestMethod.PUT)
    public void update(@PathVariable Long userId, @RequestBody JSONObject jsonObject) {
    Author author = this.authorService.findAuthor(userId);
    String realName = jsonObject.getString("real_name");
    String nickName = jsonObject.getString("nick_name");
    author.setRealName(realName);
    author.setNickName(nickName);
    try{
    this.authorService.update(author);
    }catch(Exception e){
    e.printStackTrace();
    throw new RuntimeException("更新错误");
    }
    }
    /**
    * 删除方法
    */
    @RequestMapping(value = "/{userId:\\d+}", method = RequestMethod.DELETE)
    public void delete(@PathVariable Long userId) {
    try{
    this.authorService.delete(userId);
    }catch(Exception e){
    throw new RuntimeException("删除错误");
    }
    }
    }
    
### 巴拉巴拉
日常巴拉巴拉，最近一直在忙项目的事情，发现自己的能力很欠缺，改bug改的很爽，但是发现<br/>真的改不完~ 一个接着一个，忙碌充实不用去考虑其他的（ennnnn,确实也没工夫去瞎想了）。<br/>被一些小的bug折磨了好久，一步步的试错才找到最终的解决方案（估计，老师<br/>知道我的这点儿三脚猫功夫，以后坚决不敢让我上手项目了，趁着没被揭穿，赶紧提升自己~）<br/>

其实我期待着放假呢~<br/>

安利我喜欢的公众号：拾遗<br/>
从关注以来一直置顶，基本没落下过一篇文章~<br/>
今天看了一篇写GAI爷的，一个桀骜不驯但是谦逊有礼有担当的说唱歌手~就是狗粮撒的有点儿多~<br/>
愿你喜欢~<br/>


  [1]: http://mp.weixin.qq.com/s/FRVOYlgZCO524KwzQRohLA