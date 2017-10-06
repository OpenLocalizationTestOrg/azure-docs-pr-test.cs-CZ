---
title: "Instalační program aaaCluster pro Hadoop, Spark, Kafka, HBase nebo R Server - Azure HDInsight | Microsoft Docs"
description: "Nastavte clustery Hadoop, Kafka, Spark, HBase, R Server nebo Storm pro HDInsight z prohlížeče, hello rozhraní příkazového řádku Azure, Azure PowerShell, REST nebo sady SDK."
keywords: "nastavení clusteru hadoop, kafka clusteru instalační program, nastavení clusteru spark, co je cluster v hadoop"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 23a01938-3fe5-4e2e-8e8b-3368e1bbe2ca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/06/2017
ms.author: jgao
ms.openlocfilehash: 80ec59d8a39f7fccb940503fd2dc3ae5afee6bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-clusters-in-hdinsight-with-hadoop-spark-kafka-and-more"></a>Nastavit clusterů v HDInsight Hadoop, Spark, Kafka a dalšími

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Zjistěte, jak tooset registrace a konfigurace clusterů v HDInsight Hadoop, Spark, Kafka, interaktivní Hive, HBase, R Server nebo Storm. Také zjistěte, jak toocustomize clusterů a zvýšit zabezpečení spojením tooa domény.

Hadoop cluster se skládá z několika virtuálních počítačů (uzlů), které se používají pro distribuované zpracování úlohy. Azure HDInsight zpracovává podrobnosti implementace instalace a konfigurace jednotlivých uzlů, takže máte jenom informace o obecné konfiguraci tooprovide. 

> [!IMPORTANT]
>Jakmile clusteru je vytvořen a zastaví, když odstranění clusteru hello začne běžet fakturace clusteru HDInsight. Fakturuje se fakturují za minutu, proto byste měli vždy odstranit cluster, když je již používán. Zjistěte, jak příliš[odstranit cluster.](hdinsight-delete-cluster.md)
>

## <a name="cluster-setup-methods"></a>Metody instalace clusteru
Hello následující tabulka uvádí hello různé metody, že které můžete použít tooset do clusteru HDInsight.

| Clustery jsou vytvořené pomocí | Webový prohlížeč | Příkazový řádek | REST API | Sada SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Šablony Azure Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Rychlé vytvoření: nastavení základní clusteru
Tento článek vás provede procesem instalace v hello [portál Azure](https://portal.azure.com), kde můžete vytvořit cluster HDInsight pomocí *rychle vytvořit* nebo *vlastní*. 

Postupujte podle pokynů na hello obrazovky toodo nastavení základní clusteru. Podrobnosti jsou uvedené pro:

* [Název skupiny prostředků](#resource-group-name)
* [Typy clusterů a konfigurace](#cluster-types) 
* [Přihlášení clusteru a uživatelské jméno SSH](#cluster-login-and-ssh-username)
* [Umístění](#location)

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [HDInsight 3.3 vyřazení](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a name="resource-group-name"></a>Název skupiny prostředků 

[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) pomáhá při práci s prostředky hello v aplikaci jako skupina, která označují tooas skupinu prostředků Azure. Můžete nasadit, aktualizovat, sledovat nebo odstranit všechny prostředky hello pro vaši aplikaci v rámci jediné koordinované operace.

## <a name="cluster-types"></a>Typy clusterů a konfigurace
Azure HDInsight aktuálně poskytuje následující hello clusteru typů, které mají sadu součásti tooprovide určité funkce.

> [!IMPORTANT]
> Clustery HDInsight jsou k dispozici v různých typů, které pro jedna úloha nebo technologii. Neexistuje žádná podporovaná metoda toocreate clusteru, který kombinuje více typů, jako je například Storm a HBase v jednom clusteru. Pokud vaše řešení vyžaduje technologie, které jsou rozloženy více typy clusterů HDInsight [virtuální síť Azure](https://docs.microsoft.com/azure/virtual-network) můžete připojit hello požadované typy clusteru. 
>
>

| Typ clusteru | Funkce |
| --- | --- |
| [Hadoop](hdinsight-hadoop-introduction.md) |Batch dotazu a analýzy uložených dat |
| [HBase](hdinsight-hbase-overview.md) |Zpracování velkých objemů dat schemaless, NoSQL |
| [Storm](hdinsight-storm-overview.md) |Zpracování událostí v reálném čase |
| [Spark](hdinsight-apache-spark-overview.md) |Zpracování v paměti, interaktivních dotazů datového proudu micro dávkové zpracování |
| [Kafka (Preview)](hdinsight-apache-kafka-introduction.md) | Distribuované streamování platformu, která může být použité toobuild kanálů v reálném čase streamování dat a aplikací |
| [R Server](hdinsight-hadoop-r-server-overview.md) |Různé statistiky velkých objemů dat, prediktivního modelování a strojového učení možnosti |
| [Interaktivní Hive (Preview)](hdinsight-hadoop-use-interactive-hive.md) |Ukládání do mezipaměti v paměti pro interaktivní a rychlejší dotazů Hive |

### <a name="number-of-nodes-for-each-cluster-type"></a>Počet uzlů pro každý typ clusteru
Každý typ clusteru má svou vlastní počet uzlů, terminologie pro uzly a výchozí velikost virtuálního počítače. V následující tabulce hello hello počet uzlů pro každý typ uzlu je v závorkách.

| Typ | Uzly | Diagram |
| --- | --- | --- |
| Hadoop |Hlavní uzel (2), datový uzel (1 +) |![Uzly clusteru HDInsight Hadoop](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |Hlavního serveru (2), server oblast (1 +), hlavní/ZooKeeper uzlu (3) |![Uzly clusteru HDInsight HBase](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Uzel nimbus (2), nadřízeného serveru (1 +), ZooKeeper uzlu (3) |![Uzly clusteru HDInsight Storm](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Hlavní uzel (2), pracovního uzlu (1 +), ZooKeeper uzlu (3) (zdarma pro virtuální počítač A1 ZooKeeper velikost) |![Uzly clusteru HDInsight Spark](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Další informace najdete v tématu [výchozí velikosti virtuálního počítače a konfigurace uzlu pro clustery](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) v "Jaké jsou hello komponent systému Hadoop a verzí v prostředí HDInsight?"

### <a name="hdinsight-version"></a>HDInsight verze
Zvolte hello verzi HDInsight pro tento cluster. Další informace najdete v tématu [podporované HDInsight verze](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="cluster-tiers"></a>Vrstvy clusteru: HDInsight úrovně služeb

Azure HDInsight nabízí cloud nabídky velkých objemů dat hello v dvě úrovně služby: Standard a Premium.  Další informace najdete v tématu [HDInsight Standard a HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).

Hello následující snímek obrazovky ukazuje hello Azure portálu informace pro výběr typy clusteru.

![Konfigurace HDInsight premium](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)


## <a name="cluster-login-and-ssh-user-name"></a>Přihlášení a uživatelské jméno SSH clusteru
S clustery HDInsight můžete nakonfigurovat dva účty uživatele při vytváření clusteru:

* Uživatel HTTP: hello výchozí uživatelské jméno je *správce*. Základní konfigurace hello používá na hello portálu Azure. Někdy se označuje jako "Clusteru uživatele."
* Uživatel SSH (Linux clustery): používané tooconnect toohello clusteru prostřednictvím SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="location"></a>Umístění (oblastí) a úložiště

Umístění v clusteru hello toospecify není nutné explicitně: hello clusteru je v hello stejné umístění jako hello výchozí úložiště. Seznam podporovaných oblastí, klikněte na tlačítko hello **oblast** rozevíracího seznamu na [HDInsight ceny](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Koncové body úložiště pro clustery

I když místní instalace systému Hadoop používá hello Hadoop Distributed File System (HDFS) pro úložiště na clusteru hello, v hello cloud použít koncové body úložiště připojený toocluster. Clustery HDInsight použijte buď [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) nebo [objektů BLOB v Azure Storage](hdinsight-hadoop-use-blob-storage.md). Pomocí Azure Storage nebo Data Lake Store znamená, že můžete bezpečně odstranit clustery HDInsight hello používány pro výpočty a zároveň zachovat data. 

> [!WARNING]
> Použití účtu další úložiště v jiném umístění z clusteru HDInsight hello není podporováno.

Během konfigurace zadáte pro koncový bod úložiště hello výchozí kontejner objektů blob z účtu Azure Storage nebo Data Lake Store. Hello výchozí úložiště obsahuje aplikace a systémové protokoly. Volitelně můžete zadat další propojené účty Azure Storage a účty Data Lake Store, které hello clusteru můžete získat přístup. Hello HDInsight cluster a hello závislého úložiště, které účty musí být v hello stejného umístění Azure.

![Nastavení úložiště clusteru: koncové body HDFS kompatibilní úložiště](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>Volitelné metaúložiště
Můžete vytvořit volitelné metaúložiště Hive nebo Oozie. Ale ne všechny typy clusteru podporují metaúložiště a Azure SQL Data Warehouse není kompatibilní s metaúložiště. 

> [!IMPORTANT]
> Když vytvoříte vlastní metaúložiště, nepoužívejte pomlčky, pomlčky ani mezery v hello název databáze. To může způsobit toofail procesu vytváření clusteru hello.

### <a name="use-hiveoozie-metastore"></a>Metaúložiště Hive

Pokud chcete tooretain vaše tabulek Hive po odstranění clusteru služby HDInsight, použijte vlastní metaúložiště. Pak můžete připojit cluster HDInsight tooanother metaúložiště hello.

Metaúložiště HDInsight, který se vytvoří pro jednu verzi clusteru HDInsight se nedají sdílet mezi různými verzemi clusteru HDInsight. Seznam verzí HDInsight, naleznete v části [podporované HDInsight verze](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Metaúložiště Oozie

tooincrease výkon při použití Oozie, použijte vlastní metaúložiště. Metaúložiště může také poskytnout přístup k datům úlohy tooOozie po odstranění clusteru. 

> [!IMPORTANT]
> Vlastní metaúložiště Oozie nelze znovu použít. toouse vlastní metaúložiště Oozie, při vytváření clusteru HDInsight hello je nutné zadat prázdnou databázi SQL Azure.

## <a name="configure-cluster-size"></a>Nakonfigurujte velikost clusteru

Fakturuje se využívání uzlu, dokud hello cluster existuje. Pokud cluster je vytvořen a zastaví, když odstranění clusteru hello začne běžet fakturace. Clustery nelze zrušte přidělené nebo blokovat.

Hello náklady clustery HDInsight je určen podle počtu hello uzly a hello velikosti virtuálních počítačů pro uzly hello. 

Různými typy clusteru mají různé typy uzlů, počtu uzlů a velikosti uzlu:
* Výchozí typ clusteru Hadoop: 
    * Dva *hlavní uzly*  
    * Čtyři *datové uzly*
* Výchozí typ clusteru Storm: 
    * Dva *uzly Nimbus*
    * Tři *uzly ZooKeeper*
    * Čtyři *nadřízeného uzly* 

Pokud jsou právě vyzkoušení HDInsight, doporučujeme, že abyste použili jeden datový uzel. Další informace o cenách prostředí HDInsight najdete v tématu [HDInsight ceny](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> omezení velikosti Hello clusteru se liší podle předplatných Azure. Obraťte se na [Azure podporu fakturace](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) tooincrease hello limit.
>

Pokud používáte hello Azure portálu tooconfigure hello clusteru, velikost uzlu hello je k dispozici prostřednictvím hello **cenové úrovně uzlu** okno. Hello portálu, můžete také zjistit hello náklady spojené s velikostí hello jiný uzel. 

![Virtuální počítač HDInsight velikostí uzlu](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Velikosti virtuálních počítačů 
Při nasazení clusterů, zvolte výpočetní prostředky v závislosti na řešení hello plánujete toodeploy. Hello následující virtuální počítače se používají pro clustery služby HDInsight:
* A a virtuální počítače řady D1 – 4: [velikosti virtuálního počítače s Linuxem General-purpose](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11-14 řady virtuálních počítačů: [velikosti paměťově optimalizované virtuálních počítačů Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

toofind se co hodnotou, kterou jste měli používat toospecify velikost virtuálního počítače při vytváření clusteru pomocí hello různé sady SDK nebo při použití Azure PowerShell, najdete v části [toouse pro clustery služby HDInsight velikostí virtuálních počítačů](../cloud-services/cloud-services-sizes-specs.md#size-tables). Z tohoto článku propojené hello hodnotu použít v hello **velikost** sloupec tabulek hello.

> [!IMPORTANT]
> Pokud potřebujete více než 32 uzlů pracovního procesu v clusteru, je třeba vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.
>
>

Další informace najdete v tématu [velikosti virtuálních počítačů](../virtual-machines/windows/sizes.md). Informace o cenách z hello různé velikosti najdete v tématu [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Nastavení vlastní clusteru
Instalace sestavení vlastní clusteru na hello rychlé vytvoření nastavení a přidá hello následující možnosti:
- [Aplikace HDInsight](#hdinsight-applications)
- [Velikost clusteru](#cluster-size)
- Upřesnit nastavení
  - [Akce skriptu](#customize-clusters-using-script-action)
  - [Virtuální síť](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>Instalace aplikací HDInsight v clusterech

Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux. Aplikace může používat, pokud se společností Microsoft, třetí strany nebo které vyvíjíte, sami. Další informace najdete v tématu [instalovat aplikace jiných výrobců Hadoop v Azure HDInsight](hdinsight-apps-install-applications.md).

Většina aplikací HDInsight hello jsou nainstalované na prázdný hraniční uzel.  Prázdný hraniční uzel je virtuální počítač s Linuxem pomocí hello stejné nástrojích klienta nainstalován a nakonfigurován jako hello hlavního uzlu. Hello hraniční uzel můžete použít pro přístup k hello clusteru, testování vaší klientské aplikace a hostování vaší klientské aplikace. Další informace najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Upřesňující nastavení: skript akce

Můžete nainstalovat další součásti nebo si přizpůsobit pomocí skriptů během vytváření konfigurace clusteru. Tyto skripty jsou volány prostřednictvím **akce skriptu**, což je možnost konfigurace, které je možné z hello portálu Azure, rutiny prostředí Windows PowerShell HDInsight nebo hello HDInsight .NET SDK. Další informace najdete v tématu [clusteru HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

Některé nativní součásti Java, jako je Mahout a s možností, lze spustit v clusteru hello jako soubory archivu Java (JAR). Tyto soubory JAR může být distribuované tooAzure úložiště a odeslána tooHDInsight clustery s mechanismy odesílání úloh Hadoop. Další informace najdete v tématu [Hadoop odeslání úlohy prostřednictvím kódu programu](hdinsight-submit-hadoop-jobs-programmatically.md).

> [!NOTE]
> Pokud máte problémy s nasazení clusterů tooHDInsight souborů JAR, nebo volání souborů JAR na clusterech HDInsight, obraťte se na [Microsoft Support](https://azure.microsoft.com/support/options/).
>
> Kaskádových není podporována v prostředí HDInsight a nejsou vhodné pro systém Microsoft Support. Seznam podporovaných součásti, najdete v části [co je nového ve verzích clusterů hello poskytovaných v HDInsight](hdinsight-component-versioning.md).
>
>

V některých případech je vhodné tooconfigure hello následující konfigurační soubory během procesu vytváření hello:

* clusterIdentity.xml
* Core-site.xml
* Gateway.XML
* hbase env.xml
* hbase-site.xml
* hdfs-site.xml
* Hive env.xml
* Hive-site.xml
* mapred-site
* oozie-site.xml
* oozie env.xml
* Storm-site.xml
* tez-site.xml
* webhcat site.xml
* yarn-site.xml

Další informace najdete v tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Upřesňující nastavení: rozšířit clustery s virtuální sítí
Pokud vaše řešení vyžaduje technologie, které jsou rozloženy více typy clusterů HDInsight [virtuální síť Azure](https://docs.microsoft.com/azure/virtual-network) můžete připojit hello požadované typy clusteru. Tato konfigurace umožňuje hello clustery a žádný kód nasadíte toothem, toodirectly komunikaci mezi sebou.

Další informace o používání virtuální sítě Azure s HDInsight naleznete v tématu [rozšíření prostředí HDInsight pomocí virtuální sítě Azure](hdinsight-extend-hadoop-virtual-network.md).

Příklad použití dva typy clusteru v rámci virtuální sítě Azure, naleznete v části [analýza dat snímače pomocí Storm a HBase](hdinsight-storm-sensor-data-analysis.md). Další informace o používání HDInsight s virtuální sítí, včetně určité požadavky na konfiguraci pro virtuální síť hello, najdete v části [HDInsight rozšířit možnosti pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Odstraňování potíží s přístupem ovládací prvek

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky

- [Co jsou HDInsight, hello ekosystém Hadoop a clustery systému Hadoop?](hdinsight-hadoop-introduction.md)
- [Začínáme používat Hadoop ve službě HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Práce v Hadoop v HDInsight ze systému Windows PC](hdinsight-hadoop-windows-tools.md)
