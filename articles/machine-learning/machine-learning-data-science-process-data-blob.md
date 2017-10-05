---
title: "Zpracování dat objektů blob v Azure s pokročilou analýzu | Microsoft Docs"
description: "Zpracování dat v úložišti objektů Blob Azure."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8a59078-91d3-4440-b85c-430363c3f4d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 36d950fd81029af82d9f2f652b2f01dba5fc8cc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="93702-103"><a name="heading"></a>Zpracování dat objektů blob v Azure s pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="93702-103"><a name="heading"></a>Process Azure blob data with advanced analytics</span></span>
<span data-ttu-id="93702-104">Tento dokument popisuje seznámení data a funkce generování z dat uložených v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="93702-104">This document covers exploring data and generating features from data stored in Azure Blob storage.</span></span> 

## <a name="load-the-data-into-a-pandas-data-frame"></a><span data-ttu-id="93702-105">Načíst data do rámečku Pandas dat</span><span class="sxs-lookup"><span data-stu-id="93702-105">Load the data into a Pandas data frame</span></span>
<span data-ttu-id="93702-106">Abyste mohli prozkoumat a upravit datovou sadu, se musí stáhnout ze zdroje blob do místního souboru, který lze načíst v rámci Pandas data.</span><span class="sxs-lookup"><span data-stu-id="93702-106">In order to explore and manipulate a dataset, it must be downloaded from the blob source to a local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="93702-107">Tady jsou kroky provést tento postup:</span><span class="sxs-lookup"><span data-stu-id="93702-107">Here are the steps to follow for this procedure:</span></span>

1. <span data-ttu-id="93702-108">Stahování dat z Azure blob s následujícím kódem ukázkové Python pomocí služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="93702-108">Download the data from Azure blob with the following sample Python code using blob service.</span></span> <span data-ttu-id="93702-109">Nahraďte konkrétní hodnoty proměnné v kódu níže:</span><span class="sxs-lookup"><span data-stu-id="93702-109">Replace the variable in the code below with your specific values:</span></span> 
   
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
2. <span data-ttu-id="93702-110">Načtení dat do data rámeček Pandas ze staženého souboru.</span><span class="sxs-lookup"><span data-stu-id="93702-110">Read the data into a Pandas data-frame from the downloaded file.</span></span>
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="93702-111">Nyní jste připraveni k data prozkoumat a generování funkce pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="93702-111">Now you are ready to explore the data and generate features on this dataset.</span></span>

## <span data-ttu-id="93702-112"><a name="blob-dataexploration"></a>Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="93702-112"><a name="blob-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="93702-113">Tady je několik příkladů jak prozkoumat dat pomocí Pandas:</span><span class="sxs-lookup"><span data-stu-id="93702-113">Here are a few examples of ways to explore data using Pandas:</span></span>

1. <span data-ttu-id="93702-114">Kontrola počtu řádků a sloupců</span><span class="sxs-lookup"><span data-stu-id="93702-114">Inspect the number of rows and columns</span></span> 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="93702-115">Zkontrolujte první nebo poslední několik řádků v datové sadě, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="93702-115">Inspect the first or last few rows in the dataset as below:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="93702-116">Kontrola typu dat, které každý sloupec byl importován jako pomocí následující vzorový kód</span><span class="sxs-lookup"><span data-stu-id="93702-116">Check the data type each column was imported as using the following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="93702-117">Zkontrolujte následující základní statistiky pro sloupce v datové sadě</span><span class="sxs-lookup"><span data-stu-id="93702-117">Check the basic stats for the columns in the data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="93702-118">Podívejte se na počet položek pro každou hodnotu sloupce následujícím způsobem</span><span class="sxs-lookup"><span data-stu-id="93702-118">Look at the number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="93702-119">Počet chybějících hodnot a skutečný počet položek v jednotlivých sloupcích pomocí následující vzorový kód</span><span class="sxs-lookup"><span data-stu-id="93702-119">Count missing values versus the actual number of entries in each column using the following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="93702-120">Pokud máte chybějící hodnoty pro konkrétní sloupec v datech, vyřaďte je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93702-120">If you have missing values for a specific column in the data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="93702-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="93702-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="93702-122">Jiný způsob, jak nahradit chybějící hodnoty je pomocí funkce režimu:</span><span class="sxs-lookup"><span data-stu-id="93702-122">Another way to replace missing values is with the mode function:</span></span>
   
     <span data-ttu-id="93702-123">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]</span><span class="sxs-lookup"><span data-stu-id="93702-123">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="93702-124">Vytvoření histogram vykreslení pomocí proměnné počet přihrádek k vykreslení distribuce proměnné</span><span class="sxs-lookup"><span data-stu-id="93702-124">Create a histogram plot using variable number of bins to plot the distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="93702-125">Podívejte se na korelací mezi proměnné pomocí scatterplot nebo pomocí funkce integrované korelace</span><span class="sxs-lookup"><span data-stu-id="93702-125">Look at correlations between variables using a scatterplot or using the built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <span data-ttu-id="93702-126"><a name="blob-featuregen"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="93702-126"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="93702-127">Můžete se vygeneruje funkcí s použitím Python následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93702-127">We can generate features using Python as follows:</span></span>

### <span data-ttu-id="93702-128"><a name="blob-countfeature"></a>Hodnota ukazatele na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="93702-128"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="93702-129">Kategorií funkce lze vytvořit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93702-129">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="93702-130">Zkontrolujte distribuci sloupci kategorií:</span><span class="sxs-lookup"><span data-stu-id="93702-130">Inspect the distribution of the categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="93702-131">Generování hodnot ukazatele pro jednotlivé hodnoty ve sloupcích</span><span class="sxs-lookup"><span data-stu-id="93702-131">Generate indicator values for each of the column values</span></span>
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="93702-132">Připojit ukazatel sloupec s původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="93702-132">Join the indicator column with the original data frame</span></span> 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="93702-133">Odeberte původní proměnnou:</span><span class="sxs-lookup"><span data-stu-id="93702-133">Remove the original variable itself:</span></span>
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="93702-134"><a name="blob-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="93702-134"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="93702-135">Pro generování binned funkce, budeme postupovat takto:</span><span class="sxs-lookup"><span data-stu-id="93702-135">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="93702-136">Přidat posloupnost sloupce, které chcete bin je číselný sloupec</span><span class="sxs-lookup"><span data-stu-id="93702-136">Add a sequence of columns to bin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="93702-137">Převést přihrádkování pořadí boolean proměnných</span><span class="sxs-lookup"><span data-stu-id="93702-137">Convert binning to a sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="93702-138">Nakonec připojení fiktivní proměnné zpět na původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="93702-138">Finally, Join the dummy variables back to the original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <span data-ttu-id="93702-139"><a name="sql-featuregen"></a>Zápis dat zpět do objektu blob Azure a využívají v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="93702-139"><a name="sql-featuregen"></a>Writing data back to Azure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="93702-140">Poté, co jste prozkoumali data a vytvořili nezbytné funkce, můžete nahrát data (vzorkovat nebo featurized) do Azure blob a využívat v Azure Machine Learning pomocí následujících kroků: Další funkce můžete vytvářet v Azure Machine Learning Studio také.</span><span class="sxs-lookup"><span data-stu-id="93702-140">After you have explored the data and created the necessary features, you can upload the data (sampled or featurized) to an Azure blob and consume it in Azure Machine Learning using the following steps: Note that additional features can be created in the Azure Machine Learning Studio as well.</span></span> 

1. <span data-ttu-id="93702-141">Zápis dat rámečku k místnímu souboru.</span><span class="sxs-lookup"><span data-stu-id="93702-141">Write the data frame to local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="93702-142">Nahrajte data do objektů blob v Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93702-142">Upload the data to Azure blob as follows:</span></span>
   
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
3. <span data-ttu-id="93702-143">Nyní lze číst data z objektu blob pomocí Azure Machine Learning [importovat Data] [ import-data] modulu, jak je znázorněno na obrazovce níže:</span><span class="sxs-lookup"><span data-stu-id="93702-143">Now the data can be read from the blob using the Azure Machine Learning [Import Data][import-data] module as shown in the screen below:</span></span>

![Čtečka objektů blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

