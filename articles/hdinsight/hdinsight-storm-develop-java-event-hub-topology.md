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
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)

Naučte se používat Azure Event Hubs se Storm v HDInsight. Tento příklad používá založené na jazyce Java součásti číst a zapisovat data v Azure Event Hubs.

Azure Event Hubs umožňuje zpracovat masivní objemy dat z webů, aplikací a zařízení. Spout Centrum událostí je snadno použitelný Apache Storm v HDInsight k analýze tato data v reálném čase. Můžete také zápisu dat do centra událostí z Storm pomocí bolt Event Hubs.

## <a name="prerequisites"></a>Požadavky

* Apache Storm na verzi clusteru HDInsight 3.6. Další informace najdete v tématu [Začínáme se Storm v clusteru HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

    > [!IMPORTANT]
    > HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* [Oracle Java Developer Kit (JDK) verze 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo stejná, jako třeba [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.

* Textového editoru nebo integrované vývojové prostředí (IDE).

    > [!NOTE]
    > Editor nebo IDE může mít specifické funkce pro práci s Maven, který mu není adresovaný v tomto dokumentu. Informace o možnostech vašeho prostředí pro úpravy najdete v dokumentaci pro produkt, který používáte.

    * Klientem SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* `ssh` a `scp` příkazy. Ty se používají pro kopírování souborů do clusteru HDInsight. V systému Windows můžete získat tyto prostřednictvím Bash ve Windows 10.

## <a name="understanding-the-example"></a>Principy příklad

[Hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) příklad obsahuje dvě topologie:

`resources/writer.yaml` Topologie zapíše náhodná data do centra událostí Azure. Data je generován `DeviceSpout` součást, a je ID náhodných zařízení a zařízení hodnota. Proto se simuluje některé hardwaru, který vysílá řetězec ID a číselná hodnota.

Thí `resources/reader.yaml` topologie čte data z centra událostí (data podle EventHubWriter, zapisují) analyzuje JSON data a pak protokoly `deviceId` a `deviceValue` data.

Data je naformátován jako dokument JSON předtím, než je zapsán do centra událostí a když číst čtečky ho je analyzována z JSON a do řazené kolekce členů. Formát JSON je následující:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Konfigurace projektu

`POM.xml` Soubor obsahuje informace o konfiguraci pro tento projekt Maven. Jsou zajímavé částí:

#### <a name="event-hub-components"></a>Součásti centra událostí

Komponenta, která čte a zapisuje do služby Azure Event Hubs se nachází v [úložiště HDInsight](https://github.com/hdinsight/mvn-rep). V následujících částech v `POM.xml` souborové zatížení komponenty z tohoto úložiště

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="the-eventhubs-storm-spout-dependency"></a>Závislost EventHubs Storm Spout

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

Tato konfigurace xml definuje závislost pro balíčku eventhubs, která obsahuje spout pro čtení ze služby Event Hubs a na funkce bolt pro zápis do něj.

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Tato konfigurace xml nakonfiguruje projektu pro generování výstupu pro jazyk Java 8, který se používá v HDInsight 3.5 nebo vyšší.

#### <a name="the-maven-shade-plugin"></a>Plugin stín maven

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

Tato konfigurace xml lze konfigurovat řešení tak, aby balíček výstup do uber jar. Jar obsahuje kód projektu i požadované závislosti. Používá se také na:

* Přejmenování souborů s licencí pro závislosti.
* Vylučte zabezpečení nebo podpisy.
* Zajistěte, aby více implementace stejné rozhraní jsou sloučeny do jednu položku.

Tato nastavení konfigurace zabránit chybám za běhu.

#### <a name="topology-definitions"></a>Definice topologie

Tento příklad používá [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework. Toto rozhraní používá YAML pro definování uvedené topologie. Primární výhodou je, že nejste pevné kódování topologii v jazyce Java kódu. Vzhledem k tomu, že definice je YAML, můžete ji změnit před odesláním topologii, aniž by museli znovu zkompiluje vše.

__Writer.yaml__:

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

__Reader.yaml__:

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

#### <a name="tell-the-topology-about-event-hub"></a>Topologie říct o centra událostí

V době běhu `dev.properties` soubor se používá k nastavení centra událostí předat topologii. V následujícím příkladu je výchozí obsah souboru:

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

Následující proměnné prostředí může být nastaven při instalaci Java a sadu JDK na pracovní stanici. Nicméně byste měli zkontrolovat, že existují a že obsahují správné hodnoty pro váš systém.

* **JAVA_HOME** -by měla odkazovat na adresář, kam nainstalovat prostředí Java runtime (JRE). Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou `/usr/lib/jvm/java-7-oracle`. V systému Windows má hodnotu podobnou`c:\Program Files (x86)\Java\jre1.7`
* **CESTA** -musí obsahovat následující cesty:

  * **JAVA_HOME** (nebo ekvivalentní cesta)
  * **JAVA_HOME\bin** (nebo ekvivalentní cesta)
  * Adresář, kde je nainstalován Maven

## <a name="configure-event-hub"></a>Konfigurace centra událostí

Event Hubs je zdroj dat pro tento příklad. Pomocí následujících kroků můžete vytvořit Centrum událostí.

1. Z [portálu Azure Classic](https://manage.windowsazure.com), vyberte **nový** > **Service Bus** > **centra událostí**  >  **Vytvořit vlastní**.

2. Na **přidat nového centra událostí** obrazovky, zadejte **název centra událostí**. Vyberte **oblast** vytvoření centrum a pak vytvořit obor názvů nebo vyberte nějaký existující. Nakonec klikněte na **šipku** pokračujte.

    ![Stránka 1 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > Vyberte stejný **umístění** jako Storm v HDInsight serveru a snižuje tak latenci a náklady.

3. Na **Konfigurace centra událostí** obrazovky, zadejte **oddílu počet** a **uchování zpráv** hodnoty. V tomto příkladu použijte počet oddílů 10 a uchování zpráv 1. Poznámka: počet oddílů, protože je tato hodnota potřebovat později.

    ![Stránka 2 Průvodce](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. Po vytvoření centra událostí, vyberte obor názvů, vyberte **Event Hubs**a potom vyberte centra událostí, které jste vytvořili dříve.
5. Vyberte **konfigurace**, pak vytvořte dva nové zásady přístupu pomocí následující informace:

    <table>
    <tr><th>Name (Název)</th><th>Oprávnění</th></tr>
    <tr><td>Modul pro zápis</td><td>Odeslat</td></tr>
    <tr><td>Čtenář</td><td>Naslouchání</td></tr>
    </table>

    Po vytvoření oprávnění, vyberte **Uložit** ikona v dolní části stránky. Tyto zásady sdíleného přístupu slouží ke čtení a zápisu do centra událostí.

    ![zásady](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. Po uložení zásady, pomocí **klíče generátor sdíleného přístupu** v dolní části stránky načíst klíč pro **zapisovače** a **čtečky** zásady. Uložte tyto klíče.

## <a name="download-and-build-the-project"></a>Stáhněte si a sestavte projekt

1. Stažení projektu z Githubu: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub). Můžete stáhnout balíček jako archivu zip, nebo použijte [git](https://git-scm.com/) klonovat projektu místně.

2. Změnit `dev.properties` soubor s konfigurací pro vaše Centrum událostí.

3. Použijte následující postupy k vytvoření a balíček projektu:

        mvn package

    Tento příkaz stáhne požadované závislosti, sestavení a pak balíčky projektu. Výstup je uložen v **/target** adresáři jako **EventHubExample. 1.0 SNAPSHOT.jar**.

## <a name="test-locally"></a>Test místně

Vzhledem k tomu, že tyto topologie jenom číst a zapisovat do centra událostí, je můžete otestovat místně, pokud máte [Storm vývojového prostředí](http://storm.apache.org/releases/current/Setting-up-development-environment.html). Spustit místně v prostředí dev pomocí následujících kroků:

1. Spusťte modul pro zápis:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. Spusťte program pro čtení:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * `--local`: Spusťte topologii v místním režimu (bez distribuované).
> * `-R /writer.yaml`: Načíst definici topologie z `resources` součástí jar. Pokud topologie je soubor v místním systému souborů, zadejte cestu k němu místo jako poslední parametr.
> * `--filter dev.properties`: Použijte obsah `dev.properties` vyplnit hodnoty v definicích topologie. Například, `${eventhub.read.policy.name}`.

Při místním spuštění je výstup protokolovány v konzoli. Použití __Ctrl + C__ k zastavení topologie.

## <a name="deploy-the-topologies"></a>Nasazení topologie

1. Spojovací bod služby slouží ke kopírování balíčků jar ke svému clusteru HDInsight. Nahraďte uživatelské jméno uživatele SSH pro váš cluster. Nahraďte název clusteru s názvem clusteru HDInsight:

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Pokud jste použili heslo pro účet SSH, zobrazí se výzva k zadání hesla. Pokud jste použili klíče SSH pomocí účtu, budete možná muset použít `-i` parametru určete cestu k souboru klíče. Například `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`.

    Tento příkaz zkopíruje soubor do domovského adresáře uživatelů SSH v clusteru.

2. Po dokončení nahrávání souboru použití SSH se připojit ke clusteru HDInsight. Nahraďte **uživatelské jméno** název vaší přihlašování přes SSH. Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > Pokud jste použili heslo pro účet SSH, zobrazí se výzva k zadání hesla. Pokud jste použili klíče SSH pomocí účtu, budete možná muset použít `-i` parametru určete cestu k souboru klíče. Následující příklad načte privátní klíč z `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Použijte následující příkaz pro spuštění topologie:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * `--remote`: Odešle topologii pro službu Nimbus, která spustí na pracovních uzlech v clusteru.

4. Chcete zobrazit data protokolu, přejděte na https://CLUSTERNAME.azurehdinsight.net/stormui, kde __CLUSTERNAME__ je název clusteru HDInsight. Vyberte uvedené topologie a přejít k podrobnostem a součásti. Vyberte __port__ položka pro instanci součást k zobrazení informací o protokolu.

5. Zastavit uvedené topologie použijte následující příkazy:

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)
