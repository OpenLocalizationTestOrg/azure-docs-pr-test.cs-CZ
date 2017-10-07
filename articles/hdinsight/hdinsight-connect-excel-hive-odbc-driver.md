---
title: "aaaConnect tooHadoop Excel s hello ovladače ODBC Hive - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooset nahoru a používání hello ovladače Microsoft Hive ODBC pro aplikaci Excel tooquery data v clusterech HDInsight z aplikace Microsoft Excel."
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
ms.openlocfilehash: f01f89e7d4203c739d56079dc589fc11f4aa2174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-in-azure-hdinsight-with-hello-microsoft-hive-odbc-driver"></a>Připojení aplikace Excel tooHadoop v Azure HDInsight pomocí ovladače Microsoft Hive ODBC hello

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Řešení velkých objemů dat společnosti Microsoft se integruje s clustery systému Apache Hadoop nasazených hello Azure HDInsight součásti Microsoft Business Intelligence (BI). Příklad této integrace je hello možnost tooconnect Excel toohello Hive datový sklad clusteru Hadoop v HDInsight pomocí hello ovladač Microsoft Hive připojení ODBC (Open Database).

Je také možné tooconnect hello data přidružená k clusteru služby HDInsight a jiných zdrojů dat, včetně dalších (bez HDInsight) clusterů systému Hadoop, z Excelu pomocí hello doplňku Microsoft Power Query pro Excel. Informace o instalaci a používání doplňku Power Query najdete v tématu [tooHDInsight připojení aplikace Excel pomocí doplňku Power Query][hdinsight-power-query].

> [!NOTE]
> Během hello kroky tomto článku lze použít s buď Linux nebo clusteru HDInsight se systémem Windows, Windows se vyžaduje pro hello klientské pracovní stanice.
> 
> 

**Požadavky**:

Před zahájením tohoto článku, musíte mít hello následující položky:

* **Cluster služby HDInsight**. toocreate, najdete v části [Začínáme s Azure HDInsight][hdinsight-get-started].
* **Pracovní stanice** Office 2013 Professional Plus, Office 365 Pro Plus, samostatné aplikace Excel 2013 nebo Office 2010 Professional Plus.

## <a name="install-microsoft-hive-odbc-driver"></a>Instalaci ovladače Microsoft Hive ODBC
Stáhnout a nainstalovat ovladače ODBC Microsoft Hive ze hello [Download Center][hive-odbc-driver-download].

Tento ovladač lze nainstalovat na 32bitové nebo 64bitové verze Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 a Windows Server 2012. ovladač Hello umožňuje připojení tooAzure HDInsight (verze 1.6 nebo novější) a emulátoru HDInsight Azure (v.1.0.0.0 a novější). Nainstalujete hello verze, která odpovídá hello verze aplikace hello kde pomocí ovladače ODBC hello. V tomto kurzu se používá hello ovladače z aplikace Office Excel.

## <a name="create-hive-odbc-data-source"></a>Vytvoření zdroje dat Hive ODBC
Hello následující kroky ukazují, jak toocreate Hive zdroje dat ODBC.

1. Ze systému Windows 8 nebo Windows 10, stiskněte hello Windows klíče tooopen hello úvodní obrazovce a potom zadejte **zdroje dat**.
2. Klikněte na tlačítko **nastavení zdroje dat ODBC (32 bitů)** nebo **nastavení zdroje dat ODBC (64 bitů)** v závislosti na vaší verzi Office. Pokud používáte systém Windows 7, vyberte **zdroje dat ODBC (32 bitů)** nebo **zdroje dat ODBC (64 bitů)** z **nástroje pro správu**. Zobrazí se hello **správce zdrojů dat ODBC** dialogové okno.
   
    ![Správce zdrojů dat ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurace názvu DSN pomocí Správce zdrojů dat ODBC")

3. Z DNS uživatele, klikněte na tlačítko **přidat** tooopen hello **vytvořit nový zdroj dat** průvodce.
4. Vyberte **ovladače ODBC Microsoft Hive**a potom klikněte na **Dokončit**. Zobrazí se hello **Microsoft DNS ovladače ODBC Hive instalace** dialogové okno.
5. Zadejte nebo vyberte hello následující hodnoty:
   
   | Vlastnost | Popis |
   | --- | --- |
   |  Název zdroje dat |Zadejte název zdroje dat tooyour |
   |  Hostitel |Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net. Například mujHDICluster.azurehdinsight.net. |
   |  Port |Použijte <strong>443</strong>. (Tento port se změnil z 563 too443.) |
   |  Databáze |Použijte <strong>Výchozí</strong>. |
   |  Mechanismus |Vyberte <strong>Služba Azure HDInsight</strong> |
   |  Uživatelské jméno |Zadejte uživatelské jméno uživatele ve HTTP clusteru HDInsight. výchozí uživatelské jméno Hello <strong>správce</strong>. |
   |  Heslo |Zadejte heslo uživatele clusteru HDInsight. |
   
    </table>
   
    Existují některé důležité parametry toobe vědět, když kliknete na tlačítko **pokročilé možnosti**:
   
   | Parametr | Popis |
   | --- | --- |
   |  Použít nativní dotaz |Pokud je zaškrtnuto, ovladač ODBC hello nezkusí tooconvert TSQL do HiveQL. Musí se použít pouze v případě, že jste se, že jste odeslali čistý příkazy HiveQL 100 %. Při připojování tooSQL serveru nebo v databázi SQL Azure, by měl nechte nezaškrtnuté. |
   |  Počet získaných za blok řádků |Při načítání velkému počtu záznamů, optimalizace pro tento parametr může být požadované tooensure přínos optimální. |
   |  Výchozí délka sloupce řetězec, délka binárního sloupce, měřítko Decimal sloupce |Hello datový typ délky a přesnosti může mít vliv na tom, jak je vrácená data. Způsobí nesprávné informace o toobe vrácená z důvodu tooloss přesnost nebo zkrácení. |

    ![Rozšířené možnosti](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN rozšířené možnosti konfigurace")

1. Klikněte na tlačítko **Test** zdroj dat tootest hello. Zdroj dat hello je správně nakonfigurovaný, zobrazuje *testy byla úspěšně DOKONČENA.!*.
2. Klikněte na tlačítko **OK** tooclose hello Test – dialogové okno. Hello nový zdroj dat je uvedeno v hello **správce zdrojů dat ODBC**.
3. Klikněte na tlačítko **OK** tooexit hello průvodce.

## <a name="import-data-into-excel-from-hdinsight"></a>Import dat do Excelu ze služby HDInsight
Hello následující kroky popisují hello způsob tooimport data z tabulky Hive do sešitu aplikace Excel pomocí hello zdroje dat ODBC, který jste vytvořili v předchozí části hello.

1. V Excelu otevřete nový nebo existující sešit.
2. Z hello **Data** , klikněte na **načíst Data**, klikněte na tlačítko **z jiných zdrojů**a potom klikněte na **z rozhraní ODBC** toolaunch hello  **Průvodce datovým připojením**.
   
    ![Průvodce připojením Open data](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data Průvodce připojením")
4. Název, který jste vytvořili v poslední části hello zdroje dat vyberte hello a potom klikněte na **OK**.
5. Zadejte uživatelské jméno Hadoop (hello výchozí název je správce) a hello heslo a pak klikněte na tlačítko **Connect**.
6. Navigátor, rozbalte **HIVE**, rozbalte položku **výchozí**, klikněte na tlačítko **hivesampletable**a potom klikněte na **zatížení**. Jak dlouho trvá několik sekund, než získá importované tooExcel data.

    ![Navigátor HDInsight Hive ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data Průvodce připojením")


## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak toouse hello data tooretrieve ovladače Microsoft Hive ODBC z hello služba HDInsight do aplikace Excel. Podobně můžete data načíst z hello služba HDInsight do databáze SQL. Je také možné tooupload data do služby HDInsight. toolearn více, najdete v části:

* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]
* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


