---
title: "aaaProcess Azure blob dat s pokročilou analýzu | Microsoft Docs"
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
ms.openlocfilehash: 5911d4211c4135680555a8cdd99e745499a24215
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="27425-103"><a name="heading"></a>Zpracování dat objektů blob v Azure s pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="27425-103"><a name="heading"></a>Process Azure blob data with advanced analytics</span></span>
<span data-ttu-id="27425-104">Tento dokument popisuje seznámení data a funkce generování z dat uložených v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="27425-104">This document covers exploring data and generating features from data stored in Azure Blob storage.</span></span> 

## <a name="load-hello-data-into-a-pandas-data-frame"></a><span data-ttu-id="27425-105">Načtení dat hello do rámečku Pandas dat</span><span class="sxs-lookup"><span data-stu-id="27425-105">Load hello data into a Pandas data frame</span></span>
<span data-ttu-id="27425-106">V pořadí tooexplore a pracovat s datovou sadu, se musí stáhnout z hello blob zdroj tooa místního souboru, který lze načíst v rámci Pandas data.</span><span class="sxs-lookup"><span data-stu-id="27425-106">In order tooexplore and manipulate a dataset, it must be downloaded from hello blob source tooa local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="27425-107">Zde jsou kroky toofollow hello pro tento postup:</span><span class="sxs-lookup"><span data-stu-id="27425-107">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="27425-108">Stáhněte si hello data z Azure blob s hello následující ukázkový kód Python pomocí služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="27425-108">Download hello data from Azure blob with hello following sample Python code using blob service.</span></span> <span data-ttu-id="27425-109">Nahraďte konkrétní hodnoty proměnné hello v hello kódu níže:</span><span class="sxs-lookup"><span data-stu-id="27425-109">Replace hello variable in hello code below with your specific values:</span></span> 
   
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
2. <span data-ttu-id="27425-110">Čtení hello dat do data rámečku Pandas z hello stáhli soubor.</span><span class="sxs-lookup"><span data-stu-id="27425-110">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="27425-111">Nyní jsou připravené tooexplore hello data a vygenerovat funkce pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="27425-111">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="27425-112"><a name="blob-dataexploration"></a>Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="27425-112"><a name="blob-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="27425-113">Zde je několik příkladů způsoby tooexplore dat pomocí Pandas:</span><span class="sxs-lookup"><span data-stu-id="27425-113">Here are a few examples of ways tooexplore data using Pandas:</span></span>

1. <span data-ttu-id="27425-114">Zkontrolujte hello počet řádků a sloupců</span><span class="sxs-lookup"><span data-stu-id="27425-114">Inspect hello number of rows and columns</span></span> 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="27425-115">Zkontrolujte hello první nebo poslední několik řádků v datové sadě hello jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="27425-115">Inspect hello first or last few rows in hello dataset as below:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="27425-116">Zkontrolujte hello datový typ, který každý sloupec byl importován jako pomocí hello následující ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="27425-116">Check hello data type each column was imported as using hello following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="27425-117">Zkontrolujte hello základní statistiky pro hello sloupců v datové sadě hello následujícím způsobem</span><span class="sxs-lookup"><span data-stu-id="27425-117">Check hello basic stats for hello columns in hello data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="27425-118">Podívejte se na hello počet položek pro každou hodnotu sloupce následujícím způsobem</span><span class="sxs-lookup"><span data-stu-id="27425-118">Look at hello number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="27425-119">Počet chybějících hodnot versus hello skutečný počet položek v jednotlivých sloupcích pomocí hello následující ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="27425-119">Count missing values versus hello actual number of entries in each column using hello following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="27425-120">Pokud máte chybějící hodnoty pro konkrétní sloupec v datech hello, vyřaďte je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="27425-120">If you have missing values for a specific column in hello data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="27425-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="27425-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="27425-122">Jiný způsob tooreplace chybějící hodnoty je pomocí funkce režimu hello:</span><span class="sxs-lookup"><span data-stu-id="27425-122">Another way tooreplace missing values is with hello mode function:</span></span>
   
     <span data-ttu-id="27425-123">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]</span><span class="sxs-lookup"><span data-stu-id="27425-123">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="27425-124">Vytvoření histogram vykreslení pomocí proměnný počet přihrádek tooplot hello distribuční proměnné</span><span class="sxs-lookup"><span data-stu-id="27425-124">Create a histogram plot using variable number of bins tooplot hello distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="27425-125">Podívejte se na korelací mezi proměnné pomocí scatterplot nebo pomocí funkce integrované korelace hello</span><span class="sxs-lookup"><span data-stu-id="27425-125">Look at correlations between variables using a scatterplot or using hello built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <span data-ttu-id="27425-126"><a name="blob-featuregen"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="27425-126"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="27425-127">Můžete se vygeneruje funkcí s použitím Python následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="27425-127">We can generate features using Python as follows:</span></span>

### <span data-ttu-id="27425-128"><a name="blob-countfeature"></a>Hodnota ukazatele na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="27425-128"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="27425-129">Kategorií funkce lze vytvořit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="27425-129">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="27425-130">Zkontrolujte hello distribuční sloupce hello kategorií:</span><span class="sxs-lookup"><span data-stu-id="27425-130">Inspect hello distribution of hello categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="27425-131">Generování hodnot ukazatele pro jednotlivé hodnoty ve sloupcích hello</span><span class="sxs-lookup"><span data-stu-id="27425-131">Generate indicator values for each of hello column values</span></span>
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="27425-132">Připojení k hello indikátor sloupec s hello původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="27425-132">Join hello indicator column with hello original data frame</span></span> 
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="27425-133">Odebrání hello původní proměnná sám sebe:</span><span class="sxs-lookup"><span data-stu-id="27425-133">Remove hello original variable itself:</span></span>
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="27425-134"><a name="blob-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="27425-134"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="27425-135">Pro generování binned funkce, budeme postupovat takto:</span><span class="sxs-lookup"><span data-stu-id="27425-135">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="27425-136">Přidat posloupnost toobin sloupce je číselný sloupec</span><span class="sxs-lookup"><span data-stu-id="27425-136">Add a sequence of columns toobin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="27425-137">Převést přihrádkování tooa pořadí boolean proměnných</span><span class="sxs-lookup"><span data-stu-id="27425-137">Convert binning tooa sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="27425-138">Nakonec připojení hello fiktivní proměnné back toohello původní data rámečku</span><span class="sxs-lookup"><span data-stu-id="27425-138">Finally, Join hello dummy variables back toohello original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <span data-ttu-id="27425-139"><a name="sql-featuregen"></a>Zápis dat zálohovat tooAzure objektů blob a využívání v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="27425-139"><a name="sql-featuregen"></a>Writing data back tooAzure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="27425-140">Poté, co jste prozkoumali hello dat a vytvořit hello nezbytné funkce, můžete nahrát hello data (vzorkovat nebo featurized) tooan Azure blob a využívat v Azure Machine Learning pomocí následujících kroků hello: Další funkce můžete vytvářet v hello Azure Machine Learning Studio také.</span><span class="sxs-lookup"><span data-stu-id="27425-140">After you have explored hello data and created hello necessary features, you can upload hello data (sampled or featurized) tooan Azure blob and consume it in Azure Machine Learning using hello following steps: Note that additional features can be created in hello Azure Machine Learning Studio as well.</span></span> 

1. <span data-ttu-id="27425-141">Zápis hello dat rámce toolocal souboru</span><span class="sxs-lookup"><span data-stu-id="27425-141">Write hello data frame toolocal file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="27425-142">Nahrajte objekt blob tooAzure data hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="27425-142">Upload hello data tooAzure blob as follows:</span></span>
   
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
3. <span data-ttu-id="27425-143">Teď můžete číst hello data z hello blob pomocí Azure Machine Learning hello [importovat Data] [ import-data] modulu, jak je znázorněno v úvodní obrazovka níže:</span><span class="sxs-lookup"><span data-stu-id="27425-143">Now hello data can be read from hello blob using hello Azure Machine Learning [Import Data][import-data] module as shown in hello screen below:</span></span>

![Čtečka objektů blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

