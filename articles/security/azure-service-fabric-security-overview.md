---
title: "Přehled zabezpečení služby Azure service fabric | Microsoft Docs"
description: "Tento článek obsahuje přehled zabezpečení služby Azure service fabric."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 4cbd2791649c6d2dd005521cedb44c17aa874073
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-service-fabric-security-overview"></a>Přehled zabezpečení služby Azure Service Fabric
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) je platforma distribuovaných systémů, která usnadňuje balíčku, nasazovat a spravovat škálovatelného a spolehlivého micro-services. Service Fabric řeší významné problémy ve vývoji a správě cloudových aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné.

V tomto článku Přehled zabezpečení infrastruktury služby Azure se zaměřuje na tyto oblasti:

-   Zabezpečení clusteru
-   Monitorování a Diagnostika
-   Zabezpečení pomocí certifikátů
-   Řízení přístupu na základě role (RBAC)
-   Zabezpečení clusteru pomocí zabezpečení systému Windows
-   Konfigurace zabezpečení aplikace v Service Fabric
-   Zabezpečení komunikace pro služby v Azure Service Fabric zabezpečení

## <a name="securing-your-cluster"></a>Zabezpečení clusteru
Azure Service Fabric je produktu orchestrator služeb v rámci clusteru s podporou počítačů, clustery musí být zabezpečená neoprávněným uživatelům zabránit v připojení ke clusteru, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné vytvořit zabezpečená clusteru, tak učiníte, umožníte anonymní uživatelé pro připojení, pokud vystavuje koncové body správy do veřejného Internetu.

Tato část obsahuje přehled scénáře zabezpečení pro clustery se systémem na Azure nebo samostatné a různých technologií použít k implementaci těchto scénářů. Scénáře zabezpečení clusteru jsou:

-   Zabezpečení – uzly
-   Uzel Klient zabezpečení

### <a name="node-to-node-security"></a>Zabezpečení – uzly
Zabezpečuje komunikaci mezi virtuální počítače nebo počítače v clusteru. Tím se zajistí, že pouze počítače, které jsou autorizované pro připojení ke clusteru mohou být součástí hostování aplikací a služeb v clusteru.

Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://msdn.microsoft.com/library/ff649801.aspx) nebo [zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx) pro počítače, Windows Server.

**Zabezpečení certificate – uzly**

Service Fabric používá certifikáty X.509 serveru, které zadáte jako součást konfigurace typ uzlu při vytváření clusteru. Rychlý přehled toho, co jsou tyto certifikáty a [jak můžete získat nebo jejich vytvoření je uvedené v tomto článku](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Při vytváření clusteru buď prostřednictvím portálu Azure, šablon Azure Resource Manageru nebo šablonu JSON samostatné konfigurace certifikát zabezpečení. Můžete určit primární certifikát a volitelné sekundární certifikát, který se používá pro certifikát efekty přechodu. Primární a sekundární certifikáty zadáte by měla být jiná než správce klienta a jen pro čtení klientské certifikáty, které zadáte pro [klientský uzel zabezpečení](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Uzel Klient zabezpečení
Klient uzel zabezpečení je nakonfigurován pomocí identity klienta. K vybudování důvěry mezi klientem a cluster, je nutné nakonfigurovat clusteru potřebujete vědět, který klient identity, které můžete důvěřovat. Tento krok můžete provést dvěma způsoby:

-   Zadejte skupinu uživatele domény, které se můžou připojit nebo
-   Zadejte uživatele uzlu domény, které se můžou připojit.

Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, kteří jsou připojené ke clusteru Service Fabric:

-   Správce
-   Uživatel

Řízení přístupu poskytuje možnost pro správce clusteru k omezení přístupu k určitým typům operace clusteru pro různé skupiny uživatelů, lepší zabezpečení clusteru. Správci mají plný přístup k funkcím správy (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze pro čtení přístup k možnosti správy (například možnosti dotazu) a možnost řešení aplikace a služby.

**Certifikát klienta uzel zabezpečení**

Při vytváření clusteru, buď prostřednictvím portálu Azure Resource Manager šablony nebo šablony JSON samostatné zadáním certifikát klienta správce nebo klientský certifikát uživatele konfigurace uzlu klientů certifikát zabezpečení. Správce klienta a uživatelské certifikáty klienta, který zadáte, musí být jiné než primární a sekundární certifikáty, které zadáte pro zabezpečení mezi uzly.

Klienti připojení ke clusteru pomocí Správce certifikátů mají plný přístup k možnosti správy. Klienti připojení ke clusteru pomocí klientský certifikát uživatele jen pro čtení mít přístup jenom pro čtení k možnosti správy. V jiné aplikaci word, které tyto certifikáty se používají pro roli základny řízení přístupu (RBAC).

Pro čtení Azure [nastavení clusteru pomocí šablony Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) se dozvíte, jak nakonfigurovat certifikát zabezpečení v clusteru.

**Uzel Klient zabezpečení Azure Active Directory (AAD) v Azure**

Clustery se systémem na platformě Azure můžete také zabezpečený přístup ke koncovým bodům správy pomocí Azure Active Directory (AAD). V tématu [nastavení clusteru pomocí šablony Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) informace o vytvoření artefakty potřebné AAD, jak naplnit při vytváření clusteru a jak se připojit k tyto clustery později.

Spravovat přístup uživatelů k aplikace, které jsou rozděleny do aplikace s přihlašovacími údaji webové uživatelské rozhraní a aplikací prostředí nativního klienta umožňuje AAD organizace (označované jako klienty).

Cluster Service Fabric nabízí několik vstupní body k jeho funkce správy, včetně webových Service Fabric Explorer a Visual Studio. V důsledku toho můžete vytvořit dvě AAD aplikace pro řízení přístupu do clusteru, jednu webovou aplikaci a jeden nativní aplikace.
Pro Azure clusterů se doporučuje použít k ověřování klientů a certifikáty pro zabezpečení – uzly zabezpečení AAD.

Pro samostatné clustery systému Windows Server se doporučuje používat zabezpečení systému Windows s účty spravované skupiny (GMA), pokud máte systém Windows Server 2012 R2 a služby Active Directory. Zabezpečení systému Windows stále jinak pomocí účtů systému Windows.

## <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorovací a diagnostické pro Azure Service Fabric
[Monitorovací a diagnostické](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) jsou důležité pro vývoj, testování a nasazení aplikace a služby v jakémkoli prostředí. Řešení Service Fabric fungují lépe, když plánování a implementace monitorování a diagnostiky, které pomáhají zajistit aplikace a služby jsou funguje podle očekávání v místním vývojovém prostředí, nebo v produkčním prostředí.

Z hlediska zabezpečení jsou hlavní cíle monitorování a diagnostiky pro:

-   Najít a diagnostikovat problémy hardwaru a infrastruktury, které mohou být způsobeny událostí zabezpečení.
-   Rozpoznat, softwaru a aplikace problémy, které může poskytovat ukazatele ohrožení (IoC).
-   Pochopení spotřeby prostředků, aby se zabránilo nechtěnému odepření služby.

Celkové pracovní postup monitorování a Diagnostika zahrnuje tři kroky:

-   **Generování událostí:** to zahrnuje událostí (protokoly, trasování, vlastních událostí) v infrastruktuře (cluster) a aplikace / service úroveň. Další informace o [úroveň události infrastruktury](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) a [události na úrovni aplikace](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) pochopit, co je k dispozici a jak přidat další instrumentace.
-   **Agregace událostí:** vygenerovaných událostí je třeba shromažďovat a agregovat předtím, než je možné zobrazit. Obvykle doporučujeme používat [Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (podobně jako více do kolekce založené na agentovi protokolu) nebo [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (v procesu protokolu kolekce).
-   **Analýza:** události musí být vizualizovaných a v některých formátu, aby bylo možné pro analýzu a zobrazit podle potřeby. Existuje několik skvělé platforem, které existují na trhu, pokud jde o analýzy a vizualizace dat monitorování a diagnostiky. Jsou dva, které doporučujeme [OMS](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) a [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) z důvodu jejich lepší integraci s Service Fabric.

Můžete také použít [Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) monitorovat mnoho prostředků Azure, na kterých je vytvořen cluster Service Fabric.

Sledovací zařízení je samostatný služba, která může sledovat stav a zatížení v rámci služeb a sestavy stavu pro všechno, co je v hierarchii stavu modelu. To může pomoct zabránit chybám, které by se na základě zobrazení jedné služby. Watchdogs jsou také vhodné místo pro hostitele kód, který provede nápravné akce bez zásahu uživatele (například čištění souborů protokolů v úložiště v určitých časových intervalech). Můžete najít implementaci služby sledovací zařízení ukázka [zde](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-using-certificates"></a>Zabezpečení pomocí certifikátů
Pomocí certifikátů, o způsobu k zabezpečení komunikace mezi různými uzly clusteru Windows samostatné, a také o tom, k ověřování klientů, připojení k tomuto clusteru pomocí X.509 certifikáty. To zajišťuje, že můžete jenom autorizovaným uživatelům přístup ke clusteru, nasazené aplikace a provádět úlohy správy. Certifikát zabezpečení by měly být povoleny v clusteru při vytvoření clusteru.

### <a name="x509-certificates-and-service-fabric"></a>X.509 – certifikáty a Service Fabric
Digitální certifikáty X.509 běžně se používají k ověřování klientů a serverů a k šifrování a digitálnímu podepisování zpráv.

Následující tabulka uvádí certifikáty, které budete potřebovat na vaše nastavení clusteru:

|Informace o nastavení certifikátů |Popis|
|-------------------------------|-----------|
|ClusterCertificate|    Tento certifikát je vyžadován k zabezpečení komunikace mezi uzly v clusteru. Můžete použít dvě různé certifikáty, primární a sekundární pro upgrade.|
|ServerCertificate| Tento certifikát je pro klienta zobrazí při pokusu o připojení k tomuto clusteru. Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade.|
|ClientCertificateThumbprints|  To je sada certifikáty, které chcete instalovat na ověřené klienty.|
|ClientCertificateCommonNames|  Nastavte běžný název první klientský certifikát pro CertificateCommonName. CertificateIssuerThumbprint je kryptografický otisk tohoto certifikátu vystavitele.|
|ReverseProxyCertificate|   Toto je volitelné certifikát, který může být zadán, pokud chcete zabezpečit vaše [Reverse Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Další informace o zabezpečení certifikáty [kliknutím sem](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Řízení přístupu umožňuje omezit přístup k určité operace clusteru pro různé skupiny uživatelů, lepší zabezpečení clusteru pomocí Správce clusteru. Podporuje dva typy různý přístup řízení pro klienty připojující se ke clusteru: role správce a role uživatele.

Správci mají plný přístup k funkcím správy (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze pro čtení přístup k možnosti správy (například možnosti dotazu) a možnost řešení aplikace a služby.

Zadáte správce a uživatelských rolí klienta v době vytváření clusteru poskytnutím oddělené identity (certifikáty, AAD atd.) pro každý. Další informace o výchozí nastavení řízení přístupu a jak změnit výchozí nastavení, najdete v části [řízení přístupu podle rolí pro Service Fabric klienty](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-using-windows-security"></a>Zabezpečení clusteru samostatné pomocí zabezpečení systému Windows
K zabránění neoprávněného přístupu k cluster Service Fabric, je nutné zabezpečit clusteru. Zabezpečení je zvlášť důležité při spuštění úlohy v produkčním prostředí clusteru. Popisuje konfiguraci zabezpečení – uzly a uzlu klientů pomocí zabezpečení systému Windows v souboru ClusterConfig.JSON souboru.

**Konfigurace zabezpečení systému Windows pomocí gMSA**

Uzel zabezpečení uzlu je nakonfigurovaný nastavením [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) když service fabric potřebuje pro spuštění pod gMSA. Chcete-li vytvořit vztahy důvěryhodnosti mezi uzly, se musí být provedeny vědět navzájem.

Klient uzel zabezpečení je nakonfigurován pomocí ClientIdentities. Chcete-li vytvořit vztah důvěryhodnosti mezi klientem a cluster, je nutné nakonfigurovat clusteru potřebujete vědět, který klient identity, které můžete důvěřovat.

**Konfigurace zabezpečení systému Windows ve skupině počítač**

Uzel zabezpečení uzlu je nakonfigurovaný v nastavení pomocí ClusterIdentity, pokud chcete použít skupinu počítače v doméně služby Active Directory. Další informace najdete v tématu [vytvořit skupinu počítače ve službě Active Directory](https://msdn.microsoft.com/library/aa545347).

Zabezpečení uzel klient je nakonfigurován pomocí ClientIdentities. K vybudování důvěry mezi klientem a cluster, je nutné nakonfigurovat clusteru potřebujete vědět, klient identity, které můžete důvěřovat clusteru. Můžete vytvořit vztah důvěryhodnosti v dvěma různými způsoby:

-   Zadejte skupinu uživatele domény, které se můžou připojit.
-   Zadejte uživatele uzlu domény, které se můžou připojit.

## <a name="configure-application-security-in-service-fabric"></a>Konfigurace zabezpečení aplikace v Service Fabric
### <a name="managing-secrets-in-service-fabric-applications"></a>Správa tajných klíčů v aplikace Service Fabric
Tato metoda pomáhá při správě tajných klíčů v aplikace Service Fabric. Tajné klíče může být žádné citlivé informace, jako je například úložiště připojovací řetězce, hesla nebo jiné hodnoty, které by neměly být zpracovány v prostém textu.

Tento postup používá [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) ke správě klíčů a tajných klíčů. Použití tajných klíčů v aplikaci je cloudové platformy vznikl k aplikacím umožňují nasadit do clusteru s podporou hostovat kdekoli. V tomto toku existují čtyři hlavní kroky:

-   Získejte certifikát dat šifrování.
-   Nainstalujte certifikát v clusteru.
-   Šifrování tajný hodnoty při nasazení aplikace pomocí certifikátu a vložit je do služby souborech Settings.xml konfigurační soubor.
-   Číst šifrovaných hodnot mimo souborech Settings.xml dešifrování stejným certifikátem šifrování.

>[!Note]
>Další informace o [Správa tajných klíčů v Service Fabric aplikace](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Konfigurace zásad zabezpečení pro aplikaci
Pomocí Azure Service Fabric zabezpečení, může pomoct zabezpečených aplikací, které jsou spuštěny v clusteru v rámci jiné uživatelské účty. Zabezpečení infrastruktury služby také pomáhá zabezpečit prostředky, které jsou používány aplikací v době nasazení podle uživatelských účtů – například soubory, adresářů a certifikáty. Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.
Zahrnuje následující kroky:

-   Nakonfigurujte zásady pro vstupní bod služby Instalační program.
-   Spusťte příkazy prostředí PowerShell ze vstupního bodu instalační program.
-   Pomocí konzoly přesměrování pro místní ladění.
-   Umožňuje nakonfigurujte zásadu pro balíčky služeb kódu.
-   Přiřaďte zásadu zabezpečení přístupu pro koncové body HTTP a HTTPS.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Zabezpečené komunikace pro služby v Azure Service Fabric zabezpečení
Zabezpečení je jedním z nejdůležitějších aspektů komunikace. Rozhraní spolehlivé služby poskytuje několik předem komunikace zásobníky a nástroje, které slouží k vylepšení zabezpečení.

-   [Pomoc se zabezpečením služby, pokud používáte vzdálenou komunikaci služby](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).
-   [Pomoc se zabezpečením služby při použití zásobníku komunikace na základě WCF](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack).

## <a name="next-steps"></a>Další kroky
- Koncepční informace o zabezpečení clusteru najdete v tématu [vytvořit cluster v Azure pomocí šablony Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) a [portál Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Další informace najdete v tématu [zabezpečení clusteru Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
