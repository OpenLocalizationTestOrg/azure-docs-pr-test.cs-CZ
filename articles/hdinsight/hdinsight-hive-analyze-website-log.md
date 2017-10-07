---
title: "aaaUse Hive se systémem Hadoop pro analýzu protokolu Web - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Hive s HDInsight tooanalyze webu protokoly. Můžete použít soubor protokolu jako vstup do tabulky HDInsight a použít HiveQL tooquery hello data."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9cbce3cc8cf8bc3ad104dc4ca6a5628802c8fe89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-windows-based-hdinsight-tooanalyze-logs-from-websites"></a>Použijte Hive s HDInsight se systémem Windows tooanalyze protokoly z webů
Zjistěte, jak toouse HiveQL s HDInsight tooanalyze protokoly z webu. Analýza protokolu webu můžete být použité toosegment cílovou skupinu podle podobné aktivity, kategorizace návštěvníky demografické údaje a toofind hello obsah, který uživatel vidí, hello webů, které pocházejí z a tak dále.

> [!IMPORTANT]
> Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

V této ukázce použijete HDInsight clusteru tooanalyze webu protokolu soubory tooget lépe pochopit četnost hello webu toohello návštěv z externích webových stránek za den. Pokud vytvoříte souhrn chyb na webových stránkách, které uživatelé hello zaznamenat. Se dozvíte, jak:

* Připojte tooa úložiště objektů Azure Blob, který obsahuje soubory protokolu webové stránky.
* Vytvoření HIVE tabulky tooquery tyto protokoly.
* Tooanalyze hello data vytvořte dotazy HIVE.
* Pomocí aplikace Microsoft Excel tooconnect tooHDInsight (pomocí otevřenou databázi připojení (ODBC) tooretrieve hello analyzovat data.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a>Požadavky
* Musí být zřízená clusteru Hadoop v Azure HDInsight. Pokyny najdete v tématu [zřizování clusterů HDInsight][hdinsight-provision].
* Musíte mít aplikaci Microsoft Excel 2013 nebo nainstalovat aplikaci Excel 2010.
* Musíte mít [ovladače ODBC Microsoft Hive](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data z podregistru do aplikace Excel.

## <a name="toorun-hello-sample"></a>Ukázka toorun hello
1. Z hello [portálu Azure](https://portal.azure.com/), z hello úvodní panel (Pokud je připnutý hello clusteru existuje), klikněte na dlaždici clusteru hello, na kterém chcete toorun hello ukázka.
2. Z hello clusteru okno, v části **rychlé odkazy**, klikněte na tlačítko **řídicí panel clusteru**a potom z hello **řídicí panel clusteru** okně klikněte na tlačítko **clusteru HDInsight Řídicí panel**. Alternativně můžete přímo otevřít řídicí panel hello pomocí hello následující adresu URL:

         https://<clustername>.azurehdinsight.net

    Po zobrazení výzvy ověřování pomocí hello správce uživatelského jména a hesla, který jste použili při zřizování clusteru hello.
3. Z hello webové stránky, které se otevře, klikněte na tlačítko hello **postupem Začínáme Galerie** kartě a potom v části hello **řešení s ukázkovými daty** kategorii, klikněte na tlačítko hello **analýza protokolu webu** Ukázka.
4. Postupujte podle pokynů hello vzorkem hello toofinish webovou stránku hello.

## <a name="next-steps"></a>Další kroky
Zkuste hello následující ukázka: [analýza dat snímače používání Hive s HDInsight](hdinsight-hive-analyze-sensor-data.md).

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
