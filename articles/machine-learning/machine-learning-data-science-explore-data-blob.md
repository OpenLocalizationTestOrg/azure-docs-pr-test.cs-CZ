---
title: "Prozkoumejte data v úložišti objektů blob v Azure s Pandas | Microsoft Docs"
description: "Jak chcete dynamicky prozkoumávat data, která je uložená v kontejneru objektů blob v Azure pomocí Pandas."
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
ms.openlocfilehash: e1b33b17270122a38228484a56c8324c5b4505a0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a><span data-ttu-id="2286e-103">Zkoumání dat ve službě Azure Blob Storage pomocí knihovny Pandas</span><span class="sxs-lookup"><span data-stu-id="2286e-103">Explore data in Azure blob storage with Pandas</span></span>
<span data-ttu-id="2286e-104">Tento dokument popisuje, jak chcete dynamicky prozkoumávat data, která je uložená v Azure blob kontejneru pomocí [Pandas](http://pandas.pydata.org/) balíček Python.</span><span class="sxs-lookup"><span data-stu-id="2286e-104">This document covers how to explore data that is stored in Azure blob container using [Pandas](http://pandas.pydata.org/) Python package.</span></span>

<span data-ttu-id="2286e-105">Následující **nabídky** odkazy na témata, které popisují, jak používat nástroje a prozkoumejte data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="2286e-105">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span> <span data-ttu-id="2286e-106">Tato úloha je krok v [proces vědecké účely dat]().</span><span class="sxs-lookup"><span data-stu-id="2286e-106">This task is a step in the [Data Science Process]().</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="2286e-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2286e-107">Prerequisites</span></span>
<span data-ttu-id="2286e-108">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="2286e-108">This article assumes that you have:</span></span>

* <span data-ttu-id="2286e-109">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2286e-109">Created an Azure storage account.</span></span> <span data-ttu-id="2286e-110">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="2286e-110">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="2286e-111">Vaše data uložena v účtu úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="2286e-111">Stored your data in an Azure blob storage account.</span></span> <span data-ttu-id="2286e-112">Pokud budete potřebovat pokyny, najdete v části [přesun dat do a z Azure Storage](../storage/common/storage-moving-data.md)</span><span class="sxs-lookup"><span data-stu-id="2286e-112">If you need instructions, see [Moving data to and from Azure Storage](../storage/common/storage-moving-data.md)</span></span>

## <a name="load-the-data-into-a-pandas-dataframe"></a><span data-ttu-id="2286e-113">Načíst data do Pandas DataFrame</span><span class="sxs-lookup"><span data-stu-id="2286e-113">Load the data into a Pandas DataFrame</span></span>
<span data-ttu-id="2286e-114">Pokud chcete prozkoumat a manipulaci s datovou sadu, se musí nejprve stáhnout ze zdroje blob do místního souboru, který lze načíst v Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="2286e-114">To explore and manipulate a dataset, it must first be downloaded from the blob source to a local file, which can then be loaded in a Pandas DataFrame.</span></span> <span data-ttu-id="2286e-115">Tady jsou kroky provést tento postup:</span><span class="sxs-lookup"><span data-stu-id="2286e-115">Here are the steps to follow for this procedure:</span></span>

1. <span data-ttu-id="2286e-116">Stahování dat z Azure blob s následující ukázka kódu Pythonu pomocí služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="2286e-116">Download the data from Azure blob with the following Python code sample using blob service.</span></span> <span data-ttu-id="2286e-117">Nahraďte konkrétní hodnoty proměnné v následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2286e-117">Replace the variable in the following code with your specific values:</span></span> 
   
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
2. <span data-ttu-id="2286e-118">Načtení dat do data rámeček Pandas ze staženého souboru.</span><span class="sxs-lookup"><span data-stu-id="2286e-118">Read the data into a Pandas data-frame from the downloaded file.</span></span>
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="2286e-119">Nyní jste připraveni k data prozkoumat a generování funkce pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="2286e-119">Now you are ready to explore the data and generate features on this dataset.</span></span>

## <span data-ttu-id="2286e-120"><a name="blob-dataexploration"></a>Příklady zkoumání dat pomocí Pandas</span><span class="sxs-lookup"><span data-stu-id="2286e-120"><a name="blob-dataexploration"></a>Examples of data exploration using Pandas</span></span>
<span data-ttu-id="2286e-121">Tady je několik příkladů jak prozkoumat dat pomocí Pandas:</span><span class="sxs-lookup"><span data-stu-id="2286e-121">Here are a few examples of ways to explore data using Pandas:</span></span>

1. <span data-ttu-id="2286e-122">Zkontrolujte **počet řádků a sloupců**</span><span class="sxs-lookup"><span data-stu-id="2286e-122">Inspect the **number of rows and columns**</span></span> 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="2286e-123">**Zkontrolujte** několik první nebo poslední **řádky** v datové sadě následující:</span><span class="sxs-lookup"><span data-stu-id="2286e-123">**Inspect** the first or last few **rows** in the following dataset:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="2286e-124">Zkontrolujte **datový typ** každý sloupec byl importován jako pomocí následující vzorový kód</span><span class="sxs-lookup"><span data-stu-id="2286e-124">Check the **data type** each column was imported as using the following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="2286e-125">Zkontrolujte **základní statistiky** pro sloupce v datech nastavte takto</span><span class="sxs-lookup"><span data-stu-id="2286e-125">Check the **basic stats** for the columns in the data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="2286e-126">Podívejte se na počet položek pro každou hodnotu sloupce následujícím způsobem</span><span class="sxs-lookup"><span data-stu-id="2286e-126">Look at the number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="2286e-127">**Počet chybějících hodnot** a skutečný počet položek v jednotlivých sloupcích pomocí následující vzorový kód</span><span class="sxs-lookup"><span data-stu-id="2286e-127">**Count missing values** versus the actual number of entries in each column using the following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="2286e-128">Pokud máte **chybějící hodnoty** pro konkrétní sloupec v datech, můžete jejich umístění následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2286e-128">If you have **missing values** for a specific column in the data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="2286e-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="2286e-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="2286e-130">Jiný způsob, jak nahradit chybějící hodnoty je pomocí funkce režimu:</span><span class="sxs-lookup"><span data-stu-id="2286e-130">Another way to replace missing values is with the mode function:</span></span>
   
     <span data-ttu-id="2286e-131">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]</span><span class="sxs-lookup"><span data-stu-id="2286e-131">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="2286e-132">Vytvoření **histogram** vykreslení pomocí proměnné počet přihrádek k vykreslení distribuce proměnné</span><span class="sxs-lookup"><span data-stu-id="2286e-132">Create a **histogram** plot using variable number of bins to plot the distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="2286e-133">Podívejte se na **korelací** mezi proměnné pomocí scatterplot nebo pomocí funkce integrované korelace</span><span class="sxs-lookup"><span data-stu-id="2286e-133">Look at **correlations** between variables using a scatterplot or using the built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

