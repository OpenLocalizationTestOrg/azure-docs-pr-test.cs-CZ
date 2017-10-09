---
title: "hello aaaManage clustery založené na Windows Hadoop v HDInsight pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooadminister služba HDInsight. Vytvoření clusteru HDInsight, otevřete hello interaktivní konzoly pro JavaScript a otevřete hello konzoly příkazu Hadoop."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: a237726b0e37a08005ce22e96581739e93edb050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a>Správa clusterů systému Hadoop založené na systému Windows v prostředí HDInsight pomocí hello portálu Azure

Pomocí hello [portál Azure][azure-portal], můžete vytvořit clustery se systémem Windows Hadoop v Azure HDInsight, změnit heslo uživatele Hadoop a povolení protokolu RDP (Remote Desktop), dostanete hello Hadoop příkaz konzoly v clusteru hello.

Hello informace v tomto článku se vztahuje pouze na základě tooWindow clusterů HDInsight. Informace o správě clusterech se systémem Linux naleznete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure](hdinsight-administer-use-portal-linux.md).

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Požadavky

Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Účet služby Azure Storage** -clusteru HDInsight používá kontejner úložiště objektů Blob v Azure jako hello výchozí systém souborů. Další informace o tom, jak Azure Blob storage nabízí jednotné prostředí s clustery HDInsight najdete v tématu [použití Azure Blob Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md). Podrobnosti o vytvoření účtu Azure Storage najdete v tématu [jak tooCreate účet úložiště](../storage/common/storage-create-storage-account.md).

## <a name="open-hello-portal"></a>Otevřete hello portálu
1. Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).
2. Po otevření hello portálu můžete:

   * Klikněte na tlačítko **nový** z hello levé nabídce toocreate do nového clusteru:

       ![tlačítko Nový cluster HDInsight](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Klikněte na tlačítko **clustery HDInsight** hello levé nabídce.

       ![Azure portálu tlačítko clusteru HDInsight](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Pokud **HDInsight** není nezobrazí v levé nabídce hello, klikněte na tlačítko **Procházet**.

     ![Tlačítko Procházet clusteru Azure portálu](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Vytváření clusterů
Pokyny pro vytvoření hello použití hello portálu, najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight funguje s komponentami široký rozsah Hadoop. Seznam hello hello součásti, které byly ověřeny a podporovány, naleznete v části [jaká verze Hadoop je v Azure HDInsight](hdinsight-component-versioning.md). HDInsight můžete přizpůsobit pomocí jedné z hello následující možnosti:

* Použití akce skriptu toorun vlastní skripty, které můžete přizpůsobit clusteru tooeither změnit konfiguraci clusteru nebo nainstalujte vlastní komponenty, například Giraph nebo Solr. Další informace najdete v tématu [clusteru HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md).
* Použijte vlastní nastavení parametrů clusteru hello v hello SDK rozhraní .NET HDInsight nebo Azure PowerShell při vytváření clusteru. Tyto změny konfigurace se pak zachovají prostřednictvím hello životnost hello clusteru a nemá vliv reimages uzlu clusteru, které platformy Azure pravidelně provádí za účelem údržby. Další informace o používání parametrů přizpůsobení hello clusteru najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Některé nativní součásti Java, jako je Mahout a s možností, lze spustit v clusteru hello jako JAR soubory. Tyto soubory JAR může být distribuované tooAzure úložiště objektů Blob a odeslat tooHDInsight clustery prostřednictvím mechanismy odesílání úloh Hadoop. Další informace najdete v tématu [Hadoop odeslání úlohy prostřednictvím kódu programu](hdinsight-submit-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > Pokud máte problémy s nasazení clusterů tooHDInsight souborů JAR nebo volání souborů JAR na clusterech HDInsight, obraťte se na [Microsoft Support](https://azure.microsoft.com/support/options/).
  >
  > Kaskádových není podporována v prostředí HDInsight a nejsou vhodné pro systém Microsoft Support. Seznam podporovaných součásti, najdete v části [co je nového ve verzích clusterů hello poskytovaných v HDInsight](hdinsight-component-versioning.md).
  >
  >

Instalace vlastní software na hello clusteru pomocí připojení ke vzdálené ploše není podporována. Neměli byste ukládat soubory na jednotkách hello hello hlavního uzlu, jak budou ztraceny, budete potřebovat toore-vytvořit clustery se hello. Doporučujeme ukládat soubory do úložiště objektů Blob v Azure. Úložiště objektů blob je trvalé.

## <a name="list-and-show-clusters"></a>Seznam a zobrazit clustery
1. Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** hello levé nabídce.
3. Klikněte na název clusteru hello. Pokud seznam clusterů hello je dlouhý, můžete použít možnosti filtrovat na hello horní části stránky hello.
4. Dvakrát klikněte na cluster z hello seznamu tooshow hello podrobnosti.

    **Nabídky a essentials**:

    ![Azure portálu essentials clusteru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * toocustomize hello nabídky, klikněte pravým tlačítkem na libovolné místo v nabídce hello a pak klikněte na tlačítko **přizpůsobit**.
   * **Nastavení** a **všechna nastavení**: Zobrazí hello **nastavení** okně hello clusteru, což vám umožní tooaccess podrobné informace o konfiguraci pro hello cluster.
   * **Řídicí panel**, **řídicí panel clusteru** a **adresy URL: Jedná se o všechny způsoby tooaccess hello řídicí panel clusteru, který je Ambari Web pro clustery se systémem Linux. -**Zabezpečené prostředí **: Ukazuje hello pokyny tooconnect toohello clusteru pomocí připojení Secure Shell (SSH).
   * **Škálování clusteru**: umožňuje toochange hello počet uzlů pracovního procesu pro tento cluster.
   * **Odstranit**: odstranění hello clusteru.
   * **Rychlý Start**: Zobrazí informace, které vám pomohou začněte používat HDInsight.
   * ** Uživatele: Umožňuje tooset oprávnění pro *portálu správy* tohoto clusteru pro ostatní uživatele ve vašem předplatném Azure.

     > [!IMPORTANT]
     > To *pouze* ovlivňuje oprávnění a cluster toothis v hello portál Azure a nemá žádný vliv na kdo se může připojit cluster HDInsight pro toohello tooor odeslání úlohy.
     >
     >
   * **Značky**: značky povolit jste tooset klíč/hodnota páry toodefine vlastní taxonomii cloudových služeb. Můžete například vytvořit klíč s názvem **projektu**a potom používat běžné hodnotu pro všechny služby související s konkrétní projekt.
   * **Zobrazení Ambari**: tooAmbari webové odkazy.

     > [!IMPORTANT]
     > toomanage hello služby poskytované hello clusteru HDInsight, musíte použít Ambari Web nebo Ambari REST API hello. Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Využití**:

     ![Azure portálu použití clusteru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Klikněte na tlačítko **nastavení**.

    ![Azure portálu použití clusteru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Vlastnosti**: Zobrazit vlastnosti clusteru hello.
   * **Identita AAD clusteru**:
   * **Klíče k úložišti Azure**: Zobrazit hello výchozí účet úložiště a jeho klíčem. účet úložiště Hello je konfigurace během procesu vytváření clusteru hello.
   * **Cluster přihlášení**: Změňte hello clusteru HTTP uživatelské jméno a heslo.
   * **Externí Metaúložiště**: Zobrazit hello metaúložiště Hive a Oozie. metaúložiště Hello se dá nakonfigurovat jenom během procesu vytváření clusteru hello.
   * **Škálování clusteru**: Zvyšte a snížení hello počet uzlů pracovního procesu v clusteru.
   * **Vzdálená plocha**: Povolit a zakázat přístup ke vzdálené ploše (RDP) a nakonfigurovat uživatelské jméno protokolu RDP hello.  uživatelské jméno protokolu RDP Hello se musí lišit od hello HTTP uživatelské jméno.
   * **Preferovaného partnera**:

     > [!NOTE]
     > Toto je obecná seznam dostupných nastavení; Ne všechny z nich bude k dispozici pro všechny typy clusteru.
     >
     >
6. Klikněte na tlačítko **vlastnosti**:

    oddíl properties Hello uvádí hello následující:

   * **Název hostitele**: název clusteru.
   * **Adresa URL clusteru**.
   * **Stav**: zahrnují byl zrušen, přijata, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, provozní, spuštěna, chyba, odstraňování, odstranit, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, ClusterCustomization CertRolloverQueued, ResizeQueued,
   * **Oblast**: umístění Azure. Seznam podporovaných umístění Azure, najdete v části hello **oblast** rozevírací pole se seznamem na [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Data vytvořená**.
   * **Operační systém**: buď **Windows** nebo **Linux**.
   * **Typ**: Hadoop, HBase, Storm, Spark.
   * **Verze**. V tématu [HDInsight verze](hdinsight-component-versioning.md)
   * **Předplatné**: název odběru.
   * **ID předplatného**.
   * **Primární zdroj dat**. Hello účtu Azure Blob storage používá jako výchozí hello systému souborů Hadoop.
   * **Pracovní uzly cenová úroveň**.
   * **Hlavní uzel cenová úroveň**.

## <a name="delete-clusters"></a>Odstranění clusterů
Odstranění clusteru se neodstraní hello výchozí účet úložiště nebo všechny propojené účty úložiště. Hello clusteru můžete znovu vytvořit pomocí hello stejné účty úložiště a hello stejné metaúložiště.

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **Procházet vše** hello levé nabídce, klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.
3. Klikněte na tlačítko **odstranit** z hello horní nabídce a potom postupujte podle pokynů hello.

Viz také [pozastavení nebo vypnutí clustery](#pauseshut-down-clusters).

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

    Můžete bezproblémově přidat nebo odebrat uzly clusteru HBase tooyour, když je spuštěná. Místní servery jsou automaticky vyváženy během několika minut od hello operace škálování. Po přihlášení do hello headnode clusteru a spuštěné hello následujících příkazů z okna příkazového řádku však můžete také ručně vyvážit hello místní servery:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Další informace o používání prostředí HBase hello najdete v tématu]
* Storm

    Můžete bez problémů přidat nebo odebrat cluster Storm tooyour uzly dat je spuštěna. Ale po úspěšném dokončení hello operace škálování, budete potřebovat toorebalance hello topologie.

    Vyrovnává lze dosáhnout dvěma způsoby:

  * Storm webového uživatelského rozhraní
  * Nástroj pro rozhraní příkazového řádku (CLI)

    Podrobnosti najdete toohello [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.

    Hello Storm webového uživatelského rozhraní není k dispozici v clusteru HDInsight hello:

    ![Obnovte rovnováhu škálování HDInsight storm](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Tady je příklad jak toouse hello rozhraní příkazového řádku příkaz topologie Storm toorebalance hello:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**tooscale clustery**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **Procházet vše** hello levé nabídce, klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.
3. Klikněte na tlačítko **nastavení** z hello horní nabídce a pak klikněte na tlačítko **škálování clusteru**.
4. Zadejte **číslo pracovní uzly**. limit Hello hello počtu uzlu clusteru se liší podle předplatných Azure. Obraťte se na fakturační limit hello tooincrease podpory.  informace o nákladech Hello se projeví změny hello provedené toohello počet uzlů.

    ![HDInsight Hadoop HBase, Storm Spark škálování](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Pozastavení nebo vypnutí clustery
Většina úloh Hadoop jsou dávkové úlohy, které jsou jen občas spustili. Většina clusterů systému Hadoop, jsou velké tečky času hello clusteru není používán pro zpracování. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán.
Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána.

Existuje mnoho způsobů budete programu hello proces:

* Uživatel pro vytváření dat Azure. V tématu [propojená služba Azure HDInsight](../data-factory/data-factory-compute-linked-services.md) a [transformovat a analyzovat pomocí Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) pro HDInsight na vyžádání a samoobslužné definované propojené služby.
* Použití Azure PowerShell.  V tématu [analyzovat data zpoždění letu](hdinsight-analyze-flight-delay-data.md).
* Použití Azure CLI. V tématu [Správa clusterů HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md).
* Použití sady .NET SDK HDInsight. V tématu [úloh Hadoop odeslání](hdinsight-submit-hadoop-jobs-programmatically.md).

Informace o cenách hello, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/). toodelete clusteru z hello portálu, najdete v části [odstranění clusterů](#delete-clusters)

## <a name="change-cluster-username"></a>Uživatelské jméno změnit clusteru
Cluster služby HDInsight může mít dva uživatelské účty. Hello uživatelský účet clusteru HDInsight se vytvoří během procesu vytváření hello. Můžete také vytvořit uživatelský účet protokolu RDP pro přístup ke clusteru hello prostřednictvím protokolu RDP. V tématu [povolení vzdálené plochy](#connect-to-hdinsight-clusters-by-using-rdp).

**toochange hello HDInsight clusteru uživatelské jméno a heslo**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **Procházet vše** hello levé nabídce, klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.
3. Klikněte na tlačítko **nastavení** z hello horní nabídce a pak klikněte na tlačítko **clusteru přihlášení**.
4. Pokud **clusteru přihlášení** byla povolena, musíte kliknout na **zakázat**a potom klikněte na **povolit** před změnou hello uživatelské jméno a heslo...
5. Změna hello **clusteru přihlašovací jméno** nebo hello **clusteru přihlašovacího hesla**a potom klikněte na **Uložit**.

    ![HDInsight změnit clusteru uživatel uživatelské jméno heslo http uživatele](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Udělení nebo odvolání přístupu
Clustery HDInsight mít hello následující HTTP webové služby (všechny tyto služby mají RESTful koncových bodů):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Ve výchozím nastavení jsou tyto služby oprávnění pro přístup. Můžete můžete odvolat nebo udělit přístup hello z hello portálu Azure.

> [!NOTE]
> Pomocí udělení nebo odvolání přístupu hello, obnoví hello clusteru uživatelské jméno a heslo.
>
>

**toogrant nebo odvolání HTTP webové služby přístupu**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **Procházet vše** hello levé nabídce, klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.
3. Klikněte na tlačítko **nastavení** z hello horní nabídce a pak klikněte na tlačítko **clusteru přihlášení**.
4. Pokud **clusteru přihlášení** byla povolena, musíte kliknout na **zakázat**a potom klikněte na **povolit** před změnou hello uživatelské jméno a heslo...
5. Pro **uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**, zadejte hello nové uživatelské jméno a heslo (v uvedeném pořadí) pro hello cluster.
6. Klikněte na **ULOŽIT**.

    ![HDInsight celkové odebrat přístup protokolu http webové služby](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-hello-default-storage-account"></a>Najít hello výchozí účet úložiště
Každý cluster HDInsight má výchozí účet úložiště. Hello výchozí účet úložiště a jeho klíče pro cluster se zobrazí pod **nastavení**/**vlastnosti**/**klíčů k úložišti Azure**. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="find-hello-resource-group"></a>Najít skupinu prostředků hello
V režimu Azure Resource Manager hello se vytvoří každý cluster HDInsight s skupinu prostředků Azure. Skupina prostředků Azure Hello, který je součástí clusteru s podporou tooappears v:

* seznam Hello clusteru má **skupiny prostředků** sloupce.
* Cluster **základní** dlaždici.  

V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>Otevřete konzolu HDInsight dotazu
Hello konzoly dotazu HDInsight zahrnuje hello následující funkce:

* **Hive Editor**: grafickým uživatelským rozhraním A webové rozhraní pro odesílání úloh Hive.  V tématu [spouštění dotazů Hive pomocí hello dotazu konzoly](hdinsight-hadoop-use-hive-query-console.md).

    ![Editor portálu hive HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **Historie úlohy**: úloh Hadoop monitorování.  

    ![Historie úlohy portálu HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Klikněte na tlačítko **název dotazu** tooshow hello podrobnosti, včetně vlastnosti úlohy **dotaz úlohy**, a ** výstup úlohy. Můžete také stáhnout hello dotazu a pracovní stanice tooyour výstup hello.
* **Soubor prohlížeče**: Procházet hello výchozí účet úložiště a hello propojené účty úložiště.

    ![Procházet prohlížeče portálu souboru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Na snímku obrazovky hello hello  **<Account>**  typ označuje položku hello je účet úložiště Azure.  Klikněte na tlačítko hello účet název toobrowse hello soubory.
* **Uživatelské rozhraní Hadoop**.

    ![Portál HDInsight Hadoop uživatelského rozhraní](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Z **uživatelského rozhraní Hadoop*, můžete procházet soubory a zkontrolujte protokoly.
* **Yarn uživatelského rozhraní**.

    ![Portál HDInsight YARN uživatelského rozhraní](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Spuštění dotazů Hive
úlohy Hive tooran z hello portálu, klikněte na tlačítko **Hive Editor** v hello HDInsight dotazu konzoly. V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Monitorování úloh
úlohy toomonitor z hello portálu, klikněte na tlačítko **historie úlohy** v hello HDInsight dotazu konzoly. V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).

## <a name="browse-files"></a>Procházet soubory
toobrowse soubory uložené v hello výchozí účet úložiště a hello propojené účty úložiště, klikněte na tlačítko **prohlížeč souborů** v hello HDInsight dotazu konzoly. V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).

Můžete taky hello **procházet systém souborů hello** nástroj z hello **uživatelského rozhraní Hadoop** v konzole HDInsight hello.  V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Monitorování využití clusteru
Hello **využití** části okna clusteru HDInsight hello se zobrazují informace o hello počet jader dostupné tooyour předplatné pro použití s HDInsight, jakož i hello počet jader přidělené toothis clusteru a jak jsou přidělené hello uzlů v tomto clusteru. V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).

> [!IMPORTANT]
> toomonitor hello služby poskytované hello clusteru HDInsight, musíte použít Ambari Web nebo Ambari REST API hello. Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Otevřete uživatelské rozhraní pro Hadoop
toomonitor hello clusteru, systém souborů hello Procházet a naleznete v protokolech, klikněte na **uživatelského rozhraní Hadoop** v hello HDInsight dotazu konzoly. V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Otevřete Yarn uživatelského rozhraní
Klikněte na tlačítko toouse Yarn uživatelské rozhraní, **uživatelském rozhraní Yarn** v hello HDInsight dotazu konzoly. V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).

## <a name="connect-tooclusters-using-rdp"></a>Připojit tooclusters pomocí protokolu RDP
Hello přihlašovací údaje pro hello clusteru, který jste zadali při jeho vytváření udělte přístup toohello služeb v hello clusteru, ale není toohello samotného clusteru pomocí vzdálené plochy. Přístup ke vzdálené ploše můžete zapnout při zřizování clusteru nebo po zřízení clusteru. Hello pokyny o povolení funkce Vzdálená plocha při vytváření najdete v tématu [clusteru HDInsight se vytvořit](hdinsight-hadoop-provision-linux-clusters.md).

**tooenable vzdálené plochy**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **Procházet vše** hello levé nabídce, klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.
3. Klikněte na tlačítko **nastavení** z hello horní nabídce a pak klikněte na tlačítko **vzdálené plochy**.
4. Zadejte **vyprší dne**, **uživatelské jméno vzdálené plochy** a **heslo vzdálené plochy**a potom klikněte na **povolit**.

    ![HDInsight povolit zakázat konfigurace vzdálené plochy](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    Hello výchozí hodnoty pro vyprší dne je za týden.

   > [!NOTE]
   > Můžete taky hello SDK rozhraní .NET HDInsight tooenable vzdálené plochy na clusteru. Použití hello **EnableRdp** metoda hello HDInsight klienta objektu ve hello následujícím způsobem: **klienta. EnableRdp (název clusteru, místo, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**. Podobně toodisable vzdálené plochy na hello clusteru, můžete použít **klienta. DisableRdp (název clusteru, umístění)**. Další informace o těchto metodách v tématu [HDInsight .NET SDK – referenční informace](http://go.microsoft.com/fwlink/?LinkId=529017). Tuto možnost lze použít pouze pro clustery služby HDInsight v systému Windows.
   >
   >

**tooconnect tooa clusteru pomocí protokolu RDP**

1. Přihlaste se toohello [portál][azure-portal].
2. Klikněte na tlačítko **Procházet vše** hello levé nabídce, klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.
3. Klikněte na tlačítko **nastavení** z hello horní nabídce a pak klikněte na tlačítko **vzdálené plochy**.
4. Klikněte na tlačítko **Connect** a postupujte podle pokynů hello. Pokud připojení je zakázat, musíte ji nejprve povolit. Ujistěte se, že pomocí hello vzdálené plochy uživatele, uživatelského jména a hesla.  Nelze použít pověření uživatele hello clusteru.

## <a name="open-hadoop-command-line"></a>Otevřete příkazový řádek Hadoop
tooconnect toohello clusteru pomocí vzdálené plochy a použití hello Hadoop příkazového řádku, musíte nejprve povolíte clusteru toohello přístupu ke vzdálené ploše jak je popsáno v předchozí části hello.

**tooopen Hadoop příkazového řádku**

1. Připojte toohello cluster pomocí vzdálené plochy.
2. Z plochy hello, dvakrát klikněte na **Hadoop příkazového řádku**.

    ![HDI. HadoopCommandLine][image-hadoopcommandline]

    Další informace o Hadoop příkazů najdete v tématu [Hadoop příkazy odkaz](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

V předchozím snímku obrazovky hello název složky hello má číslo verze Hadoop hello vložených. číslo verze Hello můžete změněné na základě hello verzi komponent systému Hadoop hello nainstalovat na clusteru hello. Hadoop prostředí proměnné toorefer toothose složky můžete použít. Například:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak hello toocreate clusteru HDInsight pomocí portálu, a jak tooopen hello nástroj příkazového řádku Hadoop. toolearn více, najdete v části hello následující články:

* [Spravovat HDInsight pomocí prostředí Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Spravovat HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md)
* [Vytvoření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Odesílání úloh Hadoop prostřednictvím kódu programu](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Začínáme s Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Jaká verze Hadoop je v Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop příkazového řádku"
