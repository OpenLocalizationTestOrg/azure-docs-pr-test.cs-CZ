---
title: "aaaDocument ochrany osobních údajů s Azure nástroje sestav | Microsoft Docs"
description: "jak toouse Azure reporting services a technologií toohelp ochrany osobních údajů osobních údajů."
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 3230d26ed308a8a0e72421c001793be06334a7c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="document-protection-of-personal-data-with-azure-reporting-tools"></a>Dokument ochranu osobních údajů v Azure, vytváření sestav nástroje

Tento článek popisuje, jak toouse Azure reporting services a technologií toohelp ochrany osobních údajů osobních údajů.

## <a name="scenario"></a>Scénář

Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy. toohelp úsilí, získala menší výletních Víceřádkový na základě v Itálii Německo, Dánsko a hello Spojené království

Hello společnost používá Microsoft Azure pro zpracování a ukládání podnikových dat. To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka. Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako jsou adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti. Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.

Zaměstnanci společnosti přístupu hello síti ze vzdálených pobočkách a agenty cesta hello společnosti nachází kolem hello, world mít přístup k prostředkům společnosti toosome.

## <a name="problem-statement"></a>Popis problému

Hello společnosti musí ochrany osobních údajů hello zaměstnanců a zákazníků osobních údajů prostřednictvím víceúrovňová bezpečnostní strategie, která používá Azure správy a zabezpečení funkce tooimpose přísné kontroly přístupu tooand zpracování osobních údajů a musí být možnost toodemonstrate jeho ochranných opatření toointernal a externí auditory.

## <a name="company-goal"></a>Cílem společnosti

V rámci své strategie zabezpečení obrany zabezpečení je cílem tootrack společnosti veškeré zpracování tooand přístup osobních údajů a zajistěte, aby odpovídající ochrany osobních údajů ochranu osobních údajů naleznete v dokumentaci a vyvíjí.

## <a name="solutions"></a>Řešení

Microsoft Azure poskytuje komplexní sledování, protokolování a toohelp diagnostické nástroje pro sledování dokumentů a záznamů aktivit a události související s přístup k informacím a zpracování osobní údaje, geografické toku dat a dat toopersonal přístup třetí strany. Vzhledem k zabezpečení osobních údajů v cloudu hello je sdílený odpovědnost, Microsoft také poskytuje zákazníkům:

- Podrobné informace o vlastní zpracování dat zákazníků

- Bezpečnostní opatření spravovaný společností Microsoft

- Kde a jak ho odesílá data zákazníků

- Podrobnosti o ochraně osobních údajů společnosti Microsoft vlastní zkontroluje proces

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) je společnosti Microsoft založená na cloudu, víceklientské adresáře a identity management service. Hello přihlášení služby a možnosti vytváření sestav auditování poskytují podrobné přihlášení a aplikace využití aktivity informace toohelp sledování a zajištění řádného přístup toocustomers a zaměstnanců osobní data.

Existují dva typy sestav aktivity:

- Hello [aktivity sestavy nebo protokoly auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) poskytují podrobné záznamy o aktivity nebo úlohy systému

- Hello [protokolech sestav aktivity přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) se dozvíte, kdo má provedl Každá aktivita uvedených v sestavě auditu hello

Pomocí hello dva společně, můžete sledovat hello historii každý úkol provést, a kdo každý provedl. Oba typy sestav jsou přizpůsobitelná a je možné filtrovat.

#### <a name="how-do-i-access-hello-audit-and-security-logs"></a>Jak získám přístup k zabezpečení a hello audit protokoly?

Hello protokoly auditu a zabezpečení je přístupná z portálu služby Active Directory hello třemi různými způsoby: pomocí hello **aktivity** část (vyberte buď **protokoly auditu** nebo **přihlášení**), nebo z **uživatelů a skupin** nebo **podnikové aplikace, které** pod **spravovat** ve službě Active Directory. Sestavy lze také přistupovat prostřednictvím hello Azure Active Directory reporting rozhraní API. 

1. V hello portálu Azure, vyberte **Azure Active Directory.**

2. V hello **aktivity** vyberte **protokoly auditu.**

    ![](media/protection-personal-data-azure-reporting-tools/image001.png)

3. Přizpůsobení zobrazení seznamu hello kliknutím **sloupce** v panelu nástrojů hello.

4.  Vyberte položku v toosee zobrazení seznamu hello dostupné podrobné informace o něm.

    ![](media/protection-personal-data-azure-reporting-tools/image003.png)

Generování sestav Azure Active Directory zahrnuje také dva typy sestav zabezpečení **uživatelé označení příznakem rizik** a **rizikové přihlášení**, který vám může pomoct sledovat potenciální rizika v prostředí Azure.

Další informace o vytváření sestav služby hello najdete v tématu [generování sestav Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)

Navštivte [sestav Azure Active Directory aktivity](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal#activity-reports) pro další podrobnosti o hello sestav, které jsou dostupné v Azure Active Directory. Tento web obsahuje podrobné informace o tom, tooaccess a používat [protokoly auditu sestavy aktivit](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) a [přihlašovací aktivita sestavy](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) hello portálu. Také obsahuje informace o [uživatelé označení příznakem rizik](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-user-at-risk) a [rizikové přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins) o zabezpečení.

Navštivte hello [auditování Azure Active Directory referenční dokumentace rozhraní API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference) Další informace o serveru tooconnect tooAzure Directory reporting prostřednictvím kódu programu.

### <a name="log-analytics"></a>Log Analytics

[Analýza protokolu](https://azure.microsoft.com/services/log-analytics/) můžete [shromažďování dat z Azure monitorování](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage) toocorrelate ji s ostatními daty a poskytnout další analýzu. Azure monitorování shromažďuje a analyzuje data sledování prostředí Azure. 

Nástrojů pro analýzu v analýzy protokolů například protokol hledání, zobrazení a řešení fungovat na všech shromážděná data vám poskytnou centralizované analýzu celé prostředí. Analýzy protokolů můžete agregovat a analyzovat protokoly událostí systému Windows, protokoly služby IIS a systémové protokoly, které lze zjistit potenciální narušení osobní údaje, které mohou vystavit uživatele toounauthorized osobní data.

#### <a name="how-do-i-use-log-analytics"></a>Použití analýzy protokolů

Analýzy protokolů můžete přistupovat prostřednictvím portálu OMS hello nebo hello portál Azure, z libovolného webového prohlížeče. Analýzy protokolů zahrnuje načtení tooquickly jazyk dotazu a slučování dat v úložišti hello. Můžete vytvořit a uložit protokol hledání toodirectly analyzovat data v portálu hello.

toocreate pracovní prostor analýzy protokolů v hello portál Azure, hello následující:

1. Vyberte **analýzy protokolů** z hello seznam služeb v hello Marketplace.

2. Vyberte **vytvořit,** pak zadejte název hello vaším pracovním prostorem OMS, vyberte předplatné, skupinu prostředků, umístění a cenovou úroveň.

3. Klikněte na tlačítko **OK** toodisplay seznam pracovní prostory.

4. Vyberte pracovní prostor toosee její podrobnosti.

    ![](media/protection-personal-data-azure-reporting-tools/image004.png)

Navštivte hello [analýzy protokolů dokumentaci](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) toolearn více informací o hello služby.

Navštivte hello [začít pracovat s pracovní prostor analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started) kurz toocreate pracovní prostor služby vyhodnocení a zjistěte, jak toouse hello služby základy hello.

Navštivte hello na způsob ukládání protokolů tooconnect toouse analýzy protokolů s hello následující webové stránky pro podrobnější informace popsané výše:

[Zdroje dat protokoly událostí systému Windows v analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)

[Služba IIS přihlásí analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs)

[Syslog zdroje dat v analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-syslog)

### <a name="azure-monitorazure-activity-log"></a>Protokol činnosti Azure monitorování nebo Azure 

[Azure monitorování](https://azure.microsoft.com/services/monitor/) poskytuje základní úroveň infrastruktura metriky a protokoly pro většinu služby ve službě Microsoft Azure.
Monitorování vám může pomoct toogain hlubšímu porozumění o aplikací Azure. Azure monitorování spoléhá na hello Azure rozšíření diagnostiky (Windows nebo Linux) ke shromažďování většina protokoly a metriky na úrovni aplikace. [Hello protokol činnosti Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) je jedním z hello prostředky můžete zobrazit pomocí Azure monitorování. Sleduje každé volání rozhraní API a poskytuje širokou řadu informace o aktivitách, které se vyskytují v [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
Hello protokol aktivit (dříve označovaný jako provozní nebo protokoly auditu) můžete vyhledat informace o vašem prostředku jakém ho vidí hello infrastruktury Azure. 

I když hodně hello informací zaznamenaných v hello aktivity protokolu vztahují tooperformance a služby stavu, je také informace, které jsou související tooprotection data. Pomocí hello protokolu činnosti, můžete určit hello ", kdo a kdy" pro všechny zápisu operace (PUT, POST, DELETE) pořízené hello prostředky ve vašem předplatném Azure.

Například poskytuje záznam, když správce odstraní skupinu zabezpečení sítě, což by mohlo mít vliv hello ochrany osobních údajů. Položky protokolu aktivit jsou uloženy v Azure monitorování 90 dní.

#### <a name="how-do-i-use-hello-data-collected-by-azure-monitor"></a>Použití hello data shromažďovaná společností monitorování Azure?

Existuje několik způsobů toouse hello data v protokolu aktivit hello a další prostředky Azure monitorování.

- Dá Streamovat hello dat tooother umístění skutečné řádku.

- Hello data můžete uložit pro delší dobu než hello výchozí hodnoty, pomocí [účtu úložiště Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction) a nastavení zásady uchovávání informací.

- Můžete visual hello data v grafy, pomocí hello [portál Azure](https://azure.microsoft.com/features/azure-portal/), [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), [Microsoft PowerBI](https://powerbi.microsoft.com/), nebo nástrojů pro vizualizaci třetích stran.

- Hello dat pomocí hello monitorování rozhraní REST API Azure, rozhraní příkazového řádku, se můžete dotazovat [prostředí PowerShell](https://docs.microsoft.com/powershell/) rutin, nebo hello .NET SDK.

Vyberte tooget začít s Azure monitorování **více služeb** v hello portálu Azure.

1. Posuňte se dolů příliš**monitorování** v hello **sledování a správu** části.

    ![](media/protection-personal-data-azure-reporting-tools/image005.png)

2.  Monitorování se otevře v hello **protokol aktivit** zobrazení.

    ![](media/protection-personal-data-azure-reporting-tools/image007.png)

Můžete vytvořit a uložit dotazy pro běžné filtry a pak pin hello nejdůležitější dotazy tooa řídicí panel portálu, takže budete vždy vědět, pokud došlo k události, které splňují kritéria.

1. Můžete filtrovat zobrazení hello skupinu prostředků, timespan a kategorie události.

    ![](media/protection-personal-data-azure-reporting-tools/image008.png)

2. Potom budete moct připnout řídicí panel portálu tooa dotazy kliknutím hello **Pin** tlačítko. To vám pomůže vytvořit jediný zdroj informací pro provozních dat na vaše služby. Název dotazu Hello a počet výsledků, zobrazí se na řídicí panel hello.

Můžete také použít hello monitorování tooview metriky pro všechny prostředky Azure, konfigurovat nastavení diagnostiky a výstrahy a hledání hello protokolu. Další informace o způsobu toouse hello monitorování Azure a protokol aktivit, najdete v části [Začínáme s Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

### <a name="azure-diagnostics"></a>Diagnostika Azure 

Možnosti diagnostiky Hello v Azure umožňuje shromažďování dat z různých zdrojů. protokoly událostí systému Windows Hello, které obsahují hello protokolu zabezpečení, může být obzvláště užitečný pro sledování a dokumentace ochrany osobních údajů. protokol zabezpečení Hello sleduje události úspěchy a chyby přihlášení, a také změny oprávnění, detekce vzory označující určité typy útoků, změny zásady zabezpečení, změn členství ve skupinách zabezpečení a mnoho dalšího.

Například 4695 ID události výstrahy jste se pokusili toohello unprotection kontrolovatelný chráněných dat. Tento krok se týká toohello Data Protection API (DPAPI), která pomáhá tooprotect data, jako jsou privátní klíče uložené přihlašovací údaje a jiné důvěrné údaje.

#### <a name="how-do-i-enable-hello-diagnostics-extension-for-windows-vms"></a>Jak povolit hello rozšíření diagnostiky pro virtuální počítače Windows?

Rozšíření prostředí PowerShell tooenable hello diagnostiky pro virtuální počítač s Windows, můžete tak jako toocollect data protokolu. Hello kroky, jak to udělat, závisí na modelu nasazení použijete (Resource Manager nebo Classic). tooenable hello rozšíření diagnostiky na existující virtuální počítač, který byl vytvořen pomocí modelu nasazení Resource Manager hello, můžete použít hello [rutiny prostředí PowerShell Set-AzureRMVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension?view=azurermps-4.3.1).

   ![](media/protection-personal-data-azure-reporting-tools/image009.png)

*\$diagnosticsconfig_path* je hello cesta toohello soubor, který obsahuje konfiguraci diagnostiky hello v kódu XML. Podrobné pokyny k povolení Azure Diagnostics na virtuálním počítači, najdete v části [použijte PowerShell tooenable Azure Diagnostics v virtuálního počítače se systémem Windows.](https://docs.microsoft.com/azure/virtual-machines/windows/ps-extensions-diagnostics)

Hello rozšíření diagnostiky Azure může přenést účtu úložiště Azure tooan hello shromažďovaných dat nebo odeslat tooservices například Application Insights. Potom můžete hello data pro auditování.

#### <a name="how-do-i-store-and-view-diagnostic-data"></a>Jak ukládat a zobrazení diagnostických dat?

Je důležité tooremember že diagnostických dat není uložena jako trvale, pokud přenos toohello Microsoft Azure storage emulátoru nebo tooAzure úložiště. toostore a zobrazení diagnostických dat ve službě Azure Storage, postupujte takto:

1. Zadejte účet úložiště v souboru ServiceConfiguration.cscfg souboru hello. Hello služby objektů Blob nebo hello služby Table, v závislosti na typu hello dat, můžete použít Azure Diagnostics. Protokoly událostí systému Windows se ukládají ve formátu tabulky.

2. Přenos dat hello. Může požádat o tootransfer hello diagnostických dat prostřednictvím hello konfigurační soubor. SDK 2.4 a předchozí můžete provést také hello požadavek prostřednictvím kódu programu.

3. Zobrazení dat hello, pomocí [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer), [Průzkumníka serveru](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) v sadě Visual Studio, nebo [Azure Diagnostics Manager](https://www.cerebrata.com/products/azure-diagnostics-manager) v Azure Management Studio.

Další informace o tom, tooperform z těchto kroků najdete v části [úložiště a zobrazení diagnostických dat ve službě Azure Storage.](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)

### <a name="azure-storage-analytics"></a>Azure Storage Analytics 

Úložiště analýzy protokolů podrobné informace o službě úložiště tooa úspěšné i neúspěšné požadavky. Tato informace může být použité toomonitor jednotlivých požadavků, které vám můžou pomoct při dokumentace přístupu toopersonal data uložená ve službě hello. Však není povoleno protokolování Storage Analytics ve výchozím nastavení svého účtu úložiště. Můžete ji povolit v hello portálu Azure.

#### <a name="how-do-i-configure-monitoring-for-a-storage-account"></a>Jak lze nakonfigurovat monitorování pro účet úložiště?

tooconfigure monitorování pro účet úložiště hello následující:

1. Vyberte **účty úložiště** v hello portálu Azure, pak vyberte název hello hello účtu má toomonitor.

    ![](media/protection-personal-data-azure-reporting-tools/image011.png)

2. V hello **monitorování** vyberte **diagnostiky.**

3.  Vyberte hello **typ** metriky dat chcete toomonitor u každé služby (soubor Blob, Table,). tooinstruct Azure Storage toosave diagnostické protokoly pro čtení, zápisu a odstranit požadavky hello blob, tabulky a fronty služby, vyberte **protokoly objektů Blob, tabulka protokoly** a **protokoly ve frontě.**

    ![](media/protection-personal-data-azure-reporting-tools/image013.png)

4. Pomocí posuvníku hello dolnímu hello, nastavte hello **uchování** zásad ve dnech (hodnota 1 – 365). Výchozí hello je sedm dní.

5. Vyberte **Uložit** tooapply hello konfigurační nastavení.

Položky protokolu protokolování úložiště obsahovat hello následující informace o jednotlivých požadavků:

- Načasování informace, jako je čas spuštění, začátku do konce latenci a dobu serveru.

- Podrobnosti o operaci hello úložiště jako typ operace hello hello klíč hello úložiště objekt hello klienta je přístup k, úspěch nebo selhání a vrátí toohello klienta hello stavový kód HTTP.

- Podrobnosti o ověřování třeba hello typ ověřování klienta hello používá.

- Concurrency informace, jako je hodnota ETag hello a naposledy upravené časové razítko.

- Hello velikosti hello zpráv žádostí a odpovědí.

Podrobnější pokyny o tom, najdete v části protokolování, analytika úložiště tooenable [monitorování účtu úložiště v hello portálu Azure.](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

### <a name="azure-security-center"></a>Azure Security Center 

[Azure Security Center](https://azure.microsoft.com/services/security-center/) monitorování hello stav zabezpečení vašich prostředků Azure v pořadí tooprevent detekovat hrozby a poskytovat doporučení pro reagovat. Poskytuje několik způsobů toohelp dokumentu vaše bezpečnostní opatření, které ochrany osobních údajů hello osobních údajů.

Sledování stavu zabezpečení pomáhá zajistit dodržování zásad zabezpečení. Sledování zabezpečení je proaktivní strategie, kdy se auditují vaše prostředky tooidentify systémy, které nesplňují organizační standardy nebo osvědčené postupy. Můžete sledovat stav zabezpečení hello hello následující prostředky:

- Výpočetní (virtuální počítače a cloudové služby)

- Sítě (virtuální sítě)

- Úložiště a data (server a databáze auditování a zjišťování hrozeb, šifrování TDE, šifrování úložiště)

- Aplikace (potenciální potíže se zabezpečením)

Problémy se zabezpečením v některém z těchto kategorií může představovat ochrany osobních údajů toohello threat osobních údajů.

#### <a name="how-do-i-view-hello-security-state-of-my-azure-resources"></a>Zobrazení stavu zabezpečení hello Moje prostředky Azure

Security Center pravidelně analyzuje stav zabezpečení hello vašich prostředků Azure. Zobrazí všechny potenciální ohrožení zabezpečení, který ji identifikuje v hello **prevence** část řídicího panelu hello.

   ![](media/protection-personal-data-azure-reporting-tools/image014.png)

1. V hello **prevence** části, vyberte hello **výpočetní** dlaždici. Uvidíte zde **přehled,** společně s hello **virtuální počítače** seznam všech virtuálních počítačů a stavy jejich zabezpečení a hello **cloudových služeb** seznam webových a pracovních rolí monitorovat Security Center.

2. Na hello **přehled** kartě, druhé doporučení tooview Další informace.

3. Na hello **virtuální počítače** vyberte další podrobnosti tooview virtuálních počítačů.

Při shromažďování dat je povolené v Azure Security Center, hello agenta Microsoft Monitoring Agent je automaticky zřízený na všechny stávající a nově podporované virtuální počítače, které jsou nasazeny. Data shromážděná z tohoto agenta je uložen v buď existující [analýzy protokolů](https://azure.microsoft.com/services/log-analytics/) prostoru přidružený k předplatnému nebo nový pracovní prostor.

[Hrozby sestavy Intelligence](https://docs.microsoft.com/azure/security-center/security-center-threat-report) jsou poskytovány Security Center. Tyto poskytují užitečné informace toohelp rozpoznat hello útočník identitu, cíle, aktuálním a historickém útoku kampaně a svoji, nástroje a postupy, které slouží. Informace o zmírnění a nápravy je rovněž obsažena.

primárním účelem Hello tyto hrozby sestavy je toohelp toorespond můžete efektivně měří toohello okamžitou hrozbě a nápovědy provést i později toomitigate hello problém. Hello informace v sestavách hello může být užitečná také při dokumentu vaší reakcí na incidenty pro generování sestav a auditování účely.

Hello Threat Intelligence sestavy jsou poskytovány. Formátu PDF, přístup prostřednictvím odkazů v hello **sestavy** pole z hello **podezřelé proces spuštěný** okno pro každou výstrahu zabezpečení v Azure Security Center.

Další informace o tom, jak tooview a používání hello Threat Intelligence sestavy naleznete v tématu [Azure Security Center Threat Intelligence sestavy.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

## <a name="next-steps"></a>Další kroky:

[Začínáme s Azure Active Directory, vytváření sestav API hello](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

[Co je služba Log Analytics?](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

[Přehled monitorování v Microsoft Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

[Úvod toohello protokol činnosti Azure (video)](https://azure.microsoft.com/resources/videos/intro-activity-log/)
