[TOC]

# quark-common

## src

### main

#### java/com/quark/common

##### base

###### BaseController.java

​	自动生成日志。

###### BaseService.java

| 接口：BaseService<T>                                         |
| ------------------------------------------------------------ |
| 方法T findOne(int key)                                       |
| 方法T save(T entity)                                         |
| 方法void delete(Object key)                                  |
| 方法List<T> findAll()                                        |
| 方法void deleteInBatch(Iterable<T> iterable)，批量删除，根据传入的Iterable，在表中依次删除Iterable的所有数据 |
| 方法List<T> findAll(Iterable<Integer> iterable)，批量查找，根据传入的Iterable，在表中依次查找Iterable的所有数据 |
| 方法List<T> save(Iterable<T> iterable)，批量保存，根据传入的Iterable，在表中依次保存Iterable的所有数据 |



###### BaseServiceImpl.java

​	`@Autowired`：可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。通过 @Autowired的使用来消除 set ，get方法

| 模板类：BaseServiceImpl<E extends JpaRepository,T> implements BaseService<T> |
| ------------------------------------------------------------ |
| 成员：protected E repository，是E类的（继承了JpaRepository），有注解@Autowired |
| 方法T findOne(int key)，有注解@Override（在AdminUserDao.java中解释） |
| 方法T save(T entity)，有注解@Override                        |
| 方法delete(Object key)，有注解@Override                      |
| 方法List<T> findAll()，有注解@Override                       |
| 方法void deleteInBatch(Iterable<T> iterable)，有注解@Override |
| 方法List<T> findAll(Iterable<Integer> iterable)，有注解@Override |
| 方法List<T> save(Iterable<T> iterable)，有注解@Override      |



###### ResultProcessor.java

​	`@FunctionalInterface`：只能标记在有且仅有一个抽象方法的接口上，加上该注解能够更好地让编译器进行检查。

| 接口：ResultProcessor             |
| --------------------------------- |
| 方法QuarkResult process()，仅申明 |



##### dao

数据访问对象data access object，各文件内的接口中的方法都是某一sql查询语句，分别对相应的表进行操作。

###### AdminUserDao.java

​	与表quark_adminuser有关的sql。

​	`@Repository`：把对象交给spring管理，用于注解DAO类

​	`@CacheConfig(cacheNames = "adminusers")`：类级别的注解，该类的数据缓存在名为adminusers的缓存空间中

​	`@Cacheable`：加了该注解的方法的结果可以存放在缓存里

​	`@Override`：检查对应的方法是否是你父类中所有的，如果没有则报错，防止错写

​	JpaRepository可以根据方法名自动生成sql：https://blog.csdn.net/fly910905/article/details/78557110

| 接口：AdminUserDao，继承了JpaRepository<AdminUser,Integer>和JpaSpecificationExecutor |
| ------------------------------------------------------------ |
| 方法AdminUser findOne(Integer integer)，返回AdminUser类绑定的表（即表quark_adminuser，见entity/AdiminUser.java）中primary key = integer的数据 |
| 方法List<AdminUser> findAll()，有注解@Cacheable，返回AdminUser类绑定的表的所有结果 |
| 方法AdminUser findByUsername(String username)，返回表中属性Username = username的数据 |
| 方法List findAll(Specification specification, Sort sort)，有注解@Cacheable和@Override，根据复杂查询specification得到结果，并按sort进行排序 |



###### CollectDao.java

与表quark_collect有关的sql。

接口：CollectDao extends JpaRepository<Collect,Integer>。



###### LabelDao.java

与表quark-label有关的sql。

接口：LabelDao extends JpaRepository<Label,Integer>。



###### NotificationDao.java

与表quark_notification有关的sql。

`@Query`：value中写sql语句，加上nativeQuery = true表示是原生sql语句，加了该注解的方法将使用该sql语句对响应表进行操作

`@Modifying`：涉及修改的sql语句，需要加上这个注解

| 接口：NotificationDao extends JpaRepository<Notification, Integer> |
| ------------------------------------------------------------ |
| 方法long getNotificationCount(Integer id)，有注解@Query，根据查询id的未读通知数量 |
| 方法List<Notification> getByTouserOrderByInitTimeDesc(User user)，查询所有touser=user的记录并按照InitTime进行降序排序 |
| 方法void updateByIsRead(User user)，有注释@Query和@Modifying，将user的已读通知的is_read属性更新为true |



###### PermissionDao.java

与表quark_permission有关的sql。

| 接口：PermissionDao extends JpaRepository<Permission,Integer> |
| ------------------------------------------------------------ |
| 方法Permission findOne(Integer integer)                      |
| 方法List<Permission> findAll()，有注解@cacheable             |



###### PostsDao.java

与表quark_posts有关的sql。

| 接口：PostsDao extends JpaRepository<Posts,Integer> ,JpaSpecificationExecutor |
| ------------------------------------------------------------ |
| 方法List<Posts> findAll()，有注解@Cacheable 和@Override      |
| 方法List<Object> findHot()，@Query查询表quark-posts中30天内回复数最多的10个帖子的id，title和reply_count |
| 方法Page<Posts> findByUser(User user, Pageable pageable)，返回所有User发的贴，Page<Posts>格式 |
| 方法Page<Posts> findByLabel(Label label,Pageable pageable)，返回所有标签为Label的贴，Page<Posts>格式 |



###### ReplyDao.java

与表quark_reply有关的sql。

| 接口：ReplyDao extends JpaRepository<Reply,Integer>,JpaSpecificationExecutor |
| ------------------------------------------------------------ |
| 方法List<Reply> findAll()，有注解@Cacheable和@Override       |



###### RoleDao.java

与表quark-role有关的sql。

| 接口：RoleDao extends JpaRepository<Role,Integer> |
| ------------------------------------------------- |
| 方法Role findOne(Integer integer)                 |
| 方法List<Role> findAll()，有注解@cacheable        |



###### UserDao.java

与表quark-user有关的sql。

| 接口：UserDao extends JpaRepository<User,Integer> ,JpaSpecificationExecutor |
| ------------------------------------------------------------ |
| 方法User findByUsername(String username)                     |
| 方法User findByEmail(String email)                           |
| 方法List<Object> findNewUser()，有注解@Query，找到30天内最近注册的12个用户，返回id，username和icon |



##### dto

数据传输对象data transfer object

###### PageResult.java

| 模板类：PageResult(implement serializable，实现java.io中的serializable接口) |
| ------------------------------------------------------------ |
| 成员：`private String draw请求次数`  `private Long recordsTotal总记录数`  `private Long recordsFiltered过滤后的总记录数 ` `private T data具体的数据` |
| 每个成员都有get和set方法                                     |
| 构造方法PageResult(String draw, Long recordsTotal, Long recordsFiltered, T data) |



###### QuarkResult.java

| 类：QuarkResult implement serializable                       |
| ------------------------------------------------------------ |
| 成员：`状态 private Integer status; ` `返回的数据private Object data` `错误信息private String error ` `返回总页数private long pageSize`  `本页返回数量 private Integer total` |
| 每个成员都有set和get方法                                     |
| 4个构造方法`QuarkResult(Integer status)`，`QuarkResult(Integer status, Object data)`，`QuarkResult(Integer status, Object data, long pageSize, Integer total)`，`QuarkResult(Integer status, String error)` |
| 方法ok()：返回值为状态成功（200）的QuarkResult               |
| 方法ok(Object data)：返回值状态成功且数据为data              |
| 方法warn(string warn):返回值状态警告（400）和警告信息        |
| 方法error(string error):返回值状态错误（500）和错误信息      |
| 方法QuarkResult ok(Object data,long pageSize,Integer total)  |



###### SocketMessage.java

| 类：SocketMessage implements Serializable                    |
| ------------------------------------------------------------ |
| 成员：`private Integer notice`   `private String message`    |
| 每个成员都有set和get方法                                     |
| 有两个构造方法`SocketMessage(Integer notice, String message)`和`SocketMessage(Integer notice)` |
| 方法public static SocketMessage build(Integer notice)，根据notice构造一个新的SocketMessage并返回 |
| 方法public static SocketMessage build(Integer notice,String message)，根据notice和message构造一个新的SocketMessage并返回 |



###### UploadResult.java

| 类：UploadResult implements Serializable                     |
| ------------------------------------------------------------ |
| 成员：`private Integer code，0表示成功，其余失败`   `private String msg，错误消息`， `private Data data` |
| 每个成员都有set和get方法                                     |
| 成功时的构造方法UploadResult(Integer code, Data data)        |
| 失败时的构造方法UploadResult(Integer code, String msg)       |

| 类：Data                                                     |
| ------------------------------------------------------------ |
| 成员：`private String src，路径`，`private String title，名称` |
| 两成员各有get方法                                            |
| 构造方法Data(String src)                                     |



##### entity

该目录下都是实体类。

###### AdminUser.java

​	对应表quark_adminuser。

​	`@Entity`：为某个类做注解，表明该类是一个实体类

 	`		 @Table`：为某个类做注解，声明与该实体类映射的数据库表名， 注解中的常用选项是 name，用于指明数据库的表名

​	`@Id`：为变量做注解，表示它是对应表的primary key

​	`@GeneratedValue`：为变量做注解，为一个实体生成一个唯一标识的primary key，与`@Id`配合使用

​	`@Column`：为变量做注解，标识其与数据表中字段的对应关系

​	`@JsonIgnore`：返回的json数据不包含该属性

​	`@JoinTable`：两个表相关联时使用，指出某两个表相关联时的各种关系

​	`@JoinColumn`：指出该表与其他表关联时用到的连接列，通常用于规定reference key

​	`	@ManyToMany`：和@JoinTable配合使用，指出两个表的关联是多对多的，且通常有cascade属性指明两个表的级联关系。

| 类：AdminUser implements Serializable                        |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue，表示它是表中的主键 |
| 成员：private String username，有注解@Column(unique = true,nullable = false)，表示该属性在表中是唯一的，且不能为null |
| 成员：private String password，有注解@Column(nullable = false)和@JsonIgnore，表示不能为null且返回的json数据不包括password |
| 成员：private Integer enable = 1，有注解@Column(nullable = false)，表示其默认为1，且不可为null,含义为“该管理员账号是否可以使用” |
| 成员：private Set<Role> roles = new HashSet<>()，有注解`@JsonIgnore， @JoinTable(name = "quark_adminuser_role",  joinColumns = {@JoinColumn(name = "adminuser_id",referencedColumnName = "id")},  inverseJoinColumns = {@JoinColumn(name = "role_id",referencedColumnName = "id")}) ，@ManyToMany(cascade = CascadeType.ALL)`，表示返回的json数据不包括roles，且当与表quark_adminuser_role关联时的连接列，以及两表是多对多关系，级联关系是ALL，表示一个表进行某种修改，另一个表都会级联修改。 |
| 每个成员都有get和set方法                                     |
| 方法public String toString() ，有注解@Override，规定返回的json数据的格式 |



###### Collect.java

​	对应表quark_collect。

​	`@ManyToOne `：指出两表的是多对一关系，其中的属性fetch规定加载数据时是懒加载还是急加载——https://blog.csdn.net/u010082453/article/details/43339031

| 类：Collect implements Serializable                          |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue         |
| 成员：private Posts posts，有注解@ManyToOne(fetch = FetchType.EAGER)和 @JoinColumn(nullable = false, name = "posts_id")，表示quark-collect和quark-posts是多对一关系，急加载，且关联列是posts_id，不可为null |
| 成员：private User user，有注解@ManyToOne(fetch = FetchType.EAGER) 和@JoinColumn(nullable = false, name = "user_id") |
| 成员：private Date initTime，有注解@JsonFormat(pattern = Constants.DATETIME_FORMAT)，规定了initTime的json格式是DATETIME_FORMAT（见utils/Constants.java） |
| 每个成员都有get和set方法                                     |



###### Label.java

​	对应表quark_label。

| 类：Label implements Serializable                            |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue         |
| 成员：private String name，有注解@Column(nullable = false, unique = true) |
| 成员：private Integer postsCount = 0，有注解 @Column(name = "posts_count")，主题数量默认为0 |
| 成员：private String details                                 |
| 每个成员都有get和set方法                                     |



###### Notification.java

​	对应表quark_notification。

| 类：Notification                                             |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue         |
| 成员：private boolean isRead = false，有注解@Column(name = "is_read")，是否已读 |
| 成员：private User touser，有注解@ManyToOne(fetch = FetchType.EAGER)和@JoinColumn(nullable = false,name = "touser_id")，要通知的用户 |
| 成员：private User fromuser，有注解@ManyToOne(fetch = FetchType.EAGER)和@JoinColumn(nullable = false,name = "fromuser_id")，发起通知的用户 |
| 成员：private Posts posts，有注解@ManyToOne(fetch = FetchType.EAGER)和@JoinColumn(nullable = false,name = "posts_id") |
| 成员：private Date initTime，@Column(nullable = false) @JsonFormat(pattern = Constants.DATETIME_FORMAT, timezone = "GMT+8")，发布时间 |
| 每个成员都由get和set方法                                     |



###### Permission.java

​	对应表quark_permission。

​	`@Transient`：表示该对象是个临时对象，不存入数据库

​	`mappedBy`：拥有关联关系的域，https://blog.csdn.net/xichenguan/article/details/44857799

| 类：Permission implements Serializable                       |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id @GeneratedValue          |
| 成员：private String name                                    |
| 成员：private String perurl                                  |
| 成员：private Integer type，表示资源类型，1是菜单，2是按钮   |
| 成员:private Integer parentid，有注解@Column(nullable = false)，父权限 |
| 成员：private Integer sort，排序                             |
| 成员：private String checked，@Transient，临时变量，是否选中 |
| 成员：private Set<Role> roles = new HashSet<>()，有注解@JsonIgnore和@ManyToMany(mappedBy = "permissions")，与quark_role的关系是多对多的，且在quark_role中的名称是permissions |
| 每个成员都有get和set方法                                     |



###### Posts.java

​	对应表quark_posts。

​	`columnDefinition` ：指出该列的数据类型

| 类：Posts implements Serializable                            |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue         |
| 成员：private Label label，有注解@ManyToOne和@JoinColumn(nullable = false, name = "label_id")，与标签的关系 |
| 成员：private String title，有注解@Column(unique = true, nullable = false)，标题 |
| 成员：private String content，有注解@Column(columnDefinition = "text")，内容，数据类型为文本 |
| 成员：private Date initTime，有注解@Column(nullable = false) 和@JsonFormat(pattern = Constants.DATETIME_FORMAT, timezone = "GMT+8")，发布时间 |
| 成员：private boolean top，是否置顶                          |
| 成员：private boolean good，是否是精品贴                     |
| 成员：private User user，有注解@ManyToOne和@JoinColumn(nullable = false, name = "user_id")，指出与用户的关联关系 |
| 成员：private int replyCount = 0，有注解@Column(name = "reply_count")，回复数量默认为0 |
| 每个成员都有set和get方法                                     |
| 方法String toString()，有注解@Override，规定返回的json数据的格式 |



###### Reply.java

对应表quark_reply。

| 类：Reply implements Serializable                            |
| ------------------------------------------------------------ |
| 成员：private Integer id，：有注解@Id和@GeneratedValue       |
| 成员：private String content，有注解@Column(columnDefinition = "text", nullable = false)，回复的内容 |
| 成员：private Date initTime，有注解@JsonFormat(pattern = Constants.DATETIME_FORMAT, timezone = "GMT+8")，回复的时间 |
| 成员：private Integer up = 0，点赞个数默认为0                |
| 成员：private User user，有注解@ManyToOne和@JoinColumn(nullable = false, name = "user_id")，指出与用户的关联关系 |
| 每个成员都有set和get方法                                     |
| 方法String toString()，有注解@Override，规定返回的json数据的格式 |



###### Role.java

对应表quark_role

| 类：Role implements Serializable                             |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue         |
| 成员：private String name，有注解@Column(unique = true,nullable = false) |
| 成员： private String description，对角色的描述              |
| 成员：private Integer selected，有注解@Transient，临时变量，是否选中 |
| 成员：private Set<AdminUser> adminUsers = new HashSet<>()，有注解@JsonIgnore和@ManyToMany(mappedBy = "roles")，指出角色与用户的关联关系 |
| 成员：private Set<Permission> permissions = new HashSet<>()，有注解@JsonIgnore     @JoinTable(name = "quark_role_permission",  joinColumns = {@JoinColumn(name = "role_id",referencedColumnName = "id")},  inverseJoinColumns = {@JoinColumn(name = "permissions_id",referencedColumnName = "id")}) @ManyToMany(cascade = CascadeType.ALL)，指出角色与权限的关联关系 |
| 每个成员都有set和get方法                                     |
| 方法String toString()，有注解@Override，规定返回的json数据的格式 |



###### User.java

| User implements Serializable                                 |
| ------------------------------------------------------------ |
| 成员：private Integer id，有注解@Id和@GeneratedValue         |
| 成员：private String email，有注解@Column(nullable = false)，注册邮箱 |
| 成员：private String username，有注解@Column(unique = true, nullable = false) |
| 成员：private String password，有注解@Column(nullable = false)和@JsonIgnore |
| 成员： private String icon ="http://127.0.0.1/images/upload/default.png"，头像，修改路径可以更改默认头像 |
| 成员：private String signature，个人签名                     |
| 成员：private Date initTime，有注解@Column(nullable = false) 和@JsonFormat(pattern = Constants.DATE_FORMAT, timezone = "GMT+8")，注册时间 |
| 成员：private Integer sex = 0，女性为1，男性为0              |
| 成员：private Integer enable = 1，有注解@Column(nullable = false)，是否可用，默认为１表示可用，0表示账号被封禁 |
| 每个成员都有set和get方法                                     |
| 方法String toString()，有注解@Override，规定返回的json数据的格式 |



##### enums

###### StateEnum.java

​	定义了状态的枚举：SUCCESS(200),WARN(400),ERROR(500)、

​	私有整数成员state、构造函数和getstate方法

​	

##### exception

###### ServiceProcessException.java

​	捕获运行过程中的异常。

| 类：ServiceProcessException extends RuntimeException         |
| ------------------------------------------------------------ |
| 构造方法ServiceProcessException(String message),根据异常信息构造对象 |
| 构造方法ServiceProcessException(String message, Throwable cause)，根据异常信息和原因构造对象 |



##### utils

###### Constants.java

​	给可能用到的一些常量取别名，类似于宏。

| 类：Constants                                                |
| ------------------------------------------------------------ |
| 成员`public static final String DATE_FORMAT = "yyyy-MM-dd"`，规定日期的格式 |
| 成员`public static final String DATETIME_FORMAT = "yyyy-MM-dd HH:mm:ss"`，规定日期时间格式 |



##### CommonApplication.java

1、	`@SpringBootApplication` ：申明让spring boot自动给程序进行必要的配置，开启spring boot的各项能力；

2、	`@EnableCaching` ：缓存功能，https://blog.csdn.net/weixin_38750084/article/details/83628886，https://blog.csdn.net/andy_zhang2007/article/details/97010733

3、

```
public static void main(String[] args) {
    SpringApplication.run(CommonApplication.class, args);
}
```

SpringApplication.run主要做两件事：

（1）创建SpringApplication对象：在对象初始化时保存事件监听器，容器初始化类以及判断是否为web应用，保存包含main方法的主配置类。

（2）调用run方法：准备spring的上下文，完成容器的初始化，创建，加载等。会在不同的时机触发监听器的不同事件。

SpringApplication.run的**参数是当前类的class和main的参数**，它会找到上面的注解@SpringBootApplication开始进行要求的所有配置

#### resources

##### application.properties

​	包含Mysql的配置（数据源类型、URL、数据库用户名和密码、jdbc驱动），JPA配置和对数据源的一些操作时间限制。

##### ehcache.xml

​	对各种缓存空间（默认缓存空间和@cacheconfig建立的缓存空间）进行配置。

### test/java/com/quark/common/CommonApplicationTest.java

​	构造了对数据库中的数据进行测试的类及其成员和方法名，但方法没有实现。

## target

​	都是src中的文件进行编译后产生的文件。

## pom.xml

maven的配置文件：https://blog.csdn.net/qq_33363618/article/details/79438044，针对整个quark-common

## quark-common.iml

​	IDEA自动生成的配置文件：https://blog.csdn.net/weixin_41699562/article/details/99552780，针对整个quark-common