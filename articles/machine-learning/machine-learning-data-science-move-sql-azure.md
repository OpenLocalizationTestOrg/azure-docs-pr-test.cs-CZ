---
title: "aaaMove data tooan databáze SQL Azure pro Azure Machine Learning | Microsoft Docs"
description: "Vytvoření tabulky SQL a načtení dat tooSQL tabulky"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 50f8b862-4d32-44b2-a1e2-4fbc8024acaa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: b33ef836f42c17a56794baf763281e9998b16bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooan-azure-sql-database-for-azure-machine-learning"></a>Přesunutí dat tooan databáze SQL Azure pro Azure Machine Learning
Toto téma popisuje možnosti hello pro přesun dat z plochých souborů (formáty CSV nebo TSV) nebo z dat uložených v místní systém SQL Server tooan Azure SQL database. Tyto úlohy pro přesunutí dat v cloudu toohello jsou součástí hello proces vědecké účely dat týmu.

Pro téma, které popisuje možnosti hello přesunutí dat tooan místní systém SQL Server pro Machine Learning, najdete v části [přesunout data tooSQL Server na virtuální počítač Azure](machine-learning-data-science-move-sql-server-virtual-machine.md).

Následující Hello **nabídky** odkazy tootopics, které popisují, jak tooingest data do cílové prostředí, kde můžete ukládat a zpracovávat během hello data hello tým datové vědy procesu (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Hello následující tabulka shrnuje hello možnosti pro přesunutí dat tooan Azure SQL Database.

| <b>ZDROJ</b> | <b>Cíl: Azure SQL Database</b> |
| --- | --- |
| <b>Plochý soubor (CSV nebo formátu TSV)</b> |<a href="#bulk-insert-sql-query">Hromadné vložení SQL dotazu |
| <b>Místní SQL Server</b> |1. <a href="#export-flat-file">Export tooFlat souboru<br> 2. <a href="#insert-tables-bcp">Průvodce migrací databáze SQL<br> 3. <a href="#db-migration">Databáze back up a obnovení<br> 4. <a href="#adf">Azure Data Factory |

## <a name="prereqs"></a>Požadavky
Hello postupů uvedených v tomto poli vyžadovat, že máte:

* **Předplatné**. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Účtu úložiště Azure**. Používáte účet úložiště Azure pro ukládání dat hello v tomto kurzu. Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) článku. Po vytvoření účtu úložiště hello musíte tooobtain hello účet, který klíč používá tooaccess hello úložiště. V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Přístup k tooan **Azure SQL Database**. Pokud je potřeba nastavit Azure SQL Database, [Začínáme se službou Microsoft Azure SQL Database](../sql-database/sql-database-get-started.md) obsahuje informace o tooprovision novou instanci třídy Azure SQL Database.
* Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně. Pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

**Data**: migrace procesy hello je ukázán pomocí hello [datovou sadu NYC taxíkem](http://chriswhong.com/open-data/foil_nyc_taxi/). Hello NYC taxíkem datová sada obsahuje informace o cestě dat a veletrzích a je k dispozici v úložišti objektů blob Azure: [NYC taxíkem Data](http://www.andresmh.com/nyctaxitrips/). Ukázka a popis tyto soubory jsou uvedeny v [NYC taxíkem služebních cest datovou sadu popis](machine-learning-data-science-process-sql-walkthrough.md#dataset).

Můžete přizpůsobit hello postupy popsané sem tooa sadu svoje vlastní data, nebo postupujte podle kroků hello, jak je popsáno pomocí hello NYC taxíkem datovou sadu. tooupload hello NYC taxíkem datové sady do místní databáze systému SQL Server, postupujte podle uvedených v postupu hello [hromadně importovat Data do databáze serveru SQL](machine-learning-data-science-process-sql-walkthrough.md#dbload). Tyto pokyny jsou pro systém SQL Server na virtuální počítač Azure, ale hello hello postup pro odesílání toohello místní SQL Server je stejný.

## <a name="file-to-azure-sql-database"></a>Přesun dat z databáze Azure SQL tooan zdroj plochý soubor
Data v plochých souborů (formátu CSV nebo TSV) může být přesunutý tooan Azure SQL database pomocí hromadné vložení dotazu SQL.

### <a name="bulk-insert-sql-query"></a>Hromadné vložení SQL dotazu
Hello kroky pro proceduru hello pomocí hello hromadné vložení dotazu SQL jsou podobné toothose popsané v částech hello pro přesun dat z tooSQL plochý soubor zdrojového serveru na virtuálním počítači Azure. Podrobnosti najdete v tématu [hromadné vložení dotazu SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a>Přesun dat z databáze Azure SQL tooan místní SQL Server
Pokud hello zdrojových dat je uložený v SQL serveru místní, existují různé možnosti pro přesunutí hello data tooan Azure SQL database:

1. [Export tooFlat souboru](#export-flat-file)
2. [Průvodce migrací databáze SQL](#insert-tables-bcp)
3. [Databáze back up a obnovení](#db-migration)
4. [Azure Data Factory](#adf)

Hello kroky pro první tři hello jsou velmi podobné toothose oddíly v [přesunout data tooSQL Server na virtuální počítač Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) , zahrnují tyto stejné postupy. Odkazy toohello příslušné části v tomto tématu jsou uvedeny v hello pokynů.

### <a name="export-flat-file"></a>Export tooFlat souboru
Hello kroky pro tento export plochý soubor tooa jsou podobné toothose zahrnutých v [exportovat tooFlat souboru](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>Průvodce migrací databáze SQL
Hello postup použití hello Průvodce migrací databáze SQL jsou podobné toothose zahrnutých v [Průvodce migrací databáze SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Databáze back up a obnovení
Postup Hello použití databáze zálohování a obnovení jsou podobné toothose zahrnutých v [databáze back up a obnovení](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Azure Data Factory
Hello postup přesunutí dat tooan Azure SQL database s Azure Data Factory (ADF) najdete v tématu hello [přesun dat z tooSQL serveru SQL místní Azure s Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md). Toto téma ukazuje, jak databáze toomove data z databáze tooan systému SQL Server místní Azure SQL přes Azure Blob Storage pomocí ADF.

Zvažte použití ADF při data potřebám toobe průběžně migraci v hybridním scénáři, který má přístup k místní a cloudové prostředky a při hello dat je zpracován, nebo musí toobe upravené nebo obchodní logiku přidali tooit při migraci. ADF umožňuje hello plánování a sledování úloh pomocí jednoduché skripty JSON, které spravují hello přesouvání dat v pravidelných intervalech. ADF má také další funkce, například podporu pro komplexní operace.
