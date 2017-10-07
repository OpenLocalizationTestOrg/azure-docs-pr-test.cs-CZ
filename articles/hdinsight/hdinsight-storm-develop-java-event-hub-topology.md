---
title: "aaaProcess události ze služby Event Hubs se Storm v HDInsight pomocí Java | Microsoft Docs"
description: "Zjistěte, jak vytvořit tooprocess dat služby Event Hubs s topologie Java Storm s Maven."
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
ms.openlocfilehash: 6506f5bc8f6ab0e29350c071a3f84433382038e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a><span data-ttu-id="16e8a-103">Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="16e8a-103">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>

<span data-ttu-id="16e8a-104">Zjistěte, jak toouse Azure Event Hubs se Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="16e8a-104">Learn how toouse Azure Event Hubs with Storm on HDInsight.</span></span> <span data-ttu-id="16e8a-105">Tento příklad používá založené na jazyce Java součásti tooread a zápis dat v Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="16e8a-105">This example uses Java-based components tooread and write data in Azure Event Hubs.</span></span>

<span data-ttu-id="16e8a-106">Azure Event Hubs vám umožní tooprocess masivní objemy dat z webů, aplikací a zařízení.</span><span class="sxs-lookup"><span data-stu-id="16e8a-106">Azure Event Hubs allows you tooprocess massive amounts of data from websites, apps, and devices.</span></span> <span data-ttu-id="16e8a-107">Hello Event Hub spout umožňuje snadno toouse Apache Storm v HDInsight tooanalyze tato data v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="16e8a-107">hello Event Hub spout makes it easy toouse Apache Storm on HDInsight tooanalyze this data in real time.</span></span> <span data-ttu-id="16e8a-108">Je také možné zapsat tooEvent datového centra z Storm pomocí hello funkce bolt Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="16e8a-108">You can also write data tooEvent Hubs from Storm by using hello Event Hubs bolt.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16e8a-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="16e8a-109">Prerequisites</span></span>

* <span data-ttu-id="16e8a-110">Apache Storm na verzi clusteru HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="16e8a-110">An Apache Storm on HDInsight cluster version 3.6.</span></span> <span data-ttu-id="16e8a-111">Další informace najdete v tématu [Začínáme se Storm v clusteru HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="16e8a-111">For more information, see [Get started with Storm on HDInsight cluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="16e8a-112">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="16e8a-112">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="16e8a-113">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="16e8a-113">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="16e8a-114">[Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="16e8a-114">An [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="16e8a-115">[Oracle Java Developer Kit (JDK) verze 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo stejná, jako třeba [OpenJDK](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="16e8a-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or equivalent, such as [OpenJDK](http://openjdk.java.net/).</span></span>

* <span data-ttu-id="16e8a-116">[Maven](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="16e8a-116">[Maven](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="16e8a-117">Textového editoru nebo integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="16e8a-117">A text editor or integrated development environment (IDE).</span></span>

    > [!NOTE]
    > <span data-ttu-id="16e8a-118">Editor nebo IDE může mít specifické funkce pro práci s Maven, který mu není adresovaný v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="16e8a-118">Your editor or IDE may have specific functionality for working with Maven that is not addressed in this document.</span></span> <span data-ttu-id="16e8a-119">Informace o možnostech hello prostředí pro úpravy naleznete v dokumentaci k hello hello produktu, který používáte.</span><span class="sxs-lookup"><span data-stu-id="16e8a-119">For information about hello capabilities of your editing environment, see hello documentation for hello product you are using.</span></span>

    * <span data-ttu-id="16e8a-120">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="16e8a-120">An SSH client.</span></span> <span data-ttu-id="16e8a-121">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="16e8a-121">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="16e8a-122">Hello `ssh` a `scp` příkazy.</span><span class="sxs-lookup"><span data-stu-id="16e8a-122">hello `ssh` and `scp` commands.</span></span> <span data-ttu-id="16e8a-123">Jedná se o clusteru HDInsight toohello použité toocopy soubory.</span><span class="sxs-lookup"><span data-stu-id="16e8a-123">These are used toocopy files toohello HDInsight cluster.</span></span> <span data-ttu-id="16e8a-124">V systému Windows můžete získat tyto prostřednictvím Bash ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="16e8a-124">On Windows, you can get these through Bash on Windows 10.</span></span>

## <a name="understanding-hello-example"></a><span data-ttu-id="16e8a-125">Principy hello – ukázka</span><span class="sxs-lookup"><span data-stu-id="16e8a-125">Understanding hello example</span></span>

<span data-ttu-id="16e8a-126">Hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) příklad obsahuje dvě topologie:</span><span class="sxs-lookup"><span data-stu-id="16e8a-126">hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) example contains two topologies:</span></span>

<span data-ttu-id="16e8a-127">Hello `resources/writer.yaml` topologie zapíše náhodná data tooan centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="16e8a-127">hello `resources/writer.yaml` topology writes random data tooan Azure Event Hub.</span></span> <span data-ttu-id="16e8a-128">Hello data je generována hello `DeviceSpout` součást, a je ID náhodných zařízení a zařízení hodnota.</span><span class="sxs-lookup"><span data-stu-id="16e8a-128">hello data is generated by hello `DeviceSpout` component, and is a random device ID and device value.</span></span> <span data-ttu-id="16e8a-129">Proto se simuluje některé hardwaru, který vysílá řetězec ID a číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="16e8a-129">So it's simulating some hardware that emits a string ID and a numeric value.</span></span>

<span data-ttu-id="16e8a-130">Thí `resources/reader.yaml` topologie čte data z centra událostí (služba EventHubWriter, zapíše data hello) analyzuje hello JSON data a pak protokoly hello `deviceId` a `deviceValue` data.</span><span class="sxs-lookup"><span data-stu-id="16e8a-130">Thee `resources/reader.yaml` topology reads data from Event Hub (hello data written by EventHubWriter,) parses hello JSON data, and then logs hello `deviceId` and `deviceValue` data.</span></span>

<span data-ttu-id="16e8a-131">formátování dat Hello jako dokument JSON předtím, než je zapsána tooEvent rozbočovače a když číst hello čtečky ho je analyzována z JSON a do řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="16e8a-131">hello data is formatted as a JSON document before it is written tooEvent Hub, and when read by hello reader it is parsed out of JSON and into tuples.</span></span> <span data-ttu-id="16e8a-132">Formát JSON Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="16e8a-132">hello JSON format is as follows:</span></span>

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a><span data-ttu-id="16e8a-133">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="16e8a-133">Project configuration</span></span>

<span data-ttu-id="16e8a-134">Hello `POM.xml` soubor obsahuje informace o konfiguraci pro tento projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="16e8a-134">hello `POM.xml` file contains configuration information for this Maven project.</span></span> <span data-ttu-id="16e8a-135">zajímavé částí Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="16e8a-135">hello interesting pieces are:</span></span>

#### <a name="event-hub-components"></a><span data-ttu-id="16e8a-136">Součásti centra událostí</span><span class="sxs-lookup"><span data-stu-id="16e8a-136">Event Hub components</span></span>

<span data-ttu-id="16e8a-137">Hello komponenta, která čte a zapisuje tooAzure Event Hubs se nachází v hello [úložiště HDInsight](https://github.com/hdinsight/mvn-rep).</span><span class="sxs-lookup"><span data-stu-id="16e8a-137">hello component that reads and writes tooAzure Event Hubs is located in hello [HDInsight repository](https://github.com/hdinsight/mvn-rep).</span></span> <span data-ttu-id="16e8a-138">Následující části hello Hello `POM.xml` zatížení hello komponenty soubory z tohoto úložiště</span><span class="sxs-lookup"><span data-stu-id="16e8a-138">hello following sections in hello `POM.xml` file load hello components from this repository</span></span>

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="hello-eventhubs-storm-spout-dependency"></a><span data-ttu-id="16e8a-139">Hello EventHubs Storm Spout závislostí</span><span class="sxs-lookup"><span data-stu-id="16e8a-139">hello EventHubs Storm Spout dependency</span></span>

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

<span data-ttu-id="16e8a-140">Tato konfigurace xml definuje závislost pro hello eventhubs balíček, který obsahuje spout pro čtení ze služby Event Hubs i na funkce bolt pro zápis tooit.</span><span class="sxs-lookup"><span data-stu-id="16e8a-140">This xml defines a dependency for hello eventhubs package, which contains both a spout for reading from Event Hubs, and a bolt for writing tooit.</span></span>

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

<span data-ttu-id="16e8a-141">Tato konfigurace xml nakonfiguruje toogenerate výstup hello projektu Java 8, které se používá v HDInsight 3.5 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="16e8a-141">This xml configures hello project toogenerate output for Java 8, which is used by HDInsight 3.5 or higher.</span></span>

#### <a name="hello-maven-shade-plugin"></a><span data-ttu-id="16e8a-142">Hello maven stín-modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="16e8a-142">hello maven-shade-plugin</span></span>

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

<span data-ttu-id="16e8a-143">Tato konfigurace xml nakonfiguruje hello řešení toopackage hello výstup do uber jar.</span><span class="sxs-lookup"><span data-stu-id="16e8a-143">This xml configures hello solution toopackage hello output into an uber jar.</span></span> <span data-ttu-id="16e8a-144">Hello jar obsahuje kód projektu hello a požadované závislosti.</span><span class="sxs-lookup"><span data-stu-id="16e8a-144">hello jar contains both hello project code and required dependencies.</span></span> <span data-ttu-id="16e8a-145">Používá se také na:</span><span class="sxs-lookup"><span data-stu-id="16e8a-145">It is also used to:</span></span>

* <span data-ttu-id="16e8a-146">Přejmenování souborů s licencí pro hello závislosti.</span><span class="sxs-lookup"><span data-stu-id="16e8a-146">Rename license files for hello dependencies.</span></span>
* <span data-ttu-id="16e8a-147">Vylučte zabezpečení nebo podpisy.</span><span class="sxs-lookup"><span data-stu-id="16e8a-147">Exclude security/signatures.</span></span>
* <span data-ttu-id="16e8a-148">Ujistěte se, že více implementace hello stejné rozhraní jsou sloučeny do jednu položku.</span><span class="sxs-lookup"><span data-stu-id="16e8a-148">Ensure that multiple implementations of hello same interface are merged into one entry.</span></span>

<span data-ttu-id="16e8a-149">Tato nastavení konfigurace zabránit chybám za běhu.</span><span class="sxs-lookup"><span data-stu-id="16e8a-149">These configuration settings prevent errors at runtime.</span></span>

#### <a name="topology-definitions"></a><span data-ttu-id="16e8a-150">Definice topologie</span><span class="sxs-lookup"><span data-stu-id="16e8a-150">Topology definitions</span></span>

<span data-ttu-id="16e8a-151">Tento příklad používá hello [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span><span class="sxs-lookup"><span data-stu-id="16e8a-151">This example uses hello [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span></span> <span data-ttu-id="16e8a-152">Toto rozhraní využívá YAML toodefine hello topologie.</span><span class="sxs-lookup"><span data-stu-id="16e8a-152">This framework uses YAML toodefine hello topologies.</span></span> <span data-ttu-id="16e8a-153">Hello Primární výhodou je, že nejste pevné kódování hello topologie v kódu v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="16e8a-153">hello primary benefit is that you aren't hard coding hello topology in Java code.</span></span> <span data-ttu-id="16e8a-154">Vzhledem k tomu, že definice hello je YAML, můžete ji změnit před odesláním hello topologie, bez nutnosti toorecompile vše.</span><span class="sxs-lookup"><span data-stu-id="16e8a-154">Since hello definition is YAML, you can change it before submitting hello topology, without having toorecompile everything.</span></span>

<span data-ttu-id="16e8a-155">__Writer.yaml__:</span><span class="sxs-lookup"><span data-stu-id="16e8a-155">__writer.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure hello Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
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
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through hello components
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

<span data-ttu-id="16e8a-156">__Reader.yaml__:</span><span class="sxs-lookup"><span data-stu-id="16e8a-156">__reader.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure hello Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
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
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
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

# How data flows through hello components
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

#### <a name="tell-hello-topology-about-event-hub"></a><span data-ttu-id="16e8a-157">Řekněte hello topologie o centra událostí</span><span class="sxs-lookup"><span data-stu-id="16e8a-157">Tell hello topology about Event Hub</span></span>

<span data-ttu-id="16e8a-158">V době běhu hello `dev.properties` soubor je použité toopass hello centra událostí konfigurace toohello topologie.</span><span class="sxs-lookup"><span data-stu-id="16e8a-158">At run time, hello `dev.properties` file is used toopass hello Event Hub configuration toohello topology.</span></span> <span data-ttu-id="16e8a-159">Hello následující příklad je hello výchozí obsah souboru hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-159">hello following example is hello default contents of hello file:</span></span>

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a><span data-ttu-id="16e8a-160">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="16e8a-160">Configure environment variables</span></span>

<span data-ttu-id="16e8a-161">Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="16e8a-161">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="16e8a-162">Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="16e8a-162">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="16e8a-163">**JAVA_HOME** -by měla odkazovat toohello adresáře, kde je nainstalován hello prostředí Java runtime (JRE).</span><span class="sxs-lookup"><span data-stu-id="16e8a-163">**JAVA_HOME** - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="16e8a-164">Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou příliš`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="16e8a-164">For example, in a Unix or Linux distribution, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="16e8a-165">Windows neměl by mít hodnotu podobnou příliš`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="16e8a-165">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>
* <span data-ttu-id="16e8a-166">**CESTA** -musí obsahovat hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="16e8a-166">**PATH** - should contain hello following paths:</span></span>

  * <span data-ttu-id="16e8a-167">**JAVA_HOME** (nebo ekvivalentní cesta hello)</span><span class="sxs-lookup"><span data-stu-id="16e8a-167">**JAVA_HOME** (or hello equivalent path)</span></span>
  * <span data-ttu-id="16e8a-168">**JAVA_HOME\bin** (nebo ekvivalentní cesta hello)</span><span class="sxs-lookup"><span data-stu-id="16e8a-168">**JAVA_HOME\bin** (or hello equivalent path)</span></span>
  * <span data-ttu-id="16e8a-169">Hello adresáře, kde je nainstalován Maven</span><span class="sxs-lookup"><span data-stu-id="16e8a-169">hello directory where Maven is installed</span></span>

## <a name="configure-event-hub"></a><span data-ttu-id="16e8a-170">Konfigurace centra událostí</span><span class="sxs-lookup"><span data-stu-id="16e8a-170">Configure Event Hub</span></span>

<span data-ttu-id="16e8a-171">Event Hubs je hello zdroj dat pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="16e8a-171">Event Hubs is hello data source for this example.</span></span> <span data-ttu-id="16e8a-172">Pomocí následujících kroků toocreate centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="16e8a-172">Use hello following steps toocreate a Event Hub.</span></span>

1. <span data-ttu-id="16e8a-173">Z hello [portálu Azure Classic](https://manage.windowsazure.com), vyberte **nový** > **Service Bus** > **centra událostí**  >  **Vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="16e8a-173">From hello [Azure Classic Portal](https://manage.windowsazure.com), select **NEW** > **Service Bus** > **Event Hub** > **Custom Create**.</span></span>

2. <span data-ttu-id="16e8a-174">Na hello **přidat nového centra událostí** obrazovky, zadejte **název centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="16e8a-174">On hello **Add a new Event Hub** screen, enter an **Event Hub Name**.</span></span> <span data-ttu-id="16e8a-175">Vyberte hello **oblast** toocreate, hello rozbočovače a pak vytvořit obor názvů nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="16e8a-175">Select hello **Region** toocreate hello hub in, and then create a namespace or select an existing one.</span></span> <span data-ttu-id="16e8a-176">Nakonec klikněte na hello **šipku** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="16e8a-176">Finally, click hello **Arrow** toocontinue.</span></span>

    ![Stránka 1 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > <span data-ttu-id="16e8a-178">Vyberte hello stejné **umístění** jako Storm v HDInsight server tooreduce latenci a náklady.</span><span class="sxs-lookup"><span data-stu-id="16e8a-178">Select hello same **Location** as your Storm on HDInsight server tooreduce latency and costs.</span></span>

3. <span data-ttu-id="16e8a-179">Na hello **Konfigurace centra událostí** obrazovky, zadejte hello **oddílu počet** a **uchování zpráv** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="16e8a-179">On hello **Configure Event Hub** screen, enter hello **Partition count** and **Message Retention** values.</span></span> <span data-ttu-id="16e8a-180">V tomto příkladu použijte počet oddílů 10 a uchování zpráv 1.</span><span class="sxs-lookup"><span data-stu-id="16e8a-180">For this example, use a partition count of 10 and a message retention of 1.</span></span> <span data-ttu-id="16e8a-181">Poznámka: počet oddílů hello později potřebovat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="16e8a-181">Note hello partition count because you need this value later.</span></span>

    ![Stránka 2 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. <span data-ttu-id="16e8a-183">Po centra událostí hello hello vytvořené, vyberte obor názvů, vyberte **Event Hubs**a potom vyberte hello centra událostí, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="16e8a-183">After hello event hub has been created, select hello namespace, select **Event Hubs**, and then select hello event hub that you created earlier.</span></span>
5. <span data-ttu-id="16e8a-184">Vyberte **konfigurace**, pak vytvořte dva nové zásady přístupu pomocí hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="16e8a-184">Select **Configure**, then create two new access policies by using hello following information:</span></span>

    <table>
    <tr><th><span data-ttu-id="16e8a-185">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="16e8a-185">Name</span></span></th><th><span data-ttu-id="16e8a-186">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="16e8a-186">Permissions</span></span></th></tr>
    <tr><td><span data-ttu-id="16e8a-187">Modul pro zápis</span><span class="sxs-lookup"><span data-stu-id="16e8a-187">Writer</span></span></td><td><span data-ttu-id="16e8a-188">Odeslat</span><span class="sxs-lookup"><span data-stu-id="16e8a-188">Send</span></span></td></tr>
    <tr><td><span data-ttu-id="16e8a-189">Čtenář</span><span class="sxs-lookup"><span data-stu-id="16e8a-189">Reader</span></span></td><td><span data-ttu-id="16e8a-190">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="16e8a-190">Listen</span></span></td></tr>
    </table>

    <span data-ttu-id="16e8a-191">Po vytvoření oprávnění hello vyberte hello **Uložit** ikonu v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="16e8a-191">After You create hello permissions, select hello **Save** icon at hello bottom of hello page.</span></span> <span data-ttu-id="16e8a-192">Tyto zásady sdíleného přístupu jsou použité tooread a zápis tooEvent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="16e8a-192">These shared access policies are used tooread and write tooEvent Hub.</span></span>

    ![zásady](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. <span data-ttu-id="16e8a-194">Po uložení hello zásad, pomocí hello **klíče generátor sdíleného přístupu** v dolní části hello hello stránky tooretrieve hello klíče pro hello **zapisovače** a **čtečky** zásady.</span><span class="sxs-lookup"><span data-stu-id="16e8a-194">After you save hello policies, use hello **Shared access key generator** at hello bottom of hello page tooretrieve hello key for hello **writer** and **reader** policies.</span></span> <span data-ttu-id="16e8a-195">Uložte tyto klíče.</span><span class="sxs-lookup"><span data-stu-id="16e8a-195">Save these keys.</span></span>

## <a name="download-and-build-hello-project"></a><span data-ttu-id="16e8a-196">Stáhněte si a sestavte projekt hello</span><span class="sxs-lookup"><span data-stu-id="16e8a-196">Download and build hello project</span></span>

1. <span data-ttu-id="16e8a-197">Stáhnout hello projektu z Githubu: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="16e8a-197">Download hello project from GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span></span> <span data-ttu-id="16e8a-198">Můžete stáhnout balíček hello jako archivu zip, nebo použijte [git](https://git-scm.com/) tooclone hello místně projektu.</span><span class="sxs-lookup"><span data-stu-id="16e8a-198">You can either download hello package as a zip archive, or use [git](https://git-scm.com/) tooclone hello project locally.</span></span>

2. <span data-ttu-id="16e8a-199">Upravit hello `dev.properties` soubor s hello konfiguraci pro Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="16e8a-199">Modify hello `dev.properties` file with hello configuration for your Event Hub.</span></span>

3. <span data-ttu-id="16e8a-200">Použijte následující toobuild a balíček projektu hello hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-200">Use hello following toobuild and package hello project:</span></span>

        mvn package

    <span data-ttu-id="16e8a-201">Tento příkaz stáhne požadované závislosti, sestavení, a pak balíčky hello projektu.</span><span class="sxs-lookup"><span data-stu-id="16e8a-201">This command downloads required dependencies, builds, and then packages hello project.</span></span> <span data-ttu-id="16e8a-202">výstup Hello je uložen v hello **/target** adresáři jako **EventHubExample. 1.0 SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="16e8a-202">hello output is stored in hello **/target** directory as **EventHubExample-1.0-SNAPSHOT.jar**.</span></span>

## <a name="test-locally"></a><span data-ttu-id="16e8a-203">Test místně</span><span class="sxs-lookup"><span data-stu-id="16e8a-203">Test locally</span></span>

<span data-ttu-id="16e8a-204">Vzhledem k tomu, že tyto topologie jenom číst a zapisovat tooEvent rozbočovače, je můžete otestovat místně, pokud máte [Storm vývojového prostředí](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="16e8a-204">Since these topologies just read and write tooEvent Hubs, you can test them locally if you have a [Storm development environment](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span></span> <span data-ttu-id="16e8a-205">Použijte následující postup toorun místně v prostředí dev hello hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-205">Use hello following steps toorun locally in hello dev environment:</span></span>

1. <span data-ttu-id="16e8a-206">Spusťte zapisovače hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-206">Run hello writer:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. <span data-ttu-id="16e8a-207">Spusťte čtečky hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-207">Run hello reader:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * <span data-ttu-id="16e8a-208">`--local`: Topologie spuštění hello v místním režimu (bez distribuované).</span><span class="sxs-lookup"><span data-stu-id="16e8a-208">`--local`: Run hello topology in local mode (non-distributed).</span></span>
> * <span data-ttu-id="16e8a-209">`-R /writer.yaml`: Načíst definici topologie hello z hello `resources` součástí hello jar.</span><span class="sxs-lookup"><span data-stu-id="16e8a-209">`-R /writer.yaml`: Load hello topology definition from hello `resources` packaged in hello jar.</span></span> <span data-ttu-id="16e8a-210">Pokud hello topologie je soubor na hello místního systému souborů, zadejte místo toho hello cesta tooit jako poslední parametr hello.</span><span class="sxs-lookup"><span data-stu-id="16e8a-210">If hello topology is a file on hello local file system, specify hello path tooit as hello last parameter instead.</span></span>
> * <span data-ttu-id="16e8a-211">`--filter dev.properties`: Použijte hello obsah `dev.properties` toofill hello hodnoty v definicích topologie hello.</span><span class="sxs-lookup"><span data-stu-id="16e8a-211">`--filter dev.properties`: Use hello contents of `dev.properties` toofill in hello values in hello topology definitions.</span></span> <span data-ttu-id="16e8a-212">Například, `${eventhub.read.policy.name}`.</span><span class="sxs-lookup"><span data-stu-id="16e8a-212">For example, `${eventhub.read.policy.name}`.</span></span>

<span data-ttu-id="16e8a-213">Výstup je konzola zaznamenané toohello při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="16e8a-213">Output is logged toohello console when running locally.</span></span> <span data-ttu-id="16e8a-214">Použití __Ctrl + C__ toostop hello topologie.</span><span class="sxs-lookup"><span data-stu-id="16e8a-214">Use __Ctrl+C__ toostop hello topology.</span></span>

## <a name="deploy-hello-topologies"></a><span data-ttu-id="16e8a-215">Hello topologie nasazení</span><span class="sxs-lookup"><span data-stu-id="16e8a-215">Deploy hello topologies</span></span>

1. <span data-ttu-id="16e8a-216">Použití clusteru HDInsight tooyour jar spojovací bod služby toocopy hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="16e8a-216">Use SCP toocopy hello jar package tooyour HDInsight cluster.</span></span> <span data-ttu-id="16e8a-217">Nahraďte uživatelské jméno uživatele hello SSH pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="16e8a-217">Replace USERNAME with hello SSH user for your cluster.</span></span> <span data-ttu-id="16e8a-218">Nahraďte název clusteru s názvem hello clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="16e8a-218">Replace CLUSTERNAME with hello name of your HDInsight cluster:</span></span>

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    <span data-ttu-id="16e8a-219">Pokud jste použili heslo pro účet SSH, jste výzvami tooenter hello heslo.</span><span class="sxs-lookup"><span data-stu-id="16e8a-219">If you used a password for your SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="16e8a-220">Pokud jste použili klíče SSH s účtem hello, může být nutné toouse hello `-i` parametr toospecify hello cestě toohello klíče souboru.</span><span class="sxs-lookup"><span data-stu-id="16e8a-220">If you used an SSH key with hello account, you may need toouse hello `-i` parameter toospecify hello path toohello key file.</span></span> <span data-ttu-id="16e8a-221">Například `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`.</span><span class="sxs-lookup"><span data-stu-id="16e8a-221">For example, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span></span>

    <span data-ttu-id="16e8a-222">Tento příkaz zkopíruje hello souboru toohello domovský adresář vaše uživatele SSH na hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="16e8a-222">This command copies hello file toohello home directory of your SSH user on hello cluster.</span></span>

2. <span data-ttu-id="16e8a-223">Po dokončení nahrávání souboru hello použijte clusteru HDInsight toohello tooconnect SSH.</span><span class="sxs-lookup"><span data-stu-id="16e8a-223">Once hello file has finished uploading, use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="16e8a-224">Nahraďte **uživatelské jméno** hello název vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="16e8a-224">Replace **USERNAME** hello name of your SSH login.</span></span> <span data-ttu-id="16e8a-225">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="16e8a-225">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > <span data-ttu-id="16e8a-226">Pokud jste použili heslo pro účet SSH, jste výzvami tooenter hello heslo.</span><span class="sxs-lookup"><span data-stu-id="16e8a-226">If you used a password for your SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="16e8a-227">Pokud jste použili klíče SSH s účtem hello, může být nutné toouse hello `-i` parametr toospecify hello cestě toohello klíče souboru.</span><span class="sxs-lookup"><span data-stu-id="16e8a-227">If you used an SSH key with hello account, you may need toouse hello `-i` parameter toospecify hello path toohello key file.</span></span> <span data-ttu-id="16e8a-228">Hello následující příklad načte privátní klíč hello z `~/.ssh/id_rsa`:</span><span class="sxs-lookup"><span data-stu-id="16e8a-228">hello following example loads hello private key from `~/.ssh/id_rsa`:</span></span>
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. <span data-ttu-id="16e8a-229">Použijte následující příkaz toostart hello topologie hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-229">Use hello following command toostart hello topologies:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * <span data-ttu-id="16e8a-230">`--remote`: Odešle hello topologie toohello Nimbus služba, která ji spustí na hello uzlů pracovního procesu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="16e8a-230">`--remote`: Submits hello topology toohello Nimbus service, which starts it on hello worker nodes in hello cluster.</span></span>

4. <span data-ttu-id="16e8a-231">tooview hello protokolovat data, přejděte toohttps://CLUSTERNAME.azurehdinsight.net/stormui, kde __CLUSTERNAME__ je hello název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="16e8a-231">tooview hello logged data, go toohttps://CLUSTERNAME.azurehdinsight.net/stormui, where __CLUSTERNAME__ is hello name of your HDInsight cluster.</span></span> <span data-ttu-id="16e8a-232">Vyberte hello topologie a podrobně toohello součásti.</span><span class="sxs-lookup"><span data-stu-id="16e8a-232">Select hello topologies and drill down toohello components.</span></span> <span data-ttu-id="16e8a-233">Vyberte hello __port__ položka pro instanci komponenty tooview protokolují informace.</span><span class="sxs-lookup"><span data-stu-id="16e8a-233">Select hello __port__ entry for an instance of a component tooview logged information.</span></span>

5. <span data-ttu-id="16e8a-234">Použijte následující příkazy toostop hello topologie hello:</span><span class="sxs-lookup"><span data-stu-id="16e8a-234">Use hello following commands toostop hello topologies:</span></span>

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a><span data-ttu-id="16e8a-235">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="16e8a-235">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="16e8a-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16e8a-236">Next steps</span></span>

* [<span data-ttu-id="16e8a-237">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="16e8a-237">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
