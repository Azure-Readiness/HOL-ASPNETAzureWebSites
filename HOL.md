<a name="Title" />
# Building and Publishing ASP.NET Applications with Windows Azure Web Sites and Visual Studio 2010 #

---
<a name="Overview" />
## Overview ##

Web site publication and deployment has never been easier in Windows Azure. Using familiar tools such as Web Deploy or Git, and virtually no changes to the development workflow, Windows Azure Web Sites is the next step in Microsoft Azure platform for web developers. 

In this hands-on lab, you will explore the basic elements of the **Windows Azure Web Sites** service by creating a simple [ASP.NET MVC 4](http://www.asp.net/mvc/mvc4) application, which uses scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete). Then, you will deploy it using both Web Deploy from Visual Studio 2010 and Git.

Starting from a simple model class, and without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views. After publishing and running the solution, you will have the application database generated in your SQL Database server, together with the MVC logic and views for data manipulation.

<a name="Objectives" />
### Objectives ###

In this Hands-on Lab, you will learn how to:

- Create a web site from the Windows Azure Management Portal
- Use Visual Studio 2010 to build a new ASP.NET MVC 4 application
- Deploy the application using Web Deploy
- Create a new web site with Git Repository enabled to publish the ASP.NET MVC 4 application using Git

<a name="Prerequisites" />
### Prerequisites ###

The following is required to complete this hands-on lab:

- [Microsoft Visual Studio 2010](http://msdn.microsoft.com/vstudio/products/) with Service Pack 1
- [ASP.NET MVC 4](http://www.asp.net/mvc/mvc4/)
- [Microsoft Web Publish for Visual Studio 2010 (June 2012)] (http://www.microsoft.com/web/gallery/install.aspx?appid=WebToolsExtensionPublishingVS2010_Only_1_0)
- [GIT Version Control System](http://git-scm.com/download)
- A Windows Azure subscription with the Web Sites Preview enabled - you can sign up for free trial [here](http://bit.ly/WindowsAzureFreeTrial)

>**Note:** This lab was designed using Windows 7.

<a name="Setup"/>
### Setup ###

In order to execute the exercises in this hands-on lab you need to set up your environment.

1. Open a Windows Explorer window and browse to the lab's **Source** folder. 

1. Right-click the **Setup.cmd** file and click **Run as administrator**. This will launch the setup process that will configure your environment and check the dependencies.

>**Note:** Make sure you have checked all the dependencies for this lab before running the setup. 

---
<a name="Exercises" />
## Exercises ##

This hands-on lab includes the following exercises:

- [Getting Started: Creating an MVC 4 Application using Entity Framework Code First](#GettingStarted)
- [Exercise 1: Deploying an MVC 4 Application using Web Deploy](#Exercise1)
- [Exercise 2: Deploying an MVC 4 Application using Git](#Exercise2)

<a name="GettingStarted" />
### Getting Started: Creating an MVC 4 Appliction using Entity Framework Code First ###

In this section, you will create a simple ASP.NET MVC 4 application, using MVC 4 scaffolding with Entity Framework code first to create the CRUD methods.

<a name="GettingStartedTask1" />
#### Task 1 – Creating an ASP.NET MVC 4 Application in Visual Studio ####

1. Open **Visual Studio 2010** and click **File** | **New** | **Project**.

	![Creating a new project](images/new-website-vs2010.png?raw=true "Crating a new project")

	_Creating a new project_

1. Create a new **ASP.NET MVC 4 Web Application** and name it **MVC4Sample.Web**.

	![Creating a new ASP.NET MVC 4 Web Application](images/mvc4-sample.png?raw=true "Creating a new ASP.NET MVC 4 Web Application")

	_Creating a new ASP.NET MVC 4 Web Application_

1. Select **Internet Application** and click **OK**.

	![Choosing Internet Application](images/internet-application.png?raw=true "Choosing Internet Application")

	_Choosing Internet Application_

1. In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO). Name it _Person_ and click **OK**.

1. Open the **Person** class and insert the following three properties.

	<!-- mark:10-14 -->
	````C#
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Web;

	namespace MVC4Sample.Web.Models
	{
		public class Person
		{
			public int PersonID { get; set; }

			public string FirstName { get; set; }

			public string LastName { get; set; }
		}
	}
	````

1. Press **CTRL+SHIFT+B** to build the solution.

1. In the Solution Explorer, right-click the **Controllers** folder and select **Add | Controller**. 

1. Name the controller _PersonController_ and complete the **Scaffolding options** with the following values.	
	- In the **Template** drop-down list, select the **MVC Controller with read/write actions and views, using Entity Framework** option.
	- In the **Model class** drop-down list, select the **Person** class.
	- In the **Data context class** list, select **\<New data context...\>**.  In the dialog box displayed, set the name to _PersonContext_ and click OK.
	- In the **Views** drop-down list, make sure that **Razor** is selected.

	![Adding the Person controller with scaffolding](images/add-person-controller.png?raw=true "Adding the Person controller with scaffolding")

	_Adding the Person controller with scaffolding_
	
1. Click **Add** to create the new controller for **Person** with scaffolding. You have now generated the controller actions as well as the views. 
		
	![After creating the Person controller with scaffolding ](images/person-scaffolding.png?raw=true "After creating the Person controller with scaffolding")

	_After creating the Person controller with scaffolding_

1. Open **PersonController** class. Notice that the CRUD action methods have been generated automatically. 

	![Inside the Person controller](images/person-controller-code.png?raw=true "Inside the Person controller")

	_Inside the Person controller_

1. Do not close Visual Studio.

---

<a name="Exercise1" />
### Exercise 1: Deploying an MVC 4 Application using Web Deploy ###

In this exercise you will create a new web site from the Windows Azure Management Portal and publish the application you obtained in the Getting Started, taking advantage of the Web Deploy publishing feature provided by Windows Azure.

<a name="Ex1Task1" />
#### Task 1 – Creating a New Web Site from the Windows Azure Portal ####

1. Go to the [Windows Azure Management Portal](https://manage.windowsazure.com) and sign in using your Windows account credentials.

	![Log on to Windows Azure portal](images/login.png?raw=true "Log on to Windows Azure portal")

	_Log on to Windows Azure Management Portal_

1. Click **New** at the bottom of the page.

	![Creating a new Web Site](images/new-website.png?raw=true "Creating a new Web Site")

	_Creating a new Web Site_

1. Click **Web Site** and then **Quick Create**. Provide an available URL for the new web site and click **Create Web Site**.

	> **Note:** A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage. The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal. It does not include steps for setting up a database.

	![Creating a new Web Site using Quick Create](images/quick-create.png?raw=true "Creating a new Web Site using Quick Create")

	_Creating a new web site using Quick Create_

1. Wait until the new web site is created.

1. Once the web site is created click the link under the **URL** column. Check that the new web site is working.

	![Browsing to the new web site](images/navigate-website.png?raw=true "Browsing to the new web site")

	_Browsing to the new web site_

	![Web site running](images/website-working.png?raw=true "Web site running")

	_Web site running_

1. Go back to the portal and click the name of the web site under the **Name** column to display the management pages.

	![Openining the web site management pages](images/go-to-the-dashboard.png?raw=true "Openining the web site management pages")
	
	_Openining the Web Site management pages_

1. In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.

	> **Note:** The _publish profile_ contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method. The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled. Both **Microsoft WebMatrix** and **Microsoft Visual Studio** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites. 

	![Downloading the web site publish profile](images/download-publish-profile.png?raw=true "Downloading the web site publish profile")
	
	_Downloading the Web Site publish profile_

1. Download the publish profile file to a known location. Later in this exercise you will see how to import this file to publish a web application to Windows Azure Web Sites.

	![Saving the publish profile file](images/save-link.png?raw=true "Saving the publish profile")
	
	_Saving the publish profile file_

<a name="Ex1Task2" />
#### Task 2 – Configuring the Database Server ####

You will need a SQL Database server for storing the application database. You can view the SQL Database servers from your subscription in the portal at **Sql Databases** | **Server**. If you do not have a server created, you can create one using the **Add** button at the bottom of the page. Make note of the **server name and URL, administrator login name and password**, as you will use them next. Do not create the database yet, as it will be created by Entity Framework when running the application.

![SQL Database Server Dashboard](images/sql-database-server-dashboard.png?raw=true "SQL Database Server Dashboard")

_SQL Database Server Dashboard_

1. Go back to the MVC 4 solution, open the **Web.config** file.

1. Locate the **PersonContext** connection string, and update it with the following highlighted code. Update the placeholders with your SQL Database server URL, administrator login and password.
	
	<!-- mark:4 -->

	````XML
	<connectionStrings>
		...
		<add name="PersonContext" connectionString="Server=tcp:[SERVER_URL],1433;Database=MVC4SampleDB;User ID=[ADMINISTRATOR_LOGIN];Password=[PASSWORD];Trusted_Connection=False;Encrypt=True;Connection Timeout=30;"
      providerName="System.Data.SqlClient" />
  </connectionStrings>
	````

	> **Note:** Notice that, as you are using Entity Framework Code First approach, the database will be created the first time you run the application.
	> By default, it creates a SQL Database with the default settings:
	>
	> - Max Size: _1GB_
	> - Edition: _WEB_
	> 
	> You can update the SQL Database settings within **Databases** section in the [Windows Azure Management Portal](http://manage.windowsazure.com/).

<a name="Ex1Task2" />
#### Task 3 – Publishing an ASP.NET MVC 4 Application using Web Deploy ####

1. If not already opened, open the MVC 4 application you obtained in the **Getting Started** section. In the **Solution Explorer**,  right-click the web site project and select **Publish**.

	![Publishing the web site](images/publishing-the-web-site.png?raw=true "Publishing the web site")

	_Publishing the web site_

1. In the **Profile** page, click **Import** and select the profile settings file you downloaded earlier in this Exercise. Click **Next**.

	![Importing the Profile Settings File](images/importing-the-profile-settings-file.png?raw=true "Importing the Profile Settings File")

	_Importing the Profile Settings File_

1. In the **Connection** page, leave the imported values and click **Next**.

	![Setting up Web Deploy connection](images/setting-up-web-deploy-connection.png?raw=true "Setting up Web Deploy connection")

	_Setting up Web Deploy connection_

1. In the **Settings** page, leave the default values and click **Next**.

	![Setting up additional Settings](images/setting-up-additional-settings.png?raw=true "Setting up additional Settings")

	_Setting up additional Settings_

1. In the **Publish** page, click **Publish** to begin the application publishing process.

	![Publish web application preview page](images/publish-web-application-preview-page.png?raw=true "Publish web application preview page")

	_Publish web application preview page_

	> **Note:** If this is the first time you deploy the web site, you will be prompted to accept a certificate. After the message appears, click **Accept**.
	>
	>![Publish web application certificate](images/publish-web-application-certificate.png?raw=true "Publish web application certificate")

1. Once the publishing process finishes, switch back to the Windows Azure Management Portal.

1. In the **Dashboard** page of the Web Site, under the **quick glance** section, click the **Site URL** link in order to browse to your published web site.

	![Opening the Published Site](images/opening-the-published-site.png?raw=true "Opening the Published Site")

	_Opening the published web site_

1. Verify that the web site was successfully published in Windows Azure.

	![Application published to Windows Azure](images/application-published-to-windows-azure.png?raw=true "Application published to Windows Azure")

	_Application published to Windows Azure_

1. Go to **/Person** to verify that the Persons views are working as expected. You can try adding a new Person to verify it is successfully saved to the database.

	![Application Running](images/application-running.png?raw=true "Application Running")

	_Add Person view_

---

<a name="Exercise2" />
### Exercise 2: Deploying an MVC 4 Application using Git ###

<a name="Ex2Task1" />
#### Task 1 – Creating a New Web Site from the Windows Azure Portal ####

1. Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using your Microsoft account associated with your subscription.

	![Log on to Windows Azure portal](images/login.png?raw=true "Log on to Windows Azure portal")

	_Log on to Windows Azure Management Portal_

1. Click **New** on the command bar.

	![Creating a new Web Site](images/new-website.png?raw=true "Creating a new Web Site")

	_Creating a new Web Site_

1. Click **Web Site** and then **Quick Create**. Provide an available URL for the new web site and click **Create Web Site**.

	> **Note:** A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage. The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal. It does not include steps for setting up a database.

	![Creating a new Web Site using Quick Create](images/quick-create.png?raw=true "Creating a new Web Site using Quick Create")

	_Creating a new Web Site using Quick Create_

1. Wait until the new web site is created.

1. Once the web site is created, click the link under the **URL** column. Check that the new web site is working.

	![Browsing to the new web site](images/navigate-website.png?raw=true "Browsing to the new web site")

	_Browsing to the new web site_

	![Web site running](images/website-working.png?raw=true "Web site running")

	_Web site running_

<a name="Ex2Task2" />  
#### Task 2 – Setting up Git publishing ####

1. Go back to the Windows Azure Management Portal. In the **Web Sites** section, locate the web site you created in the previous task and open its dashboard. To do this, click the web site's **Name**. 

1. Under the quick glance section, click **Set up Git publishing**.

	![Set up Git publishing](images/set-up-git-publishing.png?raw=true "Set up Git publishing")

	_Set up Git publishing_

1. A message indicating that your Git repository is being created will appear.

	![Creating Git Repository](images/creating-git-repository.png?raw=true "Creating Git Repository")

	_Creating Git repository_

1. Wait until your Git repository is ready to use before going on with the following task.

	![Git Repository Ready](images/git-repository-ready.png?raw=true)

	_Git repository is ready_


<a name="Ex2Task3" />  
#### Task 3 – Push the Application to the Git Repository ####

1. Go to web site's **Dashboard** and copy the **Git Clone URL** value. You will use it later in this task.

	![Copy Git Clone URL](images/copy-git-clone-url.png?raw=true "Copy Git Clone URL")

	_Copy Git Clone URL_

1. Open a new **Git Bash** console and insert the following code. Update the _[YOUR-APPLICATION-PATH]_ placeholder with the path of the MVC 4 solution you've created in [exercise 1](#Exercise1). 

	> **Note:** You can also get a solution to deploy from the **Source\Assets** folder of this lab. But first, you need to open it with Visual Studio, build it to download the NuGet package dependencies and update the **PersonContext** connection string in Web.config to point to a Windows Azure SQL Database. See [Task 3 from Exercise 1](Ex2Task3).
	
	<!-- mark:1-4 -->
	````CommandPrompt
	cd "[YOUR-APPLICATION-PATH]"
	git init
	git add .
	git commit -m "Initial commit"
	````

	![Git initialization and first commit](images/git-initialization-and-first-commit.png?raw=true "Git initialization and first commit")

	_Git initialization and first commit_

1. Push your web site to the remote **Git** repository by running the following command. Replace the placeholder with the URL you obtained from the Windows Azure Management Portal. You will be promted for your deployment password.

	<!-- mark:1-2 -->
	````CommandPrompt
	git remote add azure [GIT-CLONE-URL]
	git push azure master
	````

	![Pushing to Windows Azure](images/pushing-to-windows-azure.png?raw=true "Pushing to Windows Azure")

	_Pushing to Windows Azure_

	> **Note:** When you deploy content to the FTP host or GIT repository of a Windows Azure website you must authenticate using **deployment credentials** that you create from the website’s **Quick Start** or **Dashboard** management pages. If you don't know your deployment credentials you can easily reset them using the management portal. Open the web site Dashboard page and click the **Reset deployment credentials** link. Provide a new password and click Ok. Deployment credentials are valid for use with all Windows Azure websites associated with your subscription.

1. In order to verify the web site was successfully pushed to Windows Azure, go back to the **Windows Azure Management Portal** and click **Web Sites**.

1. Locate your **Web Site** (where you deployed the application) and click its **Name** to see the **Dashboard**.

1. Click **Deployments** to see the **deployment history**. Verify that there is an **Active Deployment** with your _"Initial Commit"_.

	![Deployment](images/deployment.png?raw=true "Deployment")

	_Active deployment_

1.	Expand the **active deployment** and inspect the deployment history.

	![Deployment history](images/deployment-history.png?raw=true "Deployment history")

	_Deployment history_

1. Finally, click **Browse** to go to the web site. 

	![Browse web site](images/browse-web-site.png?raw=true "Browse web site")

	_Browse web site_

1. If the application was successfully deployed, you will see the ASP.NET MVC 4 template's default home page.

	![Application Running in Windows Azure](images/application-running-in-windows-azure.png?raw=true "Application Running in Windows Azure")

	_Application Running in Windows Azure_
	

1. Go to **/Person** to verify that the Persons views are working as expected. You can try adding a new Person to verify it is successfully saved to the database.

	![Application Running](images/application-running.png?raw=true "Application Running")

	_Add Person view_

---

<a name="Summary" />
## Summary ##
In this hands-on lab, you have created a new MVC web site using MVC 4 Scaffolding and published it to Windows Azure Web Sites. Web site publication and deployment has never been easier in Windows Azure. Using familiar tools such as Web Deploy or GIT, and virtually no changes to the development workflow, Windows Azure Web Sites is the next step in the Microsoft Azure platform for web developers. 
