---
title: "aaaIntroduction tooAzure zabezpečení | Microsoft Docs"
description: "Další informace o zabezpečení Azure, jejích služeb a jak to funguje."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: TomSh
ms.openlocfilehash: 2d42057e9586a0b6ce16a1582db3b3af842297af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security"></a>Úvod tooAzure zabezpečení
## <a name="overview"></a>Přehled
Víme, že je zabezpečení úlohy, jeden v cloudu hello a jak důležité je, že zjistíte přesné a aktuální informace o zabezpečení Azure. Jedním z hello nejlepší toouse důvodů Azure pro vaše aplikace a služby je tootake výhod jeho širokou škálu zabezpečení nástroje a možnosti. Tyto nástroje a možnosti pomáhat při jeho možné toocreate zabezpečené řešení na platformu Azure zabezpečené hello. Microsoft Azure poskytuje utajení, integrita a dostupnost dat zákazníka, a zároveň umožnit transparentní odpovědnosti za.

toohelp lépe pochopit hello kolekce ovládacích prvků zabezpečení implementována v rámci Microsoft Azure z hello zákazníka a Microsoft operations perspektivy, tento dokument white paper "Úvod tooAzure zabezpečení", je zapsán tooprovide komplexní pohled na hello zabezpečení, které jsou dostupné v Microsoft Azure.

### <a name="azure-platform"></a>Platformy Azure
Azure je platforma služby veřejného cloudu, která podporuje široký výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení. Může probíhat Linux kontejnery s integrace Dockeru; vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js; sestavení back EndY pro iOS, Android a Windows zařízení.

Služby veřejného cloudu Azure podporovat hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti. Pokud sestavení nebo IT prostředky se mají migrovat, poskytovatele služeb veřejného cloudu se spoléhat na dané organizace dalo tooprotect vaší aplikace a data se službami hello a ovládací prvky hello poskytují toomanage hello zabezpečení vašeho cloudu prostředky.

Infrastruktury Azure a je určená ze zařízení tooapplications pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům splňovat požadavky jejich zabezpečení.

Kromě toho Azure poskytuje širokou škálu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vaší organizace nasazení. Tento dokument vám pomůže pochopit zabezpečení jak Azure funkce vám může pomoct splnění těchto požadavků.

> [!Note]
> Hello hlavním cílem tohoto dokumentu je na přístupném zákazníkovi ovládací prvky, můžete použít toocustomize a zvýšit zabezpečení pro aplikace a služby.
>
> Poskytujeme některé základní informace, ale podrobné informace o tom, jak Microsoft zabezpečuje hello platformy Azure, samostatně, najdete v části informací uvedených v hello [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Abstraktní
Na začátku migrace veřejného cloudu byly doprovází tooinnovate náklady úspory a flexibility. Zabezpečení byla považována za hlavní problém pro delší dobu a i zobrazit zátkou, pro migraci veřejného cloudu. Zabezpečení veřejného cloudu přešla ze hlavních však problém tooone hello ovladačů pro migrace do cloudu. důvody Hello je nadřízená možnost hello velké veřejný cloud service zprostředkovatelé tooprotect aplikací a dat hello cloudových prostředků.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům podle jejich potřeb zabezpečení. Kromě toho Azure poskytuje širokou škálu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby můžete přizpůsobit toomeet hello jedinečný požadavkům na zabezpečení vašeho nasazení toomeet IT zásad řízení a dodržovat tooexternal předpisy.

Tento dokument popisuje toosecurity přístup společnosti Microsoft v rámci Cloudová platforma Microsoft Azure hello:
* Funkce zabezpečení implementované Microsoft hello toosecure infrastrukturu Azure, zákaznická data a aplikace.
* Azure služeb i zabezpečení funkce dostupné tooyou toomanage hello zabezpečení hello služby a vaše data v rámci vašich předplatných Azure.

## <a name="summary-azure-security-capabilities"></a>Možnosti shrnutí zabezpečení Azure
Následující tabulka Hello zadejte stručný popis funkcí zabezpečení hello implementované toosecure hello infrastrukturu Azure, data zákazníků a zabezpečených aplikací společnosti Microsoft.
### <a name="security-features-implemented-toosecure-hello-azure-platform"></a>Zabezpečení funkce implementovány tooSecure hello platformě Azure:
Funkce Hello uvedené následující jsou možnosti, můžete zkontrolovat tooprovide hello záruku této hello platformě Azure je spravovat zabezpečeným způsobem. Byly zadány odkazy pro další podrobnostem na tom, jak Microsoft adresy dotazy zákazníků důvěryhodnosti čtyři oblasti: zabezpečení platformy, ochrany osobních údajů a ovládací prvky, dodržování předpisů a průhlednost.


| [Zabezpečené platformy](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Ochrana osobních údajů a ovládací prvky](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Dodržování předpisů](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Průhlednost](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Cyklus zaměřený na zabezpečení vývoj](https://www.microsoft.com/en-us/sdl/)Interní audity | [Data spravovat celou dobu hello](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Centrum zabezpečení](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Jak Microsoft zabezpečuje dat zákazníka ve službách Azure](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Povinné bezpečnostního školení, pozadí kontroly](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc) |  [Řízení na umístění dat](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Běžné ovládací prvky rozbočovače](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Jak spravovat Microsoft umístění dat do služby Azure](http://azuredatacentermap.azurewebsites.net/)|
| [Testování průnikům](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc), [zjišťování neoprávněných vniknutí DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagemen), [audity & protokolování](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Poskytovat přístup k datům na vaše podmínky](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [Hello cloudové služby kvůli opatrností kontrolní seznam](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[Kdo v Microsoft má přístup k datům na jaké podmínky](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Stav systému datové centrum obrázky](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fyzického zabezpečení [zabezpečení sítě](https://docs.microsoft.com/en-us/azure/security/security-network-overview) | [Odpovídá toolaw vynucení](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Dodržování předpisů podle umístění & odvětví služeb](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Jak Microsoft zabezpečuje dat zákazníka ve službách Azure](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Reakcí na incidenty zabezpečení](http://aka.ms/SecurityResponsepaper), [sdílené odpovědnosti](http://aka.ms/sharedresponsibility) |[Standardy přísné ochrany osobních údajů](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Zkontrolujte certifikační službách Azure průhlednost rozbočovače](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-toosecure-data-and-application"></a>Zabezpečení funkce nabízené sítěmi Azure tooSecure dat a aplikací
V závislosti na modelu služby hello cloudu je proměnné odpovědnost, pro který je zodpovědný za správu zabezpečení hello hello aplikace nebo služby. Možnosti nejsou k dispozici v tooassist hello platformě Azure můžete ke splnění těchto odpovědnosti prostřednictvím integrovaných funkcí a prostřednictvím partnerských řešení, které lze nasadit do předplatného Azure.

integrované funkce Hello jsou uspořádány do šesti (6) funkčním oblastem: operace, aplikace, úložiště, sítě, výpočetních a Identity. Další podrobnosti o hello funkce a možnosti, které jsou k dispozici v hello platformě Azure v těchto oblastech půl (6) jsou k dispozici prostřednictvím souhrnné informace.

## <a name="operations"></a>Operace
Tato část obsahuje další informace o klíčových funkcích v operacích zabezpečení a souhrnné informace o těchto možnostech.

### <a name="operations-management-suite-security-and-audit-dashboard"></a>Operations Management Suite zabezpečení a Audit řídicí panel
Hello [OMS zabezpečení a Audit řešení](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) poskytuje komplexní pohled na vaše organizace IT postavení zabezpečení s [integrovaný vyhledávací dotazy](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) pro významné problémy, které vyžadují vaši pozornost. Hello [zabezpečení a Audit](https://technet.microsoft.com/library/mt484091.aspx) řídicí panel je hello domovskou obrazovku všem související toosecurity v OMS. Poskytuje podrobný přehled o hello stav zabezpečení vašich počítačů. Zahrnuje také možnost tooview hello všechny události z hello za posledních 24 hodin, 7 dní, nebo jakékoli jiné vlastní časový rámec.

Kromě toho můžete nakonfigurovat OMS zabezpečení a dodržování předpisů příliš[automaticky provádět určité akce](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) při zjištění určité události.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager ](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) vám umožní toowork s hello prostředky ve vašem řešení jako se skupinou. Můžete nasadit, aktualizovat nebo odstranit všechny hello prostředky pro vaše řešení v rámci jediné koordinované operace. Můžete použít [šablony Azure Resource Manageru](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) pro nasazení a tato šablona může fungovat v různých prostředích, jako je testování, pracovní a provozní. Resource Manager poskytuje zabezpečení, auditování a označování funkce toohelp spravovat prostředky po nasazení.

Nasazení založené na šablonách Azure Resource Manager pomůže zlepšit zabezpečení hello řešení nasazené v Azure, protože standardní zabezpečení řídit nastavení a můžete integrovat do standardizované nasazení na základě šablon. Tím se snižuje riziko hello chyby konfigurace zabezpečení, které může probíhat během ruční nasazení.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) je rozšiřitelný služba Správa výkonu aplikace (APM) pro vývojářům webů. S nástrojem Application Insights můžete monitorovat za chodu webových aplikací a automaticky zjišťovat anomálie výkonu. Zahrnuje výkonné analytics nástroje toohelp diagnostikovat problémy a toounderstand uživatelů ve skutečnosti při práci s aplikací. Sleduje aplikace všechny hello, když je spuštěná, během testování a po publikována nebo nasazená.

Application Insights vytvoří grafů a tabulek, které můžete zobrazit, například jaké časy den zobrazí většina uživatelů, jak je aplikace reaguje hello, a také, který je poskytovaný žádné externí služby, které závisí na.

Pokud dojde k chybě, chyby nebo problémy s výkonem, můžete hledat hello telemetrická data v příčina hello toodiagnose podrobností. A hello služby vám pošle e-mailů Pokud nedošlo k žádným změnám v hello dostupnosti a výkonu vaší aplikace. Přehled aplikace proto stane nástroj cenné zabezpečení, protože pomáhá s dostupností hello v hello důvěrnosti, integrity a zabezpečení chaloupka dostupnosti.

### <a name="azure-monitor"></a>Azure Monitor
[Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) nabízí vizualizace, dotaz, směrování, výstrahy, automatické škálování a automatizace dat i z hello infrastrukturu Azure ([protokol aktivit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)) a každý jednotlivých prostředků Azure ([ Diagnostické protokoly](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). Můžete použít Azure monitorování tooalert jste na události související se zabezpečením, které se generují protokolů Azure.

### <a name="log-analytics"></a>Log Analytics
[Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) součástí [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – poskytuje do řešení pro správu IT pro místní i cloudové infrastruktuře jiných výrobců (například AWS) navíc tooAzure prostředky. Lze ho směrovat dat z Azure monitorování přímo tooLog analýzy, abyste viděli metriky a protokoly pro celé prostředí na jednom místě.

Analýzy protokolů může být užitečné nástroje forenzní a další analýzu zabezpečení hello nástroj umožňuje vám tooquickly hledání prostřednictvím velkých objemů záznamy související se zabezpečením s přístupem flexibilní dotazování. Kromě toho místní [protokoly brány firewall a proxy serveru můžete exportovat do Azure a k dispozici pro analýzu pomocí analýzy protokolů.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](https://docs.microsoft.com/azure/advisor/) je konzultantem přizpůsobené cloudu, který vám pomůže toooptimize Azure nasazení. Analyzuje telemetrii využití a konfiguraci prostředků. Potom doporučí řešení toohelp zlepšení hello [výkonu](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [zabezpečení](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), a [vysokou dostupnost](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) vašich prostředků při hledání příležitosti příliš[snížit vaše celkové Azure tráví](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Azure Advisor poskytuje doporučení zabezpečení, které může výrazné zlepšení vaší celkové postavení zabezpečení pro řešení, které můžete nasadit v Azure. Tato doporučení jsou vykreslovány z provádí analýzu zabezpečení [Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) pomáhá zabránit, zjišťovat a odpovědět toothreats s lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Kromě toho Azure Security Center pomáhá s operace zabezpečení tím, že poskytuje jednoho řídicího panelu této výstrahy ploch a doporučení, které můžete reagovali na ni okamžitě. Často můžete opravit problémy s jedním kliknutím v rámci konzoly hello Azure Security Center.
## <a name="applications"></a>Aplikace
Hello části naleznete další informace o klíčových funkcích v aplikaci zabezpečení a souhrnné informace o těchto možnostech.

### <a name="web-application-vulnerability-scanning"></a>Webové aplikace ohrožení zabezpečení kontrolu
Jedním z nejjednodušší tooget způsoby hello začít s testování pro chyby zabezpečení v vaše [aplikaci služby App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is) je toouse hello [integrace s Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) ohrožení zabezpečení jedním kliknutím tooperform kontrolu vaše aplikace. Můžete zobrazit výsledky testů hello v sestavě snadno pochopit a zjistěte, jak toofix jednotlivé chyby zabezpečení s podrobnými pokyny.

### <a name="penetration-testing"></a>Testování průniku
Pokud dáváte přednost tooperform vlastní průnikům testy nebo mají toouse jiný skener suite nebo poskytovatele, je třeba postupovat podle hello [Azure průnikům testování procesu schvalování](https://security-forms.azure.com/penetration-testing/terms) a získat průnikům hello potřeby tooperform předchozí schválení testy.

### <a name="web-application-firewall"></a>Brány firewall webových aplikací
Hello brány firewall webových aplikací (firewall webových aplikací) v [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) pomáhá chránit před běžné útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a zneužití relace webové aplikace. Vybavená předem nakonfigurovaným rozhraním ochrany před hrozbami identifikovaný hello [otevřete webovou aplikaci zabezpečení projektu (OWASP) jako hello top 10 známých chyb zabezpečení](https://msdn.microsoft.com/library/).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Ověřování a autorizace v prostředí Azure App Service
[Aplikace služby ověřování / autorizace](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) je funkce, která poskytuje způsob, jak pro vaše aplikace toosign mezi uživateli, takže nemáte toochange kód na back-end aplikace hello. Poskytne tooprotect snadný způsob, jak vaše aplikace a pracovní uživatelská data.

### <a name="layered-security-architecture"></a>Architektura vrstveného zabezpečení
Vzhledem k tomu [prostředí App Service](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-intro) poskytuje izolovaném prostředí nasadit do [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), vývojáři můžou vytvářet architektura vrstveného zabezpečení poskytuje různé úrovně přístup k síti pro každou vrstvu aplikace. Společné přání je toohide rozhraní API back EndY z obecné přístup k Internetu a povolit pouze toobe rozhraní API, které jsou volány nadřazeného webové aplikace. [Skupin zabezpečení (Nsg) sítě](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) jde použít na podsítě virtuální sítě Azure obsahující prostředí App Service toorestrict veřejný přístup tooAPI aplikace.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Webový server diagnostics a application diagnostics
Webové aplikace služby App Service poskytují diagnostické funkce pro protokolování informací z hello webový server a hello webové aplikace. Tyto jsou logicky rozdělené do [webový server diagnostiky](https://docs.microsoft.com/azure/app-service-web/web-sites-enable-diagnostic-log) a [rozhraní application diagnostics](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Webový server zahrnuje dva hlavní pokroky v diagnostice a řešení potíží s weby a aplikace.

první novou funkci Hello je informace o fondech aplikací, pracovních procesů, lokality, aplikační domény a spuštěné žádosti v reálném čase stavu. druhé nové výhody Hello jsou hello podrobné trasovat události, které trasování požadavku v rámci dokončení procesu požadavků a odpovědí hello.

kolekce hello tooenable tyto události trasování, IIS 7, může být nakonfigurované tooautomatically protokoly úplné trasování zachycení, ve formátu XML pro všechny konkrétní požadavek na základě uplynulý čas nebo Chyba kódů odpovědi.

#### <a name="web-server-diagnostics"></a>Diagnostika webového serveru
Můžete povolit nebo zakázat hello následující typy protokolů:

-   Podrobné protokolování chyb - podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (kód stavu 400 nebo vyšší). To může obsahovat informace, které vám mohou pomoci určit, proč hello server vrátil kód chyby hello.

-   Trasování neúspěšných žádostí - podrobné informace o chybných žádostech, včetně trasování hello IIS komponenty používané tooprocess hello požadavku a hello doba v jednotlivých součástí. To může být užitečné, pokud se pokoušíte výkonu webu tooincrease nebo izolovat, co ho způsobuje konkrétní toobe chyby HTTP vrátil.

-   Webový Server protokolování - informace o použití formátu souborů protokolu rozšířené hello W3C transakce HTTP. To je užitečné, když chcete určit celkový metriky lokality, jako je například hello počet požadavků zpracovaných nebo je počet požadavků z konkrétní IP adresu.

#### <a name="application-diagnostics"></a>Rozhraní Application diagnostics
[Rozhraní Application diagnostics](https://docs.microsoft.com/azure/app-service-web/web-sites-enable-diagnostic-log) můžete informace o toocapture vytvořil webovou aplikací. Aplikace ASP.NET můžete použít hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) rozhraní application diagnostics třída toolog informace toohello protokolu. V konzoli Application Diagnostics existují dva hlavní typy událostí, ty související s tooapplication výkonu a ty související tooapplication selháním a chybami. Hello selhání a chyby lze rozdělit dále do problémy s připojením, zabezpečení a selhání. Problémy selhání jsou obvykle související tooa problém s kódem aplikace hello.

V konzoli Application Diagnostics lze zobrazit události seskupené následujícími způsoby:

-   Vše (zobrazí se všechny události)
-   Chyby aplikace (události výjimky zobrazí)
-   Výkon (zobrazí se události výkonu)

## <a name="storage"></a>Úložiště
Hello části naleznete další informace o klíčových funkcích v zabezpečení a souhrnné informace o těchto možnostech úložiště Azure.

### <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Můžete zabezpečit váš účet úložiště pomocí řízení přístupu na základě rolí (RBAC). Omezení přístupu podle hello [potřebovat tooknow](https://en.wikipedia.org/wiki/Need_to_know) a [nejnižší oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) Principy zabezpečení je nutné pro organizace, které mají tooenforce zásady zabezpečení pro přístup k datům. Tato oprávnění jsou uděluje přiřazením toogroups hello odpovídající RBAC role a aplikace v určité oboru. Můžete použít [předdefinované role RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles), jako je Přispěvatel účet úložiště, tooassign oprávnění toousers. Přístup k toohello úložiště klíčů pro účet úložiště pomocí hello [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) modelu se dá řídit přes řízení přístupu na základě Role (RBAC).

### <a name="shared-access-signature"></a>Sdílený přístupový podpis
A [sdílený přístupový podpis (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště. Hello SAS znamená udělit, že klient v zadaném období a se zadanou sadou oprávnění omezená oprávnění tooobjects ve vašem účtu úložiště. Tato omezená oprávnění můžete udělit bez nutnosti tooshare klíče pro přístup k účtu.

### <a name="encryption-in-transit"></a>Šifrování během přenosu
Šifrování během přenosu je mechanismus ochrany dat při přenosu v sítích. S Azure Storage můžete zabezpečit pomocí dat:
-   [Šifrování transportní vrstvy](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), jako je například HTTPS při přenosu dat do nebo z Azure Storage.

-   [Propojit šifrování](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), jako například [šifrování SMB 3.0](https://docs.microsoft.com/azure/storage/storage-security-guide) pro [Azure sdílené složky](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   Šifrování na straně klienta, tooencrypt hello data předtím, než se přenese do úložiště a toodecrypt hello data po se přenáší z úložiště.

### <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Pro mnoho společností je šifrování dat v klidovém stavu povinný krok k ochrany osobních údajů, dodržování předpisů a suverenity data. Existují tři funkcí zabezpečení úložiště Azure, které poskytují šifrování dat, která je "v klidovém stavu":

-   [Šifrování služby úložiště](https://docs.microsoft.com/azure/storage/storage-service-encryption) vám umožní toorequest, že služba úložiště hello automaticky šifrování dat při zápisu ho tooAzure úložiště.

-   [Šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) taky nabízí funkci hello šifrování v klidovém stavu.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) vám umožní disky tooencrypt hello operačního systému a datové disky používané virtuálním počítačem IaaS.

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) provádí protokolování a poskytuje data metriky pro účet úložiště. Můžete použít tento tootrace požadavky na data, analyzovat trendy využití a diagnostikovat problémy s vaším účtem úložiště. Úložiště analýzy protokolů podrobné informace o službě úložiště tooa úspěšné i neúspěšné požadavky. Tato informace může být použité toomonitor jednotlivých požadavků a toodiagnose problémy se službou úložiště. Na základě typu best effort protokolované požadavky. jsou zaznamenány následující typy požadavků na ověření Hello:
-   Úspěšné požadavky.

-   Neúspěšné požadavky, včetně vypršení časového limitu, omezení šířky pásma, sítě, autorizace a dalších chyb.

-   Požadavky na použití sdíleného přístupového podpisu (SAS), včetně úspěšné a neúspěšné požadavky.

-   Data tooanalytics požadavků.

### <a name="enabling-browser-based-clients-using-cors"></a>Povolení založené na prohlížeči klientů pomocí CORS
[Sdílení prostředků různých původů (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) mechanismus, který umožňuje domén toogive vzájemně oprávnění pro přístup k prostředkům druhé strany. Hello uživatelský Agent odešle tooensure další hlavičky, který kód JavaScript hello načtený z určité domény je povolen tooaccess prostředky nachází v jiné doméně. doméně pozdější Hello poté odpoví odpovědí s další hlavičky povolit nebo odmítnout hello původní přístup do domény tooits prostředky.

Úložiště Azure services nyní podpory CORS, aby po nastavení hello pravidla CORS pro službu hello správně ověřeného požadavku odeslaného službě hello z jiné domény se vyhodnotí toodetermine, jestli je povolená podle toohello pravidla, které máte zadat.
## <a name="networking"></a>Sítě
Hello části naleznete další informace o klíčových funkcích v síti Azure informace o zabezpečení a souhrnné informace o těchto možnostech.

### <a name="network-layer-controls"></a>Ovládací prvky sítě vrstev
Řízení přístupu k síti je hello v rámci omezení tooand připojení z konkrétní zařízení nebo podsítě a představuje hello základní zabezpečení sítě. cílem Hello řízení přístupu k síti je toomake se, že virtuální počítače a služby jsou dostupné tooonly uživatelů a zařízení toowhich chcete je přístupný.

#### <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
A [skupina zabezpečení sítě (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) je základní stavových paketů filtrování brány firewall a umožňuje toocontrol přístup na základě [5 řazené kolekce členů](https://www.techopedia.com/definition/28190/5-tuple). Skupiny Nsg neposkytují kontroly vrstvy aplikací nebo ověřený řízení přístupu. Mohou být použity toocontrol provoz přesun mezi podsítěmi v rámci virtuální síť Azure a provoz mezi Azure Virtual Network a hello Internetu.

#### <a name="route-control-and-forced-tunneling"></a>Řízení směrování a vynucené tunelování
Hello možnost toocontrol chování směrování na virtuálních sítí Azure je kritických síťových funkce řízení zabezpečení a přístup. Pokud chcete toomake jistotu, že všechny přenosy tooand z vaší virtuální sítě Azure prochází této virtuální zabezpečení zařízení, například musí mít toocontrol toobe a přizpůsobit chování směrování. To provedete tak, že nakonfigurujete trasy definované uživatelem v Azure.

[Trasy definované uživatelem](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) umožňují toocustomize příchozí a odchozí cesty pro provoz přesouvání do a z jednotlivých virtuálních počítačů nebo podsítě tooinsure hello nejbezpečnější možné trasy. [Vynuceného tunelování](https://www.petri.com/azure-forced-tunneling) je mechanismus, který můžete použít tooensure, které vaše služby nejsou povoleny tooinitiate toodevices připojení na hello Internetu.

To se liší od je možné tooaccept příchozí připojení a pak neodpovídá toothem. Webových serverů front-end potřebovat toorespond toorequests z hostitelů v Internetu, a proto Source internetové komunikace povolena příchozí toothese webové servery a webové servery hello může reagovat.

Vynucené tunelování je běžně používané tooforce odchozí přenosy toohello Internet toogo prostřednictvím místní zabezpečení proxy servery a brány firewall.

#### <a name="virtual-network-security-appliances"></a>Virtuální síť zabezpečovací zařízení
Zatímco skupin zabezpečení sítě, trasy definované uživatelem a vynucené tunelování poskytují úroveň zabezpečení v hello sítě a transportní vrstvy hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), může nastat situace, kdy chcete tooenable zabezpečení s vyšší úrovní Hello zásobníku. Tyto funkce Rozšířené sítě zabezpečení dostanete pomocí zařízení řešení zabezpečení sítě partnera Azure. Můžete najít hello nejaktuálnější řešení zabezpečení sítě partnera Azure návštěvou hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) a při vyhledání "zabezpečení" a "zabezpečení sítě."

### <a name="azure-virtual-network"></a>Azure Virtual Network

Virtuální sítě Azure (VNet) je reprezentace vlastní sítě v cloudu hello. Je to logická izolace hello předplatné tooyour fabric vyhrazené sítě Azure. Můžete plně řídit hello bloky IP adres, nastavení DNS, zásady zabezpečení a směrovací tabulky v rámci této sítě. Můžete segmentovat do podsítí virtuální sítě a umísťovat virtuální počítače Azure IaaS (VM) nebo [cloudové služby (instance rolí PaaS)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) na virtuálních sítí Azure.

Kromě toho se můžete připojit hello virtuální sítě tooyour místní síti pomocí jedné z hello [možnosti připojení](https://docs.microsoft.com/azure/vpn-gateway/) v Azure k dispozici. V zásadě můžete rozšířit vaše síť tooAzure, s úplnou kontrolou nad bloky IP adres s výhodou hello měřítka enterprise, které poskytuje Azure.

Azure sítě podporuje různé scénáře zabezpečený vzdálený přístup. Mezi ty patří:

-   [Připojení jednotlivých pracovních stanicích tooan Azure Virtual Network](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [Připojit místní sítě tooan Azure Virtual Network prostřednictvím sítě VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [Připojit místní sítě tooan Azure Virtual Network s vyhrazenou propojení WAN](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Připojit virtuální sítě Azure tooeach jiné](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN Gateway
toosend síťový provoz mezi vaší virtuální sítě Azure a místními servery, je nutné vytvořit bránu VPN pro virtuální síť Azure. A [brány VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) je typ brány virtuální sítě, které odesílá šifrovaný provoz přes veřejný připojení. Můžete také použít VPN Gateway toosend provoz mezi virtuálními sítěmi Azure přes hello prostředky infrastruktury sítě Azure.

### <a name="express-route"></a>ExpressRoute
Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) je vyhrazené propojení WAN, který umožňuje rozšířit vaše místní sítě do hello cloudu Microsoftu přes vyhrazené soukromé připojení zajišťované poskytovatelem připojení.

![ExpressRoute](./media/azure-security/azure-security-fig1.png)

Prostřednictvím ExpressRoute můžete vytvořit připojení tooMicrosoft cloudové služby, jako je například Microsoft Azure, Office 365 a CRM Online. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), síť Ethernet typu point-to-point nebo virtuální křížové připojení prostřednictvím poskytovatele připojení ve společném umístění.

Připojení ExpressRoute se nepřenášejí prostřednictvím veřejného Internetu hello a proto lze považovat za bezpečnější než řešení založených na VPN. To umožňuje toooffer připojení ExpressRoute Další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická připojení přes hello Internet.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) poskytuje [aplikace doručení řadiče (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaši aplikaci.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

Umožní vám toooptimize webové farmy produktivitu přesměrováním zátěže procesoru náročné SSL ukončení toohello Aplikační brána (také označované jako "Přesměrování zpracování SSL" nebo "Přemostění SSL"). Poskytuje také další možnosti směrování vrstvy 7 včetně kruhového dotazování distribuce příchozí provoz, spřažení na základě souboru cookie relace, na základě cestu směrování adres URL a hello možnost toohost více webů za jeden Application Gateway. Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.

Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.

Aplikace nabízí řadu funkcí aplikace doručení řadiče (ADC), včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, [Secure Sockets Layer (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) testy snižování zátěže, vlastní stavu, podporu pro více lokalit, a mnohé další.

### <a name="web-application-firewall"></a>Web Application Firewall (Brána firewall webových aplikací)
Brány Firewall webových aplikací je funkce [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) , poskytuje ochranu tooweb aplikace, které používají aplikační brány pro standardní funkce doručování aplikací ovládací prvek (ADC). Brány firewall webových aplikací dosahuje tím, že je pro většinu hello OWASP top 10 běžné webové chyb zabezpečení, které chrání.

![Web Application Firewall (Brána firewall webových aplikací)](./media/azure-security/azure-security-fig1.png)

-   Ochrana před útoky prostřednictvím injektáže SQL.

-   Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.

-   Ochrana před narušením protokolu HTTP.

-   Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.

-   Ochrana před roboty, prohledávacími moduly a skenery.

-   Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)


Centralizované webové aplikace brány firewall tooprotect proti útokům na web umožňuje mnohem jednodušší správu zabezpečení a poskytuje lepší záruku toohello aplikaci proti hrozbám hello vniknutí. Řešení firewall webových aplikací můžete rovněž reagovat ohrožení zabezpečení tooa rychlejší podle opravy známých ohrožení zabezpečení do centrálního umístění a zabezpečení těchto jednotlivých webových aplikací. Existující aplikace, který může být brány snadno převést tooan aplikační bránu s brány firewall webových aplikací.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) vám umožní toocontrol hello distribuce provozu generovaného uživateli pro koncové body služby v různých datových střediscích. Koncové body služby, které jsou podporovány nástrojem Traffic Manager zahrnují virtuální počítače Azure, webové aplikace a cloudové služby. Službu Traffic Manager můžete používat také s externími koncovými body mimo Azure. Traffic Manager používá hello systému DNS (Domain Name) toodirect klient požádá o toohello nejvhodnější koncového bodu, na základě [metodu směrování provozu](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) a stavu hello hello koncových bodů.

Traffic Manager poskytuje řadu směrování provozu metody toosuit jinou aplikaci potřeby, koncový bod stavu [monitorování](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)a automatické převzetí služeb při selhání. Správce provozu je odolný toofailure, včetně hello selhání celé oblasti Azure.
### <a name="azure-load-balancer"></a>Nástroj pro vyrovnávání zatížení Azure
[Azure nástroj pro vyrovnávání zatížení](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) zajišťuje vysoké dostupnosti a výkonu v síti tooyour aplikace. Je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která distribuuje příchozí komunikaci mezi pořádku instancí služby definované v sadě s vyrovnáváním zatížení. Azure nástroj pro vyrovnávání zatížení můžete nakonfigurovat:

-   Vyrovnávat zatížení příchozí internetové přenosy toovirtual počítačů. Tato konfigurace se označuje jako [Vyrovnávání zatížení internetového](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Přenosy Vyrovnávání zatížení mezi virtuálními počítači ve virtuální síti, mezi virtuálními počítači v cloudové služby nebo mezi místními počítači a virtuální počítače ve virtuální síti mezi různými místy. Tato konfigurace se označuje jako [Vyrovnávání zatížení pro vnitřní](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Přesměrovat provoz externí tooa konkrétní virtuální počítač

### <a name="internal-dns"></a>Interní DNS
Můžete spravovat hello seznam serverů DNS se používá ve virtuální síti v hello portálu pro správu nebo v konfiguračním souboru na hello sítě. Zákazníka můžete přidat až too12 servery DNS pro každý virtuální síť. Při zadávání serverů DNS, je důležité tooverify, můžete seznam serverů DNS zákazníka ve správném pořadí hello prostředí zákazníka. Seznamy serveru DNS nefungují kruhového dotazování. Jsou použity v pořadí hello, že jsou uvedené. Pokud hello první server DNS v seznamu hello je možné toobe dosaženo, klient hello používá tento server DNS, bez ohledu na to, zda je hello DNS server fungovat správně nebo není. odebrat servery DNS hello ze seznamu hello toochange hello pořadí serveru DNS pro virtuální síť zákazníka a přidat je zpět v pořadí hello tohoto zákazníka chce. DNS podporuje hello dostupnosti aspektů chaloupka zabezpečení hello "c oddílu".

### <a name="azure-dns"></a>Azure DNS
Hello [Domain Name System](https://technet.microsoft.com/library/bb629410.aspx), nebo DNS, je zodpovědný za překladu (nebo vyřešení) k webu nebo službě název tooits IP adresu. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) je hostitelská služba domén DNS, poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure. Hostování domény do Azure, můžete spravovat DNS záznamů pomocí hello stejné přihlašovací údaje, rozhraní API, nástroje a fakturace jako jinými službami Azure. DNS podporuje hello dostupnosti aspektů chaloupka zabezpečení hello "c oddílu".
### <a name="log-analytics-nsgs"></a>Skupiny Nsg analýzy protokolů
Můžete povolit hello následující kategorie protokolů diagnostiky pro skupiny zabezpečení sítě:
-   Události: Obsahuje položky, u které skupina NSG použitá tooVMs a instance rolí na základě adresy MAC jsou pravidla. Hello stav pro tato pravidla se shromažďují každých 60 sekund.

-   Čítač pravidel: obsahuje položky pro počet opakování každého pravidla NSG je použité toodeny nebo povolení provozu.

### <a name="azure-security-center"></a>Azure Security Center
Security Center pomáhá zabránit, zjistit a reagovat toothreats a zajišťuje, že jste vyšší přehled a kontrolu nad, hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, které byste jinak nevšimli a spolupracuje s řadou řešení zabezpečení. Síťového střediska doporučení ohledně brány firewall, skupiny zabezpečení sítě, konfiguraci pravidla pro příchozí provoz a další.

Doporučení sítě k dispozici jsou následující:

-   [Přidat Brána Firewall příští generace](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) doporučuje přidat brány Firewall pro další generace (NGFW) z tooincrease partnera Microsoft vaše ochranu zabezpečení

-   [Směrování provozu prostřednictvím NGFW pouze](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) doporučuje, které nakonfigurujete sítě pravidla zabezpečení skupiny (NSG), která vynutit tooyour příchozí přenosy virtuálních počítačů prostřednictvím vaší NGFW.

-   [Povolení skupin zabezpečení sítě pro podsítě nebo virtuální počítače](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) doporučuje, abyste povolili skupiny Nsg na podsítě nebo virtuálních počítačů.

-   [Omezení přístupu prostřednictvím internetové koncový bod](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) doporučuje konfigurace pravidla pro příchozí provoz pro skupiny Nsg.


## <a name="compute"></a>Compute

Hello části naleznete další informace o klíčových funkcích v této oblasti a souhrnné informace o těchto možnostech.

### <a name="antimalware--antivirus"></a>Antimalwarové & antivirové
S Azure IaaS můžete antimalwarový software od dodavatelů zabezpečení, jako je Microsoft, Symantec, Trend Micro, McAfee a Kaspersky tooprotect virtuálních počítačů ze škodlivých souborů, adwaru a dalšími hrozbami. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) pro Azure Cloud Services a virtuálních počítačů je ochrana funkci, která pomáhá identifikovat a odstraňovat viry, spyware a další škodlivý software. Antimalware od Microsoftu poskytuje konfigurovat výstrahy, když známé tooinstall pokusy o škodlivého nebo nežádoucího softwaru, sám sebe nebo spustit v Azure systémech. Antimalware od Microsoftu můžete nasadit také pomocí Azure Security Center

### <a name="hardware-security-module"></a>Modul hardwarového zabezpečení
Šifrování a ověřování zlepšení zabezpečení Pokud samotné hello klíče jsou chráněny. Hello správy a zabezpečení vašich důležitých tajných klíčů a klíče můžete zjednodušit tím, že je v ukládání [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault poskytuje možnost toostore hello klíče v hardwaru zabezpečení (HSM) moduly certifikovaných tooFIPS 140-2 Level 2 standardů. Klíčů pro šifrování SQL Server pro zálohování nebo [transparentní šifrování dat](https://msdn.microsoft.com/library/bb934049.aspx) všechny se uloží Key Vault se klíčů nebo tajných údajů z vašich aplikací. Oprávnění a přístupu toothese chráněné položky se spravují prostřednictvím [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Záloha virtuálního počítače
[Zálohování Azure](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) je řešení, které chrání data aplikací s nulové kapitálovými investicemi a minimálními provozní náklady. Chyby aplikace může dojít k poškození dat a lidské chyby může způsobit chyby do vaší aplikace, které může způsobit problémy s toosecurity. S Azure Backup jsou chráněné virtuální počítače s Windows a Linux.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Důležitou součástí vaší organizace [obchodní kontinuity nebo zotavení po havárii (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) strategie je tím, jak dochází k tookeep firemní úlohy a aplikace nahoru a spuštěna při plánovaných a neplánovaných výpadků. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) pomáhá orchestraci replikace, převzetí služeb při selhání a obnovení úloh a aplikací, aby byly k dispozici ze sekundární lokality Pokud primární lokalita ocitne mimo provoz.

### <a name="sql-vm-tde"></a>VIRTUÁLNÍ POČÍTAČ TDE SQL
[Transparentní šifrování dat (šifrování TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) a šifrování na úrovni sloupce (Vymazat) jsou funkcí šifrování SQL serveru. Tato forma šifrování vyžaduje toomanage zákazníků a úložiště hello kryptografických klíčů, které používáte pro šifrování.

Hello službou Azure Key Vault (AZURE) služby je navrženou tooimprove hello zabezpečení a správu tyto klíče v umístění zabezpečené a vysoce dostupné. Hello konektor služby serveru SQL umožňuje systému SQL Server toouse tyto klíče z Azure Key Vault.

Pokud používáte systém SQL Server s místní počítače, je kroků můžete postupovat podle tooaccess Azure Key Vault z vašeho místního počítače systému SQL Server. Ale pro SQL Server ve virtuálních počítačích Azure, můžete ušetřit čas pomocí funkce Azure Key Vault integrace hello. S několika tooenable rutin prostředí Azure PowerShell tuto funkci můžete automatizovat hello konfigurace, které jsou nezbytné pro virtuální počítač SQL tooaccess trezoru klíčů.

### <a name="vm-disk-encryption"></a>Šifrování disku virtuálního počítače
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) je novou funkci, která vám pomůže zašifrovat disky virtuálního počítače s Windows a Linux IaaS. Bude se vztahovat hello oborový standard BitLocker funkce systému Windows a DM-Crypt funkce hello Linux tooprovide svazku šifrování pro hello operačního systému a datové disky hello. Hello řešení jsou integrované s Azure Key Vault toohelp řídit a spravovat hello šifrování disku klíče a tajné klíče v rámci vašeho předplatného Key Vault. Hello řešení také zajistí, že jsou všechna data na disky virtuálního počítače hello zašifrovaná přinejmenším ve službě Azure storage.

### <a name="virtual-networking"></a>Virtuální síť
Třeba virtuální počítače připojení k síti. toosupport tento požadavek Azure vyžaduje toobe virtuální počítače připojené tooan Azure Virtual Network. Virtuální síť Azure je logická konstrukce nástavbou hello Azure síťových prostředcích infrastruktury. Každý logický [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) je izolovaný od všech ostatních Azure virtuálních sítí. Tato izolace pomáhá zajistit, že síťový provoz v nasazeních není přístupný tooother Microsoft Azure zákazníků.

### <a name="patch-updates"></a>Oprava aktualizací
Oprava aktualizací zadejte hello základ pro hledání a opravě potenciální problémy a zjednodušit hello proces správy aktualizací softwaru, protože se sníží hello počet aktualizací softwaru, které je nutné nasadit ve vašem podniku a zvýšením vaší toomonitor možnost dodržování předpisů.

### <a name="security-policy-management-and-reporting"></a>Správa zásad zabezpečení a vytváření sestav
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) pomáhá zabránit, zjistit a reagovat toothreats a poskytuje vyšší přehled a kontrolu nad, hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, které byste jinak nevšimli a spolupracuje s řadou řešení zabezpečení.

### <a name="azure-security-center"></a>Azure Security Center
Security Center pomáhá zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## <a name="identify-and-access-management"></a>Identifikovat a správy přístupu

Zabezpečení systémů, aplikací a dat začíná řízení přístupu na základě identity. Hello identit a přístupu funkce správy, které jsou součástí obchodní produktů a služeb Microsoftu pomáhají chránit vaše organizace a osobní informace před neoprávněným přístupem při zajistit uživatelům k dispozici toolegitimate kdykoli a kdekoli je potřebují.

### <a name="secure-identity"></a>Zabezpečené Identity
Společnost Microsoft používá více postupy zabezpečení a technologií napříč svých produktů a služeb toomanage identity a přístup.
-   [Služba Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) vyžaduje uživatelé toouse několik metod pro přístup, místně a v cloudu hello. Poskytuje silné ověřování s celou řadu možností snadno ověření při požadavků uživatelů s jednoduchý proces přihlášení.

-   [Microsoft Authenticator](https://aka.ms/authenticator) poskytuje uživatelsky přívětivý možnosti vícefaktorového ověřování, které pracuje s Microsoft Azure Active Directory a účty Microsoft a zahrnuje podporu pro wearables a na základě otisku prstu schválení.

-   [Vynucení zásad hesel](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) zvyšuje zabezpečení tradiční hesel hello uložením délku a složitost, vynutit pravidelné otáčení a pokusy o uzamčení účtu po selhání ověření.

-   [Ověřování na základě tokenu](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) umožňuje ověřování prostřednictvím služby Active Directory Federation Services (AD FS) nebo tokenu zabezpečení systémů jiných výrobců.

-   [Řízení přístupu na základě role (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) umožňuje toogrant přístup na základě uživatele hello přiřazenou roli, takže je easy toogive uživatelé pouze hello úroveň přístupu potřebují tooperform jejich úkolů úlohy. RBAC můžete přizpůsobit podle vaší organizace obchodnímu modelu a odolnost vůči rizikům.

-   [Integrovaná správa identity (hybridní identita)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) vám umožní toomaintain řízení přístupu uživatelů napříč interní datovými centry a cloudové platformy, vytváření prostředků tooall identitu jednoho uživatele pro ověřování a autorizaci.

### <a name="secure-apps-and-data"></a>Zabezpečení aplikací a dat
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/), komplexní identit a přístupu cloudové řešení pro správu, pomáhá zabezpečit přístup toodata v aplikacích v lokalitě a v cloudu hello a zjednodušuje správu hello uživatelů a skupin. Zkombinuje základní adresářové služby, pokročilé zásady správného řízení identity, zabezpečení a správu přístupu aplikace a usnadňuje vývojářům toobuild identity na základě zásad správy do své aplikace. tooenhance služby Azure Active Directory, můžete přidat placené možnosti pomocí Azure Active Directory Basic, Premium P1 a Premium P2 edice hello.

| Volné / běžné funkce     | Základní funkce    |Premium P1 funkce |Premium P2 funkce | Azure Active Directory Join – pouze souvisejících funkcí Windows 10|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Objekty adresáře](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#directory-objects), [uživatele nebo skupiny správy (přidání, aktualizace nebo odstranění) / založená na uživateli zřizování, registrace zařízení](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#usergroup-management-addupdatedelete-user-based-provisioning-device-registration), [jednotné přihlašování (SSO)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#single-sign-on-sso), [samoobslužné služby Změna hesla pro uživatele cloud](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-change-for-cloud-users), [Connect (synchronizační modul, který rozšiřuje tooAzure místního adresáře služby Active Directory)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-sync-engine-that-extends-on-premises-directories-to-azure-active-directory), [zabezpečení / Reports využití](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#securityusage-reports)       |     [Správa přístupu na základě skupiny / zřizování](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#group-based-access-managementprovisioning), [samoobslužného resetování hesel uživatelům cloudu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-reset-for-cloud-users), [firemní Branding (vlastní nastavení přihlašovací stránky nebo přístupový Panel)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#company-branding-logon-pagesaccess-panel-customization), [ Proxy aplikací](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#application-proxy), [SLA 99,9 %](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#sla-999) |  [Skupiny samoobslužné služby a aplikace správy/samoobslužných-služeb aplikace dodatky nebo dynamické skupiny](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-group), [heslo samoobslužného resetování nebo změny nebo odemknout s místními zpětný zápis](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-resetchangeunlock-with-on-premises-write-back), [Multi-Factor Ověřování (Cloud a místní (MFA Server))](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#multi-factor-authentication-cloud-and-on-premises-mfa-server), [MIM CAL + serveru MIM](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mim-cal-mim-server), [Cloud App Discovery](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#cloud-app-discovery), [Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-health), [ Výměna automatické heslo pro skupinové účty](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#automatic-password-rollover-for-group-accounts)|     [Ochrana identity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-configure)|    [Připojení zařízení tooAzure AD, plocha jednotné přihlašování, Microsoft Passport pro Azure AD, obnovení nástroje Bitlocker správce](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery), [MDM automatický zápis, obnovení nástroje Bitlocker samoobslužného, další místní správci zařízení tooWindows 10 přes Azure Připojení AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mdm-auto-enrollment)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) je funkce premium, Azure Active Directory, která vám umožní tooidentify cloudové aplikace, které používají zaměstnanci hello ve vaší organizaci.

- [Azure Active Directory Identity Protection](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) je služba zabezpečení, která využívá Azure Active Directory anomálií detekce možnosti tooprovide ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které mohou ovlivnit vaše organizace identity.

- [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) umožňuje toojoin virtuálních počítačích Azure tooa domény bez hello potřebovat toodeploy řadiče domény. Uživatelé přihlásit toothese virtuálních počítačů pomocí své podnikové přihlašovací údaje služby Active Directory a bezproblémově přistupovat k prostředkům.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) je služba správy vysokou dostupností, globální identity pro určených aplikace, které můžete škálovat toohundreds milionů identit a integrovat do různých mobilních a webové platformy. Vašim zákazníkům přihlásit tooall aplikace prostřednictvím přizpůsobitelné používající existujících účtů na sociálních sítích, nebo můžete vytvořit nové samostatné přihlašovací údaje.

- [Azure Active Directory spolupráce B2B ve](https://aka.ms/aad-b2b-collaboration) je zabezpečený partnera integrační řešení, které podporuje vaše vztahy mezi tím, že umožňuje partnerům tooaccess podnikovým aplikacím a datům selektivně pomocí jejich autonomně spravovaná identity.

- [Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) vám umožní tooextend cloudu možnosti tooWindows 10 zařízení pro centralizovanou správu. To umožňuje uživatelům tooconnect toohello podnikové nebo organizace cloudu prostřednictvím Azure Active Directory a zjednodušuje tooapps přístup a prostředky.

- [Azure Proxy aplikace Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) zajišťuje jednotné přihlašování a zabezpečený vzdálený přístup pro webové aplikace hostované na místě.

## <a name="next-steps"></a>Další kroky
- [Začínáme s Microsoft Azure zabezpečení](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Služby a funkce, které můžete použít toohelp Azure zabezpečení služeb a dat v rámci Azure

- [Azure Security Center](https://azure.microsoft.com/services/security-center/)

Zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure

- [Sledování stavu zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

Hello možnosti monitorování v Azure Security Center toomonitor dodržování zásad.
