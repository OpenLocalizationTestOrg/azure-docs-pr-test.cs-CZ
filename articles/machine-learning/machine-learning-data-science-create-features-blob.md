---
title: "Funkce aaaCreate pro Azure blob úložiště dat pomocí Panda | Microsoft Docs"
description: "Jak toocreate funkce pro data, která je uložená v kontejneru objektů blob v Azure s balíček Panda Python hello."
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
ms.openlocfilehash: 8594046c5d76a36ad87fc77e407752489d30afcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a><span data-ttu-id="809eb-103">Vytvoření funkcí pro data služby Azure Blob Storage pomocí knihovny Pandas</span><span class="sxs-lookup"><span data-stu-id="809eb-103">Create features for Azure blob storage data using Panda</span></span>
<span data-ttu-id="809eb-104">Tento dokument ukazuje, jak funkce toocreate pro data, která je uložená v kontejneru objektů blob v Azure pomocí hello [Pandas](http://pandas.pydata.org/) balíček Python.</span><span class="sxs-lookup"><span data-stu-id="809eb-104">This document shows how toocreate features for data that is stored in Azure blob container using hello [Pandas](http://pandas.pydata.org/) Python package.</span></span> <span data-ttu-id="809eb-105">Po osnovy jak tooload hello dat do data rámečku Panda, ukazuje způsob toogenerate kategorií funkce pomocí skriptů Python s hodnotami indikátoru a přihrádkování funkce.</span><span class="sxs-lookup"><span data-stu-id="809eb-105">After outlining how tooload hello data into a Panda data frame, it shows how toogenerate categorical features using Python scripts with indicator values and binning features.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="809eb-106">To **nabídky** odkazy tootopics, které popisují, jak funkce toocreate pro data v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="809eb-106">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="809eb-107">Tato úloha je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="809eb-107">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="809eb-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="809eb-108">Prerequisites</span></span>
<span data-ttu-id="809eb-109">Tento článek předpokládá, že jste vytvořili účet úložiště objektů blob v Azure a jsou uloženy vaše data existuje.</span><span class="sxs-lookup"><span data-stu-id="809eb-109">This article assumes that you have created an Azure blob storage account and have stored your data there.</span></span> <span data-ttu-id="809eb-110">Pokud budete potřebovat pokyny tooset si účet, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="809eb-110">If you need instructions tooset up an account, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>

## <a name="load-hello-data-into-a-pandas-data-frame"></a><span data-ttu-id="809eb-111">Načtení dat hello do rámečku Pandas dat</span><span class="sxs-lookup"><span data-stu-id="809eb-111">Load hello data into a Pandas data frame</span></span>
<span data-ttu-id="809eb-112">V pořadí toodo prozkoumat a upravit datovou sadu, se musí stáhnout z hello blob zdroj tooa místního souboru, který lze načíst v rámci Pandas data.</span><span class="sxs-lookup"><span data-stu-id="809eb-112">In order toodo explore and manipulate a dataset, it must be downloaded from hello blob source tooa local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="809eb-113">Zde jsou kroky toofollow hello pro tento postup:</span><span class="sxs-lookup"><span data-stu-id="809eb-113">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="809eb-114">Stáhněte si hello data z Azure blob s hello následující ukázkový kód Python pomocí služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="809eb-114">Download hello data from Azure blob with hello following sample Python code using blob service.</span></span> <span data-ttu-id="809eb-115">Nahraďte konkrétní hodnoty proměnné hello v hello kódu níže:</span><span class="sxs-lookup"><span data-stu-id="809eb-115">Replace hello variable in hello code below with your specific values:</span></span>
   
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
        print(("It takes %s seconds toodownload "+blobname) % (t2 - t1))
2. <span data-ttu-id="809eb-116">Čtení hello dat do data rámečku Pandas z hello stáhli soubor.</span><span class="sxs-lookup"><span data-stu-id="809eb-116">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="809eb-117">Nyní jsou připravené tooexplore hello data a vygenerovat funkce pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="809eb-117">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="809eb-118"><a name="blob-featuregen"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="809eb-118"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="809eb-119">Hello další dvě části vysvětlují, jak toogenerate kategorií funkce s hodnotami indikátoru a přihrádkování funkce pomocí skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="809eb-119">hello next two sections show how toogenerate categorical features with indicator values and binning features using Python scripts.</span></span>

### <span data-ttu-id="809eb-120"><a name="blob-countfeature"></a>Hodnota ukazatele na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="809eb-120"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="809eb-121">Kategorií funkce lze vytvořit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="809eb-121">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="809eb-122">Zkontrolujte hello distribuční sloupce hello kategorií:</span><span class="sxs-lookup"><span data-stu-id="809eb-122">Inspect hello distribution of hello categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="809eb-123">Generování hodnot ukazatele pro jednotlivé hodnoty ve sloupcích hello</span><span class="sxs-lookup"><span data-stu-id="809eb-123">Generate indicator values for each of hello column values</span></span>
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="809eb-124">Připojení k hello indikátor sloupec s hello původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="809eb-124">Join hello indicator column with hello original data frame</span></span>
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="809eb-125">Odebrání hello původní proměnná sám sebe:</span><span class="sxs-lookup"><span data-stu-id="809eb-125">Remove hello original variable itself:</span></span>
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="809eb-126"><a name="blob-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="809eb-126"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="809eb-127">Pro generování binned funkce, budeme postupovat takto:</span><span class="sxs-lookup"><span data-stu-id="809eb-127">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="809eb-128">Přidat posloupnost toobin sloupce je číselný sloupec</span><span class="sxs-lookup"><span data-stu-id="809eb-128">Add a sequence of columns toobin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="809eb-129">Převést přihrádkování tooa pořadí boolean proměnných</span><span class="sxs-lookup"><span data-stu-id="809eb-129">Convert binning tooa sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="809eb-130">Nakonec připojení hello fiktivní proměnné back toohello původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="809eb-130">Finally, Join hello dummy variables back toohello original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <span data-ttu-id="809eb-131"><a name="sql-featuregen"></a>Zápis dat zálohovat tooAzure objektů blob a využívání v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="809eb-131"><a name="sql-featuregen"></a>Writing data back tooAzure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="809eb-132">Poté, co jste prozkoumali hello dat a vytvořit hello nezbytné funkce, můžete nahrát hello data (vzorkovat nebo featurized) tooan Azure blob a využívat v Azure Machine Learning pomocí následujících kroků hello: Další funkce můžete vytvářet v hello Azure Machine Learning Studio také.</span><span class="sxs-lookup"><span data-stu-id="809eb-132">After you have explored hello data and created hello necessary features, you can upload hello data (sampled or featurized) tooan Azure blob and consume it in Azure Machine Learning using hello following steps: Note that additional features can be created in hello Azure Machine Learning Studio as well.</span></span>

1. <span data-ttu-id="809eb-133">Zápis hello dat rámce toolocal souboru</span><span class="sxs-lookup"><span data-stu-id="809eb-133">Write hello data frame toolocal file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="809eb-134">Nahrajte objekt blob tooAzure data hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="809eb-134">Upload hello data tooAzure blob as follows:</span></span>
   
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
3. <span data-ttu-id="809eb-135">Teď můžete číst hello data z hello blob pomocí Azure Machine Learning hello [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulu, jak je znázorněno v úvodní obrazovka níže:</span><span class="sxs-lookup"><span data-stu-id="809eb-135">Now hello data can be read from hello blob using hello Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module as shown in hello screen below:</span></span>

![Čtečka objektů blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

