---
title: "aaaSample dat v systému SQL Server na platformě Azure | Microsoft Docs"
description: "Ukázková Data v systému SQL Server v Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: dc7f9529c771f6deb633775557e64a04b774f5b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Ukázková data v systému SQL Server v Azure
Tento dokument ukazuje, jak toosample data uložená v systému SQL Server na platformě Azure pomocí SQL nebo hello programovací jazyk Python. Také ukazuje, jak toomove vzorků dat do Azure Machine Learning uložením souboru tooa, pak ho nahrát tooan objektů blob v Azure a pak ho čtení do Azure Machine Learning Studio.

vzorkování Python Hello používá hello [pyodbc](https://code.google.com/p/pyodbc/) rozhraní ODBC knihovny tooconnect tooSQL Server v Azure a hello [Pandas](http://pandas.pydata.org/) knihovny toodo hello vzorkování.

> [!NOTE]
> Hello ukázkový SQL kód v tomto dokumentu se předpokládá, že hello dat je v systému SQL Server na platformě Azure. Pokud není, naleznete příliš[přesunout data tooSQL Server na platformě Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) tématu pokyny, jak toomove vaše data tooSQL Server na platformě Azure.
> 
> 

Následující Hello **nabídky** odkazy tootopics, které popisují, jak toosample data z různých prostředích úložiště. 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Proč ukázková data?**
Pokud datovou sadu hello plánování tooanalyze velká, je obvykle vhodné hello toodown ukázková data tooreduce ho tooa menší, ale reprezentativní a lepší správu bitlockeru velikost. To usnadňuje pochopení dat, zkoumání a funkce inženýrství. Jeho role v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je rychlé vytváření prototypů tooenable hello zpracování dat funkcí a modelů strojového učení.

Tato úloha vzorkování je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>Pomocí SQL
Tato část popisuje několik metod, pomocí SQL tooperform jednoduché náhodné vzorkování pro hello data v databázi hello. Vyberte metodu na základě vaší velikost dat a jeho distribuci.

Hello dvě položky ukazují, jak toouse newid v systému SQL Server tooperform hello vzorkování. Hello metoda zvolíte, závisí na jak náhodné chcete toobe ukázka hello (hodnota pk_id v hello následující ukázkový kód je toobe automaticky vygeneruje primární klíč).

1. Méně přísná náhodného vzorku
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Více náhodného vzorku 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Klauzule Tablesample může být použit pro vzorkování, stejně jako ukázáno níže. To může být lepším řešením Pokud (za předpokladu, že data na různých stránkách není korelační) má velká velikost dat a pro toocomplete hello dotazu v přiměřené době.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Vám umožní zkoumat a generovat funkce z této jen Vzorkovaná data ukládáním do nové tabulky
> 
> 

### <a name="sql-aml"></a>Připojení tooAzure Machine Learning
Ukázkové dotazy hello výše můžete použít přímo v Azure Machine Learning hello [importovat Data] [ import-data] modulu hello toodown ukázková data na hello fyzicky dostavili a převeďte ho do experimentu Azure Machine Learning. Snímek obrazovky s použitím hello čtečky modulu tooread hello vzorků dat je zobrazena níže:

![Čtečka sql][1]

## <a name="python"></a>Pomocí programovacího jazyka Python hello
V této části ukazuje, jak pomocí hello [pyodbc knihovny](https://code.google.com/p/pyodbc/) tooestablish ODBC připojení databáze serveru SQL tooa v Pythonu. připojovací řetězec databáze Hello je následující: (nahraďte servername, dbname, uživatelské jméno a heslo s konfigurací):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas](http://pandas.pydata.org/) knihovna v Pythonu obsahuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python. Následující kód Hello načte ukázku 0,1 % hello dat z tabulky v databázi Azure SQL do Pandas dat:

    import pandas as pd

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Teď můžete pracovat s daty hello vzorkovat hello Pandas datové rámce. 

### <a name="python-aml"></a>Připojení tooAzure Machine Learning
Můžete použít následující ukázkový kód toosave hello nižší vzorků dat tooa soubor hello a nahrajte ho tooan objektů blob v Azure. Hello hello objektu BLOB může být přímo číst data do experimentu Azure Machine Learning pomocí hello [importovat Data] [ import-data] modulu. Hello kroky jsou následující: 

1. Zápis hello pandas dat rámce tooa místního souboru
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Nahrát objekt blob tooAzure místního souboru
   
        from azure.storage import BlobService
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
3. Čtení dat z Azure blob pomocí Azure Machine Learning [importovat Data] [ import-data] modulu, jak je znázorněno níže okamžitými hello obrazovky výsledky:

![Čtečka objektů blob][2]

## <a name="hello-team-data-science-process-in-action-example"></a>v příkladu akce Hello procesu Team dat vědecké účely
Příklad začátku do konce návod hello proces vědecké účely Team dat pomocí veřejné datové sady, najdete v tématu [proces vědecké účely Team dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
