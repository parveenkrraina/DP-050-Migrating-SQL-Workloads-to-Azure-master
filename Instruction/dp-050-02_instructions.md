# DP 050 - Migrating SQL Workloads to Azure
# Lab 2 - Choose the right tools for Data Migration

**Estimated Time**: 45 minutes

**Pre-requisites**: There are no pre-requisite steps to perform this lab.

**Lab files**: There are no labfiles for this lab

## Lab overview

The students will use two tools in the prescribed Data Platform Modernization stages to perform a discovery of the environment in an automated way. They will also Identify any pre migration compatibility issues and define a plan on how to address the issues before performing a migration of an on-premises server. Finally, they will assess how a workload will perform on a target version of Azure SQL Database.

## Lab objectives
  
After completing this lab, you will be able to:

1. Build your Data Estate Inventory using Map Toolkit 
2. Identify Migration candidates using Data Migration Assistant

## Scenario
  
You are the senior data engineer of AdventureWorks and are preparing for a Data Modernization project. You will begin by performing an inventory of the SQL Servers that exist in your environment using a tool. You will then perform an assessment of the server to identify any issues that need to be resolved prior to any modernization work that will be undertaken. One of your on-premises SQL Servers is used to process sales for the business and you want to ensure that your workloads can be handled the target service that you migrate to.

## Exercise 1: Build your Data Estate Inventory using Map Toolkit

In this lab, you will  use the Microsoft Assessment and Planning Toolkit to collect information about a SQL Server instance running on an on-premises server with SQL Server 2008 R2.

Estimated Time: 25 Minutes

The main task for this exercise is as follows:

1. Installing the Microsoft Assessment and Planning Toolkit
1. Creating the MAPS inventory database
1. Enable remote administration
1. Collecting Inventory Data
1. View Microsoft SQL Server Discovery information in MAPS
1. Create SQL Server Reports

### Task 1: Installing the Microsoft Assessment and Planning Toolkit

1. Open up your web browser and go to the [Microsoft Assessment and Planning Toolkit download site](https://www.microsoft.com/en-us/download/details.aspx?id=7826)
1. Click on **Download** and save the file to a local folder.
1. When the download is completed, browse to the folder and double click **MapSetup.exe**.
1. In the Welcome to the Setup Wizard for Microsoft Assessment and Planning Toolkit, click on **Next**.
1. In the License Agreement screen, click on the checkbox next to **I accept the terms in the License Agreement** and then click on **Next**.
1. In the installation Folder screen, click on **Next**.
1. In the Begin the installation screen, click on **Install**.
1. In the Completed the Setup Wizard for Microsoft Assessment and Planning Toolkit, click on **Finish**.

### Task 2: Creating the MAPS inventory database

1. On the windows desktop, click on **Start**, and type Microsoft Assessment, then click on **Microsoft Assessment and Planning Toolkit**
1. Click **Yes** to accept the User Account Control (UAC) dialog to run this application.
1. In the Create or Select a Database dialog, click **Create an inventory database** and type **MapsInventory** as a new database name and click **OK**.

### Task 3: Enable remote administration

1. Navigate to the **command prompt** by going to **Start** → type **cmd**, right-click **Command Prompt**, then select **Run As Administrator**.
1. Type in the following command, then hit **Enter**:
**netsh advfirewall set currentprofile settings remotemanagement enable**

### Task 4: Collecting Inventory Data

1. If it is not already open, launch the MAP Toolkit. You may want to resize the application to full screen.
1. Click **Yes** to accept the User Account Control (UAC) dialog to run this application.
1. Under the **Where to Start** section, click **Perform an Inventory** to launch the inventory wizard.
1. In the **Inventory Scenarios** page, ensure that the **SQL Server** checkbox is selected, then click **Next**.
1. In the Discovery Methods page, uncheck **Use Active Directory Domain Services (AD DS)**.
1. Check **Manually enter computer names and credentials** then click **Next**.
1. In All Computer Credentials page, click **Create**.
1. In the Account Entry dialog, type the **username** for the local computer user (example: Administrator) in the Account Name field
1. In both the **Password** and **Confirm Password** fields, type the password for the local computer user (example: password) then click **Save**.
1. In the All Computer Credentials Page, click **Next**.
1. Click **Next** in the Credentials Order page. 
1. In the Enter Computers Manually Page, click **Create**.
1. In the Specify Computers and Credentials page, type the name of the local computer (example: MAP-HOL) as the computer name and click **Add**.
1. Check **Use All Computers Credential list** and click **Save**.
1. Click **Next** and review the information displayed in the Summary page.
1. Click **Finish** to launch a status dialog and the inventory process.
Note: If you are using MAP on a virtual machine, even though you have specified that you want to inventory just one machine, the MAP Toolkit detects that it is a virtual machine and attempts to inventory the VM host as well. In this case the specified credentials may not allow such an inventory to be performed.
1. When it finishes, click **Close** on the status dialog.

### Task 5: View Microsoft SQL Server Discovery information in MAPS

1.  In **MAPS**, Select **File** → **Create/Select a Database...** from the main menu to launch the **Create or select a database to use** dialog.
1.  Click **Use an Existing Database**, select **MapsInventory** from the database menu, and click **OK**.
1.  Click the **Database scenario** group in the left pane of the UI, then click the **SQL Server Discovery** tile.
1.  Inspect the scenario detail page and observe the pie chart with SQL Server information.  

### Task 6: Create SQL Server Reports

1. In the SQL Server Discovery screen, under the Options section, Click **Generate SQL Server Database Details Reports** at the top of the scenario detail page to start generating the reports and to launch a status dialog.
1. After the status dialog reports that the generation has completed, ensure that open reports folder after close is selected and then click **Close**.
1. Open the **SQLServerDatabaseDetails-<date-and-time>** Excel documents.
1. In the SQLServerAssessment Excel report, view the **SQLServerSummary** tab to see how many SQL Server database components were found in the network in total. Close the spreadsheet
1. In the SQL Server Discovery screen, under the Options section, Click **Generate SQL Server Assessment Reports** at the top of the scenario detail page to start generating the reports and to launch a status dialog.
1. After the status dialog reports that the generation has completed, ensure that open reports folder after close is selected and then click **Close**.
1. Open the **SQLServerDatabaseAssessment-<date-and-time>** Excel documents
1. View the Database Instances tab to see all database instances listed with server details including if the server is virtualized (Machine Type).
1. View the Components tab to see all installed database components listed with server details.
1. After viewing close reports and file browser.

> **Result**: After you completed this exercise, you have run the Microsoft Assessment and Planning Toolkit to collect information about a SQL Server instance

## Exercise 2: Identify Migration candidates using Data Migration Assistant
  
In this exercise, you will use the Data Migration Assistant to look for SQL Server feature parity and compatibility issues between SQL Server and Azure SQL Database

Estimated Time: 20 Minutes
  
The main task for this exercise is as follows:

1. How to install the Data Migration Assistant (DMA) on Windows
1. Use the Data Migration Assistant.

### Task 1: Install the Data Migration Assistant (DMA) on Windows

> **Note**: If Data Migration Assistant is already installed, you can skip this task.

1. Go to [https://www.microsoft.com/en-us/download/details.aspx?id=53595](https://www.microsoft.com/en-us/download/details.aspx?id=53595)
1. Confirm that your environment supports the software by checking the required System Requirements.
1. Download **DataMigrationAssistant.msi** and then click **Install**:

    1. Double-Click the installer **DataMigrationAssistant.msi**
    2. Click **Next** on the Welcome screen.
    3. Read the User License Agreement and accept if appropriate and then click **Next**.
    4. Read the Privacy Statement and then click **Install**.
    5. If prompted allow UAC control for this application. Complete install by clicking **Finish**.

### Task 2: Use the Data Migration Assistant.

1. Open the application "Microsoft Data Migration Assistant"

    1. Select **+**, this will open the dialog for a new project and type in the following information:

        1. Project Type: **Assessment**
        2. Project Name: **Migration Assessment SQL DB**
        3. Source server type: **SQL Server**
        4. Target server type: **Azure SQL Database**
        5. Click **Create**
        6. Click **Next**
        7. In the **Connect to a Server**dialog box, enter the  **Server name** of **localhost**
        8. Click **Connect**
        9. Click all databases you wish to assess, **Add**
        10. Click **Start Assessment**

1. Review the SQL Server feature parity and compatibility issues found. Note how many Feature Parity issue there are.

1. Close down the Data Migration Assistant. Replay the Microsoft Data Migration Assistant using Azure SQL Database Managed Instance.

    1. Select **+**, this will open the dialog for a new project and type in the following information:
        
        1. Project Type: **Assessment**
        2. Project Name: **Migration Assessment SQL DB MI**
        3. Source server type: **SQL Server**
        4. Target server type: **Azure SQL Database Managed Instance**
        5. Click **Create**
        6. Click **Next**
        7. In the **Connect to a Server**dialog box, enter the  **Server name** of **localhost**
        8. Click **Connect**
        9. Click all databases you wish to assess, **Add**
        10. Click **Start Assessment**

1. Review the SQL Server feature parity and compatibility issues found. Note how many Feature Parity issue there are.

1. Close down the Data Migration Assistant. Replay the Microsoft Data Migration Assistant using SQL Server on Azure Virtual Machines.

    1. Select **+**, this will open the dialog for a new project and type in the following information:
    
        1. Project Type: **Assessment**
        2. Project Name: **Migration Assessment Azure VM**
        3. Source server type: **SQL Server**
        4. Target server type: **Azure SQL Database Managed Instance**
        5. Click **Create**
        6. Click **Next**
        7. Enter **Server name**: **localhost**
        8. Click **Connect**
        9. Click all databases you wish to assess, **Add**
        10. Click **Start Assessment**

1. Review the SQL Server feature parity and compatibility issues found. Note how many Feature Parity issue there are.

> **Result**: After you completed this exercise, you have collected information about the SQL Server feature parity and compatibility issues found between the instance of SQL Server and Azure SQL Database.

## Lab Review

After approximately 45 minutes, the instructor will bring a close to this lab. The class will discuss the findings of each group.

