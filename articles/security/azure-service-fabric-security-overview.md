---
title: "Přehled zabezpečení infrastruktury služby aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje přehled hello zabezpečení služby Azure service fabric."
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
ms.openlocfilehash: ec5355983c5d59f4e0c3b855965f03ac47f1a4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-overview"></a>Přehled zabezpečení služby Azure Service Fabric
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) je platforma distribuovaných systémů, která umožňuje snadno toopackage, nasazovat a spravovat škálovatelného a spolehlivého micro-services. Service Fabric řeší hello významné problémy ve vývoji a správě cloudových aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné.

Tento přehled Azure Service Fabric zabezpečení článek se zaměřuje na hello následující oblasti:

-   Zabezpečení clusteru
-   Monitorování a Diagnostika
-   Zabezpečení pomocí certifikátů
-   Řízení přístupu na základě role (RBAC)
-   Zabezpečení clusteru pomocí zabezpečení systému Windows
-   Konfigurace zabezpečení aplikace v Service Fabric
-   Zabezpečení komunikace pro služby v Azure Service Fabric zabezpečení

## <a name="securing-your-cluster"></a>Zabezpečení clusteru
Azure Service Fabric je produktu orchestrator služeb v rámci clusteru s podporou počítačů, clustery musí být zabezpečené tooprevent neoprávněného uživatele z připojení clusteru tooyour, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné toocreate zabezpečená clusteru, tak umožňuje anonymní uživatelé tooconnect tooit, pokud vystavuje toohello koncové body správy veřejného Internetu.

Tato část obsahuje přehled hello scénáře zabezpečení pro clustery spuštěná v Azure nebo samostatné a hello tooimplement různých technologií, které používají tyto scénáře. scénáře zabezpečení clusteru Hello jsou:

-   Zabezpečení – uzly
-   Uzel Klient zabezpečení

### <a name="node-to-node-security"></a>Zabezpečení – uzly
Zabezpečuje komunikaci mezi hello virtuální počítače nebo počítače v clusteru hello. Tím se zajistí, že pouze počítače, které jsou autorizované toojoin hello clusteru mohou být součástí hostování aplikací a služeb v clusteru hello.

Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://msdn.microsoft.com/library/ff649801.aspx) nebo [zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx) pro počítače, Windows Server.

**Zabezpečení certificate – uzly**

Service Fabric používá certifikáty X.509 serveru, které zadáte jako součást konfigurace hello typ uzlu při vytváření clusteru. Rychlý přehled toho, co jsou tyto certifikáty a [jak můžete získat nebo jejich vytvoření je uvedené v tomto článku](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Při vytváření clusteru hello buď prostřednictvím hello portálu Azure, šablon Azure Resource Manageru nebo šablonu JSON samostatné konfigurace certifikát zabezpečení. Můžete určit primární certifikát a volitelné sekundární certifikát, který se používá pro certifikát efekty přechodu. Hello primární a sekundární certifikáty zadáte by měla být jiná než hello správce klienta a jen pro čtení klientských certifikátů, který určíte v parametru [klientský uzel zabezpečení](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Uzel Klient zabezpečení
Konfigurace klienta toonode zabezpečení pomocí identity klienta. tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru, je nutné nakonfigurovat hello clusteru tooknow které identity klienta, které můžete důvěřovat. Tento krok můžete provést dvěma způsoby:

-   Zadejte hello skupinu uživatele domény, které se můžou připojit nebo
-   Zadejte hello uzlu uživatele domény, které se můžou připojit.

Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric:

-   Správce
-   Uživatel

Řízení přístupu poskytuje schopnost hello hello toolimit Správce clusteru přístup toocertain typy operací clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru. Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.

**Certifikát klienta uzel zabezpečení**

Při vytváření clusteru hello buď prostřednictvím hello portál Azure Resource Manager šablony nebo šablony JSON samostatné zadáním certifikát klienta správce nebo klientský certifikát uživatele konfigurace uzlu klientů certifikát zabezpečení. Hello Správce klientů a uživatelů klientské certifikáty, které zadáte musí být jiný než hello primární a sekundární certifikáty, které zadáte pro zabezpečení mezi uzly.

Klienti připojení toohello clusteru pomocí Správce certifikátů hello mají plný přístup toomanagement funkce. Klienti připojení clusteru toohello pomocí hello jen pro čtení uživatele klientský certifikát mají jenom funkce, toomanagement přístup pro čtení. V jiné aplikaci word, které tyto certifikáty se používají pro hello základny role řízení přístupu (RBAC).

Pro čtení Azure [nastavení clusteru pomocí šablony Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) toolearn jak tooconfigure certifikátu zabezpečení v clusteru.

**Uzel Klient zabezpečení Azure Active Directory (AAD) v Azure**

Clustery se systémem na platformě Azure také můžete zabezpečit přístup koncových bodů pro správu toohello pomocí Azure Active Directory (AAD). V tématu [nastavení clusteru pomocí šablony Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) informace o tom, jak toocreate hello artefakty potřebné AAD, jak toopopulate je během clusteru vytváření a jak tooconnect toothose clusterů později.

AAD umožňuje organizacím (označované jako klienty) toomanage uživatel přístup tooapplications, které jsou rozdělené do aplikací s přihlašovacími údaji webové uživatelské rozhraní a aplikací prostředí nativního klienta.

Cluster Service Fabric nabízí několik vstupní body tooits funkce správy, včetně hello webové Service Fabric Explorer a Visual Studio. V důsledku toho můžete vytvořit dvě AAD aplikace toocontrol přístup toohello cluster, jednu webovou aplikaci a jeden nativní aplikace.
Pro Azure clusterů se doporučuje použít AAD zabezpečení tooauthenticate klientů a certifikáty pro zabezpečení mezi uzly.

Pro samostatné clustery systému Windows Server se doporučuje používat zabezpečení systému Windows s účty spravované skupiny (GMA), pokud máte systém Windows Server 2012 R2 a služby Active Directory. Zabezpečení systému Windows stále jinak pomocí účtů systému Windows.

## <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorovací a diagnostické pro Azure Service Fabric
[Monitorovací a diagnostické](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) jsou kritické toodeveloping, testování a nasazení aplikace a služby v jakémkoli prostředí. Řešení Service Fabric fungují lépe, když plánování a implementace monitorování a diagnostiky, které pomáhají zajistit aplikace a služby jsou funguje podle očekávání v místním vývojovém prostředí, nebo v produkčním prostředí.

Z hlediska zabezpečení hello hlavní cíle monitorování a Diagnostika mají:

-   Zjištění a diagnostice problémů hardwaru a infrastruktury, které mohou být z důvodu tooa událostí zabezpečení.
-   Rozpoznat, softwaru a aplikace problémy, které může poskytovat ukazatele ohrožení (IoC).
-   Pochopení prostředků spotřeba toohelp zabránit nechtěnému odepření služby.

Hello celkového pracovního postupu monitorování a Diagnostika se skládá ze tří kroků:

-   **Generování událostí:** to hello infrastruktury (cluster) a úrovni aplikace / service zahrnuje událostí (protokoly, trasování, vlastních událostí). Další informace o [úroveň události infrastruktury](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) a [události na úrovni aplikace](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) toounderstand, co jsou poskytovány a jak tooadd další instrumentace.
-   **Agregace událostí:** vygenerovaných událostí potřebovat toobe shromážděných a agregovat předtím, než je možné zobrazit. Obvykle doporučujeme používat [Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (více podobné kolekce na základě tooagent protokolu) nebo [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (v procesu protokolu kolekce).
-   **Analýza:** události potřebovat toobe vizualizovaných a v některých formát, tooallow pro analýzu a zobrazení podle potřeby. Existuje několik skvělé platforem, které existují v hello trhu při přechodu do toohello analýzy a vizualizace dat monitorování a Diagnostika. Hello dva, doporučujeme jsou [OMS](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) a [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) kvůli tootheir lepší integraci s Service Fabric.

Můžete také použít [Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) toomonitor řadu hello prostředky Azure, na kterých je vytvořen cluster Service Fabric.

Sledovací zařízení je samostatný služba, která může sledovat stav a zatížení v rámci služeb a sestavy stavu pro všechno, co je v hierarchii modelu stavu hello. To může pomoct zabránit chybám, které by se na základě zobrazení hello jedné služby. Watchdogs jsou také vhodné místo toohost kód, který provede nápravné akce bez zásahu uživatele (například čištění souborů protokolů v úložiště v určitých časových intervalech). Můžete najít implementaci služby sledovací zařízení ukázka [zde](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-using-certificates"></a>Zabezpečení pomocí certifikátů
Používáte certifikáty, obsahuje informace jak toosecure hello komunikace mezi hello různé uzly clusteru Windows samostatné taky, jak tooauthenticate klienti připojení clusteru toothis pomocí certifikátů X.509. To zajišťuje, jenom Autorizovaní uživatelé mají přístup ke clusteru hello hello nasazené aplikace a provádět úlohy správy. Certifikát zabezpečení může být povoleno v clusteru hello při vytvoření clusteru hello.

### <a name="x509-certificates-and-service-fabric"></a>X.509 – certifikáty a Service Fabric
Digitální certifikáty X.509 jsou běžně používané tooauthenticate klienty a servery a tooencrypt a digitálnímu podepisování zpráv.

Hello následující tabulka uvádí hello certifikáty, které budete potřebovat na vaše nastavení clusteru:

|Informace o nastavení certifikátů |Popis|
|-------------------------------|-----------|
|ClusterCertificate|    Tento certifikát se vyžaduje toosecure hello komunikaci mezi hello uzly v clusteru. Můžete použít dvě různé certifikáty, primární a sekundární pro upgrade.|
|ServerCertificate| Tento certifikát se zobrazí toohello klienta, když se ho pokusí tooconnect toothis clusteru. Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade.|
|ClientCertificateThumbprints|  To je sada certifikáty, které chcete tooinstall na klientech hello ověření.|
|ClientCertificateCommonNames|  Nastavit hello běžný název hello první klientský certifikát pro hello CertificateCommonName. Hello CertificateIssuerThumbprint je hello kryptografický otisk pro hello vystavitele tohoto certifikátu.|
|ReverseProxyCertificate|   Toto je volitelné certifikát, který může být zadán, pokud chcete, aby toosecure vaše [Reverse Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Další informace o zabezpečení certifikáty [kliknutím sem](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Řízení přístupu umožňuje hello toolimit clusteru správce přístup toocertain operace na clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru. Podporuje dva typy různý přístup řízení pro klienty připojení clusteru tooa: role správce a role uživatele.

Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.

Zadáte hello správce a uživatelských rolí klienta v době vytváření clusteru hello tím, že poskytuje samostatné identity (certifikáty, AAD atd.) pro každý. Další informace o nastavení řízení přístupu výchozí hello a jak toochange hello výchozí nastavení, najdete v části [řízení přístupu podle rolí pro Service Fabric klienty](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-using-windows-security"></a>Zabezpečení clusteru samostatné pomocí zabezpečení systému Windows
tooprevent Neautorizováno cluster Service Fabric tooa přístup, je nutné zabezpečit hello clusteru. Zabezpečení je zvlášť důležité při hello cluster spouští úlohy v produkčním prostředí. Popisuje, jak hello tooconfigure-uzly a klientský uzel zabezpečení pomocí zabezpečení systému Windows v souboru ClusterConfig.JSON souboru.

**Konfigurace zabezpečení systému Windows pomocí gMSA**

Konfigurace zabezpečení toonode uzlu nastavením [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) když service fabric potřebuje toorun pod gMSA. V pořadí toobuild vztahy důvěryhodnosti mezi uzly se musí být provedeny vědět navzájem.

Konfigurace klienta toonode zabezpečení pomocí ClientIdentities. Pořadí tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru musíte nakonfigurovat hello clusteru tooknow které identity klienta, které můžete důvěřovat.

**Konfigurace zabezpečení systému Windows ve skupině počítač**

Uzel toonode zabezpečení je nakonfigurovat pomocí nastavení pomocí ClusterIdentity, pokud chcete, aby toouse skupinu počítače v doméně služby Active Directory. Další informace najdete v tématu [vytvořit skupinu počítače ve službě Active Directory](https://msdn.microsoft.com/library/aa545347).

Zabezpečení uzel klient je nakonfigurován pomocí ClientIdentities. tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru, je nutné nakonfigurovat hello clusteru tooknow hello klienta, které můžete důvěřovat identity, které hello clusteru. Můžete vytvořit vztah důvěryhodnosti v dvěma různými způsoby:

-   Zadejte hello skupinu uživatele domény, které se můžou připojit.
-   Zadejte hello uzlu uživatele domény, které se můžou připojit.

## <a name="configure-application-security-in-service-fabric"></a>Konfigurace zabezpečení aplikace v Service Fabric
### <a name="managing-secrets-in-service-fabric-applications"></a>Správa tajných klíčů v aplikace Service Fabric
Tato metoda pomáhá při správě tajných klíčů v aplikace Service Fabric. Tajné klíče může být žádné citlivé informace, jako je například úložiště připojovací řetězce, hesla nebo jiné hodnoty, které by neměly být zpracovány v prostém textu.

Tento postup používá [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) toomanage klíče a tajné klíče. Pomocí tajných klíčů v aplikaci je však cloudu tooallow bez ohledu na platformu aplikace nasazené toobe tooa clusteru hostovat kdekoli. V tomto toku existují čtyři hlavní kroky:

-   Získejte certifikát dat šifrování.
-   Nainstalujte certifikát hello v clusteru.
-   Šifrování tajný hodnoty při nasazení aplikace s hello certifikátu a vložit je do služby souborech Settings.xml konfigurační soubor.
-   Čtení šifrované hodnoty mimo souborech Settings.xml dešifrováním s hello stejný certifikát pro šifrování.

>[!Note]
>Další informace o [Správa tajných klíčů v Service Fabric aplikace](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Konfigurace zásad zabezpečení pro aplikaci
Pomocí Azure Service Fabric zabezpečení, může pomoct zabezpečených aplikací, které jsou spuštěny v clusteru hello v rámci jiné uživatelské účty. Zabezpečení infrastruktury služby také pomáhá zabezpečené hello prostředky, které jsou používány aplikací v době hello nasazení pod hello uživatelských účtů – například, soubory, adresářů a certifikáty. Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.
Hello krokům patří:

-   Nakonfigurujte zásady hello pro vstupní bod služby Instalační program.
-   Spusťte příkazy prostředí PowerShell ze vstupního bodu instalační program.
-   Pomocí konzoly přesměrování pro místní ladění.
-   Umožňuje nakonfigurujte zásadu pro balíčky služeb kódu.
-   Přiřaďte zásadu zabezpečení přístupu pro koncové body HTTP a HTTPS.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Zabezpečené komunikace pro služby v Azure Service Fabric zabezpečení
Zabezpečení je jedním z nejdůležitějších aspektů komunikace hello. Architektura aplikace na Hello spolehlivé služby poskytuje několik předem komunikace zásobníky a nástroje, které se dají použít tooimprove zabezpečení.

-   [Pomoc se zabezpečením služby, pokud používáte vzdálenou komunikaci služby](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).
-   [Pomoc se zabezpečením služby při použití zásobníku komunikace na základě WCF](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack).

## <a name="next-steps"></a>Další kroky
- Koncepční informace o zabezpečení clusteru najdete v tématu [vytvořit cluster v Azure pomocí šablony Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) a [portál Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Další informace najdete v tématu [zabezpečení clusteru Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
