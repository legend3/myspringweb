# Spring项目
> 前提：javaWeb new对象需要每次都加载文件
1. Spring开发Web项目 及 拆分Spring配置文件  
   a.Spring开发Web项目

> Web项目如何初始化SpringIOC容器 ：思路：当服务启动时（tomcat），通过监听器将SpringIOC容器初始化一次（该监听器 spring-web.jar已经提供）  
因此用spring开发web项目 至少需要7个jar： spring-java的6个jar + spring-web.jar，注意：web项目的jar包 是存入到WEB-INF/lib中

web项目启动时 ，会自动加载web.xml，因此需要在web.xml中加载 监听器（ioc容器初始化）。

Web项目启动时，启动实例化Ioc容器：
 <!-- 指定 Ioc容器（applicationContext.xml）的位置-->
  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:applicationContext.xml</param-value>
  </context-param>  
  <listener>
  	<!-- 配置spring-web.jar提供的监听器，此监听器 可以在服务器启动时 初始化Ioc容器。
  		初始化Ioc容器（applicationContext.xml） ，
  			1.告诉监听器 此容器的位置：context-param
  			2.默认约定的位置	:WEB-INF/applicationContext.xml
  	 -->
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>


b.拆分Spring配置文件
java项目：
applicationContext1.xml
applicationContext2.xml
applicationContext3.xml

ApplicationContext conext = new ClassPathXmlApplicationContext("applicationContext3.xml") ;

	Web项目：
		根据什么拆分？
		i.三层结构   
			UI(html/css/jsp  、Servlet)  applicationController.xml
			Service :applicationService.xml
			Dao:applicationDao.xml
			公共 数据库:applicationDB.xml

		ii.功能结构
			学生相关配置 applicationContextStudent.xml   <bean id=""  class="X...Student">
			班级相关配置 applicationContextClass.xml 

		合并：如何将多个配置文件 加载
			（1）
		  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
<param-name>contextConfigLocation</param-name>
<param-value>
classpath:applicationContext.xml,
classpath:applicationContext-Dao.xml,
classpath:applicationContext-Service.xml,
classpath:applicationContext-Controller.xml
</param-value>
</context-param>


		（2）推荐
  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
  		<param-name>contextConfigLocation</param-name>
  		<param-value>
  			classpath:applicationContext.xml,
  			classpath:applicationContext-*.xml
  		</param-value>
  </context-param>

		（3）只在web.xml中加载主配置文件，
		<param-value>
  			classpath:applicationContext.xml
  		
  		</param-value>
		然后在主配置问加中，加载其他配置文件
			<import resource="applicationContext-*.xml"/>
		
			
					

		



	

	Web项目：


2.Spring整合MyBatis



