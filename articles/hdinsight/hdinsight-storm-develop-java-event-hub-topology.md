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
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)

Zjistěte, jak toouse Azure Event Hubs se Storm v HDInsight. Tento příklad používá založené na jazyce Java součásti tooread a zápis dat v Azure Event Hubs.

Azure Event Hubs vám umožní tooprocess masivní objemy dat z webů, aplikací a zařízení. Hello Event Hub spout umožňuje snadno toouse Apache Storm v HDInsight tooanalyze tato data v reálném čase. Je také možné zapsat tooEvent datového centra z Storm pomocí hello funkce bolt Event Hubs.

## <a name="prerequisites"></a>Požadavky

* Apache Storm na verzi clusteru HDInsight 3.6. Další informace najdete v tématu [Začínáme se Storm v clusteru HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

    > [!IMPORTANT]
    > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* [Oracle Java Developer Kit (JDK) verze 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo stejná, jako třeba [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.

* Textového editoru nebo integrované vývojové prostředí (IDE).

    > [!NOTE]
    > Editor nebo IDE může mít specifické funkce pro práci s Maven, který mu není adresovaný v tomto dokumentu. Informace o možnostech hello prostředí pro úpravy naleznete v dokumentaci k hello hello produktu, který používáte.

    * Klientem SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* Hello `ssh` a `scp` příkazy. Jedná se o clusteru HDInsight toohello použité toocopy soubory. V systému Windows můžete získat tyto prostřednictvím Bash ve Windows 10.

## <a name="understanding-hello-example"></a>Principy hello – ukázka

Hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) příklad obsahuje dvě topologie:

Hello `resources/writer.yaml` topologie zapíše náhodná data tooan centra událostí Azure. Hello data je generována hello `DeviceSpout` součást, a je ID náhodných zařízení a zařízení hodnota. Proto se simuluje některé hardwaru, který vysílá řetězec ID a číselná hodnota.

Thí `resources/reader.yaml` topologie čte data z centra událostí (služba EventHubWriter, zapíše data hello) analyzuje hello JSON data a pak protokoly hello `deviceId` a `deviceValue` data.

formátování dat Hello jako dokument JSON předtím, než je zapsána tooEvent rozbočovače a když číst hello čtečky ho je analyzována z JSON a do řazené kolekce členů. Formát JSON Hello vypadá takto:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Konfigurace projektu

Hello `POM.xml` soubor obsahuje informace o konfiguraci pro tento projekt Maven. zajímavé částí Hello jsou:

#### <a name="event-hub-components"></a>Součásti centra událostí

Hello komponenta, která čte a zapisuje tooAzure Event Hubs se nachází v hello [úložiště HDInsight](https://github.com/hdinsight/mvn-rep). Následující části hello Hello `POM.xml` zatížení hello komponenty soubory z tohoto úložiště

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="hello-eventhubs-storm-spout-dependency"></a>Hello EventHubs Storm Spout závislostí

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

Tato konfigurace xml definuje závislost pro hello eventhubs balíček, který obsahuje spout pro čtení ze služby Event Hubs i na funkce bolt pro zápis tooit.

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Tato konfigurace xml nakonfiguruje toogenerate výstup hello projektu Java 8, které se používá v HDInsight 3.5 nebo vyšší.

#### <a name="hello-maven-shade-plugin"></a>Hello maven stín-modulu plug-in

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

Tato konfigurace xml nakonfiguruje hello řešení toopackage hello výstup do uber jar. Hello jar obsahuje kód projektu hello a požadované závislosti. Používá se také na:

* Přejmenování souborů s licencí pro hello závislosti.
* Vylučte zabezpečení nebo podpisy.
* Ujistěte se, že více implementace hello stejné rozhraní jsou sloučeny do jednu položku.

Tato nastavení konfigurace zabránit chybám za běhu.

#### <a name="topology-definitions"></a>Definice topologie

Tento příklad používá hello [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework. Toto rozhraní využívá YAML toodefine hello topologie. Hello Primární výhodou je, že nejste pevné kódování hello topologie v kódu v jazyce Java. Vzhledem k tomu, že definice hello je YAML, můžete ji změnit před odesláním hello topologie, bez nutnosti toorecompile vše.

__Writer.yaml__:

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

__Reader.yaml__:

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

#### <a name="tell-hello-topology-about-event-hub"></a>Řekněte hello topologie o centra událostí

V době běhu hello `dev.properties` soubor je použité toopass hello centra událostí konfigurace toohello topologie. Hello následující příklad je hello výchozí obsah souboru hello:

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a>Nakonfigurujte proměnné prostředí

Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK na pracovní stanici. Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.

* **JAVA_HOME** -by měla odkazovat toohello adresáře, kde je nainstalován hello prostředí Java runtime (JRE). Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou příliš`/usr/lib/jvm/java-7-oracle`. Windows neměl by mít hodnotu podobnou příliš`c:\Program Files (x86)\Java\jre1.7`
* **CESTA** -musí obsahovat hello následující cesty:

  * **JAVA_HOME** (nebo ekvivalentní cesta hello)
  * **JAVA_HOME\bin** (nebo ekvivalentní cesta hello)
  * Hello adresáře, kde je nainstalován Maven

## <a name="configure-event-hub"></a>Konfigurace centra událostí

Event Hubs je hello zdroj dat pro tento příklad. Pomocí následujících kroků toocreate centra událostí hello.

1. Z hello [portálu Azure Classic](https://manage.windowsazure.com), vyberte **nový** > **Service Bus** > **centra událostí**  >  **Vytvořit vlastní**.

2. Na hello **přidat nového centra událostí** obrazovky, zadejte **název centra událostí**. Vyberte hello **oblast** toocreate, hello rozbočovače a pak vytvořit obor názvů nebo vyberte nějaký existující. Nakonec klikněte na hello **šipku** toocontinue.

    ![Stránka 1 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > Vyberte hello stejné **umístění** jako Storm v HDInsight server tooreduce latenci a náklady.

3. Na hello **Konfigurace centra událostí** obrazovky, zadejte hello **oddílu počet** a **uchování zpráv** hodnoty. V tomto příkladu použijte počet oddílů 10 a uchování zpráv 1. Poznámka: počet oddílů hello později potřebovat tuto hodnotu.

    ![Stránka 2 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. Po centra událostí hello hello vytvořené, vyberte obor názvů, vyberte **Event Hubs**a potom vyberte hello centra událostí, který jste vytvořili dříve.
5. Vyberte **konfigurace**, pak vytvořte dva nové zásady přístupu pomocí hello následující informace:

    <table>
    <tr><th>Name (Název)</th><th>Oprávnění</th></tr>
    <tr><td>Modul pro zápis</td><td>Odeslat</td></tr>
    <tr><td>Čtenář</td><td>Naslouchání</td></tr>
    </table>

    Po vytvoření oprávnění hello vyberte hello **Uložit** ikonu v hello dolní části stránky hello. Tyto zásady sdíleného přístupu jsou použité tooread a zápis tooEvent rozbočovače.

    ![zásady](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. Po uložení hello zásad, pomocí hello **klíče generátor sdíleného přístupu** v dolní části hello hello stránky tooretrieve hello klíče pro hello **zapisovače** a **čtečky** zásady. Uložte tyto klíče.

## <a name="download-and-build-hello-project"></a>Stáhněte si a sestavte projekt hello

1. Stáhnout hello projektu z Githubu: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub). Můžete stáhnout balíček hello jako archivu zip, nebo použijte [git](https://git-scm.com/) tooclone hello místně projektu.

2. Upravit hello `dev.properties` soubor s hello konfiguraci pro Centrum událostí.

3. Použijte následující toobuild a balíček projektu hello hello:

        mvn package

    Tento příkaz stáhne požadované závislosti, sestavení, a pak balíčky hello projektu. výstup Hello je uložen v hello **/target** adresáři jako **EventHubExample. 1.0 SNAPSHOT.jar**.

## <a name="test-locally"></a>Test místně

Vzhledem k tomu, že tyto topologie jenom číst a zapisovat tooEvent rozbočovače, je můžete otestovat místně, pokud máte [Storm vývojového prostředí](http://storm.apache.org/releases/current/Setting-up-development-environment.html). Použijte následující postup toorun místně v prostředí dev hello hello:

1. Spusťte zapisovače hello:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. Spusťte čtečky hello:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * `--local`: Topologie spuštění hello v místním režimu (bez distribuované).
> * `-R /writer.yaml`: Načíst definici topologie hello z hello `resources` součástí hello jar. Pokud hello topologie je soubor na hello místního systému souborů, zadejte místo toho hello cesta tooit jako poslední parametr hello.
> * `--filter dev.properties`: Použijte hello obsah `dev.properties` toofill hello hodnoty v definicích topologie hello. Například, `${eventhub.read.policy.name}`.

Výstup je konzola zaznamenané toohello při místním spuštění. Použití __Ctrl + C__ toostop hello topologie.

## <a name="deploy-hello-topologies"></a>Hello topologie nasazení

1. Použití clusteru HDInsight tooyour jar spojovací bod služby toocopy hello balíčku. Nahraďte uživatelské jméno uživatele hello SSH pro váš cluster. Nahraďte název clusteru s názvem hello clusteru HDInsight:

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Pokud jste použili heslo pro účet SSH, jste výzvami tooenter hello heslo. Pokud jste použili klíče SSH s účtem hello, může být nutné toouse hello `-i` parametr toospecify hello cestě toohello klíče souboru. Například `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`.

    Tento příkaz zkopíruje hello souboru toohello domovský adresář vaše uživatele SSH na hello clusteru.

2. Po dokončení nahrávání souboru hello použijte clusteru HDInsight toohello tooconnect SSH. Nahraďte **uživatelské jméno** hello název vaší přihlašování přes SSH. Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > Pokud jste použili heslo pro účet SSH, jste výzvami tooenter hello heslo. Pokud jste použili klíče SSH s účtem hello, může být nutné toouse hello `-i` parametr toospecify hello cestě toohello klíče souboru. Hello následující příklad načte privátní klíč hello z `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Použijte následující příkaz toostart hello topologie hello:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * `--remote`: Odešle hello topologie toohello Nimbus služba, která ji spustí na hello uzlů pracovního procesu v clusteru hello.

4. tooview hello protokolovat data, přejděte toohttps://CLUSTERNAME.azurehdinsight.net/stormui, kde __CLUSTERNAME__ je hello název clusteru HDInsight. Vyberte hello topologie a podrobně toohello součásti. Vyberte hello __port__ položka pro instanci komponenty tooview protokolují informace.

5. Použijte následující příkazy toostop hello topologie hello:

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)
