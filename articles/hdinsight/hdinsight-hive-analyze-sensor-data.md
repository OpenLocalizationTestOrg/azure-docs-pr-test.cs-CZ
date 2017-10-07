---
title: "data snímače aaaAnalyze použití Hive a Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak hello tooanalyze data snímačů pomocí konzoly dotaz Hive s HDInsight (Hadoop) a potom vizualizovat data hello v aplikaci Microsoft Excel s PowerView."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 70e595705c33d9835dc9809161f79c3ac5ece870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-using-hello-hive-query-console-on-hadoop-in-hdinsight"></a>Analýza dat snímačů pomocí hello Hive dotaz konzoly systému Hadoop v HDInsight

Zjistěte, jak hello tooanalyze data snímačů pomocí konzoly dotaz Hive s HDInsight (Hadoop) a potom vizualizovat data hello v aplikaci Microsoft Excel pomocí Power View.

> [!IMPORTANT]
> Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


V této ukázce použijete Hive tooprocess historických dat a identifikovat problémy s vytápění a klimatizace systémy. Konkrétně identifikovat systémy nebylo možné tooreliably udržovat nastavenou teplotu provedením hello následující úlohy:

* Vytvoření tabulky HIVE tooquery data uložená v souborech s oddělovači (CSV).
* Tooanalyze hello data vytvořte dotazy HIVE.
* tooretrieve hello analyzovat data, používat tooHDInsight tooconnect Microsoft Excel.
* toovisualize hello data, použijte Power View.

![Diagram architektury řešení hello](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Požadavky

* Cluster služby HDInsight (Hadoop): najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md) informace o vytvoření clusteru.
* Microsoft Excel 2013

  > [!NOTE]
  > Aplikace Microsoft Excel se používá pro vizualizaci dat s [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Ovladač ODBC Microsoft Hive](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="toorun-hello-sample"></a>Ukázka toorun hello

1. Z webového prohlížeče přejděte toohello následující adresu URL: 

         https://<clustername>.azurehdinsight.net

    Nahraďte `<clustername>` s názvem hello clusteru HDInsight.

    Po zobrazení výzvy ověřování pomocí hello správce uživatelského jména a hesla, který jste použili při zřizování tento cluster.

2. Z hello webové stránky, které se otevře, klikněte na tlačítko hello **postupem Začínáme Galerie** kartě a potom v části hello **řešení s ukázkovými daty** kategorii, klikněte na tlačítko hello **analýza dat snímačů** Ukázka.

    ![Získávání Začínáme Galerie image](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Postupujte podle pokynů hello vzorkem hello toofinish webovou stránku hello.
