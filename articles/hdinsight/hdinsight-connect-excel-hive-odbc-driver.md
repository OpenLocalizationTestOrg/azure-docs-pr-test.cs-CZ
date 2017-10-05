---
title: "Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Hive - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak nastavit a používat ovladač Microsoft Hive ODBC pro aplikaci Excel k dotazování na data v clusterech HDInsight z aplikace Microsoft Excel."
keywords: "v aplikaci excel hadoop, hive v aplikaci excel, hive rozhraní odbc"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: 7818093e42c34ee671a035cde783a6622fb2a798
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Připojení aplikace Excel k systému Hadoop v prostředí Azure HDInsight pomocí ovladače Microsoft Hive ODBC

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Řešení velkých objemů dat společnosti Microsoft se integruje s clustery Apache Hadoop, které jsou nasazené v Azure Hdinsightu součásti Microsoft Business Intelligence (BI). Příklad této integrace je schopnost připojení aplikace Excel do datového skladu Hive clusteru Hadoop v HDInsight pomocí ovladače Microsoft Hive připojení ODBC (Open Database).

Také je možné připojit data související s clusteru služby HDInsight a dalších zdrojů dat, včetně jiných clusterů systému Hadoop (bez HDInsight), z aplikace Excel pomocí doplňku Microsoft Power Query pro Excel. Informace o instalaci a používání doplňku Power Query najdete v tématu [připojení aplikace Excel do prostředí HDInsight pomocí doplňku Power Query][hdinsight-power-query].

> [!NOTE]
> Zatímco kroky v tomto článku lze použít s clusterem Linux nebo HDInsight se systémem Windows, je vyžadována pro klientské pracovní stanice systému Windows.
> 
> 

**Požadavky**:

Před zahájením tohoto článku, musíte mít následující položky:

* **Cluster služby HDInsight**. Chcete-li vytvořit, přečtěte si téma [Začínáme s Azure HDInsight][hdinsight-get-started].
* **Pracovní stanice** Office 2013 Professional Plus, Office 365 Pro Plus, samostatné aplikace Excel 2013 nebo Office 2010 Professional Plus.

## <a name="install-microsoft-hive-odbc-driver"></a>Instalaci ovladače Microsoft Hive ODBC
Stáhnout a nainstalovat ovladače ODBC Microsoft Hive z [Download Center][hive-odbc-driver-download].

Tento ovladač lze nainstalovat na 32bitové nebo 64bitové verze Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 a Windows Server 2012. Ovladač umožňuje připojení k Azure HDInsight (verze 1.6 nebo novější) a emulátoru HDInsight Azure (v.1.0.0.0 a novější). Nainstalujete verzi, která odpovídá verzi aplikace, kde používáte ovladač ODBC. V tomto kurzu se používá ovladače z aplikace Office Excel.

## <a name="create-hive-odbc-data-source"></a>Vytvoření zdroje dat Hive ODBC
Následující kroky ukazují, jak vytvořit zdroj dat ODBC Hive.

1. Ze systému Windows 8 nebo Windows 10, stiskněte klávesu Windows k otevření obrazovky Start a potom zadejte **zdroje dat**.
2. Klikněte na tlačítko **nastavení zdroje dat ODBC (32 bitů)** nebo **nastavení zdroje dat ODBC (64 bitů)** v závislosti na vaší verzi Office. Pokud používáte systém Windows 7, vyberte **zdroje dat ODBC (32 bitů)** nebo **zdroje dat ODBC (64 bitů)** z **nástroje pro správu**. Zobrazí se **správce zdrojů dat ODBC** dialogové okno.
   
    ![Správce zdrojů dat ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurace názvu DSN pomocí Správce zdrojů dat ODBC")

3. Z DNS uživatele, klikněte na tlačítko **přidat** otevřete **vytvořit nový zdroj dat** průvodce.
4. Vyberte **ovladače ODBC Microsoft Hive**a potom klikněte na **Dokončit**. Zobrazí se **Microsoft DNS ovladače ODBC Hive instalace** dialogové okno.
5. Zadejte nebo vyberte tyto hodnoty:
   
   | Vlastnost | Popis |
   | --- | --- |
   |  Název zdroje dat |Zadejte název zdroje dat. |
   |  Hostitel |Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net. Například mujHDICluster.azurehdinsight.net. |
   |  Port |Použijte <strong>443</strong>. (Tento port se změnil z 563 na 443.) |
   |  Databáze |Použijte <strong>Výchozí</strong>. |
   |  Mechanismus |Vyberte <strong>Služba Azure HDInsight</strong> |
   |  Uživatelské jméno |Zadejte uživatelské jméno uživatele ve HTTP clusteru HDInsight. Výchozí uživatelské jméno <strong>admin</strong>. |
   |  Heslo |Zadejte heslo uživatele clusteru HDInsight. |
   
    </table>
   
    Existují některé důležité parametry zajímat když kliknete na tlačítko **pokročilé možnosti**:
   
   | Parametr | Popis |
   | --- | --- |
   |  Použít nativní dotaz |Pokud je zaškrtnuto, ovladač ODBC není pokusí převést TSQL HiveQL. Musí se použít pouze v případě, že jste se, že jste odeslali čistý příkazy HiveQL 100 %. Při připojování k systému SQL Server nebo Azure SQL Database, by měl nechte nezaškrtnuté. |
   |  Počet získaných za blok řádků |Při načítání velkému počtu záznamů, optimalizace pro tento parametr může být nutné zajistit optimální přínos. |
   |  Výchozí délka sloupce řetězec, délka binárního sloupce, měřítko Decimal sloupce |Datový typ délky a přesnosti může mít vliv na tom, jak je vrácená data. Způsobí nesprávné informace o který se má vrátit z důvodu ztráty přesnost nebo zkrácení. |

    ![Rozšířené možnosti](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN rozšířené možnosti konfigurace")

1. Klikněte na tlačítko **testování** zdroj dat otestovat. Zdroj dat správně nakonfigurovaný, zobrazuje *testy byla úspěšně DOKONČENA.!*.
2. Klikněte na tlačítko **OK** zavřete dialogové okno Test. Nový zdroj dat je uvedeno v **správce zdrojů dat ODBC**.
3. Klikněte na tlačítko **OK** ukončíte průvodce.

## <a name="import-data-into-excel-from-hdinsight"></a>Import dat do Excelu ze služby HDInsight
Následující kroky popisují způsob, jak můžete importovat data z tabulky Hive do sešitu aplikace Excel pomocí zdroje dat ODBC, kterou jste vytvořili v předchozí části.

1. V Excelu otevřete nový nebo existující sešit.
2. Z **Data** , klikněte na **načíst Data**, klikněte na tlačítko **z jiných zdrojů**a potom klikněte na **z rozhraní ODBC** ke spuštění **dat Průvodce připojením**.
   
    ![Průvodce připojením Open data](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data Průvodce připojením")
4. Vyberte název zdroje dat, který jste vytvořili v poslední části a pak klikněte na tlačítko **OK**.
5. Zadejte uživatelské jméno Hadoop (výchozí název je správce) a heslo a pak klikněte na tlačítko **Connect**.
6. Navigátor, rozbalte **HIVE**, rozbalte položku **výchozí**, klikněte na tlačítko **hivesampletable**a potom klikněte na **zatížení**. Import dat do Excelu trvá několik sekund.

    ![Navigátor HDInsight Hive ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data Průvodce připojením")


## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak používat ovladač Microsoft Hive ODBC k načtení dat ze služby HDInsight do aplikace Excel. Podobně můžete načíst data ze služby HDInsight do databáze SQL. Je také možné nahrát data do služby HDInsight. Další informace naleznete v tématu:

* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]
* [Nahrání dat do HDInsight][hdinsight-upload-data]
* [Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


