---
title: "data snímače aaaAnalyze s Apache Storm a HBase | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooApache Storm s virtuální sítí. Pomocí Storm s HBase tooprocess data snímačů z centra událostí a vizualizovat s D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.openlocfilehash: e54fe9ffc720b0089f90e302b24a9438bd43999a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analýza dat snímačů s Apache Storm, centra událostí a HBase v HDInsight (Hadoop)

Zjistěte, jak toouse Apache Storm v HDInsight tooprocess senzor, data z centra událostí Azure. Hello data jsou pak uloženy do Apache HBase v HDInsight a detekují pomocí D3.js.

šablony Azure Resource Manager Hello použité v tomto dokumentu ukazuje, jak toocreate několik prostředků Azure ve skupině prostředků. Hello šablona vytvoří virtuální síť Azure, dva clustery HDInsight (Storm a HBase) a webové aplikace Azure. Implementace node.js řídicího panelu webu v reálném čase je toohello automaticky nasazené webové aplikace.

> [!NOTE]
> Hello informace v tomto dokumentu a v příkladu v tomto dokumentu vyžadují HDInsight verze 3.6.
>
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure.
* [Node.js](http://nodejs.org/): řídicího panelu webové hello použité toopreview místně na vašem vývojovém prostředí.
* [Java a hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): používá topologie Storm toodevelop hello.
* [Maven](http://maven.apache.org/what-is-maven.html): používané toobuild a kompilace projektu hello.
* [Git](http://git-scm.com/): používané toodownload hello projektu z Githubu.
* **SSH** klienta: používá clustery HDInsight se systémem Linux toohello tooconnect. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).


> [!IMPORTANT]
> Není nutné stávajícího clusteru HDInsight. Hello kroky v tomto dokumentu vytvořte hello následující prostředky:
> 
> * Virtuální síť Azure
> * Storm v clusteru HDInsight (systémem Linux dva uzly pracovního procesu)
> * Na clusteru HDInsight HBase (systémem Linux dva uzly pracovního procesu)
> * Webové aplikace Azure, který je hostitelem webové hello řídicí panel

## <a name="architecture"></a>Architektura

![diagram architektury](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

V tomto příkladu se skládá z hello následující součásti:

* **Azure Event Hubs**: obsahuje data, která se shromažďují ze senzorů.
* **Storm v HDInsight**: poskytuje v reálném čase zpracování dat z centra událostí.
* **HBase v HDInsight**: po zpracování pomocí Storm poskytuje trvalé úložiště dat typu NoSQL pro data.
* **Služba Azure virtuální sítě**: umožňuje zabezpečení komunikace mezi hello Storm v HDInsight a HBase v HDInsight clustery.
  
  > [!NOTE]
  > Při použití rozhraní API klienta hello Java HBase je potřeba virtuální síť. Ho nebude vystavena, přes hello veřejné brány pro clustery HBase. Instalaci HBase a Storm clusterů do stejné virtuální síti umožňuje hello hello clusteru Storm (nebo všechny ostatní systémy ve virtuální síti hello) toodirectly přístup HBase pomocí rozhraní API klienta.

* **Řídicí panel webu**: příklad řídicího panelu, který grafy data v reálném čase.
  
  * Hello webu je implementované v Node.js.
  * [Socket.IO](http://socket.io/) se používá pro komunikaci v reálném čase mezi topologie Storm hello a hello webu.
    
    > [!NOTE]
    > Pro komunikaci pomocí Socket.io je podrobností implementace. Můžete použít libovolnou architekturu komunikace, jako je například nezpracovaná Websocket nebo SignalR.

  * [D3.js](http://d3js.org/) jsou použité toograph hello data, která je odeslána toohello webu.

> [!IMPORTANT]
> Dva clustery jsou povinné, protože neexistuje žádná podporovaná metoda toocreate jeden cluster HDInsight Storm a HBase.

Hello topologie čte data z centra událostí pomocí hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) třídy a zápisu dat do HBase pomocí hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) Třída. Komunikace s webem hello se dá udělat pomocí [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Hello následující diagram popisuje rozložení hello hello topologie:

![diagram topologie](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Tento diagram je zjednodušený přehled topologie hello. Pro každý oddíl v Centru událostí je vytvořena instance jednotlivých součástí. Tyto instance jsou rozmístěny v hello uzly v clusteru hello a data se směruje mezi nimi takto:
> 
> * Data z hello spout toohello Analyzátor je skupinu s vyrovnáváním zatížení.
> * Data z hello analyzátor toohello řídicí panel a HBase se seskupují podle ID zařízení, tak, aby zprávy z hello stejného zařízení vždy toku toohello stejné komponenty.

### <a name="topology-components"></a>Topologie součásti

* **Event Hub Spout**: hello spout je zadaný jako součást Apache Storm verze 0.10.0 nebo vyšší.
  
  > [!NOTE]
  > spout Hello centra událostí použité v tomto příkladu vyžaduje Storm v HDInsight clusteru verze 3.5 nebo 3.6.

* **ParserBolt.java**: hello data, která je vysílaných hello spout je nezpracovaném formátu JSON a současně je vygenerované příležitostně více než jednu událost. Touto funkcí bolt čte data hello vysílaných hello spout a analyzuje uvítací zprávu JSON. Hello bolt pak vysílá hello data jako řazené kolekce členů, která obsahuje více polí.
* **DashboardBolt.java**: Tato součást ukazuje, jak toouse hello Socket.io klientské knihovny pro aplikace Java toosend data v reálném čase toohello webový řídicím panelu.
* **Ne hbase.yaml**: hello Definice topologie použitá při spuštění v místním režimu. HBase součásti nepoužívá.
* **s hbase.yaml**: hello Definice topologie použitá při spuštění v clusteru hello hello topologie. Používá HBase součásti.
* **dev.Properties**: hello informace o konfiguraci pro hello Event Hub spout HBase bolt a součástí řídicího panelu.

## <a name="prepare-your-environment"></a>Příprava prostředí

Než použijete tento příklad, musíte vytvořit centra událostí Azure, kterou topologii Storm hello čte z.

### <a name="configure-event-hub"></a>Konfigurace centra událostí

Centrum událostí je hello zdroj dat pro tento příklad. Pomocí následujících kroků toocreate centra událostí hello.

1. Z hello [portál Azure](https://portal.azure.com), vyberte **+ nový** -> **Internet věcí** -> **Event Hubs**.
2. V hello **vytvořit Namespace** část, proveďte následující úlohy hello:
   
   1. Zadejte **název** pro obor názvů hello.
   2. Vyberte cenovou úroveň. **Základní** stačí pro tento příklad.
   3. Vyberte hello Azure **předplatné** toouse.
   4. Vyberte existující skupinu prostředků nebo vytvořte novou.
   5. Vyberte hello **umístění** pro hello centra událostí.
   6. Vyberte **Pin toodashboard**a potom klikněte na **vytvořit**.

3. Po dokončení procesu vytváření hello hello Event Hubs informace pro vašeho oboru názvů se zobrazí. Zde vyberte **+ přidat centra událostí**. V hello **vytvoření centra událostí** části, zadejte název **sensordata**a potom vyberte **vytvořit**. Ostatní pole na výchozí hodnoty hello zůstanou hello.
4. Ze služby Event Hubs hello zobrazení pro obor názvů, vyberte **Event Hubs**. Vyberte hello **sensordata** položku.
5. Hello sensordata centra událostí, vyberte **zásady sdíleného přístupu**. Použití hello **+ přidat** hello tooadd odkaz následující zásady:

    | Název zásady | Deklarace identity |
    | ----- | ----- |
    | zařízení | Odeslat |
    | Storm | Naslouchání |

1. Vyberte obě zásady a poznamenejte si hello **primární klíč** hodnotu. Potřebujete hello hodnota pro obě zásady v budoucnu krocích.

## <a name="download-and-configure-hello-project"></a>Stáhnout a nakonfigurovat hello projektu

Pomocí následujících toodownload hello projektu z Githubu hello.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Po dokončení příkazu hello máte hello následující adresářovou strukturu:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains hello topology
            resources/
                log4j2.xml - set logging toominimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO toohello web dashboard.
        dashboard/nodejs/ - this is hello node.js web dashboard.
        SendEvents/ - utilities toosend fake sensor data.

> [!NOTE]
> Tento dokument nepřekračuje v podrobnostech toofull hello kódu zahrnuty v tomto příkladu. Ale hello kód plně označené jako komentář.

tooconfigure hello projektu tooread z centra událostí, otevřete hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` souboru a přidejte toohello informace vašeho centra událostí následující řádky:

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Kompilace a testování místně

> [!IMPORTANT]
> Použití topologie hello místně vyžaduje funkční Storm vývojové prostředí. Další informace najdete v tématu [nastavení vývojového prostředí Storm](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) na Apache.org.

> [!WARNING]
> Pokud používáte vývojového prostředí systému Windows, může se zobrazit `java.io.IOException` při spuštění hello topologie místně. Pokud ano, přesuňte na topologii hello toorunning v HDInsight.

Před testováním, musí začínat hello řídicí panel tooview hello výstup hello topologie a generování dat toostore v Centru událostí.

> [!IMPORTANT]
> Hello HBase součást Tato topologie není aktivní, při testování místně. Hello Java API pro hello HBase cluster není přístupná z hello mimo virtuální síť Azure, která obsahuje hello cluster.

### <a name="start-hello-web-application"></a>Spustit hello webové aplikace

1. Otevřete příkazový řádek a změňte adresáře příliš`hdinsight-eventhub-example/dashboard`. Použijte následující příkaz tooinstall hello závislosti nutné hello webové aplikace hello:
   
    ```bash
    npm install
    ```

2. Použijte následující příkaz toostart hello webové aplikace hello:
   
    ```bash
    node server.js
    ```
   
    Zobrazí zpráva podobná toohello následující text:
   
        Server listening at port 3000

3. Otevřete webový prohlížeč a zadejte `http://localhost:3000/` jako adresa hello. Zobrazí se stránka podobné toohello následující bitové kopie:
   
    ![webové řídicí panel](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    Nechte tato příkazového řádku otevřené. Po testování, pomocí kombinace kláves Ctrl-C toostop hello webový server.

### <a name="generate-data"></a>Generování dat

> [!NOTE]
> Hello kroky v této části použijte Node.js tak, aby bylo možné na jakékoli platformě. Další příklady jazyka najdete v tématu hello `SendEvents` adresáře.

1. Otevřete nový řádek, prostředí nebo terminálu a změňte adresáře příliš`hdinsight-eventhub-example/SendEvents/nodejs`. tooinstall hello závislosti potřebné hello aplikaci, použijte následující příkaz hello:

    ```bash
    npm install
    ```

2. Otevřete hello `app.js` soubor v textovém editoru a přidejte hello informace centra událostí, které jste dříve získali:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > Tento příklad předpokládá, že jste použili `sensordata` jako hello název vašeho centra událostí. A že `devices` jako název hello hello zásad, který má `Send` deklarací identity.

3. Použijte následující příkaz tooinsert nové položky v Centru událostí hello:
   
    ```bash
    node app.js
    ```
   
    Zobrazí několik řádky výstupu, které obsahují hello data odesílání tooEvent rozbočovače:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-hello-topology"></a>Sestavte a spusťte hello topologie

1. Otevřete nový příkazový řádek a změňte adresáře příliš`hdinsight-eventhub-example/TemperatureMonitor`. toobuild a balíček hello topologie, použijte následující příkaz hello: 

    ```bash
    mvn clean package
    ```

2. topologie hello toostart v místním režimu, hello použijte následující příkaz:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`Spustí hello topologie v místním režimu.
    * `--filter`hello používá `dev.properties` souboru toopopulate parametry v definici topologie hello.
    * `resources/no-hbase.yaml`hello používá `no-hbase.yaml` definice topologie.
 
   Po zahájení, hello topologie čte položky z centra událostí a odešle je řídicí panel toohello spuštěna na místním počítači. Měli byste vidět řádky v řídicím panelu webové hello, podobně jako toohello následující bitové kopie:
   
    ![řídicí panel s daty](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. Je spuštěn hello řídicí panel, použijte hello `node app.js` příkaz z hello předchozí kroky toosend nové tooEvent datového centra. Protože hello teploty hodnoty jsou náhodně vygenerované, by měl aktualizovat graf hello tooshow velké změny v teploty.
   
   > [!NOTE]
   > Musí být v hello **hdinsight-eventhub – příklad/SendEvents/Nodejs** directory při použití hello `node app.js` příkaz.

3. Po ověření, tento řídicí panel hello aktualizací, zastavte hello topologie pomocí kombinace kláves Ctrl + C. Také můžete použít kombinaci kláves Ctrl + C toostop hello místní webový server.

## <a name="create-a-storm-and-hbase-cluster"></a>Vytvoření clusteru Storm a HBase

Hello kroky v této části použijte [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md) toocreate clusteru služby Azure Virtual Network a Storm a HBase ve virtuální síti hello. šablony Hello také vytvoří Azure Web App a nasadí kopii hello řídicího panelu do ní.

> [!NOTE]
> Virtuální síť se používá, aby hello topologie běží v clusteru Storm hello přímo komunikovat s hello clusteru HBase pomocí hello HBase Java API.

Šablona Resource Manager Hello v tomto dokumentu se nachází v kontejneru veřejného objektu blob na **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Klikněte na následující tlačítko toosign v tooAzure a otevřete hello šablony Resource Manageru v hello portál Azure hello.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Z hello **vlastní nasazení** zadejte hello následující hodnoty:
   
    ![Parametry HDInsight](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * **Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Storm a HBase hello. Například zadáním **abc** vytvoří cluster Storm s názvem **storm abc** a cluster HBase s názvem **hbase abc**.
   * **Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce hello u clusterů Storm a HBase hello.
   * **Heslo pro přihlášení clusteru**: heslo uživatele správce hello u clusterů Storm a HBase hello.
   * **Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Storm a HBase hello.
   * **Heslo SSH**: hello heslo pro uživatele SSH hello u clusterů Storm a HBase hello.
   * **Umístění**: hello oblast, která hello clustery jsou vytvořeny v.
     
     Klikněte na tlačítko **OK** toosave hello parametry.

3. Použití hello **Základy** části toocreate skupinu prostředků nebo vyberte nějaký existující.
4. V hello **umístění skupiny prostředků** rozevírací nabídce vyberte hello stejné umístění, jako jste vybrali pro hello **umístění** parametr v hello **nastavení** části.
5. Přečtěte si hello podmínky a ujednání a pak vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.
6. Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**. Trvá přibližně 20 minut toocreate hello clustery.

Po vytvoření hello prostředky, zobrazí se informace o skupině prostředků hello.

![Skupinu prostředků pro virtuální síť hello a clustery](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy hello clusterů HDInsight hello **storm BASENAME** a **hbase BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony. Můžete použít tyto názvy v pozdější fázi při připojování toohello clustery. Všimněte si, že hello název hello řídicího panelu webu je také **basename – řídicí panel**. Tato hodnota se používá později v tomto dokumentu.

## <a name="configure-hello-dashboard-bolt"></a>Konfigurace funkcí bolt hello řídicí panel

toosend data toohello řídicí panel nasadit jako webovou aplikaci, musíte upravit hello následující řádek ve hello `dev.properties`souboru:

```yaml
dashboard.uri: http://localhost:3000
```

Změna `http://localhost:3000` příliš`http://BASENAME-dashboard.azurewebsites.net` a uložte soubor hello. Nahraďte **BASENAME** základní názvem hello jste zadali v předchozím kroku hello. Můžete vytvořit také skupinu prostředků hello vytvořili dříve tooselect hello řídicí panely a zobrazení hello adresy URL.

## <a name="create-hello-hbase-table"></a>Vytvoření tabulky HBase hello

toostore data v HBase, jsme musíte nejdřív vytvořit tabulku. Předem vytvořit prostředky, které potřebuje Storm toowrite k, protože snažíme toocreate prostředky z uvnitř topologie Storm může způsobit pokusu o několik instancí toocreate hello stejného zdroje. Vytvořte hello prostředky mimo hello topologie a používat Storm pro čtení/zápis a analýzy.

1. Pomocí SSH tooconnect toohello clusteru HBase pomocí hello SSH uživatele a heslo zadané toohello šablonu při vytváření clusteru. Například, pokud připojení pomocí hello `ssh` příkaz, využije hello následující syntaxi:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Nahraďte `sshuser` s uživatelským jménem SSH hello jste zadali při vytváření clusteru hello. Nahraďte `clustername` s názvem clusteru HBase hello.

2. Z relace SSH hello spusťte prostředí HBase hello.
   
    ```bash
    hbase shell
    ```
   
    Jakmile hello prostředí načetl, zobrazí `hbase(main):001:0>` řádku.

3. Z hello prostředí HBase zadejte následující příkaz toocreate dat snímačů hello v tabulce toostore hello:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Ověřte, zda že tato hello tabulka byla vytvořena pomocí hello následující příkaz:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Tento příkaz vrátí informace podobné toohello následující ukázka, označující, že jsou v tabulce hello 0 řádků.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Zadejte `exit` tooexit hello prostředí HBase:

## <a name="configure-hello-hbase-bolt"></a>Konfigurace funkcí bolt HBase hello

toowrite tooHBase z clusteru Storm hello, je nutné zadat hello HBase bolt s hello podrobnosti o konfiguraci vašeho clusteru HBase.

1. Použijte jednu z hello následující příklady tooretrieve hello Zookeeper kvora pro váš cluster HBase:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Nahraďte `your_HDInsight_cluster_name` s názvem hello clusteru HDInsight. Další informace o instalaci hello `jq` nástroj, najdete v části [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > Po zobrazení výzvy zadejte přihlašovací jméno správce HDInsight hello hello heslo.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Nahraďte ' your_HDInsight_cluster_name s názvem hello clusteru HDInsight. Po zobrazení výzvy zadejte přihlašovací jméno správce HDInsight hello hello heslo.
    >
    > Tento příklad vyžaduje prostředí Azure PowerShell. Další informace o použití prostředí Azure PowerShell najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    informace Hello vrácený tyto příklady jsou podobné toohello následující text:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Tyto informace slouží toocommunicate Storm s clusterem HBase hello.

2. Upravit hello `dev.properties` souboru a přidejte hello Zookeeper kvora informace toohello následující řádek:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-hello-solution-toohdinsight"></a>Vytvoření balíčku a nasadit řešení tooHDInsight hello

Ve vašem vývojovém prostředí použijte následující kroky toodeploy hello Storm topologie toohello cluster storm hello.

1. Z hello `TemperatureMonitor` adresář, použijte hello následující příkaz tooperform nového sestavení a vytvoření balíčku JAR z projektu:
   
        mvn clean package
   
    Tento příkaz vytvoří soubor s názvem `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `cíl ' adresáři projektu.

2. Použití spojovací bod služby tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` a `dev.properties` cluster Storm tooyour soubory. Následující příklad, nahraďte v hello `sshuser` uživatele SSH hello jste zadali při vytváření clusteru hello a `clustername` s názvem hello clusteru Storm. Po zobrazení výzvy zadejte hello heslo pro uživatele SSH hello.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Soubory hello tooupload může trvat několik minut.

    Další informace o používání hello `scp` a `ssh` příkazy s HDInsight, najdete v části [použití SSH s HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)

3. Po odeslání hello souboru, připojte cluster Storm toohello pomocí protokolu SSH.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Nahraďte `sshuser` s uživatelským jménem SSH hello. Nahraďte `clustername` s názvem clusteru Storm hello.

4. toostart hello topologie, použijte následující příkaz z relace SSH hello hello:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`odešle hello topologie toohello Nimbus služba, která distribuuje jej toohello nadřízeného uzly v clusteru hello.
    * `--filter`hello používá `dev.properties` souboru toopopulate parametry v definici topologie hello.
    * `-R /with-hbase.yaml`hello používá `with-hbase.yaml` topologie, které jsou součástí balíčku hello.

5. Po spuštění hello topologie, otevřete web toohello prohlížeče jste publikovali v Azure, a pak použijte hello `node app.js` příkaz toosend data tooEvent rozbočovače. Měli byste vidět hello webový řídicím panelu toodisplay hello informace o aktualizaci.
   
    ![řídicí panel](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Zobrazení dat HBase

Použijte následující postup tooconnect tooHBase hello a ověřte, že hello data byla zapsána toohello tabulky:

1. Použijte SSH tooconnect toohello HBase cluster.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Nahraďte `sshuser` s uživatelským jménem SSH hello. Nahraďte `clustername` s názvem clusteru HBase hello.

2. Z relace SSH hello spusťte prostředí HBase hello.
   
    ```bash
    hbase shell
    ```
   
    Jakmile hello prostředí načetl, zobrazí `hbase(main):001:0>` řádku.

3. Zobrazení řádků z tabulky hello:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Tento příkaz vrátí informace podobné toohello následující text, označující, že je v tabulce hello data.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Tato operace kontroly vrátí maximálně 10 řádků z tabulky hello.

## <a name="delete-your-clusters"></a>Odstranění clusterů

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toodelete hello clustery, úložiště a webové aplikace v jednom okamžiku odstranit hello skupinu prostředků, který je obsahuje.

## <a name="next-steps"></a>Další kroky

Další příklady topologií Storm s HDInsight naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

Další informace o Apache Storm naleznete v tématu hello [Apache Storm](https://storm.incubator.apache.org/) lokality.

Další informace o HBase v HDInsight, naleznete v části hello [HBase s HDInsight přehled](hdinsight-hbase-overview.md).

Další informace o Socket.io najdete v tématu hello [socket.io](http://socket.io/) lokality.

Další informace o D3.js najdete v tématu [D3.js - dokumenty řízené daty](http://d3js.org/).

Informace o vytváření topologie v jazyce Java najdete v tématu [Java vývoj topologií pro Apache Storm v HDInsight](hdinsight-storm-develop-java-topology.md).

Informace o vytváření topologie v rozhraní .NET najdete v tématu [vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
