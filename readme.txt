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


















