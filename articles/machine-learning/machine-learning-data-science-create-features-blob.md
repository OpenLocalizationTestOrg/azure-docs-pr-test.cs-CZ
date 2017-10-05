---
title: "Vytvoření funkce pro data úložiště objektů blob v Azure pomocí Panda | Microsoft Docs"
description: "Postup vytvoření funkce pro data, která je uložená v kontejneru objektů blob v Azure s balíčkem Panda Python."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 676b5fb0-4c89-4516-b3a8-e78ae3ca078d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: 2ef2acfea2372ac7fd52d099a2b4203ee2242d81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a><span data-ttu-id="f3029-103">Vytvoření funkcí pro data služby Azure Blob Storage pomocí knihovny Pandas</span><span class="sxs-lookup"><span data-stu-id="f3029-103">Create features for Azure blob storage data using Panda</span></span>
<span data-ttu-id="f3029-104">Tento dokument ukazuje, jak vytvořit funkcí pro data, která je uložená v pomocí kontejneru objektů blob v Azure [Pandas](http://pandas.pydata.org/) balíček Python.</span><span class="sxs-lookup"><span data-stu-id="f3029-104">This document shows how to create features for data that is stored in Azure blob container using the [Pandas](http://pandas.pydata.org/) Python package.</span></span> <span data-ttu-id="f3029-105">Po osnovy jak načíst data do rámečku data Panda, ukazuje, jak vygenerovat kategorií funkce pomocí skriptů Python s hodnotami indikátoru a přihrádkování funkce.</span><span class="sxs-lookup"><span data-stu-id="f3029-105">After outlining how to load the data into a Panda data frame, it shows how to generate categorical features using Python scripts with indicator values and binning features.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="f3029-106">To **nabídky** odkazy na témata, které popisují, jak vytvořit funkce pro data v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="f3029-106">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="f3029-107">Tato úloha je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="f3029-107">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3029-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3029-108">Prerequisites</span></span>
<span data-ttu-id="f3029-109">Tento článek předpokládá, že jste vytvořili účet úložiště objektů blob v Azure a jsou uloženy vaše data existuje.</span><span class="sxs-lookup"><span data-stu-id="f3029-109">This article assumes that you have created an Azure blob storage account and have stored your data there.</span></span> <span data-ttu-id="f3029-110">Pokud budete potřebovat pokyny k nastavení účtu, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="f3029-110">If you need instructions to set up an account, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>

## <a name="load-the-data-into-a-pandas-data-frame"></a><span data-ttu-id="f3029-111">Načíst data do rámečku Pandas dat</span><span class="sxs-lookup"><span data-stu-id="f3029-111">Load the data into a Pandas data frame</span></span>
<span data-ttu-id="f3029-112">Abyste mohli prozkoumat a upravit datovou sadu, se musí stáhnout ze zdroje blob do místního souboru, který lze načíst v rámci Pandas data.</span><span class="sxs-lookup"><span data-stu-id="f3029-112">In order to do explore and manipulate a dataset, it must be downloaded from the blob source to a local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="f3029-113">Tady jsou kroky provést tento postup:</span><span class="sxs-lookup"><span data-stu-id="f3029-113">Here are the steps to follow for this procedure:</span></span>

1. <span data-ttu-id="f3029-114">Stahování dat z Azure blob s následujícím kódem ukázkové Python pomocí služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="f3029-114">Download the data from Azure blob with the following sample Python code using blob service.</span></span> <span data-ttu-id="f3029-115">Nahraďte konkrétní hodnoty proměnné v kódu níže:</span><span class="sxs-lookup"><span data-stu-id="f3029-115">Replace the variable in the code below with your specific values:</span></span>
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. <span data-ttu-id="f3029-116">Načtení dat do data rámeček Pandas ze staženého souboru.</span><span class="sxs-lookup"><span data-stu-id="f3029-116">Read the data into a Pandas data-frame from the downloaded file.</span></span>
   
        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="f3029-117">Nyní jste připraveni k data prozkoumat a generování funkce pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="f3029-117">Now you are ready to explore the data and generate features on this dataset.</span></span>

## <span data-ttu-id="f3029-118"><a name="blob-featuregen"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="f3029-118"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="f3029-119">V následujících dvou částech ukazují, jak vygenerovat kategorií funkce s hodnotami indikátoru a přihrádkování funkcí pomocí skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="f3029-119">The next two sections show how to generate categorical features with indicator values and binning features using Python scripts.</span></span>

### <span data-ttu-id="f3029-120"><a name="blob-countfeature"></a>Hodnota ukazatele na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="f3029-120"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="f3029-121">Kategorií funkce lze vytvořit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f3029-121">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="f3029-122">Zkontrolujte distribuci sloupci kategorií:</span><span class="sxs-lookup"><span data-stu-id="f3029-122">Inspect the distribution of the categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="f3029-123">Generování hodnot ukazatele pro jednotlivé hodnoty ve sloupcích</span><span class="sxs-lookup"><span data-stu-id="f3029-123">Generate indicator values for each of the column values</span></span>
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="f3029-124">Připojit ukazatel sloupec s původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="f3029-124">Join the indicator column with the original data frame</span></span>
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="f3029-125">Odeberte původní proměnnou:</span><span class="sxs-lookup"><span data-stu-id="f3029-125">Remove the original variable itself:</span></span>
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="f3029-126"><a name="blob-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="f3029-126"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="f3029-127">Pro generování binned funkce, budeme postupovat takto:</span><span class="sxs-lookup"><span data-stu-id="f3029-127">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="f3029-128">Přidat posloupnost sloupce, které chcete bin je číselný sloupec</span><span class="sxs-lookup"><span data-stu-id="f3029-128">Add a sequence of columns to bin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="f3029-129">Převést přihrádkování pořadí boolean proměnných</span><span class="sxs-lookup"><span data-stu-id="f3029-129">Convert binning to a sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="f3029-130">Nakonec připojení fiktivní proměnné zpět na původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="f3029-130">Finally, Join the dummy variables back to the original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <span data-ttu-id="f3029-131"><a name="sql-featuregen"></a>Zápis dat zpět do objektu blob Azure a využívají v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f3029-131"><a name="sql-featuregen"></a>Writing data back to Azure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="f3029-132">Poté, co jste prozkoumali data a vytvořili nezbytné funkce, můžete nahrát data (vzorkovat nebo featurized) do Azure blob a využívat v Azure Machine Learning pomocí následujících kroků: Další funkce můžete vytvářet v Azure Machine Learning Studio také.</span><span class="sxs-lookup"><span data-stu-id="f3029-132">After you have explored the data and created the necessary features, you can upload the data (sampled or featurized) to an Azure blob and consume it in Azure Machine Learning using the following steps: Note that additional features can be created in the Azure Machine Learning Studio as well.</span></span>

1. <span data-ttu-id="f3029-133">Zápis dat rámečku k místnímu souboru.</span><span class="sxs-lookup"><span data-stu-id="f3029-133">Write the data frame to local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="f3029-134">Nahrajte data do objektů blob v Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f3029-134">Upload the data to Azure blob as follows:</span></span>
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. <span data-ttu-id="f3029-135">Nyní lze číst data z objektu blob pomocí Azure Machine Learning [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulu, jak je znázorněno na obrazovce níže:</span><span class="sxs-lookup"><span data-stu-id="f3029-135">Now the data can be read from the blob using the Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module as shown in the screen below:</span></span>

![Čtečka objektů blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

