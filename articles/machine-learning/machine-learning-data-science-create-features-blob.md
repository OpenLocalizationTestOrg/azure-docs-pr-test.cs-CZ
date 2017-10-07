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
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a>Vytvoření funkcí pro data služby Azure Blob Storage pomocí knihovny Pandas
Tento dokument ukazuje, jak funkce toocreate pro data, která je uložená v kontejneru objektů blob v Azure pomocí hello [Pandas](http://pandas.pydata.org/) balíček Python. Po osnovy jak tooload hello dat do data rámečku Panda, ukazuje způsob toogenerate kategorií funkce pomocí skriptů Python s hodnotami indikátoru a přihrádkování funkce.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

To **nabídky** odkazy tootopics, které popisují, jak funkce toocreate pro data v různých prostředích. Tato úloha je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste vytvořili účet úložiště objektů blob v Azure a jsou uloženy vaše data existuje. Pokud budete potřebovat pokyny tooset si účet, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)

## <a name="load-hello-data-into-a-pandas-data-frame"></a>Načtení dat hello do rámečku Pandas dat
V pořadí toodo prozkoumat a upravit datovou sadu, se musí stáhnout z hello blob zdroj tooa místního souboru, který lze načíst v rámci Pandas data. Zde jsou kroky toofollow hello pro tento postup:

1. Stáhněte si hello data z Azure blob s hello následující ukázkový kód Python pomocí služby objektů blob. Nahraďte konkrétní hodnoty proměnné hello v hello kódu níže:
   
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
2. Čtení hello dat do data rámečku Pandas z hello stáhli soubor.
   
        #LOCALFILE is hello file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nyní jsou připravené tooexplore hello data a vygenerovat funkce pro tuto datovou sadu.

## <a name="blob-featuregen"></a>Funkce generování
Hello další dvě části vysvětlují, jak toogenerate kategorií funkce s hodnotami indikátoru a přihrádkování funkce pomocí skriptů Python.

### <a name="blob-countfeature"></a>Hodnota ukazatele na základě funkce generování
Kategorií funkce lze vytvořit následujícím způsobem:

1. Zkontrolujte hello distribuční sloupce hello kategorií:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Generování hodnot ukazatele pro jednotlivé hodnoty ve sloupcích hello
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Připojení k hello indikátor sloupec s hello původní data rámečku
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Odebrání hello původní proměnná sám sebe:
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Přihrádkování funkce generování
Pro generování binned funkce, budeme postupovat takto:

1. Přidat posloupnost toobin sloupce je číselný sloupec
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Převést přihrádkování tooa pořadí boolean proměnných
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Nakonec připojení hello fiktivní proměnné back toohello původní data rámečku
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <a name="sql-featuregen"></a>Zápis dat zálohovat tooAzure objektů blob a využívání v Azure Machine Learning
Poté, co jste prozkoumali hello dat a vytvořit hello nezbytné funkce, můžete nahrát hello data (vzorkovat nebo featurized) tooan Azure blob a využívat v Azure Machine Learning pomocí následujících kroků hello: Další funkce můžete vytvářet v hello Azure Machine Learning Studio také.

1. Zápis hello dat rámce toolocal souboru
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Nahrajte objekt blob tooAzure data hello následujícím způsobem:
   
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
3. Teď můžete číst hello data z hello blob pomocí Azure Machine Learning hello [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulu, jak je znázorněno v úvodní obrazovka níže:

![Čtečka objektů blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

