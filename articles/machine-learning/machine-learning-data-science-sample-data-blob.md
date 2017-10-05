---
title: "Ukázková data v Azure blob storage | Microsoft Docs"
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
ms.openlocfilehash: aa9ab454706429682a393c3d5758cebe20790e19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="de1ef-103"><a name="heading"></a>Ukázková data v Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="de1ef-103"><a name="heading"></a>Sample data in Azure blob storage</span></span>
<span data-ttu-id="de1ef-104">Tento dokument popisuje vzorkování dat uložených v Azure blob storage stáhnout prostřednictvím kódu programu a potom ji pomocí postupů, které jsou napsané v Pythonu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="de1ef-104">This document covers sampling data stored in Azure blob storage by downloading it programmatically and then sampling it using procedures written in Python.</span></span>

<span data-ttu-id="de1ef-105">Následující **nabídky** odkazy na témata, které popisují, jak ukázková data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="de1ef-105">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="de1ef-106">**Proč ukázková data?**</span><span class="sxs-lookup"><span data-stu-id="de1ef-106">**Why sample your data?**</span></span>
<span data-ttu-id="de1ef-107">Pokud je velké datové sady, které chcete analyzovat, je obvykle vhodné nižší ukázková data, která mají snížit velikost menší, ale reprezentativní a lepší správu bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="de1ef-107">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="de1ef-108">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="de1ef-108">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="de1ef-109">Jeho role v procesu Cortana Analytics je umožnit rychlé vytváření prototypů zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="de1ef-109">Its role in the Cortana Analytics Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="de1ef-110">Tato úloha vzorkování je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="de1ef-110">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="download-and-down-sample-data"></a><span data-ttu-id="de1ef-111">Stažení a nižší ukázková data</span><span class="sxs-lookup"><span data-stu-id="de1ef-111">Download and down-sample data</span></span>
1. <span data-ttu-id="de1ef-112">Stáhněte data z Azure blob storage pomocí služby objektů blob z následující vzorový kód Python:</span><span class="sxs-lookup"><span data-stu-id="de1ef-112">Download the data from Azure blob storage using the blob service from the following sample Python code:</span></span> 
   
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

2. <span data-ttu-id="de1ef-113">Čtení dat do data rámečku Pandas z soubor stažený výše.</span><span class="sxs-lookup"><span data-stu-id="de1ef-113">Read data into a Pandas data-frame from the file downloaded above.</span></span>
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. <span data-ttu-id="de1ef-114">Nižší sample dat pomocí `numpy`na `random.choice` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="de1ef-114">Down-sample the data using the `numpy`'s `random.choice` as follows:</span></span>
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

<span data-ttu-id="de1ef-115">Nyní můžete pracovat s výše rámečku dat s ukázkou procent 1 pro další zkoumání a funkce generování.</span><span class="sxs-lookup"><span data-stu-id="de1ef-115">Now you can work with the above data frame with the 1 Percent sample for further exploration and feature generation.</span></span>

## <span data-ttu-id="de1ef-116"><a name="heading"></a>Nahrání dat a načtení do Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="de1ef-116"><a name="heading"></a>Upload data and read it into Azure Machine Learning</span></span>
<span data-ttu-id="de1ef-117">Následující vzorový kód vám pomůže nižší sample data a použít ho přímo v Azure Machine Learning:</span><span class="sxs-lookup"><span data-stu-id="de1ef-117">You can use the following sample code to down-sample the data and use it directly in Azure Machine Learning:</span></span>

1. <span data-ttu-id="de1ef-118">Zápis dat rámečku do místního souboru</span><span class="sxs-lookup"><span data-stu-id="de1ef-118">Write the data frame to a local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. <span data-ttu-id="de1ef-119">Nahrajte místního souboru do služby Azure blob pomocí následující vzorový kód:</span><span class="sxs-lookup"><span data-stu-id="de1ef-119">Upload the local file to an Azure blob using the following sample code:</span></span>
   
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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. <span data-ttu-id="de1ef-120">Čtení dat z Azure blob pomocí Azure Machine Learning [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) jak je znázorněno na obrázku níže:</span><span class="sxs-lookup"><span data-stu-id="de1ef-120">Read the data from the Azure blob using Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) as shown in the image below:</span></span>

![Čtečka objektů blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

