---
title: "aaaExplore data virtuálního počítače s SQL serverem v Azure | Microsoft Docs"
description: "Zkoumat data a funkce generování ve virtuálním počítači systému SQL Server v Azure"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 67f4b058b0f6557ee15fd42795c918d68f1a9871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Zpracování dat v virtuálního počítače systému SQL Server v Azure
Tento dokument popisuje jak tooexplore data a vytvářet funkce pro data uložená ve virtuálním počítači serveru SQL v Azure. Tento krok můžete provést pomocí dat wrangling pomocí SQL nebo pomocí programovacího jazyka jako Python.

> [!NOTE]
> Hello vzorové příkazy SQL v tomto dokumentu předpokládají, že data jsou v systému SQL Server. Pokud tomu tak není, podívejte toohello cloudu datové vědy proces mapy toolearn jak toomove vaše data tooSQL serveru.
> 
> 

## <a name="SQL"></a>Pomocí SQL
Jsme popisují hello následující úlohy wrangling dat v této části pomocí SQL:

1. [Zkoumání dat](#sql-dataexploration)
2. [Funkce generování](#sql-featuregen)

### <a name="sql-dataexploration"></a>Zkoumání dat
Tady jsou několik ukázkové skripty SQL, které se dají použít tooexplore datová úložiště v systému SQL Server.

> [!NOTE]
> Praktické příklad, můžete použít hello [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a toohello IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.
> 
> 

1. Získat hello počet připomínky za den
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Získat hello úrovně ve sloupci kategorií
   
    `select  distinct <column_name> from <databasename>`
3. Získat hello počet úrovní v kombinaci dvou kategorií sloupců 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Získat hello distribuce pro číselné sloupce
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Funkce generování
V této části popisují jsme způsoby generování funkcí s použitím SQL:  

1. [Počet na základě funkce generování](#sql-countfeature)
2. [Přihrádkování funkce generování](#sql-binningfeature)
3. [Zavedením hello funkce z jednoho sloupce](#sql-featurerollout)

> [!NOTE]
> Po vygenerování další funkce, můžete je přidat jako sloupce toohello existující tabulky nebo vytvořit novou tabulku s hello další funkce a primární klíč, který lze spojit s původní tabulky hello. 
> 
> 

### <a name="sql-countfeature"></a>Počet na základě funkce generování
Hello následující příklady znázorňují dva způsoby generování funkce count. První metoda Hello používá podmíněného sum a druhý používá metoda hello hello 'where' klauzule. Potom tyto lze spojit s hello původní tabulky (s použitím sloupců primárních klíčů) toohave počet funkcí společně se původní data hello.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>Přihrádkování funkce generování
Hello následující příklad ukazuje, jak toogenerate binned funkce pomocí přihrádkování (pomocí pět přihrádek) číselné sloupce, který lze použít jako funkce místo:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Zavedením hello funkce z jednoho sloupce
V této části ukážeme, jak tooroll na jeden sloupec v tabulce další funkce toogenerate. Hello příklad předpokládá, že se v tabulce hello, ze kterého se pokoušíte toogenerate funkce sloupec zeměpisné šířky nebo délky.

Zde je stručný úvod do na data o umístění zeměpisnou šířku a délku (ze zásobníku se zdroji [jak toomeasure hello přesnost zeměpisnou šířku a délku?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). To je užitečné toounderstand před featurizing hello umístění pole:

* Hello přihlašovací nám oznamuje, zda jsme jsou severně nebo Jih, východ nebo – západ na celém světě hello.
* Nenulové hodnoty stovky číslice víme, že používáme zeměpisné délky, není zeměpisnou šířku!
* Hello desítkami číslice dává tooabout pozice 1 000 kilometrech. Nabízí nám užitečné informace o jaké kontinentě nebo oceánu jsme na.
* Hello jednotky číslice (jeden decimal stupeň) poskytuje pozici nahoru too111 kilometrech (60 mílové, o 69 miles). Je nám říct zhruba jaké velké státu nebo země, které jsme jsou v.
* první desetinné místo Hello je vhodné si too11.1 km: je možné rozlišit hello pozici jedno velké město z sousedních velké města.
* Hello dvě desetinná místa je vhodné si too1.1 km: ho můžete oddělit jeden vesnice vedle z hello.
* Hello třetí desetinné místo je vhodné si too110 m: že poznáte velké zemědělských pole nebo institucionální kanceláře.
* Hello čtvrtého desetinného místa je vhodné si too11 m: že poznáte parcela. Je porovnatelný z hlediska toohello typické přesnost neopravené GPS jednotky s bez narušení.
* Hello páté desetinné místo je vhodné si too1.1 m: že stromy ho odlišuje od sebe navzájem. Přesnost úroveň toothis komerční GPS jednotky lze dosáhnout pouze s rozdílovou oprava.
* Hello šesté desetinné místo je vhodné si too0.11 m: tu můžete použít pro vytvoření rozložení struktury podrobně pro návrh krajiny, vytváření cest. Mělo by být víc než dost vhodný pro sledování pohybu glaciers a řek. Toho lze dosáhnout pomocí painstaking míry s GPS, jako je například differentially opravené GPS.

informace o umístění Hello může být featurized následujícím způsobem oddělení oblast, umístění a informace o městě. Všimněte si, že byste také zavolat koncový bod REST například rozhraní API map Bing k dispozici na [vyhledat umístění bodem](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello oblasti nebo oblastní informace.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Tyto funkce na základě umístění může být další používané toogenerate počet další funkce, jak je popsáno výše. 

> [!TIP]
> Můžete vložit prostřednictvím kódu programu hello záznamů pomocí vámi zvolený jazyk. Potřebujete tooinsert hello data v efektivitu zápisu tooimprove bloky dat (pro příklad toodo tento pomocí pyodbc, najdete v tématu [A HelloWorld ukázkové tooaccess SQLServer s pythonem](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Další alternativou je tooinsert data v databázi hello pomocí hello [nástroj BCP](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Připojení tooAzure Machine Learning
Funkce Hello nově vygenerované můžete přidat jako sloupec existující tabulky tooan nebo ukládat v nové tabulce a propojit s hello původní tabulky pro machine learning. Funkce můžete generovat ani přistupovat, pokud už vytvořili, pomocí hello [importovat Data] [ import-data] modulu v Azure Machine Learning, jak je uvedeno níže:

![azureml čtečky][1] 

## <a name="python"></a>Pomocí programovacího jazyka jako Python
Pomocí Python tooexplore data a vygenerovat funkce, když hello data jsou v systému SQL Server je podobná tooprocessing data v Azure blob, které se používá Python, jak je uvedeno v [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md). Hello data musí toobe načíst z databáze hello do rámečku pandas data a pak můžete další zpracování. Jsme dokumentů hello proces připojení toohello databázi a načítání dat hello do rámečku hello dat v této části.

Hello následující formátu řetězce připojení může být databáze systému SQL Server používané tooconnect tooa z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python. Následující kód Hello čte vráceny výsledky hello do rámečku Pandas data z databáze SQL serveru:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Teď můžete pracovat s rámečkem data Pandas hello jako popsaná v článku hello [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Vědecké zpracování dat Azure v příkladu akce
Příklad začátku do konce návod hello proces vědecké účely dat Azure pomocí veřejné datové sady, najdete v části [proces vědecké účely dat Azure v akci](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

