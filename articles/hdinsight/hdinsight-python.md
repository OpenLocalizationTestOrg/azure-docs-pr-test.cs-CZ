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
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Funkce (UDF) s Hive a Pig definované uživatelem Python použití v HDInsight

Zjistěte, jak toouse Python uživatelem definované funkce (UDF) s Apache Hive a Pig v Hadoop v Azure HDInsight.

## <a name="python"></a>Python v HDInsight

Python2.7 je nainstalována ve výchozím nastavení na HDInsight 3.0 a novější. Apache Hive lze použít s touto verzí jazyka Python pro zpracování datového proudu. Zpracování datového proudu STDOUT a stdin – data toopass mezi Hive a hello UDF používá.

HDInsight zahrnuje taky Jython, což je implementace Python napsanou v jazyce Java. Jython běží přímo na hello virtuální počítač Java a nepoužívá streamování. Jython je hello doporučuje překladač Pythonu při použití Python s Pig.

> [!WARNING]
> Hello kroky v tomto dokumentu provést hello následující předpoklady: 
>
> * Můžete vytvořit hello skriptů Python ve svém místním vývojovém prostředí.
> * Nahrát hello skripty tooHDInsight pomocí buď hello `scp` příkaz z relace místní Bash nebo hello zadaný skript prostředí PowerShell.
>
> Pokud chcete, aby toouse hello [prostředí cloudu Azure (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) náhled toowork s HDInsight, pak musíte:
>
> * Vytváření hello skriptů v prostředí shell hello cloudu.
> * Použití `scp` tooupload hello soubory z hello cloudu tooHDInsight prostředí.
> * Použití `ssh` z hello cloudové prostředí tooconnect tooHDInsight a příklady spuštění hello.

## <a name="hivepython"></a>Hive UDF

Python lze použít jako UDF z Hive prostřednictvím hello HiveQL `TRANSFORM` příkaz. Například následující HiveQL hello vyvolá hello `hiveudf.py` soubor uložený v účtu Azure Storage výchozí hello hello clusteru.

**HDInsight se systémem Linux**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**HDInsight se systémem Windows**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> Na clusterech HDInsight se systémem Windows hello `USING` klauzule musíte zadat úplnou cestu toopython.exe hello.

Zde je, jaké jsou v tomto příkladu:

1. Hello `add file` příkaz od začátku hello hello souboru přidá hello `hiveudf.py` souboru toohello distribuované mezipaměti, tak, aby byl přístupný pro všechny uzly v clusteru hello.
2. Hello `SELECT TRANSFORM ... USING` příkaz vybere data z hello `hivesampletable`. Také předá hello clientid, devicemake a devicemodel hodnoty toohello `hiveudf.py` skriptu.
3. Hello `AS` klauzule popisuje hello pole vrácená z `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-hello-hiveudfpy-file"></a>Vytvoření souboru hiveudf.py hello


Na vašem vývojovém prostředí, vytvořte textový soubor s názvem `hiveudf.py`. Použijte následující kód jako obsah hello hello souboru hello:

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

Tento skript provede hello následující akce:

1. Čtení řádek dat z STDIN.
2. Hello koncové znak nového řádku je odebrat pomocí `string.strip(line, "\n ")`.
3. Při provádění zpracování datového proudu, jeden řádek obsahuje všechny hodnoty hello s tabulátorem mezi každé hodnotě. Proto `string.split(line, "\t")` lze použít toosplit hello vstup na každé kartě vrácení právě hello pole.
4. Po dokončení zpracování výstup hello musí být napsané tooSTDOUT jako jeden řádek, na kartě mezi každé pole. Například, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. Hello `while` smyčky opakuje, dokud ne `line` je pro čtení.

výstup skriptu Hello je tvořen hello vstupní hodnoty pro `devicemake` a `devicemodel`, a hodnota hash hello zřetězených hodnotu.

V tématu [spuštění příkladů hello](#running) jak toorun v tomto příkladu v clusteru HDInsight.

## <a name="pigpython"></a>Pig UDF

Skript v jazyce Python lze použít jako UDF z Pig prostřednictvím hello `GENERATE` příkaz. Můžete spustit skript hello pomocí Jython nebo C Python.

* Jython běží na hello JVM a volat nativně z Pig.
* C Python je externí proces, takže hello data z Pig na hello odeslání JVM toohello skriptu, který běží v procesu Python. výstup Hello hello skript v jazyce Python je odeslána zpět do Pig.

překladač Pythonu toospecify hello, použijte `register` při odkazování na skript v jazyce Python hello. Hello následující příklady zaregistrovat skriptů Pig jako `myfuncs`:

* **toouse Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`
* **toouse C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Při použití Jython, hello cestě toohello pig_jython souboru může být místní cesta nebo WASB: / / cesta. Při použití C Python, ale musí odkazovat souboru na hello místního systému souborů, že používáte úlohu Pig hello toosubmit uzlu hello.

Jakmile po registraci hello Pig Latin pro tento příklad hello stejný pro obě:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Zde je, jaké jsou v tomto příkladu:

1. první řádek Hello načte hello ukázkový datový soubor `sample.log` do `LOGS`. Definuje také každý záznam jako `chararray`.
2. Další řádek Hello odfiltruje všechny hodnoty null ukládání hello výsledek operace hello do `LOG`.
3. V dalším kroku ji iteruje nad hello záznamy v `LOG` a používá `GENERATE` tooinvoke hello `create_structure` metoda obsažených ve skriptu Python nebo Jython hello načíst jako `myfuncs`. `LINE`je použité toopass hello aktuální záznamů toohello funkce.
4. Nakonec hello výstupy jsou dumpingových tooSTDOUT pomocí hello `DUMP` příkaz. Tento příkaz zobrazí výsledky hello po dokončení operace hello.

### <a name="create-hello-pigudfpy-file"></a>Vytvoření souboru pigudf.py hello

Na vašem vývojovém prostředí, vytvořte textový soubor s názvem `pigudf.py`. Použijte následující kód jako obsah hello hello souboru hello:

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

V příkladu Pig Latin hello jsme definovali hello `LINE` vstupní jako chararray, protože neexistuje žádný konzistentní schéma pro vstup hello. Hello skript v jazyce Python transformuje hello dat na konzistentní schéma pro výstup.

1. Hello `@outputSchema` příkaz definuje formát hello hello dat, která je vrácena tooPig. V takovém případě má **datový kontejner**, což je datový typ Pig. kontejner objektů a dat Hello obsahuje hello následující pole, které jsou chararray (řetězce):

   * Date – hello datum hello vytvoření položky protokolu
   * Time – hello čas vytvoření položky protokolu hello
   * Název třídy - položku hello název třídy hello vytvořený pro
   * úroveň – úroveň protokolu hello
   * Podrobnosti o - verbose podrobnosti hello položka protokolu

2. V dalším kroku hello `def create_structure(input)` definuje hello funkce, která Pig předá řádku položek.

3. Hello příklad dat, `sample.log`, většinou vyhovuje toohello datum, čas, classname úrovni a podrobností chceme tooreturn schématu. Obsahuje však pár řádků, které začínají `*java.lang.Exception*`. Tyto řádky musí být upravené toomatch hello schématu. Hello `if` příkaz kontroluje pro ty, a potom středisek hello vstupní data toomove hello `*java.lang.Exception*` end toohello řetězec, přináší hello data v řádku pomocí našich očekávaný výstup schématu.

4. V dalším kroku hello `split` příkaz je použité toosplit hello dat hello první čtyři znaky. výstup Hello je přiřazen do `date`, `time`, `classname`, `level`, a `detail`.

5. Nakonec hello hodnoty jsou vráceny tooPig.

Když jsou hello data vrácena tooPig, má konzistentní schéma definované v hello `@outputSchema` příkaz.

## <a name="running"></a>Nahrání a spuštění příkladů hello

> [!IMPORTANT]
> Hello **SSH** kroky fungovat jenom s clusterem HDInsight se systémem Linux. Hello **prostředí PowerShell** kroky fungovat s clusterem Linux nebo HDInsight se systémem Windows, ale vyžaduje klienta Windows.

### <a name="ssh"></a>SSH

Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

1. Použití `scp` toocopy hello soubory tooyour HDInsight clusteru. Například hello následující příkaz kopie hello soubory tooa clusteru s názvem **mycluster**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Použití SSH tooconnect toohello clusteru.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. Z relace SSH hello přidáte, že soubory python hello načtený dříve toohello WASB úložiště pro hello cluster.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Po odeslání hello souborů, použijte hello tyto kroky toorun hello Hive a Pig úlohy.

#### <a name="use-hello-hive-udf"></a>Použití hello Hive UDF

1. Použití hello `hive` příkazové prostředí hive toostart hello. Měli byste vidět `hive>` výzvu po načetl hello prostředí.

2. Zadejte následující dotaz u hello hello `hive>` řádku:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Po zadání hello poslední řádek, začněte hello úlohy. Po dokončení úlohy hello vrátí výstup podobný toohello následující ukázka:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-hello-pig-udf"></a>Použití hello Pig UDF

1. Použití hello `pig` příkazové prostředí toostart hello. Zobrazí `grunt>` výzvu po načetl hello prostředí.

2. Zadejte následující příkazy v hello hello `grunt>` řádku:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Po zadání hello následující řádek, začněte hello úlohy. Po dokončení úlohy hello vrátí výstup podobný toohello následující data:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Použít `quit` tooexit hello Grunt prostředí a pak použijte hello následující tooedit hello pigudf.py soubor na hello místního systému souborů:

    ```bash
    nano pigudf.py
    ```

5. Jednou v editoru hello, zrušte komentář u následující řádek odebráním hello hello `#` znak z hello začátku řádku hello:

    ```bash
    #from pig_util import outputSchema
    ```

    Jakmile hello změny, použijte editor hello tooexit Ctrl + X. Vyberte Y a potom zadejte toosave hello změny.

6. Použití hello `pig` příkazové prostředí hello toostart znovu. Jakmile jste na hello `grunt>` řádku, použijte následující skript v jazyce Python toorun hello pomocí překladač Pythonu C hello hello.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Po dokončení této úlohy, měli byste vidět hello stejnou výstupní jako když byl již spuštěn skript hello pomocí Jython.

### <a name="powershell-upload-hello-files"></a>Prostředí PowerShell: Soubory hello nahrávání

Můžete použít PowerShell tooupload hello soubory toohello serveru HDInsight. Použijte následující skript tooupload hello Python soubory hello:

> [!IMPORTANT] 
> Hello kroky v této části použijte prostředí Azure PowerShell. Další informace o použití prostředí Azure PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

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
> Změna hello `C:\path\to` hodnotu toohello cesta toohello soubory na vašem vývojovém prostředí.

Tento skript načte informace pro váš cluster HDInsight a pak extrahuje hello účtu a klíč pro hello výchozí účet úložiště a nahrávání hello soubory toohello kořenovém kontejneru hello.

> [!NOTE]
> Další informace o nahrávání souborů najdete v tématu hello [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md) dokumentu.

#### <a name="powershell-use-hello-hive-udf"></a>Prostředí PowerShell: Hello Hive UDF pomocí

Prostředí PowerShell může být také použít tooremotely spouštět dotazy Hive. Použití hello následující toorun skript prostředí PowerShell dotaz Hive, který používá **hiveudf.py** skriptu:

> [!IMPORTANT]
> Dřív, než spustíte, skript hello vás vyzve k zadání hello HTTPs nebo správu informací o účtu pro váš cluster HDInsight.

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

Hello výstup hello **Hive** úlohy by se měla zobrazit podobné toohello následující ukázka:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

Prostředí PowerShell může být také použít toorun Pig Latin úlohy. toorun Pig Latin úlohu, která používá hello **pigudf.py** skriptu, použijte hello následující skript prostředí PowerShell:

> [!NOTE]
> Při odesílání vzdáleně úlohu pomocí prostředí PowerShell, není možné toouse C Python jako hello překladač.

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

Hello výstup hello **Pig** úlohy by se měla zobrazit podobné toohello následující data:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Řešení potíží

### <a name="errors-when-running-jobs"></a>Chyby při spuštění úlohy

Při spuštění úlohy hive hello, může dojít k chybě podobné toohello následující text:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing tooyour custom script. It may have crashed with an error.

Tento problém může být způsobeno hello konců řádků v souboru Python hello. Mnohé editory Windows výchozí toousing Line FEED jako hello řádku ukončení, ale aplikace Linux obvykle předpokládají LF.

Můžete použít následující znaky hello CR tooremove příkazy prostředí PowerShell před nahráním souboru tooHDInsight hello hello:

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a>Skripty prostředí PowerShell

Obě hello ukázkové skripty prostředí PowerShell použít toorun hello příklady obsahují komentáři řádek, který zobrazí chyba výstup hello úlohy. Pokud nevidíte hello očekávaný výstup hello úlohy, zrušte komentář u následující hello řádku a zobrazit, pokud informace o chybě hello indikuje problém.

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

informace o chybě Hello (STDERR) a výsledek hello hello úlohy (STDOUT) jsou také zaznamenané tooyour HDInsight úložiště.

| Pro tuto úlohu... | Podívejte se na tyto soubory v kontejneru objektů blob hello |
| --- | --- |
| Hive |/ HivePython/stderr<p>/ HivePython/stdout |
| Pig |/ PigPython/stderr<p>/ PigPython/stdout |

## <a name="next"></a>Další kroky

Pokud potřebujete tooload moduly jazyka Python, které nejsou k dispozici ve výchozím nastavení, najdete v části [jak toodeploy modulu tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Další způsoby toouse Pig, Hive a toolearn o použití prostředí MapReduce najdete v části hello následující dokumenty:

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)
