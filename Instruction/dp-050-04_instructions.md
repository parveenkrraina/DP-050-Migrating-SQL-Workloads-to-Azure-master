# DP 050 – Migrating SQL Workloads to Azure

## Lab 4 – Migrate SQL Workloads to Azure  

**Estimated Time:** 60 minutes

**Pre-requisites:** There are no pre-requisite steps to perform in this lab.

**Lab files:** There are no lab files for this lab

## Lab Overview

In this lab you will perform a migration to Azure SQL Database. First you will perform a schema migration and create the destination database as a pre-requisite, then you migrate from a database running in a SQL instance to a SQL Database on Azure. You will perform an online migration using Database Migration Service (DMS), to keep the data in sync between the source and the target databases until cut over of the data migration.

## Lab Objectives

At the end of this lab, you will have:

1. Created a SQL Database using Azure Cloud Shell
1. Configure Azure Data Migration Service
1. Migrate the database schema to Azure SQL Database
1. Perform an online Migration using Data Migration Service

## Scenario

You are the senior database management lead of Adventureworks and are preparing to run a Data Modernization project. You will prepare the necessary environment to migrate a set of databases to SQL Server in an Azure Virtual Machine and perform test migrations using Data Migration Assistant.

## Exercise 1: Create a SQL Database using Azure Cloud Shell

In this exercise you’ll be using Azure Cloud Shell to:

- Create a new resource group

- Create a new Azure SQL Database Server Instance

- Configure the Azure SQL Database Server firewall

- Create a new General-Purpose Azure SQL Database

**Estimated Time:** 20 minutes

There are many options in Azure to automate the installation, management and deployment of services. One of the options could be installing the Azure CLI command line tool, a lightweight cross-platform command-line tool. Another option would be to automate and script using Azure Cloud Shell.

Azure Cloud Shell is an interactive shell environment hosted in Azure and managed through your browser. Cloud Shell lets you use either bash or PowerShell to work with Azure services. You can use the Cloud Shell pre-installed commands to run the code in this article without having to install anything on your local environment.

### Task 1: Log on and Configure Azure Cloud Shell

Note: This task can be fully completed in the Azure Portal

1. Go to the [Azure Shell](https://shell.azure.com)
1. Log on to your Azure Subscription with the credentials used for this training

 ![Welcome to Azure Shell Sign in Window](/images/azureshell.png)

3. Select Bash as the scripting environment

![Azure Shell Screen to select Bach shell](/images/bash.png)

### Task 2: Create a new storage account and share for Azure Cloud Shell

1. When prompted that you have no storage account created for Azure Cloud Shell, click **Show Advanced Settings**
1. Complete the following details:
    1. Subscription: **Select the Azure subscription used for this lab**
    1. Cloud Shell Region: **Select an available Cloud Shell region closest to your geographical location**
    1. Resource group:  
        1. Select **Create new**
        1. Type **dp050lab4rg**
    1. Storage account:
        1. Select **Create new**
        1. Type **dp050sa&lt;youridentifier&gt;** where **youridentifier** is a unique name
    1. File Share:
        1. Select **Create new**
        1. Type **dp050share&lt;youridentifier&gt;** where **youridentifier** is a unique name
    1. Select Create storage

Upon completion, the Azure Cloud Shell command line will open 

### Task 3: Create the SQL Database server instance and SQL Database by modifying and executing the following script

1. Click the **{}** icon to open the editor
1. Paste the script below and save it as File: **CreateSQLDBandServer**

&lt;Script file will also be available inside the hosted lab environment Labfiles folder, and in the lab starter folder on github&gt;

```shell
#bash script to create a new sql db server instance and sql db  

#Disclaimer: this script is a sample script on how to create an Azure database but uses least restrictive firewall settings for lab purposes. Do not use this script  

#defining variable to be passed to Azure CloudShell Bash

resourcegroup=dp-050-labresourcegroup

#edit the variable below to provide a unique server name

servername=dp-050-servername

#edit the variable below to provide the location – azure locations can be listed by typing az account list-locations -o table in the shell command interface

location=eastus

adminuser=sqladmin

password=Pa55w.rd.123456789

firewallrule=dp-050-access

#edit the script to provide a unique database name

labdatabase=dp-050-AdventureworksLT2008R2

#creates a resource group

az group create --name $resourcegroup --location $location

#creates a sql db servername

az sql server create --name $servername --resource-group $resourcegroup --admin-user $adminuser --admin-password $password --location $location

#shows current firewall list

az sql server firewall-rule list --resource-group $resourcegroup --server $servername

#creates a server-based firewall – note – you should restrict the start/end ip range based on your environment

az sql server firewall-rule create --resource-group $resourcegroup --server $servername --name $firewallrule --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255

#creates a general-purpose SQL database

az sql db create --name $labdatabase --resource-group $resourcegroup --server $servername -e GeneralPurpose -f Gen4 -c 1 

```

3. Execute the script by typing **sh CreateSQLDBandServer**

Upon completion and successful execution of the script you can validate that the resource group, server and database has been created in your Azure account subscription.

The bash script output should indicate a servername similar to the highlighted text below:

```shell
{ "administratorLogin": "sqladmin",

  "administratorLoginPassword": null,

  "fullyQualifiedDomainName": "dp-050-servername.database.windows.net",

  "id": "/subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/dp-050-labresourcegroup/providers/Microsoft.Sql/servers/dp-050-servername",

  "identity": null,

  "kind": "v12.0",

  "location": "eastus",

  "name": "dp-050-servername",

  "resourceGroup": "dp-050-labresourcegroup",

  "state": "Ready",

  "tags": null,

  "type": "Microsoft.Sql/servers",

  "version": "12.0"

}
```

## Exercise 2: Migrate a database to Azure SQL Database (offline)

In this exercise you’ll be migrating the database schema of a database in a SQL Server instance running on Azure (SQL 2017 VM created in previous labs) and load the schema into the SQL Database created in previous exercise.

You will:

- Create a new migration project using Data Migration Assistant
- Perform a Schema Migration Only

**Estimated Time:** 20 minutes

**Note:** This exercise will be completed using Data Migration Assistant running on the SQL Server 2008 R2 Virtual Machine

### Task 1: Create a Migration Project using Data Migration Assistant

1. Open **Microsoft Data Migration Assistant**
1. Click **+** to start a new project
1. Select **Migration** as Project type
1. Type **SQLDBMigration** as the name of the project
1. Validate the following settings:
1. Source server type: **SQL Server**
1. Target server type: **Azure SQL database**
1. Migration Scope: **Schema Only**
1. Select **Create**

### Task 2: Perform the schema migration

1. In the Connect to source server, type **LONDON**
1. Uncheck the **Encrypt Connection** option
1. Select **Connect**
1. As the list of databases displays select **AdventureworksLT2008R2**
1. As you validated there were no migration issues in a previous assessment **Uncheck the Assess database before Migration** checkbox
1. Click **Next**
1. In the Connect to target server dialog box, type the **servername** of the server you created in the previous exercise. List the server by its **full qualified name** (for example: dp-050-servername.database.windows.net)
1. Change Authentication type to **SQL Server Authentication**
1. Provide Username: **sqladmin**
1. Provide Password: **Pa55w.rd.123456789**
1. Click **Connect**
1. Select the target database you created in the previous exercise as the target database on the SQL Database server
1. Click **Next**
1. Review all the schema objects, as no issues were found click Generate SQL script

The Data Migration Assistant window will look similar to:
![Data Migration Assistant Window](/images/dbmigrate.png)

15. Click Deploy Schema
1. Close Data Migration Assistant

**Note:** All database objects will now be migrated to Azure SQL Database, this migration can take up to 10 minutes to complete 


## Exercise 3: Migrate on-premises databases to Azure SQL Database (online)

In this exercise you’ll be performing an online migration of a database using the Azure Data Migration Service.

You will be:

- Configuring the source instance for replication and make sure the pre-requisites for migration are met.
- Perform a live migration using Azure Data Migration Service

**Estimated Time:** 20 minutes

### Task 1: Configure SQL Server as Replication Distributor

This task will be completed using SQL Management Studio on the SQL 2008 R2 Virtual Machine but connected to the **SQL Server 2017 instance.**

**Note:** Perform these tasks on while connected to the SQL Server 2017 instance, to avoid having to configure VPN access between the hosted SQL 2008 R2 VM and Azure. In a non-lab environment, you would typically configure VPN or Expressroute.

1. In SQL Management Studio Object Explorer, connect to the SQL Server 2017 instance you created in Lab3.
    If the SQL Server 2017 instance was not registered, perform the following steps:
        select **Connect | Database Engine** and complete the following information in the **Connect to Server** dialog box:

            Servername: **<the full qualified domain name of your SQL 2017 VM>**

            For example sql2017vmxxxx.centralus.cloudapp.azure.com 

2. **Right-click** on **Replication**
1. Select **Configure Distribution**
1. Run the **Configure Distribution Wizard**
1. Accept the default settings on each of the Configure Distribution Pages.
    1. If the wizard prompts you to **start the SQL Server Agent Automatically**, click **Yes**.
1. Click **Next**
1. Click **Next** on the **Snapshot** folder
1. Click **Next** on the **Distribution** database name
1. Click **Next** on the **Publisher** dialog window
1. Click **Next** on the **Wizard Action** dialog window
1. Click **Finish**
1. If the **SQL Server Agent Fails to start**, start the agent manually by right-clicking on **SQL Server Agent in SQL Management Studio | Object Explorer | SQL Server Agent** and Start the service
1. Set the database recovery model for the **AdventureworksLT2008R2** database to FULL by executing the following query in a new query window:

```sql 
ALTER DATABASE AdventureworksLT2008R2 SET RECOVERY FULL WITH NO_WAIT
```

14. Perform a FULL Backup of the database by executing the following query  

```sql
BACKUP DATABASE AdventureworksLT2008R2 TO DISK = ‘d:\awlt2008r2backup.bak’
```

15. As the backup completes, close **SQL Management Studio**.

### Task 2: Perform an online migration using Azure Database Migration Service

In this task you will configure the Azure Database Migration Service to enable a live migration between the database running inside a VM and SQL Database.

**Note:** all these tasks are completed in the Azure Portal

1. In the Azure Portal, click **Create a resource**
1. Type **Azure Database Migration Service**
1. Click **Create**
1. In the Service Name – provide a service name **dp050dmsxxxxxx** where xxxxx is a unique value
1. Select your **Azure subscription**
1. Select the **resource group** you created previously
1. Select **location**
1. Create a **new Virtual network**, name it **OnPremGateway**
1. Change the pricing tier to **Premium**
1. Click **OK**
1. Click **Create**

**Note:** This deployment can take up to 10 minutes

12.Upon completion of the deployment, click Go to Resource

    Optional, you could add the Azure Data Migration Service to the left blade in the Azure portal

    a) Locate the Azure Data Migration Services in All Services on the Azure Portal 
    b) Check the star icon to add Azure Data Migration Services to the left blade in the Azure portal
    c) Expand the left blade to Azure Data Migration Service
    d) Select the Data Migration Service 

13. Click New Migration Project
1. Complete the New Migration Project dialog as:

    Project Name: **DP050OnlineMigration**

    Source Server type: **SQL Server**

    Target Server Type: **Azure SQL Database**

    Choose type of Activity: **Change to Online Data Migration**

15. Click Save
1. Click Create and Run Activity
1. In the Migration source detail provide the following information:

    Source SQL Server Instance name: &lt;full qualified domain name of your sql2017 vm&gt;

    Authentication type: **SQL Authentication**

    User name: **sqladmin**

    Password: **Pa55w.rd.123456789**

18. Uncheck Encrypt Connection
1. Click Save
1. Select the Target destination for the sql migration, and provide the following details in the Migration target details:

    Target servername: &lt;full qualified domain of the azure sql database server&gt;

    Authentication type: **SQL Authentication**

    User name: **sqladmin**

    Password: **Pa55w.rd.123456789**

21. Click **Save**
1. In **Map to target Database** window select the database for which you did the schema migration, also select the target database.
1. Click **Save**
1. In the **Configure migration** settings window, click on the dropdown list, notice that one table will not be replicated as Change Data Capture is not enabled for the dbo table.
1. Click **Save**
1. Name the **Migration project** activity, type **dp050lab4activity**

**Note:** in a production environment you would increase the database tier of the SQL database to perform faster load of the data.

27. Click **Run Migration**
1. During the migration process, **refresh** the **Activity** window.
1. Review the status, until the status is listed as **Ready to cutover**

    **Note:** if database changes were to occur, you can review the incremental data load

    Alternatively, you can insert some records in the **Productcategory** table in the source database using the following command (on the SQL instance that was set as the source database)

```sql
USE [AdventureWorksLT2008R2]
GO

INSERT INTO [Sales LT].[ProductCategory]
 ([ParentProductCategoryID]
 ,[Name]
 ,[ModifiedDate])
VALUES
 (1, ' Myproduct', getdate())

```

    Then review the Incremental data sync tab and explore the insert.

30. Select the database and then click **Start Cutover**
1. Check **Confirm**
1. Check **Validate my database(s)**
1. Check all the validation options
1. Click **Apply**

Congrats! You have now successfully completed an online migration to Azure SQL Database.
