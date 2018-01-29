---
title: "Vizualizovat velké objemy dat s Power BI v Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Microsoft Power BI můžete vizualizovat Hive data zpracovaná službou Azure HDInsight."
keywords: "hdinsight hadoop, hive, interaktivní dotazu, interaktivní hive, LLAP, odbc"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: 8b4347fde274d0d7520ef3acbd081fdb83d7dc7d
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="visualize-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Vizualizovat Hive data s Microsoft Power BI pomocí rozhraní ODBC v Azure HDInsight

Zjistěte, jak se připojit k Azure HDInsight pomocí ovladače ODBC Microsoft Power BI a vizualizovat Hive data. V tomto kurzu můžete načíst data z tabulky Hive hivesampletable do Power BI. Tabulka Hive obsahuje některé údaje o používání mobilního telefonu. Potom vykreslení dat o využití na mapě world:

![HDInsight Power BI sestavy mapy](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Informace se vztahují taky na nové [interaktivní dotazu](../interactive-query/apache-interactive-query-get-started.md) clusteru typu. Pro připojení k HDInsight interaktivní dotazu pomocí přímých dotazů najdete v tématu [vizualizovat interaktivní Hive dotaz data s Microsoft Power BI pomocí přímých dotazů v Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující položky:

* **HDInsight cluster**. Cluster může být clusteru HDInsight pomocí Hive nebo clusteru s podporou nově vydané interaktivní dotazu. Vytváření clusterů, najdete v části [vytvořit cluster](apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Můžete si stáhnout kopii z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Vytvoření zdroje dat Hive ODBC

V tématu [zdroje dat ODBC Hive vytvořit](apache-hadoop-connect-excel-hive-odbc-driver.md#create-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Načtení dat z HDInsight

Tabulka hivesampletable Hive obsahuje všechny clustery HDInsight.

1. Přihlaste se k Power BI Desktop.
2. Klikněte na tlačítko **Domů** , klikněte na **načíst Data** z **externích dat** pásu karet a potom vyberte **Další**.

    ![Otevřete data HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)
3. Z **načíst Data** podokně klikněte na tlačítko **jiné** na levé straně klikněte na **ODBC** zprava a pak klikněte na **připojit** v dolní.
4. Z **z rozhraní ODBC** podokně, vyberte název jste vytvořili v poslední části zdroje dat a pak klikněte na tlačítko **OK**.
5. Z **Navigátor** podokně rozbalte **ODBC -> HIVE -> výchozí**, vyberte **hivesampletable**a potom klikněte na **zatížení**.

## <a name="visualize-data"></a>Vizualizaci dat

Pokračujte v posledním postupu.

1. V podokně vizualizace vyberte **mapy**.  Je ikona glóbu.

    ![HDInsight Power BI přizpůsobí sestavy](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. V podokně polí, vyberte **země** a **devicemake**. Uvidíte dat zobrazovaných na mapě.
3. Rozbalte mapy.

## <a name="next-steps"></a>Další postup
V tomto článku jste zjistili, jak k vizualizaci dat z prostředí HDInsight pomocí Power BI.  Další informace naleznete v následujících článcích:

* [Použití Zeppelin ke spouštění dotazů Hive v Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Připojení aplikace Excel do HDInsight pomocí ovladače ODBC Microsoft Hive](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Připojení aplikace Excel k systému Hadoop pomocí Power Query](apache-hadoop-connect-excel-power-query.md).
* [Připojení k Azure HDInsight a spouštět dotazy Hive pomocí nástrojů Data Lake pro Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Pomocí nástroje Azure HDInsight pro Visual Studio Code](../hdinsight-for-vscode.md).
* [Nahrání dat do HDInsight](./../hdinsight-upload-data.md).
