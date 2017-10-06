---
title: "aaaTips pro pomocí Hadoop v HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Získáte implementace tipy pro použití systémem Linux HDInsight (Hadoop) clusterů ve známém prostředí Linux spuštěné v hello cloudu Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: a555622605079c9beae88ece872042e36d540c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="information-about-using-hdinsight-on-linux"></a>Informace o používání HDInsight v Linuxu

Azure clustery HDInsight poskytují Hadoop na známém prostředí Linux spuštěné v hello cloudu Azure. Pro většinu věcí by měly fungovat přesně jako jiná instalace Hadoop na Linuxu. Tento dokument volá určité rozdíly, které byste měli vědět.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Požadavky

Řada hello kroků v tomto dokumentu pomocí hello následující nástroje, které může být nutné toobe v systému nainstalována.

* [cURL](https://curl.haxx.se/) -použít toocommunicate s webových služeb
* [jq](https://stedolan.github.io/jq/) -použité dokumenty JSON tooparse
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (preview) – použité tooremotely spravovat služby Azure

## <a name="users"></a>Uživatelé

Pokud [připojený k doméně](hdinsight-domain-joined-introduction.md), měli byste zvážit HDInsight **jednoho uživatele** systému. Jeden uživatelský účet SSH je vytvořen s hello cluster s úrovně oprávnění správce. Můžete vytvořit další účty SSH, ale mají také clusteru toohello přístup správce.

Připojené k doméně HDInsight podporuje více uživatelů a podrobnější nastavení oprávnění a role. Další informace najdete v tématu [clustery HDInsight spravovat doméně](hdinsight-domain-joined-manage.md).

## <a name="domain-names"></a>Názvy domén

Při připojování toohello clusteru z Internetu je hello Hello plně kvalifikovaný název (FQDN) toouse domény  **&lt;clustername >. azurehdinsight.net** nebo (pro SSH pouze)  **&lt;clustername-ssh >. azurehdinsight.net**.

Každý uzel v clusteru hello interně, má název, který se přiřadí během konfigurace clusteru. názvy clusterů hello toofind, najdete v části hello **hostitele** stránky na hello webové uživatelské rozhraní Ambari. Můžete také použít hello následující tooreturn seznam hostitele z hello Ambari REST API:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Nahraďte **heslo** heslem hello hello účtu správce, a **CLUSTERNAME** s hello názvem vašeho clusteru. Tento příkaz vrátí dokument JSON, který obsahuje seznam hello hostitelé v clusteru hello. Jq je použité tooextract hello `host_name` hodnota elementu pro každého hostitele.

Pokud potřebujete název hello toofind hello uzlu pro konkrétní službu, můžete dotazovat Ambari pro danou součást. Například toofind hello hostitelů hello HDFS název uzlu, použijte hello následující příkaz:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Tento příkaz vrátí dokumentu JSON s popisem hello služby, a potom jq lze posunout pouze hello `host_name` hodnotu pro hostitele hello.

## <a name="remote-access-tooservices"></a>Tooservices vzdáleného přístupu

* **Ambari (web)** -https://&lt;clustername >. azurehdinsight.net

    Ověřování pomocí hello clusteru správce uživatele a heslo a znovu přihlásili tooAmbari.

    Ověřování je ve formátu prostého textu – vždy používá protokol HTTPS toohelp zajistěte, aby byl hello připojení zabezpečené.

    > [!IMPORTANT]
    > Některé webové hello uživatelská rozhraní, které jsou k dispozici prostřednictvím Ambari přístup uzly používá název interní domény. Hello interní domény, které názvy nejsou veřejně přístupné prostřednictvím Internetu. Při pokusu o tooaccess některé funkce přes hello Internetu, může se zobrazit chyby "server nebyl nalezen".
    >
    > toouse hello plné funkčnosti hello Ambari webového uživatelského rozhraní, použijte SSH tunel tooproxy webový provoz toohello hlavního uzlu clusteru. V tématu [používání tunelového propojení SSH tooaccess Ambari webové uživatelské rozhraní, ResourceManager, JobHistory, NameNode, Oozie a jiné webové uživatelská rozhraní](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;clustername >.azurehdinsight.net/ambari

    > [!NOTE]
    > Ověřování pomocí hello clusteru správce uživatele a heslo.
    >
    > Ověřování je ve formátu prostého textu – vždy používá protokol HTTPS toohelp zajistěte, aby byl hello připojení zabezpečené.

* **WebHCat (Templeton)** -https://&lt;clustername >.azurehdinsight.net/templeton

    > [!NOTE]
    > Ověřování pomocí hello clusteru správce uživatele a heslo.
    >
    > Ověřování je ve formátu prostého textu – vždy používá protokol HTTPS toohelp zajistěte, aby byl hello připojení zabezpečené.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net na port 22 a 23. Port 22 je použité tooconnect toohello primární headnode, použité tooconnect toohello sekundární při 23. Další informace o hlavních uzlech hello najdete v tématu [dostupnost a spolehlivost Hadoop clusterů v HDInsight](hdinsight-high-availability-linux.md).

    > [!NOTE]
    > Hlavních uzlech clusteru hello můžete přistupovat z počítače klienta pouze prostřednictvím SSH. Po připojení se pak dostanete hello pracovní uzly z headnode pomocí SSH.

## <a name="file-locations"></a>Umístění souborů

Soubory související s Hadoop naleznete na uzlech clusteru hello v `/usr/hdp`. Tento adresář obsahuje následující podadresáře hello:

* **2.2.4.9-1**: název adresáře hello je hello verzi hello softwaru Hortonworks Data Platform používané HDInsight. číslo Hello v clusteru se liší od hello jeden tady.
* **aktuální**: Tento adresář obsahuje toosubdirectories odkazy v části hello **2.2.4.9-1** adresáře. Tento adresář existuje, tak, že nemáte číslo verze tooremember hello.

Příklad dat a souborů JAR naleznete na Hadoop Distributed File System na `/example` a`/HdiSamples`

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS, úložiště Azure a Data Lake Store

Ve většině distribuce Hadoop je na hello počítače v clusteru hello HDFS zajištěna místní úložiště. Používá místní úložiště může být drahé pro cloudové řešení kde budou účtovat každou hodinu nebo minutu pro výpočetní prostředky.

HDInsight používá jako výchozí úložiště hello buď objektů BLOB v Azure Storage nebo Azure Data Lake Store. Tyto služby poskytovat hello následující výhody:

* Levných dlouhodobé uložení
* Usnadnění přístupu z externích služeb například weby, nástroje pro odeslání nebo stažení souboru, různých sadách SDK jazyka a webových prohlížečů

> [!WARNING]
> HDInsight podporuje pouze __pro obecné účely__ účty Azure Storage. Aktuálně nepodporuje hello __úložiště objektů Blob__ typ účtu.

Účet úložiště Azure může obsahovat až too4.75 TB, i když jednotlivé objekty BLOB (nebo soubory z hlediska HDInsight) můžete přejít jenom do too195 GB. Azure Data Lake Store můžete dynamicky zvětšovat toohold bilión souborů, s větší než petabajty jednotlivé soubory. Další informace najdete v tématu [vysvětlení objektů blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) a [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Pokud používáte Azure Storage nebo Data Lake Store, nemáte toodo žádné zvláštní z dat hello tooaccess HDInsight. Například následující příkaz hello seznam souborů hello `/example/data` složky bez ohledu na to, jestli je uložené v Azure Storage nebo Data Lake Store:

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a>Identifikátor URI a schéma

Některé příkazy může vyžadovat toospecify hello schéma jako součást hello URI při přístupu k souboru. Například hello komponenty Storm HDFS vyžaduje toospecify hello schéma. Pokud používáte jiné než výchozí úložiště (úložiště přidat jako "Další" úložiště toohello clusteru), musí vždy používat hello schéma jako součást hello identifikátor URI.

Při použití __Azure Storage__, použijte jednu z následujících schémata identifikátoru URI hello:

* `wasb:///`: Přístup k úložišti výchozí pomocí nešifrovaná komunikace.

* `wasbs:///`: Přístup k úložišti výchozí pomocí šifrovanou komunikaci.  schéma wasbs Hello je podporované jenom z HDInsight verze 3.6 a vyšší.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Použít při komunikaci s účtem storage jiné než výchozí. Například pokud máte účet další úložiště nebo pokud přístup k datům uložený v účtu úložiště veřejně přístupná.

Při použití __Data Lake Store__, použijte jednu z následujících schémata identifikátoru URI hello:

* `adl:///`: Přístup k hello výchozí Data Lake Store pro hello cluster.

* `adl://<storage-name>.azuredatalakestore.net/`: Použít při komunikaci s jiné než výchozí Data Lake Store. Používá se také tooaccess data mimo hello kořenový adresář clusteru HDInsight.

> [!IMPORTANT]
> Při použití Data Lake Store jako hello výchozí úložiště pro HDInsight, musíte zadat cestu v rámci hello úložiště toouse jako hello kořenového úložiště HDInsight. Hello výchozí cesta je `/clusters/<cluster-name>/`.
>
> Při použití `/` nebo `adl:///` tooaccess data, můžete přístup jenom k data uložená v kořenovém hello (například `/clusters/<cluster-name>/`) hello clusteru. data tooaccess kdekoli v úložišti hello používat hello `adl://<storage-name>.azuredatalakestore.net/` formátu.

### <a name="what-storage-is-hello-cluster-using"></a>Jaké úložiště clusteru hello používá

Ambari tooretrieve hello výchozí úložiště konfigurace můžete použít pro hello cluster. Následující hello použití příkazů HDFS informace o konfiguraci tooretrieve pomocí curl a filtrovat pomocí [jq](https://stedolan.github.io/jq/):

```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Tento příkaz vrátí hello první použitá konfigurace toohello server (`service_config_version=1`), která obsahuje tyto informace. Může být nutné toolist toofind hello nejnovějšího všechny verze konfigurace.

Tento příkaz vrátí hodnotu podobné toohello následující identifikátory URI:

* `wasb://<container-name>@<account-name>.blob.core.windows.net`Pokud používáte účet úložiště Azure.

    Hello název účtu je hello název účtu úložiště Azure hello, hello kontejner objektů blob při hello název kontejneru, který je kořenový hello hello clusteru úložiště.

* `adl://home`Pokud používáte Azure Data Lake Store. tooget hello název Data Lake Store, použijte následující volání REST hello:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Tento příkaz vrátí hello následující název hostitele: `<data-lake-store-account-name>.azuredatalakestore.net`.

    adresář hello tooget v rámci hello úložiště, který je hello kořenové pro HDInsight, použijte hello následující volání REST:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Tento příkaz vrátí cestu podobné toohello následující cestu: `/clusters/<hdinsight-cluster-name>/`.

Také můžete najít informace o hello úložiště pomocí hello portálu Azure pomocí následujících kroků hello:

1. V hello [portál Azure](https://portal.azure.com/), vyberte clusteru HDInsight.

2. Z hello **vlastnosti** vyberte **účty úložiště**. Zobrazí se informace Hello úložiště pro hello cluster.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Přístupu soubory z mimo HDInsight

Existují různé způsoby tooaccess data z clusteru HDInsight mimo hello. Hello následují několik tooutilities odkazy a sady SDK, které se dají použít toowork s daty:

Pokud používáte __Azure Storage__, najdete v části hello následující odkazy pro způsoby, můžete přístup k datům:

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): příkazy rozhraní příkazového řádku pro práci s Azure. Po instalaci, pomocí hello `az storage` příkaz nápovědu k použití úložiště, nebo `az storage blob` pro objekt blob specifické příkazy.
* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python script pro práci s objekty BLOB ve službě Azure Storage.
* Různých sadách SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Rozhraní REST API pro Storage](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Pokud používáte __Azure Data Lake Store__, najdete v části hello následující odkazy pro způsoby, můžete přístup k datům:

* [Webový prohlížeč](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [Rozhraní REST API WebHDFS](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Nástroje data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Škálování clusteru

Hello clusteru škálování funkce vám umožní toodynamically změnu hello počtu uzlů dat používá cluster. Můžete provádět operace škálování při dalších úloh nebo procesů běží na clusteru.

Hello různými typy clusteru se týká škálování následujícím způsobem:

* **Hadoop**: při příjmu dolů hello počet uzlů v clusteru, jsou restartovány některé hello služby v clusteru hello. Operace škálování může způsobit úlohy spuštěná nebo čeká na dokončení toofail na dokončení hello hello operace škálování. Po dokončení operace hello můžete znovu odešlete hello úlohy.
* **HBase**: místní servery jsou automaticky vyváženy během několika minut po dokončení operace škálování hello. toomanually vyrovnávání místní servery, použijte hello následující kroky:

    1. Připojte toohello clusteru HDInsight pomocí SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

    2. Použijte následující prostředí HBase hello toostart hello:

            hbase shell

    3. Jakmile načetl hello prostředí HBase, použijte hello následující toomanually vyrovnávání hello místní servery:

            balancer

* **Storm**: musíte znovu vyvážit všechny spuštěné topologie Storm po nebyla provedena škálování operace. Vyrovnává umožňuje hello topologie tooreadjust paralelismus nastavení na základě hello nové počtu uzlů v clusteru hello. toorebalance spuštěné topologie, použijte jednu z hello následující možnosti:

    * **SSH**: připojení toohello serveru a použití hello následující příkaz toorebalance topologii:

            storm rebalance TOPOLOGYNAME

        Můžete také zadat parametry toooverride hello paralelismus pomocné parametry původně poskytované hello topologie. Například `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` překonfigurujete hello topologie too5 pracovních procesů, 3 vykonavatelů pro součást blue spout hello a 10 vykonavatelů pro součást žlutý bolt hello.

    * **Uživatelské rozhraní Storm**: použití hello následující kroky toorebalance topologii pomocí hello uživatelské rozhraní Storm.

        1. Otevřete **https://CLUSTERNAME.azurehdinsight.net/stormui** ve webovém prohlížeči, kde CLUSTERNAME představuje název clusteru Storm hello. Pokud se zobrazí výzva, zadejte název hello HDInsight clusteru správce (správce) a heslo, které jste zadali při vytváření clusteru hello.
        2. Vyberte topologii hello chcete toorebalance a pak vyberte hello **znovu vyvážit** tlačítko. Zadejte zpoždění při hello před provedením operace obnovte rovnováhu hello.

Konkrétní informace o škálování clusteru HDInsight naleznete v tématu:

* [Správa clusterů systému Hadoop v HDInsight pomocí hello portálu Azure](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Správa clusterů systému Hadoop v HDInsight pomocí prostředí Azure PowerShell](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Jak nainstalovat Hue (nebo jiné součásti Hadoop)?

HDInsight je spravované služby. Pokud Azure zjistí problém s hello clusteru, může odstranit hello selhání uzlu a vytvoření uzlu tooreplace ho. Pokud je ručně nainstalovat na clusteru hello věcí, nejsou trvalé v případě tuto operaci. Místo toho použijte [akce skriptu HDInsight](hdinsight-hadoop-customize-cluster.md). Akce skriptu lze použít toomake hello následující změny:

* Nainstalujte a nakonfigurujte službu nebo webové stránky, jako je například Spark nebo Hue.
* Instalace a konfigurace komponenty, která vyžaduje změny konfigurace více uzlů v clusteru hello. Například proměnnou požadované prostředí, vytváření adresář protokolování nebo vytvoření konfiguračního souboru.

Akce skriptů jsou skripty Bash. skripty Hello při zřizování clusteru spustit a může být použité tooinstall a nakonfigurujte další součásti v clusteru hello. Příklad skriptů jsou k dispozici pro instalaci hello následující součásti:

* [HUE](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>Souborů JAR

Některé technologie Hadoop jsou uvedeny v nezávislý jar souborů, které obsahují funkce, které jsou použity jako součást úlohy MapReduce, nebo z uvnitř Pig nebo Hive. Když tyto můžete nainstalovat pomocí akcí skriptů, se často nevyžadují žádné instalační program a nahrané toohello clusteru po zřízení a použít přímo. Pokud chcete, zda součást hello toomake odolává obnovování hello clusteru, můžete uložit soubor jar hello ve hello výchozí úložiště pro váš cluster (WASB nebo ADL).

Například, pokud chcete, aby nejnovější verze hello toouse [DataFu](http://datafu.incubator.apache.org/), můžete stáhnout jar obsahující hello projektu a nahrajte ho toohello clusteru HDInsight. Potom postupujte podle dokumentace DataFu hello o toouse z Pig nebo Hive.

> [!IMPORTANT]
> Některé součásti, které jsou samostatné jar soubory jsou k dispozici s HDInsight, ale nejsou v cestě hello. Pokud hledáte konkrétní součást, můžete pro ni toosearch hello postupujte podle kroků v clusteru:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Tento příkaz vrátí hello cesta žádné odpovídající souborů jar.

toouse jinou verzi komponentu nahrávání hello verze potřebujete a použít v úlohách.

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporované a Microsoft Support pomáhá tooisolate a vyřešit problémy související toothese součásti.
>
> Vlastní komponenty přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. To může způsobit řešení problému hello nebo žádat, že jste tooengage dostupné kanály pro hello otevřít zdroj technologie, kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Další kroky

* [Migrace z HDInsight se systémem Windows na základě tooLinux](hdinsight-migrate-from-windows-to-linux.md)
* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití úloh MapReduce se službou HDInsight](hdinsight-use-mapreduce.md)
