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
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Vývoj topologií Apache Storm v HDInsight používá Python

Zjistěte, jak toocreate Apache Storm topologie, která používá Python součásti. Apache Storm podporuje několik jazyků, i povolení toocombine součásti z několika jazyků v jedné topologii. Hello tok framework (zavedené Storm 0.10.0) vám umožní tooeasily vytvářet řešení, které použití komponent, Python.

> [!IMPORTANT]
> Hello informace v tomto dokumentu byla testována pomocí Storm v HDInsight 3.6. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Hello kód pro tento projekt je k dispozici na [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Požadavky

* Python 2.7 nebo vyšší

* Java JDK 1.8 nebo vyšší

* Maven 3

* (Volitelné) Místní Storm vývojové prostředí. Místní prostředí Storm je vyžadován, pouze pokud chcete, aby toorun hello místně topologie. Další informace najdete v tématu [nastavení vývojového prostředí](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>Podpora více jazyků Storm

Apache Storm se navrženou toowork s komponentami vytvořené pomocí žádný programovací jazyk. musíte pochopit součásti Hello jak toowork s hello [definici Thrift Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Modul je pro Python, k dispozici jako součást projektu hello Apache Storm, který vám umožní tooeasily rozhraní s Storm. Můžete najít na tento modul [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm je Java proces, který běží na hello Java Virtual Machine (JVM). Součásti, které jsou napsané v dalších jazycích jsou spouštěny jako podprocesů, které se. Hello Storm komunikuje se tyto podprocesů, které se pomocí JSON zprávy odeslané přes stdin/stdout. Další informace o komunikaci mezi součástmi naleznete v hello [více jazyků protokol](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentaci.

## <a name="python-with-hello-flux-framework"></a>Python s hello tok framework

Tok framework Hello umožňuje topologie Storm toodefine nezávisle na součásti hello. Tok framework Hello používá topologie Storm hello toodefine YAML. Hello následující text je příklad toho, jak tooreference komponentu Python v dokumentu YAML hello:

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

Hello třída `FluxShellSpout` je použité toostart hello `sentencespout.py` skript, který implementuje hello spout.

Tok očekává toobe skriptů Python hello hello `/resources` adresář uvnitř soubor jar hello, který obsahuje hello topologie. Proto tento příklad ukládá skriptů Python hello hello `/multilang/resources` adresáře. Hello `pom.xml` zahrnuje tento soubor pomocí hello následující XML:

```xml
<!-- include hello Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Jak už bylo zmíněno dříve, není `storm.py` soubor, který implementuje definici Thrift hello Storm. zahrnuje Hello tok framework `storm.py` automaticky při hello sestavení projektu, takže není nutné tooworry o včetně ho.

## <a name="build-hello-project"></a>Sestavení projektu hello

Hello kořenovém hello projektu můžete hello následující příkaz:

```bash
mvn clean compile package
```

Tento příkaz vytvoří `target/WordCount-1.0-SNAPSHOT.jar` soubor, který obsahuje hello zkompilovat topologie.

## <a name="run-hello-topology-locally"></a>Místní spuštění hello topologie

toorun hello topologie místně, použijte následující příkaz hello:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Tento příkaz vyžaduje místní vývojové prostředí Storm. Další informace najdete v tématu [nastavení vývojového prostředí](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)

Jednou hello topologie spustí, se vydá informace toohello místní konzoly podobné toohello následující text:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


toostop hello topologie, použijte __kombinaci kláves Ctrl + C__.

## <a name="run-hello-storm-topology-on-hdinsight"></a>Spustit hello topologie Storm v HDInsight

1. Použití hello následující příkaz toocopy hello `WordCount-1.0-SNAPSHOT.jar` souboru tooyour Storm v clusteru HDInsight:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Nahraďte `sshuser` hello uživatele SSH pro váš cluster. Nahraďte `mycluster` s názvem clusteru hello. Může být výzvami tooenter hello heslo pro uživatele SSH hello.

    Další informace o používání SSH a SCP najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Po odeslání hello souboru, připojte toohello clusteru pomocí protokolu SSH:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Z relace SSH hello použijte následující příkaz toostart hello topologie v clusteru hello hello:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. Můžete vytvořit hello uživatelské rozhraní Storm tooview hello topologie hello clusteru. Hello uživatelské rozhraní Storm je umístěn v https://mycluster.azurehdinsight.net/stormui. Nahraďte `mycluster` se název clusteru.

> [!NOTE]
> Po spuštění, topologie Storm spustí, dokud nebude zastaven. toostop hello topologie, použijte jednu z následujících metod hello:
>
> * Hello `storm kill TOPOLOGYNAME` příkazu z příkazového řádku hello
> * Hello **Kill** tlačítka na hello uživatelské rozhraní Storm.


## <a name="next-steps"></a>Další kroky

V tématu hello následující dokumenty, kde najdete další způsoby toouse Python s HDInsight:

* [Jak toouse Python pro streamování úloh MapReduce](hdinsight-hadoop-streaming-python.md)
* [Jak toouse Python uživatele definované funkce (UDF) v Pig a Hive](hdinsight-python.md)
