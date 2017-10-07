---
title: "aaaDevelop úlohy Python streamování MapReduce s HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Python ve streamování úloh MapReduce. Hadoop poskytuje streamování rozhraní API pro MapReduce pro zápis do jiných jazyků než Java."
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: a6ae3ba650b665ecc5839a4ddf5282f8ccfb6bd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>Vývoj Python streamování MapReduce programy pro HDInsight

Zjistěte, jak toouse Python ve streamování MapReduce operations. Hadoop poskytuje streamování rozhraní API pro MapReduce, která umožní toowrite mapy a omezit funkce v jiných jazyků než Java. Hello kroky v tomto dokumentu implementovat hello mapy a snížit součásti v Pythonu.

## <a name="prerequisites"></a>Požadavky

* Systémem Linux Hadoop v HDInsight clusteru

  > [!IMPORTANT]
  > Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* V textovém editoru

  > [!IMPORTANT]
  > Hello textového editoru musí používat LF jako hello ukončování řádků. Pomocí konec řádku Line FEED způsobuje chyby při spuštění úlohy MapReduce hello v clusterech HDInsight se systémem Linux.

* Hello `ssh` a `scp` příkazy, nebo [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Počet slov

V tomto příkladu je, že počet základní slov implementované v python reduktorem a mapper. Mapovač Hello věty dělí do jednotlivých slov a hello reduktorem slučuje hello slova a počty tooproduce hello výstup.

Hello následující vývojový diagram znázorňuje, co se stane při hello mapy a snížit fáze.

![Obrázek procesu mapreduce hello](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Streamování MapReduce

Hadoop vám umožní toospecify soubor, který obsahuje hello mapy a snížit logiky, která se používá v rámci úlohy. zvláštní požadavky Hello hello mapování a snížit logiky jsou:

* **Vstupní**: hello mapy a snížit součásti musí číst vstupní data z STDIN.
* **Výstup**: hello mapy a snížit součásti musíte napsat tooSTDOUT výstupní data.
* **Formát dat**: data hello spotřebované a vytváří musí být dvojice klíč/hodnota, oddělených tabulátorem.

Python lze snadno zpracovat tyto požadavky pomocí hello `sys` modulu tooread z stdin – a pomocí `print` tooprint tooSTDOUT. Hello zbývajících úloh je jednoduše formátování hello dat s na kartě (`\t`) znak mezi hello klíč a hodnotu.

## <a name="create-hello-mapper-and-reducer"></a>Vytvoření hello mapper a reduktorem

1. Vytvořte soubor s názvem `mapper.py` a hello použijte následující kód jako obsah hello:

   ```python
   #!/usr/bin/env python

   # Use hello sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read hello data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write tooSTDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. Vytvořte soubor s názvem **reducer.py** a hello použijte následující kód jako obsah hello:

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out hello separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read hello data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using hello word as hello key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull hello count(s) for hello word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write toostdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a>Spuštění pomocí prostředí PowerShell

tooensure splnit vaše soubory hello pravého konce řádků, hello použijte následující skript prostředí PowerShell:

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Pomocí hello následující soubory hello tooupload skriptu prostředí PowerShell, spusťte úlohu hello a zobrazit výstup hello:

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Spuštění z relace SSH

1. Z vývojového prostředí v hello stejný adresář jako `mapper.py` a `reducer.py` soubory, použijte následující příkaz hello:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Nahraďte `username` hello uživatelským jménem SSH pro váš cluster a `clustername` s hello názvem vašeho clusteru.

    Tento příkaz zkopíruje soubory hello z hlavního uzlu toohello hello místní systém.

    > [!NOTE]
    > Pokud jste použili toosecure heslo účtu SSH, budete vyzváni k hello heslo. Pokud jste použili klíč SSH, můžete mít toouse hello `-i` parametr a hello cesta toohello privátní klíč. Například, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. Připojte toohello clusteru pomocí protokolu SSH:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Další informace o najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. tooensure hello mapper.py a reducer.py mají hello opravte konce řádků, použijte hello následující příkazy:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. Použijte následující příkaz toostart hello MapReduce úlohy hello.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Tento příkaz má hello následující části:

   * **hadoop streaming.jar**: používá při provádění operací streamování MapReduce. Je rozhraní Hadoop s hello externí MapReduce kód, který zadáte.

   * **-soubory**: Přidá hello zadaný úlohu MapReduce toohello soubory.

   * **-mapper**: informuje Hadoop, který toouse souborů, jako jsou třeba hello mapper.

   * **-reduktorem**: informuje Hadoop, který toouse souborů, jako jsou třeba hello reduktorem.

   * **-vstupní**: slova hello vstupního souboru, který jsme měli počítat z.

   * **-výstupu**: hello adresář, který hello výstup je zapsán do.

    Jak funguje hello úlohu MapReduce, hello procesu se zobrazí jako procenta.

        15/02/05 19:01:04 informace o mapreduce. Úloha: % 0 mapy snížit 0 % 15/02/05 19:01:16 informace o mapreduce. Úloha: 100 % mapy snížit 0 % 15/02/05 19:01:27 informace o mapreduce. Úloha: 100 % mapy snížit 100 %


5. tooview hello výstup hello použijte následující příkaz:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Tento příkaz zobrazí seznam slova a jak často hello word došlo k chybě.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toouse streamování MapRedcue úlohy s HDInsight, použijte následující odkazy tooexplore hello jiné způsoby toowork s Azure HDInsight.

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití úloh MapReduce se službou HDInsight](hdinsight-use-mapreduce.md)
