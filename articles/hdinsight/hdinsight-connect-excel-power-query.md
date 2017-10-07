---
title: "aaaConnect tooHadoop aplikace Excel pomocí doplňku Power Query - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tootake výhod komponenty business intelligence a používání doplňku Power Query pro Excel tooaccess data uložená v Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 01ad2f90-7520-44d9-8c16-4d936faaff9b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jgao
ms.openlocfilehash: 1213849f1bc04e0ed206750b80766ff2268664b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-by-using-power-query"></a>Připojení aplikace Excel tooHadoop pomocí doplňku Power Query
Klíčovou vlastností řešení velkých objemů dat společnosti Microsoft hello je hello integrace komponenty Microsoft business intelligence (BI) s clustery systému Hadoop v prostředí Azure HDInsight. Primární příkladem je hello možnost tooconnect Excel toohello účet úložiště Azure, který obsahuje data hello přidruženého k vašemu clusteru Hadoop pomocí hello Microsoft Power Query pro doplněk aplikace Excel. Tento článek vás provede tooset nahoru a používání doplňku Power Query tooquery data přidružená k clusteru Hadoop spravováni s HDInsight.

### <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít hello následující položky:

* **Cluster služby HDInsight**. tooconfigure, najdete v části [Začínáme s Azure HDInsight][hdinsight-get-started].
* **Pracovní stanice** se systémem Windows 7, Windows Server 2008 R2 nebo novějším operačním systémem.
* **Office 2016, Office Professional 2013 Plus, Office 365 ProPlus, samostatné aplikace Excel 2013 nebo Office 2010 Professional Plus**.

## <a name="install-power-query"></a>Instalace doplňku Power Query
Power Query můžete importovat data, která má výstup, nebo které byla vygenerována v rámci Hadoop úlohy spuštěné v clusteru služby HDInsight.

V aplikaci Excel 2016 Power Query integrovány do hello Data pásu karet v části hello Get & části transformace. Pro starší verze aplikace Excel, stáhnout Microsoft Power Query pro Excel z hello [Microsoft Download Center] [ powerquery-download] a nainstalujte ji.

## <a name="import-hdinsight-data-into-excel"></a>Importovat HDInsight data do aplikace Excel
Hello doplňku Power Query pro Excel umožňuje snadno tooimport data z clusteru HDInsight do Excelu, kde můžete nástrojů BI, například doplňku PowerPivot a mapa Power být použité tooinspect, analyzovat a prezentují hello data.

**tooimport data z clusteru služby HDInsight**

1. Otevřete aplikaci Excel.
2. Vytvoření nového prázdného sešitu.
3. Proveďte následující kroky, které jsou založeny na verzi aplikace Excel hello hello:

    - Excel 2016

        - Klikněte na tlačítko hello **Data** nabídky, klikněte na tlačítko **načíst Data** z hello **Get & transformace dat** pásu karet, klikněte na tlačítko **z Azure**a pak klikněte na tlačítko  **Z Azure HDInsight(HDFS)**.

        ![HDI. PowerQuery.SelectHdiSource](./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Excel 2013/2010

        - Klikněte na tlačítko hello **Power Query** nabídky, klikněte na tlačítko **z Azure**a potom klikněte na **z Microsoft Azure HDInsight**.
   
        ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Poznámka:** Pokud nevidíte hello **Power Query** nabídky, přejděte příliš**soubor** > **možnosti** > **doplňky**a vyberte **COM Doplňky** z rozevíracího seznamu hello **spravovat** pole v hello dolní části stránky hello. Vyberte hello **přejděte...**  tlačítko a ověřte dané pole hello, byl vrácen hello Power Query pro Excel add-in.
       
        **Poznámka:** Power Query lze také tooimport data z HDFS kliknutím **z jiných zdrojů**.
4. Pro **název účtu**zadejte hello název účtu úložiště objektů Blob v Azure hello přidruženého k vašemu clusteru a pak klikněte na tlačítko **OK**. Tento účet může být hello [výchozí účet úložiště](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) nebo propojený účet úložiště.  Formát Hello je *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. Pro **klíč účtu**zadejte hello klíč pro hello účtu úložiště Blob a pak klikněte na tlačítko **Uložit**. (Potřebujete tooenter hello účet informace pouze hello prvním přístupu k tomuto úložišti.)
6. V hello **Navigátor** levé podokno na hello hello editoru dotazů, dvakrát klikněte na název kontejneru úložiště objektů Blob hello. Ve výchozím nastavení, je název kontejneru hello hello stejný název jako název clusteru hello.
7. Vyhledejte **HiveSampleData.txt** v hello **název** sloupce (je cesta ke složce hello **... / hive/skladu/hivesampletable/**) a potom klikněte na **binární** na hello nalevo od HiveSampleData.txt. HiveSampleData.txt obsahuje všechny hello clusteru. Volitelně můžete vytvořit svůj vlastní soubor.
   
    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Pokud chcete, můžete přejmenovat hello názvy sloupců. Až budete připravení, klikněte na tlačítko **zavřete & načíst**.  načíst tooyour sešitu byla Hello data:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toouse Power Query tooretrieve data z HDInsight do aplikace Excel. Podobně můžete data načíst z prostředí HDInsight do Azure SQL Database. Je také možné tooupload data do HDInsight. toolearn více, najdete v části hello následující články:

* [Připojení aplikace Excel tooHDInsight s hello ovladače ODBC Microsoft Hive][hdinsight-ODBC]
* [Nahrání dat tooHDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
