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
# <a name="heading"></a>Zpracování dat objektů blob v Azure s pokročilou analýzu
Tento dokument popisuje seznámení data a funkce generování z dat uložených v Azure Blob storage. 

## <a name="load-hello-data-into-a-pandas-data-frame"></a>Načtení dat hello do rámečku Pandas dat
V pořadí tooexplore a pracovat s datovou sadu, se musí stáhnout z hello blob zdroj tooa místního souboru, který lze načíst v rámci Pandas data. Zde jsou kroky toofollow hello pro tento postup:

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

## <a name="blob-dataexploration"></a>Zkoumání dat
Zde je několik příkladů způsoby tooexplore dat pomocí Pandas:

1. Zkontrolujte hello počet řádků a sloupců 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Zkontrolujte hello první nebo poslední několik řádků v datové sadě hello jak je uvedeno níže:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Zkontrolujte hello datový typ, který každý sloupec byl importován jako pomocí hello následující ukázkový kód
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Zkontrolujte hello základní statistiky pro hello sloupců v datové sadě hello následujícím způsobem
   
        dataframe_blobdata.describe()
5. Podívejte se na hello počet položek pro každou hodnotu sloupce následujícím způsobem
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Počet chybějících hodnot versus hello skutečný počet položek v jednotlivých sloupcích pomocí hello následující ukázkový kód
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Pokud máte chybějící hodnoty pro konkrétní sloupec v datech hello, vyřaďte je následujícím způsobem:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape
   
   Jiný způsob tooreplace chybějící hodnoty je pomocí funkce režimu hello:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]        
8. Vytvoření histogram vykreslení pomocí proměnný počet přihrádek tooplot hello distribuční proměnné    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Podívejte se na korelací mezi proměnné pomocí scatterplot nebo pomocí funkce integrované korelace hello
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Funkce generování
Můžete se vygeneruje funkcí s použitím Python následujícím způsobem:

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
3. Teď můžete číst hello data z hello blob pomocí Azure Machine Learning hello [importovat Data] [ import-data] modulu, jak je znázorněno v úvodní obrazovka níže:

![Čtečka objektů blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

