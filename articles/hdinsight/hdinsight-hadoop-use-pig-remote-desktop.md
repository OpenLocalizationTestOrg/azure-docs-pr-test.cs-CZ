---
title: "aaaUse Hadoop Pig pomocí vzdálené plochy v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Pig příkaz toorun Pig Latin příkazy z clusteru systému Windows Hadoop tooa připojení vzdálené plochy v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 2a4565fa827cd45fdbe6194b0486df93a6561084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Spuštění úlohy Pig z připojení vzdálené plochy
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tento dokument poskytuje návod pro použití hello Pig příkaz toorun Pig Latin příkazy z clusteru HDInsight se systémem Windows tooa připojení vzdálené plochy. Pig Latin vám umožní toocreate MapReduce aplikace prostřednictvím popisu transformace dat, nikoli mapování a snížit funkce.

> [!IMPORTANT]
> Vzdálená plocha je dostupná pouze na clustery HDInsight, které používají Windows jako hello operační systém. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 nebo větší, projděte si téma [použijte Pig s HDInsight a SSH](hdinsight-hadoop-use-pig-ssh.md) informace o interaktivním spouštění Pig úlohy přímo na hello clusteru z příkazového řádku.

## <a id="prereq"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat následující hello.

* Cluster HDInsight se systémem Windows (Hadoop v HDInsight)
* Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7

## <a id="connect"></a>Připojit pomocí vzdálené plochy
Povolení vzdálené plochy pro hello HDInsight cluster a pak připojit tooit podle následujících pokynů hello [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Použijte příkaz Pig hello
1. Po připojení ke vzdálené ploše, začít hello **Hadoop příkazového řádku** pomocí hello ikony na ploše hello.
2. Použijte následující příkaz Pig hello toostart hello:

        %pig_home%\bin\pig

    Zobrazí se `grunt>` řádku.
3. Zadejte hello následující příkaz:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Tento příkaz načte obsah hello hello sample.log souboru do souboru protokoly hello. Hello obsah souboru hello můžete zobrazit pomocí hello následující příkaz:

        DUMP LOGS;
4. Transformace dat hello použitím úroveň protokolování hello pouze tooextract regulární výraz z jednotlivých záznamů:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Můžete použít **DUMP** tooview hello dat po transformaci hello. V takovém případě `DUMP LEVELS;`.
5. Pokračujte v použití transformací pomocí hello následující příkazy. Použití `DUMP` tooview hello výsledek hello transformace po dokončení každého kroku.

    <table>
    <tr>
    <th>Příkaz</th><th>Výsledek</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = filtr úrovně podle LOGLEVEL není null.</td><td>Odebere řádky, které obsahují hodnotu null pro úroveň protokolu hello a ukládá výsledky hello do FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS skupiny podle LOGLEVEL;</td><td>Hello skupiny řádků podle úrovně protokolu a ukládá výsledky hello do GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREKVENCE = foreach GROUPEDLEVELS generovat skupiny jako LOGLEVEL, počet (FILTEREDLEVELS. LOGLEVEL) jako počet;</td><td>Vytvoří novou sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se. To je uložena do frekvence</td>
    </tr>
    <tr>
    <td>VÝSLEDEK = pořadí FREKVENCÍ podle počtu desc;</td><td>Řadí hello protokolu úrovně podle počtu (sestupně) a úložiště do výsledku</td>
    </tr>
    </table>
6.Můžete také uložit hello výsledky transformace pomocí hello `STORE` příkaz. Například následující příkaz hello uloží hello `RESULT` toohello **/example/data/pigout** adresář v hello výchozí kontejner úložiště pro cluster:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > Hello data jsou uložena v hello zadaný adresář v souborech s názvem **část nnnnn**. Pokud hello adresář již existuje, zobrazí se chybová zpráva.
   >
   >
7. tooexit hello grunt řádku, zadejte následující příkaz hello.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin dávkové soubory
Můžete taky hello Pig příkaz toorun Pig Latin, který je obsažen v souboru.

1. Po ukončení řádku hello grunt, otevřete **Poznámkový blok** a vytvořte nový soubor s názvem **pigbatch.pig** v hello **PIG_HOME %** adresáře.
2. Typ nebo následující hello vložení řádků do hello **pigbatch.pig** souboru a pak ho uložte:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Použití hello následující toorun hello **pigbatch.pig** soubor pomocí příkazu pig hello.

        pig %PIG_HOME%\pigbatch.pig

    Po dokončení úlohy batch hello byste měli vidět následující výstupu, který by měl být hello stejný jako při použití hello `DUMP RESULT;` v předchozích krocích hello:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Shrnutí
Jak vidíte, hello Pig příkaz vám umožní toointeractively spustit MapReduce operations, nebo spustit úlohy Pig Latin, které jsou uložené v dávkovém souboru.

## <a id="nextsteps"></a>Další kroky
Obecné informace o Pig v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
