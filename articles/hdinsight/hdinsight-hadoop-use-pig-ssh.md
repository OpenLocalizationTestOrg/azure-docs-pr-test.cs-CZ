---
title: "aaaUse Hadoop Pig pomocí protokolu SSH v clusteru HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak připojit tooa systémem Linux Hadoop clusteru pomocí protokolu SSH a pak použijte hello Pig příkaz toorun Pig Latin příkazy interaktivně, nebo prostřednictvím dávky úlohy."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 1da303e239b537e6b331b1d33010058582718c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-hello-pig-command-ssh"></a>Spuštění úlohy Pig na cluster se systémem Linux s hello příkaz Pig (SSH)

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Zjistěte, jak toointeractively spouštět úlohy Pig z clusteru služby HDInsight tooyour připojení SSH. Hello programovacího jazyka Pig Latin můžete toodescribe transformace, které jsou použité toohello vstupní data tooproduce hello potřeby výstup.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Připojení pomocí protokolu SSH

Použití clusteru HDInsight tooyour tooconnect SSH. Hello následující příklad se připojí tooa clusteru s názvem **myhdinsight** jako hello účet s názvem **sshuser**:

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali klíč certifikátu pro ověřování SSH** při vytvoření clusteru HDInsight hello budete potřebovat toospecify hello umístění hello privátní klíč klientského systému.

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Pokud jste zadali heslo pro ověřování SSH** při vytváření clusteru HDInsight hello, zadejte heslo hello po zobrazení výzvy.

Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Použijte příkaz Pig hello

1. Po připojení, spusťte hello Pig rozhraní příkazového řádku (CLI) pomocí hello následující příkaz:

        pig

    Po chvíli, měli byste vidět `grunt>` řádku.

2. Zadejte hello následující příkaz:

        LOGS = LOAD '/example/data/sample.log';

    Tento příkaz načte hello obsah souboru sample.log hello do PROTOKOLŮ. Hello obsah souboru hello můžete zobrazit pomocí hello následující příkaz:

        DUMP LOGS;

3. V dalším kroku transformovat hello data použitím úroveň protokolování hello pouze tooextract regulární výraz z jednotlivých záznamů pomocí hello následující příkaz:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Můžete použít **DUMP** tooview hello dat po transformaci hello. V takovém případě použijte `DUMP LEVELS;`.

4. Pokračujte v použití transformací pomocí hello příkazy v hello následující tabulka:

    | Pig Latin – příkaz | Jaké příkaz hello nemá |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Odebere řádky, které obsahují hodnotu null pro úroveň protokolu hello a ukládá výsledky hello do `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Hello skupiny řádků podle úrovně protokolu a ukládá výsledky hello do `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Vytvoří sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se. Hello datové sady je uložena do `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Řadí hello protokolu úrovně podle počtu (sestupně) a ukládá do `RESULT`. |

    > [!TIP]
    > Použití `DUMP` tooview hello výsledek hello transformace po dokončení každého kroku.

5. Můžete také uložit hello výsledky transformace pomocí hello `STORE` příkaz. Například následující příkaz hello uloží hello `RESULT` toohello `/example/data/pigout` v hello výchozí úložiště pro cluster:

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > Hello data jsou uložena v hello zadaný adresář v souborech s názvem `part-nnnnn`. Pokud hello adresář již existuje, obdržíte chybu.

6. tooexit hello grunt řádku, zadejte následující příkaz hello:

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin dávkové soubory

Můžete také použít hello Pig příkaz toorun Pig Latin obsaženého v načítaném souboru.

1. Po ukončení řádku hello grunt, použijte následující hello příkaz toopipe STDIN do souboru s názvem `pigbatch.pig`. Tento soubor je vytvořen v hello domovský adresář pro hello SSH uživatelský účet.

        cat > ~/pigbatch.pig

2. Zadejte nebo vložte hello následující řádky a potom pomocí kombinace kláves Ctrl + D po dokončení.

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. Použití hello následující příkaz toorun hello `pigbatch.pig` souboru pomocí příkazu Pig hello.

        pig ~/pigbatch.pig

    Po dokončení úlohy batch hello zobrazí hello následující výstup:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Další kroky

Obecné informace o Pig v HDInsight najdete v části hello následujícím dokumentu:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Další informace o dalších způsobů toowork s Hadoop v HDInsight naleznete v tématu hello následující dokumenty:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
