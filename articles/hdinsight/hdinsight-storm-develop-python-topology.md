---
title: aaaApache Storm s comopnents Python - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toocreate Apache Storm topologie, která používá Python součásti."
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
ms.openlocfilehash: 143c639623f1992f913900a7c52d6e3f03c701e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a><span data-ttu-id="2a910-104">Vývoj topologií Apache Storm v HDInsight používá Python</span><span class="sxs-lookup"><span data-stu-id="2a910-104">Develop Apache Storm topologies using Python on HDInsight</span></span>

<span data-ttu-id="2a910-105">Zjistěte, jak toocreate Apache Storm topologie, která používá Python součásti.</span><span class="sxs-lookup"><span data-stu-id="2a910-105">Learn how toocreate an Apache Storm topology that uses Python components.</span></span> <span data-ttu-id="2a910-106">Apache Storm podporuje několik jazyků, i povolení toocombine součásti z několika jazyků v jedné topologii.</span><span class="sxs-lookup"><span data-stu-id="2a910-106">Apache Storm supports multiple languages, even allowing you toocombine components from several languages in one topology.</span></span> <span data-ttu-id="2a910-107">Hello tok framework (zavedené Storm 0.10.0) vám umožní tooeasily vytvářet řešení, které použití komponent, Python.</span><span class="sxs-lookup"><span data-stu-id="2a910-107">hello Flux framework (introduced with Storm 0.10.0) allows you tooeasily create solutions that use Python components.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a910-108">Hello informace v tomto dokumentu byla testována pomocí Storm v HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="2a910-108">hello information in this document was tested using Storm on HDInsight 3.6.</span></span> <span data-ttu-id="2a910-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2a910-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2a910-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2a910-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="2a910-111">Hello kód pro tento projekt je k dispozici na [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="2a910-111">hello code for this project is available at [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a910-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a910-112">Prerequisites</span></span>

* <span data-ttu-id="2a910-113">Python 2.7 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="2a910-113">Python 2.7 or higher</span></span>

* <span data-ttu-id="2a910-114">Java JDK 1.8 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="2a910-114">Java JDK 1.8 or higher</span></span>

* <span data-ttu-id="2a910-115">Maven 3</span><span class="sxs-lookup"><span data-stu-id="2a910-115">Maven 3</span></span>

* <span data-ttu-id="2a910-116">(Volitelné) Místní Storm vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a910-116">(Optional) A local Storm development environment.</span></span> <span data-ttu-id="2a910-117">Místní prostředí Storm je vyžadován, pouze pokud chcete, aby toorun hello místně topologie.</span><span class="sxs-lookup"><span data-stu-id="2a910-117">A local Storm environment is only needed if you want toorun hello topology locally.</span></span> <span data-ttu-id="2a910-118">Další informace najdete v tématu [nastavení vývojového prostředí](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="2a910-118">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span></span>

## <a name="storm-multi-language-support"></a><span data-ttu-id="2a910-119">Podpora více jazyků Storm</span><span class="sxs-lookup"><span data-stu-id="2a910-119">Storm multi-language support</span></span>

<span data-ttu-id="2a910-120">Apache Storm se navrženou toowork s komponentami vytvořené pomocí žádný programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="2a910-120">Apache Storm was designed toowork with components written using any programming language.</span></span> <span data-ttu-id="2a910-121">musíte pochopit součásti Hello jak toowork s hello [definici Thrift Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span><span class="sxs-lookup"><span data-stu-id="2a910-121">hello components must understand how toowork with hello [Thrift definition for Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span></span> <span data-ttu-id="2a910-122">Modul je pro Python, k dispozici jako součást projektu hello Apache Storm, který vám umožní tooeasily rozhraní s Storm.</span><span class="sxs-lookup"><span data-stu-id="2a910-122">For Python, a module is provided as part of hello Apache Storm project that allows you tooeasily interface with Storm.</span></span> <span data-ttu-id="2a910-123">Můžete najít na tento modul [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span><span class="sxs-lookup"><span data-stu-id="2a910-123">You can find this module at [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span></span>

<span data-ttu-id="2a910-124">Storm je Java proces, který běží na hello Java Virtual Machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="2a910-124">Storm is a Java process that runs on hello Java Virtual Machine (JVM).</span></span> <span data-ttu-id="2a910-125">Součásti, které jsou napsané v dalších jazycích jsou spouštěny jako podprocesů, které se.</span><span class="sxs-lookup"><span data-stu-id="2a910-125">Components written in other languages are executed as subprocesses.</span></span> <span data-ttu-id="2a910-126">Hello Storm komunikuje se tyto podprocesů, které se pomocí JSON zprávy odeslané přes stdin/stdout.</span><span class="sxs-lookup"><span data-stu-id="2a910-126">hello Storm communicates with these subprocesses using JSON messages sent over stdin/stdout.</span></span> <span data-ttu-id="2a910-127">Další informace o komunikaci mezi součástmi naleznete v hello [více jazyků protokol](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="2a910-127">More details on communication between components can be found in hello [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) documentation.</span></span>

## <a name="python-with-hello-flux-framework"></a><span data-ttu-id="2a910-128">Python s hello tok framework</span><span class="sxs-lookup"><span data-stu-id="2a910-128">Python with hello Flux framework</span></span>

<span data-ttu-id="2a910-129">Tok framework Hello umožňuje topologie Storm toodefine nezávisle na součásti hello.</span><span class="sxs-lookup"><span data-stu-id="2a910-129">hello Flux framework allows you toodefine Storm topologies separately from hello components.</span></span> <span data-ttu-id="2a910-130">Tok framework Hello používá topologie Storm hello toodefine YAML.</span><span class="sxs-lookup"><span data-stu-id="2a910-130">hello Flux framework uses YAML toodefine hello Storm topology.</span></span> <span data-ttu-id="2a910-131">Hello následující text je příklad toho, jak tooreference komponentu Python v dokumentu YAML hello:</span><span class="sxs-lookup"><span data-stu-id="2a910-131">hello following text is an example of how tooreference a Python component in hello YAML document:</span></span>

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

<span data-ttu-id="2a910-132">Hello třída `FluxShellSpout` je použité toostart hello `sentencespout.py` skript, který implementuje hello spout.</span><span class="sxs-lookup"><span data-stu-id="2a910-132">hello class `FluxShellSpout` is used toostart hello `sentencespout.py` script that implements hello spout.</span></span>

<span data-ttu-id="2a910-133">Tok očekává toobe skriptů Python hello hello `/resources` adresář uvnitř soubor jar hello, který obsahuje hello topologie.</span><span class="sxs-lookup"><span data-stu-id="2a910-133">Flux expects hello Python scripts toobe in hello `/resources` directory inside hello jar file that contains hello topology.</span></span> <span data-ttu-id="2a910-134">Proto tento příklad ukládá skriptů Python hello hello `/multilang/resources` adresáře.</span><span class="sxs-lookup"><span data-stu-id="2a910-134">So this example stores hello Python scripts in hello `/multilang/resources` directory.</span></span> <span data-ttu-id="2a910-135">Hello `pom.xml` zahrnuje tento soubor pomocí hello následující XML:</span><span class="sxs-lookup"><span data-stu-id="2a910-135">hello `pom.xml` includes this file using hello following XML:</span></span>

```xml
<!-- include hello Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

<span data-ttu-id="2a910-136">Jak už bylo zmíněno dříve, není `storm.py` soubor, který implementuje definici Thrift hello Storm.</span><span class="sxs-lookup"><span data-stu-id="2a910-136">As mentioned earlier, there is a `storm.py` file that implements hello Thrift definition for Storm.</span></span> <span data-ttu-id="2a910-137">zahrnuje Hello tok framework `storm.py` automaticky při hello sestavení projektu, takže není nutné tooworry o včetně ho.</span><span class="sxs-lookup"><span data-stu-id="2a910-137">hello Flux framework includes `storm.py` automatically when hello project is built, so you don't have tooworry about including it.</span></span>

## <a name="build-hello-project"></a><span data-ttu-id="2a910-138">Sestavení projektu hello</span><span class="sxs-lookup"><span data-stu-id="2a910-138">Build hello project</span></span>

<span data-ttu-id="2a910-139">Hello kořenovém hello projektu můžete hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a910-139">From hello root of hello project, use hello following command:</span></span>

```bash
mvn clean compile package
```

<span data-ttu-id="2a910-140">Tento příkaz vytvoří `target/WordCount-1.0-SNAPSHOT.jar` soubor, který obsahuje hello zkompilovat topologie.</span><span class="sxs-lookup"><span data-stu-id="2a910-140">This command creates a `target/WordCount-1.0-SNAPSHOT.jar` file that contains hello compiled topology.</span></span>

## <a name="run-hello-topology-locally"></a><span data-ttu-id="2a910-141">Místní spuštění hello topologie</span><span class="sxs-lookup"><span data-stu-id="2a910-141">Run hello topology locally</span></span>

<span data-ttu-id="2a910-142">toorun hello topologie místně, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2a910-142">toorun hello topology locally, use hello following command:</span></span>

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> <span data-ttu-id="2a910-143">Tento příkaz vyžaduje místní vývojové prostředí Storm.</span><span class="sxs-lookup"><span data-stu-id="2a910-143">This command requires a local Storm development environment.</span></span> <span data-ttu-id="2a910-144">Další informace najdete v tématu [nastavení vývojového prostředí](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span><span class="sxs-lookup"><span data-stu-id="2a910-144">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span></span>

<span data-ttu-id="2a910-145">Jednou hello topologie spustí, se vydá informace toohello místní konzoly podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="2a910-145">Once hello topology starts, it emits information toohello local console similar toohello following text:</span></span>


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


<span data-ttu-id="2a910-146">toostop hello topologie, použijte __kombinaci kláves Ctrl + C__.</span><span class="sxs-lookup"><span data-stu-id="2a910-146">toostop hello topology, use __Ctrl + C__.</span></span>

## <a name="run-hello-storm-topology-on-hdinsight"></a><span data-ttu-id="2a910-147">Spustit hello topologie Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="2a910-147">Run hello Storm topology on HDInsight</span></span>

1. <span data-ttu-id="2a910-148">Použití hello následující příkaz toocopy hello `WordCount-1.0-SNAPSHOT.jar` souboru tooyour Storm v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="2a910-148">Use hello following command toocopy hello `WordCount-1.0-SNAPSHOT.jar` file tooyour Storm on HDInsight cluster:</span></span>

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="2a910-149">Nahraďte `sshuser` hello uživatele SSH pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="2a910-149">Replace `sshuser` with hello SSH user for your cluster.</span></span> <span data-ttu-id="2a910-150">Nahraďte `mycluster` s názvem clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="2a910-150">Replace `mycluster` with hello cluster name.</span></span> <span data-ttu-id="2a910-151">Může být výzvami tooenter hello heslo pro uživatele SSH hello.</span><span class="sxs-lookup"><span data-stu-id="2a910-151">You may be prompted tooenter hello password for hello SSH user.</span></span>

    <span data-ttu-id="2a910-152">Další informace o používání SSH a SCP najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2a910-152">For more information on using SSH and SCP, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="2a910-153">Po odeslání hello souboru, připojte toohello clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="2a910-153">Once hello file has been uploaded, connect toohello cluster using SSH:</span></span>

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="2a910-154">Z relace SSH hello použijte následující příkaz toostart hello topologie v clusteru hello hello:</span><span class="sxs-lookup"><span data-stu-id="2a910-154">From hello SSH session, use hello following command toostart hello topology on hello cluster:</span></span>

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. <span data-ttu-id="2a910-155">Můžete vytvořit hello uživatelské rozhraní Storm tooview hello topologie hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="2a910-155">You can use hello Storm UI tooview hello topology on hello cluster.</span></span> <span data-ttu-id="2a910-156">Hello uživatelské rozhraní Storm je umístěn v https://mycluster.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="2a910-156">hello Storm UI is located at https://mycluster.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="2a910-157">Nahraďte `mycluster` se název clusteru.</span><span class="sxs-lookup"><span data-stu-id="2a910-157">Replace `mycluster` with your cluster name.</span></span>

> [!NOTE]
> <span data-ttu-id="2a910-158">Po spuštění, topologie Storm spustí, dokud nebude zastaven.</span><span class="sxs-lookup"><span data-stu-id="2a910-158">Once started, a Storm topology runs until stopped.</span></span> <span data-ttu-id="2a910-159">toostop hello topologie, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="2a910-159">toostop hello topology, use one of hello following methods:</span></span>
>
> * <span data-ttu-id="2a910-160">Hello `storm kill TOPOLOGYNAME` příkazu z příkazového řádku hello</span><span class="sxs-lookup"><span data-stu-id="2a910-160">hello `storm kill TOPOLOGYNAME` command from hello command line</span></span>
> * <span data-ttu-id="2a910-161">Hello **Kill** tlačítka na hello uživatelské rozhraní Storm.</span><span class="sxs-lookup"><span data-stu-id="2a910-161">hello **Kill** button in hello Storm UI.</span></span>


## <a name="next-steps"></a><span data-ttu-id="2a910-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a910-162">Next steps</span></span>

<span data-ttu-id="2a910-163">V tématu hello následující dokumenty, kde najdete další způsoby toouse Python s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="2a910-163">See hello following documents for other ways toouse Python with HDInsight:</span></span>

* [<span data-ttu-id="2a910-164">Jak toouse Python pro streamování úloh MapReduce</span><span class="sxs-lookup"><span data-stu-id="2a910-164">How toouse Python for streaming MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)
* [<span data-ttu-id="2a910-165">Jak toouse Python uživatele definované funkce (UDF) v Pig a Hive</span><span class="sxs-lookup"><span data-stu-id="2a910-165">How toouse Python User Defined Functions (UDF) in Pig and Hive</span></span>](hdinsight-python.md)
