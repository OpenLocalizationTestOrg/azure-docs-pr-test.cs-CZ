---
title: Apache Storm s comopnents Python - Azure HDInsight | Microsoft Docs
description: "Naučte se vytvářet topologie Apache Storm, která používá Python součásti."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
keywords: Apache storm python
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: 305c4060ad81458b254e66a4bad6dfd7bf69b28d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a><span data-ttu-id="4cf64-104">Vývoj topologií Apache Storm v HDInsight používá Python</span><span class="sxs-lookup"><span data-stu-id="4cf64-104">Develop Apache Storm topologies using Python on HDInsight</span></span>

<span data-ttu-id="4cf64-105">Naučte se vytvářet topologie Apache Storm, která používá Python součásti.</span><span class="sxs-lookup"><span data-stu-id="4cf64-105">Learn how to create an Apache Storm topology that uses Python components.</span></span> <span data-ttu-id="4cf64-106">Apache Storm podporuje několik jazyků, i umožňuje zkombinovat součásti z několika jazyků v jedné topologii.</span><span class="sxs-lookup"><span data-stu-id="4cf64-106">Apache Storm supports multiple languages, even allowing you to combine components from several languages in one topology.</span></span> <span data-ttu-id="4cf64-107">Rozhraní framework tok (zavedené Storm 0.10.0) umožňuje snadno vytvářet řešení, která používají Python součásti.</span><span class="sxs-lookup"><span data-stu-id="4cf64-107">The Flux framework (introduced with Storm 0.10.0) allows you to easily create solutions that use Python components.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cf64-108">Informace v tomto dokumentu byla testována pomocí Storm v HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="4cf64-108">The information in this document was tested using Storm on HDInsight 3.6.</span></span> <span data-ttu-id="4cf64-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="4cf64-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4cf64-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4cf64-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="4cf64-111">Kód pro tento projekt je k dispozici na [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="4cf64-111">The code for this project is available at [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4cf64-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4cf64-112">Prerequisites</span></span>

* <span data-ttu-id="4cf64-113">Python 2.7 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="4cf64-113">Python 2.7 or higher</span></span>

* <span data-ttu-id="4cf64-114">Java JDK 1.8 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="4cf64-114">Java JDK 1.8 or higher</span></span>

* <span data-ttu-id="4cf64-115">Maven 3</span><span class="sxs-lookup"><span data-stu-id="4cf64-115">Maven 3</span></span>

* <span data-ttu-id="4cf64-116">(Volitelné) Místní Storm vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="4cf64-116">(Optional) A local Storm development environment.</span></span> <span data-ttu-id="4cf64-117">Místní prostředí Storm je vyžadován, pouze pokud chcete spustit topologii místně.</span><span class="sxs-lookup"><span data-stu-id="4cf64-117">A local Storm environment is only needed if you want to run the topology locally.</span></span> <span data-ttu-id="4cf64-118">Další informace najdete v tématu [nastavení vývojového prostředí](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="4cf64-118">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span></span>

## <a name="storm-multi-language-support"></a><span data-ttu-id="4cf64-119">Podpora více jazyků Storm</span><span class="sxs-lookup"><span data-stu-id="4cf64-119">Storm multi-language support</span></span>

<span data-ttu-id="4cf64-120">Apache Storm je navržené pro práci s součásti, které jsou napsané v žádný programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="4cf64-120">Apache Storm was designed to work with components written using any programming language.</span></span> <span data-ttu-id="4cf64-121">Součásti musíte pochopit, jak pracovat [definici Thrift Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span><span class="sxs-lookup"><span data-stu-id="4cf64-121">The components must understand how to work with the [Thrift definition for Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span></span> <span data-ttu-id="4cf64-122">Pro Python se v rámci Apache Storm projekt, který umožňuje snadno rozhraní s Storm poskytuje modul.</span><span class="sxs-lookup"><span data-stu-id="4cf64-122">For Python, a module is provided as part of the Apache Storm project that allows you to easily interface with Storm.</span></span> <span data-ttu-id="4cf64-123">Můžete najít na tento modul [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span><span class="sxs-lookup"><span data-stu-id="4cf64-123">You can find this module at [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span></span>

<span data-ttu-id="4cf64-124">Storm je Java proces, který běží na virtuálním počítači Java (JVM).</span><span class="sxs-lookup"><span data-stu-id="4cf64-124">Storm is a Java process that runs on the Java Virtual Machine (JVM).</span></span> <span data-ttu-id="4cf64-125">Součásti, které jsou napsané v dalších jazycích jsou spouštěny jako podprocesů, které se.</span><span class="sxs-lookup"><span data-stu-id="4cf64-125">Components written in other languages are executed as subprocesses.</span></span> <span data-ttu-id="4cf64-126">Storm komunikuje se tyto podprocesů, které se pomocí JSON zprávy odeslané přes stdin/stdout.</span><span class="sxs-lookup"><span data-stu-id="4cf64-126">The Storm communicates with these subprocesses using JSON messages sent over stdin/stdout.</span></span> <span data-ttu-id="4cf64-127">Další informace o komunikaci mezi součástmi najdete v [více jazyků protokol](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4cf64-127">More details on communication between components can be found in the [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) documentation.</span></span>

## <a name="python-with-the-flux-framework"></a><span data-ttu-id="4cf64-128">Python s tokem framework</span><span class="sxs-lookup"><span data-stu-id="4cf64-128">Python with the Flux framework</span></span>

<span data-ttu-id="4cf64-129">Rozhraní framework tok umožňuje definovat topologie Storm nezávisle na součásti.</span><span class="sxs-lookup"><span data-stu-id="4cf64-129">The Flux framework allows you to define Storm topologies separately from the components.</span></span> <span data-ttu-id="4cf64-130">Rozhraní framework tok YAML používá k definování topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="4cf64-130">The Flux framework uses YAML to define the Storm topology.</span></span> <span data-ttu-id="4cf64-131">Tento text je příklad toho, jak odkazovat na komponentu Python v dokumentu YAML:</span><span class="sxs-lookup"><span data-stu-id="4cf64-131">The following text is an example of how to reference a Python component in the YAML document:</span></span>

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

<span data-ttu-id="4cf64-132">Třída `FluxShellSpout` se používá ke spuštění `sentencespout.py` skript, který implementuje funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="4cf64-132">The class `FluxShellSpout` is used to start the `sentencespout.py` script that implements the spout.</span></span>

<span data-ttu-id="4cf64-133">Tok očekává skriptů Python ve `/resources` adresář uvnitř soubor jar obsahující topologii.</span><span class="sxs-lookup"><span data-stu-id="4cf64-133">Flux expects the Python scripts to be in the `/resources` directory inside the jar file that contains the topology.</span></span> <span data-ttu-id="4cf64-134">Takže ukládá skriptů Python v tomto příkladu `/multilang/resources` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4cf64-134">So this example stores the Python scripts in the `/multilang/resources` directory.</span></span> <span data-ttu-id="4cf64-135">`pom.xml` Zahrnuje tento soubor pomocí následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="4cf64-135">The `pom.xml` includes this file using the following XML:</span></span>

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

<span data-ttu-id="4cf64-136">Jak už bylo zmíněno dříve, není `storm.py` soubor, který implementuje definici Thrift Storm.</span><span class="sxs-lookup"><span data-stu-id="4cf64-136">As mentioned earlier, there is a `storm.py` file that implements the Thrift definition for Storm.</span></span> <span data-ttu-id="4cf64-137">Zahrnuje rozhraní tok `storm.py` automaticky při sestavení projektu, takže není nutné se obávat, včetně ho.</span><span class="sxs-lookup"><span data-stu-id="4cf64-137">The Flux framework includes `storm.py` automatically when the project is built, so you don't have to worry about including it.</span></span>

## <a name="build-the-project"></a><span data-ttu-id="4cf64-138">Sestavení projektu</span><span class="sxs-lookup"><span data-stu-id="4cf64-138">Build the project</span></span>

<span data-ttu-id="4cf64-139">Z kořenového adresáře projektu použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4cf64-139">From the root of the project, use the following command:</span></span>

```bash
mvn clean compile package
```

<span data-ttu-id="4cf64-140">Tento příkaz vytvoří `target/WordCount-1.0-SNAPSHOT.jar` soubor, který obsahuje kompilované topologie.</span><span class="sxs-lookup"><span data-stu-id="4cf64-140">This command creates a `target/WordCount-1.0-SNAPSHOT.jar` file that contains the compiled topology.</span></span>

## <a name="run-the-topology-locally"></a><span data-ttu-id="4cf64-141">Místní spuštění topologie</span><span class="sxs-lookup"><span data-stu-id="4cf64-141">Run the topology locally</span></span>

<span data-ttu-id="4cf64-142">Pokud chcete spustit topologii místně, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4cf64-142">To run the topology locally, use the following command:</span></span>

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> <span data-ttu-id="4cf64-143">Tento příkaz vyžaduje místní vývojové prostředí Storm.</span><span class="sxs-lookup"><span data-stu-id="4cf64-143">This command requires a local Storm development environment.</span></span> <span data-ttu-id="4cf64-144">Další informace najdete v tématu [nastavení vývojového prostředí](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span><span class="sxs-lookup"><span data-stu-id="4cf64-144">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span></span>

<span data-ttu-id="4cf64-145">Jednou spuštěním topologie, se vydá informace do místní konzoly podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4cf64-145">Once the topology starts, it emits information to the local console similar to the following text:</span></span>


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


<span data-ttu-id="4cf64-146">Chcete-li zastavit topologii, použijte __kombinaci kláves Ctrl + C__.</span><span class="sxs-lookup"><span data-stu-id="4cf64-146">To stop the topology, use __Ctrl + C__.</span></span>

## <a name="run-the-storm-topology-on-hdinsight"></a><span data-ttu-id="4cf64-147">Spustit topologie Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="4cf64-147">Run the Storm topology on HDInsight</span></span>

1. <span data-ttu-id="4cf64-148">Použijte následující příkaz pro kopírování `WordCount-1.0-SNAPSHOT.jar` soubor Storm na clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="4cf64-148">Use the following command to copy the `WordCount-1.0-SNAPSHOT.jar` file to your Storm on HDInsight cluster:</span></span>

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="4cf64-149">Nahraďte `sshuser` s uživatelem SSH pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="4cf64-149">Replace `sshuser` with the SSH user for your cluster.</span></span> <span data-ttu-id="4cf64-150">Nahraďte `mycluster` se název clusteru.</span><span class="sxs-lookup"><span data-stu-id="4cf64-150">Replace `mycluster` with the cluster name.</span></span> <span data-ttu-id="4cf64-151">Můžete být vyzváni k zadání hesla pro uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="4cf64-151">You may be prompted to enter the password for the SSH user.</span></span>

    <span data-ttu-id="4cf64-152">Další informace o používání SSH a SCP najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4cf64-152">For more information on using SSH and SCP, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4cf64-153">Jakmile soubor byl odeslán, připojte se ke clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="4cf64-153">Once the file has been uploaded, connect to the cluster using SSH:</span></span>

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="4cf64-154">Z relace SSH použijte následující příkaz v clusteru spusťte topologie:</span><span class="sxs-lookup"><span data-stu-id="4cf64-154">From the SSH session, use the following command to start the topology on the cluster:</span></span>

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. <span data-ttu-id="4cf64-155">Uživatelské rozhraní Storm můžete použít k zobrazení topologii v clusteru.</span><span class="sxs-lookup"><span data-stu-id="4cf64-155">You can use the Storm UI to view the topology on the cluster.</span></span> <span data-ttu-id="4cf64-156">Uživatelské rozhraní Storm je umístěn v https://mycluster.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="4cf64-156">The Storm UI is located at https://mycluster.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="4cf64-157">Nahraďte `mycluster` se název clusteru.</span><span class="sxs-lookup"><span data-stu-id="4cf64-157">Replace `mycluster` with your cluster name.</span></span>

> [!NOTE]
> <span data-ttu-id="4cf64-158">Po spuštění, topologie Storm spustí, dokud nebude zastaven.</span><span class="sxs-lookup"><span data-stu-id="4cf64-158">Once started, a Storm topology runs until stopped.</span></span> <span data-ttu-id="4cf64-159">Zastavení topologie, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="4cf64-159">To stop the topology, use one of the following methods:</span></span>
>
> * <span data-ttu-id="4cf64-160">`storm kill TOPOLOGYNAME` Příkazu z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4cf64-160">The `storm kill TOPOLOGYNAME` command from the command line</span></span>
> * <span data-ttu-id="4cf64-161">**Kill** tlačítko v uživatelském rozhraní Storm.</span><span class="sxs-lookup"><span data-stu-id="4cf64-161">The **Kill** button in the Storm UI.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4cf64-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cf64-162">Next steps</span></span>

<span data-ttu-id="4cf64-163">Najdete v následujících dokumentech pro další způsoby použití Python s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="4cf64-163">See the following documents for other ways to use Python with HDInsight:</span></span>

* [<span data-ttu-id="4cf64-164">Jak používat Python pro streamování úloh MapReduce</span><span class="sxs-lookup"><span data-stu-id="4cf64-164">How to use Python for streaming MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)
* [<span data-ttu-id="4cf64-165">Jak používat Python uživatele definované funkce (UDF) v Pig a Hive</span><span class="sxs-lookup"><span data-stu-id="4cf64-165">How to use Python User Defined Functions (UDF) in Pig and Hive</span></span>](hdinsight-python.md)
