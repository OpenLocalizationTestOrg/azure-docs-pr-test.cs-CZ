---
title: aaaApache Storm zapsat tooStorage nebo Data Lake Store - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse hello Apache Storm toowrite toohello HDFS kompatibilní úložiště pro HDInsight. Azure Storage nebo Azure Data Lake Store zadejte hello HDFS comptabile úložiště pro HDInsight. Tento dokument a přidružené příkladu hello ukazují, jak součást HdfsBolt hello lze použít toowrite toohello výchozí úložiště cluster Storm v HDInsight."
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: d76159a9ecd1be18e519511cfdb3bcfd18ae4d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="write-toohdfs-from-apache-storm-on-hdinsight"></a><span data-ttu-id="a8a3c-105">Zápis tooHDFS z Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a8a3c-105">Write tooHDFS from Apache Storm on HDInsight</span></span>

<span data-ttu-id="a8a3c-106">Zjistěte, jak toouse Storm toowrite data toohello HDFS kompatibilní úložiště používané Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-106">Learn how toouse Storm toowrite data toohello HDFS-compatible storage used by Apache Storm on HDInsight.</span></span> <span data-ttu-id="a8a3c-107">HDInsight můžete je používat jako úložiště HDFS comptabile úložiště Azure Storage a Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-107">HDInsight can use both Azure Storage and Azure Data Lake store as HDFS-comptabile storage.</span></span> <span data-ttu-id="a8a3c-108">Storm poskytuje [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) komponenta, která zapisuje data tooHDFS.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-108">Storm provides an [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) component that writes data tooHDFS.</span></span> <span data-ttu-id="a8a3c-109">Tento dokument obsahuje informace o zápis z hello HdfsBolt tooeither typu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-109">This document provides information on writing tooeither type of storage from hello HdfsBolt.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="a8a3c-110">Příklad Hello topologie použitý v tomto dokumentu využívá součásti, které jsou součástí Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-110">hello example topology used in this document relies on components that are included with Storm on HDInsight.</span></span> <span data-ttu-id="a8a3c-111">Změna toowork s Azure Data Lake Store při použití s další clustery Apache Storm může požadovat.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-111">It may require modification toowork with Azure Data Lake Store when used with other Apache Storm clusters.</span></span>

## <a name="get-hello-code"></a><span data-ttu-id="a8a3c-112">Získat kód hello</span><span class="sxs-lookup"><span data-stu-id="a8a3c-112">Get hello code</span></span>

<span data-ttu-id="a8a3c-113">je k dispozici ke stažení z projektu Hello obsahující tato topologie [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-113">hello project containing this topology is available as a download from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span></span>

<span data-ttu-id="a8a3c-114">toocompile tento projekt, musíte hello následující konfigurace pro vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-114">toocompile this project, you need hello following configuration for your development environment:</span></span>

* <span data-ttu-id="a8a3c-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="a8a3c-116">HDInsight 3.5 nebo vyšší vyžadují Java 8.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-116">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="a8a3c-117">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="a8a3c-117">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

<span data-ttu-id="a8a3c-118">Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-118">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="a8a3c-119">Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-119">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="a8a3c-120">`JAVA_HOME`-by měla odkazovat toohello adresář, kam hello JDK je nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-120">`JAVA_HOME` - should point toohello directory where hello JDK is installed.</span></span>
* <span data-ttu-id="a8a3c-121">`PATH`-musí obsahovat hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-121">`PATH` - should contain hello following paths:</span></span>
  
    * <span data-ttu-id="a8a3c-122">`JAVA_HOME`(nebo ekvivalentní cesta hello).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-122">`JAVA_HOME` (or hello equivalent path).</span></span>
    * <span data-ttu-id="a8a3c-123">`JAVA_HOME\bin`(nebo ekvivalentní cesta hello).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-123">`JAVA_HOME\bin` (or hello equivalent path).</span></span>
    * <span data-ttu-id="a8a3c-124">Hello adresář, kde je nainstalován Maven.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-124">hello directory where Maven is installed.</span></span>

## <a name="how-toouse-hello-hdfsbolt-with-hdinsight"></a><span data-ttu-id="a8a3c-125">Jak toouse hello HdfsBolt s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a8a3c-125">How toouse hello HdfsBolt with HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8a3c-126">Před použitím hello HdfsBolt se Storm v HDInsight, musíte nejprve použít souborů jar toocopy požadované akce skriptu do hello `extpath` pro Storm.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-126">Before using hello HdfsBolt with Storm on HDInsight, you must first use a script action toocopy required jar files into hello `extpath` for Storm.</span></span> <span data-ttu-id="a8a3c-127">Další informace najdete v tématu hello [konfigurovat hello clusteru](#configure) části.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-127">For more information, see hello [Configure hello cluster](#configure) section.</span></span>

<span data-ttu-id="a8a3c-128">Hello HdfsBolt používá schéma souboru hello jak poskytnout toounderstand toowrite tooHDFS.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-128">hello HdfsBolt uses hello file scheme that you provide toounderstand how toowrite tooHDFS.</span></span> <span data-ttu-id="a8a3c-129">S HDInsight použijte jednu z následujících schémat hello:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-129">With HDInsight, use one of hello following schemes:</span></span>

* <span data-ttu-id="a8a3c-130">`wasb://`: Používá se účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-130">`wasb://`: Used with an Azure Storage account.</span></span>
* <span data-ttu-id="a8a3c-131">`adl://`: Použít s Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-131">`adl://`: Used with Azure Data Lake Store.</span></span>

<span data-ttu-id="a8a3c-132">Hello následující tabulka obsahuje příklady použití hello souboru schématu pro různé scénáře:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-132">hello following table provides examples of using hello file scheme for different scenarios:</span></span>

| <span data-ttu-id="a8a3c-133">Schéma</span><span class="sxs-lookup"><span data-stu-id="a8a3c-133">Scheme</span></span> | <span data-ttu-id="a8a3c-134">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a8a3c-134">Notes</span></span> |
| ----- | ----- |
| `wasb:///` | <span data-ttu-id="a8a3c-135">Hello výchozí účet úložiště je kontejner objektů blob v účtu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a8a3c-135">hello default storage account is a blob container in an Azure Storage account</span></span> |
| `adl:///` | <span data-ttu-id="a8a3c-136">Hello výchozí účet úložiště je adresář v Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-136">hello default storage account is a directory in Azure Data Lake Store.</span></span> <span data-ttu-id="a8a3c-137">Při vytváření clusteru zadejte hello adresář v Data Lake Store, který je hello kořenovém clusteru hello HDFS.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-137">During cluster creation, you specify hello directory in Data Lake Store that is hello root of hello cluster's HDFS.</span></span> <span data-ttu-id="a8a3c-138">Například hello `/clusters/myclustername/` adresáře.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-138">For example, hello `/clusters/myclustername/` directory.</span></span> |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | <span data-ttu-id="a8a3c-139">Účet úložiště Azure (Další) jiné než výchozí přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-139">A non-default (additional) Azure storage account associated with hello cluster.</span></span> |
| `adl://STORENAME/` | <span data-ttu-id="a8a3c-140">kořenový adresář Hello hello používá hello cluster Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-140">hello root of hello Data Lake Store used by hello cluster.</span></span> <span data-ttu-id="a8a3c-141">Toto schéma umožňuje tooaccess data, která se nachází mimo hello adresář, který obsahuje systém souborů clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-141">This scheme allows you tooaccess data that is located outside hello directory that contains hello cluster file system.</span></span> |

<span data-ttu-id="a8a3c-142">Další informace najdete v tématu hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) odkaz na Apache.org.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-142">For more information, see hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) reference at Apache.org.</span></span>

### <a name="example-configuration"></a><span data-ttu-id="a8a3c-143">Příklad konfigurace</span><span class="sxs-lookup"><span data-stu-id="a8a3c-143">Example configuration</span></span>

<span data-ttu-id="a8a3c-144">Hello následující YAML je výňatek ze hello `resources/writetohdfs.yaml` zahrnutý v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-144">hello following YAML is an excerpt from hello `resources/writetohdfs.yaml` file included in hello example.</span></span> <span data-ttu-id="a8a3c-145">Tento soubor definuje topologie Storm hello pomocí hello [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework pro Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-145">This file defines hello Storm topology using hello [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework for Apache Storm.</span></span>

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.NoRotationPolicy"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

<span data-ttu-id="a8a3c-146">Tato YAML definuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-146">This YAML defines hello following items:</span></span>

* <span data-ttu-id="a8a3c-147">`syncPolicy`: Definuje, když jsou soubory synchronizována Vyprázdněné toohello systému souborů.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-147">`syncPolicy`: Defines when files are synched/flushed toohello file system.</span></span> <span data-ttu-id="a8a3c-148">V tomto příkladu každých 1000 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-148">In this example, every 1000 tuples.</span></span>
* <span data-ttu-id="a8a3c-149">`fileNameFormat`: Definuje hello cestu a název vzor toouse při zápisu souborů.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-149">`fileNameFormat`: Defines hello path and file name pattern toouse when writing files.</span></span> <span data-ttu-id="a8a3c-150">V tomto příkladu je poskytnuta cesta hello za běhu pomocí filtru, a přípona souboru hello je `.txt`.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-150">In this example, hello path is provided at runtime using a filter, and hello file extension is `.txt`.</span></span>
* <span data-ttu-id="a8a3c-151">`recordFormat`: Definuje hello interní formát souborů hello zapsána.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-151">`recordFormat`: Defines hello internal format of hello files written.</span></span> <span data-ttu-id="a8a3c-152">V tomto příkladu pole jsou oddělená hello `|` znak.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-152">In this example, fields are delimited by hello `|` character.</span></span>
* <span data-ttu-id="a8a3c-153">`rotationPolicy`: Určuje, kdy toorotate soubory.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-153">`rotationPolicy`: Defines when toorotate files.</span></span> <span data-ttu-id="a8a3c-154">V tomto příkladu se provádí bez otočení.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-154">In this example, no rotation is performed.</span></span>
* <span data-ttu-id="a8a3c-155">`hdfs-bolt`: Používá hello předchozí komponenty jako parametry konfigurace pro hello `HdfsBolt` třídy.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-155">`hdfs-bolt`: Uses hello previous components as configuration parameters for hello `HdfsBolt` class.</span></span>

<span data-ttu-id="a8a3c-156">Další informace o hello tok framework najdete v tématu [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-156">For more information on hello Flux framework, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="configure-hello-cluster"></a><span data-ttu-id="a8a3c-157">Konfigurace clusteru hello</span><span class="sxs-lookup"><span data-stu-id="a8a3c-157">Configure hello cluster</span></span>

<span data-ttu-id="a8a3c-158">Ve výchozím nastavení Storm v HDInsight nezahrnuje hello komponenty, který HdfsBolt používá toocommunicate s Azure Storage nebo Data Lake Store v Storm je cesta pro třídy.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-158">By default, Storm on HDInsight does not include hello components that HdfsBolt uses toocommunicate with Azure Storage or Data Lake Store in Storm's classpath.</span></span> <span data-ttu-id="a8a3c-159">Použití hello následující skript akce tooadd tyto součásti toohello `extlib` adresář pro Storm v clusteru:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-159">Use hello following script action tooadd these components toohello `extlib` directory for Storm on your cluster:</span></span>

<span data-ttu-id="a8a3c-160">| Identifikátor URI skriptu | Uzly tooapply jeho | Parametry || `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, nadřízeného | None |</span><span class="sxs-lookup"><span data-stu-id="a8a3c-160">| Script URI | Nodes tooapply it to| Parameters | | `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, Supervisor | None |</span></span>

<span data-ttu-id="a8a3c-161">Informace o použití tohoto skriptu k vašemu clusteru najdete v tématu hello [HDInsight přizpůsobit clustery pomocí akcí skriptů](./hdinsight-hadoop-customize-cluster-linux.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-161">For information on using this script with your cluster, see hello [Customize HDInsight clusters using script actions](./hdinsight-hadoop-customize-cluster-linux.md) document.</span></span>

## <a name="build-and-package-hello-topology"></a><span data-ttu-id="a8a3c-162">Sestavení a balíček hello topologie</span><span class="sxs-lookup"><span data-stu-id="a8a3c-162">Build and package hello topology</span></span>

1. <span data-ttu-id="a8a3c-163">Stáhnout hello příklad projektu ze [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-163">Download hello example project from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour development environment.</span></span>

2. <span data-ttu-id="a8a3c-164">Z příkazového řádku stáhnou Terminálové nebo relace prostředí, změna adresáře toohello kořenovém hello projektu.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-164">From a command prompt, terminal, or shell session, change directories toohello root of hello downloaded project.</span></span> <span data-ttu-id="a8a3c-165">toobuild a balíček hello topologie, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-165">toobuild and package hello topology, use hello following command:</span></span>
   
        mvn compile package
   
    <span data-ttu-id="a8a3c-166">Po dokončení sestavení hello a balení, je nový adresář s názvem `target`, který obsahuje soubor s názvem `StormToHdfs-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-166">Once hello build and packaging completes, there is a new directory named `target`, that contains a file named `StormToHdfs-1.0-SNAPSHOT.jar`.</span></span> <span data-ttu-id="a8a3c-167">Tento soubor obsahuje topologie hello zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-167">This file contains hello compiled topology.</span></span>

## <a name="deploy-and-run-hello-topology"></a><span data-ttu-id="a8a3c-168">Nasazení a spuštění hello topologie</span><span class="sxs-lookup"><span data-stu-id="a8a3c-168">Deploy and run hello topology</span></span>

1. <span data-ttu-id="a8a3c-169">Použijte následující příkaz toocopy hello topologie toohello clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-169">Use hello following command toocopy hello topology toohello HDInsight cluster.</span></span> <span data-ttu-id="a8a3c-170">Nahraďte **uživatele** s uživatelským jménem SSH hello jste použili při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-170">Replace **USER** with hello SSH user name you used when creating hello cluster.</span></span> <span data-ttu-id="a8a3c-171">Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-171">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    <span data-ttu-id="a8a3c-172">Po zobrazení výzvy zadejte heslo hello použité při vytváření uživatele SSH hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-172">When prompted, enter hello password used when creating hello SSH user for hello cluster.</span></span> <span data-ttu-id="a8a3c-173">Pokud jste použili veřejný klíč místo hesla, může být nutné toouse hello `-i` parametr toospecify hello cesta toohello odpovídající privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-173">If you used a public key instead of a password, you may need toouse hello `-i` parameter toospecify hello path toohello matching private key.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a8a3c-174">Další informace o používání `scp` s HDInsight, najdete v části [použití SSH s HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-174">For more information on using `scp` with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="a8a3c-175">Po dokončení nahrávání hello použijte hello následující tooconnect toohello HDInsight clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-175">Once hello upload completes, use hello following tooconnect toohello HDInsight cluster using SSH.</span></span> <span data-ttu-id="a8a3c-176">Nahraďte **uživatele** s uživatelským jménem SSH hello jste použili při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-176">Replace **USER** with hello SSH user name you used when creating hello cluster.</span></span> <span data-ttu-id="a8a3c-177">Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-177">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="a8a3c-178">Po zobrazení výzvy zadejte heslo hello použité při vytváření uživatele SSH hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-178">When prompted, enter hello password used when creating hello SSH user for hello cluster.</span></span> <span data-ttu-id="a8a3c-179">Pokud jste použili veřejný klíč místo hesla, může být nutné toouse hello `-i` parametr toospecify hello cesta toohello odpovídající privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-179">If you used a public key instead of a password, you may need toouse hello `-i` parameter toospecify hello path toohello matching private key.</span></span>
   
   <span data-ttu-id="a8a3c-180">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-180">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="a8a3c-181">Po připojení, použijte hello následující příkaz toocreate soubor s názvem `dev.properties`:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-181">Once connected, use hello following command toocreate a file named `dev.properties`:</span></span>

        nano dev.properties

4. <span data-ttu-id="a8a3c-182">Použití hello následující text jako hello obsah hello `dev.properties` souboru:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-182">Use hello following text as hello contents of hello `dev.properties` file:</span></span>

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > <span data-ttu-id="a8a3c-183">Tento příklad předpokládá, že váš cluster používá účet úložiště Azure jako hello výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-183">This example assumes that your cluster uses an Azure Storage account as hello default storage.</span></span> <span data-ttu-id="a8a3c-184">Pokud váš cluster používá Azure Data Lake Store, použijte `hdfs.url: adl:///` místo.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-184">If your cluster uses Azure Data Lake Store, use `hdfs.url: adl:///` instead.</span></span>
    
    <span data-ttu-id="a8a3c-185">toosave hello soubor, použijte __kombinaci kláves Ctrl + X__, pak __Y__a v neposlední řadě __Enter__.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-185">toosave hello file, use __Ctrl + X__, then __Y__, and finally __Enter__.</span></span> <span data-ttu-id="a8a3c-186">Hello hodnoty v tomto souboru nastavit adresu URL úložiště Data Lake hello a hello název adresáře, která data se zapisují do.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-186">hello values in this file set hello Data Lake store URL and hello directory name that data is written to.</span></span>

3. <span data-ttu-id="a8a3c-187">Použijte následující příkaz toostart hello topologie hello:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-187">Use hello following command toostart hello topology:</span></span>
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    <span data-ttu-id="a8a3c-188">Tento příkaz spustí hello topologie pomocí hello tok framework odesláním uzel Nimbus toohello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-188">This command starts hello topology using hello Flux framework by submitting it toohello Nimbus node of hello cluster.</span></span> <span data-ttu-id="a8a3c-189">topologie Hello je definována hello `writetohdfs.yaml` zahrnutý v hello jar.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-189">hello topology is defined by hello `writetohdfs.yaml` file included in hello jar.</span></span> <span data-ttu-id="a8a3c-190">Hello `dev.properties` soubor je předán jako filtr a hodnoty hello obsažené v souboru hello se čtou podle topologie hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-190">hello `dev.properties` file is passed as a filter, and hello values contained in hello file are read by hello topology.</span></span>

## <a name="view-output-data"></a><span data-ttu-id="a8a3c-191">Zobrazení výstupní data</span><span class="sxs-lookup"><span data-stu-id="a8a3c-191">View output data</span></span>

<span data-ttu-id="a8a3c-192">tooview hello data, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-192">tooview hello data, use hello following command:</span></span>

    hdfs dfs -ls /stormdata/

<span data-ttu-id="a8a3c-193">Zobrazí se seznam hello soubory vytvořené v této topologii.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-193">A list of hello files created by this topology is displayed.</span></span>

<span data-ttu-id="a8a3c-194">Hello následujícím seznamu je příklad dat hello retuned podle předchozích příkazů hello:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-194">hello following list is an example of hello data retuned by hello previous commands:</span></span>

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-hello-topology"></a><span data-ttu-id="a8a3c-195">Zastavení topologie hello</span><span class="sxs-lookup"><span data-stu-id="a8a3c-195">Stop hello topology</span></span>

<span data-ttu-id="a8a3c-196">Topologie Storm spustit, dokud nebude zastaven nebo odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a8a3c-196">Storm topologies run until stopped, or hello cluster is deleted.</span></span> <span data-ttu-id="a8a3c-197">toostop hello topologie, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8a3c-197">toostop hello topology, use hello following command:</span></span>

    storm kill hdfswriter

## <a name="delete-your-cluster"></a><span data-ttu-id="a8a3c-198">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="a8a3c-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="a8a3c-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8a3c-199">Next steps</span></span>

<span data-ttu-id="a8a3c-200">Teď, když jste se naučili, jak toouse Storm toowrite tooAzure úložiště a Azure Data Lake Store, zjišťovat, jiné [Storm příklady pro HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a8a3c-200">Now that you have learned how toouse Storm toowrite tooAzure Storage and Azure Data Lake Store, discover other [Storm examples for HDInsight](hdinsight-storm-example-topology.md).</span></span>

