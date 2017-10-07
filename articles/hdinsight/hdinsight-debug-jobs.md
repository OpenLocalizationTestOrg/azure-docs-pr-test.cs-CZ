---
title: "Ladění Hadoop v HDInsight: Zobrazit protokoly a interpretovat chybové zprávy - Azure | Microsoft Docs"
description: "Další informace o hello chybové zprávy, které vám mohou být při správě HDInsight pomocí prostředí PowerShell a kroky, které můžete provést toorecover."
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: mumian
documentationcenter: 
ms.assetid: 7e6ceb0e-8be8-4911-bc80-20714030a3ad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
ms.openlocfilehash: 2f12a40e9dcac32ce2e11d66d60d8b3b212ecb53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-hdinsight-logs"></a>Analýza protokolů služby HDInsight
Každý cluster Hadoop v prostředí Azure HDInsight má účet úložiště Azure používat jako výchozí systém souborů hello. účet úložiště Hello se označuje jako hello výchozí účet úložiště. Cluster používá hello Azure Table storage a hello úložiště objektů Blob na hello výchozí úložiště účet toostore protokoly.  toofind out hello výchozí účet úložiště pro cluster, najdete v části [spravovat Hadoop clusterů v HDInsight](hdinsight-administer-use-management-portal.md#find-the-default-storage-account). protokoly Hello zachovat v hello účet úložiště i po odstranění clusteru hello.

## <a name="logs-written-tooazure-tables"></a>Protokoly zapisují tooAzure tabulky
Hello protokoly zapisují tooAzure tabulky poskytují jednu úroveň přehled o dění pomocí clusteru služby HDInsight.

Při vytváření clusteru služby HDInsight, 6 tabulky se vytvářejí automaticky pro clustery se systémem Linux v hello výchozí tabulky úložiště:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

3 tabulky jsou vytvořeny pro clustery se systémem Windows:

* setuplog: protokolu událostí výjimek/došlo při zřizování nebo nastavení služby clusterů HDInsight.
* hadoopinstalllog: protokolu událostí výjimek/došlo při instalaci hello clusteru Hadoop. Tato tabulka může být užitečné při ladění problémů souvisejících s tooclusters vytvořit pomocí vlastních parametrů.
* hadoopservicelog: protokolu události nebo výjimek zaznamenaného všechny služby Hadoop. Tato tabulka může být užitečné při ladění problémů souvisejících toojob chyby v clusterech prostředí HDInsight.

názvy souborů Hello tabulky jsou **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Tyto tabulky obsahuje hello následující pole:

* ClusterDnsName
* Název součásti –
* eventTimestamp
* Hostitel
* MALoggingHash
* Zpráva
* N
* PreciseTimeStamp
* Role
* RowIndex
* Klienta
* ČASOVÉ RAZÍTKO
* TraceLevel

### <a name="tools-for-accessing-hello-logs"></a>Nástroje pro přístup k hello protokoly
Nejsou k dispozici pro přístup k datům v těchto tabulkách celou řadu nástrojů:

* Visual Studio
* Azure Storage Explorer
* Power Query pro Excel

#### <a name="use-power-query-for-excel"></a>Použijte Power Query pro Excel
Power Query lze nainstalovat z [www.microsoft.com/en-us/download/details.aspx?id=39379](http://www.microsoft.com/en-us/download/details.aspx?id=39379). V tématu hello stáhnout stránku hello požadavky na systém

**Power Query tooopen toouse a analyzovat protokol služby hello**

1. Otevřete **aplikace Microsoft Excel**.
2. Z hello **Power Query** nabídky, klikněte na tlačítko **z Azure**a potom klikněte na **z Microsoft Azure Table storage**.
   
    ![HDInsight Hadoop Excel PowerQuery otevřete Azure Table storage](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Zadejte název účtu úložiště hello. To může být krátký název hello nebo hello plně kvalifikovaný název domény.
4. Zadejte klíč účtu úložiště hello. Zobrazí se seznam tabulek:
   
    ![HDInsight Hadoop protokolů uložených v Azure Table storage](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Klikněte pravým tlačítkem na hello hadoopservicelog tabulky v hello **Navigátor** panelu a vyberte **upravit**. Zobrazí se 4 sloupce. Volitelně můžete odstranit hello **klíč oddílu**, **klíč řádku**, a **časové razítko** sloupce je vyberete a poté na **odebrat sloupce** z možností hello hello pásu karet.
6. Klikněte na tlačítko hello rozbalte ikonu na hello obsahu sloupce toochoose hello sloupce, které chcete tooimport do tabulky Excel hello. Pro tento ukázkový vybrali jste TraceLevel a ComponentName: ho můžete mě některých základních informací, na kterém byl součástí problémy.
   
    ![Protokoly HDInsight Hadoop zvolit sloupce](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Klikněte na tlačítko **OK** tooimport hello data.
8. Vyberte hello **TraceLevel**, Role, a **ComponentName** sloupce a pak klikněte na tlačítko **Group By** ovládacího prvku hello pásu karet.
9. Klikněte na tlačítko **OK** v hello Group By dialogové okno
10. Klikněte na tlačítko ** Použít & Zavřít **.

Nyní můžete aplikaci Excel toofilter a řazení podle potřeby. Samozřejmě můžete tooinclude ostatních sloupců (např. zprávy) v pořadí toodrill dolů do problémy když k nim dojde, ale výběru a seskupování sloupců hello popsané výše poskytuje dostatečnou přehled o co se děje s služby Hadoop. Hello stejné nápad můžou být použité toohello setuplog a hadoopinstalllog tabulky.

#### <a name="use-visual-studio"></a>Použití Visual Studia
**toouse Visual Studio**

1. Otevřete sadu Visual Studio.
2. Z hello **zobrazení** nabídky, klikněte na tlačítko **Průzkumník cloudu**. Nebo jednoduše klikněte na tlačítko **CTRL +\, CTRL + X**.
3. Z **Průzkumník cloudu**, vyberte **typy prostředků**.  Hello jiných k dispozici možnost je **skupiny prostředků**.
4. Rozbalte položku **účty úložiště**, hello výchozí účet úložiště pro cluster a potom **tabulky**.
5. Klikněte dvakrát na **hadoopservicelog**.
6. Přidání filtru. Například:
   
        TraceLevel eq 'ERROR'
   
    ![Protokoly HDInsight Hadoop zvolit sloupce](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Další informace o vytváření filtrů najdete v tématu [vytvořit filtr řetězce pro hello návrháře tabulky](../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-tooazure-blob-storage"></a>Protokoly Written tooAzure úložiště objektů Blob
[Hello protokoly napsané tooAzure tabulky](#log-written-to-azure-tables) zadejte jednu úroveň přehled o dění pomocí clusteru služby HDInsight. Tyto tabulky však neposkytuje protokoly úrovni úkolů, které mohou být užitečné při procházení další problémy, když k nim dojde. tooprovide tento další úroveň podrobností, HDInsight clustery jsou nakonfigurovaný účet úložiště objektů Blob toowrite úloh protokoly tooyour pro všechny úlohy, které je odeslána prostřednictvím Templeton. Prakticky to znamená, úlohy, odeslané pomocí rutin prostředí Azure PowerShell Microsoft hello nebo hello úlohy odeslání rozhraní API technologie .NET, není úlohy, odeslané prostřednictvím protokolu RDP nebo příkazového-řádku toohello clusteru přístupu. 

protokoly hello tooview, najdete v části [protokolů YARN přístup k aplikaci v HDInsight se systémem Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md).

Další informace o protokoly aplikací najdete v tématu [zjednodušit správu přihlášení uživatele a přístup v YARN](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

## <a name="view-cluster-health-and-job-logs"></a>Zobrazení stavu a úlohu protokoly clusteru
### <a name="access-hadoop-ui"></a>Přístup k Hadoop uživatelského rozhraní
Z hello portálu Azure klikněte na tlačítko HDInsight clusteru název tooopen hello clusteru okno. V okně hello clusteru, klikněte na tlačítko **řídicí panel**.

![Spusťte řídicí panel clusteru](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard.png)

Po zobrazení výzvy zadejte přihlašovací údaje Správce clusteru hello. V hello konzoly dotazu, které se otevře, klikněte na **uživatelského rozhraní Hadoop**.

![Spuštění uživatelského rozhraní pro Hadoop](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard-hadoop-ui.png)

### <a name="access-hello-yarn-ui"></a>Přístup k hello Yarn uživatelského rozhraní
Z hello portálu Azure klikněte na tlačítko HDInsight clusteru název tooopen hello clusteru okno. V okně hello clusteru, klikněte na tlačítko **řídicí panel**. Po zobrazení výzvy zadejte přihlašovací údaje Správce clusteru hello. V hello konzoly dotazu, které se otevře, klikněte na **uživatelském rozhraní YARN**.

Můžete použít hello uživatelském rozhraní YARN toodo hello následující:

* **Získat stav clusteru**. V levém podokně hello, rozbalte položku **clusteru**a klikněte na tlačítko **o**. Tato přítomen clusteru podrobnosti o stavu jako přidělené paměti jádra používá, celkový stav hello správce prostředků clusteru, cluster verze atd.
  
    ![Spusťte řídicí panel clusteru](./media/hdinsight-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Získat stav uzlu**. V levém podokně hello, rozbalte položku **clusteru**a klikněte na tlačítko **uzly**. Rutina Vypíše seznam všech hello uzlech v clusteru hello, HTTP adresa každý uzel, uzel prostředky přidělené tooeach atd.
* **Monitorovat stav úlohy**. V levém podokně hello, rozbalte položku **clusteru**a potom klikněte na **aplikace** toolist všechny úlohy v clusteru hello hello. Pokud chcete, aby toolook v úloh v určitém stavu (například nové, odeslaná, spuštěná atd.), klikněte na příslušný odkaz hello pod **aplikace**. Dále můžete kliknout na toofind název úlohy hello Další informace o hello úlohy takové včetně výstup hello, protokoly, atd.

### <a name="access-hello-hbase-ui"></a>Přístup k hello HBase uživatelského rozhraní
Z hello portálu Azure klikněte na tlačítko HDInsight HBase clusteru název tooopen hello clusteru okno. V okně hello clusteru, klikněte na tlačítko **řídicí panel**. Po zobrazení výzvy zadejte přihlašovací údaje Správce clusteru hello. V hello konzoly dotazu, které se otevře, klikněte na **uživatelského rozhraní HBase**.

## <a name="hdinsight-error-codes"></a>Kódy chyb HDInsight
Hello chybové zprávy uvedeno v této části jsou uvedeny možné chybové stavy, které narazí při správě služby hello pomocí Azure PowerShell a tooadvise pochopit toohelp hello uživatelé systému Hadoop v prostředí Azure HDInsight je na kroky hello které můžete provést toorecover z chyby hello.

Některé z těchto chybových zpráv může se zobrazí také v hello portálu Azure při použité toomanage clustery HDInsight. Další chybové zprávy, které můžete narazit, ale jsou méně granulární z důvodu omezení toohello hello nápravné akce v tomto kontextu. Další chybové zprávy jsou uvedeny v hello kontexty, kde je zřejmé zmírnění hello. 

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Popis**: Zadejte prosím podrobnosti databáze Azure SQL pro alespoň jedna součást v pořadí toouse vlastní nastavení pro metaúložiště Hive a Oozie.
* **Zmírnění dopadů**: hello uživatel potřebuje toosupply platným požadavkem hello SQL Azure metaúložiště a zkuste to znovu.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Popis**: Nepodařilo se vytvořit cluster v oblasti *nameOfYourRegion*. Použijte platný oblast Hdinsightu a opakujte žádost.
* **Zmírnění dopadů**: zákazníka měli vytvořit oblast hello clusteru, který je aktuálně podporuje: jihovýchodní Asie, západní Evropa, Severní Evropa, východní USA nebo západní USA.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Popis**: hello server nemohl najít hello požadovaný záznam clusteru.  
* **Zmírnění dopadů**: hello operaci opakujte.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Popis**: název DNS clusteru *yourDnsName* je neplatný. Zkontrolujte, zda název spustí a končí alfanumerické znaky a může obsahovat pouze '-' speciální znak  
* **Zmírnění dopadů**: Ujistěte se, zda používáte platný název DNS pro váš cluster, který začíná a končí alfanumerické znaky a obsahuje žádné speciální znaky jiné než hello dash '-' a zopakujte operaci hello.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Popis**: název clusteru *yourClusterName* není k dispozici. Vyberte prosím jiný název.  
* **Zmírnění dopadů**: hello uživatele nesmí zadat název clusteru, který je jedinečný a existují a opakujte. Pokud uživatel hello používá hello portál, vytvoříte hello uživatelské rozhraní vás upozorní, je-li název clusteru je již používán během hello kroky.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Popis**: clusteru heslo je neplatné. Heslo musí mít minimálně 10 znaků a musí obsahovat aspoň jednu číslici, velké písmeno, malé písmeno a speciální znak bez mezer a nesmí obsahovat uživatelské jméno hello jako jeho součástí.  
* **Zmírnění dopadů**: Zadejte platný clusteru heslo a zkuste operaci zopakovat hello.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Popis**: uživatelské jméno clusteru je neplatná. Zkontrolujte, zda uživatelské jméno nebude obsahovat speciální znaky ani mezery.  
* **Zmírnění dopadů**: zadat uživatelské jméno, platné clusteru a opakujte operaci hello.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Popis**: název DNS clusteru *yourDnsClusterName* je neplatný. Zkontrolujte, zda název spustí a končí alfanumerické znaky a může obsahovat pouze '-' speciální znak  
* **Zmírnění dopadů**: Zadejte platné uživatelské jméno DNS clusteru a opakujte operaci hello.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Popis**: název kontejneru v identifikátoru URI *yourcontainerURI* a název DNS *yourDnsName* v žádosti subjekt musí být hello stejné.  
* **Zmírnění dopadů**: Zkontrolujte, že váš název kontejneru a název DNS jsou hello stejné a hello operaci opakujte.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Popis**: Konfigurace clusteru neplatný. Nelze toofind definice uzlu všech dat v velikost uzlu.  
* **Zmírnění dopadů**: hello operaci opakujte.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Popis**: odstranění nasazení selhalo pro hello clusteru  
* **Zmírnění dopadů**: hello odstranění operaci opakujte.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Popis**: služby Chyba konfigurace. Požadované informace mapování DNS nebyl nalezen.  
* **Zmírnění dopadů**: odstranění clusteru a vytvoření nového clusteru.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Popis**: Duplicitní pokusu o vytvoření clusteru kontejneru. Existuje záznam pro *nameOfYourContainer* ale značky etag binárním rozsáhlým se neshodují.
* **Zmírnění dopadů**: Zadejte jedinečný název pro operace vytvoření kontejneru hello a zkuste to znovu hello.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Popis**: hostovaná služba *nameOfYourHostedService* již obsahuje clusteru. Hostovaná služba nemůže obsahovat více clusterů  
* **Zmírnění dopadů**: hostitelský cluster hello na jiné hostované služby.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Popis**: hello server nemohl aktualizovat stav hello hello nasazení clusteru.  
* **Zmírnění dopadů**: hello operaci opakujte. V takovém případě vícekrát, kontaktujte CSS.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Popis**: clusteru *yourClusterName* byla odstraněna jako součást údržby. Vytvořte znovu hello clusteru.
* **Zmírnění dopadů**: vytvořte ho znovu hello clusteru.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Popis**: Konfigurace clusteru neplatný. Požadované konfigurace hlavního uzlu nebyla nalezena v velikostí uzlu.
* **Zmírnění dopadů**: hello operaci opakujte.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Popis**: nelze toocreate hostovaná služba *nameOfYourHostedService*. Opakujte žádost.  
* **Zmírnění dopadů**: opakujte hello žádost.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Popis**: hostované služby *nameOfYourHostedService* již produkčním nasazení. Hostovaná služba nemůže obsahovat více nasazení v produkčním prostředí. Opakujte žádost hello s jiný název clusteru.
* **Zmírnění dopadů**: použijte jiný název clusteru a opakujte žádost hello.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Popis**: hostované služby *nameOfYourHostedService* pro hello cluster se nepodařilo najít.  
* **Zmírnění dopadů**: Pokud hello clusteru je v chybovém stavu, odstraňte jej a akci opakujte.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Popis**: hostované služby *nameOfYourHostedService* nemá žádné přidružené nasazení.  
* **Zmírnění dopadů**: Pokud hello clusteru je v chybovém stavu, odstraňte jej a akci opakujte.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Popis**: hello SubscriptionId *yourSubscriptionId* nemá jader levém toocreate clusteru *yourClusterName*. Požadováno: *resourcesRequired*, k dispozici: *resourcesAvailable*.  
* **Zmírnění dopadů**: Uvolněte prostředky ve vašem předplatném nebo zvýšit hello prostředky k dispozici toohello předplatné a opakujte toocreate hello clusteru.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Popis**: ID předplatného *yourSubscriptionId* nemá kvóty pro nový cluster toocreate HostedService *yourClusterName*.  
* **Zmírnění dopadů**: Uvolněte prostředky ve vašem předplatném nebo zvýšit hello prostředky k dispozici toohello předplatné a opakujte toocreate hello clusteru.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Popis**: hello serveru došlo k vnitřní chybě. Opakujte žádost.  
* **Zmírnění dopadů**: opakujte hello žádost.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Popis**: umístění úložiště Azure *dataRegionName* není platné umístění. Ujistěte se, že oblast hello je správný a opakujte žádost.
* **Zmírnění dopadů**: Vyberte umístění úložiště, který podporuje HDInsight, zkontrolujte, jestli je váš cluster společně umístěné a opakujte operaci hello.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Popis**: velikost neplatný virtuálního počítače pro datové uzly. Pro všechny uzly dat je podporována pouze velikost "Velký virtuálního počítače".  
* **Zmírnění dopadů**: Určete velikost uzlu hello podporované pro hello datový uzel a opakujte operaci hello.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Popis**: velikost neplatný virtuálního počítače pro hlavního uzlu. Pouze velikost 'ExtraLarge virtuálního počítače, je podporována pro hlavního uzlu.  
* **Zmírnění dopadů**: Určete velikost uzlu hello podporované pro hello hlavní uzel a opakujte operaci hello

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Popis**: ID předplatného *yourSubscriptionId* používá operace odstranění tooexecute dostatečná oprávnění pro cluster nemá *yourClusterName*.  
* **Zmírnění dopadů**: Pokud hello clusteru je v chybovém stavu, zahoďte ho a potom akci opakujte.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Popis**: název kontejneru objektu blob účtu externího úložiště *yourContainerName* je neplatný. Zajistěte, aby název začíná písmenem a obsahuje jenom malá písmena, číslice a pomlčky.  
* **Zmírnění dopadů**: Zadejte název kontejneru objektu blob účet platný úložiště a opakujte operaci hello.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Popis**: konfigurace pro externí účet úložiště *yourStorageAccountName* je požadovaná toohave podrobnosti tajné klíče toobe sady.  
* **Zmírnění dopadů**: Zadejte platný tajný klíč pro účet úložiště hello a opakujte operaci hello.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Popis**: Hlavička verze *yourVersionHeader* není v platném formátu rrrr mm-dd.  
* **Zmírnění dopadů**: Zadejte platný formát pro verze hlavičky hello a opakujte žádost hello.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Popis**: Konfigurace clusteru neplatný. Nalezena minimálně jedna konfigurace hlavního uzlu.  
* **Zmírnění dopadů**: Konfigurace hello upravit tak, že pouze jeden hlavního uzlu je zadán.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Popis**: hello operaci nelze dokončit, v rámci hello povolené čas nebo hello maximální počet opakování pokusů o zadání možné. Opakujte žádost.  
* **Zmírnění dopadů**: opakujte hello žádost.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Popis**: Parametr *yourParameterName* nemůže být null nebo prázdný.  
* **Zmírnění dopadů**: Zadejte platnou hodnotu pro parametr hello.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Popis**: jeden nebo více vstupů žádost o vytvoření clusteru hello není platný. Zajistěte, aby hello vstupní hodnoty jsou správné a opakujte žádost.  
* **Zmírnění dopadů**: Zkontrolujte, zda hello vstupní hodnoty jsou správné a opakujte žádost.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Popis**: oblast funkce není k dispozici pro oblast *yourRegionName* a ID předplatného *yourSubscriptionId*.  
* **Zmírnění dopadů**: určit oblasti, která podporuje clustery HDInsight. Hello veřejně podporované oblasti: jihovýchodní Asie, západní Evropa, Severní Evropa, východní USA nebo západní USA.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Popis**: účet úložiště *yourStorageAccountName* je v oblasti *currentRegionName*. Musí být stejné jako hello clusteru oblast *yourClusterRegionName*.  
* **Zmírnění dopadů**: buď zadejte účet úložiště v hello stejné oblasti, kterou váš cluster nebo pokud vašich dat je již v účtu úložiště hello, vytvořte nový cluster v hello stejné oblasti jako hello stávající účet úložiště. Pokud používáte portál, hello hello uživatelské rozhraní vás upozorní, je tento problém předem.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Popis**: zadané ID předplatného *yourSubscriptionId* není aktivní.  
* **Zmírnění dopadů**: předplatné znovu aktivujete nebo získat nové platné předplatné.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Popis**: ID předplatného *yourSubscriptionId* nebyl nalezen.  
* **Zmírnění dopadů**: Zkontrolujte, zda si ID předplatného je platný a hello operaci opakujte.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Popis**: nelze tooresolve DNS *yourDnsUrl*. Ověřte, zda hello plně kvalifikovaná adresa URL pro koncový bod hello objektů blob je k dispozici.  
* **Zmírnění dopadů**: Zadejte adresu URL platný objekt blob. Hello adresa URL musí být plně platný, včetně počínaje *http://* a končící na *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Popis**: nelze tooverify umístění prostředku *yourDnsUrl*. Ověřte, zda hello plně kvalifikovaná adresa URL pro koncový bod hello objektů blob je k dispozici.  
* **Zmírnění dopadů**: Zadejte adresu URL platný objekt blob. Hello adresa URL musí být plně platný, včetně počínaje *http://* a končící na *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Popis**: verze funkce není k dispozici pro verzi *specifiedVersion* a ID předplatného *yourSubscriptionId*.  
* **Zmírnění dopadů**: Zvolte verzi, která je k dispozici a opakujte operaci hello.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Popis**: verze *specifiedVersion* není podporována.
* **Zmírnění dopadů**: Zvolte verzi, která je podporována a opakujte operaci hello.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Popis**: verze *specifiedVersion* není k dispozici v oblasti Azure *specifiedRegion*.  
* **Zmírnění dopadů**: Zvolte verzi, která je podporována v zadané oblasti hello a opakujte operaci hello.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Popis**: Konfigurace clusteru neplatný. Požadované WASB účet konfigurace nebyla nalezena v externích účtů.  
* **Zmírnění dopadů**: Ověřte, zda text hello účet existuje a je v konfiguraci a opakujte operaci hello správně zadán.

## <a name="next-steps"></a>Další kroky
* [Pomocí zobrazení Ambari toodebug Tez úloh v HDInsight](hdinsight-debug-ambari-tez-view.md)
* [Povolit výpisů paměti haldy pro služby Hadoop v HDInsight se systémem Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Správa clusterů HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md)

