---
title: Create an Azure data factory using the Azure Data Factory UI | Microsoft Docs
description: 'This tutorial shows you how to create a data factory with a pipeline that copies data from one folder to another folder in Azure Blob Storage.'
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: spelluru

ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 02/01/2018
ms.author: jingwang

---
# Create a data factory by using the Azure Data Factory UI
> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Version 1 - GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 - Preview](quickstart-create-data-factory-portal.md)

This quickstart describes how to use the Azure Data Factory UI to create and monitor a data factory. The pipeline that you create in this data factory *copies* data from one folder to another folder in Azure Blob storage. For a tutorial on how to *transform* data by using Azure Data Factory, see [Tutorial: Transform data by using Spark](tutorial-transform-data-spark-portal.md). 


> [!NOTE]
> If you are new to Azure Data Factory, see [Introduction to Azure Data Factory](data-factory-introduction.md) before doing this quickstart. 
>
> This article applies to version 2 of Data Factory, which is currently in preview. If you are using version 1 of the service, which is in general availability (GA), see the [Data Factory version 1 tutorial](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### Video 
Watching this video helps you understand the Data Factory UI: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## Create a data factory

1. Launch **Microsoft Edge** or **Google Chrome** web browser. Currently, Data Factory UI is supported only in Microsoft Edge and Google Chrome web browsers.
2. Go to the [Azure portal](https://portal.azure.com). 
3. Select **New** on the left menu, select **Data + Analytics**, and then select **Data Factory**. 
   
   ![Data Factory selection in the "New" pane](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. On the **New data factory** page, enter **ADFTutorialDataFactory** for **Name**. 
      
   !["New data factory" page](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   The name of the Azure data factory must be *globally unique*. If you see the following error, change the name of the data factory (for example, **&lt;yourname&gt;ADFTutorialDataFactory**) and try creating again. For naming rules for Data Factory artifacts, see the [Data Factory - naming rules](naming-rules.md) article.
  
   ![Error when a name is not available](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. For **Subscription**, select your Azure subscription in which you want to create the data factory. 
4. For **Resource Group**, use one of the following steps:
     
   - Select **Use existing**, and select an existing resource group from the list. 
   - Select **Create new**, and enter the name of a resource group.   
         
   To learn about resource groups, see [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md).  
4. For **Version**, select **V2 (Preview)**.
5. For **Location**, select the location for the data factory.

   The list shows only locations that Data Factory supports. The data stores (like Azure Storage and Azure SQL Database) and computes (like Azure HDInsight) that Data Factory uses can be in other locations.
6. Select **Pin to dashboard**.     
7. Select **Create**.
8. On the dashboard, you see the following tile with the status **Deploying Data Factory**: 

   !["Deploying Data Factory" tile](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
9. After the creation is complete, you see the **Data Factory** page. Select the **Author & Monitor** tile to start the Azure Data Factory user interface (UI) application on a separate tab.
   
   ![Home page for the data factory, with the "Author & Monitor" tile](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
10. On the **Let's get started** page, switch to the **Edit** tab in the left panel. 

    !["Let's get started" page](./media/quickstart-create-data-factory-portal/get-started-page.png)

## Create a linked service
In this procedure, you create a linked service to link your Azure storage account to the data factory. The linked service has the connection information that the Data Factory service uses at runtime to connect to it.

1. Select **Connections**, and then select the **New** button on the toolbar. 

   ![Buttons for creating a new connection](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
2. On the **New Linked Service** page, select **Azure Blob Storage**, and then select **Continue**. 

   ![Selecting the "Azure Blob Storage" tile](./media/quickstart-create-data-factory-portal/select-azure-storage.png)
3. Complete the following steps: 

   a. For **Name**, enter **AzureStorageLinkedService**.

   b. For **Storage account name**, select the name of your Azure storage account.

   c. Select **Test connection** to confirm that the Data Factory service can connect to the storage account. 

   d. Select **Save** to save the linked service. 

   ![Azure Storage linked service settings](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 
4. Confirm that you see **AzureStorageLinkedService** in the list of linked services. 

   ![Azure Storage linked service in the list](./media/quickstart-create-data-factory-portal/azure-storage-linked-service-in-list.png)

## Create datasets
In this procedure, you create two datasets: **InputDataset** and **OutputDataset**. These datasets are of type **AzureBlob**. They refer to the Azure Storage linked service that you created in the previous section. 

The input dataset represents the source data in the input folder. In the input dataset definition, you specify the blob container (**adftutorial**), the folder (**input**), and the file (**emp.txt**) that contain the source data. 

The output dataset represents the data that's copied to the destination. In the output dataset definition, you specify the blob container (**adftutorial**), the folder (**output**), and the file to which the data is copied. Each run of a pipeline has a unique ID associated with it. You can access this ID by using the system variable **RunId**. The name of the output file is dynamically evaluated based on the run ID of the pipeline.   

In the linked service settings, you specified the Azure storage account that contains the source data. In the source dataset settings, you specify where exactly the source data resides (blob container, folder, and file). In the sink dataset settings, you specify where the data is copied to (blob container, folder, and file). 
 
1. Select the **+** (plus) button, and then select **Dataset**.

   ![Menu for creating a dataset](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
2. On the **New Dataset** page, select **Azure Blob Storage**, and then select **Finish**. 

   ![Selecting "Azure Blob Storage"](./media/quickstart-create-data-factory-portal/select-azure-blob-storage.png)
3. In the **Properties** window for the dataset, enter **InputDataset** for **Name**. 

   ![Dataset general settings](./media/quickstart-create-data-factory-portal/dataset-general-page.png)
4. Switch to the **Connection** tab and complete the following steps: 

   a. For **Linked service**, select **AzureStorageLinkedService**.

   b. For **File path**, select the **Browse** button.

      !["Connection" tab and "Browse" button](./media/quickstart-create-data-factory-portal/file-path-browse-button.png)
   c. In the **Choose a file or folder** window, browse to the **input** folder in the **adftutorial** container, select the **emp.txt** file, and then select **Finish**.

      ![Browse for the input file](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    
   d. (optional) Select **Preview data** to preview the data in the emp.txt file.     
5. Repeat the steps to create the output dataset:  

   a. Select the **+** (plus) button, and then select **Dataset**.

   b. On the **New Dataset** page, select **Azure Blob Storage**, and then select **Finish**.

   c. Specify **OutputDataset** for the name.

   d. Enter **adftutorial/output** for the folder. If the **output** folder does not exist, the copy activity creates it at runtime.

   e. Enter `@CONCAT(pipeline().RunId, '.txt')` for the file name. 
   
      Each time you run a pipeline, the pipeline run has a unique ID associated with it. The expression concatenates the run ID of the pipeline with **.txt** to evaluate the output file name. For the supported list of system variables and expressions, see [System variables](control-flow-system-variables.md) and [Expression language](control-flow-expression-language-functions.md).

   ![Output dataset settings](./media/quickstart-create-data-factory-portal/output-dataset-settings.png)

## Create a pipeline 
In this procedure, you create and validate a pipeline with a copy activity that uses the input and output datasets. The copy activity copies data from the file you specified in the input dataset settings to the file you specified in the output dataset settings. If the input dataset specifies only a folder (not the file name), the copy activity copies all the files in the source folder to the destination. 

1. Select the **+** (plus) button, and then select **Pipeline**. 

   ![Menu for creating a new pipeline](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
2. In the **Properties** window, specify **CopyPipeline** for **Name**. 

   ![Pipeline general settings](./media/quickstart-create-data-factory-portal/pipeline-general-settings.png)
3. In the **Activities** toolbox, expand **Data Flow**. Drag the **Copy** activity from the **Activities** toolbox to the pipeline designer surface. You can also search for activities in the **Activities** toolbox. Specify **CopyFromBlobToBlob** for **Name**.

   ![Copy activity general settings](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
4. Switch to the **Source** tab in the copy activity settings, and select **InputDataset** for **Source Dataset**.

   ![Copy activity source settings](./media/quickstart-create-data-factory-portal/copy-activity-source-settings.png)    
5. Switch to the **Sink** tab in the copy activity settings, and select **OutputDataset** for **Sink Dataset**.

   ![Copy activity sink settings](./media/quickstart-create-data-factory-portal/copy-activity-sink-settings.png)    
7. Select **Validate** to validate the pipeline settings. Confirm that the pipeline has been successfully validated. To close the validation output, select the **>>** (right arrow) button. 

   ![Validate the pipeline](./media/quickstart-create-data-factory-portal/pipeline-validate-button.png)

## Test run the pipeline
In this step, you test run the pipeline before deploying it to Data Factory. 

1. On the toolbar for the pipeline, select **Test Run**. 
    
   ![Pipeline test runs](./media/quickstart-create-data-factory-portal/pipeline-test-run.png)
2. Confirm that you see the status of the pipeline run on the **Output** tab of the pipeline settings. 

   ![Test run output](./media/quickstart-create-data-factory-portal/test-run-output.png)    
3. Confirm that you see an output file in the **output** folder of the **adftutorial** container. If the output folder does not exist, the Data Factory service automatically creates it. 
    
   ![Verify output](./media/quickstart-create-data-factory-portal/verify-output.png)


## Trigger the pipeline manually
In this procedure, you deploy entities (linked services, datasets, pipelines) to Azure Data Factory. Then, you manually trigger a pipeline run. You can also publish entities to your own Visual Studio Team Services Git repository, which is covered in [another tutorial](tutorial-copy-data-portal.md?#configure-code-repository).

1. Before you trigger a pipeline, you must publish entities to Data Factory. To publish, select **Publish All** in the left pane. 

   ![Publish button](./media/quickstart-create-data-factory-portal/publish-button.png)
2. To trigger the pipeline manually, select **Trigger** on the toolbar, and then select **Trigger Now**. 
    
   !["Trigger Now" command](./media/quickstart-create-data-factory-portal/pipeline-trigger-now.png)

## Monitor the pipeline

1. Switch to the **Monitor** tab on the left. Use the **Refresh** button to refresh the list.

   ![Tab for monitoring pipeline runs, with "Refresh" button](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
2. Select the **View Activity Runs** link under **Actions**. You see the status of the copy activity run on this page. 

   ![Pipeline activity runs](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
3. To view details about the copy operation, select the **Details** (eyeglasses image) link in the **Actions** column. For details about the properties, see [Copy Activity overview](copy-activity-overview.md). 

   ![Copy operation details](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
4. Confirm that you see a new file in the **output** folder. 
5. You can switch back to the **Pipeline Runs** view from the **Activity Runs** view by selecting the **Pipelines** link. 

## Trigger the pipeline on a schedule
This procedure is optional in this tutorial. You can create a *scheduler trigger* to schedule the pipeline to run periodically (hourly, daily, and so on). In this procedure, you create a trigger to run every minute until the end date and time that you specify. 

1. Switch to the **Edit** tab. 

   ![Edit button](./media/quickstart-create-data-factory-portal/switch-edit-tab.png)
1. Select **Trigger** on the menu, and then select **New/Edit**. 

   ![Menu for new trigger](./media/quickstart-create-data-factory-portal/new-trigger-menu.png)
2. On the **Add Triggers** page, select **Choose trigger**, and then select **New**. 

   ![Selections for adding a new trigger](./media/quickstart-create-data-factory-portal/add-trigger-new-button.png)
3. On the **New Trigger** page, under **End**, select **On Date**, specify an end time a few minutes after the current time, and then select **Apply**. 

   A cost is associated with each pipeline run, so specify the end time only minutes apart from the start time. Ensure that it's the same day. However, ensure that there is enough time for the pipeline to run between the publish time and the end time. The trigger comes into effect only after you publish the solution to Data Factory, not when you save the trigger in the UI. 

   ![Trigger settings](./media/quickstart-create-data-factory-portal/trigger-settings.png)
4. On the **New Trigger** page, select the **Activated** check box, and then select **Next**. 

   !["Activated" check box and "Next" button](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
5. Review the warning message, and select **Finish**.

   ![Warning and "Finish" button](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
6. Select **Publish All** to publish changes to Data Factory. 

   ![Publish button](./media/quickstart-create-data-factory-portal/publish-button.png)
8. Switch to the **Monitor** tab on the left. Select **Refresh** to refresh the list. You see that the pipeline runs once every minute from the publish time to the end time. 

   Notice the values in the **Triggered By** column. The manual trigger run was from the step (**Trigger Now**) that you did earlier. 

   ![List of triggered runs](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
9. Select the down arrow next to **Pipeline Runs** to switch to the **Trigger Runs** view. 

   ![Switch to "Trigger Runs" view](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
10. Confirm that an output file is created for every pipeline run until the specified end date and time in the **output** folder. 

## Next steps
The pipeline in this sample copies data from one location to another location in Azure Blob storage. To learn about using Data Factory in more scenarios, go through the [tutorials](tutorial-copy-data-portal.md). 