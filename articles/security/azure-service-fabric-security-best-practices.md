---
title: "osvědčené postupy zabezpečení aaaAzure Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: 483a21240da17d56bb4641653093ddcbad379d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric osvědčené postupy zabezpečení
Nasazení aplikace v Azure je rychlý, snadný a nákladově efektivní. Před nasazením cloudových aplikací v provozním užitečné toohave představuje dobrou praxi tooassist při hodnocení aplikace podle seznamu důležité a doporučené osvědčené postupy.

Azure Service Fabric je platforma distribuovaných systémů, která umožňuje snadno toopackage, nasadit a spravovat škálovatelného a spolehlivého mikroslužeb. Service Fabric také řeší hello významné problémy ve vývoji a správě cloudových aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné. 

Pro každý osvědčený postup objasníme:

-   Jaké hello osvědčeným postupem je
-   Proč chcete tooenable, že osvědčeným postupem
-   Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello
-   Jak můžete získat informace tooenable hello osvědčený postup

V současné době máme hello následující osvědčené postupy zabezpečení Azure Service Fabric:

-   Pomocí šablony Azure arm prostředků a zabezpečení clusteru Service Fabric Azure PowerShell Module toocreate
-   Použijte certifikáty X.509
-   Konfigurovat zásady zabezpečení
-   Konfigurace zabezpečení spolehlivé Actors
-   Konfigurace protokolu SSL pro Azure Service Fabric
-   Izolace nebo zabezpečení sítě pomocí služby Azure Service Fabric
-   Nastavení trezoru klíčů pro zabezpečení
-   Přiřazení uživatelů tooroles


## <a name="best-practices-for-securing-your-cluster"></a>Osvědčené postupy pro zabezpečení vašeho clusteru

**Velký obrázek**

Vždy používat cluster s podporou zabezpečení
-   Cluster zabezpečení – použití certifikátů
-   Klientský přístup (správce a jen pro čtení) – použijte AAD

Použití automatického nasazení
-   Používat skripty toogenerate, nasazení a mění tajné klíče
-   Mějte KV hello tajných klíčů, použijte AD pro všechny ostatní klientský přístup
-   Bez lidské by měl mít toothem přístup bez ověřování.

Kromě toho zvažte následující hello:
-   Vytvoření zóny DMZ pomocí skupin zabezpečení sítě (Nsg)
-   Použít tooRDP servery přechod do clusteru virtuálních počítačů nebo toomanage clusteru

Clustery musí být zabezpečené tooprevent neoprávněného uživatele z připojení clusteru tooyour, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné toocreate zabezpečená clusteru, tak umožňuje anonymní uživatelé tooconnect tooit, pokud vystavuje toohello koncové body správy veřejného Internetu.

Tyto scénáře tooimplement použít technologie. Hello [clusteru scénáře zabezpečení](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) jsou:

-   Uzly zabezpečení This zabezpečuje komunikaci mezi hello virtuálních počítačů a počítačů v clusteru hello. Tím se zajistí, že pouze počítače, které jsou autorizované toojoin hello clusteru mohou být součástí hostování aplikací a služeb v clusteru hello.
Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) nebo [zabezpečení systému Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) pro počítače, Windows Server.
-   Uzel Klient zabezpečení This zabezpečuje komunikaci mezi klientem Service Fabric a jednotlivé uzly v clusteru hello.
-   Řízení přístupu na základě role (RBAC) – pro každou zadáte hello správce a uživatelských rolí klienta v době vytváření clusteru hello tím, že poskytuje samostatné identity (certifikáty, AAD atd.).
-   Doporučení zabezpečení-clusterů pro Azure, se doporučuje použít AAD zabezpečení tooauthenticate klientů a certifikáty pro zabezpečení mezi uzly.

tooconfigure hello samostatný cluster systému Windows, najdete v části [konfiguraci nastavení pro cluster windows samostatné](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Pomocí šablony Azure Resource Manager a zabezpečení clusteru Service Fabric Azure PowerShell Module toocreate.
Je k dispozici podrobný průvodce nevystavíte slabé stránky zabezpečení můžete pomocí nastavení zabezpečení Azure Service Fabric clusteru v Azure pomocí Azure Resource Manager [zde](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Použít toocustomize šablony Azure Resource Manager hello clusteru
-   Instalační program spravované úložiště pro virtuální pevné disky virtuálního počítače

Použít hello Azure Resource Manager šablony toodrive změny tooyour skupiny prostředků
-   Snadno configuration management.
-   Auditování

Konfiguraci clusteru považovat za kódu
-   Při kontrole konfigurace hello zvolíte toodeploy důkladné
-   Vyhněte se použití implicitní příkazy tootweak vaše prostředky přímo

Mnoho aspektů hello [životního cyklu aplikace Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) lze automatizovat. [Modul prostředí PowerShell Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatizuje běžné úlohy pro nasazení, upgrade, odebrání a testování aplikací pro Azure Service Fabric. Spravovat a rozhraní API HTTP pro správu aplikací jsou také k dispozici.

## <a name="use-x509-certificates"></a>Použijte certifikáty X.509
Clustery by měly být zabezpečeny vždy pomocí certifikátů X.509 nebo zabezpečení systému Windows. Zabezpečení je nakonfigurovaná pouze při vytváření clusteru a není možné tooenable zabezpečení po vytvoření clusteru hello.

Pokud zadáte [clusteru certifikát](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), nastavte hodnotu hello ClusterCredentialType tooX509. Pokud zadáte certifikát serveru pro připojení mimo, nastavte hello ServerCredentialType tooX509.

-   Certifikáty používané v clusterech spuštění úlohy v produkčním prostředí by měla být vytvořen pomocí správně nakonfigurovaných certifikát služby Windows Server nebo získat ze schválené certifikační autoritou (CA).
-   Nikdy nepoužívejte žádné dočasných nebo testovací certifikáty v produkčním prostředí, které jsou vytvořeny pomocí nástrojů, jako je MakeCert.exe.
-   Můžete použít certifikát podepsaný svým držitelem, ale měli tak učinit pouze pro testovací clustery a ne v produkčním prostředí.

Pokud hello cluster nezabezpečená. Každý se může anonymně připojit a provádět operace správy, proto by produkční clustery vždy měly být zabezpečené pomocí certifikátů X.509 nebo zabezpečení systému Windows.

toolearn více jak tooenable certifikáty v service fabric cluster najdete [přidat nebo odebrat certifikáty pro service fabric cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Konfigurovat zásady zabezpečení
Service Fabric také pomáhá zabezpečené hello prostředky, které jsou používány aplikací v době hello nasazení pod hello uživatelských účtů – například, soubory, adresářů a certifikáty. Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.

-   Použít skupiny domény služby Active Directory nebo uživatele: můžete spustit služba hello hello pověření pro účet uživatele nebo skupiny služby Active Directory. Toto je služby Active Directory místně ve vaší doméně a není v Azure Active Directory (Azure AD). Pomocí účtem uživatele domény nebo skupiny můžete pak přístup k jiným prostředkům v doméně hello (například sdílené složky), kterým bylo uděleno oprávnění.

-   Přiřadit zásady zabezpečení přístupu pro koncové body HTTP a HTTPS: Pokud použijete službu tooa zásad RunAs a hello service manifest deklaruje koncový bod prostředků s protokolem hello HTTP, je nutné zadat SecurityAccessPolicy tooensure, že porty přidělené toothese Koncové body jsou správně přístup řízenou uvedené pro hello RunAs uživatelský účet, který spouští služba hello. Jinak http.sys nemá přístup k toohello službu a získat selhání pomocí volání od klienta hello.
toolearn více povolit zásady zabezpečení v service fabric najdete [konfigurovat zásady zabezpečení pro vaši aplikaci](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="reliable-actors-security-configuration"></a>Konfigurace zabezpečení spolehlivé Actors
Služba Fabric Reliable Actors je implementace tohoto vzoru návrhu objektu actor hello. Stejně jako u jakékoli vzoru návrhu softwaru hello rozhodnutí, jestli se provádí specifického vzoru toouse podle zda softwaru návrh problém vyhovuje vzoru hello.

Jako obecné pokyny vezměte v úvahu hello objektu actor vzor toomodel problém nebo scénář pokud:
-   Váš prostor problém zahrnuje velký počet (tisíc nebo více) malé, nezávislé a izolované jednotek stavu a logiku.
-   Chcete toowork s jedním podprocesem objekty, které nevyžadují významné interakce z externí součásti, včetně dotaz na stav mezi sadu aktéři.
-   Vaše instance objektu actor neblokuje volající s nepředvídatelným zpoždění vydáním vstupně-výstupních operací.

V Service Fabric, kteří se implementují ve hello Reliable Actors framework: na základě objektu actor vzor aplikační rozhraní založené na [spolehlivé služby Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Každý spolehlivé objektu Actor službu, kterou píšete je ve skutečnosti oddílů, stavová spolehlivá služba.
Každý objektu actor je definován jako instance typu objektu actor, způsob identické toohello objekt .NET je instance typu .NET. Například může být typ objektu actor, který implementuje hello funkce kalkulačky a může být mnoho aktéři daného typu, které jsou rozmístěny v různých uzlech v clusteru. Každé takové objektu actor je jedinečně identifikovaný identifikátor objektu actor.

[Konfigurace zabezpečení Replikátor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) jsou použité toosecure hello komunikační kanál, který se používá během replikace. To znamená, že služby nelze vzájemně provoz replikace, zajišťující, že hello data, která vysokou dostupnost, je také zabezpečené. Ve výchozím nastavení zabraňuje konfigurační oddíl prázdný zabezpečení zabezpečení replikace.
Konfigurace Replikátor nakonfigurovat hello Replikátor, která je odpovědná za vytváření vysoce spolehlivé hello zprostředkovatele stavu objektu Actor stavu.

## <a name="configure-ssl-for-azure-service-fabric"></a>Konfigurace protokolu SSL pro Azure Service Fabric

Ověření serveru: [ověřuje](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) hello clusteru správy koncové body tooa správy klienta, tak, aby hello klienta pro správu zná ho je rozhovoru toohello skutečné clusteru. Tento certifikát také poskytuje [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) hello rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.
Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.

tooconfigure SSL pro aplikaci, musíte nejdřív tooget certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel. Pokud není již nemáte, je třeba tooobtain jeden ze společnosti, která prodává certifikáty SSL.

certifikát Hello musí splňovat následující požadavky na certifikáty SSL v Azure hello:
-   Hello certifikát musí obsahovat privátní klíč.
-   Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).
-   Hello názvu subjektu certifikátu musí odpovídat hello domény použít tooaccess hello cloudové služby. Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net hello. Musíte získat toouse název vlastní domény při přístupu ke službě. Pokud budete požadovat certifikát od certifikační Autority, musí se název subjektu certifikátu hello odpovídat tooaccess název používaný hello vlastní domény aplikace. Například pokud je váš vlastní název domény contoso.com můžete by žádost o certifikát z certifikační Autority pro **. contoso.com** nebo **www.contoso.com.**
-   Hello certifikát musí používat minimálně 2048bitové šifrování.

HTTP je nezabezpečené a je subjektu tooeavesdropping útokům, protože hello dat přenášených z hello webové prohlížeče toohello webový server nebo mezi ostatní koncové body, se přenášejí v podobě prostého textu. To znamená, že útočník může zachytávat a zobrazit citlivá data, jako je například údajů z platební karty a účet přihlášení. Když nebo odeslání dat odeslány prostřednictvím prohlížeče pomocí protokolu HTTPS, SSL zajišťuje, že tyto údaje šifrované a před narušením zabezpečení.

toolearn více, zobrazit, [konfigurace protokolu SSL pro aplikaci azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="network-isolationsecurity-with-azure-service-fabric"></a>Izolace nebo zabezpečení sítě pomocí služby Azure Service Fabric
Použití [šablony Azure Resource Manager (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) jako ukázka pro nastavení tři nodetype zabezpečené clusteru a toocontrol hello příchozí a odchozí síťový provoz pomocí skupin zabezpečení sítě.

Šablona Hello má skupina zabezpečení sítě pro každou hello škálování set(VMSS) toocontrol hello přenosy dat virtuálních počítačů a deaktivovat hello VMSS. Ve výchozím nastavení jsou hello systému služby a hello aplikace porty zadané v šabloně hello hello pravidla nastavit tooallow všechny hello přenos nezbytný. Zkontrolujte tato pravidla a provést změny toofit vašim potřebám, včetně přidání jakékoli nové pro vaše aplikace.

Další informace najdete v tématu [Azure Service Fabric – běžné scénáře sítě](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-a-key-vault-for-security"></a>Nastavení trezoru klíčů pro zabezpečení
Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace.

Service Fabric používá toosecure certifikáty X.509 cluster a poskytují funkce zabezpečení aplikací. Pomocí Key Vault příliš[spravovat certifikáty](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) pro clusterů Service Fabric v Azure. Pokud cluster je nasazené v Azure, vyžaduje certifikáty od Key Vault hello poskytovatel prostředků Azure, která je odpovědná za vytváření clusterů Service Fabric a nainstaluje je hello clusteru virtuálních počítačů.

vztah mezi Hello [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), service fabric cluster a hello poskytovatel prostředků Azure, která používá certifikáty uložené v trezoru klíčů při vytváření clusteru.

**Vytvořte skupinu prostředků** hello prvním krokem je toocreate skupinu prostředků speciálně pro váš trezor klíčů. Doporučujeme převést trezoru klíčů hello do vlastní skupiny prostředků. Tato akce umožňuje odstranit hello výpočetního prostředí a úložiště skupiny prostředků, včetně hello skupinu prostředků, která obsahuje daný cluster Service Fabric bez ztráty klíče a tajné klíče. Skupina prostředků Hello, která obsahuje váš trezor klíčů musí být v hello stejné oblasti jako hello clusteru, který se používá.

**Vytvoření trezoru klíčů v novou skupinu prostředků hello** hello trezoru klíčů musí být povolen pro nasazení tooallow hello výpočetních prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte ji na instancí virtuálního počítače.
toolearn více jak zjistit, tooset si Azure trezoru klíčů, [Začínáme s Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-roles"></a>Přiřadit role uživatele
Po vytvoření aplikace toorepresent hello cluster, přiřadit uživatelům role toohello nepodporuje Service Fabric: jen pro čtení a správce. Můžete přiřadit role hello pomocí hello portál Azure classic.

>[!Note]
> Další informace o rolích v Service Fabric najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa [cluster Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): správce a uživatele. Řízení přístupu umožňuje hello toolimit clusteru správce přístup toocertain operace na clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru.

## <a name="next-steps"></a>Další kroky
- Nastavení Service Fabric [vývojového prostředí](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Další informace o [možnosti podpory Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

