# DP 050 – Migrating SQL Workloads to Azure

## Lab 3 – Migrate SQL Workloads to SQL Server in Azure Virtual Machine

**Estimated Time:** 60 minutes
**Pre-requisites:** There are no pre-requisite steps to perform in this lab.
**Lab files:** There are no lab files for this lab

## Lab Overview

The students will initially evaluate the migration process that they will use to migrate from an on-premises SQL Server 2008 R2 instance to an SQL Server 2017 running in a virtual machine. They will then perform a migration using Data Migration Assistant to move databases using Data Migration Assistant.  Finally, they will assess a successful migration.

## Lab Objectives

At the end of this lab, you will have:

1. Created a new SQL Server 2017 Virtual Machine on Azure
2. Created an Azure Storage Account and Fileshare
3. Performed a Migration of SQL Server 2008 R2 databases to SQL Server in Azure VM

## Scenario

You are the senior database management lead of Adventureworks and are preparing to run a Data Modernization project. You will prepare the necessary environment to migrate a set of databases to SQL Server in an Azure Virtual Machine and perform test migrations using Data Migration Assistant.

## Exercise 1: Creating a new SQL Server 2017 Virtual Machine on Azure

In this exercise, you will create a new virtual machine on Azure, using the Azure Portal.

**Estimated Time:** 20 Minutes

The main task for this exercise is as follows:

1. Create a New Virtual Machine in the Azure Portal

### Task 1: Provision a SQL Server 2017 Virtual Machine

**Note:** if you are running this lab in a hosted lab environment, perform these steps inside the provisioned lab environment

1. In the [Azure portal] (https://portal.azure.com), select **Create a resource**, then type **SQL Server 2017 on Windows 2016** in the Marketplace search box.
1. Select **Free SQL Server License: SQL Server 2017 Developer on Windows Server 2016** in the **select a software plan** dropdown.
1. Select Start with a pre-set configuration.
1. In **Select a workload environment** select **Dev/Test**.
1. In **Select a workload type**, keep the default **General purpose (D-series)**.
1. Select **Continue to Create a VM**.
1. In the Project Details window, Select **Create new** to create a new resource group.
1. Specify **DP-050-Training** as the name for the resource group.
1. Provide the instance details by providing the following details:

    1. Virtual Machine Name: **sql2017vm**
    1. Region: **select a region closest to your physical location**
    1. Image: **Free SQL Server License: SQL Server 2017 Developer on Windows Server 2016**.
    1. Username: **sqladmin**
    1. Password: **Pa55w.rd.123456789**
    1. Confirm Password: **Pa55w.rd.123456789**

    **Firewall settings:**

      a. Public inbound ports: select Allow selected ports

      b. Select RDP from the Select inbound ports dropdown.

10. Select **Next: Disks**

    Review the Disks settings, accept without any changes.

1. Select **Next: Networking**

    Review the Networking settings, accept without any changes.

1. Select **Next: Management**

    Review the Management settings, and change Boot Diagnostics:

    a. Set Boot Diagnostics to **Off**

13. Select **Next: Advanced**
1. Skip the Advanced tab and select **SQL Server settings**

    On the SQL Server settings tab complete the following information:

    1. SQL connectivity: **select Public (Internet)**
    1. SQL Authentication: **Select Enable**
    1. Type **Pa55w.rd.123456789** in the Password box

15. Select **Review + create**

    Review the settings on Review + create tab

    a. Select **Create** to initiate the Virtual Machine (VM) creation.

    **Note:** This step could take about 10 minutes to complete

16.	Upon completion of the VM creation, open the Virtual Machine blade.
17.	 Select the **sql2017vm** you created.
18.	Locate the Public IP Address and write it down for future connectivity
19.	Select **Configure DNS name**
20.	Type a uniquely identifiable DNS name and write down full dns name you specified.

    For example: SQL2017VMxxxx.centralus.cloudapp.azure.com

21.	Click save

``` shell
Results: After you completed this exercise you have a SQL Server 2017 instance running in an Azure Virtual Machine.
```

## Exercise 2: Create an Azure Storage Account and Fileshare

In this exercise, you will create a new virtual machine on Azure, using the Azure Portal.

**Estimated Time:** 15 Minutes

The main task for this exercise is as follows:

1. Create an Azure Storage Account
2. Create a fileshare on the azure Storage Account

### Task 1: Create an Azure Storage Account

1. In the [Azure portal](https://portal.azure.com), select the **Storage Accounts** blade.
2. Select **Add**.
3. In the Create storage account window complete the following information:

    1. Resource Group: Select **Existing**
    1. select the **DP-050-Training** resource group you created in the previous exercise
    1. Storage account name: **dp050storagexxxx** (where xxxx) is a random number of characters
    1. Location: select the location closest to where you created the virtual machine in the previous exercise.
    1. Leave other default settings as is.

1. Skip the Advanced and Tags section by clicking on **Review + create**
1. Select **Create**

    **Note:**  this deployment could take a few minutes

6. Upon Completion, click **go To Resource**

### Task 2: Create an Azure FileShare

1. On the storage account page, select **Files**
2. On the Files pages, select **+ File share**
3. Complete the following information:
    a. Name: **backupshare**
    b. Quota: **200 Gib**
4. Select **Create**
5. After creation of the file share, select … at the right side of the fileshare created
6. Select **Connect** from the dropdown list
7. In the Connect blade, select drive letter **U:**
8. Copy the connection command syntax from the text listed under Alternatively, run this command if they key doesn’t begin with a forward slash:

The text will look like as the command syntax below:

```shell
cmdkey /add:dp050storagexxxx.file.core.windows.net /user:Azure\dp050storagexxxx /pass:Hcty8OXn7jcON/ePk3OvswD7eRusfWWAw9lakhX9P9m4MnuqZEt2I8pDYAEiyAZJx4Na39UZEwAyH8PlnVa36Q==

```

9. Open **Notepad**
10. Paste the content of the above command and save the file into the **Labfiles** folder as **MapNetworkdrive.txt**
11. Close **Notepad**.
12.	You can now close the Azure Portal  Notepad.

```shell
Results: You have now successful created an Azure Fileshare which will be used as a shared access location for SQL Server database backup files. In the next exercise you will configure the SQL instances to be able to access the shared locations.
```

## Exercise 3: Create a connection for the SQL Server instances to connect to the Azure Fileshare

In this exercise, you will configure the SQL Server environment to be able to access the Azure Fileshare
**Estimated Time:** 10 Minutes

The main task for this exercise is as follows:

1. Register the fileshare through SQL Management Studio by mapping a network drive
2. Create a fileshare on the azure Storage Account

### Task 1: Register the server instances in SQL Management Studio

1. In the SQL Server 2008 R2 lab environment, Start SQL Management Studio
2. Connect to the local instance (LONDON)
3. In SQL Management Studio Object Explorer, select **Connect | Database Engine** and complete the following information in the **Connect to Server** dialog box:
    Servername: 'the full qualified domain name of your SQL 2017 VM'
    For example sql2017vmxxxx.centralus.cloudapp.azure.com
4. Select SQL Server Authentication and provide the following user and password
    Login: **sqladmin**
    Password: **Pa55w.rd.123456789**

### Task 2: Connect the SQL instances to the fileshares

**Note:** In order for SQL Server to be able to connect to a drive letter residing on a file share, you have to map the network drive running xp_cmdshell. Because of security reasons, command line access should be limited for SQL Server service accounts. By default SQL command line is disabled.

1. Open the MapNetworkdrive.txt you created in the previous part of the exercise in Notepad.
2.	Copy the text
3.	In SQL Management studio, create a new query while connected to the local server (LONDON)
4.	Paste the text from the MapNetworkdrive into the query window
5.	Change the query to reflect the following screenshot and run the command lines through the xp_cmdshell stored procedure.

```sql
SP_CONFIGURE 'show advanced options', 1
RECONFIGURE
SP_CONFIGURE 'xp_cmdshell', 1
RECONFIGURE
GO
EXEC xp_cmdshell 'cmdkey /add:dp050storagexxxx.file.core.windows.net /user:Azure\dp050storagexxxx /pass:Mcty80xn7j'
EXEC xp_cmdshell 'net use U: \\dp050storagexxxx.file.core.windows.net\backupshare /persistent:Yes'
EXEC xp_cmdshell 'dir u:'
```

6.	Run the query and validate that the networkdrive is accessible
7.	Save the query in the Labfiles folder as **MapNetworkdrive.sql**
8.	Start a new query window and disable xp_cmdshell by running the following query:

```sql
    SP_CONFIGURE ‘xp_cmdshell’,0
```

9.	In Object Explorer, select the Azure VM SQL Server instance and create a new query
10.	Open the saved file Mapnetworkdrive.sql and run the script in the SQLS Server 2017 instance
11.	Repeat the step to disable xp_cmdshell in the SQL Server 2017 instance.


## Exercise 4: Perform a Database Migration using SQL Server Data 

In this exercise, you will create a new virtual machine on Azure, using the Azure Portal.
**Estimated Time:** 10 Minutes

The main task for this exercise is as follows:

1. Migrate databases Data Migration Assistant
2. Validate a successful migration of the database

### Task 1: Migrate SQL Databases using Data Migration Assistant

1.	In the SQL Server 2008 R2 lab environment, open the application **“Microsoft Data Migration Assistant”**
2.	Select **+**, this will open the dialog for a new project and type in the following information:
    1. Project Type: **Migration**
    1. Project Name: **Migration to SQL VM**
    1. Source server type: **SQL Server**
    1. Target server type: **SQL Server on Azure Virtual Machines**
3.	Click **Create**
4.	Click **Next**
5.	In the source server details Servername dialog box, enter the **Server name of localhost**
6.	In the target server details Servername dialog box, enter the Server name of  your Azure SQL Virtual machine using its full qualified name
7.	In the target server Authentication Type dialog box, select **SQL Server Authentication**
8.	In the Target Server details SQL Authentication credentials, type **sqladmin** and provide **Pa55.w.rd.123456789** as the password.
9.	In Connection Properties, uncheck the **Encrypt Connection** properties.
10.	Click **Next**.
11.	Unselect the following databases:
    •	AdventureworksDW2008_4M
    •	Reportserver
    •	ReportServerTempDB
12.	In the Shared Location Dialog box type: U:\
13.	Review the select Logins window
14.	Click **StartMigration**

**Note:** All databases will be backing up to the shared network drive (Azure Fileshare)

15.	Monitor the migration process.
16.	Upon completion, close Data Migration Assistant

### Task 2: Validate a successful Migration

1.	In the SQL Server 2008 R2 lab environment, open **“SQL Management Studio”**
2.	In Object Explorer expand the **Databases** list on the **SQL Server 2017 instance**
3.	Validate that the databases have been successfully migrated
4.	Create a **New Query**
5.	Type and execute the following query to validate the database compatibility level of each of the databases.

```sql
SELECT name, compatibility_level FROM sys.databases
```

6.	Review the query results
7.	Alter the database compatibility level for the **AdventureworksLT2008R2** database using the following query:

```sql
ALTER DATABASE AdventureWorksLT2008R2
SET COMPATIBILITY_LEVEL = 110;
GO
```

8.	Backup the AdventureworksLT2008R2 database using the following query:
  
```sql
BACKUP DATABASE AdventureworksLT2008R2  
TO DISK = 'U:\AdventureworksLT2008R2'  
   WITH FORMAT,  
      MEDIANAME = 'AdventureworksLT2008R2',  
      NAME = 'Full Backup of AdventureworksLT2008R2;  
```


9.	Upon successful completion of the backup close **SQL Management Studio.**

```shell
Results: You have now completed the successful migration of SQL 2008R2 Databases to SQL Server 2017 running in an Azure VM.
```
