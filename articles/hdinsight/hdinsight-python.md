---
title: "Python UDF s Apache Hive a vepřových - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Python uživatele definované funkce (UDF) z Hive a Pig v HDInsight, technologie zásobníku Hadoop v Azure."
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
ms.openlocfilehash: 9b67ded05a52f1e68580434667495cf6cf939871
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a><span data-ttu-id="79eb6-103">Funkce (UDF) s Hive a Pig definované uživatelem Python použití v HDInsight</span><span class="sxs-lookup"><span data-stu-id="79eb6-103">Use Python User Defined Functions (UDF) with Hive and Pig in HDInsight</span></span>

<span data-ttu-id="79eb6-104">Naučte se používat s Apache Hive a Pig v Hadoop v Azure HDInsight Python uživatelsky definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="79eb6-104">Learn how to use Python user-defined functions (UDF) with Apache Hive and Pig in Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="79eb6-105"><a name="python"></a>Python v HDInsight</span><span class="sxs-lookup"><span data-stu-id="79eb6-105"><a name="python"></a>Python on HDInsight</span></span>

<span data-ttu-id="79eb6-106">Python2.7 je nainstalována ve výchozím nastavení na HDInsight 3.0 a novější.</span><span class="sxs-lookup"><span data-stu-id="79eb6-106">Python2.7 is installed by default on HDInsight 3.0 and later.</span></span> <span data-ttu-id="79eb6-107">Apache Hive lze použít s touto verzí jazyka Python pro zpracování datového proudu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-107">Apache Hive can be used with this version of Python for stream processing.</span></span> <span data-ttu-id="79eb6-108">Zpracování datového proudu používá STDOUT a stdin – k předávání dat mezi Hive a UDF.</span><span class="sxs-lookup"><span data-stu-id="79eb6-108">Stream processing uses STDOUT and STDIN to pass data between Hive and the UDF.</span></span>

<span data-ttu-id="79eb6-109">HDInsight zahrnuje taky Jython, což je implementace Python napsanou v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="79eb6-109">HDInsight also includes Jython, which is a Python implementation written in Java.</span></span> <span data-ttu-id="79eb6-110">Jython běží přímo na virtuálním počítači Java a nepoužívá streamování.</span><span class="sxs-lookup"><span data-stu-id="79eb6-110">Jython runs directly on the Java Virtual Machine and does not use streaming.</span></span> <span data-ttu-id="79eb6-111">Jython je doporučené překladač Pythonu při použití Python s Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-111">Jython is the recommended Python interpreter when using Python with Pig.</span></span>

> [!WARNING]
> <span data-ttu-id="79eb6-112">Kroky v tomto dokumentu provést následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="79eb6-112">The steps in this document make the following assumptions:</span></span> 
>
> * <span data-ttu-id="79eb6-113">Vytváření skriptů Python ve svém místním vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="79eb6-113">You create the Python scripts on your local development environment.</span></span>
> * <span data-ttu-id="79eb6-114">Nahrát skripty do HDInsight pomocí `scp` příkaz z relace místní Bash nebo zadaný skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="79eb6-114">You upload the scripts to HDInsight using either the `scp` command from a local Bash session or the provided PowerShell script.</span></span>
>
> <span data-ttu-id="79eb6-115">Pokud chcete použít [prostředí cloudu Azure (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview pro práci s HDInsight, pak musíte:</span><span class="sxs-lookup"><span data-stu-id="79eb6-115">If you want to use the [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview to work with HDInsight, then you must:</span></span>
>
> * <span data-ttu-id="79eb6-116">Vytváření skriptů v prostředí shell cloudovému prostředí.</span><span class="sxs-lookup"><span data-stu-id="79eb6-116">Create the scripts inside the cloud shell environment.</span></span>
> * <span data-ttu-id="79eb6-117">Použití `scp` ukládání souborů z prostředí cloudu do HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79eb6-117">Use `scp` to upload the files from the cloud shell to HDInsight.</span></span>
> * <span data-ttu-id="79eb6-118">Použití `ssh` z cloudové prostředí pro připojení k HDInsight a spuštění příkladů.</span><span class="sxs-lookup"><span data-stu-id="79eb6-118">Use `ssh` from the cloud shell to connect to HDInsight and run the examples.</span></span>

## <span data-ttu-id="79eb6-119"><a name="hivepython"></a>Hive UDF</span><span class="sxs-lookup"><span data-stu-id="79eb6-119"><a name="hivepython"></a>Hive UDF</span></span>

<span data-ttu-id="79eb6-120">Python lze použít jako UDF z Hive prostřednictvím HiveQL `TRANSFORM` příkaz.</span><span class="sxs-lookup"><span data-stu-id="79eb6-120">Python can be used as a UDF from Hive through the HiveQL `TRANSFORM` statement.</span></span> <span data-ttu-id="79eb6-121">Například následující HiveQL vyvolá `hiveudf.py` souboru uložený v výchozí účet úložiště Azure pro cluster.</span><span class="sxs-lookup"><span data-stu-id="79eb6-121">For example, the following HiveQL invokes the `hiveudf.py` file stored in the default Azure Storage account for the cluster.</span></span>

<span data-ttu-id="79eb6-122">**HDInsight se systémem Linux**</span><span class="sxs-lookup"><span data-stu-id="79eb6-122">**Linux-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

<span data-ttu-id="79eb6-123">**HDInsight se systémem Windows**</span><span class="sxs-lookup"><span data-stu-id="79eb6-123">**Windows-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> <span data-ttu-id="79eb6-124">Na clusterech HDInsight se systémem Windows `USING` klauzule musíte zadat úplnou cestu k python.exe.</span><span class="sxs-lookup"><span data-stu-id="79eb6-124">On Windows-based HDInsight clusters, the `USING` clause must specify the full path to python.exe.</span></span>

<span data-ttu-id="79eb6-125">Zde je, jaké jsou v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="79eb6-125">Here's what this example does:</span></span>

1. <span data-ttu-id="79eb6-126">`add file` Přidá příkaz na začátku souboru `hiveudf.py` soubor do distribuované mezipaměti, tak, aby byl přístupný pro všechny uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="79eb6-126">The `add file` statement at the beginning of the file adds the `hiveudf.py` file to the distributed cache, so it's accessible by all nodes in the cluster.</span></span>
2. <span data-ttu-id="79eb6-127">`SELECT TRANSFORM ... USING` Příkaz vybere data z `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-127">The `SELECT TRANSFORM ... USING` statement selects data from the `hivesampletable`.</span></span> <span data-ttu-id="79eb6-128">Také předává clientid, devicemake a devicemodel hodnoty, které mají `hiveudf.py` skriptu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-128">It also passes the clientid, devicemake, and devicemodel values to the `hiveudf.py` script.</span></span>
3. <span data-ttu-id="79eb6-129">`AS` Klauzule popisuje pole vrácená z `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-129">The `AS` clause describes the fields returned from `hiveudf.py`.</span></span>

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a><span data-ttu-id="79eb6-130">Vytvoření souboru hiveudf.py</span><span class="sxs-lookup"><span data-stu-id="79eb6-130">Create the hiveudf.py file</span></span>


<span data-ttu-id="79eb6-131">Na vašem vývojovém prostředí, vytvořte textový soubor s názvem `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-131">On your development environment, create a text file named `hiveudf.py`.</span></span> <span data-ttu-id="79eb6-132">Použijte následující kód jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="79eb6-132">Use the following code as the contents of the file:</span></span>

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

<span data-ttu-id="79eb6-133">Tento skript provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="79eb6-133">This script performs the following actions:</span></span>

1. <span data-ttu-id="79eb6-134">Čtení řádek dat z STDIN.</span><span class="sxs-lookup"><span data-stu-id="79eb6-134">Read a line of data from STDIN.</span></span>
2. <span data-ttu-id="79eb6-135">Koncový znak nového řádku je odebrat pomocí `string.strip(line, "\n ")`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-135">The trailing newline character is removed using `string.strip(line, "\n ")`.</span></span>
3. <span data-ttu-id="79eb6-136">Při provádění zpracování datového proudu, jeden řádek obsahuje všechny hodnoty pomocí znaku tabulátoru mezi každé hodnotě.</span><span class="sxs-lookup"><span data-stu-id="79eb6-136">When doing stream processing, a single line contains all the values with a tab character between each value.</span></span> <span data-ttu-id="79eb6-137">Proto `string.split(line, "\t")` slouží k rozdělení vstupu v každé kartě vrací pouze pole.</span><span class="sxs-lookup"><span data-stu-id="79eb6-137">So `string.split(line, "\t")` can be used to split the input at each tab, returning just the fields.</span></span>
4. <span data-ttu-id="79eb6-138">Po dokončení zpracování musí být výstup zapsané do datového proudu STDOUT jako jeden řádek, na kartě mezi každé pole.</span><span class="sxs-lookup"><span data-stu-id="79eb6-138">When processing is complete, the output must be written to STDOUT as a single line, with a tab between each field.</span></span> <span data-ttu-id="79eb6-139">Například, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-139">For example, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span></span>
5. <span data-ttu-id="79eb6-140">`while` Smyčky opakuje, dokud ne `line` je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="79eb6-140">The `while` loop repeats until no `line` is read.</span></span>

<span data-ttu-id="79eb6-141">Výstup skriptu je tvořen vstupní hodnoty pro `devicemake` a `devicemodel`a hodnota hash zřetězených hodnoty.</span><span class="sxs-lookup"><span data-stu-id="79eb6-141">The script output is a concatenation of the input values for `devicemake` and `devicemodel`, and a hash of the concatenated value.</span></span>

<span data-ttu-id="79eb6-142">V tématu [spuštění příkladů](#running) informace o spuštění tohoto příkladu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79eb6-142">See [Running the examples](#running) for how to run this example on your HDInsight cluster.</span></span>

## <span data-ttu-id="79eb6-143"><a name="pigpython"></a>Pig UDF</span><span class="sxs-lookup"><span data-stu-id="79eb6-143"><a name="pigpython"></a>Pig UDF</span></span>

<span data-ttu-id="79eb6-144">Skript v jazyce Python, můžete využít jako UDF z Pig prostřednictvím `GENERATE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="79eb6-144">A Python script can be used as a UDF from Pig through the `GENERATE` statement.</span></span> <span data-ttu-id="79eb6-145">Můžete spustit skript pomocí Jython nebo C Python.</span><span class="sxs-lookup"><span data-stu-id="79eb6-145">You can run the script using either Jython or C Python.</span></span>

* <span data-ttu-id="79eb6-146">Jython běží na systém JVM a lze volat nativně z Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-146">Jython runs on the JVM, and can natively be called from Pig.</span></span>
* <span data-ttu-id="79eb6-147">C Python je externí proces, takže data z Pig na systém JVM odeslání do skriptu, který běží v procesu Python.</span><span class="sxs-lookup"><span data-stu-id="79eb6-147">C Python is an external process, so the data from Pig on the JVM is sent out to the script running in a Python process.</span></span> <span data-ttu-id="79eb6-148">Výstup skriptu Python je odeslána zpět do Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-148">The output of the Python script is sent back into Pig.</span></span>

<span data-ttu-id="79eb6-149">Pokud chcete zadat překladač Pythonu, použijte `register` při odkazování na skript Pythonu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-149">To specify the Python interpreter, use `register` when referencing the Python script.</span></span> <span data-ttu-id="79eb6-150">Následující příklady zaregistrovat skriptů Pig jako `myfuncs`:</span><span class="sxs-lookup"><span data-stu-id="79eb6-150">The following examples register scripts with Pig as `myfuncs`:</span></span>

* <span data-ttu-id="79eb6-151">**Chcete-li použít Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="79eb6-151">**To use Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`</span></span>
* <span data-ttu-id="79eb6-152">**Použít C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="79eb6-152">**To use C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79eb6-153">Při použití Jython, cestu k souboru pig_jython může být místní cesta nebo WASB: / / cesta.</span><span class="sxs-lookup"><span data-stu-id="79eb6-153">When using Jython, the path to the pig_jython file can be either a local path or a WASB:// path.</span></span> <span data-ttu-id="79eb6-154">Pokud používáte C Python, ale musí odkazovat souboru v místním systému souborů uzlu, který používáte se odeslat úlohu Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-154">However, when using C Python, you must reference a file on the local file system of the node that you are using to submit the Pig job.</span></span>

<span data-ttu-id="79eb6-155">Jednou za registraci Pig Latin pro tento příklad je stejný pro obě:</span><span class="sxs-lookup"><span data-stu-id="79eb6-155">Once past registration, the Pig Latin for this example is the same for both:</span></span>

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

<span data-ttu-id="79eb6-156">Zde je, jaké jsou v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="79eb6-156">Here's what this example does:</span></span>

1. <span data-ttu-id="79eb6-157">První řádek načte ukázkový datový soubor, `sample.log` do `LOGS`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-157">The first line loads the sample data file, `sample.log` into `LOGS`.</span></span> <span data-ttu-id="79eb6-158">Definuje také každý záznam jako `chararray`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-158">It also defines each record as a `chararray`.</span></span>
2. <span data-ttu-id="79eb6-159">Na další řádek odfiltruje všechny hodnoty null ukládání výsledek operace do `LOG`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-159">The next line filters out any null values, storing the result of the operation into `LOG`.</span></span>
3. <span data-ttu-id="79eb6-160">V dalším kroku ji iteruje nad záznamy v `LOG` a používá `GENERATE` má být vyvolán `create_structure` metoda obsažených ve skriptu Python nebo Jython načíst jako `myfuncs`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-160">Next, it iterates over the records in `LOG` and uses `GENERATE` to invoke the `create_structure` method contained in the Python/Jython script loaded as `myfuncs`.</span></span> <span data-ttu-id="79eb6-161">`LINE`slouží k předávání na aktuální záznam funkce.</span><span class="sxs-lookup"><span data-stu-id="79eb6-161">`LINE` is used to pass the current record to the function.</span></span>
4. <span data-ttu-id="79eb6-162">Nakonec jsou zálohované výstupy STDOUT pomocí `DUMP` příkaz.</span><span class="sxs-lookup"><span data-stu-id="79eb6-162">Finally, the outputs are dumped to STDOUT using the `DUMP` command.</span></span> <span data-ttu-id="79eb6-163">Tento příkaz zobrazí výsledky po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="79eb6-163">This command displays the results after the operation completes.</span></span>

### <a name="create-the-pigudfpy-file"></a><span data-ttu-id="79eb6-164">Vytvoření souboru pigudf.py</span><span class="sxs-lookup"><span data-stu-id="79eb6-164">Create the pigudf.py file</span></span>

<span data-ttu-id="79eb6-165">Na vašem vývojovém prostředí, vytvořte textový soubor s názvem `pigudf.py`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-165">On your development environment, create a text file named `pigudf.py`.</span></span> <span data-ttu-id="79eb6-166">Použijte následující kód jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="79eb6-166">Use the following code as the contents of the file:</span></span>

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

<span data-ttu-id="79eb6-167">V příkladu Pig Latin definovaného `LINE` vstupní jako chararray, protože neexistuje žádný konzistentní schéma pro vstup.</span><span class="sxs-lookup"><span data-stu-id="79eb6-167">In the Pig Latin example, we defined the `LINE` input as a chararray because there is no consistent schema for the input.</span></span> <span data-ttu-id="79eb6-168">Skript Pythonu transformuje dat na konzistentní schéma pro výstup.</span><span class="sxs-lookup"><span data-stu-id="79eb6-168">The Python script transforms the data into a consistent schema for output.</span></span>

1. <span data-ttu-id="79eb6-169">`@outputSchema` Příkaz definuje formát data, která se vrátí do Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-169">The `@outputSchema` statement defines the format of the data that is returned to Pig.</span></span> <span data-ttu-id="79eb6-170">V takovém případě má **datový kontejner**, což je datový typ Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-170">In this case, it's a **data bag**, which is a Pig data type.</span></span> <span data-ttu-id="79eb6-171">Kontejneru objektů a dat obsahuje následující pole, které jsou chararray (řetězce):</span><span class="sxs-lookup"><span data-stu-id="79eb6-171">The bag contains the following fields, all of which are chararray (strings):</span></span>

   * <span data-ttu-id="79eb6-172">Datum - datum vytvoření položky protokolu</span><span class="sxs-lookup"><span data-stu-id="79eb6-172">date - the date the log entry was created</span></span>
   * <span data-ttu-id="79eb6-173">Time – čas vytvoření položky protokolu</span><span class="sxs-lookup"><span data-stu-id="79eb6-173">time - the time the log entry was created</span></span>
   * <span data-ttu-id="79eb6-174">Název třídy - název třídy položka byla vytvořena pro</span><span class="sxs-lookup"><span data-stu-id="79eb6-174">classname - the class name the entry was created for</span></span>
   * <span data-ttu-id="79eb6-175">úroveň – úroveň protokolu</span><span class="sxs-lookup"><span data-stu-id="79eb6-175">level - the log level</span></span>
   * <span data-ttu-id="79eb6-176">Podrobnosti o - verbose podrobnosti záznam protokolu</span><span class="sxs-lookup"><span data-stu-id="79eb6-176">detail - verbose details for the log entry</span></span>

2. <span data-ttu-id="79eb6-177">Dále `def create_structure(input)` definuje funkci, která Pig předá řádku položek.</span><span class="sxs-lookup"><span data-stu-id="79eb6-177">Next, the `def create_structure(input)` defines the function that Pig passes line items to.</span></span>

3. <span data-ttu-id="79eb6-178">Příklad dat, `sample.log`, většinou vyhovuje datum, čas, classname úrovni a podrobností schématu chceme vrátit.</span><span class="sxs-lookup"><span data-stu-id="79eb6-178">The example data, `sample.log`, mostly conforms to the date, time, classname, level, and detail schema we want to return.</span></span> <span data-ttu-id="79eb6-179">Obsahuje však pár řádků, které začínají `*java.lang.Exception*`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-179">However, it contains a few lines that begin with `*java.lang.Exception*`.</span></span> <span data-ttu-id="79eb6-180">Tyto řádky musí být upraven tak, aby odpovídala schématu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-180">These lines must be modified to match the schema.</span></span> <span data-ttu-id="79eb6-181">`if` Příkaz kontroluje pro ty pak massages vstupní data přesunout `*java.lang.Exception*` řetězec za účelem uvedení data v řádku pomocí našich očekávaný výstup schématu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-181">The `if` statement checks for those, then massages the input data to move the `*java.lang.Exception*` string to the end, bringing the data in-line with our expected output schema.</span></span>

4. <span data-ttu-id="79eb6-182">Dále `split` příkaz slouží k rozdělení dat na první čtyři znaky.</span><span class="sxs-lookup"><span data-stu-id="79eb6-182">Next, the `split` command is used to split the data at the first four space characters.</span></span> <span data-ttu-id="79eb6-183">Výstup je přiřazen do `date`, `time`, `classname`, `level`, a `detail`.</span><span class="sxs-lookup"><span data-stu-id="79eb6-183">The output is assigned into `date`, `time`, `classname`, `level`, and `detail`.</span></span>

5. <span data-ttu-id="79eb6-184">Nakonec hodnoty jsou vráceny do Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-184">Finally, the values are returned to Pig.</span></span>

<span data-ttu-id="79eb6-185">Pokud data se vrátí k Pig, má konzistentní schéma definované v `@outputSchema` příkaz.</span><span class="sxs-lookup"><span data-stu-id="79eb6-185">When the data is returned to Pig, it has a consistent schema as defined in the `@outputSchema` statement.</span></span>

## <span data-ttu-id="79eb6-186"><a name="running"></a>Nahrání a spuštění příkladů</span><span class="sxs-lookup"><span data-stu-id="79eb6-186"><a name="running"></a>Upload and run the examples</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79eb6-187">**SSH** kroky fungovat jenom s clusterem HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="79eb6-187">The **SSH** steps only work with a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="79eb6-188">**Prostředí PowerShell** kroky fungovat s clusterem Linux nebo HDInsight se systémem Windows, ale vyžaduje klienta Windows.</span><span class="sxs-lookup"><span data-stu-id="79eb6-188">The **PowerShell** steps work with either a Linux or Windows-based HDInsight cluster, but require a Windows client.</span></span>

### <a name="ssh"></a><span data-ttu-id="79eb6-189">SSH</span><span class="sxs-lookup"><span data-stu-id="79eb6-189">SSH</span></span>

<span data-ttu-id="79eb6-190">Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="79eb6-190">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="79eb6-191">Použití `scp` zkopírovat soubory ke svému clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79eb6-191">Use `scp` to copy the files to your HDInsight cluster.</span></span> <span data-ttu-id="79eb6-192">Například následující příkaz zkopíruje soubory do clusteru s názvem **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="79eb6-192">For example, the following command copies the files to a cluster named **mycluster**.</span></span>

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. <span data-ttu-id="79eb6-193">Připojte se ke clusteru pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="79eb6-193">Use SSH to connect to the cluster.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="79eb6-194">Z relace SSH přidejte soubory python dříve nahrán do úložiště WASB pro cluster.</span><span class="sxs-lookup"><span data-stu-id="79eb6-194">From the SSH session, add the python files uploaded previously to the WASB storage for the cluster.</span></span>

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

<span data-ttu-id="79eb6-195">Mezi nahráním souborů, použijte následující postup ke spuštění úloh Hive a Pig.</span><span class="sxs-lookup"><span data-stu-id="79eb6-195">After uploading the files, use the following steps to run the Hive and Pig jobs.</span></span>

#### <a name="use-the-hive-udf"></a><span data-ttu-id="79eb6-196">Použijte Hive UDF</span><span class="sxs-lookup"><span data-stu-id="79eb6-196">Use the Hive UDF</span></span>

1. <span data-ttu-id="79eb6-197">Použití `hive` příkaz ke spuštění prostředí hive.</span><span class="sxs-lookup"><span data-stu-id="79eb6-197">Use the `hive` command to start the hive shell.</span></span> <span data-ttu-id="79eb6-198">Měli byste vidět `hive>` výzvu po načetl prostředí.</span><span class="sxs-lookup"><span data-stu-id="79eb6-198">You should see a `hive>` prompt once the shell has loaded.</span></span>

2. <span data-ttu-id="79eb6-199">Zadejte následující dotaz na `hive>` řádku:</span><span class="sxs-lookup"><span data-stu-id="79eb6-199">Enter the following query at the `hive>` prompt:</span></span>

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. <span data-ttu-id="79eb6-200">Po zadání poslední řádek, by měla spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-200">After entering the last line, the job should start.</span></span> <span data-ttu-id="79eb6-201">Po dokončení úlohy vrátí výstup podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="79eb6-201">Once the job completes, it returns output similar to the following example:</span></span>

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-the-pig-udf"></a><span data-ttu-id="79eb6-202">Použijte Pig UDF</span><span class="sxs-lookup"><span data-stu-id="79eb6-202">Use the Pig UDF</span></span>

1. <span data-ttu-id="79eb6-203">Použití `pig` příkaz ke spuštění prostředí.</span><span class="sxs-lookup"><span data-stu-id="79eb6-203">Use the `pig` command to start the shell.</span></span> <span data-ttu-id="79eb6-204">Zobrazí `grunt>` výzvu po načetl prostředí.</span><span class="sxs-lookup"><span data-stu-id="79eb6-204">You see a `grunt>` prompt once the shell has loaded.</span></span>

2. <span data-ttu-id="79eb6-205">Zadejte následující příkazy v `grunt>` řádku:</span><span class="sxs-lookup"><span data-stu-id="79eb6-205">Enter the following statements at the `grunt>` prompt:</span></span>

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. <span data-ttu-id="79eb6-206">Po zadání následujícího řádku, by měla spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-206">After entering the following line, the job should start.</span></span> <span data-ttu-id="79eb6-207">Po dokončení úlohy vrátí výstup podobná následující data:</span><span class="sxs-lookup"><span data-stu-id="79eb6-207">Once the job completes, it returns output similar to the following data:</span></span>

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. <span data-ttu-id="79eb6-208">Použít `quit` opusťte prostředí Grunt, a potom pomocí následující upravit soubor pigudf.py v místním systému souborů:</span><span class="sxs-lookup"><span data-stu-id="79eb6-208">Use `quit` to exit the Grunt shell, and then use the following to edit the pigudf.py file on the local file system:</span></span>

    ```bash
    nano pigudf.py
    ```

5. <span data-ttu-id="79eb6-209">Jednou v editoru, zrušte komentář u následujícího řádku odebráním `#` znak od začátku řádku:</span><span class="sxs-lookup"><span data-stu-id="79eb6-209">Once in the editor, uncomment the following line by removing the `#` character from the beginning of the line:</span></span>

    ```bash
    #from pig_util import outputSchema
    ```

    <span data-ttu-id="79eb6-210">Jakmile má byly provedeny změny, ukončete editor pomocí kombinace kláves Ctrl + X.</span><span class="sxs-lookup"><span data-stu-id="79eb6-210">Once the change has been made, use Ctrl+X to exit the editor.</span></span> <span data-ttu-id="79eb6-211">Vyberte Y a potom zadejte a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="79eb6-211">Select Y, and then enter to save the changes.</span></span>

6. <span data-ttu-id="79eb6-212">Použití `pig` příkaz ke spuštění prostředí znovu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-212">Use the `pig` command to start the shell again.</span></span> <span data-ttu-id="79eb6-213">Jakmile jste na `grunt>` řádku, použijte tento příkaz pro spuštění skriptu Python pomocí překladač Pythonu C.</span><span class="sxs-lookup"><span data-stu-id="79eb6-213">Once you are at the `grunt>` prompt, use the following to run the Python script using the C Python interpreter.</span></span>

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    <span data-ttu-id="79eb6-214">Po dokončení této úlohy, měli byste vidět stejný výstup jako když byl již spuštěn skript pomocí Jython.</span><span class="sxs-lookup"><span data-stu-id="79eb6-214">Once this job completes, you should see the same output as when you previously ran the script using Jython.</span></span>

### <a name="powershell-upload-the-files"></a><span data-ttu-id="79eb6-215">Prostředí PowerShell: Odesílat soubory</span><span class="sxs-lookup"><span data-stu-id="79eb6-215">PowerShell: Upload the files</span></span>

<span data-ttu-id="79eb6-216">Prostředí PowerShell můžete použít k nahrání souborů na HDInsight server.</span><span class="sxs-lookup"><span data-stu-id="79eb6-216">You can use PowerShell to upload the files to the HDInsight server.</span></span> <span data-ttu-id="79eb6-217">K odeslání souborů Python použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="79eb6-217">Use the following script to upload the Python files:</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="79eb6-218">Postup v této části použijte prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="79eb6-218">The steps in this section use Azure PowerShell.</span></span> <span data-ttu-id="79eb6-219">Další informace o použití prostředí Azure PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="79eb6-219">For more information on using Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
# Change the path to match the file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage content and upload the file
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
> <span data-ttu-id="79eb6-220">Změna `C:\path\to` hodnotu do cesty k souborům na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="79eb6-220">Change the `C:\path\to` value to the path to the files on your development environment.</span></span>

<span data-ttu-id="79eb6-221">Tento skript načte informace pro váš cluster HDInsight a extrahuje účtu a klíč pro výchozí účet úložiště a odesílá soubory do kořenového adresáře kontejneru.</span><span class="sxs-lookup"><span data-stu-id="79eb6-221">This script retrieves information for your HDInsight cluster, then extracts the account and key for the default storage account, and uploads the files to the root of the container.</span></span>

> [!NOTE]
> <span data-ttu-id="79eb6-222">Další informace o nahrávání souborů najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="79eb6-222">For more information on uploading files, see the [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md) document.</span></span>

#### <a name="powershell-use-the-hive-udf"></a><span data-ttu-id="79eb6-223">Prostředí PowerShell: Podregistr UDF pomocí</span><span class="sxs-lookup"><span data-stu-id="79eb6-223">PowerShell: Use the Hive UDF</span></span>

<span data-ttu-id="79eb6-224">Prostředí PowerShell lze také vzdáleně spouštět dotazy Hive.</span><span class="sxs-lookup"><span data-stu-id="79eb6-224">PowerShell can also be used to remotely run Hive queries.</span></span> <span data-ttu-id="79eb6-225">Pomocí následujícího skriptu prostředí PowerShell pro spouštění dotazů Hive, který používá **hiveudf.py** skriptu:</span><span class="sxs-lookup"><span data-stu-id="79eb6-225">Use the following PowerShell script to run a Hive query that uses **hiveudf.py** script:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79eb6-226">Dřív, než spustíte, skript zobrazí výzvu pro protokol HTTPs nebo správu informací o účtu pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79eb6-226">Before running, the script prompts you for the HTTPs/Admin account information for your HDInsight cluster.</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

# If using a Windows-based HDInsight cluster, change the USING statement to:
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
Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment the following to see stderr output
# Get-AzureRmHDInsightJobOutput `
#   -Clustername $clusterName `
#   -JobId $job.JobId `
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="79eb6-227">Výstup **Hive** úlohy by měl vypadat přibližně v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="79eb6-227">The output for the **Hive** job should appear similar to the following example:</span></span>

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a><span data-ttu-id="79eb6-228">Pig (Jython)</span><span class="sxs-lookup"><span data-stu-id="79eb6-228">Pig (Jython)</span></span>

<span data-ttu-id="79eb6-229">Prostředí PowerShell můžete použít také ke spuštění úloh Pig Latin.</span><span class="sxs-lookup"><span data-stu-id="79eb6-229">PowerShell can also be used to run Pig Latin jobs.</span></span> <span data-ttu-id="79eb6-230">Spustit úlohu Pig Latin, který používá **pigudf.py** skriptu, použijte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="79eb6-230">To run a Pig Latin job that uses the **pigudf.py** script, use the following PowerShell script:</span></span>

> [!NOTE]
> <span data-ttu-id="79eb6-231">Při odesílání vzdáleně úlohu pomocí prostředí PowerShell, není možné použít C Python jako překladač.</span><span class="sxs-lookup"><span data-stu-id="79eb6-231">When remotely submitting a job using PowerShell, it is not possible to use C Python as the interpreter.</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

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

Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment the following to see stderr output
# Get-AzureRmHDInsightJobOutput `
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="79eb6-232">Výstup **Pig** úlohy by měl vypadat přibližně následující data:</span><span class="sxs-lookup"><span data-stu-id="79eb6-232">The output for the **Pig** job should appear similar to the following data:</span></span>

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <span data-ttu-id="79eb6-233"><a name="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="79eb6-233"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="errors-when-running-jobs"></a><span data-ttu-id="79eb6-234">Chyby při spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="79eb6-234">Errors when running jobs</span></span>

<span data-ttu-id="79eb6-235">Při spuštění úlohy hive, můžete se setkat chyba podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="79eb6-235">When running the hive job, you may encounter an error similar to the following text:</span></span>

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

<span data-ttu-id="79eb6-236">Tento problém může být způsobeno konce řádků v souboru Python.</span><span class="sxs-lookup"><span data-stu-id="79eb6-236">This problem may be caused by the line endings in the Python file.</span></span> <span data-ttu-id="79eb6-237">Mnoho Windows editory výchozí nastavení Line FEED ukončování řádků, ale aplikace Linux obvykle očekávat LF.</span><span class="sxs-lookup"><span data-stu-id="79eb6-237">Many Windows editors default to using CRLF as the line ending, but Linux applications usually expect LF.</span></span>

<span data-ttu-id="79eb6-238">Chcete-li odebrat znaky CR před nahráním souboru do HDInsight můžete použít následující příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="79eb6-238">You can use the following PowerShell statements to remove the CR characters before uploading the file to HDInsight:</span></span>

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a><span data-ttu-id="79eb6-239">Skripty prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="79eb6-239">PowerShell scripts</span></span>

<span data-ttu-id="79eb6-240">Obě skriptů prostředí PowerShell příklad, který se používá ke spouštění příklady obsahují komentáři řádek, který zobrazí chyba výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="79eb6-240">Both of the example PowerShell scripts used to run the examples contain a commented line that displays error output for the job.</span></span> <span data-ttu-id="79eb6-241">Pokud nevidíte očekávaný výstup úlohy, zrušte komentář u následujícího řádku a zobrazit, pokud informace o chybě indikuje problém.</span><span class="sxs-lookup"><span data-stu-id="79eb6-241">If you are not seeing the expected output for the job, uncomment the following line and see if the error information indicates a problem.</span></span>

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="79eb6-242">Informace o chybě (STDERR) a výsledek úlohy (STDOUT) jsou taky zaznamenává do úložiště HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79eb6-242">The error information (STDERR) and the result of the job (STDOUT) are also logged to your HDInsight storage.</span></span>

| <span data-ttu-id="79eb6-243">Pro tuto úlohu...</span><span class="sxs-lookup"><span data-stu-id="79eb6-243">For this job...</span></span> | <span data-ttu-id="79eb6-244">Podívejte se na tyto soubory v kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="79eb6-244">Look at these files in the blob container</span></span> |
| --- | --- |
| <span data-ttu-id="79eb6-245">Hive</span><span class="sxs-lookup"><span data-stu-id="79eb6-245">Hive</span></span> |<span data-ttu-id="79eb6-246">/ HivePython/stderr</span><span class="sxs-lookup"><span data-stu-id="79eb6-246">/HivePython/stderr</span></span><p><span data-ttu-id="79eb6-247">/ HivePython/stdout</span><span class="sxs-lookup"><span data-stu-id="79eb6-247">/HivePython/stdout</span></span> |
| <span data-ttu-id="79eb6-248">Pig</span><span class="sxs-lookup"><span data-stu-id="79eb6-248">Pig</span></span> |<span data-ttu-id="79eb6-249">/ PigPython/stderr</span><span class="sxs-lookup"><span data-stu-id="79eb6-249">/PigPython/stderr</span></span><p><span data-ttu-id="79eb6-250">/ PigPython/stdout</span><span class="sxs-lookup"><span data-stu-id="79eb6-250">/PigPython/stdout</span></span> |

## <span data-ttu-id="79eb6-251"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="79eb6-251"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="79eb6-252">Pokud budete potřebovat načíst moduly jazyka Python, které nejsou k dispozici ve výchozím nastavení, najdete v části [nasazení modul Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span><span class="sxs-lookup"><span data-stu-id="79eb6-252">If you need to load Python modules that aren't provided by default, see [How to deploy a module to Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span></span>

<span data-ttu-id="79eb6-253">Další způsoby použití vepřových, Hive a další informace o použití prostředí MapReduce, najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="79eb6-253">For other ways to use Pig, Hive, and to learn about using MapReduce, see the following documents:</span></span>

* [<span data-ttu-id="79eb6-254">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="79eb6-254">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="79eb6-255">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="79eb6-255">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="79eb6-256">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="79eb6-256">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
