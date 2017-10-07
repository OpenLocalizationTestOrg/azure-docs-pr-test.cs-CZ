---
title: "aaaManage Hadoop clusterů v HDInsight pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate a Správa clusterů HDInsight pomocí hello portálu Azure."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: c242d43d4ccea7cf1e7be19c3f3d7ed3c4f50918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a>Správa clusterů systému Hadoop v HDInsight pomocí hello portálu Azure
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Pomocí hello [portál Azure][azure-portal], můžete spravovat clustery systému Hadoop v prostředí Azure HDInsight. Použití hello karta selektor pro informace o správě clusterů systému Hadoop v HDInsight pomocí jiných nástrojů.

**Požadavky**

Před zahájením tohoto článku, musíte mít hello následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-hello-portal"></a>Otevřete hello portálu
1. Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).
2. Po otevření hello portálu můžete:

   * Klikněte na tlačítko **nový** z hello levé nabídce toocreate do nového clusteru:

       ![tlačítko Nový cluster HDInsight](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Klikněte na tlačítko **clustery HDInsight** z hello levé nabídce toolist hello stávajících clusterů

       ![Azure portálu tlačítko clusteru HDInsight](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Pokud nevidíte clusteru HDInsight, klikněte na tlačítko **další služby** hello dolní části seznamu hello a pak klikněte na tlačítko **clustery HDInsight** pod hello **Intelligence + analýzy** oddíl.


## <a name="create-clusters"></a>Vytváření clusterů
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight funguje s komponentami široký rozsah Hadoop. Seznam hello hello součásti, které byly ověřeny a podporovány, naleznete v části [jaká verze Hadoop je v Azure HDInsight](hdinsight-component-versioning.md). Informace o vytvoření hello obecné clusteru, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

Předplatné Azure je třeba zadat při vytváření clusteru HDInsight. Tento cluster lze vytvořit novou skupinu prostředků Azure nebo existující skupinu prostředků. Následující kroky tooverify hello můžete použít pro vytváření clusterů HDInsight vaše oprávnění:

- toouse existující skupinu prostředků.

    1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
    2. Klikněte na tlačítko **skupiny prostředků** ze skupin prostředků hello levé nabídce toolist hello.
    3. Klikněte na skupinu prostředků hello chcete toouse pro vytvoření clusteru HDInsight.
    4. Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)**a ověřte, že vy (nebo skupinu, která patří do) mají alespoň hello skupiny prostředků toohello Přispěvatel přístup.

- toocreate novou skupinu prostředků

    1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
    2. Klikněte na tlačítko **předplatné** hello levé nabídce. Obsahuje žlutou ikonu klíče. Zobrazí se seznam předplatných.
    3. Klikněte na předplatné hello použít toocreate clustery. 
    4. Klikněte na tlačítko **Moje oprávnění**.  Zobrazuje vaše [role](../active-directory/role-based-access-control-what-is.md#built-in-roles) v předplatném hello. Musí být minimálně clusteru HDInsight toocreate Přispěvatel přístup.

Pokud se zobrazí chyba NoRegisteredProviderFound hello nebo hello MissingSubscriptionRegistration chyba, přečtěte si téma [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Seznam a zobrazit clustery
1. Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** z hello levé nabídce toolist hello stávajících clusterů.
3. Klikněte na název clusteru hello. Pokud seznam clusterů hello je dlouhý, můžete použít možnosti filtrovat na hello horní části stránky hello.
4. Klikněte na cluster z hello seznamu toosee hello Přehled stránky:

    ![Azure portálu essentials clusteru HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * **Řídicí panel**: řídicí panel hello clusteru se otevře, což je Ambari Web pro clustery se systémem Linux.
    * **Secure Shell**: ukazuje hello pokyny tooconnect toohello clusteru pomocí připojení Secure Shell (SSH).
    * **Škálování clusteru**: umožňuje toochange hello počet uzlů pracovního procesu pro tento cluster.
    * **Odstranit**: odstranění hello clusteru.
    * **Protokoly aktivity**: zobrazení a dotaz aktivity protokoly.
    * **Přístup k ovládacímu prvku (IAM)**: použití přiřazení rolí.  V tématu [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md).
    * **Značky**: umožňuje vám tooset klíč/hodnota páry toodefine vlastní taxonomii cloudových služeb. Můžete například vytvořit klíč s názvem **projektu**a potom používat běžné hodnotu pro všechny služby související s konkrétní projekt.
    * **Diagnostika a řešení problémů**: Zobrazit informace o odstraňování potíží.
    * **Zamkne**: přidejte zámku tooprevent hello clusteru se změněn nebo odstraněn.
    * **Skriptu pro automatizaci**: zobrazení a export šablony Azure Resource Manageru hello hello clusteru. V současné době můžete exportovat pouze účet závislého úložiště Azure hello. V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Rychlý Start**: Zobrazí informace, které vám pomůžou začněte používat HDInsight.
    * **Nástroje pro HDInsight**: informace nápovědy pro HDInsight související nástroje.
    * **Cluster přihlášení**: Zobrazit hello clusteru přihlašovací informace.
    * **Použití jádra předplatné**: zobrazení hello dostupná a použitá jader pro předplatné.
    * **Škálování clusteru**: Zvyšte a snížení hello počet uzlů pracovního procesu v clusteru. V tématu[škálování clusterů](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Secure Shell**: ukazuje hello pokyny tooconnect toohello clusteru pomocí připojení Secure Shell (SSH). Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **Partnera HDInsight**: hello přidat nebo odebrat současného partnera HDInsight.
    * **Externí Metaúložiště**: Zobrazit hello metaúložiště Hive a Oozie. metaúložiště Hello se dá nakonfigurovat jenom během procesu vytváření clusteru hello. V tématu [použít metaúložiště Hive nebo Oozie](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Skript akce**: Spusťte Bash skripty v clusteru hello. V tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).
    * **Aplikace**: aplikace HDInsight přidat nebo odebrat.  V tématu [instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md).
    * **Vlastnosti**: Zobrazit vlastnosti clusteru hello.
    * **Účty úložiště**: Zobrazit hello úložiště účtů a klíčů hello. účty úložiště Hello nakonfigurované během procesu vytváření clusteru hello.
    * **Identita AAD clusteru**:
    * **Nová žádost o podporu**: vám umožní toocreate lístek podpory s podporu společnosti Microsoft.
    
6. Klikněte na tlačítko **vlastnosti**:

    Vlastnosti Hello jsou:

   * **Název hostitele**: název clusteru.
   * **Adresa URL clusteru**. Hello adresa URL pro webové rozhraní Ambari hello.
   * **Stav**: zahrnují byl zrušen, přijata, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, provozní, spuštěna, chyba, odstraňování, odstranit, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, ClusterCustomization CertRolloverQueued, ResizeQueued,
   * **Oblast**: umístění Azure. Seznam podporovaných umístění Azure, najdete v části hello **oblast** rozevírací pole se seznamem na [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Datum vytvoření**.
   * **Operační systém**: buď **Windows** nebo **Linux**.
   * **Typ**: Hadoop, HBase, Storm, Spark.
   * **Verze**. V tématu [HDInsight verze](hdinsight-component-versioning.md)
   * **Předplatné**: název odběru.
   * **Zdroj dat výchozí**: hello výchozí systém souborů clusteru.
   * **Velikost uzlů pracovního procesu**.
   * **HEAD velikost uzlu**.

## <a name="delete-clusters"></a>Odstranění clusterů
Odstranění clusteru neodstraní hello výchozí účet úložiště nebo všechny propojené účty úložiště. Hello clusteru můžete znovu vytvořit pomocí hello stejné účty úložiště a hello stejné metaúložiště. Je doporučeno toouse nový výchozí kontejner objektu Blob když znovu vytvoříte hello clusteru.

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **clustery HDInsight** hello levé nabídce. Pokud nevidíte **clustery HDInsight**, klikněte na tlačítko **další služby** první.
3. Klikněte na tlačítko, které chcete toodelete clusteru hello.
4. Klikněte na tlačítko **odstranit** z hello horní nabídce a potom postupujte podle pokynů hello.

Viz také [pozastavení nebo vypnutí clustery](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Přidání dalších účtů úložiště

Po vytvoření clusteru můžete přidat další účty Azure Storage a účtů Azure Data Lake Store. Další informace najdete v tématu [přidejte další úložiště účtů tooHDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Škálování clusterů
Hello clusteru škálování funkce vám umožní toochange hello počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti toore – vytvoření clusteru hello.

> [!NOTE]
> Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány. Pokud si nejste jistí hello verze vašeho clusteru, můžete zkontrolovat hello stránku vlastností.  V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).
>
>

Hello dopad změny hello počet uzlů dat pro každý typ clusteru podporuje HDInsight:

* Hadoop

    Můžete zvýšit bezproblémově hello počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné. Nové úlohy můžete také odeslány, když probíhá operace hello. Selhání v rámci operace škálování pohodlné zpracování tak, aby hello clusteru vždy zůstává ve funkčním stavu.

    Pokud se Hadoop cluster měřítko snížením hello počet uzlů data, některé služby hello v clusteru hello restartovat. To způsobí, že všechny spuštěné a čeká se na úlohy toofail na dokončení hello hello operace škálování. Po dokončení operace hello může, ale odešlete znovu hello úlohy.
* HBase

    Můžete bezproblémově přidat nebo odebrat uzly clusteru HBase tooyour, když je spuštěná. Místní servery jsou automaticky vyváženy během několika minut od hello operace škálování. Ale můžete také ručně vyvážit hello místní servery přihlášením toohello headnode clusteru a spuštěné hello z okna příkazového řádku následující příkazy:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Další informace o používání prostředí HBase hello najdete v tématu]
* Storm

    Můžete bez problémů přidat nebo odebrat cluster Storm tooyour uzly dat je spuštěna. Ale po úspěšném dokončení hello operace škálování, budete potřebovat toorebalance hello topologie.

    Vyrovnává lze dosáhnout dvěma způsoby:

  * Storm webového uživatelského rozhraní
  * Nástroj pro rozhraní příkazového řádku (CLI)

    Odkazovat toohello [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.

    Hello Storm webového uživatelského rozhraní není k dispozici v clusteru HDInsight hello:

    ![Obnovte rovnováhu škálování Storm v HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Tady je příklad jak toouse hello rozhraní příkazového řádku příkaz topologie Storm toorebalance hello:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**tooscale clustery**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **clustery HDInsight** hello levé nabídce.
3. Klikněte na tlačítko hello clusteru chcete tooscale.
3. Klikněte na tlačítko **škálování clusteru**.
4. Zadejte **číslo pracovní uzly**. limit Hello hello počtu uzlů clusteru se liší podle předplatných Azure. Obraťte se na fakturační limit hello tooincrease podpory.  informace o nákladech Hello odráží hello změny provedené toohello počet uzlů.

    ![HDInsight hadoop hbase storm spark škálování](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Pozastavení nebo vypnutí clustery

Většina úloh Hadoop jsou dávkové úlohy, které jsou spustit pouze občas. Většina clusterů systému Hadoop, jsou velké tečky času hello clusteru není používán pro zpracování. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán.
Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána.

Existuje mnoho způsobů budete programu hello proces:

* Uživatel pro vytváření dat Azure. V tématu [vytvořit na vyžádání systémem Linux Hadoop clusterů v HDInsight pomocí Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) pro vytváření HDInsight na vyžádání propojené služby.
* Použití Azure PowerShell.  V tématu [analyzovat data zpoždění letu](hdinsight-analyze-flight-delay-data.md).
* Použití Azure CLI. V tématu [Správa clusterů HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md).
* Použití sady .NET SDK HDInsight. V tématu [úloh Hadoop odeslání](hdinsight-submit-hadoop-jobs-programmatically.md).

Informace o cenách hello, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/). toodelete clusteru z hello portálu, najdete v části [odstranění clusterů](#delete-clusters)


## <a name="upgrade-clusters"></a>Upgrade clustery

V tématu [Upgrade HDInsight clusteru tooa novější verze](./hdinsight-upgrade-cluster.md).

## <a name="change-passwords"></a>Změna hesla
Cluster služby HDInsight může mít dva uživatelské účty. Hello (také známa jako uživatelský účet clusteru HDInsight Uživatelský účet HTTP) a vytvoří se během procesu vytváření hello hello SSH uživatelský účet. Můžete použít hello Ambari webového uživatelského rozhraní toochange hello clusteru uživatel účet uživatelské jméno a heslo a skript akce toochange hello SSH uživatelský účet

### <a name="change-hello-cluster-user-password"></a>Změnit heslo uživatele clusteru hello
Můžete použít hello webové uživatelské rozhraní Ambari toochange hello clusteru heslo uživatele. toolog v tooAmbari, musíte použít hello existující cluster uživatelské jméno a heslo.

> [!NOTE]
> Změna hesla uživatele (správce) hello clusteru může způsobit skript, který pro tento cluster toofail byla spuštěna akce. Pokud máte jakékoli trvalého skriptu akce, které cílí na uzly pracovního procesu, tyto skripty mohou selhat, když přidáte uzly clusteru toohello prostřednictvím operace změny velikosti. Další informace o akcí skriptů naleznete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Přihlašování pomocí webové uživatelské rozhraní Ambari toohello hello přihlašovací údaje uživatele clusteru HDInsight. výchozí uživatelské jméno Hello **správce**. adresa URL je hello **https://&lt;název clusteru HDInsight > azurehdinsight.net**.
2. Klikněte na tlačítko **správce** hello hlavní nabídce a pak klikněte na tlačítko "Správa Ambari".
3. V levé nabídce hello, klikněte na **uživatelé**.
4. Klikněte na tlačítko **správce**.
5. Klikněte na tlačítko **změnit heslo**.

Ambari poté změní heslo hello ve všech uzlech v clusteru hello.

### <a name="change-hello-ssh-user-password"></a>Změnit heslo uživatele SSH hello
1. Pomocí textového editoru, uložte hello následující text jako soubor s názvem **changepassword.sh**.

   > [!IMPORTANT]
   > Musíte použít editor, který používá LF jako hello ukončování řádků. Pokud hello editor používá Line FEED, skript hello se nefunguje.
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Nahrajte hello umístění úložiště tooa v souboru, který je přístupný z prostředí HDInsight pomocí adresy protokolu HTTP nebo HTTPS. Například soubor veřejného uložit jako je například OneDrive nebo Azure Blob storage. Uložte soubor toohello hello identifikátor URI (adresa protokolu HTTP nebo HTTPS), jako tento identifikátor URI je potřeba v dalším kroku hello.
3. Z hello portálu Azure, klikněte na tlačítko **clustery HDInsight**.
4. Klikněte na clusteru HDInsight.
4. Klikněte na tlačítko **skript akce**.
4. Z hello **akcí skriptů** vyberte **odeslání nové**. Když hello **odeslat akci se skripty** okno se zobrazí, zadejte hello následující informace:

   | Pole | Hodnota |
   | --- | --- |
   | Name (Název) |Ssh heslo změnit |
   | Skript bash identifikátor URI |Hello URI toohello changepassword.sh souboru |
   | Uzly (Head, pracovního procesu, Nimbus, nadřízeného, Zookeeper atd.) |✓ pro všechny typy uzlů, které jsou uvedené |
   | Parametry |Zadejte uživatelské jméno SSH hello a pak hello nové heslo. Měla by existovat jediného místa mezi hello uživatelské jméno a heslo hello. |
   | Zachovat tuto akci skriptu... |Nechte pole nezaškrtnuté. |
5. Vyberte **vytvořit** tooapply hello skriptu. Po dokončení skriptu hello budete moct tooconnect toohello clusteru pomocí protokolu SSH s hello nové heslo.

## <a name="grantrevoke-access"></a>Udělení nebo odvolání přístupu
Clustery HDInsight mít hello následující HTTP webové služby (všechny tyto služby mají RESTful koncových bodů):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Ve výchozím nastavení jsou tyto služby oprávnění pro přístup. Vám může odvolat/grant hello přístup pomocí [rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) a [prostředí Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-hello-subscription-id"></a>Najít ID předplatného hello

**toofind vašeho předplatného Azure ID**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **odběry**. Každé předplatné má název a identifikátor.

Každý cluster je vázané tooan předplatného Azure. Hello předplatné ID se zobrazí v clusteru hello **základní** dlaždici. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="find-hello-resource-group"></a>Najít skupinu prostředků hello
V režimu Azure Resource Manager Dobrý den každý cluster HDInsight vytvoří pomocí Azure Resource Manager skupiny. skupiny Hello Resource Manager, která patří tooappears v clusteru s podporou:

* seznam Hello clusteru má **skupiny prostředků** sloupce.
* Cluster **základní** dlaždici.  

V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="find-hello-default-storage-account"></a>Najít hello výchozí účet úložiště
Každý cluster HDInsight má výchozí účet úložiště. Hello výchozí účet úložiště a jeho klíče pro cluster se zobrazí pod **účty úložiště**. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Spuštění dotazů Hive
Nelze spustit úlohy Hive přímo z hello portálu Azure, ale můžete použít hello Hive zobrazení na webové uživatelské rozhraní Ambari.

**toorun dotazů Hive pomocí zobrazení Ambari Hive**

1. Přihlašování pomocí webové uživatelské rozhraní Ambari toohello hello přihlašovací údaje uživatele clusteru HDInsight. výchozí uživatelské jméno Hello **správce**. adresa URL je hello **https://&lt;název clusteru HDInsight > azurehdinsight.net**.
2. Otevřete zobrazení Hive, jak ukazuje následující snímek obrazovky hello:  

    ![Zobrazení hive HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Klikněte na tlačítko **dotazu** hello hlavní nabídce.
4. Zadejte dotaz Hive v **Editor dotazů**a potom klikněte na **Execute**.

## <a name="monitor-jobs"></a>Monitorování úloh
V tématu [Správa clusterů HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Procházet soubory
Pomocí hello portálu Azure, můžete procházet obsah hello hello výchozí kontejner.

1. Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** z hello levé nabídce toolist hello stávajících clusterů.
3. Klikněte na název clusteru hello. Pokud seznam clusterů hello je dlouhý, můžete použít možnosti filtrovat na hello horní části stránky hello.
4. Klikněte na tlačítko **účty úložiště** hello clusteru levé nabídce.
5. Klikněte na účet úložiště.
7. Klikněte na tlačítko hello **objekty BLOB** dlaždici.
8. Klikněte na název kontejneru výchozí hello.

## <a name="monitor-cluster-usage"></a>Monitorování využití clusteru
Hello **využití** části okna clusteru HDInsight hello se zobrazují informace o hello počet jader dostupné tooyour předplatné pro použití s HDInsight, jakož i hello počet jader přidělené toothis clusteru a jak jsou přidělené hello uzlů v tomto clusteru. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

> [!IMPORTANT]
> toomonitor hello služby poskytované hello clusteru HDInsight, musíte použít Ambari Web nebo Ambari REST API hello. Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="connect-tooa-cluster"></a>Připojte tooa cluster

* [Použití Hivu se službou HDInsight](hdinsight-hadoop-use-hive-ambari-view.md)
* [Použití SSH s HDInsightem](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili některé funkce základní nástrojích. toolearn více, najdete v části hello následující články:

* [Spravovat HDInsight pomocí prostředí Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Spravovat HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md)
* [Vytvoření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Používání Hive v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig v HDInsight](hdinsight-use-pig.md)
* [Použití nástroje Sqoop v HDInsight](hdinsight-use-sqoop.md)
* [Začínáme s Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Jaká verze Hadoop je v Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop příkazového řádku"
