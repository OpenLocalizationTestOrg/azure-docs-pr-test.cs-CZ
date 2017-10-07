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
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Zkoumání dat ve službě Azure Blob Storage pomocí knihovny Pandas
Tento dokument popisuje, jak tooexplore data uložená v Azure blob pomocí kontejneru [Pandas](http://pandas.pydata.org/) balíček Python.

Následující Hello **nabídky** odkazy tootopics, které popisují, jak toouse nástroje tooexplore data z různých prostředích úložiště. Tato úloha je krok v hello [proces vědecké účely dat]().

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* Vytvořit účet úložiště Azure. Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Vaše data uložena v účtu úložiště objektů blob v Azure. Pokud budete potřebovat pokyny, najdete v části [tooand přesouvání dat z Azure Storage](../storage/common/storage-moving-data.md)

## <a name="load-hello-data-into-a-pandas-dataframe"></a>Načtení dat hello do Pandas DataFrame
tooexplore a pracovat s datovou sadu, se musí nejprve stáhnout z hello objektů blob zdroj tooa místního souboru, které pak mohou být načteny v Pandas DataFrame. Zde jsou kroky toofollow hello pro tento postup:

1. Stáhněte si hello data z Azure blob s hello následující ukázka kódu Pythonu pro pomocí služby objektů blob. Nahraďte proměnnou hello v hello následující kód s konkrétními hodnotami: 
   
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

## <a name="blob-dataexploration"></a>Příklady zkoumání dat pomocí Pandas
Zde je několik příkladů způsoby tooexplore dat pomocí Pandas:

1. Zkontrolujte hello **počet řádků a sloupců** 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Zkontrolujte** hello první nebo poslední několik **řádky** v hello následující datové sady:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Zkontrolujte hello **datový typ** každý sloupec byl importován jako pomocí hello následující ukázkový kód
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Zkontrolujte hello **základní statistiky** pro hello sloupců v následujícím způsobem hello datové sady
   
        dataframe_blobdata.describe()
5. Podívejte se na hello počet položek pro každou hodnotu sloupce následujícím způsobem
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Počet chybějících hodnot** versus hello skutečný počet položek v jednotlivých sloupcích pomocí hello následující ukázkový kód
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Pokud máte **chybějící hodnoty** pro konkrétní sloupec v hello data, můžete jejich umístění následujícím způsobem:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape
   
   Jiný způsob tooreplace chybějící hodnoty je pomocí funkce režimu hello:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]        
8. Vytvoření **histogram** vykreslení pomocí proměnný počet přihrádek tooplot hello distribuční proměnné    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Podívejte se na **korelací** mezi proměnné pomocí scatterplot nebo pomocí funkce integrované korelace hello
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

