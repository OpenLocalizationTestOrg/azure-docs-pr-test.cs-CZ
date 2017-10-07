---
title: aaaAzure Advanced detekce hrozeb. | Microsoft Docs
description: "Další informace o ochrany identit a možnosti."
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
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 63e2c541fd4ce2c571af9d5845c9a9bd4b03b2a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-advanced-threat-detection"></a>Azure Advanced Threat detekce
## <a name="introduction"></a>Úvod

### <a name="overview"></a>Přehled

Společnost Microsoft vyvinula řadu dokumenty White Paper, zabezpečení přehledy, osvědčené postupy a kontrolní seznamy tooassist Azure zákazníků týkající se hello různé související se zabezpečením možnosti dostupné v a okolního hello platformě Azure. témata Hello rozsahu z hlediska spektra a hloubky a jsou pravidelně aktualizovány. Tento dokument je součástí této řady dle souhrnu v následující části abstraktní hello.

### <a name="azure-platform"></a>Platformy Azure

Azure je platforma služby veřejného cloudu, která podporuje hello nejširší výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení.
Podporuje následující programovací jazyky hello:
-   Spusťte Linux kontejnery s integrace Dockeru.
-   Vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js
-   Sestavení back EndY pro iOS, Android a Windows zařízení.

Služby veřejného cloudu Azure podporovat hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti.

Když provádíte migraci tooa veřejného cloudu s organizací, dané organizace je zodpovědná tooprotect vaše data a zadejte zabezpečení a zásad správného řízení kolem hello systému.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům podle jejich potřeb zabezpečení. Azure nabízí širokou škálu možností tooconfigure a přizpůsobit toomeet hello požadavkům na zabezpečení nasazení aplikací. Tento dokument vám tyto požadavky splňují pomůže.

### <a name="abstract"></a>Abstraktní

Microsoft Azure nabízí integrovanou funkci zjišťování rozšířené hrozba prostřednictvím služeb, jako třeba Azure Active Directory, Azure Operations Management Suite (OMS) a Azure Security Center. Tato kolekce služeb zabezpečení a možnosti poskytuje toounderstand jednoduchým a rychlým způsobem, co se děje v rámci Azure nasazení.

Tento dokument vám pomohou hello "Microsoft Azure přístupy" směrem k ohrožení zabezpečení threat diagnostiky a analyzování hello rizika spojená s hello škodlivých aktivit cílené na servery a dalším prostředkům služby Azure. To vám pomůže tooidentify hello metody identifikace a ohrožení zabezpečení správy s optimalizovaný řešení hello platformy Azure a službami zabezpečení zákazníkem a technologie.

Tento dokument se zaměřuje na hello technologie platformy Azure a ovládacích prvků zákazníkem a nepokusí tooaddress SLA, ceny modely a aspekty postupem DevOps.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

![Azure Active Directory Identity Protection](./media/azure-threat-detection/azure-threat-detection-fig1.png)


[Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) je funkce hello [Azure AD Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) edici, která poskytuje přehled o rizikových událostech hello a potenciální ohrožení zabezpečení, které mají vliv na vaší organizace identity. Microsoft byl přes deset aplikace zabezpečení cloudových identit pro, a s Azure AD Identity Protection Microsoft je provádění tyto systémy stejné ochrany k dispozici tooenterprise zákazníků. Ochrana identit využívá možností detekce anomálií existující Azure AD je k dispozici prostřednictvím [neobvyklé aktivity sestav Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports#anomalous-activity-reports)a zavádí nové typy událostí rizik, které můžete zjišťovat anomálie v reálném čase.

Ochrana identity používá algoritmy adaptivní strojového učení a heuristiky toodetect anomálií a rizikových událostech, které může znamenat, že byl napaden identity. Na základě těchto dat Identity Protection generuje sestavy a výstrahy, které umožňují tooinvestigate těchto rizik událostí a proveďte příslušné akce zmírnění nebo nápravy.

Ale Azure Active Directory Identity Protection je větší než nástroj pro monitorování a vytváření sestav. Na základě riziko událostí, Identity Protection vypočítá úroveň rizika uživatele pro každého uživatele, umožňuje na základě riziko zásady tooconfigure tooautomatically chránit hello identity vaší organizace.

Tyto zásady na základě riziko, v přidání tooother [řízení podmíněného přístupu](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) poskytuje služba Azure Active Directory a [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), můžete automaticky blokovat nebo nabízejí adaptivní nápravné akce, které obsahují Resetování hesel a vynucení služby Multi-Factor authentication.

### <a name="identity-protections-capabilities"></a>Možnosti ochrany identit

Azure Active Directory Identity Protection je větší než monitorování a vytváření sestav nástroje. tooprotect identity ve vaší organizaci, můžete nakonfigurovat zásady na základě rizik, které automaticky odpovídat toodetected problémy po dosažení zadané riziko úroveň. Tyto zásady, kromě tooother podmíněný přístup k ovládací prvky, které poskytuje Azure Active Directory a EMS, můžete buď automaticky blokovat nebo zahájit adaptivní nápravné akce, včetně resetování hesel a vynucení služby Multi-Factor authentication.

Uvedené příklady některých hello způsoby ochrany identit Azure může pomoci zabezpečit vaše účty a identity zahrnují:

[Zjišťování rizikových událostech a rizikové účty:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Zjišťování šest typů událostí rizik pomocí strojového učení a heuristické pravidla
-   Výpočet úrovní rizika uživatele
-   Poskytování vlastních doporučení tooimprove celkové postavení zabezpečení podle zvýraznění ohrožení zabezpečení

[Zkoumání rizikových událostí:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Odesílání oznámení o rizikových událostech
-   Zkoumání rizikových událostí pomocí relevantní a kontextové informace
-   Poskytuje základní pracovních tootrack šetření
-   Poskytuje snadný přístup tooremediation akcí, jako je resetování hesla

[Zásady podmíněného přístupu na základě rizika:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#risky-sign-ins)
-   Zásady toomitigate rizikové přihlášení blokování přihlášení nebo že vyřeší problémy spojené služby Multi-Factor authentication.
-   Zásady tooblock nebo zabezpečený rizikové uživatelské účty
-   Tooregister uživatelé toorequire zásad pro službu Multi-Factor authentication

### <a name="azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM)

S [Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure),

![Azure AD Privileged Identity Management](./media/azure-threat-detection/azure-threat-detection-fig2.png)

můžete spravovat, řízení a monitorování přístupu v rámci vaší organizace. To zahrnuje přístup tooresources ve službě Azure AD a dalších služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.

Azure AD Privileged Identity Management vám pomůže:

-   Objeví se výstraha a tvorba sestav o Správci služby Azure AD a "právě v čase" tooMicrosoft přístup pro správu Online služby, jako je Office 365 a Intune

-   Získání sestavy o historii přístup správce a změny v přiřazení správců

-   Dostávat upozornění na přístup tooa privilegované role

## <a name="microsoft-operations-management-suite-oms"></a>Microsoft Operations Management Suite (OMS)

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury. Vzhledem k tomu, že je OMS implementována jako cloudová služba, je možné ji zprovoznit velmi rychle a s minimální investicí do služeb infrastruktury. Nové funkce zabezpečení se dodávají automaticky, ukládání průběžnou údržbu a upgradovat náklady.

Kromě toho tooproviding cenné služeb v jeho vlastní, OMS může integrovat součástí produktu System Center, jako [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/) tooextend stávající investice správu zabezpečení do hello cloudu. System Center a OMS vzájemně spolupracují tooprovide úplné hybridní Správa prostředí.

### <a name="holistic-security-and-compliance-posture"></a>Komplexní zabezpečení a dodržování předpisů postavení

Hello [OMS zabezpečení a Audit řídicí panel](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) poskytuje komplexní pohled na vaše organizace IT postavení zabezpečení s integrovaný vyhledávací dotazy pro významné problémy, které vyžadují vaši pozornost. řídicí panel zabezpečení a Audit Hello je hello domovskou obrazovku všem související toosecurity v OMS. Poskytuje podrobný přehled o stavu zabezpečení hello vašich počítačů. Zahrnuje také možnost tooview hello všechny události z hello za posledních 24 hodin, 7 dní, nebo jakékoli jiné vlastní časový rámec.

Nápověda OMS řídicích panelů můžete snadno a rychle pochopit hello celkové postavení zabezpečení jakékoli prostředí vše v rámci kontextu hello IT oddělení, včetně: posouzení aktualizace softwaru, antimalwarových a standardní hodnoty konfigurace. Kromě toho je snadno přístupné data protokolu zabezpečení toostreamline hello zabezpečení a dodržování předpisů auditování procesů.

řídicí panel OMS zabezpečení a Audit Hello je uspořádány do čtyř hlavních kategorií:

![Řídicí panel Zabezpečení a audit v OMS](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

-   **Zabezpečení domény:** v této oblasti, budete moct toofurther prozkoumat záznamy zabezpečení v čase, přístup k posouzením malwaru, aktualizovat hodnocení, zabezpečení sítě, informace o přístupu a identit, počítače s událostmi zabezpečení a rychle mají řídicí panel Security Center tooAzure přístup.

-   **Významné problémy:** této možnosti lze tooquickly identifikovat hello počet aktivní problémy a hello závažnost těchto problémů.

-   **Detekce (Preview):** vám umožní vzorů útoků tooidentify vizualizací výstrahy zabezpečení, která je uloženo na vaše prostředky.

-   **Analýzou hrozeb:** vám umožní vzorů útoků tooidentify vizualizací hello celkový počet servery s odchozím škodlivým provozem IP, typu hello zjištění ohrožení a mapu, která ukazuje, kde jsou tyto IP adresy pocházejících z.

-   **Běžné dotazy zabezpečení:** poskytuje tuto možnost, můžete seznam nejčastějších zabezpečení hello dotazy, které můžete použít toomonitor prostředí. Po kliknutí na jednu z těchto dotazů, otevře se okno vyhledávání hello s hello výsledky pro tento dotaz.

### <a name="insight-and-analytics"></a>Statistiky a analýza
Hello středu [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) je hello OMS úložiště, který je hostován v hello cloudu Azure.

![Statistiky a analýza](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Data jsou shromažďována do úložiště hello z připojených zdrojů tak, že konfigurace zdroje dat a přidání předplatného tooyour řešení.

![předplatné](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Zdroje dat a řešení se vytvoří každý jiný záznam typy, které mají své vlastní sadu vlastností, ale může stále analyzovat společně v úložišti toohello dotazy. To vám umožní toouse hello stejné nástroje a metody toowork s různými druhy dat shromažďovaných pomocí různých zdrojů.


Většina interakce se analýzy protokolů je prostřednictvím portálu OMS hello, který běží v libovolného prohlížeče a vám poskytne přístup tooconfiguration nastavení a více tooanalyze nástroje a postupovat podle shromážděná data. Z portálu hello, můžete použít [protokolu hledání](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) tam, kde vytvoříte dotazy tooanalyze shromažďovaných dat, [řídicí panely](https://docs.microsoft.com/azure/log-analytics/log-analytics-dashboards), které můžete přizpůsobit pomocí grafické zobrazení nejcennější hledání a [řešení](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), které poskytují další funkce a analytických nástrojích.

![nástrojů pro analýzu](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Řešení přidat funkce tooLog Analytics. Se primárně spustit v cloudu hello a poskytnout analýzy dat shromážděných v úložišti OMS hello. Také se může definovat nové typy záznamů toobe shromážděných, který lze analyzovat pomocí protokolu hledání nebo další uživatelské rozhraní služby hello řešení hello OMS řídicím panelu.
Hello zabezpečení a Audit je příkladem těchto typů řešení.



### <a name="automation--control-alert-on-security-configuration-drifts"></a>Automatizace a řízení: drifts výstrah Konfigurace zabezpečení

Automatizace Azure umožňuje automatizovat procesy správy s sady runbook, které jsou založené na prostředí PowerShell a spusťte v hello cloudu Azure. Sady Runbook lze také provést na serveru v místních prostředků vaše místní data center toomanage. Azure Automation nabízí správy konfigurací pomocí prostředí PowerShell DSC (Konfigurace požadovaného stavu).

![Azure Automation](./media/azure-threat-detection/azure-threat-detection-fig7.png)

Můžete vytvořit a spravovat prostředky DSC hostované v Azure a použít je toocloud a místních systémech toodefine a automaticky vynutit jejich konfigurace nebo získat sestavy na odlišily toohelp zajistěte, aby zůstanou konfigurace zabezpečení v rámci zásad.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center pomáhá chránit prostředky v Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure. V rámci služby hello, budete moct toodefine zásady nejen pro svá předplatná Azure, ale i proti [skupiny prostředků](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), aby mohli být podrobnější.

![Azure Security Center](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Zabezpečení výzkumníci společnosti Microsoft jsou neustále na hello hledejte hrozeb. Mají přístup tooan rozsáhlou sadu telemetrie získaných globální přítomnosti společnosti Microsoft v hello cloudu a místně. Tuto kolekci dosažení celou a různých datových sad umožňuje Microsoft toodiscover nových vzorů útoků a trendů v jeho místní spotřebních a podnikových produktech, jakož i jeho online služeb.

Proto Security Center můžete rychle aktualizovat své detekční algoritmy jako útočníci vydání nové a stále sofistikované zneužití. Tento přístup umožňuje držet krok s prostředím přesunutí fast hrozeb.

![Security Center](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Detekce hrozeb Security Center funguje tak, že automaticky shromažďování informací o zabezpečení z vaše prostředky Azure, sítě hello a připojených partnerských řešení.  Analyzuje tyto informace korelace informace z více zdrojů, tooidentify hrozeb.
Výstrahy zabezpečení budou mít vyšší prioritu v Centru zabezpečení včetně doporučení, na tom, jak tooremediate hello hrozeb.

Služba Security Center využívá pokročilou analýzu zabezpečení, která daleko překračuje možnosti detekce založené na signaturách či příznacích. Změnám ve velkých objemů dat a [strojového učení](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologie jsou události použité tooevaluate napříč hello celý cloudových prostředků infrastruktury – zjišťování hrozeb, které by bylo možné tooidentify ruční přístupy a predikci hello vývoj útoků. Tyto analýzy zabezpečení zahrnuje následující hello.

### <a name="threat-intelligence"></a>Analýza hrozeb

Společnost Microsoft má k dispozici rozsáhlé zdroje globální analýzy hrozeb.
Telemetrická data proudí z více zdrojů, například Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, hello Microsoft digitální činů jednotky (DCU) a Microsoft Security Response Center (MSRC).

![Analýza hrozeb](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Výzkumní pracovníci také přijímat intelligence informací o hrozbách, která je sdílena mezi poskytovatele hlavní cloudových služeb a přihlásí se k odběru informačních kanálů intelligence toothreat od jiných výrobců. Azure Security Center můžete použít tento tooalert informace můžete toothreats ze známé nesprávnými účastníky. Možné příklady:

-   **Využití hello Power Machine Learning -** Azure Security Center má přístup tooa velká množství dat o síti aktivita v cloudu, což může být použit toodetect hrozeb cílení na vaše nasazení Azure. Například:

-   **Útok hrubou silou detekce -** strojového učení je použité toocreate historických vzor pokusy o vzdáleného přístupu, to umožňuje útoky hrubou silou toodetect porty SSH, RDP a SQL.

-   **Odchozí DDoS a detekce Botnet** -společného cíle útoků cílení na cloudové prostředky je toouse hello výpočetní výkon tyto prostředky tooexecute jiným útokům.

-   **Nové chování Analytics serverů a virtuálních počítačů -** po ohrožení server nebo virtuální počítač, útočníci využít při vyloučení detekce, zajištění trvalosti a spravovatelný širokou škálu techniky tooexecute škodlivý kód v tomto systému ovládací prvky zabezpečení.

-   **Detekce hrozeb databáze Azure SQL -** detekce hrozeb pro databázi SQL Azure, které identifikuje nezvyklé databázové aktivity, které indikují neobvyklou a potenciálně škodlivé pokusí tooaccess nebo zneužití databází.

### <a name="behavioral-analytics"></a>Behaviorální analýza

Pro vypracování analýzy chování jde o techniku, která analyzuje a porovná tooa shromažďování dat ze známé vzorů. Tato schémata však nepředstavují jednoduché příznaky. Jsou určené prostřednictvím algoritmy komplexní strojového učení, které jsou použité toomassive datové sady.

![Behaviorální analýza](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)


Určují se také prostřednictvím pečlivé analýzy škodlivého chování, kterou provádí zkušení analytici. Azure Security Center může používat prostředky pro vypracování analýzy chování tooidentify ohrožení zabezpečení na základě analýzy protokolů virtuálního počítače, protokolů zařízení virtuální sítě, infrastruktury protokoly, výpisy stavu systému a jiných zdrojů.

Kromě toho je korelace s další toocheck signály pro podporu důkaz rozšířeným kampaně. Tato korelace pomáhá tooidentify události, které jsou konzistentní s vytvořeným ukazatele ohrožení zabezpečení.

Možné příklady:
-   **Provádění podezřelé procesu:** útočníci využít několik technik tooexecute škodlivého softwaru bez detekce. Například útočník může poskytnout malwaru hello stejné názvy jako soubory legitimní systému, ale tyto soubory umístit do alternativního umístění, použijte název, který se velmi jako neškodné soubor nebo soubor hello maska true rozšíření. Security Center modelů procesů chování a monitorování procesu spuštěních toodetect extrémních takovéto.

-   **Skrytý malwaru a jeho zneužití pokusů:** sofistikované malwaru můžete obejít tradiční antimalwarové produkty nikdy zápis toodisk nebo šifrování softwarové komponenty, které jsou uložená na disku. Však takové malwaru lze zjistit pomocí analýza paměti jako hello malware musí zůstat trasování v paměti toofunction. Když dojde k chybě softwaru, stav systému zaznamená část paměti v době hello hello havárie. Analýzou hello paměti v hello stav systému, můžete zjistit, že techniky používá tooexploit ohrožení zabezpečení v softwaru, přístup důvěrných dat a tajně zachovat v rámci ohroženého počítače bez dopadu na výkon hello Azure Security Center váš počítač.

-   **Pomoci odhalit laterální pohyb a interní rekognoskace:** toopersist v ohrožení zabezpečení sítě a vyhledejte/sklizně cenných dat, útočníci často usilují toomove následně k laterálnímu z hello ohrožené hello tooothers počítače ve stejné síti. Security Center sleduje proces a přihlašovací aktivity toodiscover pokusí tooexpand útočníka dostane v rámci hello sítě, jako je vzdálené spouštění příkazů, zjišťování sítě a účtů.

-   **Škodlivých skriptů prostředí PowerShell:** prostředí PowerShell můžete použít útočníci tooexecute škodlivý kód na cílových virtuálních počítačů pro různé účely. Služba Security Center kontroluje aktivitu prostředí PowerShell a hledá známky podezřelé aktivity.

-   **Odchozí útoků:** útočníci často cíle cloudových prostředků s cílem hello pomocí těchto prostředků toomount další útoky. Ohrožené virtuální počítače, například může být použité toolaunch útoky hrubou silou na jiné virtuální počítače, odesílání nevyžádané pošty nebo skenování otevřené porty a jinými zařízeními v hello Internetu. Použitím machine learning toonetwork provoz, Security Center může rozpoznat, kdy se odchozí síťovou komunikaci překročit hello norm. Při zasílání nevyžádané pošty, Security Center také korelaci provoz neobvyklou e-mailu s intelligence z Office 365 toodetermine, zda hello e-mailu je pravděpodobné, nefarious nebo výsledek hello legitimní e-mailové kampaně.

### <a name="anomaly-detection"></a>Detekce anomálií

Azure Security Center používá také hrozeb tooidentify detekce anomálií. V analýzu toobehavioral kontrast (což závisí na známé vzorce, které jsou odvozené z velkých datových sad) detekce anomálií je více "individuální" a se zaměřuje na standardních hodnot, které jsou specifické tooyour nasazení. Machine learning je použité toodetermine normální aktivity nasazení a potom pravidla jsou generované toodefine outlier podmínky, které by mohl představovat událostí zabezpečení. Zde naleznete příklad:

-   **Příchozí připojení RDP/SSH útoky hrubou silou:** vaše nasazení může mít zaneprázdněn virtuálních počítačů s mnoha přihlášení každý den a dalších virtuálních počítačů, které mají několik nebo všechny přihlášení. Azure Security Center můžete určit činnost přihlášení standardních hodnot pro tyto virtuální počítače a použít machine learning toodefine kolem hello normální přihlašovací aktivity. Pokud je rozdíl oproti definovaný pro základní hello související s přihlášením charakteristiky, pak může být vygenerována výstraha. A strojové učení tu zase určuje, co je významné.

### <a name="continuous-threat-intelligence-monitoring"></a>Analýzou hrozeb nepřetržité monitorování

Azure Security Center funguje s zabezpečení dat vědecké účely týmy pro výzkum a v rámci hello, world které neustále monitorovat změny v šířku threat hello. To zahrnuje následující iniciativy hello:

-   **Monitorování intelligence Threat:** Threat intelligence zahrnuje mechanismy, ukazatelů, dopad a řešitelné Rady o existující nebo vznikající hrozby. Tyto informace jsou sdílena v komunitě hello zabezpečení a Microsoft neustále monitoruje threat intelligence informační kanály z interních a externích zdrojů.

-   **Signál sdílení:** přehled o zabezpečení týmy ve společnosti Microsoft široké portfolio cloudové a místní služby, serverů a klientů endpoint zařízení jsou sdíleny a analyzovat.

-   **Zabezpečení odborníky společnosti Microsoft:** probíhající zapojení týmy ve Microsoft, které fungují v polích specializované zabezpečení, jako je forenzních a webové detekce útoku.

-   **Detekce ladění:** algoritmy, které se spouští nad skutečné zákazníka datových sad a výzkumných pracovníků zabezpečení pracovat s výsledky hello toovalidate zákazníků. Hodnota TRUE a false pozitivních jsou algoritmů strojového učení použité toorefine.

Tyto společného úsilí případech v nových a vylepšených detekce, které můžete využívat výhod okamžitě – neexistuje žádná akce pro tootake je.

## <a name="advanced-threat-detection-features---other-azure-services"></a>Funkce zjišťování Advanced Threat – jinými službami Azure

### <a name="virtual-machine-microsoft-antimalware"></a>Virtuálního počítače: Antimalware od Microsoftu

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) pro Azure je řešení jednoho agenta pro aplikace a klienta prostředí, určené toorun hello pozadí bez lidského zásahu. Můžete nasadit ochranu na základě potřeb hello vašich zatížení aplikací, s buď základní zabezpečení výchozím nebo advanced vlastní konfigurace, včetně monitorování proti malwaru. Azure antimalwarových je možnost zabezpečení pro virtuální počítače Azure a je automaticky nainstalován na všech virtuálních počítačích Azure PaaS.

**Funkce Azure toodeploy a povolit Antimalware od Microsoftu pro vaše aplikace**

#### <a name="microsoft-antimalware-core-features"></a>Microsoft Antimalware klíčových funkcí

-   **Ochrana v reálném čase -** monitoruje aktivity v cloudové služby a na spouštění virtuálních počítačů toodetect a blokování malwaru.

-   **Naplánované prohledávání -** pravidelně provádí cílové prohledávání malwaru toodetect, včetně aktivně spouštění programů.

-   **Malwarové nápravy -** automaticky provádět akce se zjištěným malwarem, jako je například odstranit nebo umístění do karantény škodlivé soubory a čištění položky škodlivé registru.

-   **Aktualizace podpisu -** automaticky nainstaluje hello nejnovější ochrany podpisy (definice virů) tooensure ochrany je aktuální na předem určené frekvenci.

-   **Aktualizace antimalwarový stroj –** automaticky aktualizace hello modul Antimalware od Microsoftu.

-   **Aktualizace platformy antimalwarových –** automaticky aktualizace hello platformy Antimalware od Microsoftu.

-   **Aktivní ochranu -** sestavy telemetrie metadata o zjištěných hrozeb a podezřelé prostředky tooMicrosoft Azure tooensure rychlé odpovědi toohello vyvíjejí šířku hrozeb a povolení v reálném čase synchronní podpis doručení prostřednictvím Hello Microsoft Active Protection System (MAPS).

-   **Ukázky vytváření sestav -** poskytuje a sestavy ukázky toohelp služba Microsoft Antimalware toohello vylepšit hello služby a řešení potíží s povolit.

-   **Vyloučení –** umožňuje aplikace a služby správci tooconfigure určitých souborů a procesy a jednotky tooexclude je z ochrany a prohledávání pro výkon nebo z jiných důvodů.

-   **Shromažďování událostí antimalwarových -** zaznamenává stav antimalwarové služby hello podezřelé aktivity a nápravné akce prováděné v protokolu událostí hello operačního systému a shromažďuje je do hello zákazníkův účet úložiště Azure.

### <a name="azure-sql-database-threat-detection"></a>Detekce hrozeb databáze Azure SQL

[Služba detekce hrozeb databáze Azure SQL](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) je nová funkce zabezpečení intelligence součástí hello služby Azure SQL Database. Obcházet hello taktovací toolearn, profil a zjišťovat nezvyklé databázové aktivity, databáze SQL Azure služba detekce hrozeb identifikuje databáze toohello potenciální hrozby.

Osoby zabezpečení nebo jiné určený správce můžete získat okamžité odeslání oznámení o aktivitách podezřelé databáze, kdy k nim dojde. Každý oznámení poskytuje podrobné informace o hello podezřelé aktivity a doporučuje jak toofurther prozkoumat a zmírnit hrozby hello.

Detekce hrozeb databáze SQL Azure v současné době zjistí potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL a vzory přístupu k databázi neobvyklé.

Po přijetí e-mailové oznámení detekce hrozeb, jsou uživatelé moct toonavigate a zobrazení hello příslušné záznamy auditu pomocí hello přímý odkaz v e-mailu hello, které se otevře audit prohlížeč nebo předkonfigurované auditování šablony aplikace Excel, která uvádí relevantní auditu hello zaznamenává době hello hello podezřelé události podle toohello následující:
-   Úložiště auditu pro hello databázi nebo server s hello nezvyklé databázové aktivity

-   Tabulka úložiště relevantní auditu, která byl použit v době hello protokolu auditu toowrite hello událostí

-   Audit záznamy hello následujících hodin vzhledem k tomu, že dojde k události hello.

-   Záznamy auditu s podobnou ID události v době hello hello události (pro některé detektory volitelné)

Detektory Threat databáze SQL použijte jednu z následujících metod detekce hello:

-   **Deterministickou detekci –** zjistí podezřelou vzory (na základě pravidel) v dotazech hello SQL klienta, které odpovídají známé útoky. Tato metoda má vysokou detekce a nízkou falešně pozitivní, ale omezené pokrytí, protože spadají do kategorie hello "atomic detekce".

-   **Chování detekce –** vad neobvyklé aktivity, která je neobvyklé chování pro hello databázi, která nebyla během hello posledních 30 dnů.  Příklad SQL klienta neobvyklé aktivity může být Špička neúspěšných přihlášení nebo dotazy, velký objem dat, které se extrahují, neobvyklé kanonický dotazy a neznámého IP adresy používané tooaccess hello databáze

### <a name="application-gateway-web-application-firewall"></a>Brány Firewall webových aplikací Application Gateway

[Web Application Firewall](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall) je funkce [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) , poskytuje ochranu tooweb aplikace, které používají aplikační brány pro standardní [řízení doručení aplikace](https://kemptechnologies.com/in/application-delivery-controllers) funkce. Brány firewall webových aplikací dosahuje tím, že je pro většinu hello chrání [OWASP top 10 známých chyb zabezpečení webové](https://www.owasp.org/index.php/Top_10_2010-Main)

![Firewally aplikace brány webové aplikace](./media/azure-threat-detection/azure-threat-detection-fig13.png)

-   Ochrana před útoky prostřednictvím injektáže SQL.

-   Ochrana před skriptováním mezi weby.

-   Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.

-   Ochrana před narušením protokolu HTTP.

-   Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.

-   Ochrana před roboty, prohledávacími moduly a skenery.

-   Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)

Konfigurace firewall webových aplikací na Application Gateway poskytuje následující výhody tooyou hello:

-   Chraňte před ohrožení zabezpečení webové a útoky bez úpravy kódu toobackend webové aplikace.

-   Ochrana více webových aplikací na hello stejný čas za služby application gateway. Aplikační brána podporuje, hostování až too20 weby za jednu bránu, která by mohla všechny chráněné proti útokům na web.

-   Můžete monitorovat útoky na své webové aplikace pomocí sestavy vygenerované v reálném čase z protokolů WAF služby Application Gateway.

-   Některé ovládací prvky dodržování předpisů vyžadují všechny internetové koncové body toobe chráněn řešení firewall webových aplikací. Používáním služby Application Gateway s povoleným WAF můžete splnit tyto požadavky dodržování předpisů.

### <a name="anomaly-detection--an-api-built-with-azure-machine-learning"></a>Detekce anomálií – rozhraní API vytvořené s nástroji Azure Machine Learning

Detekce anomálií je rozhraní API vytvořené s nástroji Azure Machine Learning, které jsou užitečné pro různé typy neobvyklé vzory zjišťování v datové řady čas. Hello API přiřadí anomálií skóre tooeach datového bodu v řadě hello čas, který můžete použít pro generování výstrahy, monitorování pomocí řídicích panelů nebo připojení se všemi systémy lístků.

Hello [API detekce anomálií](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) může zjistit hello následující typy anomálií na data řady čas:

-   **Špičky a klesne:** například při monitorování hello počtu služby tooa selhání přihlášení nebo počet rezervace web elektronického obchodu,, neobvyklé špičky vyhrazené IP adresy může znamenat útoky na zabezpečení nebo přerušení služeb.

-   **Kladné a záporné trendy:** při monitorování využití paměti v oblasti výpočetních, například zmenšit velikost volné paměti je naznačuje výslednou potenciální nevrácená paměť systému; při monitorování délka fronty služby, může znamenat trvalé vzestupný trend základní problém softwaru.

-   **Úroveň změny a změny v dynamické rozsahu hodnot:** například úroveň změn v latenci služby po upgradu služby nebo po upgradu může být zajímavé toomonitor nižší úrovně výjimek.

Hello machine learning na základě rozhraní API umožňuje:

-   **Flexibilní a robustní detekce:** modely detekce anomálií hello uživatelům povolit nastavení tooconfigure velkých a malých písmen a zjišťovat anomálie mezi sezónní a sezónní datovými sadami. Uživatele můžete upravit menší hello anomálií detekce modelu toomake hello zjišťování rozhraní API nebo citlivější podle tootheir potřebuje. To by znamenalo, zjišťování hello menší nebo viditelné anomálie v datech a bez sezónní vzory.

-   **Škálovatelná a včas detekce:** hello tradiční způsob sledování s přítomen prahové hodnoty nastavit znalostmi odborníků domény jsou nákladná a není škálovatelné toomillions dynamické změny datových sad. modely detekce anomálií Hello v tomto rozhraní API se naučili a modely jsou automaticky přizpůsobená z dat historická i v reálném čase.

-   **Proaktivní a možné použít zjišťování:** pomalé trendu a detekce úrovně změn je možné použít pro včasné detekce anomálií. časná neobvyklé signály Hello zjistil lze použít toodirect člověka tooinvestigate a fungovat na hello problémových oblastí.  Kromě toho příčina analysis modely a výstrah nástroje mohou být vytvořeny na základě této detekce anomálií rozhraní API služby.

detekce anomálií Hello rozhraní API je efektivní a efektivní řešení pro širokou škálu scénářů, jako je stav služby & klíčového ukazatele výkonu monitorování, IoT, monitorování výkonu a sledování síťových přenosů. Tady je několik oblíbených scénářů, kde může být užitečné toto rozhraní API:
- IT oddělení potřebují nástroje tootrack události, kód chyby, protokolu využití a výkonu (procesoru, paměti a tak dále) včas.

-   Online obchodu lokalit mají tootrack zákazníka aktivity, zobrazení stránek, kliknutí a tak dále.

-   Nástroj společností má spotřeba tootrack horních, plynu, elektřiny a dalším prostředkům.

-   Služby pro zařízení nebo vytváření má toomonitor teploty, vlhkosti, provozu a tak dále.

-   IoT/výrobců má toouse dat snímačů v časové řady toomonitor pracovní postup, kvality a tak dále.

-   Poskytovatelé služeb, jako jsou volání centrech musí toomonitor služby vyžádání trendu, objem incidentů, počkejte, než délka fronty a tak dále.

-   Obchodní analýza skupiny mají toomonitor obchodní klíčové ukazatele výkonu. (například prodejní svazek chráněny zákazníka, ceny) neobvyklé pohyb v reálném čase.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) je zásadní součástí hello Microsoft Cloud Security zásobníku. Je komplexní řešení, které vaše organizace může pomoci, jak přesunout tootake všech výhod hello potenciálu cloudových aplikací, ale mějte řízení prostřednictvím lepší přehled o aktivity. Také pomáhá zvýšit hello ochranu důležitých dat napříč cloudovými aplikacemi.

S nástroji, které pomáhají možnost odhalit stínové IT, vyhodnocování rizik, vynucování zásad, prozkoumat aktivit a zastavení hrozeb může vaše organizace více bezpečně přesunout toohello cloudu při zachování kontroly nad důležitými daty.

<table style="width:100%">
 <tr>
   <td>Informace</td>
   <td>Možnost odhalit stínové IT s Cloud App Security. Získejte potřebný přehled díky zjišťování aplikací, aktivit, uživatelů, dat a souborů ve vašem cloudovém prostředí. Zjistit aplikacím třetích stran, které jsou připojené tooyour cloudu.</td>
 </tr>
 <tr>
   <td>Prozkoumat</td>
   <td>Prozkoumejte cloudových aplikací pomocí nástroje pro forenzní cloudu toodeep informace do rizikových aplikací, konkrétní uživatele a soubory v síti. Najít vzorů v datech hello shromážděných z vašeho cloudu. Generování sestav toomonitor vašeho cloudu.</td>

 </tr>
 <tr>
   <td>Ovládací prvek</td>
   <td>Zmírnění rizik pomocí nastavení zásad a výstrahy tooachieve maximální kontroly nad síťovými přenosy v cloudu. Pomocí Cloud App Security toomigrate uživatelé toosafe, alternativy schválené cloudové aplikace.</td>

 </tr>
 <tr>
   <td>Ochrana</td>
   <td>Pomocí Cloud App Security toosanction nebo aplikacím, vynucovat ochranu před únikem dat, řízení oprávnění a sdílení a generovat vlastní sestavy a výstrahy.</td>

 </tr>
 <tr>
   <td>Ovládací prvek</td>
   <td>Zmírnění rizik pomocí nastavení zásad a výstrahy tooachieve maximální kontroly nad síťovými přenosy v cloudu. Pomocí Cloud App Security toomigrate uživatelé toosafe, alternativy schválené cloudové aplikace.</td>

 </tr>
</table>


![Cloud App Security](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security integruje s vaším cloudovým podle viditelnost

-   Pomocí Cloud Discovery toomap a identifikovat vašeho cloudového prostředí a hello cloudových aplikací vaše organizace používá.


-   Schvalování a zákazu aplikací ve vašem cloudu.



-   Použití konektorů snadno nasadit aplikace, které využívají poskytovatele rozhraní API, viditelnost a zásady správného řízení aplikací, které se připojují k.

-   Vám pomáhá průběžné řízení podle nastavení a pak průběžně doladění, zásady.

Cloud App Security na shromažďování dat z těchto zdrojů, spustí pokročilé analýzy na hello data. Ji okamžitě upozorňuje tooanomalous aktivity a vám podrobně je prozkoumat do vašeho cloudového prostředí. Můžete nakonfigurovat zásadu v Cloud App Security a použít ho tooprotect všechno, co ve vašem cloudovém prostředí.

## <a name="third-party-atd-capabilities-through-azure-marketplace"></a>Možnosti ATD třetích stran prostřednictvím Azure Marketplace

### <a name="web-application-firewall"></a>Web Application Firewall (Brána firewall webových aplikací)

Brány Firewall webových aplikací kontroluje příchozí webové přenosy a bloky vložení SQL skriptování mezi servery, malwaru nahrávání & aplikace DDoS a jiným útokům zaměřený na vaše webové aplikace. Je také zkontroluje, zda obsahuje hello odpovědí z hello back endové webové servery pro prevenci ztráty dat (DLP). stroj řízení Hello integrované přístup umožňuje zásady řízení granulární přístupu toocreate správci pro ověřování, autorizace a monitorování (AAA), která umožňuje organizacím silné ověřování a uživatelského ovládacího prvku.

**Označuje:**
-   Zjišťuje a blokuje SQL injekce, skriptování mezi, nahrávání malware, aplikace DDoS nebo jiným útokům proti vaší aplikace.

-   Ověřování a řízení přístupu.

-   Vyhledá odchozí přenosy toodetect citlivá data a maskovat nebo blokovat hello informace z úniku.

-   Zrychluje hello doručování obsahu webové aplikace, pomocí funkcí, jako je ukládání do mezipaměti, kompresi a další optimalizace provoz.

Příklad brány firewall webových aplikací v Azure Marketplace k dispozici jsou následující:

[Brány Firewall webových aplikací barracuda, Brocade virtuální brány Firewall webových aplikací (Brocade vWAF), SecureSphere společnosti Imperva a hello ThreatSTOP IP brány Firewall.](https://azure.microsoft.com/marketplace/partners/brocade_communications/brocade-virtual-web-application-firewall-templatevtmcluster/)

## <a name="next-steps"></a>Další kroky

- [Možnosti detekce v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)

Funkce Rozšířené zjišťování Azure Security Center pomáhá tooidentify active hrozeb cílení na vaše prostředky Microsoft Azure a zajišťuje, že jste se statistickými hello toorespond rychle.

- [Detekce hrozeb databáze Azure SQL](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/)

Služba detekce hrozeb databáze Azure SQL pomohly vyřešit jejich pochybnostmi databáze tootheir potenciální hrozby.
