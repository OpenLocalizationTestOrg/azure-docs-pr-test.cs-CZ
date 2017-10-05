---
title: "Konfigurace clusteru samostatné Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat samostatnou nebo privátní cluster Service Fabric."
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
ms.openlocfilehash: 9885dce18dabac4a945dafd219e3ae190e34a83b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Nastavení konfigurace pro samostatný cluster Windows
Tento článek popisuje postup konfigurace použití clusteru samostatné Service Fabric ***souboru ClusterConfig.JSON*** souboru. Tento soubor můžete zadat informace, jako je Service Fabric uzlů a jejich IP adresy, různé typy uzlů v clusteru, konfigurace zabezpečení, jakož i topologie sítě z hlediska domén selhání nebo upgradovat pro váš cluster samostatné.

Pokud jste [Stáhnout balíček Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), několik ukázky souboru ClusterConfig.JSON souboru se stáhnou do počítače pracovní. Ukázky s *DevCluster* v jejich názvy vám pomůže vytvořit cluster s všechny tři uzly na stejném počítači, jako jsou logické uzly. Z toho musí být nejméně jeden uzel označen jako primárního uzlu. Tento cluster je vhodný pro vývoj nebo testovací prostředí a není podporován jako provozní cluster. Ukázky s *MultiMachine* v jejich názvy vám pomůže vytvořit cluster produkční kvality s každý uzel na samostatný počítač.

1. *ClusterConfig.Unsecure.DevCluster.JSON* a *ClusterConfig.Unsecure.MultiMachine.JSON* ukazují, jak vytvořit zabezpečená testu nebo produkční cluster v uvedeném pořadí. 
2. *ClusterConfig.Windows.DevCluster.JSON* a *ClusterConfig.Windows.MultiMachine.JSON* ukazují, jak vytvořit testovací nebo produkčního prostředí clusteru, zabezpečené pomocí [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).
3. *ClusterConfig.X509.DevCluster.JSON* a *ClusterConfig.X509.MultiMachine.JSON* ukazují, jak vytvořit testovací nebo produkčního prostředí clusteru, zabezpečené pomocí [X509 zabezpečení na základě certifikátu](service-fabric-windows-cluster-x509-security.md). 

Teď vyzkoušíme různé části ***souboru ClusterConfig.JSON*** souboru, jak je uvedeno níže.

## <a name="general-cluster-configurations"></a>Konfigurace obecných clusteru
Toto se vztahuje konkrétní konfigurace široký clusteru, jak je znázorněno v následujícím fragmentu JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

Můžete udělit libovolný popisný název pro váš cluster Service Fabric přiřazením jeho **název** proměnné. **ClusterConfigurationVersion** je číslo verze vašeho clusteru; je třeba zvýšit pokaždé, když upgradujete cluster Service Fabric. Ale nechat **apiVersion** na výchozí hodnotu.

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a>Uzly v clusteru
Můžete nakonfigurovat uzly v clusteru Service Fabric pomocí **uzly** části, jak ukazuje následující fragment kódu.

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

Cluster Service Fabric musí obsahovat aspoň 3 uzly. Do této části můžete přidat další uzly, podle vašeho nastavení. Následující tabulka popisuje nastavení konfigurace pro každý uzel.

| **Konfigurace uzlu** | **Popis** |
| --- | --- |
| nodeName |Do uzlu můžete dát libovolný popisný název. |
| IP adresa |Zjistěte, tím otevřete okno příkazového řádku a zadáte IP adresu vašeho uzlu `ipconfig`. Poznamenejte si adresu IPV4 a přiřadit ji ke **iPAddress** proměnné. |
| nodeTypeRef |Každý uzel může být přiřazen jiný uzel typu. [Typy uzlů](#nodetypes) jsou definovány v následující části. |
| faultDomain |Domén selhání umožňují správcům clusteru definování fyzických uzlů, které může selhat z důvodu sdílené fyzické závislosti současně. |
| upgradeDomain |Domén upgradu popisují sadu uzlů, které jsou vypnuté upgradů Service Fabric v přibližně ve stejnou dobu. Můžete přiřadit které Upgradovacích domén, které uzly jako nejsou omezeny všechny fyzické požadavky. |

## <a name="cluster-properties"></a>Vlastnosti clusteru
**Vlastnosti** kapitoly souboru ClusterConfig.JSON slouží ke konfiguraci clusteru následujícím způsobem.

<a id="reliability"></a>

### <a name="reliability"></a>Spolehlivost
Koncept **reliabilityLevel** definuje počet replik nebo instancí systémových služeb Service Fabric, které můžou běžet na primární uzlů v clusteru. Určuje spolehlivost tyto služby a proto clusteru. Hodnota je počítanou v systému v době vytváření a upgrade clusteru.

### <a name="diagnostics"></a>Diagnostika
**DiagnosticsStore** část vám pomůže nakonfigurovat parametry Povolit diagnostiku a řešení potíží uzel nebo selhání clusteru, jak je znázorněno v následujícím fragmentu kódu. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metadata** popis vašeho clusteru diagnostiky a může nastavit podle vašeho nastavení. Tyto proměnné pomáhají při shromažďování ETW protokolů trasování, výpisy stavu systému a také čítače výkonu. Čtení [protokolu trasování](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) a [trasování ETW](https://msdn.microsoft.com/library/ms751538.aspx) protokoly trasování pro další informace o trasování událostí pro Windows. Všechny protokoly, včetně [havárií výpisy](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) a [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) může přesměrovat k **connectionString** složky v počítači. Můžete také použít *azurestorage* pro ukládání diagnostiky. Níže najdete fragment ukázka.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Zabezpečení
**Zabezpečení** je nezbytné pro cluster Service Fabric zabezpečené samostatné části. Následující fragment kódu ukazuje součástí této části.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metadata** popis zabezpečení clusteru a může nastavit podle vašeho nastavení. **ClusterCredentialType** a **ServerCredentialType** určit typ zabezpečení, která implementuje cluster a uzly. Se může být nastaven na hodnotu *X509* pro zabezpečení na základě certifikátů nebo *Windows* pro zabezpečení služby založené na Azure Active Directory. Zbytek **zabezpečení** části budou založeny na typu zabezpečení. Čtení [zabezpečení na základě certifikátů v clusteru s podporou samostatné](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows v clusteru s podporou samostatné](service-fabric-windows-cluster-windows-security.md) informace o tom, jak vyplnit zbytek **zabezpečení** části.

<a id="nodetypes"></a>

### <a name="node-types"></a>Typy uzlů
**NodeTypes** část popisuje typ uzly, které má váš cluster. Zadejte nejméně jeden uzel lze uvést pro cluster, jak je znázorněno v následujícím fragmentu. 

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

**Název** je popisný název pro tento typ konkrétním uzlu. Chcete-li vytvořit uzel typu uzlu, přiřadit popisný název, který **nodeTypeRef** proměnné pro tento uzel, jak je uvedeno [výše](#clusternodes). Pro každý typ uzlu definujte koncové body připojení, která se bude používat. Jakékoli číslo portu pro tyto koncové body připojení můžete vybrat tak dlouho, dokud nedošlo ke konfliktu s žádné koncové body v tomto clusteru. V clusteru s několika uzly budou jeden nebo více primární uzlů (tj. **isPrimary** nastavena na *true*), v závislosti na [ **reliabilityLevel**](#reliability). Čtení [aspekty plánování kapacity služby cluster Service Fabric](service-fabric-cluster-capacity.md) informace o **nodeTypes** a **reliabilityLevel**a potřebujete vědět, co jsou primární a typy uzel není primární. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Koncové body, které slouží ke konfiguraci typy uzlů
* *clientConnectionEndpointPort* je port používaný klientem pro připojení ke clusteru, pokud používáte rozhraní API klienta. 
* *clusterConnectionEndpointPort* je port, ve kterém uzly vzájemně komunikovat.
* *leaseDriverEndpointPort* je port je používán ovladač zapůjčení clusteru a zjistěte, zda jsou stále aktivní uzly. 
* *serviceConnectionEndpointPort* je port používaný aplikace a služby, které jsou nasazeny na uzlu, ke komunikaci s klientem nástroje Service Fabric na příslušném uzlu.
* *httpGatewayEndpointPort* je port používaný pro připojení ke clusteru Service Fabric Exploreru.
* *ephemeralPorts* přepsat [dynamické porty používané operačního systému](https://support.microsoft.com/kb/929851). Service Fabric bude používat jako porty aplikace součástí a zbývající bude k dispozici pro operační systém. Také mapuje tento rozsah na existující rozsah, který je součástí operačního systému, takže pro všechny účely, můžete používat rozsahy uvedenému v ukázkové soubory JSON. Musíte se ujistit, že rozdíl mezi počáteční a koncové porty alespoň 255. Do konfliktu se může spustit, pokud tento rozdíl je příliš nízká, protože tento rozsah je sdílen s operačním systémem. V tématu rozsah dynamických portů nakonfigurované tak, že spustíte `netsh int ipv4 show dynamicport tcp`.
* *applicationPorts* porty, které se použijí aplikace Service Fabric. Rozsah portů aplikace by měl být dostatečně velký pro požadavek koncový bod, vaše aplikace. Tento rozsah by měl být výhradní z rozsahu dynamický port na počítači, tj. *ephemeralPorts* rozsah nastavený v konfiguraci.  Service Fabric se použít vždy, když nové porty jsou povinné, jakož i postará o otevření brány firewall pro tyto porty. 
* *reverseProxyEndpointPort* je koncový bod volitelné reverzní proxy server. V tématu [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) další podrobnosti. 

### <a name="log-settings"></a>Nastavení protokolu
**FabricSettings** část vám pomůže nastavit kořenové adresáře pro data Service Fabric a protokoly. Můžete přizpůsobit tyto pouze při vytváření počáteční clusteru. Ukázka fragmentu v této části jsou uvedeny níže.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Je doporučeno použití jednotku operačního systému jako proměnná FabricDataRoot a adresáři FabricLogRoot, jako nabízí že další spolehlivost proti operačního systému, dojde k chybě. Všimněte si, že pokud přizpůsobíte pouze kořenové datové, pak kořenu protokolu budou umístěny jednu úroveň pod kořenovým adresářem data.

### <a name="stateful-reliable-service-settings"></a>Nastavení stavové spolehlivé služby
**KtlLogger** část vám pomůže globální konfiguraci nastavení pro spolehlivé služby. Další informace o těchto nastaveních najdete [konfigurovat stavová spolehlivé služby](service-fabric-reliable-services-configuration.md).
Následující příklad ukazuje, jak změnit sdílené transakčního protokolu, která je vytvořena zálohovat všechny spolehlivé kolekce pro stavové služby.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Funkce rozšíření
Ke konfiguraci rozšíření funkcí, apiVersion musí být nakonfigurován jako "04-2017' nebo vyšší a addonFeatures je potřeba nakonfigurovat:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Podpora kontejnerů
Povolení podpory kontejner pro kontejner windows serveru a technologie hyper-v kontejneru pro samostatné clustery, musí být povolena funkce rozšíření 'DnsService'.


## <a name="next-steps"></a>Další kroky
Až budete mít soubor dokončení souboru ClusterConfig.JSON nakonfigurovaný podle vaší samostatné nastavení clusteru, můžete nasadit cluster pomocí následujícího článku [vytvořit cluster Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md) a pak pokračujte [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).

