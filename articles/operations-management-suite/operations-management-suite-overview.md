---
title: "aaaOperations Management Suite (OMS) – přehled | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.  Tento článek popisuje hello hodnotu OMS, identifikuje hello různými službami a nabídky součástí OMS a poskytuje odkazy tootheir podrobné obsah."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.openlocfilehash: ec3fe6d82aec46d1f715a4338f126e79e04a9147
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Co je Operations Management Suite (OMS)?
Tento článek obsahuje úvod tooOperations Management Suite (OMS) včetně stručný přehled hello firmy hodnota poskytuje, služby a správu řešení hello obsahuje a hello nabídky, které balíčku společně různé služby a řešení.  Odkazy jsou zahrnuty toohello podrobná dokumentace na nasazení a používání jednotlivé služby a řešení.

## <a name="from-on-premises-toohello-cloud"></a>Z místní toohello cloudu
Microsoft už dlouho poskytuje produkty pro správu podnikových prostředí.  Více produktů se konsolidují do Sada System Center hello produkty pro správu ve verzi 2007.  To zahrnuté nástroje Configuration Manager, která poskytuje funkce, jako distribuce softwaru a o inventáři, Operations Manager, který poskytuje Proaktivní monitorování systémů a aplikací, Orchestrator, včetně sady runbook tooautomate manuálních procesů a Data Protection Manager pro zálohování a obnovení kritických dat.

Produkty System Center s další výpočetní prostředky přesunutí toohello cloudu, získávají další funkce cloudu jako Operations Manager a Orchestrator správu prostředků v Azure.  V podstatě ale byly navržené jako místní řešení a vyžadují značné investice při nasazování a údržbě místního prostředí pro správu.  toocompletely využít hello cloudu a podporovat budoucí aplikace, nové toomanagement přístup nebyla nutná.

## <a name="introducing-operations-management-suite"></a>Představujeme Operations Management Suite
Služby Operations Management Suite (OMS) je kolekce služeb pro správu, které byly navrženy v cloudu hello od začátku hello.  Namísto nasazení a správy místních prostředků jsou komponenty OMS výhradně hostované v Azure.  Konfigurace je minimální a během několika minut můžete začít pracovat.  

- **Minimální náklady a složitost nasazení.**  Všechny součásti hello a dat pro OMS jsou uloženy v Azure, může být až a spuštěna v krátkém čase bez hello složitost a investice do místní součásti.
- **Škálování toocloud úrovně.**  Není k dispozici tooworry o platícího pro výpočetní prostředky, které nepotřebujete nebo o nedostatku místa úložiště od hello cloudu vám umožní toopay pouze pro co skutečně používáte a bude snadno škálovat tooany zatížení, které vyžadujete.  Můžete začít spravovat několik prostředků tooget spuštění a pak škálovat tooyour celé prostředí.
- **Výhodou hello nejnovější funkce.**  Funkce ve službách OMS se průběžně přidávají a aktualizují.  Máte neustále získat přístup k nejnovější funkcím toohello bez jakékoli aktualizace toodeploy požadavek.
- **Integrované služby.**  Při každé služby OMS hello zadejte významné hodnotu na své vlastní, mohou společně pracovat toosolve komplexní správu scénáře.  Sady runbook ve službě Azure Automation může například jednotky proces převzetí služeb při selhání s Azure Site Recovery a potom se přihlaste informace tooLog Analytics toogenerate výstrahu.
- **Globální znalosti.**  Řešení pro správu v OMS nepřetržitě měli přístup toohello nejnovější informace.  Hello řešení zabezpečení a auditování můžete například proveďte analýzu hrozeb pomocí nejnovější hrozbám hello detekovaly kolem hello, world.
- **Přístup odkudkoli.**  Přistupujte ke svému prostředí pro správu odkudkoli, kde máte prohlížeč.  Na vašem smartphonu pro data monitorování tooyour připravené pro přístup k instalaci aplikace OMS hello.

### <a name="is-it-just-for-hello-cloud"></a>Je jenom pro hello cloud?
Právě vzhledem k tomu OMS služby běží cloudu hello neznamená, že nelze spravují efektivně v místním prostředí.  Uvedené agenta na všechny Windows nebo počítač se systémem Linux v datovém centru a odešle data tooLog analýzy, kde lze analyzovat společně s všechna ostatní data shromážděná z cloudu nebo na místní služby.  Pro zálohování a vysoké dostupnosti pro místních prostředků pomocí Azure Backup a Azure Site Recovery tooleverage hello cloudu.  
Sady Runbook v cloudu hello nelze obvykle přístup k prostředkům místně, ale můžete nainstalovat agenta na jeden nebo více počítačů příliš, který bude hostitelem sady runbook ve vašem datovém centru.  Při spuštění sady runbook, jednoduše zadejte, zda se má toorun v hello cloudu nebo na místní pracovní.

## <a name="hybrid-management-with-system-center"></a>Hybridní správa s nástrojem System Center
Pokud máte existující instalace nástroje System Center, můžete integrovat tyto součásti OMS služby tooprovide hybridní řešení pro obě místní a Cloudová prostředí využití hello relativní specializací každého produktu.  Připojte vaše stávající nástroje Operations Manager skupiny tooLog Analytics tooanalyze spravované agenti pro správu v cloudu hello.  Použijte existující procesu zálohování s toobackup Data Protection Manager vaší toohello dat v cloudu.  


## <a name="oms-services"></a>Služby OMS
Hello základní funkce služby OMS poskytuje sadu služby, které běží v Azure.  Každá služba poskytuje funkce správy specifických a můžete kombinovat scénářů tooachieve různých správy služeb.

|| Služba | Popis |
|:--|:--|:--|
| ![Log Analytics](media/operations-management-suite-overview/icon-log-analytics.png) | Log Analytics | Monitorovat a analyzovat hello dostupnosti a výkonu různých prostředků, včetně fyzických a virtuálních počítačů. |
| ![Azure Automation](media/operations-management-suite-overview/icon-automation.png) | Automation | Automatizace ručních procesů a vynucení konfigurací pro fyzické a virtuální počítače |
| ![Azure Backup](media/operations-management-suite-overview/icon-backup.png) | Zálohování | Zálohování a obnovení důležitých dat |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Poskytnutí vysoké dostupnosti pro důležitá data |

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) poskytuje služby monitorování pro OMS získáváním dat ze spravovaných prostředků do centrálního úložiště.  Tato data můžou zahrnovat události, údaje o výkonu nebo vlastní data poskytnutá prostřednictvím hello rozhraní API. Jakmile získány, je k dispozici pro výstrahy, analýzu a export dat hello.  Tato metoda umožňuje tooconsolidate data z různých zdrojů, můžete kombinovat data ze služeb Azure s vaší stávající místní prostředí.  Také jasně odděluje hello shromažďování dat hello od hello akce prováděné na tato data tak, aby všechny akce jsou k dispozici tooall druhy dat.  

![Přehled služby Log Analytics](media/operations-management-suite-overview/overview-log-analytics.png)

#### <a name="collecting-data"></a>Shromažďování dat
Existuje mnoho různých způsobů, jak můžete získat data do úložiště analýzy protokolů tooanalyze hello.

- **Virtuální počítače a počítače s Windows nebo Linuxem.**  Instalace agenta Microsoft Monitoring Agent hello na [Windows](../log-analytics/log-analytics-windows-agents.md) a [Linux](../log-analytics/log-analytics-linux-agents.md) nebo virtuálních počítačích, které chcete toocollect data z.  Hello agent automaticky stáhne z konfigurace analýzy protokolů, který definuje události a toocollect data výkonu.  Hello agenta můžete snadno nainstalovat na virtuální počítače běžící v Azure pomocí hello portálu Azure.  Pokud máte stávající prostředí nástroje Operations Manager, můžete připojit hello správy skupiny tooLog analýzy a automaticky spustí shromažďování dat od všechny existující agentů.
- **Služby Azure.**  Analýzy protokolů shromažďuje telemetrická data z [diagnostiky Azure a Azure Monitoring](../log-analytics/log-analytics-azure-storage.md) do úložiště hello tak, abyste je mohli monitorovat prostředků Azure.
- **Rozhraní API kolekce dat.**  Log Analytics má rozhraní [REST API pro naplňování dat z libovolného klienta](../log-analytics/log-analytics-data-collector-api.md).  To vám umožní toocollect data z aplikací třetí strany nebo jsou implementovány vlastní správu scénáře.  Běžnou metodou je toouse sady runbook v Azure Automation toocollect data a potom pomocí rozhraní API sady kolekcí dat toowrite hello ho toohello úložiště.

#### <a name="reporting-and-analyzing-data"></a>Vytváření sestav a analýza dat
Analýzy protokolů zahrnuje účinný dotazovací jazyk tooextract data uložená v úložišti hello.  Protože jsou data ze všech zdrojů uložená jako záznamy, můžete analyzovat data z více zdrojů v jediném dotazu.
  
Kromě toho tooad hoc analýzy, analýzy protokolů poskytuje několik způsobů tooreport a analyzovat data z dotazu.

- **Zobrazení a řídicí panely.**  [Zobrazení](../log-analytics/log-analytics-view-designer.md) a [řídicí panely](../log-analytics/log-analytics-dashboards.md) vizualizace hello výsledků dotazu hello portálu.  Řešení pro správu by měl obvykle zahrnovat zobrazení, která analyzovat data hello z hello řešení.  Můžete také vytvořit vlastní zobrazení dat tooanalyze a nastavte jej na portálu vlastní snadno dostupné.
- **Export.**  Máte možnost hello tooexport hello výsledky jakýkoli dotaz tak, aby je bylo možné analyzovat mimo analýzy protokolů.  Můžete také naplánovat pravidelné export příliš[Power BI](../log-analytics/log-analytics-powerbi.md) který poskytuje vizualizaci a analýzu schopnosti.
- **Rozhraní API pro prohledávání protokolů.**  Log Analytics má rozhraní [REST API pro shromažďování dat z libovolného klienta](../log-analytics/log-analytics-log-search-api.md).  To vám umožní tooprogrammatically práci s daty shromážděnými v úložišti hello nebo získat přístup pomocí jiného nástroje pro monitorování.

#### <a name="alerting"></a>Zobrazení výstrah
Log Analytics vám může [proaktivně zobrazovat výstrahy](../log-analytics/log-analytics-alerts.md) nebo provádět nápravné akce, když zjistí problém.  Stejně jako všechny ostatní analýzy v Log Analytics se to provádí na základě prohledávání protokolu.  Toto hledání spouští v pravidelných intervalech a vytváří výstrahy, pokud výsledky hello odpovídají konkrétním kritériím.

![Výstrahy Log Analytics](media/operations-management-suite-overview/overview-alerts.png)

Kromě toho toocreating záznam výstrah v úložišti analýzy protokolů hello výstrahy může trvat hello následující akce.

- **E-mail.**  E-mailovou zprávu tooproactively oznámíme vám zjištěný problém.
- **Runbook.**  Výstraha v Log Analytics může spustit runbook v Azure Automation.  To se obvykle provádí tooattempt toocorrect hello zjistil problém.  Hello runbook lze spustit v cloudu hello v hello mohlo být spuštěno případ problém v Azure nebo jiném cloudu nebo ji na místní agent pro problém na fyzický nebo virtuální počítač.
- **Webhook.**  Výstrahu můžete spustit webhook, jehož a předávání dat z hello výsledky hledání protokolů hello.  To umožňuje integraci se službou externí například alternativní výstrahy systém, nebo se pokusit tootake opravné akce pro externí webový server.

### <a name="azure-automation"></a>Azure Automation
[Služby Azure Automation](http://azure.microsoft.com/documentation/services/automation) poskytuje tooOMS procesu automatizace a konfigurace správy.  Ho automatizuje manuální procesy a pomáhá tooenforce konfigurace pro fyzické a virtuální počítače.  

#### <a name="process-automation"></a>Automatizace procesů
Azure Automation automatizuje ruční procesy s využitím [runbooků](../automation/automation-runbook-types.md), které jsou založené na skriptu PowerShellu nebo pracovním postupu PowerShellu.  Zahrnuje také prostředky podpora sady runbook, jako je například proměnné, které lze sdílet mezi více sad runbook a přihlašovací údaje a připojení, které vám umožňují toostore šifrované informace, které mohou být požadovány pro sadu runbook pro ověřování.
Sady Runbook nabízejí automatizace procesu pro hello další služby v sadě hello.  Od těchto hello je přístupná dalších služeb v prostředí PowerShell nebo přes REST API, můžete vytvořit runbook tooperform funkce, jako je shromažďování dat správy v analýzy protokolů nebo inicializaci zálohování pomocí zálohování Azure.

##### <a name="accessing-resources"></a>Přístup k prostředkům
Vzhledem k tomu, že jsou runbooky založené na PowerShellu, můžou spravovat všechny prostředky, ke kterým jde přistupovat pomocí rutin PowerShellu.  Pokud jste [načíst modul](../automation/automation-integration-modules.md) do vašeho účtu Automation bude k dispozici tooall sady runbook v daném účtu. 
 
Při spuštění sady runbook v cloudu hello přístupem všechny prostředky, které jsou přístupné z cloudu hello.  To můžou být prostředky v předplatném Azure, v jiném cloudu, jako je AWS (Amazon Web Services), nebo ve službě přístupné prostřednictvím rozhraní REST API.  Sady Runbook v cloudu hello nespouštět pod všechny přihlašovací údaje, ale jejich můžete využít prostředky Automation jako je například přihlašovací údaje, připojení a tooresources tooauthenticate certifikáty, ke kterým mají přístup.

Prostředky ve vašem datovém centru s největší pravděpodobností nelze přistupovat ze sady runbook spuštěna v cloudu hello.  Můžete nainstalovat jednu nebo více [procesy Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) ve vašich datech center ale toorun sady runbook, které vyžadují přístup k prostředkům toolocal.  Při spuštění sady runbook, zadejte, zda má být spuštěn v hello cloudu nebo na konkrétní pracovní.

![Runbooky Azure Automation](media/operations-management-suite-overview/overview-runbooks.png)

##### <a name="starting-a-runbook"></a>Spuštění runbooku
Runbooky je možné [spustit několika způsoby](../automation/automation-starting-a-runbook.md), takže je jde zahrnout do široké škály scénářů správy.  

- **Azure Portal.**  Jako další služby Azure Azure Automation jde spravovat z hello portálu Azure.  Kromě toho toostarting sady runbook, můžete je importovat nebo vytvářet vlastní.
- **Naplánované.**  Toostart sady runbook můžete naplánovat v pravidelných intervalech.  To vám umožní tooautomatically opakovat proces regulární správy nebo shromažďovat data tooLog Analytics.
- **PowerShell a rozhraní API.**  Můžete spustit sady runbook a předejte jí je požadované informace o parametrech z rutiny prostředí PowerShell nebo hello REST API služby Azure Automation.  
- **Webhook.**  Webhook, jehož lze vytvořit pro jakoukoli sadu runbook, který umožní toobe spuštěné z externí aplikace nebo webové stránky.
- **Výstraha Log Analytics.**  Výstrahu v analýzy protokolů můžete automaticky spustit runbook tooattempt toocorrect hello problém s identifikovaný hello výstrahy.

#### <a name="configuration-management"></a>Správa konfigurace
[Prostředí PowerShell požadovaného stavu konfigurace (DSC)](../automation/automation-dsc-overview.md) je platforma pro správu v prostředí Windows PowerShell, který vám umožní toodeploy a vynutit hello konfigurace fyzických a virtuálních počítačů.  Služby Azure Automation spravuje konfigurace DSC a poskytuje na vyžádání obsahu server v cloudu hello agentů k přístup tooretrieve požadované konfigurace.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

### <a name="azure-backup-and-azure-site-recovery"></a>Azure Backup a Azure Site Recovery
Azure Backup a Azure Site Recovery přispívat toobusiness kontinuitu a zotavení po havárii.  Oba mají funkce, které vám pomohou tooensure aplikace zůstávají dostupné při výpadku dojde k a vracet toonormal operations systémy dostane zpět online.  Obě služby přispívat toohello plánovaných bodů obnovení (rpo) a cíli doby obnovení (RTO) definované pro vaši organizaci. Vaše plánovaný bod obnovení definuje hello přijatelný limit, ve kterém data nejsou k dispozici při výpadku a hello RTO omezuje hello přijatelné množství času, ve kterém služba nebo aplikace není k dispozici při výpadku.

#### <a name="azure-backup"></a>Azure Backup
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) poskytuje služby zálohování a obnovení dat pro OMS.  Chrání data vaší aplikace a dlouhá léta je uchovává bez nutnosti velkých investic a s minimálními provozními náklady.  Ho můžete zálohovat data ze fyzickými a virtuálními Windows serverech v přidání tooapplication úlohy, jako například SQL Server a SharePoint.  Ho lze také pomocí System Center Data Protection Manager (DPM) tooreplicate chráněná data tooAzure redundanci a dlouhodobého uložení.

Chráněná data ve službě Azure Backup se ukládají do trezoru záloh umístěného v konkrétní geografické oblasti. Hello data se replikují uvnitř hello stejné oblasti a v závislosti na typu hello trezoru, může být také replikované tooanother oblast pro další odolnost proti chybám.

Azure Backup obsahuje tři základní scénáře.

- **Počítač s Windows a agentem služby Azure Backup.** Zálohovat soubory a složky v jakémkoli systému Windows server nebo klienta přímo tooyour trezor služby Azure backup.<br><br>![Počítač s Windows a agentem služby Azure Backup](media/operations-management-suite-overview/overview-backup-01.png)
- **Server System Center Data Protection Manageru (DPM) nebo Microsoft Azure Backup Server.** Využívejte v aplikaci DPM nebo Microsoft Azure Backup Server toobackup soubory a složky v přidání tooapplication úlohy, jako například toolocal úložiště SQL a službu SharePoint a pak replikovat tooyour trezor služby Azure backup. Podporuje virtuální počítače s Windows a Linuxem na technologii Hyper-V nebo VMware.<br><br>![Server System Center Data Protection Manageru (DPM) nebo Microsoft Azure Backup Server](media/operations-management-suite-overview/overview-backup-02.png)
- **Rozšíření virtuálního počítače Azure.** Zálohování systému Windows nebo Linux virtuálního počítače v Azure tooyour trezor služby Azure backup.<br><br>![Rozšíření virtuálního počítače Azure](media/operations-management-suite-overview/overview-backup-03.png)



#### <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) poskytuje provozní kontinuitu tím, že orchestruje replikaci místní virtuální a fyzické počítače tooAzure nebo tooa sekundární lokality. Pokud primární lokalita není k dispozici, můžete převzít toohello sekundárního umístění, aby uživatelé mohli zachovat pracovní a nesplní zpět, když systémy vrátit tooworking pořadí. 

Azure Site Recovery nabízí vysokou dostupnost pro servery a aplikace.  Přispívá tooyour provozní kontinuitu a strategie po havárii (BCDR) obnovení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V místní virtuální počítače VMware a fyzické servery Windows nebo Linuxem. Můžete replikovat počítače tooa sekundárního datového centra nebo rozšířit replikaci je tooAzure datového centra. Site Recovery také poskytuje možnosti jednoduchého převzetí služeb při selhání úloh a jejich obnovení. Integruje se s mechanismy zotavení po havárii, jako je SQL Server AlwaysOn, a poskytuje plány obnovení pro snadné převzetí služeb při selhání úloh vrstvených napříč více počítači.

Azure Site Recovery obsahuje tři základní scénáře replikace.

- **Replikace virtuálních počítačů Hyper-V.**  Pokud jsou virtuální počítače Hyper-V spravované v cloudech VMM, můžete replikovat tooa sekundárního datového centra nebo tooAzure úložiště. TooAzure replikace je přes zabezpečené připojení k Internetu. Replikace tooa sekundárního datového centra je nad hello LAN.  Pokud virtuální počítače Hyper-V nejsou spravované přes VMM, můžete replikovat jenom tooAzure úložiště. TooAzure replikace je přes zabezpečené připojení k Internetu.<br><br>![Replikace virtuálních počítačů Hyper-V](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **Replikace virtuálních počítačů VMware.**  Můžete provádět replikaci VMware virtuální počítače tooa sekundárního datacentra systémem VMware nebo tooAzure úložiště. Replikace tooAzure situace může nastat, přes síť site-to-site VPN nebo Azure ExpressRoute nebo přes zabezpečené internetové připojení. Sekundární datacentrum tooa replikace probíhá přes hello InMage Scout datový kanál.<br><br>![Replikace virtuálních počítačů VMware](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **Replikace fyzických serverů s Windows nebo Linuxem**  Můžete replikovat fyzické servery tooa sekundárního datového centra nebo tooAzure úložiště. Replikace tooAzure situace může nastat, přes síť site-to-site VPN nebo Azure ExpressRoute nebo přes zabezpečené internetové připojení. Sekundární datacentrum tooa replikace probíhá přes hello InMage Scout datový kanál. Azure Site Recovery má OMS řešení, která zobrazuje statistikami, ale hello portálu Azure, musíte použít pro žádné operace.<br><br>![Replikace fyzických serverů s Windows nebo Linuxem](media/operations-management-suite-overview/overview-siterecovery-physical.png)


Site Recovery ukládá metadata do trezorů umístěných v konkrétní oblasti Azure. Žádná replikovaná data jsou uložena ve hello služba Site Recovery.

## <a name="management-solutions"></a>Řešení pro správu
[Řešení pro správu](operations-management-suite-solutions.md) jsou předpřipravené sady logik, které implementují konkrétní scénáře správy s využitím jedné nebo více služeb OMS.  Různá řešení jsou dostupné od Microsoftu a partnerů, můžete snadno přidat tooyour předplatného Azure tooincrease hello hodnotu investice v OMS.  Jako partner můžete vytvořit vlastní řešení toosupport vašim aplikacím a službám a poskytněte toousers prostřednictvím hello Azure Marketplace nebo šablony rychlý start.

Dobrým příkladem řešení, které využívá několik dalších funkcí služby tooprovide je hello [řešení pro správu aktualizací](oms-solution-update-management.md).  Toto řešení používá hello analýzy protokolů agenta pro Windows a Linux toocollect informace o požadovaných aktualizací na každého agenta.  Zapíše úložiště analýzy protokolů toohello tato data kde můžete analyzovat s zahrnuté řídicího panelu.  Když vytvoříte nasazení, runbooky ve službě Azure Automation jsou použité tooinstall požadované aktualizace.  Spravovat tento celý proces hello portálu a není nutné tooworry o základní podrobnosti hello.

![Řešení](media/operations-management-suite-overview/overview-solution.png)

Většina řešení může provést jeden nebo více hello následující funkce.

- Shromažďování dalších informací.  Log Analytics shromažďuje širokou škálu dat z klientů a služeb včetně výkonnostních dat a událostí.  Řešení pro správu může, obvykle s využitím runbooků Azure Automation, shromažďovat další informace, které nejsou dostupné z jiných datových zdrojů.
- Poskytování další analýzy získaných informací.  Řešení pro správu zahrnují řídicí panely a zobrazení, které poskytují analýzu a vizualizaci dat.  Tato odkaz back toopredefined protokolu hledání, které vám umožňují toodrill do hello podrobné data.  Mohou také provádět analýzy na data shromážděná již byla do hello úložiště, například vyhledávání napříč událostí zabezpečení pro vzorů, které ukazují hrozbou.
- Přidávání funkcí.  Některá řešení od společnosti Microsoft může stavějí hello možnosti hello základní služby tooprovide další funkce.  Mapy služeb například nabízí vlastní toodiscover konzoly a mapuje serveru a procesní závislosti v reálném čase.
Řešení jsou pravidelně přidávány tooOMS společností Microsoft a partnery, což vám toocontinuously zvýšit hodnotu hello vašich investic.  Můžete procházet a instalovat řešení společnosti Microsoft prostřednictvím hello řešení katalogu na portálu OMS hello nebo procházet a instalovat řešení společnosti Microsoft a partnery prostřednictvím hello Azure Marketplace v hello portálu Azure.  

![Galerie řešení](media/operations-management-suite-overview/solution-gallery.png)


## <a name="next-steps"></a>Další kroky
* Další informace o [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Další informace o [Azure Automation](../automation/automation-intro.md).
* Další informace o [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Další informace o [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).
* Zjistit hello [řešení, které jsou k dispozici](../log-analytics/log-analytics-add-solutions.md) v různých nabídky OMS hello. 

