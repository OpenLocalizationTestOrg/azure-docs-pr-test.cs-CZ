---
title: "aaaAzure dokumentace k Operations Management Suite (OMS) – kurzy | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur. Tento článek identifikuje hello různé služby, zahrnuté v OMS a poskytuje odkazy tootheir podrobné obsah."
services: operations-management-suite
author: carolz
manager: carolz
layout: LandingPage
ms.assetid: 
ms.service: operations-management-suite
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 01/23/2017
ms.author: carolz
ms.openlocfilehash: 11a8f5ecb3d84aed90554510fc1bb43320fdebb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Co je Operations Management Suite (OMS)?
Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.  Vzhledem k tomu, že je OMS implementována jako cloudová služba, je možné ji zprovoznit velmi rychle a s minimální investicí do služeb infrastruktury.  Nové funkce jsou doručovány pravidelně, což šetří průběžné náklady na údržbu a aktualizace.

Kromě toho tooproviding cenné služeb v jeho vlastní, OMS může integrovat součástí produktu System Center, jako je například tooextend System Center Operations Manager stávající investice správy do cloudu hello.  System Center a OMS vzájemně spolupracují tooprovide úplné hybridní Správa prostředí.

Hello následující části zadejte popis nejvyšší úrovni oblasti jinou hodnotu hello OMS a hello služeb, které je provede.  Architektura tooOMS najdete přehled o různých komponent OMS hello před kontrola hello podrobné dokumentaci pro službu.

## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Statistiky a analýza](media/operations-management-suite-overview/icon-insight-analytics.png) Statistiky a analýza
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) pomáhá shromažďovat, korelovat, vyhledávat a jednat podle dat protokolu a dat o výkonu vygenerovaných operačními systémy a aplikacemi. Nabízí v reálném čase Statistika provozu pomocí integrovaného hledání a vlastní řídicí panely tooreadily analyzovat miliony záznamů napříč všemi servery bez ohledu na jejich fyzické umístění a úloh.

Můžete snadno přidat tooLog řešení analýz, které se definovat toobe dat shromážděných a hello logiku pro jeho analýzu.  Řešení může obsahovat další funkce, která je dodávána automaticky tooagents s minimální nebo žádnou konfiguraci.  Kromě toho nástrojů pro analýzu toousing poskytovaný jednotlivá řešení, můžete nastavit vlastní vyhledávání napříč celou datovou sadu hello v pořadí toocorrelate dat mezi systémy a aplikace.  

## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatizace a řízení](media/operations-management-suite-overview/icon-automation-control.png) Automatizace a řízení
Automatizace Azure umožňuje automatizovat procesy správy s [runbooky](../automation/automation-runbook-types.md) které jsou založené na prostředí PowerShell a spusťte v hello cloudu Azure.  Runbooky mohou přistupovat ke všem produktům a službám, které lze spravovat pomocí prostředí PowerShell, včetně prostředků v dalších cloudech, jako je například Amazon Web Services (AWS).  Sady Runbook lze také provést na serveru v místních prostředků vaše místní data center toomanage.

Azure Automation poskytuje správu konfigurace pomocí [PowerShell DSC](../automation/automation-dsc-overview.md).  Můžete vytvořit a spravovat prostředky DSC hostované v Azure a použít je toocloud a místních systémech toodefine a automaticky vynutit jejich konfigurace.

## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Ochrana a zotavení](media/operations-management-suite-overview/icon-protection-recovery.png) Ochrana a zotavení po havárii
Služba [Azure Backup](http://azure.microsoft.com/documentation/services/backup) chrání data vaší aplikace a dlouhá léta je uchovává bez nutnosti velkých investic a s minimálními provozními náklady.  Ho můžete zálohovat data ze fyzickými a virtuálními Windows serverech v přidání tooapplication úlohy, jako například SQL Server a SharePoint.  Ho lze také pomocí System Center Data Protection Manager (DPM) tooreplicate chráněná data tooAzure redundanci a dlouhodobého uložení.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) přispívá tooyour provozní kontinuitu a strategie po havárii (BCDR) obnovení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V místní, virtuální počítače VMware a fyzické Windows nebo Servery se systémem Linux. Můžete replikovat počítače tooa sekundárního datového centra nebo rozšířit replikaci je tooAzure datového centra. Site Recovery také poskytuje možnosti jednoduchého převzetí služeb při selhání úloh a jejich obnovení. Integruje se s mechanismy zotavení po havárii, jako je SQL Server AlwaysOn, a poskytuje plány obnovení pro snadné převzetí služeb při selhání úloh vrstvených napříč více počítači.

## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![Zabezpečení a dodržování předpisů v OMS](media/operations-management-suite-overview/icon-security-compliance.png) Zabezpečení a dodržování předpisů
Zabezpečení a dodržování předpisů pomáhá identifikovat, hodnocení a zmírnit rizika zabezpečení tooyour infrastruktury.  Tyto funkce OMS jsou implementovány pomocí více řešení v analýzy protokolů, které analyzovat data protokolu a konfiguraci z agenta systémů tooassist v zajistíte hello probíhající zabezpečení vašeho prostředí.

* Hello [řešení zabezpečení a Audit](oms-security-getting-started.md) shromažďuje a analyzuje události zabezpečení na spravované systémy tooidentify podezřelou aktivitu.
* Hello [Antimalwarové řešení](../log-analytics/log-analytics-malware.md) hlásí hello stav ochrany proti malwaru na spravovaných systémech.  
* Hello řešení aktualizací systému provádí analýzu hello bezpečnostní aktualizace a jiné aktualizace spravovaných systémech tak, aby snadno určit systémy, které vyžadují opravy.

## <a name="next-steps"></a>Další kroky
* Další informace o [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Další informace o [Azure Automation](../automation/automation-intro.md).
* Další informace o [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Další informace o [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).

