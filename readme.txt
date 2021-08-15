Day1    15-Aug-2021
====================
JDK 8
=====
https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html#license-lightbox

STS Download
============
https://dist.springsource.com/release/STS/3.9.11.RELEASE/dist/e4.14/spring-tool-suite-3.9.11.RELEASE-e4.14.0-win32-x86_64.zip

Spring Distrubution 5.0
=======================
https://repo.spring.io/release/org/springframework/spring/5.0.2.RELEASE/


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

singleton -Stateless Application
prototype -Stateful Application

request  -web
session  -web




         