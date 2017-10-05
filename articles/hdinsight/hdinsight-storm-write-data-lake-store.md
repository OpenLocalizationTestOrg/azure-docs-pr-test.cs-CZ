---
title: "Apache Storm při zápisu do úložiště nebo Data Lake Store - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Apache Storm k zápisu do HDFS kompatibilní úložiště pro HDInsight. Azure Storage nebo Azure Data Lake Store zadejte HDFS comptabile úložiště pro HDInsight. Tento dokument a související příklad ukazují, jak součást HdfsBolt slouží k zápisu do výchozího úložiště Storm v clusteru HDInsight."
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
ms.openlocfilehash: 10dc8789e8f4a2b27fd3a4c6fec2ab28c674170a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="write-to-hdfs-from-apache-storm-on-hdinsight"></a><span data-ttu-id="b351c-105">Zápis do HDFS z Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b351c-105">Write to HDFS from Apache Storm on HDInsight</span></span>

<span data-ttu-id="b351c-106">Naučte se používat Storm k zápisu dat do HDFS kompatibilní úložiště používané Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b351c-106">Learn how to use Storm to write data to the HDFS-compatible storage used by Apache Storm on HDInsight.</span></span> <span data-ttu-id="b351c-107">HDInsight můžete je používat jako úložiště HDFS comptabile úložiště Azure Storage a Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="b351c-107">HDInsight can use both Azure Storage and Azure Data Lake store as HDFS-comptabile storage.</span></span> <span data-ttu-id="b351c-108">Storm poskytuje [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) komponenty, která zapisuje data do HDFS.</span><span class="sxs-lookup"><span data-stu-id="b351c-108">Storm provides an [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) component that writes data to HDFS.</span></span> <span data-ttu-id="b351c-109">Tento dokument obsahuje informace o zápis z HdfsBolt na některý typ úložiště.</span><span class="sxs-lookup"><span data-stu-id="b351c-109">This document provides information on writing to either type of storage from the HdfsBolt.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b351c-110">Příklad topologii použitou v tomto dokumentu využívá součásti, které jsou součástí Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b351c-110">The example topology used in this document relies on components that are included with Storm on HDInsight.</span></span> <span data-ttu-id="b351c-111">Změny pro práci s Azure Data Lake Store při použití s další clustery Apache Storm může požadovat.</span><span class="sxs-lookup"><span data-stu-id="b351c-111">It may require modification to work with Azure Data Lake Store when used with other Apache Storm clusters.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="b351c-112">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="b351c-112">Get the code</span></span>

<span data-ttu-id="b351c-113">Projekt obsahující tato topologie je k dispozici ke stažení z [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="b351c-113">The project containing this topology is available as a download from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span></span>

<span data-ttu-id="b351c-114">Kompilace projektu, potřebujete následující konfigurace pro vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="b351c-114">To compile this project, you need the following configuration for your development environment:</span></span>

* <span data-ttu-id="b351c-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="b351c-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="b351c-116">HDInsight 3.5 nebo vyšší vyžadují Java 8.</span><span class="sxs-lookup"><span data-stu-id="b351c-116">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="b351c-117">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="b351c-117">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

<span data-ttu-id="b351c-118">Následující proměnné prostředí může být nastaven při instalaci Java a sadu JDK na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="b351c-118">The following environment variables may be set when you install Java and the JDK on your development workstation.</span></span> <span data-ttu-id="b351c-119">Nicméně byste měli zkontrolovat, že existují a že obsahují správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="b351c-119">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="b351c-120">`JAVA_HOME`-by měla odkazovat na adresář, kam nainstalovat sadu JDK.</span><span class="sxs-lookup"><span data-stu-id="b351c-120">`JAVA_HOME` - should point to the directory where the JDK is installed.</span></span>
* <span data-ttu-id="b351c-121">`PATH`-musí obsahovat následující cesty:</span><span class="sxs-lookup"><span data-stu-id="b351c-121">`PATH` - should contain the following paths:</span></span>
  
    * <span data-ttu-id="b351c-122">`JAVA_HOME`(nebo ekvivalentní cesta).</span><span class="sxs-lookup"><span data-stu-id="b351c-122">`JAVA_HOME` (or the equivalent path).</span></span>
    * <span data-ttu-id="b351c-123">`JAVA_HOME\bin`(nebo ekvivalentní cesta).</span><span class="sxs-lookup"><span data-stu-id="b351c-123">`JAVA_HOME\bin` (or the equivalent path).</span></span>
    * <span data-ttu-id="b351c-124">Adresář, kde je nainstalován Maven.</span><span class="sxs-lookup"><span data-stu-id="b351c-124">The directory where Maven is installed.</span></span>

## <a name="how-to-use-the-hdfsbolt-with-hdinsight"></a><span data-ttu-id="b351c-125">Postup použití HdfsBolt s HDInsight</span><span class="sxs-lookup"><span data-stu-id="b351c-125">How to use the HdfsBolt with HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b351c-126">Před použitím HdfsBolt se Storm v HDInsight, musíte nejprve použít akci skriptu pro kopírování souborů vyžaduje jar do `extpath` pro Storm.</span><span class="sxs-lookup"><span data-stu-id="b351c-126">Before using the HdfsBolt with Storm on HDInsight, you must first use a script action to copy required jar files into the `extpath` for Storm.</span></span> <span data-ttu-id="b351c-127">Další informace najdete v tématu [konfigurovat cluster](#configure) části.</span><span class="sxs-lookup"><span data-stu-id="b351c-127">For more information, see the [Configure the cluster](#configure) section.</span></span>

<span data-ttu-id="b351c-128">HdfsBolt používá schéma souboru, které poskytujete pochopit, jak k zápisu do HDFS.</span><span class="sxs-lookup"><span data-stu-id="b351c-128">The HdfsBolt uses the file scheme that you provide to understand how to write to HDFS.</span></span> <span data-ttu-id="b351c-129">S HDInsight použijte jednu z následujících schémat:</span><span class="sxs-lookup"><span data-stu-id="b351c-129">With HDInsight, use one of the following schemes:</span></span>

* <span data-ttu-id="b351c-130">`wasb://`: Používá se účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b351c-130">`wasb://`: Used with an Azure Storage account.</span></span>
* <span data-ttu-id="b351c-131">`adl://`: Použít s Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="b351c-131">`adl://`: Used with Azure Data Lake Store.</span></span>

<span data-ttu-id="b351c-132">Následující tabulka obsahuje příklady použití souboru schématu pro různé scénáře:</span><span class="sxs-lookup"><span data-stu-id="b351c-132">The following table provides examples of using the file scheme for different scenarios:</span></span>

| <span data-ttu-id="b351c-133">Schéma</span><span class="sxs-lookup"><span data-stu-id="b351c-133">Scheme</span></span> | <span data-ttu-id="b351c-134">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b351c-134">Notes</span></span> |
| ----- | ----- |
| `wasb:///` | <span data-ttu-id="b351c-135">Výchozí účet úložiště je kontejner objektů blob v účtu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b351c-135">The default storage account is a blob container in an Azure Storage account</span></span> |
| `adl:///` | <span data-ttu-id="b351c-136">Výchozí účet úložiště je adresář v Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="b351c-136">The default storage account is a directory in Azure Data Lake Store.</span></span> <span data-ttu-id="b351c-137">Při vytváření clusteru zadejte adresář v Data Lake Store, který je kořenem HDFS clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-137">During cluster creation, you specify the directory in Data Lake Store that is the root of the cluster's HDFS.</span></span> <span data-ttu-id="b351c-138">Například `/clusters/myclustername/` adresáře.</span><span class="sxs-lookup"><span data-stu-id="b351c-138">For example, the `/clusters/myclustername/` directory.</span></span> |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | <span data-ttu-id="b351c-139">Účet úložiště Azure (Další) jiné než výchozí přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-139">A non-default (additional) Azure storage account associated with the cluster.</span></span> |
| `adl://STORENAME/` | <span data-ttu-id="b351c-140">Kořenovém adresáři Data Lake Store používaný v clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-140">The root of the Data Lake Store used by the cluster.</span></span> <span data-ttu-id="b351c-141">Toto schéma umožňuje přístup k datům, která se nachází mimo adresář, který obsahuje clusteru systému souborů.</span><span class="sxs-lookup"><span data-stu-id="b351c-141">This scheme allows you to access data that is located outside the directory that contains the cluster file system.</span></span> |

<span data-ttu-id="b351c-142">Další informace najdete v tématu [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) odkaz na Apache.org.</span><span class="sxs-lookup"><span data-stu-id="b351c-142">For more information, see the [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) reference at Apache.org.</span></span>

### <a name="example-configuration"></a><span data-ttu-id="b351c-143">Příklad konfigurace</span><span class="sxs-lookup"><span data-stu-id="b351c-143">Example configuration</span></span>

<span data-ttu-id="b351c-144">Následující YAML je výňatek ze `resources/writetohdfs.yaml` zahrnutý v příkladu.</span><span class="sxs-lookup"><span data-stu-id="b351c-144">The following YAML is an excerpt from the `resources/writetohdfs.yaml` file included in the example.</span></span> <span data-ttu-id="b351c-145">Tento soubor definuje pomocí topologie Storm [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework pro Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="b351c-145">This file defines the Storm topology using the [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework for Apache Storm.</span></span>

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

<span data-ttu-id="b351c-146">Tato YAML definuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="b351c-146">This YAML defines the following items:</span></span>

* <span data-ttu-id="b351c-147">`syncPolicy`: Definuje, když jsou soubory synchronizována/vyprazdňuje na systém souborů.</span><span class="sxs-lookup"><span data-stu-id="b351c-147">`syncPolicy`: Defines when files are synched/flushed to the file system.</span></span> <span data-ttu-id="b351c-148">V tomto příkladu každých 1000 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="b351c-148">In this example, every 1000 tuples.</span></span>
* <span data-ttu-id="b351c-149">`fileNameFormat`: Definuje použijte při zápisu souborů vzoru pro název a cesta k souboru.</span><span class="sxs-lookup"><span data-stu-id="b351c-149">`fileNameFormat`: Defines the path and file name pattern to use when writing files.</span></span> <span data-ttu-id="b351c-150">V tomto příkladu jsou poskytovány cesta za běhu pomocí filtru, a přípona souboru `.txt`.</span><span class="sxs-lookup"><span data-stu-id="b351c-150">In this example, the path is provided at runtime using a filter, and the file extension is `.txt`.</span></span>
* <span data-ttu-id="b351c-151">`recordFormat`: Definuje interní formát zapisovat soubory.</span><span class="sxs-lookup"><span data-stu-id="b351c-151">`recordFormat`: Defines the internal format of the files written.</span></span> <span data-ttu-id="b351c-152">V tomto příkladu jsou odděleny pole `|` znak.</span><span class="sxs-lookup"><span data-stu-id="b351c-152">In this example, fields are delimited by the `|` character.</span></span>
* <span data-ttu-id="b351c-153">`rotationPolicy`: Určuje, kdy otočení soubory.</span><span class="sxs-lookup"><span data-stu-id="b351c-153">`rotationPolicy`: Defines when to rotate files.</span></span> <span data-ttu-id="b351c-154">V tomto příkladu se provádí bez otočení.</span><span class="sxs-lookup"><span data-stu-id="b351c-154">In this example, no rotation is performed.</span></span>
* <span data-ttu-id="b351c-155">`hdfs-bolt`: Používá předchozí komponenty jako parametry konfigurace pro `HdfsBolt` třídy.</span><span class="sxs-lookup"><span data-stu-id="b351c-155">`hdfs-bolt`: Uses the previous components as configuration parameters for the `HdfsBolt` class.</span></span>

<span data-ttu-id="b351c-156">Další informace o rozhraní tok najdete v tématu [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="b351c-156">For more information on the Flux framework, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="configure-the-cluster"></a><span data-ttu-id="b351c-157">Konfigurace clusteru</span><span class="sxs-lookup"><span data-stu-id="b351c-157">Configure the cluster</span></span>

<span data-ttu-id="b351c-158">Ve výchozím nastavení Storm v HDInsight neobsahuje součásti, které HdfsBolt používá ke komunikaci s Azure Storage nebo Data Lake Store v Storm je cesta pro třídy.</span><span class="sxs-lookup"><span data-stu-id="b351c-158">By default, Storm on HDInsight does not include the components that HdfsBolt uses to communicate with Azure Storage or Data Lake Store in Storm's classpath.</span></span> <span data-ttu-id="b351c-159">Pomocí následující akce skriptu přidejte tyto součásti `extlib` adresář pro Storm v clusteru:</span><span class="sxs-lookup"><span data-stu-id="b351c-159">Use the following script action to add these components to the `extlib` directory for Storm on your cluster:</span></span>

<span data-ttu-id="b351c-160">| Identifikátor URI skriptu | Uzly a použijte ji k | Parametry || `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, nadřízeného | None |</span><span class="sxs-lookup"><span data-stu-id="b351c-160">| Script URI | Nodes to apply it to| Parameters | | `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, Supervisor | None |</span></span>

<span data-ttu-id="b351c-161">Informace o použití tohoto skriptu k vašemu clusteru najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](./hdinsight-hadoop-customize-cluster-linux.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b351c-161">For information on using this script with your cluster, see the [Customize HDInsight clusters using script actions](./hdinsight-hadoop-customize-cluster-linux.md) document.</span></span>

## <a name="build-and-package-the-topology"></a><span data-ttu-id="b351c-162">Sestavení a balíček topologie</span><span class="sxs-lookup"><span data-stu-id="b351c-162">Build and package the topology</span></span>

1. <span data-ttu-id="b351c-163">Stáhněte si příklad projektu ze [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) na svoje vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b351c-163">Download the example project from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) to your development environment.</span></span>

2. <span data-ttu-id="b351c-164">Z příkazového řádku, terminálu nebo skořápce relace, změnit adresáře do kořenového adresáře staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="b351c-164">From a command prompt, terminal, or shell session, change directories to the root of the downloaded project.</span></span> <span data-ttu-id="b351c-165">Pro sestavení a balíček topologii, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b351c-165">To build and package the topology, use the following command:</span></span>
   
        mvn compile package
   
    <span data-ttu-id="b351c-166">Po dokončení sestavení a balení, je nový adresář s názvem `target`, který obsahuje soubor s názvem `StormToHdfs-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="b351c-166">Once the build and packaging completes, there is a new directory named `target`, that contains a file named `StormToHdfs-1.0-SNAPSHOT.jar`.</span></span> <span data-ttu-id="b351c-167">Tento soubor obsahuje kompilované topologie.</span><span class="sxs-lookup"><span data-stu-id="b351c-167">This file contains the compiled topology.</span></span>

## <a name="deploy-and-run-the-topology"></a><span data-ttu-id="b351c-168">Nasazení a spuštění topologie</span><span class="sxs-lookup"><span data-stu-id="b351c-168">Deploy and run the topology</span></span>

1. <span data-ttu-id="b351c-169">Použijte následující příkaz pro kopírování topologie do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b351c-169">Use the following command to copy the topology to the HDInsight cluster.</span></span> <span data-ttu-id="b351c-170">Nahraďte **uživatele** uživatelským jménem SSH, které jste použili při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-170">Replace **USER** with the SSH user name you used when creating the cluster.</span></span> <span data-ttu-id="b351c-171">Místo **CLUSTERNAME** zadejte název vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-171">Replace **CLUSTERNAME** with the name of the cluster.</span></span>
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    <span data-ttu-id="b351c-172">Po zobrazení výzvy zadejte heslo použité při vytváření uživatele SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="b351c-172">When prompted, enter the password used when creating the SSH user for the cluster.</span></span> <span data-ttu-id="b351c-173">Pokud jste použili veřejný klíč místo hesla, budete možná muset použít `-i` parametru určete cestu k odpovídající soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="b351c-173">If you used a public key instead of a password, you may need to use the `-i` parameter to specify the path to the matching private key.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b351c-174">Další informace o používání `scp` s HDInsight, najdete v části [použití SSH s HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b351c-174">For more information on using `scp` with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="b351c-175">Po dokončení nahrávání, použijte následující se připojit ke clusteru HDInsight pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="b351c-175">Once the upload completes, use the following to connect to the HDInsight cluster using SSH.</span></span> <span data-ttu-id="b351c-176">Nahraďte **uživatele** uživatelským jménem SSH, které jste použili při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-176">Replace **USER** with the SSH user name you used when creating the cluster.</span></span> <span data-ttu-id="b351c-177">Místo **CLUSTERNAME** zadejte název vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-177">Replace **CLUSTERNAME** with the name of the cluster.</span></span>
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="b351c-178">Po zobrazení výzvy zadejte heslo použité při vytváření uživatele SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="b351c-178">When prompted, enter the password used when creating the SSH user for the cluster.</span></span> <span data-ttu-id="b351c-179">Pokud jste použili veřejný klíč místo hesla, budete možná muset použít `-i` parametru určete cestu k odpovídající soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="b351c-179">If you used a public key instead of a password, you may need to use the `-i` parameter to specify the path to the matching private key.</span></span>
   
   <span data-ttu-id="b351c-180">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b351c-180">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="b351c-181">Po připojení, použijte následující příkaz k vytvoření souboru s názvem `dev.properties`:</span><span class="sxs-lookup"><span data-stu-id="b351c-181">Once connected, use the following command to create a file named `dev.properties`:</span></span>

        nano dev.properties

4. <span data-ttu-id="b351c-182">Použít následující text jako obsah `dev.properties` souboru:</span><span class="sxs-lookup"><span data-stu-id="b351c-182">Use the following text as the contents of the `dev.properties` file:</span></span>

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > <span data-ttu-id="b351c-183">Tento příklad předpokládá, že váš cluster používá jako úložiště pro výchozí účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b351c-183">This example assumes that your cluster uses an Azure Storage account as the default storage.</span></span> <span data-ttu-id="b351c-184">Pokud váš cluster používá Azure Data Lake Store, použijte `hdfs.url: adl:///` místo.</span><span class="sxs-lookup"><span data-stu-id="b351c-184">If your cluster uses Azure Data Lake Store, use `hdfs.url: adl:///` instead.</span></span>
    
    <span data-ttu-id="b351c-185">Chcete-li uložit soubor, použijte __kombinaci kláves Ctrl + X__, pak __Y__a v neposlední řadě __Enter__.</span><span class="sxs-lookup"><span data-stu-id="b351c-185">To save the file, use __Ctrl + X__, then __Y__, and finally __Enter__.</span></span> <span data-ttu-id="b351c-186">Hodnoty v tomto souboru nastavit adresu URL obchodu s Data Lake a název adresáře, který data se zapisují do.</span><span class="sxs-lookup"><span data-stu-id="b351c-186">The values in this file set the Data Lake store URL and the directory name that data is written to.</span></span>

3. <span data-ttu-id="b351c-187">Použijte následující příkaz spusťte topologie:</span><span class="sxs-lookup"><span data-stu-id="b351c-187">Use the following command to start the topology:</span></span>
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    <span data-ttu-id="b351c-188">Tento příkaz spustí topologie pomocí rozhraní tok odesláním uzel Nimbus clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-188">This command starts the topology using the Flux framework by submitting it to the Nimbus node of the cluster.</span></span> <span data-ttu-id="b351c-189">Topologie je definována `writetohdfs.yaml` souborů, které jsou součástí jar.</span><span class="sxs-lookup"><span data-stu-id="b351c-189">The topology is defined by the `writetohdfs.yaml` file included in the jar.</span></span> <span data-ttu-id="b351c-190">`dev.properties` Souboru se předá jako filtr a hodnoty obsažené v souboru jsou číst topologii.</span><span class="sxs-lookup"><span data-stu-id="b351c-190">The `dev.properties` file is passed as a filter, and the values contained in the file are read by the topology.</span></span>

## <a name="view-output-data"></a><span data-ttu-id="b351c-191">Zobrazení výstupní data</span><span class="sxs-lookup"><span data-stu-id="b351c-191">View output data</span></span>

<span data-ttu-id="b351c-192">Chcete-li zobrazit data, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b351c-192">To view the data, use the following command:</span></span>

    hdfs dfs -ls /stormdata/

<span data-ttu-id="b351c-193">Zobrazí se seznam souborů vytvořených pomocí této topologii.</span><span class="sxs-lookup"><span data-stu-id="b351c-193">A list of the files created by this topology is displayed.</span></span>

<span data-ttu-id="b351c-194">V následujícím seznamu je příklad dat retuned podle předchozích příkazů:</span><span class="sxs-lookup"><span data-stu-id="b351c-194">The following list is an example of the data retuned by the previous commands:</span></span>

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-the-topology"></a><span data-ttu-id="b351c-195">Zastavení topologie</span><span class="sxs-lookup"><span data-stu-id="b351c-195">Stop the topology</span></span>

<span data-ttu-id="b351c-196">Topologie Storm spustit, dokud nebude zastaven nebo odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="b351c-196">Storm topologies run until stopped, or the cluster is deleted.</span></span> <span data-ttu-id="b351c-197">K zastavení topologie, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b351c-197">To stop the topology, use the following command:</span></span>

    storm kill hdfswriter

## <a name="delete-your-cluster"></a><span data-ttu-id="b351c-198">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="b351c-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="b351c-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b351c-199">Next steps</span></span>

<span data-ttu-id="b351c-200">Teď, když jste se naučili jak používat Storm k zápisu do úložiště Azure a Azure Data Lake Store, zjišťování dalších [Storm příklady pro HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="b351c-200">Now that you have learned how to use Storm to write to Azure Storage and Azure Data Lake Store, discover other [Storm examples for HDInsight](hdinsight-storm-example-topology.md).</span></span>

