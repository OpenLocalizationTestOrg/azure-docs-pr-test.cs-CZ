---
title: "Zpracování událostí ze služby Event Hubs se Storm v HDInsight pomocí Java | Microsoft Docs"
description: "Informace o zpracování dat služby Event Hubs s topologie Java Storm vytvořené pomocí Maven."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 453fa7b0-c8a6-413e-8747-3ac3b71bed86
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/13/2017
ms.author: larryfr
ms.openlocfilehash: 2e8ebbdab2be7bed224a67facec798820615bb22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a><span data-ttu-id="d1bb8-103">Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="d1bb8-103">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>

<span data-ttu-id="d1bb8-104">Naučte se používat Azure Event Hubs se Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-104">Learn how to use Azure Event Hubs with Storm on HDInsight.</span></span> <span data-ttu-id="d1bb8-105">Tento příklad používá založené na jazyce Java součásti číst a zapisovat data v Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-105">This example uses Java-based components to read and write data in Azure Event Hubs.</span></span>

<span data-ttu-id="d1bb8-106">Azure Event Hubs umožňuje zpracovat masivní objemy dat z webů, aplikací a zařízení.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-106">Azure Event Hubs allows you to process massive amounts of data from websites, apps, and devices.</span></span> <span data-ttu-id="d1bb8-107">Spout Centrum událostí je snadno použitelný Apache Storm v HDInsight k analýze tato data v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-107">The Event Hub spout makes it easy to use Apache Storm on HDInsight to analyze this data in real time.</span></span> <span data-ttu-id="d1bb8-108">Můžete také zápisu dat do centra událostí z Storm pomocí bolt Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-108">You can also write data to Event Hubs from Storm by using the Event Hubs bolt.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1bb8-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d1bb8-109">Prerequisites</span></span>

* <span data-ttu-id="d1bb8-110">Apache Storm na verzi clusteru HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-110">An Apache Storm on HDInsight cluster version 3.6.</span></span> <span data-ttu-id="d1bb8-111">Další informace najdete v tématu [Začínáme se Storm v clusteru HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-111">For more information, see [Get started with Storm on HDInsight cluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d1bb8-112">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-112">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d1bb8-113">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-113">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="d1bb8-114">[Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-114">An [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="d1bb8-115">[Oracle Java Developer Kit (JDK) verze 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo stejná, jako třeba [OpenJDK](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or equivalent, such as [OpenJDK](http://openjdk.java.net/).</span></span>

* <span data-ttu-id="d1bb8-116">[Maven](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-116">[Maven](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="d1bb8-117">Textového editoru nebo integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-117">A text editor or integrated development environment (IDE).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d1bb8-118">Editor nebo IDE může mít specifické funkce pro práci s Maven, který mu není adresovaný v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-118">Your editor or IDE may have specific functionality for working with Maven that is not addressed in this document.</span></span> <span data-ttu-id="d1bb8-119">Informace o možnostech vašeho prostředí pro úpravy najdete v dokumentaci pro produkt, který používáte.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-119">For information about the capabilities of your editing environment, see the documentation for the product you are using.</span></span>

    * <span data-ttu-id="d1bb8-120">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-120">An SSH client.</span></span> <span data-ttu-id="d1bb8-121">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-121">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="d1bb8-122">`ssh` a `scp` příkazy.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-122">The `ssh` and `scp` commands.</span></span> <span data-ttu-id="d1bb8-123">Ty se používají pro kopírování souborů do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-123">These are used to copy files to the HDInsight cluster.</span></span> <span data-ttu-id="d1bb8-124">V systému Windows můžete získat tyto prostřednictvím Bash ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-124">On Windows, you can get these through Bash on Windows 10.</span></span>

## <a name="understanding-the-example"></a><span data-ttu-id="d1bb8-125">Principy příklad</span><span class="sxs-lookup"><span data-stu-id="d1bb8-125">Understanding the example</span></span>

<span data-ttu-id="d1bb8-126">[Hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) příklad obsahuje dvě topologie:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-126">The [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) example contains two topologies:</span></span>

<span data-ttu-id="d1bb8-127">`resources/writer.yaml` Topologie zapíše náhodná data do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-127">The `resources/writer.yaml` topology writes random data to an Azure Event Hub.</span></span> <span data-ttu-id="d1bb8-128">Data je generován `DeviceSpout` součást, a je ID náhodných zařízení a zařízení hodnota.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-128">The data is generated by the `DeviceSpout` component, and is a random device ID and device value.</span></span> <span data-ttu-id="d1bb8-129">Proto se simuluje některé hardwaru, který vysílá řetězec ID a číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-129">So it's simulating some hardware that emits a string ID and a numeric value.</span></span>

<span data-ttu-id="d1bb8-130">Thí `resources/reader.yaml` topologie čte data z centra událostí (data podle EventHubWriter, zapisují) analyzuje JSON data a pak protokoly `deviceId` a `deviceValue` data.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-130">Thee `resources/reader.yaml` topology reads data from Event Hub (the data written by EventHubWriter,) parses the JSON data, and then logs the `deviceId` and `deviceValue` data.</span></span>

<span data-ttu-id="d1bb8-131">Data je naformátován jako dokument JSON předtím, než je zapsán do centra událostí a když číst čtečky ho je analyzována z JSON a do řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-131">The data is formatted as a JSON document before it is written to Event Hub, and when read by the reader it is parsed out of JSON and into tuples.</span></span> <span data-ttu-id="d1bb8-132">Formát JSON je následující:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-132">The JSON format is as follows:</span></span>

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a><span data-ttu-id="d1bb8-133">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="d1bb8-133">Project configuration</span></span>

<span data-ttu-id="d1bb8-134">`POM.xml` Soubor obsahuje informace o konfiguraci pro tento projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-134">The `POM.xml` file contains configuration information for this Maven project.</span></span> <span data-ttu-id="d1bb8-135">Jsou zajímavé částí:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-135">The interesting pieces are:</span></span>

#### <a name="event-hub-components"></a><span data-ttu-id="d1bb8-136">Součásti centra událostí</span><span class="sxs-lookup"><span data-stu-id="d1bb8-136">Event Hub components</span></span>

<span data-ttu-id="d1bb8-137">Komponenta, která čte a zapisuje do služby Azure Event Hubs se nachází v [úložiště HDInsight](https://github.com/hdinsight/mvn-rep).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-137">The component that reads and writes to Azure Event Hubs is located in the [HDInsight repository](https://github.com/hdinsight/mvn-rep).</span></span> <span data-ttu-id="d1bb8-138">V následujících částech v `POM.xml` souborové zatížení komponenty z tohoto úložiště</span><span class="sxs-lookup"><span data-stu-id="d1bb8-138">The following sections in the `POM.xml` file load the components from this repository</span></span>

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="the-eventhubs-storm-spout-dependency"></a><span data-ttu-id="d1bb8-139">Závislost EventHubs Storm Spout</span><span class="sxs-lookup"><span data-stu-id="d1bb8-139">The EventHubs Storm Spout dependency</span></span>

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

<span data-ttu-id="d1bb8-140">Tato konfigurace xml definuje závislost pro balíčku eventhubs, která obsahuje spout pro čtení ze služby Event Hubs a na funkce bolt pro zápis do něj.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-140">This xml defines a dependency for the eventhubs package, which contains both a spout for reading from Event Hubs, and a bolt for writing to it.</span></span>

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

<span data-ttu-id="d1bb8-141">Tato konfigurace xml nakonfiguruje projektu pro generování výstupu pro jazyk Java 8, který se používá v HDInsight 3.5 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-141">This xml configures the project to generate output for Java 8, which is used by HDInsight 3.5 or higher.</span></span>

#### <a name="the-maven-shade-plugin"></a><span data-ttu-id="d1bb8-142">Plugin stín maven</span><span class="sxs-lookup"><span data-stu-id="d1bb8-142">The maven-shade-plugin</span></span>

```xml
<!-- build an uber jar -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>2.3</version>
<configuration>
    <transformers>
    <!-- Keep us from getting a can't overwrite file error -->
    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
    </transformers>
    <!-- Keep us from getting a bad signature error -->
    <filters>
    <filter>
        <artifact>*:*</artifact>
        <excludes>
            <exclude>META-INF/*.SF</exclude>
            <exclude>META-INF/*.DSA</exclude>
            <exclude>META-INF/*.RSA</exclude>
        </excludes>
    </filter>
    </filters>
</configuration>
<executions>
    <execution>
    <phase>package</phase>
    <goals>
        <goal>shade</goal>
    </goals>
    </execution>
</executions>
</plugin>
```

<span data-ttu-id="d1bb8-143">Tato konfigurace xml lze konfigurovat řešení tak, aby balíček výstup do uber jar.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-143">This xml configures the solution to package the output into an uber jar.</span></span> <span data-ttu-id="d1bb8-144">Jar obsahuje kód projektu i požadované závislosti.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-144">The jar contains both the project code and required dependencies.</span></span> <span data-ttu-id="d1bb8-145">Používá se také na:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-145">It is also used to:</span></span>

* <span data-ttu-id="d1bb8-146">Přejmenování souborů s licencí pro závislosti.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-146">Rename license files for the dependencies.</span></span>
* <span data-ttu-id="d1bb8-147">Vylučte zabezpečení nebo podpisy.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-147">Exclude security/signatures.</span></span>
* <span data-ttu-id="d1bb8-148">Zajistěte, aby více implementace stejné rozhraní jsou sloučeny do jednu položku.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-148">Ensure that multiple implementations of the same interface are merged into one entry.</span></span>

<span data-ttu-id="d1bb8-149">Tato nastavení konfigurace zabránit chybám za běhu.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-149">These configuration settings prevent errors at runtime.</span></span>

#### <a name="topology-definitions"></a><span data-ttu-id="d1bb8-150">Definice topologie</span><span class="sxs-lookup"><span data-stu-id="d1bb8-150">Topology definitions</span></span>

<span data-ttu-id="d1bb8-151">Tento příklad používá [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-151">This example uses the [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span></span> <span data-ttu-id="d1bb8-152">Toto rozhraní používá YAML pro definování uvedené topologie.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-152">This framework uses YAML to define the topologies.</span></span> <span data-ttu-id="d1bb8-153">Primární výhodou je, že nejste pevné kódování topologii v jazyce Java kódu.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-153">The primary benefit is that you aren't hard coding the topology in Java code.</span></span> <span data-ttu-id="d1bb8-154">Vzhledem k tomu, že definice je YAML, můžete ji změnit před odesláním topologii, aniž by museli znovu zkompiluje vše.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-154">Since the definition is YAML, you can change it before submitting the topology, without having to recompile everything.</span></span>

<span data-ttu-id="d1bb8-155">__Writer.yaml__:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-155">__writer.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure the Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
      - "${eventhub.write.policy.name}"
      - "${eventhub.write.policy.key}"
      - "${eventhub.namespace}"
      - "servicebus.windows.net"
      - "${eventhub.name}"

spouts:
  - id: "device-emulator-spout"
    className: "com.microsoft.example.DeviceSpout"
    parallelism: ${eventhub.partitions}

bolts:
  - id: "eventhub-bolt"
    className: "org.apache.storm.eventhubs.bolt.EventHubBolt"
    constructorArgs:
      - ref: "eventhubbolt-config" # config declared in components section
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through the components
streams:
  - name: "spout -> eventhub" # just a string used for logging
    from: "device-emulator-spout"
    to: "eventhub-bolt"
    grouping:
        type: SHUFFLE

  - name: "spout -> logger"
    from: "device-emulator-spout"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

<span data-ttu-id="d1bb8-156">__Reader.yaml__:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-156">__reader.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure the Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
      - "${eventhub.read.policy.name}"
      - "${eventhub.read.policy.key}"
      - "${eventhub.namespace}"
      - "${eventhub.name}"
      - ${eventhub.partitions}

spouts:
  - id: "eventhub-spout"
    className: "org.apache.storm.eventhubs.spout.EventHubSpout"
    constructorArgs:
      - ref: "eventhubspout-config" # config declared in components section
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

bolts:
  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

  # Parses from JSON into tuples
  - id: "parser-bolt"
    className: "com.microsoft.example.ParserBolt"
    parallelism: ${eventhub.partitions}

# How data flows through the components
streams:
  - name: "spout -> parser" # just a string used for logging
    from: "eventhub-spout"
    to: "parser-bolt"
    grouping:
        type: SHUFFLE

  - name: "parser -> log-bolt"
    from: "parser-bolt"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

#### <a name="tell-the-topology-about-event-hub"></a><span data-ttu-id="d1bb8-157">Topologie říct o centra událostí</span><span class="sxs-lookup"><span data-stu-id="d1bb8-157">Tell the topology about Event Hub</span></span>

<span data-ttu-id="d1bb8-158">V době běhu `dev.properties` soubor se používá k nastavení centra událostí předat topologii.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-158">At run time, the `dev.properties` file is used to pass the Event Hub configuration to the topology.</span></span> <span data-ttu-id="d1bb8-159">V následujícím příkladu je výchozí obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-159">The following example is the default contents of the file:</span></span>

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a><span data-ttu-id="d1bb8-160">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="d1bb8-160">Configure environment variables</span></span>

<span data-ttu-id="d1bb8-161">Následující proměnné prostředí může být nastaven při instalaci Java a sadu JDK na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-161">The following environment variables may be set when you install Java and the JDK on your development workstation.</span></span> <span data-ttu-id="d1bb8-162">Nicméně byste měli zkontrolovat, že existují a že obsahují správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-162">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="d1bb8-163">**JAVA_HOME** -by měla odkazovat na adresář, kam nainstalovat prostředí Java runtime (JRE).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-163">**JAVA_HOME** - should point to the directory where the Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="d1bb8-164">Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou `/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-164">For example, in a Unix or Linux distribution, it should have a value similar to `/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="d1bb8-165">V systému Windows má hodnotu podobnou`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="d1bb8-165">In Windows, it would have a value similar to `c:\Program Files (x86)\Java\jre1.7`</span></span>
* <span data-ttu-id="d1bb8-166">**CESTA** -musí obsahovat následující cesty:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-166">**PATH** - should contain the following paths:</span></span>

  * <span data-ttu-id="d1bb8-167">**JAVA_HOME** (nebo ekvivalentní cesta)</span><span class="sxs-lookup"><span data-stu-id="d1bb8-167">**JAVA_HOME** (or the equivalent path)</span></span>
  * <span data-ttu-id="d1bb8-168">**JAVA_HOME\bin** (nebo ekvivalentní cesta)</span><span class="sxs-lookup"><span data-stu-id="d1bb8-168">**JAVA_HOME\bin** (or the equivalent path)</span></span>
  * <span data-ttu-id="d1bb8-169">Adresář, kde je nainstalován Maven</span><span class="sxs-lookup"><span data-stu-id="d1bb8-169">The directory where Maven is installed</span></span>

## <a name="configure-event-hub"></a><span data-ttu-id="d1bb8-170">Konfigurace centra událostí</span><span class="sxs-lookup"><span data-stu-id="d1bb8-170">Configure Event Hub</span></span>

<span data-ttu-id="d1bb8-171">Event Hubs je zdroj dat pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-171">Event Hubs is the data source for this example.</span></span> <span data-ttu-id="d1bb8-172">Pomocí následujících kroků můžete vytvořit Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-172">Use the following steps to create a Event Hub.</span></span>

1. <span data-ttu-id="d1bb8-173">Z [portálu Azure Classic](https://manage.windowsazure.com), vyberte **nový** > **Service Bus** > **centra událostí**  >  **Vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-173">From the [Azure Classic Portal](https://manage.windowsazure.com), select **NEW** > **Service Bus** > **Event Hub** > **Custom Create**.</span></span>

2. <span data-ttu-id="d1bb8-174">Na **přidat nového centra událostí** obrazovky, zadejte **název centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-174">On the **Add a new Event Hub** screen, enter an **Event Hub Name**.</span></span> <span data-ttu-id="d1bb8-175">Vyberte **oblast** vytvoření centrum a pak vytvořit obor názvů nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-175">Select the **Region** to create the hub in, and then create a namespace or select an existing one.</span></span> <span data-ttu-id="d1bb8-176">Nakonec klikněte na **šipku** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-176">Finally, click the **Arrow** to continue.</span></span>

    ![Stránka 1 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > <span data-ttu-id="d1bb8-178">Vyberte stejný **umístění** jako Storm v HDInsight serveru a snižuje tak latenci a náklady.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-178">Select the same **Location** as your Storm on HDInsight server to reduce latency and costs.</span></span>

3. <span data-ttu-id="d1bb8-179">Na **Konfigurace centra událostí** obrazovky, zadejte **oddílu počet** a **uchování zpráv** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-179">On the **Configure Event Hub** screen, enter the **Partition count** and **Message Retention** values.</span></span> <span data-ttu-id="d1bb8-180">V tomto příkladu použijte počet oddílů 10 a uchování zpráv 1.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-180">For this example, use a partition count of 10 and a message retention of 1.</span></span> <span data-ttu-id="d1bb8-181">Poznámka: počet oddílů, protože je tato hodnota potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-181">Note the partition count because you need this value later.</span></span>

    ![Stránka 2 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. <span data-ttu-id="d1bb8-183">Po vytvoření centra událostí, vyberte obor názvů, vyberte **Event Hubs**a potom vyberte centra událostí, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-183">After the event hub has been created, select the namespace, select **Event Hubs**, and then select the event hub that you created earlier.</span></span>
5. <span data-ttu-id="d1bb8-184">Vyberte **konfigurace**, pak vytvořte dva nové zásady přístupu pomocí následující informace:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-184">Select **Configure**, then create two new access policies by using the following information:</span></span>

    <table>
    <tr><th><span data-ttu-id="d1bb8-185">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d1bb8-185">Name</span></span></th><th><span data-ttu-id="d1bb8-186">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="d1bb8-186">Permissions</span></span></th></tr>
    <tr><td><span data-ttu-id="d1bb8-187">Modul pro zápis</span><span class="sxs-lookup"><span data-stu-id="d1bb8-187">Writer</span></span></td><td><span data-ttu-id="d1bb8-188">Odeslat</span><span class="sxs-lookup"><span data-stu-id="d1bb8-188">Send</span></span></td></tr>
    <tr><td><span data-ttu-id="d1bb8-189">Čtenář</span><span class="sxs-lookup"><span data-stu-id="d1bb8-189">Reader</span></span></td><td><span data-ttu-id="d1bb8-190">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="d1bb8-190">Listen</span></span></td></tr>
    </table>

    <span data-ttu-id="d1bb8-191">Po vytvoření oprávnění, vyberte **Uložit** ikona v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-191">After You create the permissions, select the **Save** icon at the bottom of the page.</span></span> <span data-ttu-id="d1bb8-192">Tyto zásady sdíleného přístupu slouží ke čtení a zápisu do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-192">These shared access policies are used to read and write to Event Hub.</span></span>

    ![zásady](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. <span data-ttu-id="d1bb8-194">Po uložení zásady, pomocí **klíče generátor sdíleného přístupu** v dolní části stránky načíst klíč pro **zapisovače** a **čtečky** zásady.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-194">After you save the policies, use the **Shared access key generator** at the bottom of the page to retrieve the key for the **writer** and **reader** policies.</span></span> <span data-ttu-id="d1bb8-195">Uložte tyto klíče.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-195">Save these keys.</span></span>

## <a name="download-and-build-the-project"></a><span data-ttu-id="d1bb8-196">Stáhněte si a sestavte projekt</span><span class="sxs-lookup"><span data-stu-id="d1bb8-196">Download and build the project</span></span>

1. <span data-ttu-id="d1bb8-197">Stažení projektu z Githubu: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-197">Download the project from GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span></span> <span data-ttu-id="d1bb8-198">Můžete stáhnout balíček jako archivu zip, nebo použijte [git](https://git-scm.com/) klonovat projektu místně.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-198">You can either download the package as a zip archive, or use [git](https://git-scm.com/) to clone the project locally.</span></span>

2. <span data-ttu-id="d1bb8-199">Změnit `dev.properties` soubor s konfigurací pro vaše Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-199">Modify the `dev.properties` file with the configuration for your Event Hub.</span></span>

3. <span data-ttu-id="d1bb8-200">Použijte následující postupy k vytvoření a balíček projektu:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-200">Use the following to build and package the project:</span></span>

        mvn package

    <span data-ttu-id="d1bb8-201">Tento příkaz stáhne požadované závislosti, sestavení a pak balíčky projektu.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-201">This command downloads required dependencies, builds, and then packages the project.</span></span> <span data-ttu-id="d1bb8-202">Výstup je uložen v **/target** adresáři jako **EventHubExample. 1.0 SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-202">The output is stored in the **/target** directory as **EventHubExample-1.0-SNAPSHOT.jar**.</span></span>

## <a name="test-locally"></a><span data-ttu-id="d1bb8-203">Test místně</span><span class="sxs-lookup"><span data-stu-id="d1bb8-203">Test locally</span></span>

<span data-ttu-id="d1bb8-204">Vzhledem k tomu, že tyto topologie jenom číst a zapisovat do centra událostí, je můžete otestovat místně, pokud máte [Storm vývojového prostředí](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-204">Since these topologies just read and write to Event Hubs, you can test them locally if you have a [Storm development environment](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span></span> <span data-ttu-id="d1bb8-205">Spustit místně v prostředí dev pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-205">Use the following steps to run locally in the dev environment:</span></span>

1. <span data-ttu-id="d1bb8-206">Spusťte modul pro zápis:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-206">Run the writer:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. <span data-ttu-id="d1bb8-207">Spusťte program pro čtení:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-207">Run the reader:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * <span data-ttu-id="d1bb8-208">`--local`: Spusťte topologii v místním režimu (bez distribuované).</span><span class="sxs-lookup"><span data-stu-id="d1bb8-208">`--local`: Run the topology in local mode (non-distributed).</span></span>
> * <span data-ttu-id="d1bb8-209">`-R /writer.yaml`: Načíst definici topologie z `resources` součástí jar.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-209">`-R /writer.yaml`: Load the topology definition from the `resources` packaged in the jar.</span></span> <span data-ttu-id="d1bb8-210">Pokud topologie je soubor v místním systému souborů, zadejte cestu k němu místo jako poslední parametr.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-210">If the topology is a file on the local file system, specify the path to it as the last parameter instead.</span></span>
> * <span data-ttu-id="d1bb8-211">`--filter dev.properties`: Použijte obsah `dev.properties` vyplnit hodnoty v definicích topologie.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-211">`--filter dev.properties`: Use the contents of `dev.properties` to fill in the values in the topology definitions.</span></span> <span data-ttu-id="d1bb8-212">Například, `${eventhub.read.policy.name}`.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-212">For example, `${eventhub.read.policy.name}`.</span></span>

<span data-ttu-id="d1bb8-213">Při místním spuštění je výstup protokolovány v konzoli.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-213">Output is logged to the console when running locally.</span></span> <span data-ttu-id="d1bb8-214">Použití __Ctrl + C__ k zastavení topologie.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-214">Use __Ctrl+C__ to stop the topology.</span></span>

## <a name="deploy-the-topologies"></a><span data-ttu-id="d1bb8-215">Nasazení topologie</span><span class="sxs-lookup"><span data-stu-id="d1bb8-215">Deploy the topologies</span></span>

1. <span data-ttu-id="d1bb8-216">Spojovací bod služby slouží ke kopírování balíčků jar ke svému clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-216">Use SCP to copy the jar package to your HDInsight cluster.</span></span> <span data-ttu-id="d1bb8-217">Nahraďte uživatelské jméno uživatele SSH pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-217">Replace USERNAME with the SSH user for your cluster.</span></span> <span data-ttu-id="d1bb8-218">Nahraďte název clusteru s názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-218">Replace CLUSTERNAME with the name of your HDInsight cluster:</span></span>

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    <span data-ttu-id="d1bb8-219">Pokud jste použili heslo pro účet SSH, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-219">If you used a password for your SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="d1bb8-220">Pokud jste použili klíče SSH pomocí účtu, budete možná muset použít `-i` parametru určete cestu k souboru klíče.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-220">If you used an SSH key with the account, you may need to use the `-i` parameter to specify the path to the key file.</span></span> <span data-ttu-id="d1bb8-221">Například `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-221">For example, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span></span>

    <span data-ttu-id="d1bb8-222">Tento příkaz zkopíruje soubor do domovského adresáře uživatelů SSH v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-222">This command copies the file to the home directory of your SSH user on the cluster.</span></span>

2. <span data-ttu-id="d1bb8-223">Po dokončení nahrávání souboru použití SSH se připojit ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-223">Once the file has finished uploading, use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="d1bb8-224">Nahraďte **uživatelské jméno** název vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-224">Replace **USERNAME** the name of your SSH login.</span></span> <span data-ttu-id="d1bb8-225">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-225">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > <span data-ttu-id="d1bb8-226">Pokud jste použili heslo pro účet SSH, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-226">If you used a password for your SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="d1bb8-227">Pokud jste použili klíče SSH pomocí účtu, budete možná muset použít `-i` parametru určete cestu k souboru klíče.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-227">If you used an SSH key with the account, you may need to use the `-i` parameter to specify the path to the key file.</span></span> <span data-ttu-id="d1bb8-228">Následující příklad načte privátní klíč z `~/.ssh/id_rsa`:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-228">The following example loads the private key from `~/.ssh/id_rsa`:</span></span>
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. <span data-ttu-id="d1bb8-229">Použijte následující příkaz pro spuštění topologie:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-229">Use the following command to start the topologies:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * <span data-ttu-id="d1bb8-230">`--remote`: Odešle topologii pro službu Nimbus, která spustí na pracovních uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-230">`--remote`: Submits the topology to the Nimbus service, which starts it on the worker nodes in the cluster.</span></span>

4. <span data-ttu-id="d1bb8-231">Chcete zobrazit data protokolu, přejděte na https://CLUSTERNAME.azurehdinsight.net/stormui, kde __CLUSTERNAME__ je název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-231">To view the logged data, go to https://CLUSTERNAME.azurehdinsight.net/stormui, where __CLUSTERNAME__ is the name of your HDInsight cluster.</span></span> <span data-ttu-id="d1bb8-232">Vyberte uvedené topologie a přejít k podrobnostem a součásti.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-232">Select the topologies and drill down to the components.</span></span> <span data-ttu-id="d1bb8-233">Vyberte __port__ položka pro instanci součást k zobrazení informací o protokolu.</span><span class="sxs-lookup"><span data-stu-id="d1bb8-233">Select the __port__ entry for an instance of a component to view logged information.</span></span>

5. <span data-ttu-id="d1bb8-234">Zastavit uvedené topologie použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d1bb8-234">Use the following commands to stop the topologies:</span></span>

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a><span data-ttu-id="d1bb8-235">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="d1bb8-235">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="d1bb8-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1bb8-236">Next steps</span></span>

* [<span data-ttu-id="d1bb8-237">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d1bb8-237">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
