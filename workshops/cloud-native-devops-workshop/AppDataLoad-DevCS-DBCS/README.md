![](../common/images/customer.logo.png)
---
# ORACLE Cloud-Native DevOps workshop #

## Creating DB Schema, Loading Application Data and Creating Weblogic DataSource ##

### Introduction ###

This lab goes through the process of loading application data into DBCS. First we would create a Application specific user in DBCS after which we would be creating required tables and loading the application data. 

Once the data loading is complete we would create a DataSocurce in weblogic using which application can access the data.

### Prerequisites ###

+ Oracle DevCS account access
+ Oracle DBCS access



### Steps ###


1. Clone the Alpha office source code using DevCS 
2. Creating DB schema and loading data
3. Creating weblogic DataSource

#### 1. Log into your Cloud Account ####

+ Click Sign In from cloud.oracle.com

![](images/Sign-In.png)

+ On the next page enter your Cloud Account and click **My Services**

![](images/MyService.png)

+ In the next screen provide the appropriate username and password.

##### 2. Create a Project in Developer Cloud Service #####

Oracle Developer Cloud Service provides a complete development platform that streamlines team development processes and automates software delivery. The integrated platform includes an issue tracking system, agile development dashboards, code versioning and review platform, continuous integration and delivery automation, as well as team collaboration features such as wikis and live activity stream. With a rich web based dashboard and integration with popular development tools, Oracle Developer Cloud Service helps deliver better applications faster.

+	Open Developer Cloud Service console
+	From the Cloud UI dashboard click on the Instance on **Developer** service.

![](images/DevCs-instance.png)

+	Click on **Access Service Instance** 

![](images/DevCS-access.png)

+ Click **New Project** to start the project create wizard. **Note:** Depending on the status of your developer cloud service, it is possible that the button may be labeled **Create Project**

+ On Details screen enter the following data and click on **Next**.

**Name:** `Alpha Office`

**Description:** `Alpha Office Product Catalog`

![](images/Alpha_Office_Application_Workshop-05.png)

+ Leave default template set to **Empty Project** and click **Next**

![](images/DevCS-project-template.png)

+ Select your **Wiki Markup** preference to **MARKDOWN** and click **Finish**.

![](images/Alpha_Office_Application_Workshop-07.png)

+ The Project Creation will take about 1 minute.

![](images/Alpha_Office_Application_Workshop-08.png)

##### 3. Create Initial GIT Repository  #####

+ Click on **New Repository** to create a new Git Repository.

![](images/Alpha_Office_Application_Workshop-09.png)

+	In the New Repository wizard enter the following information and click **Create**.

**Name:** `AlphaOfficeProductCatalogUI`

**Description:** `Alpha Office Product Catalog UI`

**Initial content:** `Import existing repository`

**Enter the URL:** `https://github.com/dvukmano/cloud-native-devops-workshop.git`

![](images/Alpha_Office_Application_Workshop-10.png)


##### 4. Creating DB Schema and Loading Application Data #####

Before you start loading data into the Database, the SQL port 1521 of your DBCS instace will be diabled by default.

Follow the below steps to enable the port for your appliaction access.

+ Login to your cloud account and click on **Database** from the dashboard

![](images/AppDataLoad-DevCS-DBCS-01.png)

+	Click on **Open Service Console** 

•	Select the hamburger symbol to the right of DBCS instance **AlphaDBCS** and select **Access Rules**

![](images/AppDataLoad-DevCS-DBCS-02.png)

+ Select the hamburger symbol to the right of your of rule **ora_p2_dblistener** and select **Enable**

![](images/AppDataLoad-DevCS-DBCS-03.png)

+ Select **Enable** in the popup screen.

+ Open **Developer cloud Service** and click on the **Project** you have created **Alpha Office**

+ On navigation panel, click **Build** to access the build page and click **New Job**.

+ Give the Job Name as `DBBuild` and choose your software template `myLabTemplate` and click **Create Job**

![](images/DevCS-create-job.png)

+ Click on **Source Control**, click on **Add Source Code** and select **Git**

![](images/DevCS-add-git.png)

+ Under **Repository** select 'AlphaOfficeProductCatalogUI.git'

![](images/DevCS-choose-rep.png)

+ Click on tab **Builders**, click on **Add Builder** and select **SQLcl Builder**

![](images/DevCS-add-sql-builder.png)

+ Enter the following details

**Username:** `system`

**Password:** `Alpha2018#`

**Credentials File:** `leave it blank`

**Connect String:** `<public IP of DBCS instance>:1521:ORCL`

**Source:** `SQL File` 

**SQL File Path:** `AlphaOfficeProductCatalogUI/AlphaProducts/ConfigFiles/DB/createUserAlpha.sql`

![](images/DevCS-build-step1.png.png)

+ Click **Add Builder** and select **SQLcl Builder**

![](images/Devcs-add-build-step2.png)

+ Enter the following details

**Username:** `c##alpha`

**Password:** `Alpha2017_`

**Credentials File:** `leave it blank`

**Connect String:** `<public IP of DBCS instance>:1521:ORCL`

**Source:** `SQL File` 

**SQL statements:** `AlphaOfficeProductCatalogUI/AlphaProducts/ConfigFiles/DB/createProducts.sql`

![](images/DevCS-build-step2.png)

+ Click **Save**

+ In the next screen click **Build Now**

##### 5. Creating Weblogic DataSource #####

+ Login to your cloud account and click on **Java** from the dashboard

![](images/AppDataLoad-DevCS-DBCS-10.png)

+ Click on **Open Service console** in the next page

+ Select the hamburger symbol to the right of your JCS instance (AlphaJCS) and select **Open Weblogic Server Console**

![](images/AppDataLoad-DevCS-DBCS-11.png)

+ Log into Weblogic sever console with the `weblogic / Alpha2018#` 

+ From the left pane expand **Services** and select **Data Sources**

+ Lock domain, click on **New** and then select **Generic Data Source**

![](images/AppDataLoad-DevCS-DBCS-12.png)

+ Provide the following details in the next screen and click Next

**Name:** `AlphaProductsDB`

**JNDI Name:** `jdbc/Alpha01A-DBCS-ds-CanDo`

**Database type:** `Oracle`

+	Leave all the values as default and click **Next**

+	Leave all the values as default and click **Next**

+ Give the following Connection Properties in the next screen and click **Next**

**Database Name:** `ORCL`

**Host name:** `<Public IP address of DBCS>`

**Port:** `1521`

**Database User Name:** `c##alpha`

**Password:** `Alpha2017_`

+ Change the Connection URL to the format below and click **Test Configuration**

 `jdbc:oracle:thin:@< Public IP address of DBCS>:1521:ORCL`
 
![](images/AppDataLoad-DevCS-DBCS-13.png) 

•	In the next screen select **All Servers in the Cluster** and click **Finish** and **Activate Changes**

![](images/AppDataLoad-DevCS-DBCS-14.png) 












