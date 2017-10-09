---
title: "osvědčené postupy aaaAzure provozní zabezpečení | Microsoft Docs"
description: "Tento článek obsahuje sadu osvědčené postupy pro zabezpečení provozu Azure."
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
ms.openlocfilehash: b3b17ef20fb3545b1c268ac0d7ce692e07c8da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security-best-practices"></a>Osvědčené postupy provozní zabezpečení Azure
Zabezpečení provozu Azure odkazuje toohello služeb, ovládací prvky a funkce dostupné toousers pro ochranu svá data, aplikace a dalších prostředků ve službě Microsoft Azure. Zabezpečení provozu Azure je založený na rozhraní, které zahrnuje hello poznatky získané při různých možnostech, které jsou jedinečné tooMicrosoft, včetně hello Microsoft SDL Security Development Lifecycle (), hello Microsoft Security Response Center program a hloubkové povědomí o šířku threat počítačové bezpečnosti hello.

V tomto článku probereme kolekce osvědčené postupy zabezpečení databáze Azure. Tyto doporučené postupy jsou odvozeny od našich zkušeností s zabezpečení databáze Azure a hello prostředí zákazníků, jako sami.

Pro každý osvědčený postup objasníme:
-   Jaké hello osvědčeným postupem je
-   Proč chcete tooenable, že osvědčeným postupem
-   Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello
- Jak můžete získat informace tooenable hello osvědčený postup

Tento článek osvědčené postupy provozní zabezpečení Azure je založen na konsenzu stanovisko a možnostech platformy Azure a sady funkcí, které jsou ve hello dobu, kdy byla zapsána v tomto článku. Názory a technologie mění v čase a budou v tomto článku v pravidelných intervalech tooreflect aktualizovat tyto změny.

Azure provozního zabezpečení osvědčené postupy popsané v tomto článku patří:

-   Monitorovat, spravovat a chránit infrastruktury cloudu
-   Správa identity a implementovat jednotné přihlašování (SSO)
-   Trasování požadavků, analyzovat trendy využití a diagnostikovat problémy
-   Monitorování služeb s řešením centralizované monitorování
-   Zabránit, zjistit a reagovat toothreats
-   Monitorování sítě na základě scénáře začátku do konce
-   Zabezpečit pomocí Principy DevOps nástrojů nasazení

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Monitorovat, spravovat a chránit infrastruktury cloudu
IT oddělení je zodpovědný za správu infrastruktuře datacentra, aplikace a data, včetně hello stability a zabezpečení těchto systémech. Získat přehled o zabezpečení napříč zvýšení komplexní prostředí IT často však vyžaduje organizace toocobble společně data z několika systémů, zabezpečení a správu.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury.

[Řešení OMS zabezpečení a Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) umožňuje IT tooactively monitorovat všechny prostředky, které může pomoci minimalizovat hello dopad incidentů, zabezpečení. OMS zabezpečení a Audit mít zabezpečení domény, které lze použít ke sledování prostředků.

Další informace o OMS, přečtěte si článek hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

toohelp zabránit, zjistit a reagovat toothreats, [Operations Management Suite (OMS) zabezpečení a Audit řešení](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) shromažďuje a zpracovává data o vašich prostředků, která zahrnuje:

-   Protokol událostí zabezpečení
-   Události Trasování událostí pro Windows
-   Události auditování AppLocker
-   Protokol brány Windows Firewall
-   Události Rozšířené analýzy hrozeb
-   Výsledky základního vyhodnocování
-   Výsledky antimalwarového vyhodnocování
-   Výsledky vyhodnocování aktualizací a oprav
-   Datové proudy Syslog, které jsou explicitně povoleno na hello agenta


## <a name="manage-identity-and-implement-single-sign-on"></a>Správa identity a implementovat jednotné přihlašování
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) víceklientské cloudový adresář společnosti Microsoft a služba identity management.

[Azure AD](https://azure.microsoft.com/services/active-directory/) také zahrnuje úplná sada [správy identit](https://docs.microsoft.com/azure/security/security-identity-management-overview) funkcí, včetně [služby Multi-Factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), registrace zařízení, správu hesla pomocí samoobslužné služby, Samoobslužná správa skupin, správy privilegovaného účtu, řízení přístupu na základě rolí, využití aplikací, sledování, bohaté auditování a sledování a výstrah zabezpečení.

Hello následující možnosti můžete pomůže zabezpečené cloudové aplikace, zjednodušit procesy IT, snížit náklady a ujistěte se, zda jsou splněny dodržování podnikových předpisů cíle:

-   Správu identit a přístupu pro hello cloud
-   Zjednodušení uživatel přístup tooany cloudové aplikace
-   Ochrana citlivých dat a aplikací
-   Zajistěte pro svoje zaměstnance samoobslužné možnosti.
-   Integrace se službou Azure Active Directory

### <a name="identity-and-access-management-for-hello-cloud"></a>Správu identit a přístupu pro hello cloud
Azure Active Directory (Azure AD) představuje komplexní [cloudové řešení správy identit a přístupu](https://www.microsoft.com/cloud-platform/identity-management), který poskytuje výkonnou sadu funkcí toomanage uživatelů a skupin. Pomáhá zabezpečit přístup k místním tooon a cloudovým aplikacím, včetně webových služeb společnosti Microsoft jako je Office 365 a množství softwaru jiných společností než Microsoft jako služba (SaaS) aplikace.
toolearn více jak tooenable ochrany identit ve službě Azure AD, najdete v části [povolení Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-tooany-cloud-app"></a>Zjednodušení uživatel přístup tooany cloudové aplikace
[Povolit jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) toothousands přístup uživatele toosimplify cloudových aplikací ze zařízení s Windows, Mac, Android a iOS. Uživatele můžete spustit panel přizpůsobený webový přístup nebo mobilní aplikace pomocí firemních přihlašovacích aplikací. Použijte toogo modulu Proxy aplikace hello Azure AD nad rámec SaaS aplikace a publikovat místní webové aplikace tooprovide v vysoce zabezpečený vzdálený přístup a jednotné přihlašování.

### <a name="protect-sensitive-data-and-applications"></a>Ochrana citlivých dat a aplikací
Povolit [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) tooprevent Neautorizováno přístup tooon místním a cloudovým aplikacím tím, že poskytuje další úroveň ověřování. Chraňte svoji firmu a reagujte na potenciální hrozby sledováním výstrah zabezpečení a sestav založených na strojovém učení, které rozpoznávají nekonzistentní vzorce přístupu.

### <a name="enable-self-service-for-your-employees"></a>Zajistěte pro svoje zaměstnance samoobslužné možnosti.
Delegate zaměstnanci tooyour důležité úkoly, jako je například resetování hesla a vytváření a Správa skupin. [Povolit změnu hesla pomocí samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), resetování a samoobslužné služby skupiny správy s Azure AD.

### <a name="integrate-with-azure-active-directory"></a>Integrace se službou Azure Active Directory
Rozšíření [služby Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) a všemi ostatními místních adresářů tooAzure AD tooenable jednotné přihlašování pro všechny cloudové aplikace. Atributy uživatele může být automaticky synchronizované tooyour cloudového adresáře z nejrůznějších druhy místních adresářů.

Další informace o integraci služby Azure Active Directory toolearn a jak tooenable, přečtěte si článek hello [integraci místních adresářů se službou Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Trasování požadavků, analyzovat trendy využití a diagnostikovat problémy
[Azure Storage Analytics](https://docs.microsoft.com/azure/storage/storage-analytics) provádí protokolování a poskytuje data metriky pro účet úložiště. Můžete použít tento tootrace požadavky na data, analyzovat trendy využití a diagnostikovat problémy s vaším účtem úložiště.

Metriky Analytics úložiště jsou povolené ve výchozím nastavení pro nové účty úložiště. Můžete povolit protokolování a nakonfigurovat metrik a protokolování v hello portál Azure; Podrobnosti najdete v tématu [monitorovat účet úložiště na portálu Azure hello](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Můžete také povolit Storage Analytics prostřednictvím kódu programu prostřednictvím hello REST API nebo knihovny klienta hello. Použijte hello nastavit vlastnosti služby operaci tooenable Storage Analytics samostatně pro každou službu.

Pro podrobné příručce pomocí analytika úložiště a dalších nástrojů tooidentify diagnostikovat a vyřešit problémy související s Azure Storage najdete v tématu [monitorování, Diagnostika a řešení Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Další informace o integraci služby Azure Active Directory toolearn a jak tooenable, přečtěte si článek hello [povolení a konfigurace úložiště analýzy](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Monitorování služeb
Cloudové aplikace jsou komplexní s mnoha přesunutí částmi. Monitorování poskytuje tooensure data, která vaše aplikace zůstává nahoru a spuštěn v dobrém stavu. Pomáhá také můžete toostave vypnout potenciální problémy a řešení potíží s uplynulou těch, které jsou. Kromě toho můžete použít monitorování hlubšímu porozumění toogain data o vaší aplikaci. Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.

### <a name="monitor-azure-resources"></a>Sledování prostředků Azure
[Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) je služba hello platformy, která poskytuje jednoho zdroje pro monitorování prostředků Azure. S Azure monitorování můžete vizualizovat, dotaz, směrování, archivaci a provádět příslušné akce hello metriky a protokoly pocházejících z prostředků ve službě Azure. Můžete pracovat s těmito daty pomocí portálu okně monitorování hello [rutiny prostředí PowerShell monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [rozhraní příkazového řádku a platformy](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples), nebo [rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/dn931943.aspx).

### <a name="enable-autoscale-with-azure-monitor"></a>Povolit automatické škálování s Azure monitorování
Povolit [Azure monitorování škálování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) platí pouze toovirtual počítače sady škálování (VMSS), cloudové služby, aplikace služby plány a prostředí app service.

### <a name="manage-roles-permissions-and-security"></a>Správa rolí zabezpečení a oprávnění
Mnoha týmy potřebovat toostrictly [regulovat přístup toomonitoring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) data a nastavení. Například pokud máte členové týmu, kteří pracují výhradně na monitorování (pracovníci technické podpory, technici devops) nebo pokud používáte poskytovatel spravované služby, může být vhodné toogrant je přístup k tooonly monitorování dat při jejich toocreate možnost omezení, upravit, nebo Odstraňte prostředky.

Ukazuje to, jak tooquickly použít integrované monitorování RBAC role tooa uživatele v Azure nebo vytvořit vlastní vlastní role pro uživatele, který potřebuje monitorování omezenými oprávněními. Potom popisuje aspekty zabezpečení pro vaše prostředky související s monitorování Azure a jak můžete omezit přístup k datům toohello obsahují.

## <a name="prevent-detect-and-respond-toothreats"></a>Zabránit, zjistit a reagovat toothreats
Detekce hrozeb Security Center funguje tak, že automaticky shromažďování informací o zabezpečení prostředků Azure, sítě hello a připojených partnerských řešení. Ho analýz tyto informace, často korelace informace z více zdrojů, tooidentify hrozeb. Výstrahy zabezpečení budou mít vyšší prioritu v Centru zabezpečení včetně doporučení, na tom, jak tooremediate hello hrozeb.

-   [Konfigurace zásad zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-policies) pro vaše předplatné Azure.
-   Použití hello [doporučení v Centru zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-recommendations) toohelp chránit prostředky v Azure.
-   Zkontrolujte a spravovat vaše aktuální [výstrahy zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) pomáhá zabránit, zjišťovat a odpovědět toothreats s lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Security Center nabízí snadno použitelné a efektivní threat prevence, zjišťování a reakce na funkce, které jsou součástí tooAzure. Klíčové funkce:

-   Pochopení stav zabezpečení cloudu
-   Převzetí kontroly nad zabezpečením cloudu
-   Snadné nasazení integrovaných řešení cloudového zabezpečení
-   Rozpoznání hrozeb a rychlá reakce

### <a name="understand-cloud-security-state"></a>Pochopení stav zabezpečení cloudu
Použití Azure Security Center tooget centrální zobrazení stavu zabezpečení hello všech vašich prostředků Azure. Na první pohled ověřte, zda hello potřebné ovládací prvky zabezpečení jsou na místě a správně nakonfigurovaný a rychle identifikovat všechny prostředky, které vyžadují pozornost.

### <a name="take-control-of-cloud-security"></a>Převzetí kontroly nad zabezpečením cloudu
Definování [zásady zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-policies) pro vaše předplatná Azure podle tooyour společnosti je cloudové zabezpečení vyžaduje, přizpůsobit toohello typu aplikací nebo citlivosti dat hello v každém předplatném. Použít řízená zásadami doporučení tooguide prostředků vlastníky procesem hello implementace požadovaných ovládacích prvků – trvat hello složité zabezpečení cloudu.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Snadné nasazení integrovaných řešení cloudového zabezpečení
[Povolit řešení zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) od společnosti Microsoft a jeho partnery, včetně špičkový brány firewall a antimalwaru. Použít zjednodušený zřizování řešení zabezpečení toodeploy – i síťové změny jsou nakonfigurovány pro vás. Události zabezpečení z partnerských řešení se automaticky shromažďují, aby je bylo možné analyzovat a generovat z nich upozornění.

### <a name="detect-threats-and-respond-fast"></a>Rozpoznání hrozeb a rychlá reakce
Udržování náskoku před aktuálními a nově vznikajícími cloudovými hrozbami vyžaduje integrovaný analytický přístup. Kombinací Microsoft globální [hrozby intelligence](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) a znalosti, s přehledy cloudu události související se zabezpečením v nasazeních Azure, Security Center pomáhá již v rané fázi detekovat skutečné hrozby a snížil počet falešných poplachů. Výstrahy zabezpečení cloudu získáte přehled o kampaň hello útoku, včetně související události a zasažené prostředky a zjistíte, jak tooremediate problémy a rychle obnovit.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Monitorování sítě na základě scénáře začátku do konce
Zákazníci vytvářet síť začátku do konce v Azure tak, že orchestruje a skládání různé jednotlivých síťovým prostředkům, například virtuální síť, ExpressRoute, aplikační bránu, nástroje pro vyrovnávání zatížení a další. Monitorování je k dispozici na každém hello síťových prostředků.

[Sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) je místní služba, která vám umožní toomonitor a diagnostikovat podmínky na úrovni sítě scénář v, do a z Azure. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Automatizace monitorování vzdálené sítě pomocí zachytávání paketů
Monitorování a diagnostikovat problémy se sítí bez přihlášení tooyour virtuálních počítačů (VM) pomocí sledovací proces sítě. Aktivační událost [zachytáváním paketů](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) pomocí nastavení výstrah a získat informace o výkonu tooreal čas přístupu na úrovni paketů hello. Když narazíte na problém, můžete ho prozkoumat podrobněji a lépe diagnostikovat.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Získejte přehled o provozu sítě pomocí protokolů toků
Sestavení lépe pochopili, váš síťový provoz vzor pomocí [skupinu zabezpečení sítě toku protokoly](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Informace poskytované toku protokoly umožňuje shromažďování dat pro dodržování předpisů, auditování a sledování profil zabezpečení sítě.

### <a name="diagnose-vpn-connectivity-issues"></a>Diagnostika potíží s připojením VPN
Sledovací proces sítě poskytuje příliš hello možnost[diagnostikovat nejběžnějších problémů brány sítě VPN a připojení](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Díky tomu můžete pouze tooidentify hello problém, ale také toouse hello podrobné protokoly vytvořené toohelp dále prozkoumat.

Další informace o tom toolearn sledovací proces sítě tooconfigure a jak tooenable, přečtěte si článek hello [konfigurace sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Zabezpečit pomocí Principy DevOps nástrojů nasazení
Toto jsou některé z hello seznam z Azure DevOps postupy v tomto cloudu Microsoftu prostor, který umožňuje podnikům a týmům produktivní a efektivní.

-   **Infrastruktura jako kód (IaC):** infrastruktury jako kód je sada technologií a postupů, které odborníkům v oblasti IT pomohou odeberte hello zatížení přidružené hello den tooday sestavení a správu modulární infrastruktury. To umožňuje odborníkům v oblasti toobuild a zachovat jejich prostředí moderní server tak, že je jako jak vývojářům tvorbu a udržování kódu aplikace. Pro Azure, máme [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) vám umožní tooprovision vaší aplikace s použitím deklarativní šablony. S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi. Použít hello stejné šablony toorepeatedly nasazení aplikace během každé fáze životního cyklu aplikace hello.
-   **Průběžnou integraci a nasazení:** Visual Studio Online týmových projektech můžete nakonfigurovat příliš[automaticky sestavení a nasazení](https://www.visualstudio.com/docs/build/overview) tooAzure webové aplikace nebo cloudové služby. Binární soubory hello VSO automaticky nasadí po provádění sestavení tooAzure po každé změnami kódu. Hello popsaného procesu sestavení balíčku je ekvivalentní toohello příkaz balíčku v sadě Visual Studio a hello publikování kroky jsou ekvivalentní toohello příkazu Publikovat v sadě Visual Studio.
-   **Správa verzí:** Visual Studio [řízení vydání](https://msdn.microsoft.com/library/vs/alm/release/overview) je vynikající řešení pro automatizaci více fáze nasazení a správa hello verze procesu. Vytvoření spravované průběžné nasazování kanály toorelease rychle, snadno a často. Pomocí řízení vydání jsme mnohem můžete automatizovat proces naše verze a jsme může mít předdefinované schválení pracovních postupů. Nasadit místně a toohello cloudu, rozšířit a přizpůsobit podle potřeby.
-   **Monitorování výkonu aplikace:** rozpoznat problémy, řešení problémů a neustálé zlepšování aplikací. Rychle diagnostikuje problémy uvedené ve vaší živé aplikaci. Pochopte, co s ní vaši uživatelé dělají. Konfigurace je snadno řádu přidání kódu JS a položku webconfig a zobrazte výsledky v rámci minut hello portálu s všechny podrobnosti hello. [App insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) pomáhá podnikům pro rychlejší detekce problémy & nápravy problému.
-   **Spouštění testování & škálování:** problémy s výkonem jsme můžete najít v naší aplikaci tooimprove nasazení kvality a toomake se naše aplikace je vždy nahoru nebo dostupné toocater toohello obchodním potřebám. Ujistěte se, že vaše aplikace dokáže zpracovat provoz další spuštění nebo marketingové kampaně. Spustit cloudové [spouštění testů](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) v současně s Visual Studio Online.

## <a name="next-steps"></a>Další kroky
- Další informace o [provozního zabezpečení Azure](https://docs.microsoft.com/azure/security/azure-operational-security).
- Další tooLearn [Operations Management Suite | Zabezpečení a dodržování předpisů](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Začínáme s Operations Management Suite zabezpečení a Audit řešení](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).
