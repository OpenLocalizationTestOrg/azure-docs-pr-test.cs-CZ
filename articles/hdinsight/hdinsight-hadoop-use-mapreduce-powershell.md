---
title: "aaaUse MapReduce a prostředí PowerShell s Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí PowerShell tooremotely spouštění úloh MapReduce s Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 59524f0e8813d4c017f92bccb2e50d4c018acf71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Spuštění úloh MapReduce s Hadoop v HDInsight pomocí prostředí PowerShell

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tento dokument obsahuje příklad použití Azure PowerShell toorun úlohu MapReduce v Hadoop na clusteru HDInsight.

## <a id="prereq"></a>Požadavky

* **Cluster Azure HDInsight (Hadoop v HDInsight)**

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Pracovní stanice s prostředím Azure PowerShell**.

## <a id="powershell"></a>Spustit úlohu MapReduce pomocí Azure PowerShell

Prostředí Azure PowerShell poskytuje *rutiny* , umožňují úloh MapReduce tooremotely spustit v HDInsight. Interně toho dosahuje pomocí volání REST příliš[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (dříve se označovaly jako Templeton) systémem hello clusteru HDInsight.

Hello se používají následující rutiny při spuštění úloh MapReduce v vzdáleného clusteru HDInsight.

* **Login-AzureRmAccount**: ověřuje Azure PowerShell tooyour předplatného Azure.

* **Nové AzureRmHDInsightMapReduceJobDefinition**: vytvoří novou *úlohy definice* pomocí hello zadané informace o MapReduce.

* **Spuštění AzureRmHDInsightJob**: odešle tooHDInsight definice úlohy hello, spustí hello úlohy a vrátí *úlohy* objekt, který lze použít toocheck hello stav úlohy hello.

* **Počkejte AzureRmHDInsightJob**: používá objekt úlohy hello toocheck hello stav úlohy hello. Se čeká na dokončení úlohy hello nebo je překročena doba čekání hello.

* **Get-AzureRmHDInsightJobOutput**: používá tooretrieve hello výstup hello úlohy.

Hello následující kroky ukazují, jak toouse tyto rutiny toorun úlohu v clusteru HDInsight.

1. Pomocí editoru, uložte hello následující kód jako **mapreducejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Otevřete nový **prostředí Azure PowerShell** příkazového řádku. Umístění adresáře toohello hello změnit **mapreducejob.ps1** souboru, potom použijte následující příkaz toorun hello skriptu hello:

        .\mapreducejob.ps1

    Při spuštění skriptu hello, zobrazí se výzva k hello název clusteru HDInsight hello a hello HTTPS nebo správce účtu jména a hesla pro hello cluster. Může být také výzvami tooauthenticate tooyour předplatného Azure.

3. Po dokončení úlohy hello, zobrazí se výstup podobný toohello následující text:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Tento výstup označuje, že hello úloha úspěšně dokončena.

    > [!NOTE]
    > Pokud hello **ExitCode** je hodnota než 0, najdete v části [Poradce při potížích s](#troubleshooting).

    Tento příklad také ukládá hello stažené soubory tooan **výstup.txt** soubor v adresáři hello, který spustí skript hello z.

### <a name="view-output"></a>Zobrazení výstupu

Otevřete hello **výstup.txt** soubor ve textového editoru toosee hello slova a počty vytvořené úlohou hello.

> [!NOTE]
> výstupní soubory Hello úlohy MapReduce jsou neměnné. Pokud tuto ukázku, takže musíte toochange hello názvu výstupního souboru hello.

## <a id="troubleshooting"></a>Řešení potíží

Když se po dokončení úlohy hello nevrátí žádné informace, může mít došlo k chybě během zpracování. informace o chybě tooview pro tuto úlohu, přidejte následující příkaz toohello konec hello hello **mapreducejob.ps1** souboru, uložit jej a znovu jej spusťte.

```powershell
# Print hello output of hello WordCount job.
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Tato rutina vrátí informace hello, která byla zapsána tooSTDERR na serveru hello při spuštění úlohy hello a může pomoci zjistit, proč se nedaří hello úlohy.

## <a id="summary"></a>Shrnutí

Jak vidíte, bude Azure PowerShell na clusteru HDInsight, stav úlohy hello monitorování a výstup hello načtení poskytuje snadný způsob toorun úloh MapReduce.

## <a id="nextsteps"></a>Další kroky

Obecné informace o úloh MapReduce v HDInsight:

* [Používání nástroje MapReduce systému HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
