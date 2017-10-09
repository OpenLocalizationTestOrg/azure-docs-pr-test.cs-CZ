---
title: "Přehled zabezpečení provozní aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje přehled hello zabezpečení provozu Azure."
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
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: b91c7889660b32e4933c305007692bd6e1ded05f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security-overview"></a>Přehled Azure provozního zabezpečení
Zabezpečení provozu Azure odkazuje toohello služeb, ovládací prvky a funkce dostupné toousers pro ochranu svá data, aplikace a dalších prostředků ve službě Microsoft Azure. [Zabezpečení provozu Azure](https://docs.microsoft.com/azure/security/azure-operational-security) je rozhraní, které zahrnuje znalostní báze hello získaných prostřednictvím nejrůznějších možností, které jsou jedinečné tooMicrosoft, včetně hello Microsoft SDL Security Development Lifecycle (), hello Microsoft Security Program Response Center a hloubkové povědomí o hello kybernetického zabezpečení threat na šířku.

V tomto článku Přehled zabezpečení provozu Azure se zaměřuje na hello následující oblasti:

- Azure Operations Management Suite
-   Azure Security Center
-   Azure Monitor
-   Azure sledovací proces sítě
-   Azure Storage analytics
-   Azure Active directory

## <a name="azure-operations-management-suite"></a>Azure Operations Management Suite
IT oddělení je zodpovědný za správu infrastruktuře datacentra, aplikace a data, včetně hello stability a zabezpečení těchto systémech. Získat přehled o zabezpečení napříč zvýšení komplexní prostředí IT často však vyžaduje organizace toocobble společně data z několika systémů, zabezpečení a správu.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury.

OMS je cloudové řešení správy IT s mnoha nabídky, jako je například IT automatizace, zabezpečení a dodržování předpisů, analýzy protokolů a zálohování a obnovení. Jako takový je ideální podpory toomanage a chránit vaši IT infrastrukturu – místně a v cloudu hello.

Hello základní funkce služby OMS poskytuje sadu služby, které běží v Azure. Každá služba poskytuje funkce správy specifických a můžete kombinovat scénářů tooachieve různých správy služeb. Který zahrnuje:

-   Log Analytics
-   Automation
-   Backup
-   Site Recovery

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) poskytuje služby monitorování pro OMS získáváním dat ze spravovaných prostředků do centrálního úložiště. Tato data můžou zahrnovat události, údaje o výkonu nebo vlastní data poskytnutá prostřednictvím hello rozhraní API. Jakmile získány, je k dispozici pro výstrahy, analýzu a export dat hello. Tato metoda umožňuje tooconsolidate data z různých zdrojů, můžete kombinovat data ze služeb Azure s vaší stávající místní prostředí. Také jasně odděluje hello shromažďování dat hello od hello akce prováděné na tato data tak, aby všechny akce jsou k dispozici tooall druhy dat.

### <a name="automation"></a>Automation
Microsoft [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) poskytuje způsob, jak pro uživatele tooautomate hello ruční, dlouhotrvajících, problematických a často se opakujících úloh, které se běžně provádějí v cloudovém a podnikovém prostředí. Šetří čas a zvyšuje spolehlivost hello běžných administrativních úloh a umí je dokonce naplánovat toobe automaticky v pravidelných intervalech prováděly. Procesy můžete automatizovat pomocí runbooků nebo můžete automatizovat správu konfigurace pomocí DSC (požadovaný stav konfigurace).

### <a name="backup"></a>Zálohování
[Zálohování Azure](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) je hello služby Azure můžete použít tooback (nebo chránit) a obnovování vašich dat v cloudu Microsoft hello. Azure Backup nahrazuje současná řešení místního nebo odlehlého zálohování spolehlivým, bezpečným a cenově konkurenceschopným cloudovým řešením. Zálohování Azure nabízí několik komponent, které můžete stáhnout a nasadit na hello příslušný počítač, server, nebo v cloudu hello. součást Hello nebo agenta, který nasazujete, závisí na co chcete tooprotect. Všechny součásti zálohování Azure (bez ohledu na to, jestli chcete chránit data místně nebo v cloudu hello) lze použít tooback do trezoru služeb zotavení tooa dat v Azure. V tématu hello [tabulky Azure Backup součásti](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Obnovení lokality
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) poskytuje provozní kontinuitu tím, že orchestruje replikaci místní virtuální a fyzické počítače tooAzure nebo tooa sekundární lokality. Pokud primární lokalita není k dispozici, můžete převzít toohello sekundárního umístění, aby uživatelé mohli zachovat pracovní a nesplní zpět, když systémy vrátit tooworking pořadí. detekce hrozeb inteligentního a efektivní.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) je společnosti Microsoft komplexní Identity jako služby (IDaaS) řešení který:

-   Jako cloudová služba umožňuje IAM
-   Poskytuje správu centrální přístupu, jednotného přihlašování (SSO) a vytváření sestav
-   Podporuje správu integrovaného přístupu k [tisíce aplikace](https://azure.microsoft.com/marketplace/active-directory/) v galerii aplikací hello, včetně služby Salesforce, Google Apps, pole, Concur a další.

Azure AD také zahrnuje úplná sada [funkce správy identit](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports) včetně [služby Multi-Factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), [registrace zařízení]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview), [ hesla pomocí samoobslužné služby správy](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/), [Samoobslužná správa skupin](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), [privilegovaný účet správy](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), [řízení přístupu na základě role](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is), [sledování využití aplikací](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), [bohaté auditování](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), a [sledování a výstrah zabezpečení](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).

S Azure Active Directory, všechny aplikace publikujete pro partnery a zákazníky (obchodní nebo příjemce) mají hello stejné možnosti správy identit a přístupu. Díky této může toosignificantly můžete snížit provozní náklady.

## <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started) pomáhá zabránit, zjišťovat a odpovědět toothreats s lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) pomáhá zabezpečit data virtuálního počítače v Azure poskytuje přehled o nastavení zabezpečení virtuálního počítače a monitorování hrozby. Security Center může u virtuálních počítačů monitorovat:

-   Nastavení zabezpečení operačního systému (OS) s hello doporučená pravidla konfigurace
-   Zabezpečení systému a chybějící kritické aktualizace
-   Doporučení ochrany koncových bodů
-   Ověření šifrování disku
-   Útoky ze sítě

Azure Security Center používá [řízení přístupu na základě Role (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure), který poskytuje [předdefinované role](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) , lze přiřadit toousers, skupiny a služby v Azure.

Security Center vyhodnocuje hello konfiguraci problémy se zabezpečením tooidentify prostředky a ohrožení zabezpečení. V Centru zabezpečení, uvidíte jenom informace související s tooa prostředků, když jsou přiřazeny hello roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo prostředek skupiny hello, která daný prostředek patří.

>[!Note]
>V tématu [oprávnění v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-permissions) toolearn více informací o rolí a povolených akcí v Centru zabezpečení.

Security Center používá hello agenta Microsoft Monitoring Agent – to je hello hello Operations Management Suite a analýzy protokolů služba používá stejný agenta. Data shromážděná z tohoto agenta je uložen v buď existující analýzy protokolů [prostoru](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) přidružený k předplatnému Azure nebo nové pracovních prostorů, s ohledem na zeměpisnou polohu hello účet Dobrý den virtuálních počítačů.

## <a name="azure-monitor"></a>Azure Monitor
Problémy s výkonem v cloudové aplikace může mít vliv na vaši firmu. S více vzájemně propojena součástmi a často verzích může dojít, degradations kdykoli. A pokud vyvíjíte aplikace, uživatelé obvykle zjistit problémy, které nebyl nalezen v testování. Měli vědět o tyto problémy okamžitě a mít nástroje pro diagnostiku a řešení problémů hello.

[Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) je základní nástroj pro monitorování služby spuštěné v Azure. Nabízí data na úrovni infrastruktury o hello propustnost služby a které obaluje prostředí hello. Pokud spravujete své aplikace v Azure, rozhodování, zda tooscale nahoru nebo dolů prostředky, pak monitorování Azure vám dává můžete použít toostart.

Kromě toho můžete použít monitorování hlubšímu porozumění toogain data o vaší aplikaci. Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah. Obsahuje:

-   Protokol činnosti Azure
-   Azure diagnostických protokolů
-   Metriky
-   Diagnostika Azure

### <a name="azure-activity-log"></a>Protokol činnosti Azure
Je protokol, který poskytuje vhled do hello operací, které byly provedeny v prostředky ve vašem předplatném. Hello [protokol aktivit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) byla dříve označována jako "Protokoly auditu" nebo "Provozní protokoly,", protože oznámí události rovině řízení pro vaše předplatné.

### <a name="azure-diagnostic-logs"></a>Azure diagnostických protokolů
[Azure diagnostické protokoly](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) jsou vygenerované prostředek a poskytují bohatou a často data o operaci hello tohoto prostředku. obsah Hello tyto protokoly se liší podle typu prostředku.

Například protokoly událostí systému Windows jsou jednu kategorii protokolů diagnostiky pro virtuální počítače a objektů blob, table a queue protokoly jsou kategorie diagnostické protokoly pro účty úložiště.

Diagnostické protokoly se liší od hello [protokol aktivit (dříve označované jako protokol auditů nebo operační protokol)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Protokol aktivit Hello poskytuje náhled do hello operací, které byly provedeny v prostředky ve vašem předplatném. Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.

### <a name="metrics"></a>Metriky
Azure monitorování umožňuje tooconsume telemetrie toogain přehled hello výkonu a stavu úlohy v Azure. Hello nejdůležitější typ Azure telemetrická data je hello metriky (také nazývané čítače výkonu) vysílaných prostředků nejvíce Azure. Monitorování Azure poskytuje několik způsobů tooconfigure a využívat tyto [metriky](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) pro monitorování a řešení potíží.

### <a name="azure-diagnostics"></a>Diagnostika Azure
Je funkce hello v rámci Azure, která umožňuje hello shromažďování diagnostických dat na nasazené aplikace. Můžete použít rozšíření diagnostiky hello z různých různých zdrojů. Aktuálně podporované jsou [webové služby Azure Cloud a rolí pracovního procesu](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [virtuální počítače Azure](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) systémem Microsoft Windows, a [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="network-watcher"></a>Network Watcher
Zákazníci vytvářet síť začátku do konce v Azure tak, že orchestruje a skládání různé jednotlivých síťovým prostředkům, například virtuální síť, ExpressRoute, aplikační bránu, nástroje pro vyrovnávání zatížení a další. Monitorování je k dispozici na každém hello síťových prostředků.

komplexní konfigurace a interakce mezi prostředky, vytváření komplexní scénáře, které je třeba na základě scénáře monitorování prostřednictvím sledovací proces sítě, může mít síťové tooend Hello.

[Sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) bude usnadňuje monitorování a Diagnostika sítě Azure. Diagnostiky a vizualizace nástroje, které jsou k dispozici povolit sledovací proces sítě, které jste tootake vzdálené paketu zaznamená na virtuální počítač Azure, proniknout do vašeho síťového provozu pomocí protokolů z toku a diagnostikovat brána sítě VPN a připojení.

Sledovací proces sítě má aktuálně hello následující možnosti:

- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -poskytuje síti úrovně zobrazení zobrazující hello různé propojení a přidružení mezi síťovým prostředkům ve skupině prostředků.
-   [Proměnné zachytáváním paketů](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -zaznamená data paketů do/z virtuálního počítače. Pokročilé možnosti a podrobně nastavit ovládací prvky, jako je například se může tooset čas filtrování a univerzálnost zadejte omezení velikosti. Hello paketů data mohou být uložena v úložišti objektů blob nebo na místní disk hello ve formátu CAP.
-   [Ověřte IP toky](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) – ověří, zda je paket povolený nebo zakázaný na základě toku informace 5 řazené kolekce členů paketu parametrů (cílovou IP adresu, zdrojové IP adresy, cílový Port, zdrojový Port a protokol). Pokud paket hello je zakázané skupiny zabezpečení, hello pravidlo a skupiny, který odepřen hello paketů je vrácena.
-   [Další směrování](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -určuje hello další segment směrování paketů směrovány v hello prostředky infrastruktury sítě Azure, umožňuje toodiagnose všechny nesprávně nakonfigurované uživatelem definované trasy.
-   [Zobrazení skupiny zabezpečení](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -získá hello zabezpečení efektivní a použitých pravidel, která se použijí na virtuálním počítači.
-   [Protokolování toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) -toku protokoly pro skupinu zabezpečení sítě umožňují protokoly toocapture související se tootraffic, které jsou povolené nebo zakázané pravidla zabezpečení hello ve skupině hello. tok Hello je definována informací o 5-n-tice – zdrojové adresy IP, cílovou IP adresu, zdrojový Port, cílový Port a protokol.
-   [Brána virtuální sítě a řešení potíží s připojení](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -poskytuje možnost tootroubleshoot hello brány virtuální sítě a připojení.
-   [Sítě limity předplatného](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -umožňuje využití prostředků sítě tooview omezení.
-   [Konfigurace protokolu diagnostiky](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) – poskytuje jedno podokno tooenable nebo zakázat diagnostické protokoly pro síťové prostředky ve skupině prostředků.

toolearn více jak zjistit sledovací proces sítě tooconfigure, [konfigurace sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="developer-operations-devops"></a>Operace Developer (DevOps)
Vývoj aplikací předchozí tooDevOps, týmy byly starosti shromažďování podnikových požadavků pro softwarový program a psaní kódu. Samostatný tým QA pak testů v prostředí izolované vývoj programu hello, pokud byly splněny požadavky, a verze hello kód pro toodeploy operace. týmy Hello nasazení jsou další rozdělené na zastaralý skupin, jako je sítě a databáze. Pokaždé, když program softwaru je "vyvolána přes hello wall" nezávislé team tooan přidá kritická místa.

[DevOps](https://www.visualstudio.com/learn/what-is-devops/) umožňuje týmy toodeliver bezpečnější, kvalitnější řešení zrychluje a zlevňuje. Zákazníci očekávat dynamické a spolehlivé prostředí při využívání softwaru a služeb.  Týmy musí rychle iterovat aktualizace softwaru, měřit dopad hello hello aktualizace a rychle reagovat s novou vývoj iterací tooaddress problémy nebo zadejte další hodnotu.  Cloudové platformy, jako je Microsoft Azure mají odebrat tradiční kritická místa a pomohl commoditize infrastruktury. Software reigns v každé obchodní jako hello klíčovým rozdílem a faktorem obchodní výsledky. Žádná organizace, vývojáře nebo pracovníka IT může nebo byste neměli hello pohybů DevOps.

Profesionálové zabývající vyspělá DevOps přijmout řadu hello následující postupy. Tyto postupy [zahrnují osoby](https://www.visualstudio.com/learn/what-is-devops-culture/) tooform strategie podle hello obchodních scénářů.  Nástrojů můžete automatizovat hello různé postupy:

-   [Agilní plánování a řízení projektů](https://www.visualstudio.com/learn/what-is-agile/) techniky jsou použité tooplan a izolovat pracovní do sprintů, spravovat team kapacity a pomoci týmy rychle přizpůsobit toochanging obchodním potřebám.
-   [Správa verzí, obvykle s Gitem](https://www.visualstudio.com/learn/what-is-git/), umožňuje týmy umístěným kdekoliv ve zdroji tooshare hello world a integrovat softwaru vývoj nástroje tooautomate hello verzi kanálu.
-   [Průběžnou integraci](https://www.visualstudio.com/learn/what-is-continuous-integration/) jednotky hello Probíhající sloučení a testování kódu, což vede toofinding defekty již v rané fázi.  Mezi další výhody patří méně času ke znehodnocení části na požárů sloučení problémy a rychlé zpětnou vazbu pro vývojové týmy.
-   [Nastavené průběžné doručování](https://www.visualstudio.com/learn/what-is-continuous-delivery/) řešení softwaru tooproduction a testovacích prostředích pomůžou organizacím rychle opravit chyby, a odpověď změna tooever obchodní požadavky.
-   [Monitorování](https://www.visualstudio.com/learn/what-is-monitoring/) spuštěných aplikací, včetně produkčního prostředí pro stavu aplikace jako i jako zákazník využití pomoc organizacím formuláře předpoklad a rychle ověření nebo disprove strategie.  Bohaté data jsou zachytit a uložená v různých formátech protokolování.
-   [Infrastruktura jako kód (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) postupem, který umožňuje hello automatizace a ověření vytvoření a rušením toohelp sítě a virtuální počítače s doručováním hostování platformy zabezpečené, stabilní aplikace je.
-   [Mikroslužeb](https://www.visualstudio.com/learn/what-are-microservices/) architektura je případy obchodního použití využít tooisolate do malých opakovaně použitelné služby.  Tato architektura umožňuje škálovatelnost a efektivitu.

## <a name="next-steps"></a>Další kroky
toolearn informace o OMS zabezpečení a Audit řešení, najdete v části hello následující články:

- [Služby Operations Management Suite | Zabezpečení a dodržování předpisů](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Monitorování a odpovídá tooSecurity výstrahy nástroje Operations Management Suite zabezpečení a Audit řešení](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-responding-alerts).
- [Sledování prostředků v Operations Management Suite zabezpečení a Audit řešení](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-monitoring-resources).
