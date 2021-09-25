Day1    15-Aug-2021
====================
JDK 8
=====
https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html#license-lightbox

or

https://drive.google.com/file/d/1zXJcNOxU9CtKV0bB7F4OU19Fv1t05ugk/view?usp=sharing


STS Download
============
https://dist.springsource.com/release/STS/3.9.11.RELEASE/dist/e4.14/spring-tool-suite-3.9.11.RELEASE-e4.14.0-win32-x86_64.zip

Spring Distrubution 5.0
=======================
https://repo.spring.io/release/org/springframework/spring/5.0.2.RELEASE/

or

https://drive.google.com/file/d/1zXJcNOxU9CtKV0bB7F4OU19Fv1t05ugk/view?usp=sharing




Enterprise Application
=======================
Enterprise :(Business Organization) : make the money by providing services
===================================
Banks  :withdraw,deposit,fundTransfer,loan
LICS   :Insurance policy
Transports:book,cancel ticket
Hotels  :order food,book table
Hospitals:appointment
School   :admission,teaching,result
College  :admission,teaching,result


Applications development platforms
=======================
Java
.Net
PHP
Node JS
Python

Database Operation
=====================
C=Create
R=Retrieve
U=Update
D=Delete


Every Enterprise application is divided into 4 Layers
========================================
=>Layer :Logical separation of code
=>Tier  :Physical separation of code

1.Presentation Layer :Code written to provide the input screen and resposne to the user
2.Service Layer      :Logical implementation of business rules
         fundTransfer:(int source,int destination ,int amount)
             
             1.retireve source and validate amount < avialable
             2.retirve destination and validate 
             3.debit source -update
             4.credit destination -update
             5.commit

3.Data Access Layer  :Code written to access the data from Data source
4.Data Layer               :It is source for Data   


Three data sources
==================
Collection (List,Set,Map)
MySQL
MongoDB
Another Business Appln


Data Access Layer  : Hibernate
Service Layer      : EJB
Presentation Layer : Struts

Spring -One stop shop application
       
	It takes care of all layers of enterprise application


Develop an  Online Shopping Application

Write a Java application to perform standard CRUD operations on Customer and Product Domain Objects using Map as DataSource.
 
     
Customer
        customerId (unique ) -auto generated
        name
        pan
        mobile
	email
        address
        dob
        

Product:
        productId
	productName 
        prodctPrice
        productCategory
	productBrand
	
        
         
CustomerMainApp
              CustomerService
                           CustomerDao
                                        MapCustomerDaoImpl
                                                                CustomerMap
               







Spring :  IOC ,DI via XML/Annotation


IOC -Inversion of Control - Don't call me I  will call you

DI  -It is a mechanism of initializing the dependencies 

      1.setter injection      :
      2.constructor injection :     
      
      
We have to explain Spring Container about the spring beans(Java Classes) via XML file or annotations


Step 1: create a Java Project and add spring jar files to class path


Step 2: create a spring container (controls life-cyles of bean)

                 1.Core Container -BeanFactory based

                 2.Advanced Container -ApplicationContext

                 3.Web Container -WebApplicationContext



                             BeanFactory (I)  -Core Container
                                 |
                                 |XMLBeanFactory (C) :Lazy Initialization
                                 |
                            ApplicationContext(I)    :Eager Initialization  -Advanced Container
                                 |
ClasspathXMLApplicationContext (C)|    AnnotationConfigApplicationContext (C)
                                 |
                                 |
                           ServletWebApplicationContext(I)  
			   


//core container
Lazy Initiialization
XmlBeanFactory c=new XmlBeanFactory(new ClassPathResource("beans.xml"));
         
         
//advanced container
Eager Initialization 
ClassPathXmlApplicationContext c=new ClassPathXmlApplicationContext("beans.xml");
	    

<beans default-lazy-init=false|true/>
	    
<bean lazy-init=false|true/>


        
1.get bean by type if only one bean of specific type
  
  CustomerMainApp cma=c.getBean(CustomerMainApp.class);
         
2.get bean by id when multiple beans of same type

CustomerMainApp cma=(CustomerMainApp)c.getBean("customerMainApp");
        


DI :Mechamsim of initialzing the depencencies

  setter   ->     <property name="cs" ref="customerService">

  constructor -> <constructor-arg   name="cs" ref="customerService">
                          
                                                    
Spring Bean Life Cycle
==========================
1.Instantiation
2.Dependency Injection(setter/constructor)
3.Initialization   (init-method)
4.Service
5.Destruction      (destroy-method)



Spring Bean Scope : singleton|prototype|request|session





Spring Bean Scope
==================
<bean  scope="singleton"/>

singleton -Stateless Application   -> can be shared across multiple clients at same time
prototype -Stateful Application    -> can not be shared across multiple clients at same time

request  -web
session  -web


day2    :21-Aug-2021
=====================

Auto-wiring
============
Wiring - It's a mechanisam of associating the beans with each other

Auto-wiring -It's a mechanisam of delegating the responsisbilty of associating the beans with each other to the spring container

auto-wire="no|byType|byName|constructor"


byType: use if only one bean of specific type is avaiabale in xml
byName: use if more than one bean of specific type is avaiabale xml

Annotation Based configuration
===============================
                                           @Component

              @Controller                  @Service                   @Reposiotry

              Controller                   Service Layer               Dao Layer


<context:component-scan base-package="com"/>
=============================================

Scans the classpath for annotated components that will be auto-registered as Spring beans.
By default, the Spring- provided

@Component,
@Repository, 
@Service, 
@Controller,
@RestController, 
@ControllerAdvice,
@Configuration 

stereotypes will be detected. 




@Configuration   => <beans.xml>

@PostConstruct   => init-method

@PreDestroy      =>destroy-method

@Autowire        =>autwire (byType)   can be applied to constructor/setter/intreface

@Qualifier       =>byName 
   

<bean    init-method=""   destroy-method=""/>    @PostConstruct    @PreDestroy

<bean    autowire="no|constructor|byName|byType"/>    

@Autowire  =>default byType



day3    :  28-Aug-2021  & 29-Aug-2021
=====================================
Todays Topic :

1.JdbcTemplate 
2.Property PlaceHolderConfigurer



CustomerMainApp
              CustomerService
                           CustomerDao
                                        MapCustomerDaoImpl
                                                                CustomerMap
               

                                       MySQLCustomerDaoImpl
                                                                 JdbcTemplate
                                                                               DataSource
                                                                                       PropertyPlaceHolderConfigurer
					                db.properties
							 driverClassName
                                                                                                                 url
                                                                                                                 username
                                                                                                                 password

                                                                                                           



<!-- jdbcTemplate -->
<bean class="org.springframework.jdbc.core.JdbcTemplate" id="jdbcTemplate">
<property name="dataSource"   ref="dataSource"/>
</bean>


<!-- DriverManagerDataSource -->

<bean class="org.springframework.jdbc.datasource.DriverManagerDataSource" id="dataSource">
<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/company"/>
<property name="username" value="root"/>
<property name="password" value="admin"/>
</bean>



MySQL  -Script
 ===========
drop database company;
create database company;
use  company;
create table customers (customerId int primary key,name text,pan text(10),mobile text(10),email text,address text,dob date);
insert into customers values(1,'Pradeep Chinchole','ampid9854d','9158652627','pradeepch82@gmail.com','Pune','2011-10-10');
insert into customers values(2,'Pradeep Chinchole','ampid9854d','9158652627','pradeepch82@gmail.com','Pune','2011-10-10');
insert into customers values(3,'Pradeep Chinchole','ampid9854d','9158652627','pradeepch82@gmail.com','Pune','2011-10-10');
select * from customers;



1.RowMapper interface allows to map a row of the relations with the instance of user-defined class.
     It iterates the ResultSet internally and adds it into the collection. So we don't need to write a lot of code to fetch the records as ResultSetExtractor.

2.Advantage of RowMapper over ResultSetExtractor
  RowMapper saves a lot of code becuase it internally adds the data of ResultSet into the collection.

3.Method of RowMapper interface
    It defines only one method mapRow that accepts ResultSet instance and int as the parameter list.
   
      public T mapRow(ResultSet rs, int rowNumber)throws SQLException  



JdbcTemplate methods
===================
int update(String sql)               :DML   -insert,update,delete
List query(String sql)               :DRL   -select
T queryForObject(String sql)   :DRL   -select


SimpleDateFormat sdf=new SimpleDateFormat("dd-MM-yyyy");

date to string  => String  sdf.format(Date date)
string to date  => Date   sdf.parse(String date)





day4 : 4-Sep-2021    Spring Web MVC
====================================
1.Restful Web API             => Map 
2.Front end with JSP          => MySQL





http://localhost:8080/spring-web-mvc-cms/spring/hello
http://localhost:8080/spring-web-mvc-cms/spring/welcome
http://localhost:8080/spring-web-mvc-cms/spring/greet
http://localhost:8080/spring-web-mvc-cms/spring/today



1.Tomcat Download :https://apachemirror.wuchna.com/tomcat/tomcat-9/v9.0.46/bin/apache-tomcat-9.0.46.zip

2.Add Spring jar file to Tomcat lib directory

3.Create a dynamic web project with web.xml and Target runtime as Tomcat 9.0

4.Register a DispatcherServlet in web.xml as below

<servlet>
<servlet-name>spring</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
<servlet-name>spring</servlet-name>
<url-pattern>/spring/*</url-pattern>
</servlet-mapping>

5.By default DispatcherServlet will load the file with name (DispatcherServletName-serlvet.xml) 
  So create a file spring-servlet.xml in WEB-INF

6.Register a Internal ResourceViewResolver and enable componentscanning



<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

<mvc:annotation-driven/>
<context:component-scan base-package="com"/>
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/views/"/>
<property name="suffix" value=".jsp"/>
</bean>

</beans>


7.Create a Root Web application Context by adding listener in web.xml

by default it will search applicationContext.xml
  
<listener>
<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>/WEB-INF/applicationContext.xml</param-value>
</context-param>


Java Web MVC Models
====================
MVC (                 Model                   View                   Controller)
================================================================================
JSP Model-1          Java Bean                JSP                     JSP
JSP Model-2          Java Bean                JSP                     Servlet    (Struts1.x,JSF 1.x,2.x ,Spring Web MVC)
JSP Model-3          Java Bean                JSP                     Filter     (Struts2.x)
JSP Model-4          Java Bean                JSP                     Tag Handler


Spring Web MVC follows MVC2 /JSP Model2


Spring Web MVC
==============
Develop a Spring Web MVC application to build RESTful Web API to perform CRUD Operations


URI                      METHOD                           OPERATION
========================================================================================= 
/customers               GET                               GET ALL CUSTOMERS
/customers/1             GET                               GET CUSTOMER BY ID
/customers/1             PUT                               UPDATE CUSTOMER BY ID
/customers/1             DELETE                            DELETE CUSTOMER BY ID
/customers               POST                              ADD CUSTOMER 



HTTP status codes
======================
200    :   OK
201    :   Created
500    :   INTERNAL SERVER ERROR
404    :   NOT FOUND
204    :   NO CONTENT
401    :   INVALID CREDENTAILS
403    :   FORBIDDEN
405    :   METHOD NOT ALLOWED



@RestController   =>  @Controller +  @ResponseBody 



<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven/>
</beans>
 


Java To JSON    : @ResponseBody

JSON to Java    : @RequestBody
================================


@ResponseBody  => Server to Client =>  Java Object to JSON or XML   => Accept       = application/json 

@RequestBody  =>  Client to Server =>  JSON or XML to Java Object   => Content-Type = application/json


@ResponseBody  => Server to Client => Java Object to JSON or XML  => Accept       = application/json 

@RequestBody  => Client to Server =>  JSON or XML Java Object to  => Content-Type = application/json


For Message Conversion :

Step 1:  add below tag in spring-config file
=======
<mvc:annotation-driven/>

Step 2:  Add below jars in class path
=======
jackson-core-2.9.8
jackson-annotations-2.9.8.jar
jackson-databind-2.9.8.jar


https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.9.8
https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations/2.9.8
https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind/2.9.8


hello   =>       /WEB-INF/views/hello.jsp
welcome =>       /WEB-INF/views/welcome.jsp
greet    =>       /WEB-INF/views/greet.jsp


http://localhost:8080/spring-web-mvc-cms/spring/*
http://localhost:8080/spring-web-mvc-cms/rest/*


/spring/*   spring-ds    Controller        Service              Dao/Repository (mySQL)

/rest/*     rest-ds      Controller        Service              Dao/Repository (map)


day6  :  11-Sep-2021 
======================

What is Spring Boot?
=====================
1.Spring Boot provides a good platform for Java developers to develop a stand-alone and 
production-grade spring application that you can just run.
2.You can get started with minimum configurations without the need for an entire Spring configuration setup.

Spring Boot offers the following advantages to its developers -
=================================================================
1.Easy to understand and develop spring applications
2.Increases productivity
3.Reduces the development time

Goals
======
1.To avoid complex XML configuration in Spring
2.To develop a production ready Spring applications in an easier way
3.To reduce the development time and run the application independently
4.Offer an easier way of getting started with the application

Why Spring Boot?
================

You can choose Spring Boot because of the features and benefits it offers as given here -

1.It provides a flexible way to configure Java Beans, XML configurations, and Database Transactions.

2.It provides a powerful batch processing and manages REST endpoints.

3.In Spring Boot, everything is auto configured; no manual configurations are needed.

4.It offers annotation-based spring application

5.Eases dependency management (POM)

6.It includes Embedded Servlet Container


How does it work?
=>Spring Boot automatically configures your application based on the dependencies you have added to the project by using
 @EnableAutoConfiguration annotation. 
 
 For example, if MySQL database is on your classpath, but you have not configured any database connection, 
 then Spring Boot auto-configures an in-memory database.

=>The entry point of the spring boot application is the class contains @SpringBootApplication annotation 
   and the main method.

Spring Boot automatically scans all the components included in the project by using @ComponentScan annotation.



Spring Boot Starters   (https://start.spring.io/)
=====================
=>Handling dependency management is a difficult task for big projects. 
=>Spring Boot resolves this problem by providing a set of dependencies for developers convenience.
=>For example, if you want to use Spring and JPA for database access, 
    it is sufficient if you include spring-boot-starter-data-jpa dependency in your project.

Note that all Spring Boot starters follow the same naming pattern spring-boot-starter- *, 
  where * indicates that it is a type of the application.

Examples
Look at the following Spring Boot starters explained below for a better understanding -

1.Spring Boot Starter Actuator dependency is used to monitor and manage your application. Its code is shown below -

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>


2.Spring Boot Starter Security dependency is used for Spring Security. Its code is shown below -

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>


3.Spring Boot Starter web dependency is used to write a Rest Endpoints. Its code is shown below -

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>
      
4.Spring Boot Starter Test dependency is used for writing Test cases. Its code is shown below -

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-test</artifactId>
</dependency>      
      
      
Auto Configuration
====================


1.Spring Boot CLI (https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.5.4/spring-boot-cli-2.5.4-bin.zip)

2.https://start.spring.io/
3.STS



https://start.spring.io/




Spring Web WEB
==================
Build web, including RESTful, applications using Spring MVC. Uses Apache Tomcat as the default embedded container.

Spring Boot DevTools DEVELOPER TOOLS
=====================================
Provides fast application restarts, LiveReload, and configurations for enhanced development experience.

Spring Boot Actuator OPS
========================
Supports built in (or custom) endpoints that let you monitor and manage your application - 
such as application health, metrics, sessions, etc.

Spring Security SECURITY
=========================
Highly customizable authentication and access-control framework for Spring applications.

Spring Data JPA SQL
===================
Persist data in SQL stores with Java Persistence API using Spring Data and Hibernate.

CRUDRepository                CRUDRepository
     |                              |
PagingAndSortingRespistory    MongoRepository
     |
JPAReposiotry




Spring Data MongoDB NOSQL
=========================
Store data in flexible, JSON-like documents, meaning fields can vary from document to document and data structure can be changed over time.




https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html


spring.datasource.url=jdbc:mysql://localhost:3306/company
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=admin
#spring.jpa.database-platform=org.hibernate.dialect.H2Dialect


#InternalResourceViewResolver 
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp



<!--   To generate JSP output -->
		

                <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</dependency>

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>
		


day7:  12-Sep-2021 
======================

Todays Topics
==============
1.Spring Boot Security Customization

Spring Security
================
Default username :user
Password         :generated on console


To customize it
================

add below properties in application.properties
==============================================
spring.security.user.name=pradeep
spring.security.user.password=pradeep


JAAS -API (Java Authentication and Authorization Service)


401    :   INVALID CREDENTAILS   :NOT Authenticated
403    :   FORBIDDEN             :NOT AUTHORIZED 



Java Authentication and Authorization Service

Authentication
================
1.BASIC  (Before Spring Boot 2.x)
2.DIGEST
3.FORM BASED (From Spring Boot 2.x)  : Endpoints login,logout


Authorization (role)
====================

To Customize default JAAS

We should write a configuration class by extending WebSecurityConfigureerAdapter and override configure methods.


@Configuration
public class BankConfig  extends WebSecurityConfigurerAdapter{

	@Autowired
	private DataSource dataSource;
	
	
	public BankConfig() {
	System.out.println("==========BankConfig created=================");
	}
	
	
	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		System.out.println("==========BankConfig created  AuthManagerBuilder=================");
		//super.configure(auth);
	
		
		/*
		 * auth.inMemoryAuthentication()
		 * .withUser("RAM").password("{noop}RAM").roles("ADMIN").and()
		 * .withUser("RAHIM").password("{noop}RAHIM").roles("STUDENT").and()
		 * .withUser("DAVID").password("{noop}DAVID").roles("TEACHER");
		 * 
		 * 
		 */
		
		auth.jdbcAuthentication()
		    .passwordEncoder(new BCryptPasswordEncoder())
		    .dataSource(dataSource);
		
	}
	
	
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		System.out.println("==========BankConfig created  HttpSecurity=================");
		//super.configure(http);  Default is Form Based
		
		
		http.authorizeRequests()
		.antMatchers("/rest/*").permitAll()
		.antMatchers("/rest/*").denyAll()
		.antMatchers("/spring/*").hasRole("ADMIN")
		.antMatchers("/spring/welcome","/spring/today").hasRole("STUDENT")
		.antMatchers("/spring/hello","/spring/greet").hasAnyRole("ADMIN","TEACHER")
	
		
		    .anyRequest()
		    .authenticated()
		    .and()
		    .formLogin();//  Form Based
		    //.httpBasic();// Basic Auth
		
	}
	
	
}


data.sql
=========
insert into users(username, password, enabled)values('RAM','$2a$10$ge2ybXdrHWmY6qNWOkCaze2jAgTfeTovMsbUciUJCzIcfG/x.YGZi',true);
insert into authorities(username,authority)values('RAM','ROLE_ADMIN');
 
insert into users(username, password, enabled)values('RAHIM','$2a$10$ZuVLPUbpOc7Bxeu5GRdD4.XH0XWl4H6103a9OqNP58PTX/zqeGe1W',true);
insert into authorities(username,authority)values('RAHIM','ROLE_STUDENT');

insert into users(username, password, enabled)values('DAVID','$2a$10$9T6gPdQEug.dtrOorCwlVeQLd14OzZv649bGnl2fv3/jl4NZVc22u',true);
insert into authorities(username,authority)values('DAVID','ROLE_TEACHER');

schema.sql
============
drop table authorities;
drop table users;

create table users (
    username varchar(50) not null primary key,
    password varchar(120) not null,
    enabled boolean not null
);

create table authorities (
    username varchar(50) not null,
    authority varchar(50) not null,
    foreign key (username) references users (username)
);



application.properties
=========================
spring.datasource.initialization-mode=ALWAYS



#datasource details
#spring.datasource.url=${RDS_URL:jdbc:h2:mem:testdb}
#spring.datasource.driverClassName=${RDS_DRIVER_CLASS:org.h2.Driver}
#spring.datasource.username=${RDS_USERNAME:sa}
#spring.datasource.password=${RDS_PASSWORD:blank}
#spring.jpa.database-platform=${RDS_DIALECT:org.hibernate.dialect.H2Dialect}


spring.datasource.url=jdbc:mysql://localhost:3306/company
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=admin
#spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect


2.Spring Boot -JdbcTemplate -Application


CustomerService
              CustomerDao
                      JdbcTemplate
                                DataSource
                                       driverClassName
                                       url
                                       username
                                       password


day8:  12-Sep-2021 
==================
1.Spring  Data JPA -  MySQL
2.Spring -Data MongoDB -  MongoDB
3.Hibernate Relations



Spring Data JPA SQL
Persist data in SQL stores with Java Persistence API using Spring Data and Hibernate.


JPA -Java Persistence API  -ORM Specification

    Hibernate 
	ibatis
    toplink

	
Object Relational Mapping

============================================
            Customer
===========================================
Id      Name     email           Mobile
============================================
101     Pradeep p@gmail.com      9836353535      Customer c=new Customer(101,"Pradeep" ,"P@gmail.com","9836353535");
102     Sachin  s@gmail.com      8836353535
 



@Entity
@Table(name="pc_customers")
class Customer{
@Id
@Column(name="Id")
private int id;
private String name;
private String email;
privste String mobile
} 
 
 
                                      CRUDRepository
                                            |
                                   PaginAndSortingRepository
                                            |
                ----------------------------------------------------------
                |                                                        |  				
            JPARepository                                          MongoRepository




Mongo DB  -NOSQL
=================

https://www.tutorialspoint.com/mongodb/index.htm


The following table shows the relationship of RDBMS terminology with MongoDB.

================================================================
RDBMS	                                     MongoDB
================================================================
Database	                                  Database
Table	                                      Collection
Tuple/Row	                                  Document
column	                                      Field
Table Join	                                  Embedded Documents
Primary Key	                                  Primary Key (Default key _id provided by MongoDB itself)

Database Server and Client

mysqld/Oracle	                                   mongod  -Server
mysql/sqlplus	                                   mongo   -Client  


MySQL Workbench                                     ROBOMONGO
SQL Developer                                       Mongo Compass  





MongoDB  download  
==================
https://fastdl.mongodb.org/windows/mongodb-windows-x86_64-5.0.2-signed.msi



To start mongo db server  (default it needs folder c:\data\db so first create it)
=========================
C:\Program Files\MongoDB\Server\4.4\bin\>mongod


C:\Program Files\MongoDB\Server\4.4\bin\>mongod --dbpath e:\xyz



3.Hibernate Relations
======================
1.One to One         => Employee =>EmployeeAddress
2.One to Many        => Group    => Stories
3.Many To One        => Stories  =>Group
4.Many to Many       => Book     => Author 

   
  
Day 9
=========
1.Spring AOP
2.Spring AOP Transaction
3.Valdiations
3.Google Oauth Security



Spring AOP  (Aspect Oriented Programming)
===========================================


Application Requirement    =     Functional requirement           + Non Functional requriement


Net Banking                =      withdraw                        +  Security  
                                  deposit                            logging
                                  fundTransfer   					 tx management			


Application  Logic	       =       Business Logic                 + common logic

                              
Concern :   Piece of code 

Application Code           =       Core Concern                   + Corss cutting concern


Why we need AOP?

AOP is used to separate Cross Cutting Concern from Core Concern and attach that Cross Cutting Concern to Core Concern dynamaically at run time.


Can't we separate Cross Cutting Concern from Core Concern using Object Oriented Programming? 

Yes ,But it is a static apporach.Every thing we have to decide in advance.


Core Concern
=============

class Bank{

  public boolean withdraw(String source,double amount){
  Securiy s=new Securiy();
  if(s.login()){
  }
  }
  
 public boolean deposit(String source,double amount){
  Securiy s=new Securiy();
  if(s.login()){
  }
  
  }
  
 public boolean fundTransfer(String source,String destnation,double amount){
  Securiy s=new Securiy();
  if(s.login()){
  }
   
 }
}

Corss Cutting Concern
=====================

class Security{

 boolean login(String username,String password){
 
 }

}


Does AOP replace OOP?

No ,AOP complements OOP


AOP terminologies?
===================
1.Target :It is piece of code that implements Core concern.
          Ex. Bank

2.Advice :It is a piece of code that implements Cross Cutting Concern
         
		 what to execute and when to execute

          Ex. login  
		  
before          :It executes before of method excutes. 
after           :It executes irrespective of method excutes successfully or not. similar to finally
afterreturning  :It executes irrespective of method excutes successfully only
afterthrowing   :It executes when method throws the exception. 
around 		    :It is a combination of before,after,afterreturn and after throwing 
 
 
AOP use cases
============= 
logging-related          =around
security checks          =before
transaction management   =after return ,after after throwing 
tweaking of a legacy application 
		  

3.Join point :It is a well defined point in Core concern  where you want to execute the Cross cutting concern.

        1.When class is loaded
		2.When method is called
        3.When constructor is invoked

Spring supports only method call as a join point


4.Point cut :It is set of one or more join points
             

5.Aspect : Advice + Point Cut

           What +When + Where          

6.Weaving : It is mechanism of attaching the CCC to CC.









  


 							  


















 
 
 
 
 
 
 
 
 
 
 







Shopping Cart Case Study
=========================
https://o7planning.org/10683/create-a-shopping-cart-web-application-with-spring-boot-hibernate


Spring Boot Shopping Cart Tutorial with MySQL Database, Thymeleaf, Bootstrap and jQuery
https://www.youtube.com/watch?v=rFSxmKen6aQ




									   
















