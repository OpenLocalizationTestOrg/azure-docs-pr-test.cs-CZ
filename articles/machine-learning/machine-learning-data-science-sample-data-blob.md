---
title: aaaSample dat v Azure blob storage | Microsoft Docs
description: "Ukázková data do Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8d9ad2c-86c5-43d6-80b8-d355b5c0dccf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: cceadf1fb1fb4804fc5b5a3da55c82854651026e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="9e730-103"><a name="heading"></a>Ukázková data v Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="9e730-103"><a name="heading"></a>Sample data in Azure blob storage</span></span>
<span data-ttu-id="9e730-104">Tento dokument popisuje vzorkování dat uložených v Azure blob storage stáhnout prostřednictvím kódu programu a potom ji pomocí postupů, které jsou napsané v Pythonu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="9e730-104">This document covers sampling data stored in Azure blob storage by downloading it programmatically and then sampling it using procedures written in Python.</span></span>

<span data-ttu-id="9e730-105">Následující Hello **nabídky** odkazy tootopics, které popisují, jak toosample data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e730-105">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="9e730-106">**Proč ukázková data?**</span><span class="sxs-lookup"><span data-stu-id="9e730-106">**Why sample your data?**</span></span>
<span data-ttu-id="9e730-107">Pokud datovou sadu hello plánování tooanalyze velká, je obvykle vhodné hello toodown ukázková data tooreduce ho tooa menší, ale reprezentativní a lepší správu bitlockeru velikost.</span><span class="sxs-lookup"><span data-stu-id="9e730-107">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="9e730-108">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="9e730-108">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="9e730-109">Jeho role v hello Cortana Analytics proces je rychlé vytváření prototypů tooenable hello zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="9e730-109">Its role in hello Cortana Analytics Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="9e730-110">Tato úloha vzorkování je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="9e730-110">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="download-and-down-sample-data"></a><span data-ttu-id="9e730-111">Stažení a nižší ukázková data</span><span class="sxs-lookup"><span data-stu-id="9e730-111">Download and down-sample data</span></span>
1. <span data-ttu-id="9e730-112">Stáhněte si hello data z Azure blob storage pomocí služby objektů blob hello z následující ukázkový kód Python hello:</span><span class="sxs-lookup"><span data-stu-id="9e730-112">Download hello data from Azure blob storage using hello blob service from hello following sample Python code:</span></span> 
   
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

2. <span data-ttu-id="9e730-113">Čtení dat do data rámečku Pandas z hello soubor stažený výše.</span><span class="sxs-lookup"><span data-stu-id="9e730-113">Read data into a Pandas data-frame from hello file downloaded above.</span></span>
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. <span data-ttu-id="9e730-114">Nižší sample hello dat pomocí hello `numpy`na `random.choice` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9e730-114">Down-sample hello data using hello `numpy`'s `random.choice` as follows:</span></span>
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

<span data-ttu-id="9e730-115">Nyní můžete pracovat s hello nad rámec dat s ukázkou hello procent 1 pro další zkoumání a funkce generování.</span><span class="sxs-lookup"><span data-stu-id="9e730-115">Now you can work with hello above data frame with hello 1 Percent sample for further exploration and feature generation.</span></span>

## <span data-ttu-id="9e730-116"><a name="heading"></a>Nahrání dat a načtení do Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9e730-116"><a name="heading"></a>Upload data and read it into Azure Machine Learning</span></span>
<span data-ttu-id="9e730-117">Můžete použít hello následující ukázková data hello toodown ukázkový kód a použít ho přímo v Azure Machine Learning:</span><span class="sxs-lookup"><span data-stu-id="9e730-117">You can use hello following sample code toodown-sample hello data and use it directly in Azure Machine Learning:</span></span>

1. <span data-ttu-id="9e730-118">Zápis hello dat rámce tooa místního souboru</span><span class="sxs-lookup"><span data-stu-id="9e730-118">Write hello data frame tooa local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. <span data-ttu-id="9e730-119">Nahrát hello místního souboru tooan Azure blob pomocí hello následující ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="9e730-119">Upload hello local file tooan Azure blob using hello following sample code:</span></span>
   
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
            print ("Something went wrong with uploading toohello blob:"+ BLOBNAME)

3. <span data-ttu-id="9e730-120">Čtení dat hello z hello objektů blob v Azure pomocí Azure Machine Learning [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="9e730-120">Read hello data from hello Azure blob using Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) as shown in hello image below:</span></span>

![Čtečka objektů blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

