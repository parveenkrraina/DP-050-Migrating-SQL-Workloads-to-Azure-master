# DP 050 - Migrating SQL Workloads to Azure
# Lab 1 - Azure Architecture Considerations

**Estimated Time**: 50 minutes

**Pre-requisites**: There are no pre-requisite steps to perform this lab.

**Lab files**: The files for this lab are in the _Allfiles\Labfiles\Starter\DP-150.1_ folder.

## Lab overview

The students will use the information gained in this module to demonstrate an understanding of Data Platform modernization, and why and how organizations should undertake a modernization project at a high level. They will demonstrate how to determine the cost of moving to Azure. They will also gather information about the environment that they are working on. Students will finally determine a data platform modernization strategy that should be used for a given scenario.

## Lab objectives
  
After completing this lab, you will be able to:

1. Understand Data Platform Modernization
2. Understand the Stages of Migration
3. Data Migration Technologies

## Scenario
  
You have recently been hired as a senior data engineer at AdventureWorks and are working with a consultant and architects to initiate a Data Platform Modernization project that meets the organizations technical and business requirements. You will demonstrate an example of how they can calculate the Total Cost of Ownership of migrating to Azure. You then ask a Junior Data Engineer to collect information about the SQL Server that will be migrated to Azure. Then for a given scenario, you will advise the best Azure Data Platform Technology to migrate to for a given scenario.

At the end of this lab, you will have:

1. Understand Data Platform Modernization
2. Understand the Stages of Migration
3. Data Migration Technologies

## Exercise 1: Understand Data Platform Modernization

In this lab, you will demonstrate how you can calculate the costs for a Databases

Estimated Time: 15 Minutes

The main task for this exercise is as follows:

1. Open the Microsoft Total Cost of Ownership (TCO) Calculator
1. Define a Server based workload
1. Add some storage
1. Add some networking
1. Adjust the currency
1. Adjust the Electricity costs
1. Adjust the Other Assumptions
1. Compare the on-premises and Azure Costs

### Task 1: Open the Total Cost of Ownership (TCO) Calculator

1. Open your Browser
1. navigate to [Azure TCO Calculator](https://azure.microsoft.com/en-us/pricing/tco/calculator/)
1. from the Define workloads section ensure that there are no workloads in the **Servers** Section

### Task 2: Enter the Database workload

1. In the **Databases** workload section, click on the **+ Add database** hyperlink.
1. Name the workload **Accounting**
1. Enter the following under **Source**
   - Database **Microsoft SQL Server**
   - License **Enterprise**
   - Environment **Physical Servers**
   - Operating System **Windows**
   - Servers **1**
   - Procs per server **1**
   - Core(s) per proc **4**
   - RAM (GB) **64**
   - Optimize by **CPU**
   - SQL Server 2008/2008R2 **True**
1. Enter the following under **Destination**
   - Service **SQL Server VM**
   - Disk Type **SSD**

    > This is the recommendation for production servers in Azure
   - IOPS **5000**
   - SQL Server storage **32 GB**
   - SQL Server Backup **32 GB**

### Task 3: Enter the Storage workload

1. In the **Storage** workload section
2. Name the workload **Accounting Local Disks**
   - Storage Type **Local Disk/SAN**
   - Disk Type **HDD**
   - Capacity **3TB**
   - Backup **1TB**
   - Archive **0TB**

### Task 4: Enter the Networking workload

1. In the **Networking** workload section
   - Outbound Bandwidth **Leave as default**


1. Scroll down the web page and then click on the blue button marked **Next**

### Task 5: Adjust currency

1. In the **Adjust assumptions** section ensure that the Currency is set to your home currency

### Task 6: Software Assurance

1. in the **Software Assurance coverage (provides Azure Hybrid Benefit)** section
   - Windows Server Software Assurance coverage **True**
   - SQL Server Software Assurance coverage **True**
   - Click on the links and investigate the content

### Task 7: Geo-redundant storage (GRS)

1. in the **Geo-redundant storage (GRS)** section
   - GRS replicates your data to a secondary region that is hundreds of miles away from the primary region. **Leave as default**
   - Click on the links and investigate the content

### Task 8: Virtual Machine costs

1. in the **Virtual Machine costs** section
   - Enable this for the Calculator to not recommend B-series virtual machines **Leave as default**
   - Click on the links and investigate the content

### Task 9: Electricity costs

1. in the **Geo-redundant storage (GRS)** section
   - price per kw hour **enter a realistic value if known**

     > **NOTE** Global prices for electricity in 2018 can be found here [Global electricity prices in 2018](https://www.statista.com/statistics/263492/electricity-prices-in-selected-countries/). These prices are in USD ($) and will need converting to your home currency

### Task 10: Storage costs

1. in the **Storage costs** section
    - Storage procurement cost/GB for local disk/SAN-SSD **Leave as default**
    - Storage procurement cost/GB for local disk/SAN-HDD **Leave as default**
    - Storage procurement cost/GB for NAS/file storage **Leave as default**
    - Storage procurement cost/GB for Blob storage **Leave as default**
    - Annual enterprise storage software support cost **Leave as default**
    - Cost per tape drive **Leave as default**

### Task 11: IT labor costs

1. in the **IT labor costs** section
    - Number of physical servers that can be managed by a full-time administrator **Leave as default**
    - Number of virtual machines that can be managed by a full-time administrator **Leave as default**
    - Hourly rate for IT administrator **Leave as default**

### Task 12: Other assumptions

1. in the **Other assumption** section
   - Expand each section and look at the associated costs

### Task 13: The Report (1 year)

1. Click **Next**
1. Note at the top (under "View report") that the Timeframe defaults to 5 year
1. scroll down to the report and investigate the estimated breakdown of costs for on-premises vs Azure
1. Where is the majority of the spend for on-Premises?
1. Investigate where the largest cost saving is
1. Expand each section in turn and investigate the breakdown of costs

### Task 14: The Report (3 year)

1. scroll to the top of the page
1. under "View report"
   - Timeframe **3 years**
1. scroll down to the report and investigate the estimated breakdown of costs for on-premises vs Azure
1. How much is the total spend over 5 years?
1. Where is the majority of the spend for on-Premises?
1. Expand each section in turn and investigate the breakdown of costs

> **Result**: After you completed this exercise, you have used the Azure TCO calculator to identify cost differences between on-Premises and Azure deployments for Adatum Corporation's Accounting server and its associated databases

## Exercise 2: Understand the Stages of Migration
  
In this exercise, you will perform a discovery of the server that you are working with to understand the specs of the machine.

Estimated Time: 15 Minutes
  
The main task for this exercise is as follows:

1. Determine the amount of memory that is being used 
1. To determine the CPU that is being used in the system
1. Determine the disk configurations on the server

### Task 1: Determine the amount of memory that is being used on the server.

1. Choose any method of your choice to determine how much memory is being used on the server. 

> **Hint**: Use the System information tools to capture this information

1. Make a note of the value in Notepad.

### Task 2: Determine the amount of CPU that is being used on the server.

1. Choose any method of your choice to determine how much memory is being used on the server.

1. Make a note of the value in Notepad.

### Task 3: Determine the disk configuration that is being used on the server.

1. Choose any method of your choice to determine how much memory is being used on the server.

1. Make a note of the value in Notepad.

1. Save the Notepad into the _Allfiles\Labfiles\Starter\DP-150.1_ folder with the name of ServerSpecs.txt

> **Result**: After you completed this exercise, you have collected information on the hardware specs of the server.

## Exercise 3: Data Migration Technologies

In this exercise, you will select a data platform technology to help a customer migrate from an on-premises SQL Server to Azure 

Estimated Time: 15 Minutes 

The main task for this exercise is as follows:

1. Identify the database technology required to facilitate a migration in scenario A.
1. Identify the database technology required to facilitate a migration in scenario B.

### Task 1: Identify the database technology required to facilitate a migration in scenario A.

1. Read the following scenario

    The customer has an application that uses many databases currently residing in an on-premises version of Microsoft SQL Server 2008. The total database footprint is large, and rapidly growing by several terabytes per year. The existing SAN storage that the databases are located on is almost at capacity, expensive to expand, and nearing the end of its life. The application is critical to the company, with a moderate transaction rate, and any downtime would have significant business impact. Small maintenance windows are available in which to make changes to maximize the availability of the application. The high growth rate has seen more and more time being spent by DBAs and sysadmins just to keep everything running

1. From the scenario, which Database Platform Technology would be appropriate?

### Task 2: Identify the database technology required to facilitate a migration in scenario B.

1. Read the following scenario

    The customer in this example has a SQL Server that stores databases that fulfil departmental needs. The server on which the databases are host is a quad core server, with 16GB of memory and is used as a backend to simple data access for spreadsheets and an Access form. There is a total of 6 databases that takes up 350 MB in total. The maximum number of concurrent connection to this server is 12.

1. From the scenario, which Database Platform Technology would be appropriate?

> **Result**: After you completed this exercise, you will have identified the appropriate database technologies to migrate to base on.

## Lab Review

After approximately 45 minutes, the instructor will bring a close to this lab. The class will discuss the findings of each group.

