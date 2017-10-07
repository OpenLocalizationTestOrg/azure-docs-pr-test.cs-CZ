---
title: "aaaConfigure samostatný cluster Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak tooconfigure samostatné nebo privátní cluster Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dekapur
ms.openlocfilehash: ce2ad387162a05668bbd3a271c754776fe471850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Nastavení konfigurace pro samostatný cluster Windows
Tento článek popisuje, jak pomocí clusteru samostatné Service Fabric tooconfigure hello ***souboru ClusterConfig.JSON*** souboru. Tyto informace toospecify souboru jako uzly hello Service Fabric a jejich IP adresy, různé typy uzlů můžete použít na hello clusteru, konfigurace zabezpečení hello, jakož i hello topologie sítě z hlediska selhání nebo upgradovat domén pro vaše samostatnou cluster.

Pokud jste [Stáhnout balíček Service Fabric samostatné hello](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), několik ukázky hello souboru ClusterConfig.JSON souboru jsou stažené tooyour pracovní počítače. Ukázky Hello s *DevCluster* v jejich názvy vám pomůže vytvořit cluster s všechny tři uzly na stejný počítač, jako jsou logické uzly hello. Z toho musí být nejméně jeden uzel označen jako primárního uzlu. Tento cluster je vhodný pro vývoj nebo testovací prostředí a není podporován jako provozní cluster. Ukázky Hello s *MultiMachine* v jejich názvy vám pomůže vytvořit cluster produkční kvality s každý uzel na samostatný počítač.

1. *ClusterConfig.Unsecure.DevCluster.JSON* a *ClusterConfig.Unsecure.MultiMachine.JSON* ukazují, jak toocreate test zabezpečená nebo produkčního prostředí clusteru v uvedeném pořadí. 
2. *ClusterConfig.Windows.DevCluster.JSON* a *ClusterConfig.Windows.MultiMachine.JSON* zobrazit jak toocreate testu nebo produkční clusteru zabezpečené pomocí [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).
3. *ClusterConfig.X509.DevCluster.JSON* a *ClusterConfig.X509.MultiMachine.JSON* zobrazit jak toocreate testu nebo produkční clusteru zabezpečené pomocí [X509 zabezpečení na základě certifikátu](service-fabric-windows-cluster-x509-security.md). 

Teď vyzkoušíme hello různé části ***souboru ClusterConfig.JSON*** souboru, jak je uvedeno níže.

## <a name="general-cluster-configurations"></a>Konfigurace obecných clusteru
Toto se vztahuje hello široký clusteru určité konfigurace, jak je uvedené v následující fragment hello JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

Cluster Service Fabric tooyour žádné popisný název můžete udělit přiřazením toohello **název** proměnné. Hello **clusterConfigurationVersion** je číslo verze hello clusteru; je třeba zvýšit pokaždé, když upgradujete cluster Service Fabric. Ale nechat hello **apiVersion** toohello výchozí hodnotu.

<a id="clusternodes"></a>

## <a name="nodes-on-hello-cluster"></a>Uzly v clusteru hello
Můžete nakonfigurovat hello uzly v clusteru Service Fabric pomocí hello **uzly** jako hello následující fragment kódu ukazuje.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Cluster Service Fabric musí obsahovat aspoň 3 uzly. Můžete přidat další uzly toothis oddíl podle vašeho nastavení. Hello následující tabulka vysvětluje hello nastavení konfigurace pro každý uzel.

| **Konfigurace uzlu** | **Popis** |
| --- | --- |
| nodeName |Můžete udělit libovolného uzlu toohello popisný název. |
| IP adresa |Zjistěte, tím otevřete okno příkazového řádku a zadáte hello IP adresu vašeho uzlu `ipconfig`. Poznamenejte si adresu hello IPV4 a přiřaďte ho toohello **iPAddress** proměnné. |
| nodeTypeRef |Každý uzel může být přiřazen jiný uzel typu. Hello [typy uzlů](#nodetypes) jsou definovány v následující části hello. |
| faultDomain |Chyby domény povolit clusteru správci toodefine hello fyzickém uzlu, který může selhat v hello stejný čas kvůli tooshared fyzické závislosti. |
| upgradeDomain |Domén upgradu popisují sadu uzlů, které jsou vypnuty pro Service Fabric upgraduje na o hello stejný čas. Můžete toowhich tooassign které uzly domén upgradu, jako nejsou omezeny všechny fyzické požadavky. |

## <a name="cluster-properties"></a>Vlastnosti clusteru
Hello **vlastnosti** je oddíl v souboru ClusterConfig.JSON hello použité tooconfigure hello clusteru následujícím způsobem.

<a id="reliability"></a>

### <a name="reliability"></a>Spolehlivost
Koncept Hello **reliabilityLevel** definuje hello počet replik nebo instancí hello Service Fabric systémových služeb, které můžou běžet na hello primární uzly clusteru hello. To určuje spolehlivost hello z těchto služeb a proto hello clusteru. Hodnota Hello je počítanou hello systému v době vytváření a upgrade clusteru.

### <a name="diagnostics"></a>Diagnostika
Hello **diagnosticsStore** část vám tooconfigure parametry tooenable Diagnostika a řešení potíží uzel nebo selhání clusteru, jak je znázorněno v následujícím fragmentu kódu hello. 

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

Hello **metadata** popis vašeho clusteru diagnostiky a může nastavit podle vašeho nastavení. Tyto proměnné pomáhají při shromažďování ETW protokolů trasování, výpisy stavu systému a také čítače výkonu. Čtení [protokolu trasování](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) a [trasování ETW](https://msdn.microsoft.com/library/ms751538.aspx) protokoly trasování pro další informace o trasování událostí pro Windows. Všechny protokoly, včetně [havárií výpisy](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) a [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) může být směrovanou toohello **connectionString** složky v počítači. Můžete také použít *azurestorage* pro ukládání diagnostiky. Níže najdete fragment ukázka.

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Zabezpečení
Hello **zabezpečení** je nezbytné pro cluster Service Fabric zabezpečené samostatné části. Hello následující fragment kódu ukazuje součástí této části.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

Hello **metadata** popis zabezpečení clusteru a může nastavit podle vašeho nastavení. Hello **ClusterCredentialType** a **ServerCredentialType** určit typ hello zabezpečení, která implementuje hello cluster a uzly hello. Lze nastavit tooeither *X509* pro zabezpečení na základě certifikátů nebo *Windows* pro zabezpečení služby založené na Azure Active Directory. Hello zbytek hello **zabezpečení** části budou založeny na typ hello hello zabezpečení. Čtení [zabezpečení na základě certifikátů v clusteru s podporou samostatné](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows v clusteru s podporou samostatné](service-fabric-windows-cluster-windows-security.md) informace o způsobu toofill out hello zbytku hello **zabezpečení**části.

<a id="nodetypes"></a>

### <a name="node-types"></a>Typy uzlů
Hello **nodeTypes** část popisuje typ hello hello uzlů, které má váš cluster. Typ nejméně jeden uzel lze uvést pro cluster, jak je znázorněno v následující fragment hello. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Hello **název** je hello popisný název pro tento typ konkrétním uzlu. toocreate uzlu tento typ uzlu přiřadit jeho popisný název toohello **nodeTypeRef** proměnné pro tento uzel, jak je uvedeno [výše](#clusternodes). Pro každý typ uzlu definujte koncové body hello připojení, která se bude používat. Jakékoli číslo portu pro tyto koncové body připojení můžete vybrat tak dlouho, dokud nedošlo ke konfliktu s žádné koncové body v tomto clusteru. V clusteru s několika uzly budou jeden nebo více primární uzlů (tj. **isPrimary** nastavit příliš*true*), v závislosti na hello [ **reliabilityLevel** ](#reliability). Čtení [aspekty plánování kapacity služby cluster Service Fabric](service-fabric-cluster-capacity.md) informace o **nodeTypes** a **reliabilityLevel**a tooknow co jsou primární a hello typy uzel není primární. 

#### <a name="endpoints-used-tooconfigure-hello-node-types"></a>Koncové body používá typy uzlů tooconfigure hello
* *clientConnectionEndpointPort* je port hello používá hello klienta tooconnect toohello cluster, při použití rozhraní API klienta hello. 
* *clusterConnectionEndpointPort* je hello port, ve kterém uzly hello vzájemně komunikovat.
* *leaseDriverEndpointPort* je hello port používaný hello clusteru zapůjčení ovladač toofind out, pokud jsou stále aktivní hello uzly. 
* *serviceConnectionEndpointPort* je hello port je používán hello aplikacím a službám nasazeným v uzlu, toocommunicate s klientem hello Service Fabric na příslušném uzlu.
* *httpGatewayEndpointPort* je port hello používá hello Service Fabric Explorer tooconnect toohello cluster.
* *ephemeralPorts* přepsat hello [dynamické porty používané hello OS](https://support.microsoft.com/kb/929851). Service Fabric bude používat jako porty aplikace součástí a zbývající hello bude k dispozici pro hello operačního systému. Tento rozsah toohello existující rozsah součástí hello operačního systému, také mapuje proto pro všechny účely, můžete použít hello rozsahy zadané v souborech JSON ukázka hello. Je nutné toomake se, že hello rozdíl mezi počáteční hello hello porty a end je alespoň 255. Do konfliktu se může spustit, pokud tento rozdíl je příliš nízká, protože tento rozsah je sdílena s hello operačního systému. V tématu hello nakonfigurované dynamický rozsah portů spuštěním `netsh int ipv4 show dynamicport tcp`.
* *applicationPorts* hello porty, které se použijí aplikace Service Fabric hello. rozsah portů aplikace Hello by měl být dostatečně velký toocover požadavek na koncový bod hello aplikací. Tento rozsah by měl být výhradní z hello dynamický rozsah portů na počítači hello, tj. hello *ephemeralPorts* rozsah nastavený v konfiguraci hello.  Service Fabric se použít vždy, když nové porty jsou povinné, jakož i postará o otevření hello brány firewall pro tyto porty. 
* *reverseProxyEndpointPort* je koncový bod volitelné reverzní proxy server. V tématu [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) další podrobnosti. 

### <a name="log-settings"></a>Nastavení protokolu
Hello **fabricSettings** část vám tooset hello kořenové adresáře pro data hello Service Fabric a protokoly. Můžete přizpůsobit tyto pouze při vytváření počáteční clusteru hello. Ukázka fragmentu v této části jsou uvedeny níže.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Doporučujeme pomocí jednotce operačního systému jako hello proměnná FabricDataRoot a adresáři FabricLogRoot, poskytuje další spolehlivost proti dojde k chybě operačního systému. Všimněte si, že pokud přizpůsobíte pouze kořenové datové hello, pak hello protokolu kořenové bude umístěna o jednu úroveň pod kořenové datové hello.

### <a name="stateful-reliable-service-settings"></a>Nastavení stavové spolehlivé služby
Hello **KtlLogger** část vám tooset hello globální nastavení konfigurace pro spolehlivé služby. Další informace o těchto nastaveních najdete [konfigurovat stavová spolehlivé služby](service-fabric-reliable-services-configuration.md).
Následující příklad Hello ukazuje, jak toochange hello hello sdílené transakčního protokolu, který získá vytváří tooback všechny spolehlivé kolekce pro stavové služby.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Funkce rozšíření
Funkce rozšíření tooconfigure, hello apiVersion musí být nakonfigurován jako "04-2017' nebo vyšší a addonFeatures musí toobe nakonfigurovat:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Podpora kontejnerů
Podpora kontejnerů tooenable pro windows server kontejneru a technologie hyper-v kontejneru pro samostatné clustery, musí hello 'DnsService' rozšíření funkce toobe povolena.


## <a name="next-steps"></a>Další kroky
Až budete mít soubor dokončení souboru ClusterConfig.JSON nakonfigurovaný podle vaší samostatné nastavení clusteru, můžete nasadit cluster následující článek hello [vytvořit cluster Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md) a poté pokračujte příliš[vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).

