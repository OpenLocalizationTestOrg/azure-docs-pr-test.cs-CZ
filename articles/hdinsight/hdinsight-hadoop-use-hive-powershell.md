---
title: "aaaUse Hadoop Hive v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Pomocí dotazů Hive toorun prostředí PowerShell v Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 9e0b72a25c5b12431f837b1a34a63ecc06223528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-powershell"></a>Spouštění dotazů Hive pomocí prostředí PowerShell
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tento dokument poskytuje příklad použití Azure PowerShell v dotazy Hive hello skupiny prostředků Azure režimu toorun v Hadoop v clusteru HDInsight.

> [!NOTE]
> Tento dokument neposkytuje podrobný popis co dělat hello HiveQL příkazy, které se používají v příkladech hello. Informace o hello HiveQL, který se používá v tomto příkladu najdete v tématu [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md).

**Požadavky**

* **Cluster Azure HDInsight**: nezáleží na tom, jestli je Windows hello clusteru nebo systémem Linux.

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Pracovní stanice s prostředím Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Spouštění dotazů Hive pomocí Azure PowerShell

Prostředí Azure PowerShell poskytuje *rutiny* , umožňují tooremotely spouštění dotazů Hive v HDInsight. Interně hello rutiny volání REST příliš[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) na clusteru HDInsight hello.

Hello se používají následující rutiny při spuštění dotazů Hive v clusteru s podporou vzdálené HDInsight:

* **Přidat-AzureRmAccount**: ověřuje Azure PowerShell tooyour předplatného Azure
* **Nové AzureRmHDInsightHiveJobDefinition**: vytvoří *úlohy definice* pomocí hello zadaný příkazy HiveQL
* **Spuštění AzureRmHDInsightJob**: odešle tooHDInsight definice úlohy hello, spustí hello úlohy a vrátí *úlohy* objekt, který lze použít toocheck hello stav úlohy hello
* **Počkejte AzureRmHDInsightJob**: používá objekt úlohy hello toocheck hello stav úlohy hello. Se čeká na dokončení úlohy hello nebo je překročena doba čekání hello.
* **Get-AzureRmHDInsightJobOutput**: používá tooretrieve hello výstup hello úlohy
* **Vyvolání AzureRmHDInsightHiveJob**: používat příkazy HiveQL toorun. Tento dotaz hello bloky rutiny dokončení a potom vrátí výsledky hello
* **Použití AzureRmHDInsightCluster**: Nastaví hello aktuální toouse clusteru pro hello **Invoke-AzureRmHDInsightHiveJob** příkaz

Hello následující kroky ukazují, jak toouse tyto rutiny toorun úlohu v clusteru HDInsight:

1. Pomocí editoru, uložte hello následující kód jako **hivejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Otevřete nový **prostředí Azure PowerShell** příkazového řádku. Umístění adresáře toohello hello změnit **hivejob.ps1** souboru, potom použijte následující příkaz toorun hello skriptu hello:

        .\hivejob.ps1

    Při spuštění skriptu hello jste výzvami tooenter hello clusteru název a hello HTTPS nebo přihlašovací údaje účtu správce pro hello cluster. Může být také výzvami toolog v tooyour předplatného Azure.

3. Po dokončení úlohy hello vrátí následující thext podobné toohello informace:

        Display hello standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Jak už bylo zmíněno dříve, **Invoke-Hive** lze použít toorun dotazu a čeká na odpověď hello. Použijte následující skript toosee fungování Invoke-Hive hello:

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    výstup Hello vypadá hello následující text:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Pro delší HiveQL dotazy, můžete použít hello prostředí Azure PowerShell **sem řetězce** rutina nebo HiveQL soubory skriptů. Následující fragment kódu ukazuje, jak Hello toouse hello **Invoke-Hive** rutiny toorun soubor skriptu HiveQL. Hello HiveQL souboru skriptu musí být nahrán toowasb: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Další informace o **sem řetězce**, najdete v části <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">pomocí Windows PowerShell zde-řetězce</a>.

## <a name="troubleshooting"></a>Řešení potíží

Když se po dokončení úlohy hello nevrátí žádné informace, může mít došlo k chybě během zpracování. informace o chybě tooview pro tuto úlohu, přidejte následující toohello konec hello hello **hivejob.ps1** souboru, uložit jej a znovu jej spusťte.

```powershell
# Print hello output of hello Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Tato rutina vrátí hello informace, které se zapisují tooSTDERR na serveru hello při spuštění úlohy hello.

## <a name="summary"></a>Souhrn

Jak vidíte, prostředí Azure PowerShell poskytuje snadný způsob toorun dotazů Hive v clusteru služby HDInsight, monitorování hello stav úlohy a načíst výstup hello.

## <a name="next-steps"></a>Další kroky

Obecné informace o Hive v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
