---
title: "Azure Service Fabric osvědčené postupy zabezpečení | Microsoft Docs"
description: "Tento článek obsahuje sadu osvědčené postupy pro zabezpečení Azure Service Fabric."
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
ms.openlocfilehash: 3132e42aac72c0132c7526ac56d80bc5eec269e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric osvědčené postupy zabezpečení
Nasazení aplikace v Azure je rychlý, snadný a nákladově efektivní. Před nasazením cloudových aplikací v provozním prostředí, které jsou užitečné používat osvědčený postup jako pomoc při hodnocení aplikace podle seznamu důležité a doporučené osvědčené postupy.

Azure Service Fabric je platforma distribuovaných systémů usnadňující balení, nasazování a spravování škálovatelných a spolehlivých mikroslužeb. Service Fabric se taky zaměřuje na problematiku vývoje a správy cloudových aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné. 

Pro každý osvědčený postup objasníme:

-   Co je osvědčeným postupem
-   Proč chcete povolit tento osvědčený postup
-   Pokud se nepodaří povolit osvědčený postup, co může být výsledkem
-   Jak můžete informace o povolení osvědčený postup

V současné době máme následující osvědčené postupy zabezpečení Azure Service Fabric:

-   Vytvoření zabezpečeného clusteru pomocí šablony arm prostředků Azure a modul prostředí PowerShell Azure Service Fabric
-   Použijte certifikáty X.509
-   Konfigurovat zásady zabezpečení
-   Konfigurace zabezpečení spolehlivé Actors
-   Konfigurace protokolu SSL pro Azure Service Fabric
-   Izolace nebo zabezpečení sítě pomocí služby Azure Service Fabric
-   Nastavení trezoru klíčů pro zabezpečení
-   Přiřadit uživatele k rolím


## <a name="best-practices-for-securing-your-cluster"></a>Osvědčené postupy pro zabezpečení vašeho clusteru

**Velký obrázek**

Vždy používat cluster s podporou zabezpečení
-   Cluster zabezpečení – použití certifikátů
-   Klientský přístup (správce a jen pro čtení) – použijte AAD

Použití automatického nasazení
-   Použít skripty k vytvoření, nasazení a mění tajné klíče
-   Mějte KV těchto tajných klíčů, použijte AD pro všechny ostatní klientský přístup
-   Bez lidské měli přístup k nim bez ověřování.

Kromě toho zvažte následující:
-   Vytvoření zóny DMZ pomocí skupin zabezpečení sítě (Nsg)
-   Použít Jump servery pro připojení RDP do clusteru virtuálních počítačů nebo ke správě vašeho clusteru

Clustery musí být zabezpečená neoprávněným uživatelům zabránit v připojení ke clusteru, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné vytvořit zabezpečená clusteru, tak učiníte, umožníte anonymní uživatelé pro připojení, pokud vystavuje koncové body správy do veřejného Internetu.

Technologie použít k implementaci těchto scénářů. [Clusteru scénáře zabezpečení](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) jsou:

-   Uzel zabezpečení This zabezpečuje komunikaci mezi virtuálních počítačů a počítačů v clusteru. Tím se zajistí, že pouze počítače, které jsou autorizované pro připojení ke clusteru mohou být součástí hostování aplikací a služeb v clusteru.
Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) nebo [zabezpečení systému Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) pro počítače, Windows Server.
-   Uzel Klient zabezpečení This zabezpečuje komunikaci mezi klientem Service Fabric a jednotlivé uzly v clusteru.
-   Řízení přístupu na základě role (RBAC) – pro každou zadáte správce a uživatelských rolí klienta v době vytváření clusteru poskytnutím oddělené identity (certifikáty, AAD atd.).
-   Doporučení zabezpečení-clusterů pro Azure, se doporučuje použít k ověřování klientů a certifikáty pro zabezpečení – uzly zabezpečení AAD.

Pro nakonfigurování clusteru Windows samostatné, najdete v části [konfiguraci nastavení pro cluster windows samostatné](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

K vytvoření zabezpečeného clusteru pomocí šablony Azure Resource Manager a Service Fabric Azure PowerShell Module.
Je k dispozici podrobný průvodce nevystavíte slabé stránky zabezpečení můžete pomocí nastavení zabezpečení Azure Service Fabric clusteru v Azure pomocí Azure Resource Manager [zde](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Přizpůsobení clusteru pomocí šablony Azure Resource Manager
-   Instalační program spravované úložiště pro virtuální pevné disky virtuálního počítače

Pomocí šablony Azure Resource Manager na jednotce změny ve vaší skupině prostředků
-   Snadno configuration management.
-   Auditování

Konfiguraci clusteru považovat za kódu
-   Při kontrole konfigurace, které se rozhodnete nasadit důkladné
-   Nepoužívejte implicitní příkazy přímo upravit vašich prostředků

Mnoho aspektů [životního cyklu aplikace Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) lze automatizovat. [Modul prostředí PowerShell Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatizuje běžné úlohy pro nasazení, upgrade, odebrání a testování aplikací pro Azure Service Fabric. Spravovat a rozhraní API HTTP pro správu aplikací jsou také k dispozici.

## <a name="use-x509-certificates"></a>Použijte certifikáty X.509
Clustery by měly být zabezpečeny vždy pomocí certifikátů X.509 nebo zabezpečení systému Windows. Zabezpečení se konfiguruje jenom při vytváření clusteru a není možné povolit zabezpečení po vytvoření clusteru.

Pokud zadáte [clusteru certifikát](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), nastavte hodnotu ClusterCredentialType na X509. Pokud zadáte certifikát serveru pro připojení mimo, nastavte ServerCredentialType na X509.

-   Certifikáty používané v clusterech spuštění úlohy v produkčním prostředí by měla být vytvořen pomocí správně nakonfigurovaných certifikát služby Windows Server nebo získat ze schválené certifikační autoritou (CA).
-   Nikdy nepoužívejte žádné dočasných nebo testovací certifikáty v produkčním prostředí, které jsou vytvořeny pomocí nástrojů, jako je MakeCert.exe.
-   Můžete použít certifikát podepsaný svým držitelem, ale měli tak učinit pouze pro testovací clustery a ne v produkčním prostředí.

Pokud cluster zrušit zabezpečení. Každý se může anonymně připojit a provádět operace správy, proto by produkční clustery vždy měly být zabezpečené pomocí certifikátů X.509 nebo zabezpečení systému Windows.

Další informace o tom, jak povolit certifikátů najdete v tématu clusteru service fabric, [přidat nebo odebrat certifikáty pro service fabric cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Konfigurovat zásady zabezpečení
Service Fabric také pomáhá zabezpečit prostředky, které jsou používány aplikací v době nasazení podle uživatelských účtů – například soubory, adresářů a certifikáty. Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.

-   Použít skupiny domény služby Active Directory nebo uživatele: můžete spustit službu v části přihlašovací údaje pro účet uživatele nebo skupiny služby Active Directory. Toto je služby Active Directory místně ve vaší doméně a není v Azure Active Directory (Azure AD). Pomocí účtem uživatele domény nebo skupiny můžete pak přístup k jiným prostředkům v doméně (například sdílené složky), kterým bylo uděleno oprávnění.

-   Přiřadit zásady zabezpečení přístupu pro koncové body HTTP a HTTPS: Pokud použít zásady RunAs služby a služby manifest deklaruje koncový bod prostředků pomocí protokolu HTTP, je nutné zadat SecurityAccessPolicy zajistit, že porty přidělené tyto Koncové body jsou správně přístup řízenou uvedené pro uživatelský účet Spustit jako, který se spouští služba. Jinak ovladač http.sys nemá přístup ke službě a získáte selhání pomocí volání z klienta.
Další informace povolit zásady zabezpečení v service fabric najdete [konfigurovat zásady zabezpečení pro vaši aplikaci](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="reliable-actors-security-configuration"></a>Konfigurace zabezpečení spolehlivé Actors
Služba Fabric Reliable Actors je implementace tohoto vzoru návrhu objektu actor. Rozhodnutí, jestli se má používat specifického vzoru se provádí podle zda softwaru návrh problém stejně jako u jakékoli vzoru návrhu softwaru vyhovuje vzoru.

Jako obecné pokyny vezměte v úvahu vzor objektu actor pro modelování problém nebo scénář, pokud:
-   Váš prostor problém zahrnuje velký počet (tisíc nebo více) malé, nezávislé a izolované jednotek stavu a logiku.
-   Budete chtít pracovat s jedním podprocesem objekty, které nevyžadují významné interakce z externí součásti, včetně dotaz na stav mezi sadu aktéři.
-   Vaše instance objektu actor neblokuje volající s nepředvídatelným zpoždění vydáním vstupně-výstupních operací.

V Service Fabric aktéři jsou implementované v rámci Reliable Actors: na základě objektu actor vzor aplikační rozhraní založené na [spolehlivé služby Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Každý spolehlivé objektu Actor službu, kterou píšete je ve skutečnosti oddílů, stavová spolehlivá služba.
Každý objekt actor je definován jako instanci objektu actor typu identické způsobem, jakým objekt .NET je instance typu .NET. Například může být typ objektu actor, která implementuje funkce kalkulačky a může být mnoho aktéři daného typu, které jsou rozmístěny v různých uzlech v clusteru. Každé takové objektu actor je jedinečně identifikovaný identifikátor objektu actor.

[Konfigurace zabezpečení Replikátor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) se používají k zabezpečení komunikačního kanálu, který se používá během replikace. To znamená, že služby nelze vzájemně provoz replikace, zajišťující, že data, která vysokou dostupnost, je také zabezpečené. Ve výchozím nastavení zabraňuje konfigurační oddíl prázdný zabezpečení zabezpečení replikace.
Konfigurace Replikátor nakonfigurovat Replikátor, která zodpovídá za vytvoření zprostředkovatele stavu objektu Actor stavu vysoce spolehlivé.

## <a name="configure-ssl-for-azure-service-fabric"></a>Konfigurace protokolu SSL pro Azure Service Fabric

Ověření serveru: [ověřuje](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) clusteru koncových bodů správy klient pro správu, tak, aby klient správy zná je rozhovoru s skutečné clusteru. Tento certifikát také poskytuje [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) pro rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.
Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

Konfigurace protokolu SSL pro aplikaci, musíte nejdřív získat certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel. Pokud není již nemáte, budete muset získat jeden ze společnosti, která prodává certifikáty SSL.

Certifikát musí splňovat následující požadavky na certifikáty SSL v Azure:
-   Certifikát musí obsahovat privátní klíč.
-   Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).
-   Název subjektu certifikátu musí odpovídat domény používá pro přístup ke cloudové službě. Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net. Musíte získat vlastní název domény má použít při přístupu ke službě. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, používá pro přístup k aplikaci. Například pokud je váš vlastní název domény contoso.com můžete by žádost o certifikát z certifikační Autority pro **. contoso.com** nebo **www.contoso.com.**
-   Certifikát musí používat minimálně 2048bitové šifrování.

HTTP je nezabezpečené a vystavené útokům odposlechu protože dat přenášených z webového prohlížeče na webový server nebo mezi ostatní koncové body, se přenášejí v podobě prostého textu. To znamená, že útočník může zachytávat a zobrazit citlivá data, jako je například údajů z platební karty a účet přihlášení. Když nebo odeslání dat odeslány prostřednictvím prohlížeče pomocí protokolu HTTPS, SSL zajišťuje, že tyto údaje šifrované a před narušením zabezpečení.

Další informace najdete v tématu, [konfigurace protokolu SSL pro aplikaci azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="network-isolationsecurity-with-azure-service-fabric"></a>Izolace nebo zabezpečení sítě pomocí služby Azure Service Fabric
Použití [šablony Azure Resource Manager (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) jako ukázka pro nastavení zabezpečení clusteru tři nodetype a řídit příchozí a odchozí síťový provoz, použití skupin zabezpečení sítě.

Šablona má skupina zabezpečení sítě pro každou set(VMSS) škálování virtuálního počítače k řízení provozu do a z VMSS. Ve výchozím nastavení jsou pravidla nastavit tak, aby veškerý provoz vyžaduje systémové služby a porty aplikace zadané v šabloně. Zkontrolujte tato pravidla a provést změny podle vašich potřeb, včetně přidání jakékoli nové pro vaše aplikace.

Další informace najdete v tématu [Azure Service Fabric – běžné scénáře sítě](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-a-key-vault-for-security"></a>Nastavení trezoru klíčů pro zabezpečení
Ve službě Service Fabric se k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací využívají certifikáty.

Service Fabric používá certifikáty X.509 k zabezpečení clusteru a poskytují funkce zabezpečení aplikací. Pomocí Key Vault [spravovat certifikáty](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) pro clusterů Service Fabric v Azure. Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure, která je odpovědná za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je v clusteru virtuálních počítačů.

Vztah mezi [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), service fabric cluster a poskytovatel prostředků Azure, která používá certifikáty uložené v trezoru klíčů při vytváření clusteru.

**Vytvořte skupinu prostředků** prvním krokem je vytvoření skupiny prostředků speciálně pro váš trezor klíčů. Doporučujeme převést trezoru klíčů do vlastní skupiny prostředků. Tato akce umožňuje odstranit výpočetního prostředí a úložiště skupiny prostředků, včetně skupinu prostředků, která obsahuje daný cluster Service Fabric bez ztráty klíče a tajné klíče. Skupinu prostředků, která obsahuje váš trezor klíčů musí být ve stejné oblasti jako cluster, který se používá.

**Vytvoření trezoru klíčů do nové skupiny prostředků** trezoru klíčů musí být povolen pro nasazení, který poskytovatele výpočetních prostředků z něj získat certifikáty a nainstalujte ji na instancí virtuálního počítače.
Další informace o tom, jak nastavit trezor klíčů služby Azure naleznete v tématu [Začínáme s Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-roles"></a>Přiřadit role uživatele
Po vytvoření aplikace, které chcete představují clusteru přiřadit uživatelům role nepodporuje Service Fabric: jen pro čtení a správce. Pomocí portálu Azure classic můžete přiřadit role.

>[!Note]
> Další informace o rolích v Service Fabric najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené k [cluster Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): správce a uživatele. Řízení přístupu umožňuje omezit přístup k určité operace clusteru pro různé skupiny uživatelů, lepší zabezpečení clusteru pomocí Správce clusteru.

## <a name="next-steps"></a>Další kroky
- Nastavení Service Fabric [vývojového prostředí](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Další informace o [možnosti podpory Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

