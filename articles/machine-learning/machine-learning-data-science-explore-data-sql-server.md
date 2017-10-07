---
title: "aaaExplore data v SQL serveru virtuálního počítače na Azure | Microsoft Docs"
description: "Jak tooexplore data, která je uložená ve virtuálním počítači serveru SQL v Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fcc449fc0d0e49be9b673cfb2de347cf44804017
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Zkoumání dat na virtuálním počítači s SQL Serverem v Azure
Tento dokument popisuje jak tooexplore data, která je uložená ve virtuálním počítači serveru SQL v Azure. Tento krok můžete provést pomocí dat wrangling pomocí SQL nebo pomocí programovacího jazyka jako Python.

Následující Hello **nabídky** odkazy tootopics, které popisují, jak toouse nástroje tooexplore data z různých prostředích úložiště. Tato úloha je krok v hello Cortana Analytics procesu (CAP).

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> Hello vzorové příkazy SQL v tomto dokumentu předpokládají, že data jsou v systému SQL Server. Pokud tomu tak není, podívejte toohello cloudu datové vědy proces mapy toolearn jak toomove vaše data tooSQL serveru.
> 
> 

## <a name="sql-dataexploration"></a>Prozkoumejte data SQL pomocí skriptů SQL
Tady jsou několik ukázkové skripty SQL, které se dají použít tooexplore datová úložiště v systému SQL Server.

1. Získat hello počet připomínky za den
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Získat hello úrovně ve sloupci kategorií
   
    `select  distinct <column_name> from <databasename>`
3. Získat hello počet úrovní v kombinaci dvou kategorií sloupců 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Získat hello distribuce pro číselné sloupce
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Praktické příklad, můžete použít hello [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a toohello IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.
> 
> 

## <a name="python"></a>Prozkoumejte data SQL s Pythonem
Pomocí Python tooexplore data a vygenerovat funkce, když hello data jsou v systému SQL Server je podobná tooprocessing data v Azure blob pomocí Python, jak je uvedeno v [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md). Hello data musí načíst z databáze hello do pandas DataFrame toobe a pak můžete další zpracování. Jsme dokumentů hello proces připojení toohello databázi a načítání dat hello do hello DataFrame v této části.

Hello následující formátu řetězce připojení může být databáze systému SQL Server používané tooconnect tooa z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python. Hello následující kód čte vráceny výsledky hello do rámečku Pandas data z databáze SQL serveru:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Teď můžete pracovat s hello Pandas DataFrame jako popsané v tématu hello [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Proces Cortana analýzy v příkladu akce
Příklad začátku do konce návod hello Cortana procesu analýzy použití veřejné datové sady, najdete v části [hello proces vědecké účely Team dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).

