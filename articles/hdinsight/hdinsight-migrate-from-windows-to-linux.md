---
title: "aaaMigrate z HDInsight se systémem Windows Azure na základě tooLinux HDInsight - | Microsoft Docs"
description: "Zjistěte, jak toomigrate z HDInsight se systémem Windows clusteru tooa clusteru HDInsight se systémem Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: ff35be59-bae3-42fd-9edc-77f0041bab93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 7e5e536e8672d7e7c3086c6860cec062d05eda65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-tooa-linux-based-cluster"></a>Migrace z tooa systémem Linux clusteru HDInsight se systémem Windows clusteru

Tento dokument poskytuje podrobné informace o hello rozdíly mezi HDInsight v systému Windows a Linux a pokyny toomigrate existující úlohy tooa systémem Linux clusteru.

Zatímco HDInsight se systémem Windows poskytuje snadný způsob toouse Hadoop v cloudu hello, může být nutné toomigrate tooa clusteru se systémem Linux. Například tootake výhod systémem Linux nástroje a technologie, které jsou požadovány pro vaše řešení. Celou řadu věcí v ekosystému Hadoop hello se vyvíjí na systémy Linux a nemusí být k dispozici pro použití s HDInsight se systémem Windows. Mnoho knihy, videa a další materiály školení předpokládá, že používáte systém Linux, při práci s Hadoop.

> [!NOTE]
> Clustery HDInsight používat dlouhodobé podporu Ubuntu (LTS) jako hello operační systém pro hello uzly v clusteru hello. Informace v hello verzi Ubuntu s HDInsight, společně s další informace o správě verzí součásti najdete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Úlohy migrace

Hello obecný pracovní postup migrace je následující.

![Diagram pracovního postupu migrace](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Číst každá část tohoto dokumentu toounderstand změny, které může být potřeba při migraci stávající pracovního postupu, úlohy, clusteru se systémem Linux tooa atd.

2. Vytvoření clusteru se systémem Linux jako prostředí testovací a kvalita záruky. Další informace týkající se vytvoření clusteru se systémem Linux najdete v tématu [vytvořit systémem Linux clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3. Kopie existující úlohy, zdroje dat a jímky toohello nové prostředí.

4. Provedení ověření testování toomake jistotu, že vaše úlohy fungují podle očekávání na novém clusteru hello.

Jakmile si ověříte, že vše funguje podle očekávání, naplánujte dobu odstávky hello migrace. Během této výpadky proveďte hello následující akce:

1. Zálohujte všechna přechodný data uložená místně na uzlech clusteru hello. Například pokud máte data uložená přímo na hlavního uzlu.

2. Odstranění clusteru systému Windows hello.

3. Vytvořte cluster se systémem Linux pomocí hello stejné datové úložiště, které hello založené na Windows cluster, který používá výchozí. Hello clusteru se systémem Linux můžete pokračovat v práci s existující provozní data.

4. Importujte přechodný data, která jste zálohovali.

5. Spuštění úlohy nebo pokračovat ve zpracovávání pomocí hello nového clusteru.

### <a name="copy-data-toohello-test-environment"></a>Kopírování dat toohello testovacího prostředí

Existuje mnoho metody toocopy hello data a úlohy, ale hello dva popsaných v této části jsou hello nejjednodušší metody toodirectly přesunout soubory tooa zkušební cluster.

#### <a name="hdfs-copy"></a>HDFS kopie

Pomocí následujících kroků toocopy data z hello produkční clusteru toohello zkušební cluster hello. Tyto kroky používají hello `hdfs dfs` nástroj, který se dodává s HDInsight.

1. Najít hello úložiště účet a výchozí informace o kontejneru pro existující cluster. Následující ukázka Hello používá prostředí PowerShell tooretrieve tyto informace:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. toocreate testovacím prostředí, postupujte podle kroků hello v clusterech se systémem Linux vytvořit hello v HDInsight dokumentu. Zastavit před vytvořením clusteru hello a místo toho vyberte **volitelné konfiguraci**.

3. V okně hello volitelné konfiguraci, vyberte **propojených účtech Storage**.

4. Vyberte **přidejte klíč k úložišti**a po zobrazení výzvy vyberte hello účet úložiště, který vrátil hello skript prostředí PowerShell v kroku 1. Klikněte na tlačítko **vyberte** v každém okně. Nakonec vytvořte hello cluster.

5. Po vytvoření clusteru hello připojit pomocí tooit **SSH.** Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

6. Z relace SSH hello použijte následující příkaz toocopy soubory z hello propojené úložiště účet toohello nový výchozí účet úložiště hello. Nahraďte KONTEJNERU informace o kontejneru hello vrácený prostředí PowerShell. Nahraďte __účet__ s názvem účtu hello. Nahraďte hello cesta toodata hello cesta tooa datový soubor.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]
    > Pokud hello adresářovou strukturu, která obsahuje hello data neexistuje na hello testovací prostředí, můžete vytvořit pomocí hello následující příkaz:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    Hello `-p` přepínač umožňuje vytvoření hello všechny adresáře v cestě hello.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Přímé kopírování mezi objekty BLOB ve službě Azure Storage

Alternativně můžete toouse hello `Start-AzureStorageBlobCopy` objekty BLOB toocopy rutiny prostředí Azure PowerShell mezi účty úložiště mimo HDInsight. Další informace najdete v tématu hello jak toomanage části objektů BLOB služby Azure pomocí Azure powershellu s Azure Storage.

## <a name="client-side-technologies"></a>Technologie na straně klienta

Klientské technologie, jako [rutin prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs), [rozhraní příkazového řádku Azure](../cli-install-nodejs.md), nebo hello [.NET SDK pro Hadoop](https://hadoopsdk.codeplex.com/) pokračovat toowork clusterech se systémem Linux. Tyto technologie spoléhají na REST API, které jsou stejné hello napříč oba typy clusteru operačního systému.

## <a name="server-side-technologies"></a>Technologie na straně serveru

Hello následující tabulka obsahuje pokyny k migraci serverové komponenty, které jsou specifické pro systém Windows.

| Pokud použijete tuto technologii... | Tato akce trvat... |
| --- | --- |
| **Prostředí PowerShell** (skripty na straně serveru, včetně akcí skriptů používají při vytváření clusteru) |Přepisování jako skripty Bash. Pro skript akce, najdete v části [HDInsight se systémem Linux přizpůsobit pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md) a [vývoj akcí pro HDInsight se systémem Linux skriptů](hdinsight-hadoop-script-actions-linux.md). |
| **Rozhraní příkazového řádku Azure** (skripty na straně serveru) |Při hello rozhraní příkazového řádku Azure je k dispozici v systému Linux, nepochází předem nainstalovaná na head uzlech clusteru HDInsight hello. Další informace o instalaci hello rozhraní příkazového řádku Azure najdete v tématu [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **Součásti rozhraní .NET** |Rozhraní .NET je podporována v HDInsight se systémem Linux prostřednictvím [Mono](https://mono-project.com). Další informace najdete v tématu [řešení migrovat .NET HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **Součásti Win32 nebo jiné technologie pouze pro systém Windows** |Pokyny, závisí na komponentu hello nebo technologii. Může být schopný toofind na verzi, která je kompatibilní s Linuxem, nebo můžete potřebovat toofind alternativní řešení nebo přepsání této součásti. |

> [!IMPORTANT]
> Správa Hello HDInsight SDK není plně kompatibilní se Mono. Není vhodné používat jako součást řešení nasazené v tuto chvíli toohello clusteru HDInsight.

## <a name="cluster-creation"></a>Vytvoření clusteru

Tato část obsahuje informace o rozdíly ve vytváření clusteru.

### <a name="ssh-user"></a>SSH uživatele

Clustery HDInsight se systémem Linux použijte hello **Secure Shell (SSH)** protokolu uzly clusteru toohello tooprovide vzdáleného přístupu. Na rozdíl od clustery založené na vzdálené plochy pro Windows neposkytují většině klientů SSH v grafickém uživatelském rozhraní. Místo toho klientů SSH zadejte příkazový řádek, který vám umožní toorun příkazy v clusteru hello. Někteří klienti (například [MobaXterm](http://mobaxterm.mobatek.net/)) zadejte Prohlížeč systému grafické souboru v přidání tooa vzdálené příkazového řádku.

Při vytváření clusteru, je třeba zadat uživatel SSH a buď **heslo** nebo **certifikátu veřejného klíče** pro ověřování.

Doporučujeme používat certifikát veřejného klíče, jako je bezpečnější než použití hesla. Ověřování pomocí certifikátu funguje tak, že generování podepsaný pár veřejného a privátního klíče, pak při vytváření clusteru hello poskytování hello veřejný klíč. Při připojování toohello serveru pomocí protokolu SSH, poskytuje hello privátní klíč na klientovi hello ověřování pro připojení hello.

Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="cluster-customization"></a>Přizpůsobení clusteru

**Skript akce** použít s clustery se systémem Linux musí být napsaný ve skriptu Bash. Zatímco skript akce lze použít při vytváření clusteru pro clustery se systémem Linux použité tooperform přizpůsobení mohou být až po clusteru a spuštěna. Další informace najdete v tématu [HDInsight se systémem Linux přizpůsobit pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md) a [vývoj akcí pro HDInsight se systémem Linux skriptů](hdinsight-hadoop-script-actions-linux.md).

Další přizpůsobení funkcí je **bootstrap**. Pro clustery systému Windows tato funkce vám umožní toospecify hello umístění další knihovny pro použití s Hive. Po vytvoření clusteru, jsou automaticky dostupné pro použití s dotazy Hive, bez nutnosti toouse hello tyto knihovny `ADD JAR`.

Hello Bootstrap funkce pro clustery se systémem Linux neposkytuje tuto funkci. Místo toho použijte akce skriptu, které jsou dokumentovány v článku [přidat Hive knihovny při vytváření clusteru](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuální sítě

HDInsight se systémem Windows clusterů funguje jenom s klasické virtuální sítě, zatímco clustery HDInsight se systémem Linux vyžadují virtuální sítě Resource Manager. Pokud máte prostředky klasické virtuální sítě, která hello clusteru Linux HDInsight musí připojit k, viz téma [připojení klasické virtuální sítě tooa virtuální sítě Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Další informace o požadavky na konfiguraci pro použití virtuálních sítí Azure s HDInsight naleznete v tématu [HDInsight rozšířit možnosti pomocí virtuální sítě](hdinsight-extend-hadoop-virtual-network.md).

## <a name="management-and-monitoring"></a>Monitorování a správa

Mnoho webových hello uživatelská možná zneužil s HDInsight se systémem Windows, jako je historie úlohy nebo uživatelského rozhraní Yarn, jsou k dispozici prostřednictvím Ambari. Kromě toho hello zobrazení Ambari Hive poskytuje způsob toorun dotazů Hive pomocí webového prohlížeče. Hello webové uživatelské rozhraní Ambari je k dispozici v clusterech se systémem Linux v https://CLUSTERNAME.azurehdinsight.net.

Další informace o práci s Ambari najdete v tématu hello následující dokumenty:

* [Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari výstrahy

Ambari má upozornění systému, který zjistí potenciální problémy s clusterem hello. Výstrahy se zobrazují jako red nebo žlutou položek v hello webové uživatelské rozhraní Ambari, ale můžete také načíst prostřednictvím hello REST API.

> [!IMPORTANT]
> Výstrahy Ambari označuje, zda *může* se jednat o problém není že v něm *je* problém. Například můžete obdržet oznámení, že nejsou k dispozici HiveServer2, i když je k němu přístup normálně.
>
> Mnoho výstrah jsou implementované jako dotazy na bázi intervalů pro služby a očekávat odpovědi v určitém časovém rámci. Proto hello upozornění nemusí nutně znamenat, že hello služba není spuštěna, aby nevrátila výsledky v časovém rámci hello očekává právě.

Byste měli zvážit, zda výstrahu projevuje po delší dobu, nebo zrcadlí uživatele problémy, které byla nahlášena před provedením akce na něm.

## <a name="file-system-locations"></a>Umístění systému souborů

Hello systém souborů clusteru Linux rozložená jinak než clustery HDInsight se systémem Windows. Pomocí následující tabulky toofind běžně používané soubory hello.

| Potřebuji toofind... | Je umístěný... |
| --- | --- |
| Konfigurace |`/etc`. Například `/etc/hadoop/conf/core-site.xml`. |
| Soubory protokolu |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. Existují dva adresáře tady, ten, který je aktuální verze softwaru HDP hello a `current`. Hello `current` adresář obsahuje toofiles symbolické odkazy a adresářů v adresáři číslo verze hello. Hello `current` adresář je zadaný jako pohodlný způsob přístupu k souborům HDP od hello změní se číslo verze jako hello HDP verzi aktualizovat. |
| hadoop streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Obecně platí Pokud znáte název hello hello souboru, můžete použít následující příkaz z relace SSH toofind hello souboru cesty hello:

    find / -name FILENAME 2>/dev/null

Můžete také použít zástupné znaky s názvem souboru hello. Například `find / -name *streaming*.jar 2>/dev/null` vrátí hello cesta tooany jar soubory, které obsahují hello word streamování jako součást hello název souboru.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig a MapReduce

Pig a MapReduce úlohy jsou podobné v clusterech se systémem Linux. Ale clustery HDInsight se systémem Linux můžete vytvořit pomocí novější verze systému Hadoop, Hive a Pig. Tyto rozdíly verze může znamenat změny v jak vaše stávající řešení funkce. Další informace o verzích hello součásti, které jsou zahrnuté do HDInsight naleznete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).

HDInsight se systémem Linux neposkytuje funkce vzdálené plochy. Místo toho můžete použít SSH tooremotely připojit hlavních uzlech clusteru toohello. Další informace najdete v tématu hello následující dokumenty:

* [Použijte Hive pomocí protokolu SSH](hdinsight-hadoop-use-hive-ssh.md)
* [Použijte Pig s SSH](hdinsight-hadoop-use-pig-ssh.md)
* [Používání nástroje MapReduce pomocí protokolu SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]
> Pokud chcete použít externí metaúložiště Hive, byste měli zálohovat hello metaúložiště před jeho použitím s HDInsight se systémem Linux. HDInsight se systémem Linux je k dispozici novější verze Hive, který může mít nekompatibilitu s metaúložiště vytvořené pomocí dřívějších verzí.

Následující graf Hello obsahuje pokyny k migraci vašich úloh Hive.

| V systému Windows použít... | Na základě Linux... |
| --- | --- |
| **Hive Editor** |[Zobrazení Ambari Hive](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`tooenable Tez |Tez hello výchozí spouštěcí modul pro clustery se systémem Linux, takže hello set – příkaz již není potřeba. |
| Uživatelem definované funkce jazyka C# | Informace o ověřování součásti C# s HDInsight se systémem Linux najdete v tématu [řešení migrovat .NET HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD soubory nebo skripty na serveru hello vyvolána jako součást úlohy Hive |pomocí skriptů Bash |
| `hive`příkaz z vzdálené plochy |Použití [Beeline](hdinsight-hadoop-use-hive-beeline.md) nebo [Hive z relace SSH](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| V systému Windows použít... | Na základě Linux... |
| --- | --- |
| Uživatelem definované funkce jazyka C# | Informace o ověřování součásti C# s HDInsight se systémem Linux najdete v tématu [řešení migrovat .NET HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD soubory nebo skripty na serveru hello volána v rámci úlohy Pig |pomocí skriptů Bash |

### <a name="mapreduce"></a>MapReduce

| V systému Windows použít... | Na základě Linux... |
| --- | --- |
| C# mapper a reduktorem součásti | Informace o ověřování součásti C# s HDInsight se systémem Linux najdete v tématu [řešení migrovat .NET HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD soubory nebo skripty na serveru hello vyvolána jako součást úlohy Hive |pomocí skriptů Bash |

## <a name="oozie"></a>Oozie

> [!IMPORTANT]
> Pokud chcete použít externí metaúložiště Oozie, byste měli zálohovat hello metaúložiště před jeho použitím s HDInsight se systémem Linux. HDInsight se systémem Linux je k dispozici novější verze Oozie, který může mít nekompatibilitu s metaúložiště vytvořené pomocí dřívějších verzí.

Pracovní postupy Oozie prostředí akce povolit. Akce prostředí využívají hello výchozí prostředí pro příkazy příkazového řádku toorun hello operačního systému. Pokud máte Oozie pracovní postupy, které jsou závislé na hello prostředí systému Windows, musí přepsat hello toorely pracovních postupů v prostředí shell Linux hello (Bash). Další informace o používání prostředí akce s Oozie najdete v tématu [rozšíření akce prostředí Oozie](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Pokud máte Oozie pracovní postupy, které jsou závislé na aplikací v C# vyvolat prostřednictvím akce prostředí, musíte ověřit tyto aplikace v prostředí Linux. Další informace najdete v tématu [řešení migrovat .NET HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| V systému Windows použít... | Na základě Linux... |
| --- | --- |
| Řídicí panel Storm |Hello řídicí panel Storm není k dispozici. V tématu [topologií nasazení a správa Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md) pro způsoby toosubmit topologie |
| Storm uživatelského rozhraní |je k dispozici na https://CLUSTERNAME.azurehdinsight.net/stormui Hello uživatelské rozhraní Storm |
| Visual Studio toocreate, nasazovat a spravovat jazyka C# nebo hybridní topologie |Visual Studio může být použité toocreate, nasadit a spravovat jazyka C# (SCP.NET) nebo hybridní topologie na Storm se systémem Linux v HDInsight clustery, které jsou vytvořené po 10/28/2016. |

## <a name="hbase"></a>HBase

V clusterech se systémem Linux, hello znode nadřazený prvek HBase je `/hbase-unsecure`. Nastavte tuto hodnotu v hello konfigurace pro všechny klientské aplikace Java, které pomocí nativních rozhraní API HBase Java.

V tématu [sestavení aplikace založené na jazyce Java HBase](hdinsight-hbase-build-java-maven.md) pro příklad klienta, který nastavuje tuto hodnotu.

## <a name="spark"></a>Spark

Clustery Spark nebyly k dispozici v clusterech Windows verzi Preview. Spark GA je dostupná jenom s clustery se systémem Linux. Neexistuje žádné cesty migrace z clusteru systému Windows Spark preview clusteru tooa verze systémem Linux Spark.

## <a name="known-issues"></a>Známé problémy

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory vlastních aktivit rozhraní .NET

Azure Data Factory vlastních aktivit .NET nejsou aktuálně podporovány na clusterech HDInsight se systémem Linux. Místo toho měli používat jednu z následujících metod tooimplement vlastní aktivity v rámci vašeho kanálu ADF hello.

* Spuštění aktivity rozhraní .NET ve fondu Azure Batch. Najdete v části služby Azure Batch pomocí propojené hello [použít vlastní aktivity v kanálu Azure Data Factory](../data-factory/data-factory-use-custom-activities.md)
* Implementovat hello aktivitu jako činnost MapReduce. Další informace najdete v tématu [vyvolání MapReduce programy z datové továrny](../data-factory/data-factory-map-reduce.md).

### <a name="line-endings"></a>Konce řádků

Konce řádků na systémy Windows se obvykle používá Line FEED, při systémy Linux používat LF. Pokud vytvořit, nebo očekávat, data s Line FEED konce řádků, musíte toowork producenti a spotřebitelé hello toomodify s hello LF řádku ukončuje.

Například pomocí prostředí Azure PowerShell tooquery HDInsight v clusteru se systémem Windows vrací data s Line FEED. Hello stejný dotaz s clusterem se systémem Linux vrátí LF. Měli byste otestovat toosee Pokud ukončuje hello řádku způsobí, že problém s vaší solutuion před migrací tooa clusteru se systémem Linux.

Pokud máte skripty, které jsou provedeny přímo na uzly clusteru Linux hello, byste měli vždycky používat LF jako hello ukončování řádků. Pokud používáte Line FEED, může se zobrazí chyby při spouštění skriptů hello v clusteru se systémem Linux.

Pokud víte, že hello skripty neobsahují řetězce s embedded znaky CR, je možné hromadně konce řádků hello změn pomocí jedné z následujících metod hello:

* **Před nahráním toohello clusteru**: hello použijte následující příkazy prostředí PowerShell, toochange konce řádků hello z Line FEED tooLF před nahráním hello skriptu toohello clusteru.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Po nahrání toohello clusteru**: použití hello následující příkaz z relace toohello SSH clusteru se systémem Linux toomodify hello skriptu.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak toocreate HDInsight se systémem Linux clusterů](hdinsight-hadoop-provision-linux-clusters.md)
* [Použití SSH tooconnect tooHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Spravovat cluster se systémem Linux pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md)
