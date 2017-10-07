---
title: "aaaUse Hadoop Pig v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak clusteru toosubmit Pig úlohy tooa Hadoop v HDInsight pomocí prostředí Azure PowerShell."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 771617df203011eaec715a0dba6f5014a42877f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toorun-pig-jobs-with-hdinsight"></a>Použijte úlohy Azure PowerShell toorun Pig s HDInsight

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tento dokument obsahuje příklad použití Azure PowerShell toosubmit Pig úlohy tooa Hadoop na clusteru HDInsight. Pig vám umožní úloh MapReduce toowrite pomocí jazyka (Pig Latin), který modelů transformace dat, nikoli mapování a snížit funkce.

> [!NOTE]
> Tento dokument neposkytuje podrobný popis co dělat, příkazy Pig Latin hello použitých v ukázkách hello. Informace o hello Pig Latin použité v tomto příkladu najdete v tématu [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).

## <a id="prereq"></a>Požadavky

* **Cluster Azure HDInsight**

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Pracovní stanice s prostředím Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>Spuštění úlohy Pig pomocí prostředí PowerShell

Prostředí Azure PowerShell poskytuje *rutiny* , umožňují úlohy Pig tooremotely spustit v HDInsight. Interně, prostředí PowerShell používá volání REST příliš[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) běžící v clusteru HDInsight hello.

Hello se používají následující rutiny při spuštění úlohy Pig na vzdálený cluster HDInsight:

* **Login-AzureRmAccount**: ověřuje Azure PowerShell tooyour předplatné Azure
* **Nové AzureRmHDInsightPigJobDefinition**: vytvoří *úlohy definice* pomocí hello zadaný Pig Latin příkazy
* **Spuštění AzureRmHDInsightJob**: odešle tooHDInsight definice úlohy hello, spustí hello úlohy a vrátí *úlohy* objekt, který lze použít toocheck hello stav úlohy hello
* **Počkejte AzureRmHDInsightJob**: používá objekt úlohy hello toocheck hello stav úlohy hello. Se čeká na dokončení úlohy hello nebo byla překročena doba čekání hello.
* **Get-AzureRmHDInsightJobOutput**: používá tooretrieve hello výstup hello úlohy

Hello následující kroky ukazují, jak toouse tyto rutiny toorun úlohu v clusteru HDInsight.

1. Pomocí editoru, uložte hello následující kód jako **pigjob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Otevřete nový příkazový řádek prostředí Windows PowerShell. Umístění adresáře toohello hello změnit **pigjob.ps1** souboru, potom použijte následující příkaz toorun hello skriptu hello:

        .\pigjob.ps1

    Jste výzvami toolog v tooyour předplatného Azure. Potom budete vyzváni k hello HTTPs nebo správce účtu jména a hesla pro hello HDInsight cluster.

2. Po dokončení úlohy hello, měla by vrátit informace podobná toohello následující text:

        Start hello Pig job ...
        Wait for hello Pig job toocomplete ...
        Display hello standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Řešení potíží

Když se po dokončení úlohy hello nevrátí žádné informace, může mít došlo k chybě během zpracování. informace o chybě tooview pro tuto úlohu, přidejte následující příkaz toohello konec hello hello **pigjob.ps1** souboru, uložit jej a znovu jej spusťte.

    # Print hello output of hello Pig job.
    Write-Host "Display hello standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Vrátí hello informace, která byla zapsána tooSTDERR na serveru hello při spuštění úlohy hello a zkuste zjistit, proč se nedaří hello úlohy.

## <a id="summary"></a>Shrnutí
Jak můžete vidět, prostředí Azure PowerShell poskytuje snadný způsob toorun úlohy Pig v clusteru HDInsight, stav úlohy hello monitorování a načíst výstup hello.

## <a id="nextsteps"></a>Další kroky
Obecné informace o Pig v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
