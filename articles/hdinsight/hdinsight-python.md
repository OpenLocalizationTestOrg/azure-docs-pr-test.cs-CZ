---
title: aaaPython UDF s Apache Hive a Pig - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse Python uživatele definované funkce (UDF) z Hive a Pig v HDInsight, hello Hadoop technologie zásobníku v Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c44d6606-28cd-429b-b535-235e8f34a664
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/17/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 26d8160cc6ed7fc22c3f06f7c1c9954c224b2366
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a><span data-ttu-id="d8a16-103">Funkce (UDF) s Hive a Pig definované uživatelem Python použití v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8a16-103">Use Python User Defined Functions (UDF) with Hive and Pig in HDInsight</span></span>

<span data-ttu-id="d8a16-104">Zjistěte, jak toouse Python uživatelem definované funkce (UDF) s Apache Hive a Pig v Hadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8a16-104">Learn how toouse Python user-defined functions (UDF) with Apache Hive and Pig in Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="d8a16-105"><a name="python"></a>Python v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8a16-105"><a name="python"></a>Python on HDInsight</span></span>

<span data-ttu-id="d8a16-106">Python2.7 je nainstalována ve výchozím nastavení na HDInsight 3.0 a novější.</span><span class="sxs-lookup"><span data-stu-id="d8a16-106">Python2.7 is installed by default on HDInsight 3.0 and later.</span></span> <span data-ttu-id="d8a16-107">Apache Hive lze použít s touto verzí jazyka Python pro zpracování datového proudu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-107">Apache Hive can be used with this version of Python for stream processing.</span></span> <span data-ttu-id="d8a16-108">Zpracování datového proudu STDOUT a stdin – data toopass mezi Hive a hello UDF používá.</span><span class="sxs-lookup"><span data-stu-id="d8a16-108">Stream processing uses STDOUT and STDIN toopass data between Hive and hello UDF.</span></span>

<span data-ttu-id="d8a16-109">HDInsight zahrnuje taky Jython, což je implementace Python napsanou v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="d8a16-109">HDInsight also includes Jython, which is a Python implementation written in Java.</span></span> <span data-ttu-id="d8a16-110">Jython běží přímo na hello virtuální počítač Java a nepoužívá streamování.</span><span class="sxs-lookup"><span data-stu-id="d8a16-110">Jython runs directly on hello Java Virtual Machine and does not use streaming.</span></span> <span data-ttu-id="d8a16-111">Jython je hello doporučuje překladač Pythonu při použití Python s Pig.</span><span class="sxs-lookup"><span data-stu-id="d8a16-111">Jython is hello recommended Python interpreter when using Python with Pig.</span></span>

> [!WARNING]
> <span data-ttu-id="d8a16-112">Hello kroky v tomto dokumentu provést hello následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="d8a16-112">hello steps in this document make hello following assumptions:</span></span> 
>
> * <span data-ttu-id="d8a16-113">Můžete vytvořit hello skriptů Python ve svém místním vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8a16-113">You create hello Python scripts on your local development environment.</span></span>
> * <span data-ttu-id="d8a16-114">Nahrát hello skripty tooHDInsight pomocí buď hello `scp` příkaz z relace místní Bash nebo hello zadaný skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8a16-114">You upload hello scripts tooHDInsight using either hello `scp` command from a local Bash session or hello provided PowerShell script.</span></span>
>
> <span data-ttu-id="d8a16-115">Pokud chcete, aby toouse hello [prostředí cloudu Azure (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) náhled toowork s HDInsight, pak musíte:</span><span class="sxs-lookup"><span data-stu-id="d8a16-115">If you want toouse hello [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview toowork with HDInsight, then you must:</span></span>
>
> * <span data-ttu-id="d8a16-116">Vytváření hello skriptů v prostředí shell hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-116">Create hello scripts inside hello cloud shell environment.</span></span>
> * <span data-ttu-id="d8a16-117">Použití `scp` tooupload hello soubory z hello cloudu tooHDInsight prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8a16-117">Use `scp` tooupload hello files from hello cloud shell tooHDInsight.</span></span>
> * <span data-ttu-id="d8a16-118">Použití `ssh` z hello cloudové prostředí tooconnect tooHDInsight a příklady spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-118">Use `ssh` from hello cloud shell tooconnect tooHDInsight and run hello examples.</span></span>

## <span data-ttu-id="d8a16-119"><a name="hivepython"></a>Hive UDF</span><span class="sxs-lookup"><span data-stu-id="d8a16-119"><a name="hivepython"></a>Hive UDF</span></span>

<span data-ttu-id="d8a16-120">Python lze použít jako UDF z Hive prostřednictvím hello HiveQL `TRANSFORM` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8a16-120">Python can be used as a UDF from Hive through hello HiveQL `TRANSFORM` statement.</span></span> <span data-ttu-id="d8a16-121">Například následující HiveQL hello vyvolá hello `hiveudf.py` soubor uložený v účtu Azure Storage výchozí hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8a16-121">For example, hello following HiveQL invokes hello `hiveudf.py` file stored in hello default Azure Storage account for hello cluster.</span></span>

<span data-ttu-id="d8a16-122">**HDInsight se systémem Linux**</span><span class="sxs-lookup"><span data-stu-id="d8a16-122">**Linux-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

<span data-ttu-id="d8a16-123">**HDInsight se systémem Windows**</span><span class="sxs-lookup"><span data-stu-id="d8a16-123">**Windows-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> <span data-ttu-id="d8a16-124">Na clusterech HDInsight se systémem Windows hello `USING` klauzule musíte zadat úplnou cestu toopython.exe hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-124">On Windows-based HDInsight clusters, hello `USING` clause must specify hello full path toopython.exe.</span></span>

<span data-ttu-id="d8a16-125">Zde je, jaké jsou v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="d8a16-125">Here's what this example does:</span></span>

1. <span data-ttu-id="d8a16-126">Hello `add file` příkaz od začátku hello hello souboru přidá hello `hiveudf.py` souboru toohello distribuované mezipaměti, tak, aby byl přístupný pro všechny uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-126">hello `add file` statement at hello beginning of hello file adds hello `hiveudf.py` file toohello distributed cache, so it's accessible by all nodes in hello cluster.</span></span>
2. <span data-ttu-id="d8a16-127">Hello `SELECT TRANSFORM ... USING` příkaz vybere data z hello `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-127">hello `SELECT TRANSFORM ... USING` statement selects data from hello `hivesampletable`.</span></span> <span data-ttu-id="d8a16-128">Také předá hello clientid, devicemake a devicemodel hodnoty toohello `hiveudf.py` skriptu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-128">It also passes hello clientid, devicemake, and devicemodel values toohello `hiveudf.py` script.</span></span>
3. <span data-ttu-id="d8a16-129">Hello `AS` klauzule popisuje hello pole vrácená z `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-129">hello `AS` clause describes hello fields returned from `hiveudf.py`.</span></span>

<a name="streamingpy"></a>

### <a name="create-hello-hiveudfpy-file"></a><span data-ttu-id="d8a16-130">Vytvoření souboru hiveudf.py hello</span><span class="sxs-lookup"><span data-stu-id="d8a16-130">Create hello hiveudf.py file</span></span>


<span data-ttu-id="d8a16-131">Na vašem vývojovém prostředí, vytvořte textový soubor s názvem `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-131">On your development environment, create a text file named `hiveudf.py`.</span></span> <span data-ttu-id="d8a16-132">Použijte následující kód jako obsah hello hello souboru hello:</span><span class="sxs-lookup"><span data-stu-id="d8a16-132">Use hello following code as hello contents of hello file:</span></span>

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

<span data-ttu-id="d8a16-133">Tento skript provede hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="d8a16-133">This script performs hello following actions:</span></span>

1. <span data-ttu-id="d8a16-134">Čtení řádek dat z STDIN.</span><span class="sxs-lookup"><span data-stu-id="d8a16-134">Read a line of data from STDIN.</span></span>
2. <span data-ttu-id="d8a16-135">Hello koncové znak nového řádku je odebrat pomocí `string.strip(line, "\n ")`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-135">hello trailing newline character is removed using `string.strip(line, "\n ")`.</span></span>
3. <span data-ttu-id="d8a16-136">Při provádění zpracování datového proudu, jeden řádek obsahuje všechny hodnoty hello s tabulátorem mezi každé hodnotě.</span><span class="sxs-lookup"><span data-stu-id="d8a16-136">When doing stream processing, a single line contains all hello values with a tab character between each value.</span></span> <span data-ttu-id="d8a16-137">Proto `string.split(line, "\t")` lze použít toosplit hello vstup na každé kartě vrácení právě hello pole.</span><span class="sxs-lookup"><span data-stu-id="d8a16-137">So `string.split(line, "\t")` can be used toosplit hello input at each tab, returning just hello fields.</span></span>
4. <span data-ttu-id="d8a16-138">Po dokončení zpracování výstup hello musí být napsané tooSTDOUT jako jeden řádek, na kartě mezi každé pole.</span><span class="sxs-lookup"><span data-stu-id="d8a16-138">When processing is complete, hello output must be written tooSTDOUT as a single line, with a tab between each field.</span></span> <span data-ttu-id="d8a16-139">Například, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-139">For example, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span></span>
5. <span data-ttu-id="d8a16-140">Hello `while` smyčky opakuje, dokud ne `line` je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d8a16-140">hello `while` loop repeats until no `line` is read.</span></span>

<span data-ttu-id="d8a16-141">výstup skriptu Hello je tvořen hello vstupní hodnoty pro `devicemake` a `devicemodel`, a hodnota hash hello zřetězených hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-141">hello script output is a concatenation of hello input values for `devicemake` and `devicemodel`, and a hash of hello concatenated value.</span></span>

<span data-ttu-id="d8a16-142">V tématu [spuštění příkladů hello](#running) jak toorun v tomto příkladu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8a16-142">See [Running hello examples](#running) for how toorun this example on your HDInsight cluster.</span></span>

## <span data-ttu-id="d8a16-143"><a name="pigpython"></a>Pig UDF</span><span class="sxs-lookup"><span data-stu-id="d8a16-143"><a name="pigpython"></a>Pig UDF</span></span>

<span data-ttu-id="d8a16-144">Skript v jazyce Python lze použít jako UDF z Pig prostřednictvím hello `GENERATE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8a16-144">A Python script can be used as a UDF from Pig through hello `GENERATE` statement.</span></span> <span data-ttu-id="d8a16-145">Můžete spustit skript hello pomocí Jython nebo C Python.</span><span class="sxs-lookup"><span data-stu-id="d8a16-145">You can run hello script using either Jython or C Python.</span></span>

* <span data-ttu-id="d8a16-146">Jython běží na hello JVM a volat nativně z Pig.</span><span class="sxs-lookup"><span data-stu-id="d8a16-146">Jython runs on hello JVM, and can natively be called from Pig.</span></span>
* <span data-ttu-id="d8a16-147">C Python je externí proces, takže hello data z Pig na hello odeslání JVM toohello skriptu, který běží v procesu Python.</span><span class="sxs-lookup"><span data-stu-id="d8a16-147">C Python is an external process, so hello data from Pig on hello JVM is sent out toohello script running in a Python process.</span></span> <span data-ttu-id="d8a16-148">výstup Hello hello skript v jazyce Python je odeslána zpět do Pig.</span><span class="sxs-lookup"><span data-stu-id="d8a16-148">hello output of hello Python script is sent back into Pig.</span></span>

<span data-ttu-id="d8a16-149">překladač Pythonu toospecify hello, použijte `register` při odkazování na skript v jazyce Python hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-149">toospecify hello Python interpreter, use `register` when referencing hello Python script.</span></span> <span data-ttu-id="d8a16-150">Hello následující příklady zaregistrovat skriptů Pig jako `myfuncs`:</span><span class="sxs-lookup"><span data-stu-id="d8a16-150">hello following examples register scripts with Pig as `myfuncs`:</span></span>

* <span data-ttu-id="d8a16-151">**toouse Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="d8a16-151">**toouse Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`</span></span>
* <span data-ttu-id="d8a16-152">**toouse C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="d8a16-152">**toouse C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8a16-153">Při použití Jython, hello cestě toohello pig_jython souboru může být místní cesta nebo WASB: / / cesta.</span><span class="sxs-lookup"><span data-stu-id="d8a16-153">When using Jython, hello path toohello pig_jython file can be either a local path or a WASB:// path.</span></span> <span data-ttu-id="d8a16-154">Při použití C Python, ale musí odkazovat souboru na hello místního systému souborů, že používáte úlohu Pig hello toosubmit uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-154">However, when using C Python, you must reference a file on hello local file system of hello node that you are using toosubmit hello Pig job.</span></span>

<span data-ttu-id="d8a16-155">Jakmile po registraci hello Pig Latin pro tento příklad hello stejný pro obě:</span><span class="sxs-lookup"><span data-stu-id="d8a16-155">Once past registration, hello Pig Latin for this example is hello same for both:</span></span>

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

<span data-ttu-id="d8a16-156">Zde je, jaké jsou v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="d8a16-156">Here's what this example does:</span></span>

1. <span data-ttu-id="d8a16-157">první řádek Hello načte hello ukázkový datový soubor `sample.log` do `LOGS`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-157">hello first line loads hello sample data file, `sample.log` into `LOGS`.</span></span> <span data-ttu-id="d8a16-158">Definuje také každý záznam jako `chararray`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-158">It also defines each record as a `chararray`.</span></span>
2. <span data-ttu-id="d8a16-159">Další řádek Hello odfiltruje všechny hodnoty null ukládání hello výsledek operace hello do `LOG`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-159">hello next line filters out any null values, storing hello result of hello operation into `LOG`.</span></span>
3. <span data-ttu-id="d8a16-160">V dalším kroku ji iteruje nad hello záznamy v `LOG` a používá `GENERATE` tooinvoke hello `create_structure` metoda obsažených ve skriptu Python nebo Jython hello načíst jako `myfuncs`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-160">Next, it iterates over hello records in `LOG` and uses `GENERATE` tooinvoke hello `create_structure` method contained in hello Python/Jython script loaded as `myfuncs`.</span></span> <span data-ttu-id="d8a16-161">`LINE`je použité toopass hello aktuální záznamů toohello funkce.</span><span class="sxs-lookup"><span data-stu-id="d8a16-161">`LINE` is used toopass hello current record toohello function.</span></span>
4. <span data-ttu-id="d8a16-162">Nakonec hello výstupy jsou dumpingových tooSTDOUT pomocí hello `DUMP` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8a16-162">Finally, hello outputs are dumped tooSTDOUT using hello `DUMP` command.</span></span> <span data-ttu-id="d8a16-163">Tento příkaz zobrazí výsledky hello po dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-163">This command displays hello results after hello operation completes.</span></span>

### <a name="create-hello-pigudfpy-file"></a><span data-ttu-id="d8a16-164">Vytvoření souboru pigudf.py hello</span><span class="sxs-lookup"><span data-stu-id="d8a16-164">Create hello pigudf.py file</span></span>

<span data-ttu-id="d8a16-165">Na vašem vývojovém prostředí, vytvořte textový soubor s názvem `pigudf.py`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-165">On your development environment, create a text file named `pigudf.py`.</span></span> <span data-ttu-id="d8a16-166">Použijte následující kód jako obsah hello hello souboru hello:</span><span class="sxs-lookup"><span data-stu-id="d8a16-166">Use hello following code as hello contents of hello file:</span></span>

<a name="streamingpy"></a>

```python
# Uncomment hello following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

<span data-ttu-id="d8a16-167">V příkladu Pig Latin hello jsme definovali hello `LINE` vstupní jako chararray, protože neexistuje žádný konzistentní schéma pro vstup hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-167">In hello Pig Latin example, we defined hello `LINE` input as a chararray because there is no consistent schema for hello input.</span></span> <span data-ttu-id="d8a16-168">Hello skript v jazyce Python transformuje hello dat na konzistentní schéma pro výstup.</span><span class="sxs-lookup"><span data-stu-id="d8a16-168">hello Python script transforms hello data into a consistent schema for output.</span></span>

1. <span data-ttu-id="d8a16-169">Hello `@outputSchema` příkaz definuje formát hello hello dat, která je vrácena tooPig.</span><span class="sxs-lookup"><span data-stu-id="d8a16-169">hello `@outputSchema` statement defines hello format of hello data that is returned tooPig.</span></span> <span data-ttu-id="d8a16-170">V takovém případě má **datový kontejner**, což je datový typ Pig.</span><span class="sxs-lookup"><span data-stu-id="d8a16-170">In this case, it's a **data bag**, which is a Pig data type.</span></span> <span data-ttu-id="d8a16-171">kontejner objektů a dat Hello obsahuje hello následující pole, které jsou chararray (řetězce):</span><span class="sxs-lookup"><span data-stu-id="d8a16-171">hello bag contains hello following fields, all of which are chararray (strings):</span></span>

   * <span data-ttu-id="d8a16-172">Date – hello datum hello vytvoření položky protokolu</span><span class="sxs-lookup"><span data-stu-id="d8a16-172">date - hello date hello log entry was created</span></span>
   * <span data-ttu-id="d8a16-173">Time – hello čas vytvoření položky protokolu hello</span><span class="sxs-lookup"><span data-stu-id="d8a16-173">time - hello time hello log entry was created</span></span>
   * <span data-ttu-id="d8a16-174">Název třídy - položku hello název třídy hello vytvořený pro</span><span class="sxs-lookup"><span data-stu-id="d8a16-174">classname - hello class name hello entry was created for</span></span>
   * <span data-ttu-id="d8a16-175">úroveň – úroveň protokolu hello</span><span class="sxs-lookup"><span data-stu-id="d8a16-175">level - hello log level</span></span>
   * <span data-ttu-id="d8a16-176">Podrobnosti o - verbose podrobnosti hello položka protokolu</span><span class="sxs-lookup"><span data-stu-id="d8a16-176">detail - verbose details for hello log entry</span></span>

2. <span data-ttu-id="d8a16-177">V dalším kroku hello `def create_structure(input)` definuje hello funkce, která Pig předá řádku položek.</span><span class="sxs-lookup"><span data-stu-id="d8a16-177">Next, hello `def create_structure(input)` defines hello function that Pig passes line items to.</span></span>

3. <span data-ttu-id="d8a16-178">Hello příklad dat, `sample.log`, většinou vyhovuje toohello datum, čas, classname úrovni a podrobností chceme tooreturn schématu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-178">hello example data, `sample.log`, mostly conforms toohello date, time, classname, level, and detail schema we want tooreturn.</span></span> <span data-ttu-id="d8a16-179">Obsahuje však pár řádků, které začínají `*java.lang.Exception*`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-179">However, it contains a few lines that begin with `*java.lang.Exception*`.</span></span> <span data-ttu-id="d8a16-180">Tyto řádky musí být upravené toomatch hello schématu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-180">These lines must be modified toomatch hello schema.</span></span> <span data-ttu-id="d8a16-181">Hello `if` příkaz kontroluje pro ty, a potom středisek hello vstupní data toomove hello `*java.lang.Exception*` end toohello řetězec, přináší hello data v řádku pomocí našich očekávaný výstup schématu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-181">hello `if` statement checks for those, then massages hello input data toomove hello `*java.lang.Exception*` string toohello end, bringing hello data in-line with our expected output schema.</span></span>

4. <span data-ttu-id="d8a16-182">V dalším kroku hello `split` příkaz je použité toosplit hello dat hello první čtyři znaky.</span><span class="sxs-lookup"><span data-stu-id="d8a16-182">Next, hello `split` command is used toosplit hello data at hello first four space characters.</span></span> <span data-ttu-id="d8a16-183">výstup Hello je přiřazen do `date`, `time`, `classname`, `level`, a `detail`.</span><span class="sxs-lookup"><span data-stu-id="d8a16-183">hello output is assigned into `date`, `time`, `classname`, `level`, and `detail`.</span></span>

5. <span data-ttu-id="d8a16-184">Nakonec hello hodnoty jsou vráceny tooPig.</span><span class="sxs-lookup"><span data-stu-id="d8a16-184">Finally, hello values are returned tooPig.</span></span>

<span data-ttu-id="d8a16-185">Když jsou hello data vrácena tooPig, má konzistentní schéma definované v hello `@outputSchema` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8a16-185">When hello data is returned tooPig, it has a consistent schema as defined in hello `@outputSchema` statement.</span></span>

## <span data-ttu-id="d8a16-186"><a name="running"></a>Nahrání a spuštění příkladů hello</span><span class="sxs-lookup"><span data-stu-id="d8a16-186"><a name="running"></a>Upload and run hello examples</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8a16-187">Hello **SSH** kroky fungovat jenom s clusterem HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="d8a16-187">hello **SSH** steps only work with a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="d8a16-188">Hello **prostředí PowerShell** kroky fungovat s clusterem Linux nebo HDInsight se systémem Windows, ale vyžaduje klienta Windows.</span><span class="sxs-lookup"><span data-stu-id="d8a16-188">hello **PowerShell** steps work with either a Linux or Windows-based HDInsight cluster, but require a Windows client.</span></span>

### <a name="ssh"></a><span data-ttu-id="d8a16-189">SSH</span><span class="sxs-lookup"><span data-stu-id="d8a16-189">SSH</span></span>

<span data-ttu-id="d8a16-190">Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d8a16-190">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="d8a16-191">Použití `scp` toocopy hello soubory tooyour HDInsight clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8a16-191">Use `scp` toocopy hello files tooyour HDInsight cluster.</span></span> <span data-ttu-id="d8a16-192">Například hello následující příkaz kopie hello soubory tooa clusteru s názvem **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="d8a16-192">For example, hello following command copies hello files tooa cluster named **mycluster**.</span></span>

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. <span data-ttu-id="d8a16-193">Použití SSH tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8a16-193">Use SSH tooconnect toohello cluster.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="d8a16-194">Z relace SSH hello přidáte, že soubory python hello načtený dříve toohello WASB úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="d8a16-194">From hello SSH session, add hello python files uploaded previously toohello WASB storage for hello cluster.</span></span>

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

<span data-ttu-id="d8a16-195">Po odeslání hello souborů, použijte hello tyto kroky toorun hello Hive a Pig úlohy.</span><span class="sxs-lookup"><span data-stu-id="d8a16-195">After uploading hello files, use hello following steps toorun hello Hive and Pig jobs.</span></span>

#### <a name="use-hello-hive-udf"></a><span data-ttu-id="d8a16-196">Použití hello Hive UDF</span><span class="sxs-lookup"><span data-stu-id="d8a16-196">Use hello Hive UDF</span></span>

1. <span data-ttu-id="d8a16-197">Použití hello `hive` příkazové prostředí hive toostart hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-197">Use hello `hive` command toostart hello hive shell.</span></span> <span data-ttu-id="d8a16-198">Měli byste vidět `hive>` výzvu po načetl hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8a16-198">You should see a `hive>` prompt once hello shell has loaded.</span></span>

2. <span data-ttu-id="d8a16-199">Zadejte následující dotaz u hello hello `hive>` řádku:</span><span class="sxs-lookup"><span data-stu-id="d8a16-199">Enter hello following query at hello `hive>` prompt:</span></span>

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. <span data-ttu-id="d8a16-200">Po zadání hello poslední řádek, začněte hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="d8a16-200">After entering hello last line, hello job should start.</span></span> <span data-ttu-id="d8a16-201">Po dokončení úlohy hello vrátí výstup podobný toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d8a16-201">Once hello job completes, it returns output similar toohello following example:</span></span>

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-hello-pig-udf"></a><span data-ttu-id="d8a16-202">Použití hello Pig UDF</span><span class="sxs-lookup"><span data-stu-id="d8a16-202">Use hello Pig UDF</span></span>

1. <span data-ttu-id="d8a16-203">Použití hello `pig` příkazové prostředí toostart hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-203">Use hello `pig` command toostart hello shell.</span></span> <span data-ttu-id="d8a16-204">Zobrazí `grunt>` výzvu po načetl hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8a16-204">You see a `grunt>` prompt once hello shell has loaded.</span></span>

2. <span data-ttu-id="d8a16-205">Zadejte následující příkazy v hello hello `grunt>` řádku:</span><span class="sxs-lookup"><span data-stu-id="d8a16-205">Enter hello following statements at hello `grunt>` prompt:</span></span>

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. <span data-ttu-id="d8a16-206">Po zadání hello následující řádek, začněte hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="d8a16-206">After entering hello following line, hello job should start.</span></span> <span data-ttu-id="d8a16-207">Po dokončení úlohy hello vrátí výstup podobný toohello následující data:</span><span class="sxs-lookup"><span data-stu-id="d8a16-207">Once hello job completes, it returns output similar toohello following data:</span></span>

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. <span data-ttu-id="d8a16-208">Použít `quit` tooexit hello Grunt prostředí a pak použijte hello následující tooedit hello pigudf.py soubor na hello místního systému souborů:</span><span class="sxs-lookup"><span data-stu-id="d8a16-208">Use `quit` tooexit hello Grunt shell, and then use hello following tooedit hello pigudf.py file on hello local file system:</span></span>

    ```bash
    nano pigudf.py
    ```

5. <span data-ttu-id="d8a16-209">Jednou v editoru hello, zrušte komentář u následující řádek odebráním hello hello `#` znak z hello začátku řádku hello:</span><span class="sxs-lookup"><span data-stu-id="d8a16-209">Once in hello editor, uncomment hello following line by removing hello `#` character from hello beginning of hello line:</span></span>

    ```bash
    #from pig_util import outputSchema
    ```

    <span data-ttu-id="d8a16-210">Jakmile hello změny, použijte editor hello tooexit Ctrl + X.</span><span class="sxs-lookup"><span data-stu-id="d8a16-210">Once hello change has been made, use Ctrl+X tooexit hello editor.</span></span> <span data-ttu-id="d8a16-211">Vyberte Y a potom zadejte toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="d8a16-211">Select Y, and then enter toosave hello changes.</span></span>

6. <span data-ttu-id="d8a16-212">Použití hello `pig` příkazové prostředí hello toostart znovu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-212">Use hello `pig` command toostart hello shell again.</span></span> <span data-ttu-id="d8a16-213">Jakmile jste na hello `grunt>` řádku, použijte následující skript v jazyce Python toorun hello pomocí překladač Pythonu C hello hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-213">Once you are at hello `grunt>` prompt, use hello following toorun hello Python script using hello C Python interpreter.</span></span>

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    <span data-ttu-id="d8a16-214">Po dokončení této úlohy, měli byste vidět hello stejnou výstupní jako když byl již spuštěn skript hello pomocí Jython.</span><span class="sxs-lookup"><span data-stu-id="d8a16-214">Once this job completes, you should see hello same output as when you previously ran hello script using Jython.</span></span>

### <a name="powershell-upload-hello-files"></a><span data-ttu-id="d8a16-215">Prostředí PowerShell: Soubory hello nahrávání</span><span class="sxs-lookup"><span data-stu-id="d8a16-215">PowerShell: Upload hello files</span></span>

<span data-ttu-id="d8a16-216">Můžete použít PowerShell tooupload hello soubory toohello serveru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8a16-216">You can use PowerShell tooupload hello files toohello HDInsight server.</span></span> <span data-ttu-id="d8a16-217">Použijte následující skript tooupload hello Python soubory hello:</span><span class="sxs-lookup"><span data-stu-id="d8a16-217">Use hello following script tooupload hello Python files:</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="d8a16-218">Hello kroky v této části použijte prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8a16-218">hello steps in this section use Azure PowerShell.</span></span> <span data-ttu-id="d8a16-219">Další informace o použití prostředí Azure PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d8a16-219">For more information on using Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
# Change hello path toomatch hello file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage content and upload hello file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

Set-AzureStorageBlobContent `
    -File $pathToStreamingFile `
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context

Set-AzureStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```
> [!IMPORTANT]
> <span data-ttu-id="d8a16-220">Změna hello `C:\path\to` hodnotu toohello cesta toohello soubory na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8a16-220">Change hello `C:\path\to` value toohello path toohello files on your development environment.</span></span>

<span data-ttu-id="d8a16-221">Tento skript načte informace pro váš cluster HDInsight a pak extrahuje hello účtu a klíč pro hello výchozí účet úložiště a nahrávání hello soubory toohello kořenovém kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-221">This script retrieves information for your HDInsight cluster, then extracts hello account and key for hello default storage account, and uploads hello files toohello root of hello container.</span></span>

> [!NOTE]
> <span data-ttu-id="d8a16-222">Další informace o nahrávání souborů najdete v tématu hello [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d8a16-222">For more information on uploading files, see hello [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md) document.</span></span>

#### <a name="powershell-use-hello-hive-udf"></a><span data-ttu-id="d8a16-223">Prostředí PowerShell: Hello Hive UDF pomocí</span><span class="sxs-lookup"><span data-stu-id="d8a16-223">PowerShell: Use hello Hive UDF</span></span>

<span data-ttu-id="d8a16-224">Prostředí PowerShell může být také použít tooremotely spouštět dotazy Hive.</span><span class="sxs-lookup"><span data-stu-id="d8a16-224">PowerShell can also be used tooremotely run Hive queries.</span></span> <span data-ttu-id="d8a16-225">Použití hello následující toorun skript prostředí PowerShell dotaz Hive, který používá **hiveudf.py** skriptu:</span><span class="sxs-lookup"><span data-stu-id="d8a16-225">Use hello following PowerShell script toorun a Hive query that uses **hiveudf.py** script:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8a16-226">Dřív, než spustíte, skript hello vás vyzve k zadání hello HTTPs nebo správu informací o účtu pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8a16-226">Before running, hello script prompts you for hello HTTPs/Admin account information for your HDInsight cluster.</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

# If using a Windows-based HDInsight cluster, change hello USING statement to:
# "USING 'D:\Python27\python.exe hiveudf.py' AS " +
$HiveQuery = "add file wasb:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
                "(clientid string, phoneLabel string, phoneHash string) " +
                "FROM hivesampletable " +
                "ORDER BY clientid LIMIT 50;"

$jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
    -Query $HiveQuery

$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds
Write-Host "Wait for hello Hive job toocomplete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment hello following toosee stderr output
# Get-AzureRmHDInsightJobOutput `
#   -Clustername $clusterName `
#   -JobId $job.JobId `
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="d8a16-227">Hello výstup hello **Hive** úlohy by se měla zobrazit podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d8a16-227">hello output for hello **Hive** job should appear similar toohello following example:</span></span>

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a><span data-ttu-id="d8a16-228">Pig (Jython)</span><span class="sxs-lookup"><span data-stu-id="d8a16-228">Pig (Jython)</span></span>

<span data-ttu-id="d8a16-229">Prostředí PowerShell může být také použít toorun Pig Latin úlohy.</span><span class="sxs-lookup"><span data-stu-id="d8a16-229">PowerShell can also be used toorun Pig Latin jobs.</span></span> <span data-ttu-id="d8a16-230">toorun Pig Latin úlohu, která používá hello **pigudf.py** skriptu, použijte hello následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d8a16-230">toorun a Pig Latin job that uses hello **pigudf.py** script, use hello following PowerShell script:</span></span>

> [!NOTE]
> <span data-ttu-id="d8a16-231">Při odesílání vzdáleně úlohu pomocí prostředí PowerShell, není možné toouse C Python jako hello překladač.</span><span class="sxs-lookup"><span data-stu-id="d8a16-231">When remotely submitting a job using PowerShell, it is not possible toouse C Python as hello interpreter.</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

$PigQuery = "Register wasb:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);" +
            "LOG = FILTER LOGS by LINE is not null;" +
            "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
            "DUMP DETAILS;"

$jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

Write-Host "Wait for hello Pig job toocomplete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment hello following toosee stderr output
# Get-AzureRmHDInsightJobOutput `
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="d8a16-232">Hello výstup hello **Pig** úlohy by se měla zobrazit podobné toohello následující data:</span><span class="sxs-lookup"><span data-stu-id="d8a16-232">hello output for hello **Pig** job should appear similar toohello following data:</span></span>

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <span data-ttu-id="d8a16-233"><a name="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d8a16-233"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="errors-when-running-jobs"></a><span data-ttu-id="d8a16-234">Chyby při spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="d8a16-234">Errors when running jobs</span></span>

<span data-ttu-id="d8a16-235">Při spuštění úlohy hive hello, může dojít k chybě podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="d8a16-235">When running hello hive job, you may encounter an error similar toohello following text:</span></span>

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing tooyour custom script. It may have crashed with an error.

<span data-ttu-id="d8a16-236">Tento problém může být způsobeno hello konců řádků v souboru Python hello.</span><span class="sxs-lookup"><span data-stu-id="d8a16-236">This problem may be caused by hello line endings in hello Python file.</span></span> <span data-ttu-id="d8a16-237">Mnohé editory Windows výchozí toousing Line FEED jako hello řádku ukončení, ale aplikace Linux obvykle předpokládají LF.</span><span class="sxs-lookup"><span data-stu-id="d8a16-237">Many Windows editors default toousing CRLF as hello line ending, but Linux applications usually expect LF.</span></span>

<span data-ttu-id="d8a16-238">Můžete použít následující znaky hello CR tooremove příkazy prostředí PowerShell před nahráním souboru tooHDInsight hello hello:</span><span class="sxs-lookup"><span data-stu-id="d8a16-238">You can use hello following PowerShell statements tooremove hello CR characters before uploading hello file tooHDInsight:</span></span>

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a><span data-ttu-id="d8a16-239">Skripty prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d8a16-239">PowerShell scripts</span></span>

<span data-ttu-id="d8a16-240">Obě hello ukázkové skripty prostředí PowerShell použít toorun hello příklady obsahují komentáři řádek, který zobrazí chyba výstup hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="d8a16-240">Both of hello example PowerShell scripts used toorun hello examples contain a commented line that displays error output for hello job.</span></span> <span data-ttu-id="d8a16-241">Pokud nevidíte hello očekávaný výstup hello úlohy, zrušte komentář u následující hello řádku a zobrazit, pokud informace o chybě hello indikuje problém.</span><span class="sxs-lookup"><span data-stu-id="d8a16-241">If you are not seeing hello expected output for hello job, uncomment hello following line and see if hello error information indicates a problem.</span></span>

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="d8a16-242">informace o chybě Hello (STDERR) a výsledek hello hello úlohy (STDOUT) jsou také zaznamenané tooyour HDInsight úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8a16-242">hello error information (STDERR) and hello result of hello job (STDOUT) are also logged tooyour HDInsight storage.</span></span>

| <span data-ttu-id="d8a16-243">Pro tuto úlohu...</span><span class="sxs-lookup"><span data-stu-id="d8a16-243">For this job...</span></span> | <span data-ttu-id="d8a16-244">Podívejte se na tyto soubory v kontejneru objektů blob hello</span><span class="sxs-lookup"><span data-stu-id="d8a16-244">Look at these files in hello blob container</span></span> |
| --- | --- |
| <span data-ttu-id="d8a16-245">Hive</span><span class="sxs-lookup"><span data-stu-id="d8a16-245">Hive</span></span> |<span data-ttu-id="d8a16-246">/ HivePython/stderr</span><span class="sxs-lookup"><span data-stu-id="d8a16-246">/HivePython/stderr</span></span><p><span data-ttu-id="d8a16-247">/ HivePython/stdout</span><span class="sxs-lookup"><span data-stu-id="d8a16-247">/HivePython/stdout</span></span> |
| <span data-ttu-id="d8a16-248">Pig</span><span class="sxs-lookup"><span data-stu-id="d8a16-248">Pig</span></span> |<span data-ttu-id="d8a16-249">/ PigPython/stderr</span><span class="sxs-lookup"><span data-stu-id="d8a16-249">/PigPython/stderr</span></span><p><span data-ttu-id="d8a16-250">/ PigPython/stdout</span><span class="sxs-lookup"><span data-stu-id="d8a16-250">/PigPython/stdout</span></span> |

## <span data-ttu-id="d8a16-251"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8a16-251"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="d8a16-252">Pokud potřebujete tooload moduly jazyka Python, které nejsou k dispozici ve výchozím nastavení, najdete v části [jak toodeploy modulu tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8a16-252">If you need tooload Python modules that aren't provided by default, see [How toodeploy a module tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span></span>

<span data-ttu-id="d8a16-253">Další způsoby toouse Pig, Hive a toolearn o použití prostředí MapReduce najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="d8a16-253">For other ways toouse Pig, Hive, and toolearn about using MapReduce, see hello following documents:</span></span>

* [<span data-ttu-id="d8a16-254">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8a16-254">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="d8a16-255">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8a16-255">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="d8a16-256">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8a16-256">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
