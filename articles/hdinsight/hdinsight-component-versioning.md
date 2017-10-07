---
title: "aaaHadoop součásti a verze - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, hello komponent systému Hadoop a verzí v HDInsight a hello úrovně služeb k dispozici v této cloudové distribuce softwaru Hortonworks Data Platform."
keywords: "hadoop verze součástí ekosystému hadoop, komponent systému hadoop, jak toocheck hadoop verze"
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: bprakash
ms.openlocfilehash: b661d901b0113458c3501ec06454fc8841189672
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-hello-hadoop-components-and-versions-available-with-hdinsight"></a>Co jsou hello komponent systému Hadoop a verze, které jsou k dispozici v prostředí HDInsight?

Další informace o součástech ekosystém hello Apache Hadoop a verzí v Microsoft Azure HDInsight, jakož i hello úrovní služeb Standard a Premium. Také zjistěte, jak verze součástí toocheck Hadoop v HDInsight. 

Každá verze HDInsight je distribuce cloudu verze Hortonworks Data Platform (HDP).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Hadoop součásti, které jsou k dispozici s různými verzemi HDInsight
Azure HDInsight podporuje více verzích clusterů Hadoop, které lze nasadit kdykoli. Každou verzi volbu vytvoří na konkrétní verzi distribuce softwaru HDP hello a sadu součástí, které jsou obsaženy v rámci této distribuce. Od 17. února 2017 verze clusteru výchozí hello používá Azure HDInsight je 3.5 a je založená na softwaru HDP 2.5.

verze součástí Hello přidružené verze clusteru HDInsight jsou uvedeny v hello následující tabulka. 

> [!NOTE]
> Hello výchozí verze pro hello služba HDInsight mohou změnit bez předchozího upozornění. Pokud máte verzi závislostí, zadejte verzi HDInsight hello při vytváření clusterů s hello .NET SDK služby Azure PowerShell a rozhraní příkazového řádku Azure.

| Komponenta | HDInsight 3.6 (výchozí) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Datová platforma Hortonworks |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop a YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive a HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache škálu | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (pouze Linux) |1.6.2 + 2.0 (pouze Linux) |1.6.0 (pouze Linux) |1.5.2 (pouze Linux experimentální sestavení) |1.3.1 (jenom Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Zkontrolujte informace o verzi součástí aktuální Hadoop

Hello Hadoop ekosystém verze součástí přidružené verze clusteru HDInsight můžete změnit pomocí tooHDInsight aktualizace. komponent systému Hadoop hello toocheck a tooverify, jaké verze jsou používány pro cluster, použijte hello Ambari REST API. Hello **GetComponentInformation** příkaz načte informace o součásti služby. Podrobnosti najdete v tématu hello [Ambari dokumentace][ambari-docs].

Pro clustery systému Windows verze toocheck komponenty hello jiný způsob, jak je toolog v clusteru tooa pomocí vzdálené plochy a prozkoumat hello obsah adresáře C:\apps\dist\ hello.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [vyřazení Windows v HDInsight](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Poznámky k verzi

V tématu [poznámky k verzi HDInsight](hdinsight-release-notes.md) pro další poznámky na hello nejnovější verze služby HDInsight.

## <a name="supported-hdinsight-versions"></a>Podporované verze HDInsight
Hello následující tabulka uvádí verze hello služby HDInsight, které jsou aktuálně dostupné na hello portálu Azure. verze softwaru HDP Hello, které odpovídají verzi tooeach HDInsight jsou uvedeny společně s hello data vydání produktu. Datum vypršení platnosti a vyřazení podporu Hello jsou tu taky, při jejich jste známé.

> [!NOTE]
> Po podpora pro verze vypršela platnost, nemusí být k dispozici prostřednictvím portálu classic hello Microsoft Azure. Však verze clusteru pokračovat toobe dostupné pomocí hello `Version` parametr v prostředí Windows PowerShell hello [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) příkazů a dokud hello verze datu vyřazení hello .NET SDK.
> 
> Vysoce dostupný clustery se dvěma uzly head nasazených ve výchozím nastavení pro HDInsight verze 2.1 nebo novější. Nejsou k dispozici pro verzi 1.6 clustery služby HDInsight.

| HDInsight verze | Verze softwaru HDP | OPERAČNÍM SYSTÉMEM VIRTUÁLNÍHO POČÍTAČE | Vysoká dostupnost | Datum vydání | Dostupnost hello portálu Azure | Datum vypršení platnosti podpory | Datum vyřazení |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16 |Ano |4. dubna 2017 |Ano | | |
| HDInsight 3.5 |HDP 2.5 |Ubuntu 16 |Ano |30. září 2016 |Ano |5 září 2017 |31 může 2018 |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |Ano |29. března 2016 |Ano |29. prosinci 2016 |9 leden 2018 |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |Ano |2. prosince 2015 |Ano |27. června 2016 |31. července 2018 |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |Ano |2. prosince 2015 |Ano |27. června 2016 |31. července 2017 |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS nebo Windows Server 2012 R2 |Ano |18 únor 2015 |Ne |1. března 2016 |1. dubna 2017 |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |Ano |24. června 2014 |Ne |18 květen 2015 |30. června 2016 |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |Ano |11. února 2014 |Ne |17. září 2014 |30. června 2015 |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |Ano |28 říjen 2013 |Ne |12 může 2014 |31 květen 2015 |
| HDInsight 1.6 |HDP 1.1 | |Ne |28 říjen 2013 |Ne |26. dubna 2014 |31 květen 2015 |

## <a name="hdinsight-windows-retirement"></a>Vyřazení HDInsight Windows
Microsoft Azure HDInsight verze 3.3 byl hello poslední verze služby HDInsight v systému Windows. Hello datu vyřazení pro HDInsight v systému Windows je 31 července 2018. Pokud máte všechny clustery HDInsight v systému Windows 3.3 nebo dřívější, je nutné před 31 července 2018 migrovat tooHDInsight v systému Linux (HDInsight verze 3.5 nebo novější). Migrace toohello operační systém Linux vám umožní tooretain hello možnost toocreate nebo změňte velikost clusterů HDInsight. 27. června 2016 vypršela platnost podpory pro HDInsight verze 3.3 v systému Windows.

Počínaje HDInsight verze 3.4, společnost Microsoft vydala HDInsight pouze na hello operační systém Linux. V důsledku toho některé hello součásti v HDInsight jsou k dispozici pro Linux jenom. Patří mezi ně Apache škálu, Kafka, interaktivní Hive, Spark, aplikace HDInsight a Azure Data Lake Store jako systém hello primární soubor. Budoucích verzích HDInsight jsou k dispozici pouze na hello operační systém Linux. Budou existovat žádné budoucích verzích HDInsight v systému Windows. 

## <a name="faqs"></a>Nejčastější dotazy

### <a name="what-is-hello-timeline-for-retiring-hdinsight-on-windows"></a>Co je hello časovou osu pro vyřazení HDInsight v systému Windows?
31. července 2018, je hello datu vyřazení pro HDInsight v systému Windows. Pokud plánované hello datu vyřazení se liší pro vaši oblast, zobrazí se upozornění samostatně. 

### <a name="what-is-hello-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>Co je hello dopad vyřazení HDInsight v systému Windows pro stávající zákazníky služby?
Po vyřazení HDInsight v systému Windows nelze vytvořit nový cluster HDInsight Windows, nebo přizpůsobit existující cluster HDInsight Windows. Podpora pro HDInsight verze 3.3 platnost 27. června 2016. Proto není žádná podpora nebo opravy chyb pro HDInsight 3.3 nebo starší verze. Budoucích verzích HDInsight jsou k dispozici pouze na hello operační systém Linux. Budou existovat žádné budoucích verzích HDInsight v systému Windows.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Jaké verze HDInsight v systému Windows se problém týká?
Azure HDInsight verze 3.3 je poslední verzi HDInsight pro Windows hello. Předtím, než je vyřazeno HDInsight v systému Windows, všechny systémy HDInsight Windows verze clustery 3.3 nebo starším musí být migrovány tooHDInsight v systému Linux verze 3.5 nebo novější. Migrace vaší tooHDInsight clustery v Linuxu umožňuje tooretain hello možnost toocreate nových clusterů nebo změňte velikost stávajících clusterů. 

### <a name="what-do-i-need-toodo"></a>Co dělat, je potřeba toodo?
Migrace clusteru HDInsight Linux tooa podporované clustery HDInsight Windows před 31 července 2018. Další informace v hello [dokumentu migrace HDInsight](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). Podrobnosti o Azure HDInsight verze najdete v tématu hello seznam [podporované verze](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). 

### <a name="where-do-i-find-hello-cluster-os-type"></a>Kde typ operačního systému clusteru hello
V hello portálu Azure, přejděte toohello stránku přehled HDInsight Cluster a vyhledejte **clusteru typ** pod **Essentials**. typy OS Hello clusteru jsou uvedené na této stránce. 

### <a name="i-cant-migrate-tooan-hdinsight-linux-cluster-by-july-31-2018-what-is-hello-impact-toomy-hdinsight-windows-cluster"></a>Nelze migrovat tooan clusteru HDInsight Linux podle 31 července 2018. Co je hello dopad toomy clusteru se systémem HDInsight Windows?
Hello clusteru HDInsight Windows běží jako-se, ale nelze vytvořit nový cluster HDInsight Windows, nebo přizpůsobit existující cluster HDInsight Windows. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>Moje clusteru má závislost na rozhraní .NET. Jak lze vyřešit tuto závislost na platformě Linux?
Vaše závislost clusteru Linux lze vyřešit pomocí hello [Mono projektu](http://www.mono-project.com/). Tato implementace rozhraní .NET s otevřeným zdrojem je k dispozici u clusterů HDInsight Linux. Další informace v hello [dokumentu migrace HDInsight](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Jsem nového odběratele pro HDInsight v systému Windows. Jak můžete vytvořit cluster služby HDInsight Windows?
Od verze 3. července 2017 mohou pouze stávající zákazníky služby HDInsight Windows vytvářet nová okna HDInsight clustery. Nové zákazníky v nelze vytvořit cluster služby HDInsight Windows hello portálu Azure pomocí prostředí PowerShell nebo hello SDK. Doporučujeme vám, že nové zákazníky vytvoření clusteru Linux HDInsight. Stávající zákazníky služby můžete vytvořit nové HDInsight Windows clustery dokud hello HDInsight v systému Windows datu vyřazení. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-toohdinsight-on-linux"></a>Je k dispozici cenovou dopad přidružené přechod z prostředí HDInsight v systému Windows tooHDInsight v systému Linux?
Ne, ceny hello je hello stejné pro HDInsight na buď operačního systému. 

### <a name="what-are-hello-customer-advantages-associated-with-hello-move-tooonly-using-hdinsight-on-linux"></a>Jaké jsou výhody zákazníka hello přidružené hello přesunout tooonly pomocí HDInsight v Linuxu?
* Rychlejší čas na trh pro velké objemy dat technologiích s otevřeným zdrojem prostřednictvím hello služba HDInsight
* Velké komunity a ekosystém pro podporu
* Možnost tooexercise active vývoj pomocí hello otevřít zdroj komunity pro Hadoop a další technologie velkých objemů dat

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Poskytuje prostředí HDInsight v Linuxu další funkce nad rámec dostupné v prostředí HDInsight v systému Windows?
Počínaje HDInsight verze 3.4, společnost Microsoft vydala HDInsight pouze na hello operační systém Linux. V důsledku toho některé hello součásti v HDInsight jsou k dispozici pro Linux jenom. Patří mezi ně Apache škálu, Kafka, interaktivní Hive, Spark, aplikace HDInsight a Azure Data Lake Store jako systém hello primární soubor. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Dohoda o úrovni služeb pro verze clusteru HDInsight
Hello smlouvu o úrovni služeb (SLA) je definován z hlediska _podporu okno_. okno podporu Hello je hello časový interval, ve verzi clusteru HDInsight je podporován Microsoft zákaznický servis a podporu. Pokud má hello verze _podporu datum vypršení platnosti_ , byla úspěšná, hello HDInsight cluster je mimo časový interval pro podporu hello. Další informace o podporovaných verzích najdete v tématu hello seznam [podporované verze clusteru HDInsight](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). Datum vypršení platnosti Hello podpora pro zadanou HDInsight verze X (po je k dispozici novější verze X + 1) se počítá jako hello později z:  

* Vzorec 1: Přidáte 180 dnů toohello datum, kdy byl vydán verze clusteru HDInsight hello X.
* Vzorec 2: Přidáte 90 dnů toohello datum, kdy verze clusteru HDInsight hello X + 1 je k dispozici na portálu Azure.

Hello _datu vyřazení_ je hello datum, po jejímž uplynutí nelze vytvořit hello verze clusteru HDInsight. Od 31 července 2017, nelze změnit velikost clusteru služby HDInsight po datu jeho vyřazení. 

> [!NOTE]
> Clustery HDInsight Windows (včetně verze 2.1, 3.0, 3.1, 3.2 a 3.3) spustit v Azure hostovaného operačního systému rodiny verze 4, který používá hello 64bitová verze systému Windows Server 2012 R2. Skupina Azure hostovaných operačních systémů verze 4 podporuje hello rozhraní .NET Framework verze 4.0, 4.5, 4.5.1 a 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Poznámky k spojené s HDInsight verze verzi Hortonworks

Hello část obsahuje odkazy toorelease poznámky pro distribuce softwaru Hortonworks Data Platform hello a Apache součásti, které se používají s HDInsight.
* Verze clusteru HDInsight 3.6 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* Verze clusteru HDInsight 3.5 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). Verze clusteru HDInsight 3.5 je hello _výchozí_ clusteru Hadoop, který je vytvořen v hello portálu Azure.
* Verze clusteru HDInsight 3.4 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* Verze clusteru HDInsight 3.3 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Poznámky k verzi Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html) jsou k dispozici na webu Apache hello.
  * [Apache Hive poznámky k verzi](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) jsou k dispozici na webu Apache hello.
* Verze clusteru HDInsight 3.2 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.2][hdp-2-2].

  * Poznámky k verzi pro konkrétní Apache součásti jsou k dispozici následující: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [běžné](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), a [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* Verze clusteru HDInsight verze 3.1 používá distribuce Hadoop, která je založena na [softwaru Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Clustery HDInsight 3.1 vytvořené před listopadu, 7, 2014, jsou založeny na [softwaru Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* Verze clusteru HDInsight 3.0 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.0][hdp-2-0-8].
* Verze clusteru HDInsight 2.1 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 1.3][hdp-1-3-0].
* Verze clusteru HDInsight 1.6 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard a HDInsight Premium

Azure HDInsight nabízí cloud nabídky velkých objemů dat hello ve dvou kategoriích: _standardní_ a _Premium_. Hello následující tabulka uvádí funkce, které jsou k dispozici _pouze_ v HDInsight Premium. Funkce, které nejsou výslovně popsané v tabulce hello jsou k dispozici v HDInsight Standard a Premium.

> [!NOTE]
> Hello nabídka je aktuálně ve verzi preview a je k dispozici pouze pro Linux clusterů HDInsight Premium.

| Funkce HDInsight Premium | Popis |
| --- | --- |
| Clustery HDInsight připojený k doméně |Připojení k HDInsight clustery tooAzure služby Active Directory (Azure AD) domén pro zabezpečení na úrovni podniku. V HDInsight Premium můžete nakonfigurovat seznam zaměstnanci z vaší organizace, který může ověřit pomocí služby Azure AD toolog na tooan clusteru HDInsight. Dobrý den, správce rozlehlé sítě můžete nakonfigurovat pomocí řízení přístupu na základě rolí pro zabezpečení Hive [Apache škálu](http://hortonworks.com/apache/ranger/) a omezit přístup toouse data jenom tolik, kolik potřeby. Nakonec Dobrý den, správce můžete auditovat přístup k datům, zaměstnanci a změny tooaccess řídit zásady, a tím dosáhnout vysoký stupeň zásady správného řízení podnikovým prostředkům. Další informace najdete v tématu [nakonfigurovat připojený k doméně clusterů HDInsight](hdinsight-domain-joined-configure.md). |

### <a name="cluster-types-supported-in-hdinsight-premium"></a>Typy clusterů v HDInsight Premium podporované
Hello následující tabulka uvádí typy hello clusterů, které jsou podporovány v HDInsight Premium.

| Typ clusteru | Standard | Premium (Preview) |
| --- | --- | --- |
| Hadoop |Ano |Ano (jenom HDInsight 3.6) |
| Spark |Ano |Ne |
| HBase |Ano |Ne |
| Storm |Ano |Ne |
| R Server |Ano |Ne |
| Interaktivní Hive (Preview) |Ano |Ne |
| Kafka (Preview) |Ano |Ne | 

### <a name="support-for-azure-data-lake-store-in-hdinsight-premium"></a>Podpora pro Azure Data Lake Store v HDInsight Premium

Clustery HDInsight Premium nepodporují pomocí Azure Data Lake Store jako primární úložiště. Můžete však použít Azure Data Lake Store jako rozšíření úložiště s clustery HDInsight Premium.

### <a name="pricing-and-sla"></a>Ceny a smlouva SLA
Informace o cenách a SLA pro HDInsight Premium najdete v tématu [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Velikost virtuálního počítače a konfigurace uzlu výchozí pro clustery
Hello následující velikostí virtuálních počítačů (VM) tabulky seznam hello výchozí pro clustery služby HDInsight.

> [!IMPORTANT]
> Pokud potřebujete více než 32 uzlů pracovního procesu v clusteru, je třeba vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.
> 
> 

* Všechny podporované oblasti s výjimkou Brazílie – jih a Japonsko – západ:

  | Typ clusteru | Hadoop | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | HEAD: velikost virtuálního počítače výchozí |D3 v2 |D3 v2 |A3 |D12 v2 |D12 v2 |
  | HEAD: doporučené velikosti virtuálních počítačů |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |A3 A4, A5 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Worker: velikost virtuálního počítače výchozí |D3 v2 |D3 v2 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Pracovní: doporučené velikosti virtuálních počítačů |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: velikost virtuálního počítače výchozí | |A3 |A2 | | |
  | ZooKeeper: doporučené velikosti virtuálních počítačů | |A3 A4, A5 |A2, A3, A4 | | |
  | Okraj: velikost virtuálního počítače výchozí | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Okraj: Doporučená velikost virtuálního počítače | | | | |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Brazílie – jih a Japonsko – západ pouze (žádné velikosti v2):

  | Typ clusteru | Hadoop | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | HEAD: velikost virtuálního počítače výchozí |D3 |D3 |A3 |D12 |D12 |
  | HEAD: doporučené velikosti virtuálních počítačů |D12 D3, D4, |D12 D3, D4, |A3 A4, A5 |D12 D13, D14 |D12 D13, D14 |
  | Worker: velikost virtuálního počítače výchozí |D3 |D3 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Pracovní: doporučené velikosti virtuálních počítačů |D12 D3, D4, |D12 D3, D4, |D12 D3, D4, |Windows: D12 D13, D14; Linux: D4, D14 D12 D13, |Windows: D12 D13, D14; Linux: D4, D14 D12 D13, |
  | ZooKeeper: velikost virtuálního počítače výchozí | |A2 |A2 | | |
  | ZooKeeper: doporučené velikosti virtuálních počítačů | |A2, A3, A4 |A2, A3, A4 | | |
  | Hraniční: velikosti virtuálních počítačů výchozí | | | | |Windows: D12; Linux: D4 |
  | Okraj: doporučené velikosti virtuálních počítačů | | | | |Windows: D12 D13, D14; Linux: D4, D14 D12 D13, |

> [!NOTE]
> - HEAD se označuje jako *Nimbus* hello Storm clusteru typu.
> - Pracovního procesu se označuje jako *nadřízeného* hello Storm clusteru typu.
> - Pracovního procesu se označuje jako *oblast* hello HBase clusteru typu.

## <a name="next-steps"></a>Další kroky
- [Instalace clusteru pro Hadoop, Spark a další v HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Práce v Hadoop v HDInsight ze systému Windows PC](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
