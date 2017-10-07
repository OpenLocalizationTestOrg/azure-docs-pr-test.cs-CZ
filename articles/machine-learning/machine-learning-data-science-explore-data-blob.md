---
title: aaaExplore dat v Azure blob storage s Pandas | Microsoft Docs
description: "Jak tooexplore data uložená v Azure kontejner objektů blob pomocí Pandas."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 28f3c0aebf2300006066c4b19dcb1f0a76a1deb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a><span data-ttu-id="cf687-103">Zkoumání dat ve službě Azure Blob Storage pomocí knihovny Pandas</span><span class="sxs-lookup"><span data-stu-id="cf687-103">Explore data in Azure blob storage with Pandas</span></span>
<span data-ttu-id="cf687-104">Tento dokument popisuje, jak tooexplore data uložená v Azure blob pomocí kontejneru [Pandas](http://pandas.pydata.org/) balíček Python.</span><span class="sxs-lookup"><span data-stu-id="cf687-104">This document covers how tooexplore data that is stored in Azure blob container using [Pandas](http://pandas.pydata.org/) Python package.</span></span>

<span data-ttu-id="cf687-105">Následující Hello **nabídky** odkazy tootopics, které popisují, jak toouse nástroje tooexplore data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="cf687-105">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span> <span data-ttu-id="cf687-106">Tato úloha je krok v hello [proces vědecké účely dat]().</span><span class="sxs-lookup"><span data-stu-id="cf687-106">This task is a step in hello [Data Science Process]().</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="cf687-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cf687-107">Prerequisites</span></span>
<span data-ttu-id="cf687-108">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="cf687-108">This article assumes that you have:</span></span>

* <span data-ttu-id="cf687-109">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cf687-109">Created an Azure storage account.</span></span> <span data-ttu-id="cf687-110">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="cf687-110">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="cf687-111">Vaše data uložena v účtu úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="cf687-111">Stored your data in an Azure blob storage account.</span></span> <span data-ttu-id="cf687-112">Pokud budete potřebovat pokyny, najdete v části [tooand přesouvání dat z Azure Storage](../storage/common/storage-moving-data.md)</span><span class="sxs-lookup"><span data-stu-id="cf687-112">If you need instructions, see [Moving data tooand from Azure Storage](../storage/common/storage-moving-data.md)</span></span>

## <a name="load-hello-data-into-a-pandas-dataframe"></a><span data-ttu-id="cf687-113">Načtení dat hello do Pandas DataFrame</span><span class="sxs-lookup"><span data-stu-id="cf687-113">Load hello data into a Pandas DataFrame</span></span>
<span data-ttu-id="cf687-114">tooexplore a pracovat s datovou sadu, se musí nejprve stáhnout z hello objektů blob zdroj tooa místního souboru, které pak mohou být načteny v Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="cf687-114">tooexplore and manipulate a dataset, it must first be downloaded from hello blob source tooa local file, which can then be loaded in a Pandas DataFrame.</span></span> <span data-ttu-id="cf687-115">Zde jsou kroky toofollow hello pro tento postup:</span><span class="sxs-lookup"><span data-stu-id="cf687-115">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="cf687-116">Stáhněte si hello data z Azure blob s hello následující ukázka kódu Pythonu pro pomocí služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cf687-116">Download hello data from Azure blob with hello following Python code sample using blob service.</span></span> <span data-ttu-id="cf687-117">Nahraďte proměnnou hello v hello následující kód s konkrétními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="cf687-117">Replace hello variable in hello following code with your specific values:</span></span> 
   
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
2. <span data-ttu-id="cf687-118">Čtení hello dat do data rámečku Pandas z hello stáhli soubor.</span><span class="sxs-lookup"><span data-stu-id="cf687-118">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="cf687-119">Nyní jsou připravené tooexplore hello data a vygenerovat funkce pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="cf687-119">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="cf687-120"><a name="blob-dataexploration"></a>Příklady zkoumání dat pomocí Pandas</span><span class="sxs-lookup"><span data-stu-id="cf687-120"><a name="blob-dataexploration"></a>Examples of data exploration using Pandas</span></span>
<span data-ttu-id="cf687-121">Zde je několik příkladů způsoby tooexplore dat pomocí Pandas:</span><span class="sxs-lookup"><span data-stu-id="cf687-121">Here are a few examples of ways tooexplore data using Pandas:</span></span>

1. <span data-ttu-id="cf687-122">Zkontrolujte hello **počet řádků a sloupců**</span><span class="sxs-lookup"><span data-stu-id="cf687-122">Inspect hello **number of rows and columns**</span></span> 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="cf687-123">**Zkontrolujte** hello první nebo poslední několik **řádky** v hello následující datové sady:</span><span class="sxs-lookup"><span data-stu-id="cf687-123">**Inspect** hello first or last few **rows** in hello following dataset:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="cf687-124">Zkontrolujte hello **datový typ** každý sloupec byl importován jako pomocí hello následující ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="cf687-124">Check hello **data type** each column was imported as using hello following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="cf687-125">Zkontrolujte hello **základní statistiky** pro hello sloupců v následujícím způsobem hello datové sady</span><span class="sxs-lookup"><span data-stu-id="cf687-125">Check hello **basic stats** for hello columns in hello data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="cf687-126">Podívejte se na hello počet položek pro každou hodnotu sloupce následujícím způsobem</span><span class="sxs-lookup"><span data-stu-id="cf687-126">Look at hello number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="cf687-127">**Počet chybějících hodnot** versus hello skutečný počet položek v jednotlivých sloupcích pomocí hello následující ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="cf687-127">**Count missing values** versus hello actual number of entries in each column using hello following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="cf687-128">Pokud máte **chybějící hodnoty** pro konkrétní sloupec v hello data, můžete jejich umístění následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cf687-128">If you have **missing values** for a specific column in hello data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="cf687-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="cf687-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="cf687-130">Jiný způsob tooreplace chybějící hodnoty je pomocí funkce režimu hello:</span><span class="sxs-lookup"><span data-stu-id="cf687-130">Another way tooreplace missing values is with hello mode function:</span></span>
   
     <span data-ttu-id="cf687-131">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]</span><span class="sxs-lookup"><span data-stu-id="cf687-131">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="cf687-132">Vytvoření **histogram** vykreslení pomocí proměnný počet přihrádek tooplot hello distribuční proměnné</span><span class="sxs-lookup"><span data-stu-id="cf687-132">Create a **histogram** plot using variable number of bins tooplot hello distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="cf687-133">Podívejte se na **korelací** mezi proměnné pomocí scatterplot nebo pomocí funkce integrované korelace hello</span><span class="sxs-lookup"><span data-stu-id="cf687-133">Look at **correlations** between variables using a scatterplot or using hello built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

