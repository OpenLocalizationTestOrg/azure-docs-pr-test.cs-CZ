---
title: "Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet a spravovat clusterů HDInsight pomocí portálu Azure."
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
ms.openlocfilehash: c9cb631aef71f72457c3517d02566a56919f82bc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Pomocí [portál Azure][azure-portal], můžete spravovat clustery systému Hadoop v prostředí Azure HDInsight. Použijte volič karty informace o správě clusterů systému Hadoop v HDInsight pomocí jiných nástrojů.

**Požadavky**

Před zahájením tohoto článku, musíte mít následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-portal"></a>Otevřete portál
1. Přihlaste se k [https://portal.azure.com](https://portal.azure.com).
2. Po otevření portálu můžete:

   * Klikněte na tlačítko **nový** v levé nabídce na vytvoření nového clusteru:

       ![tlačítko Nový cluster HDInsight](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Klikněte na tlačítko **clustery HDInsight** v levé nabídce seznam stávajících clusterů

       ![Azure portálu tlačítko clusteru HDInsight](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Pokud nevidíte clusteru HDInsight, klikněte na tlačítko **další služby** v dolní části seznamu a pak klikněte na tlačítko **clustery HDInsight** pod **Intelligence + analýzy** části.


## <a name="create-clusters"></a>Vytváření clusterů
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight funguje s komponentami široký rozsah Hadoop. Seznam součástí, které byly ověřeny a podporovány, naleznete v části [jaká verze Hadoop je v Azure HDInsight](hdinsight-component-versioning.md). Informace o vytvoření obecné clusteru, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

Předplatné Azure je třeba zadat při vytváření clusteru HDInsight. Tento cluster lze vytvořit novou skupinu prostředků Azure nebo existující skupinu prostředků. Ověření oprávnění pro vytváření clusterů HDInsight, můžete použít následující kroky:

- Chcete-li použít existující skupinu prostředků.

    1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
    2. Klikněte na tlačítko **skupiny prostředků** v levé nabídce na seznam skupin prostředků.
    3. Klikněte na skupinu prostředků, kterou chcete použít pro vytvoření clusteru HDInsight.
    4. Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)**a ověřte, že vy (nebo skupinu, která patří do) mají alespoň Přispěvatel přístup ke skupině prostředků.

- Chcete-li vytvořit novou skupinu prostředků

    1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
    2. Klikněte na tlačítko **předplatné** v levé nabídce. Obsahuje žlutou ikonu klíče. Zobrazí se seznam předplatných.
    3. Klikněte na předplatné, který použijete k vytvoření clusterů. 
    4. Klikněte na tlačítko **Moje oprávnění**.  Zobrazuje vaše [role](../active-directory/role-based-access-control-what-is.md#built-in-roles) u předplatného. Musí být minimálně Přispěvatel přístup k vytvoření clusteru HDInsight.

Pokud se zobrazí chyba NoRegisteredProviderFound nebo MissingSubscriptionRegistration chyba, přečtěte si téma [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Seznam a zobrazit clustery
1. Přihlaste se k [https://portal.azure.com](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce seznam stávajících clusterů.
3. Klikněte na název clusteru. Pokud je seznam clusteru dlouho, můžete použít možnosti filtrovat horní části stránky.
4. Klikněte na cluster, ze seznamu na stránce Přehled v tématu:

    ![Azure portálu essentials clusteru HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * **Řídicí panel**: Otevře řídicí panel clusteru, který je Ambari Web pro clustery se systémem Linux.
    * **Secure Shell**: zobrazí pokyny pro připojení ke clusteru pomocí připojení Secure Shell (SSH).
    * **Škálování clusteru**: umožňuje změnit počet uzlů pracovního procesu pro tento cluster.
    * **Odstranit**: Odstraní clusteru.
    * **Protokoly aktivity**: zobrazení a dotaz aktivity protokoly.
    * **Přístup k ovládacímu prvku (IAM)**: použití přiřazení rolí.  V tématu [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](../active-directory/role-based-access-control-configure.md).
    * **Značky**: můžete nastavit páry klíč/hodnota k definování vlastní taxonomii cloudových služeb. Můžete například vytvořit klíč s názvem **projektu**a potom používat běžné hodnotu pro všechny služby související s konkrétní projekt.
    * **Diagnostika a řešení problémů**: Zobrazit informace o odstraňování potíží.
    * **Zamkne**: uzamčení, aby se zabránilo clusteru se přidat, upravit nebo odstranit.
    * **Skriptu pro automatizaci**: zobrazení a export šablony Azure Resource Manageru pro cluster. V současné době můžete exportovat pouze účet závislého úložiště Azure. V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Rychlý Start**: Zobrazí informace, které vám pomůžou začněte používat HDInsight.
    * **Nástroje pro HDInsight**: informace nápovědy pro HDInsight související nástroje.
    * **Cluster přihlášení**: Zobrazit informace o přihlášení clusteru.
    * **Použití jádra předplatné**: Zobrazit jádra dostupná a použitá pro vaše předplatné.
    * **Škálování clusteru**: Zvyšte a snižte počet uzlů pracovního procesu clusteru. V tématu[škálování clusterů](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Secure Shell**: zobrazí pokyny pro připojení ke clusteru pomocí připojení Secure Shell (SSH). Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **Partnera HDInsight**: Přidat nebo odebrat současného partnera HDInsight.
    * **Externí Metaúložiště**: Zobrazit metaúložiště Hive a Oozie. Metaúložiště se dá nakonfigurovat jenom během procesu vytváření clusteru. V tématu [použít metaúložiště Hive nebo Oozie](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Skript akce**: Spusťte Bash skripty v clusteru. V tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).
    * **Aplikace**: aplikace HDInsight přidat nebo odebrat.  V tématu [instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md).
    * **Vlastnosti**: zobrazení vlastností clusteru.
    * **Účty úložiště**: Zobrazit účty úložiště a klíče. Účty úložiště jsou nakonfigurované během procesu vytváření clusteru.
    * **Identita AAD clusteru**:
    * **Nová žádost o podporu**: vám umožní vytvořit lístek podpory s podporu společnosti Microsoft.
    
6. Klikněte na tlačítko **vlastnosti**:

    Mezi vlastnosti patří:

   * **Název hostitele**: název clusteru.
   * **Adresa URL clusteru**. Adresa URL pro webové rozhraní Ambari.
   * **Stav**: zahrnují byl zrušen, přijata, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, provozní, spuštěna, chyba, odstraňování, odstranit, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, ClusterCustomization CertRolloverQueued, ResizeQueued,
   * **Oblast**: umístění Azure. Seznam podporovaných umístění Azure, najdete v článku **oblast** rozevírací pole se seznamem na [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Datum vytvoření**.
   * **Operační systém**: buď **Windows** nebo **Linux**.
   * **Typ**: Hadoop, HBase, Storm, Spark.
   * **Verze**. V tématu [HDInsight verze](hdinsight-component-versioning.md)
   * **Předplatné**: název odběru.
   * **Zdroj dat výchozí**: výchozí systém souborů clusteru.
   * **Velikost uzlů pracovního procesu**.
   * **HEAD velikost uzlu**.

## <a name="delete-clusters"></a>Odstranění clusterů
Odstranění clusteru neodstraní výchozí účet úložiště nebo všechny propojené účty úložiště. Clusteru můžete znovu vytvořit pomocí stejné účty úložiště a stejné metaúložiště. Doporučujeme použít nový výchozí kontejner objektu Blob, když znovu vytvoříte cluster.

1. Přihlaste se k [portál][azure-portal].
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce. Pokud nevidíte **clustery HDInsight**, klikněte na tlačítko **další služby** první.
3. Klikněte na cluster, který chcete odstranit.
4. Klikněte na tlačítko **odstranit** z hlavní nabídky a pak postupujte podle pokynů.

Viz také [pozastavení nebo vypnutí clustery](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Přidání dalších účtů úložiště

Po vytvoření clusteru můžete přidat další účty Azure Storage a účtů Azure Data Lake Store. Další informace najdete v tématu [Přidání dalších účtů úložiště do služby HDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Škálování clusterů
Funkce škálování clusteru umožňuje změnit počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti znovu vytvořit cluster.

> [!NOTE]
> Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány. Pokud si nejste jistí na verzi vašeho clusteru, můžete zkontrolovat stránku vlastností.  V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).
>
>

Dopad změny v počtu uzlů dat pro každý typ clusteru podporuje HDInsight:

* Hadoop

    Můžete bez problémů zvýšit počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné. Nové úlohy můžete také odeslány, když probíhá operace. Selhání v rámci operace škálování pohodlné zpracování tak, aby cluster zůstane vždy ve funkčním stavu.

    Pokud se Hadoop cluster měřítko snížením počtu uzlů data, některé služby v clusteru restartovat. To způsobí, že všechny spuštěné a čeká se na úlohy selhání po dokončení operace škálování. Můžete, ale odešlete znovu úloh po dokončení operace.
* HBase

    Bezproblémově můžete přidávat nebo odebírat uzly do clusteru HBase, když je spuštěná. Místní servery jsou automaticky vyváženy během několika minut po dokončení operace škálování. Místní servery však můžete také ručně vyvážit přihlašují k headnode clusteru a spuštěním následujících příkazů z okna příkazového řádku:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Další informace o používání prostředí HBase naleznete v tématu]
* Storm

    Můžete bezproblémově přidávat nebo odebírat uzly dat do clusteru Storm, když je spuštěná. Ale po úspěšném dokončení operace škálování, budete muset znovu vyvážit topologii.

    Vyrovnává lze dosáhnout dvěma způsoby:

  * Storm webového uživatelského rozhraní
  * Nástroj pro rozhraní příkazového řádku (CLI)

    Odkazovat [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.

    Uživatelské rozhraní Storm webu je k dispozici v clusteru HDInsight:

    ![Obnovte rovnováhu škálování Storm v HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Tady je příklad jak znovu vyvážit topologie Storm pomocí rozhraní příkazového řádku příkaz:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Škálování clusterů**

1. Přihlaste se k [portál][azure-portal].
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce.
3. Klikněte na cluster, kterou chcete škálovat.
3. Klikněte na tlačítko **škálování clusteru**.
4. Zadejte **číslo pracovní uzly**. Limit počtu uzlů clusteru se liší podle předplatných Azure. Obraťte se na podporu fakturace o navýšení limitu.  Informace o nákladech odráží změny, které jste udělali počet uzlů.

    ![HDInsight hadoop hbase storm spark škálování](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Pozastavení nebo vypnutí clustery

Většina úloh Hadoop jsou dávkové úlohy, které jsou spustit pouze občas. Pro většinu clusterů systému Hadoop jsou velké období clusteru není použitá ke zpracování. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán.
Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány.

Existuje mnoho způsobů, které můžete naprogramovat proces:

* Uživatel pro vytváření dat Azure. V tématu [vytvořit na vyžádání systémem Linux Hadoop clusterů v HDInsight pomocí Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) pro vytváření HDInsight na vyžádání propojené služby.
* Použití Azure PowerShell.  V tématu [analyzovat data zpoždění letu](hdinsight-analyze-flight-delay-data.md).
* Použití Azure CLI. V tématu [Správa clusterů HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md).
* Použití sady .NET SDK HDInsight. V tématu [úloh Hadoop odeslání](hdinsight-submit-hadoop-jobs-programmatically.md).

Informace o cenách najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/). Pokud chcete odstranit cluster z portálu, přečtěte si téma [odstranění clusterů](#delete-clusters)


## <a name="upgrade-clusters"></a>Upgrade clustery

V tématu [clusteru HDInsight se Upgrade na novější verzi](./hdinsight-upgrade-cluster.md).

## <a name="change-passwords"></a>Změna hesla
Cluster služby HDInsight může mít dva uživatelské účty. HDInsight clusteru (také známa jako uživatelský účet. Uživatelský účet HTTP) a vytvoří se během procesu vytváření uživatelského účtu SSH. Chcete-li změnit clusteru uživatel účet uživatelské jméno a heslo a akce skriptu ke změně účtu uživatele SSH můžete použít webovému uživatelskému rozhraní Ambari

### <a name="change-the-cluster-user-password"></a>Změnit heslo uživatele clusteru
Webové uživatelské rozhraní Ambari slouží ke změně hesla uživatele clusteru. K přihlášení do Ambari, musíte použít existující cluster uživatelské jméno a heslo.

> [!NOTE]
> Změna hesla uživatele (správce) clusteru může způsobit, že skript, který spustil akce na tomto clusteru k selhání. Pokud máte všechny akce trvalého skriptu této cílové uzly pracovního procesu, tyto skripty mohou selhat, když přidáte, že uzly clusteru prostřednictvím změnit velikost operace. Další informace o akcí skriptů naleznete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Přihlaste se k webové uživatelské rozhraní Ambari pomocí pověření uživatele clusteru HDInsight. Výchozí uživatelské jméno **admin**. Adresa URL je **https://&lt;název clusteru HDInsight > azurehdinsight.net**.
2. Klikněte na tlačítko **správce** z hlavní nabídky a pak klikněte na tlačítko "Správa Ambari".
3. V levé nabídce klikněte na tlačítko **uživatelé**.
4. Klikněte na tlačítko **správce**.
5. Klikněte na tlačítko **změnit heslo**.

Ambari poté změní heslo na všech uzlech v clusteru.

### <a name="change-the-ssh-user-password"></a>Změnit heslo uživatele SSH
1. Pomocí textového editoru, uložte následující text jako soubor s názvem **changepassword.sh**.

   > [!IMPORTANT]
   > Musíte použít editor, který používá LF jako ukončování řádků. Pokud editor používá Line FEED, skript se nefunguje.
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Nahrajte soubor do umístění úložiště, který je přístupný z prostředí HDInsight pomocí adresy protokolu HTTP nebo HTTPS. Například soubor veřejného uložit jako je například OneDrive nebo Azure Blob storage. Uložte identifikátor URI (adresa protokolu HTTP nebo HTTPS) do souboru, jak tento identifikátor URI je potřeba v dalším kroku.
3. Z portálu Azure klikněte na tlačítko **clustery HDInsight**.
4. Klikněte na clusteru HDInsight.
4. Klikněte na tlačítko **skript akce**.
4. Z **akcí skriptů** vyberte **odeslání nové**. Když **odeslat akci se skripty** okno se zobrazí, zadejte následující informace:

   | Pole | Hodnota |
   | --- | --- |
   | Name (Název) |Ssh heslo změnit |
   | Skript bash identifikátor URI |Identifikátor URI k souboru changepassword.sh |
   | Uzly (Head, pracovního procesu, Nimbus, nadřízeného, Zookeeper atd.) |✓ pro všechny typy uzlů, které jsou uvedené |
   | Parametry |Zadejte uživatelské jméno SSH a pak nové heslo. Měla by existovat jediného místa mezi uživatelské jméno a heslo. |
   | Zachovat tuto akci skriptu... |Nechte pole nezaškrtnuté. |
5. Vyberte **vytvořit** použít skript. Po dokončení skriptu, budete moci připojit ke clusteru pomocí SSH pomocí nového hesla.

## <a name="grantrevoke-access"></a>Udělení nebo odvolání přístupu
Clustery HDInsight mají následující webové služby HTTP (všechny tyto služby mají RESTful koncových bodů):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Ve výchozím nastavení jsou tyto služby oprávnění pro přístup. Vám může odvolání nebo udělit přístup pomocí [rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) a [prostředí Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Najít ID předplatného

**K vyhledání vašeho předplatného Azure ID**

1. Přihlaste se k [portál][azure-portal].
2. Klikněte na tlačítko **odběry**. Každé předplatné má název a identifikátor.

Každý cluster je vázaný na předplatné Azure. Předplatné v clusteru se zobrazí ID **základní** dlaždici. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Najít skupinu prostředků
V režimu Azure Resource Manager se vytvoří každý cluster HDInsight pomocí Azure Resource Manager skupiny. Správce prostředků skupiny, který je součástí clusteru s podporou se zobrazí v:

* Seznam clusteru má **skupiny prostředků** sloupce.
* Cluster **základní** dlaždici.  

V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="find-the-default-storage-account"></a>Najít výchozí účet úložiště
Každý cluster HDInsight má výchozí účet úložiště. Výchozí účet úložiště a jeho klíče pro cluster se zobrazí v části **účty úložiště**. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Spuštění dotazů Hive
Nelze spustit úlohy Hive přímo z portálu Azure, ale můžete použít zobrazení na webové uživatelské rozhraní Ambari Hive.

**Ke spouštění dotazů Hive pomocí zobrazení Ambari Hive**

1. Přihlaste se k webové uživatelské rozhraní Ambari pomocí pověření uživatele clusteru HDInsight. Výchozí uživatelské jméno **admin**. Adresa URL je **https://&lt;název clusteru HDInsight > azurehdinsight.net**.
2. Otevřete zobrazení Hive, jak je znázorněno na následujícím snímku obrazovky:  

    ![Zobrazení hive HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Klikněte na tlačítko **dotazu** v hlavní nabídce.
4. Zadejte dotaz Hive v **Editor dotazů**a potom klikněte na **Execute**.

## <a name="monitor-jobs"></a>Monitorování úloh
V tématu [Správa clusterů HDInsight pomocí webového uživatelského rozhraní Ambari](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Procházet soubory
Pomocí portálu Azure, můžete procházet obsah výchozí kontejner.

1. Přihlaste se k [https://portal.azure.com](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce seznam stávajících clusterů.
3. Klikněte na název clusteru. Pokud je seznam clusteru dlouho, můžete použít možnosti filtrovat horní části stránky.
4. Klikněte na tlačítko **účty úložiště** v levé nabídce clusteru.
5. Klikněte na účet úložiště.
7. Klikněte **objekty BLOB** dlaždici.
8. Klikněte na název výchozí kontejner.

## <a name="monitor-cluster-usage"></a>Monitorování využití clusteru
**Využití** části okna clusteru HDInsight se zobrazují informace o počet jader, které jsou k dispozici pro vaše předplatné pro použití s HDInsight, jakož i počet jader přidělené k tomuto clusteru a jak jsou přiděleny pro uzly v tomto clusteru. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

> [!IMPORTANT]
> Pokud chcete monitorovat služby poskytované clusteru HDInsight, musíte použít Ambari Web nebo Ambari REST API. Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="connect-to-a-cluster"></a>Připojení ke clusteru

* [Použití Hivu se službou HDInsight](hdinsight-hadoop-use-hive-ambari-view.md)
* [Použití SSH s HDInsightem](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili některé funkce základní nástrojích. Další informace naleznete v následujících článcích:

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
