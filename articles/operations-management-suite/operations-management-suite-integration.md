---
title: aaaIntegrating s Operations Management Suite (OMS) | Microsoft Docs
description: "Kromě toho toousing hello standardní funkce OMS, můžete službu integrovat s další správy aplikací a služeb tooprovide prostředí správy hybridní, tooprovide vlastní správu scénáře jedinečný tooyour prostředí nebo tooprovide vlastní prostředí pro správu pro vaše zákazníky.  Tento článek obsahuje přehled různé možnosti pro integraci s OMS a propojuje tooarticles poskytuje podrobné technické informace."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fc5f3a8a-77f7-4103-bd7e-744c15ffcca7
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: dce752dcdc6c725bbafd49db4a5055750487ecf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-with-operations-management-suite-oms"></a>Integrace s Operations Management Suite (OMS)
Služby Operations Management Suite je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury.  Kromě toho toousing hello standardní funkce OMS, můžete službu integrovat s další správy aplikací a služeb tooprovide prostředí správy hybridní, tooprovide vlastní správu scénáře jedinečný tooyour prostředí nebo tooprovide vlastní prostředí pro správu pro vaše zákazníky.  Tento článek obsahuje přehled různé možnosti pro integraci s OMS služby a odkazy tooarticles poskytuje podrobné technické informace. 

## <a name="log-analytics"></a>Log Analytics
Správa data shromažďovaná společností analýzy protokolů je uložen v úložišti, který je hostován v Azure.  Všechna data uložená v úložišti hello je k dispozici ve vyhledávání protokolu, které poskytují rychlé analýzy napříč velmi velké objemy dat.  Požadavky na integraci může být toopopulate hello úložiště s zpřístupnění pro analýzu, nová data nebo data tooextract v hello úložiště tooprovide novou vizualizaci nebo toointegrate pomocí jiného nástroje pro správu.

Každá položka dat v úložišti hello je uloženo jako záznam.  Při naplňování hello úložiště by měl poskytovat uživatelům s hello záznam typ, který používá vaše řešení a popis jeho vlastnosti.  Při načítání dat, musíte tyto informace o datech hello, kterou právě pracujete s.

![Naplnění hello OMS úložiště](media/operations-management-suite-integration/repository.png)

### <a name="populate-hello-log-analytics-repository"></a>Naplnění hello úložiště analýzy protokolů
Existuje více metod pro sestavování hello OMS úložiště.  Hello metoda, kterou použijete bude záviset na faktorech, například kde se nachází hello zdrojová data, formát hello hello dat, a klienti potřebovat toosupport.  Jakmile data uložena v úložišti hello, nezáleží na tom, jak byl shromážděný.

Hello následující oddíly popisují různé možnosti hello pro sestavování hello OMS úložiště.

#### <a name="connected-sources-and-data-sources"></a>Připojené zdroje a zdroje dat.
Připojené zdroje jsou hello umístění, kde můžete načíst data pro hello OMS úložiště.  Zdroje dat a řešení pro spuštění na připojené zdroje a definovat hello konkrétní data, která se shromažďují.  Pokud vaše aplikace zapisuje data tooone zdrojů dat, můžete ho shromáždit konfigurací zdroje dat hello.  Například pokud vaše aplikace vytvoří události procesu Syslog, pak se můžou shromažďovat zdrojem dat hello Syslog na agenta systému Linux.

* [Zdroje dat v analýzy protokolů](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Řešení
Řešení rozšířit možnosti hello OMS.  Řešení může shromažďovat data z připojeného zdroje hello nebo jej může provádět analýzy na záznamy již shromážděné v úložišti hello.  Každé řešení od společnosti Microsoft má jednotlivých článek, který poskytuje hello podrobné informace o hello data, která shromažďuje.

* [Řešení v analýzy protokolů](../log-analytics/log-analytics-add-solutions.md)

#### <a name="http-data-collector-api"></a>Sběrač dat protokolu HTTP rozhraní API
Hello Log Analytics HTTP dat kolekce API je rozhraní REST API, která umožňuje úložiště analýzy protokolů pro toohello tooadd JSON data.  Toto rozhraní API můžete využít, když máte aplikaci, která neposkytuje data prostřednictvím jednoho z hello jiné zdroje dat nebo řešení.  Dá se použít toopopulate hello úložiště z libovolného klienta, který můžete volat rozhraní API hello a nespoléhá se na plán kolekce hello zdroj dat nebo řešení.

* [Kolektor protokolů analýzy dat protokolu HTTP rozhraní API](../log-analytics/log-analytics-data-collector-api.md)

### <a name="retrieve-data-from-hello-log-analytics-repository"></a>Načtení dat z úložiště analýzy protokolů hello
Existuje několik metod načítání dat z úložiště OMS hello.  Můžete mají uživatelé tooretrieve data pomocí konzoly OMS hello a poskytují různé druhy vizualizaci a analýzu.  Můžete také načíst hello data z externího procesu například jiné řešení pro správu.

#### <a name="log-searches"></a>Protokol hledání
Všechna data uložená v úložišti OMS hello je k dispozici prostřednictvím protokolu hledání.  Uživatelé mohou proveďte vlastní analýz ad hoc v konzole OMS hello nebo vytvořit řídicí panel s vizualizací pro konkrétní protokolu vyhledávání.  Řešení může obsahovat vlastní zobrazení s vizualizacemi podle předdefinovaných hledání.  Můžete vytvořit hello rozhraní API pro vyhledávání protokolu tooaccess dat v úložišti OMS hello z externí aplikace nebo správy nástroje.  

* [Protokol hledání v analýzy protokolů](../log-analytics/log-analytics-log-searches.md)
* [Hledání protokolu analýzy protokolů REST API](../log-analytics/log-analytics-log-search-api.md)
* [Rutiny analýzy protokolů](https://msdn.microsoft.com/library/mt188224.aspx)

#### <a name="custom-views"></a>Vlastní zobrazení
Hello Návrhář zobrazení můžete toocreate vlastní zobrazení v konzole OMS hello uživatelům poskytnout vizualizaci a analýzu dat. hello ve vašem řešení.  Každé zobrazení zahrnuje dlaždice, který se zobrazí na hlavní stránku hello hello konzoly a libovolný počet částí vizualizace, které jsou založené na protokolu hledání, které definujete.

* [Návrhář zobrazení analýzy protokolů](../log-analytics/log-analytics-view-designer.md)

#### <a name="power-bi"></a>Power BI
Analýzy protokolů můžete automaticky exportovat data z úložiště OMS hello do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Provede export podle plánu, takže hello je zajištěna maximální toodate. 

* [Export dat tooPower analýzy protokolů BI](../log-analytics/log-analytics-powerbi.md)

## <a name="automation"></a>Automation
OMS můžete automatizovat procesy tooreact toocollected data nebo tooperform další funkce správy.  Může shromažďovat data z vaší aplikace a vložte ji do hello OMS úložiště, nebo může automatizovat hello oprava příslušných známý problém v odpovědi toodata v úložišti hello se nenašla. 

![Automation](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooky
Runbooky ve službě Azure Automation spustit skripty prostředí PowerShell a pracovních postupů v hello cloudu Azure.  Můžete je používat toomanage prostředky v Azure nebo jiným prostředkům, které jsou přístupné z cloudu hello.  Sady Runbook lze také spustit v místním datacentru pomocí hybridní pracovní proces Runbooku.  Můžete spuštění runbooku z hello portál Azure nebo z externí procesů pomocí metody, jako je PowerShell nebo hello automatizace rozhraní API.

* [Spuštění sady runbook ve službě Azure Automation](../automation/automation-starting-a-runbook.md)
* [Rutiny Azure Automation](https://msdn.microsoft.com/library/dn690262.aspx)
* [Rozhraní API REST automatizace](https://msdn.microsoft.com/library/mt662285.aspx)
* [Automatizace rozhraní .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Výstrahy
Pravidla výstrah automaticky spouštět podle plánu tooa vyhledávání protokolu.  Pokud se shodují konkrétní výsledky hello může spuštění sady runbook ve službě Azure Automation nebo volat webhook, který můžete spustit externího procesu kritéria hello výsledné výstrah.  Obě tyto odpovědí může obsahovat podrobnosti o hello výstrahu včetně hello data v hledání protokolů hello.

* [Výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md)
* [Log Analytics výstrahy API](../log-analytics/log-analytics-api-alerts.md)

## <a name="backup-and-site-recovery"></a>Zálohování a obnovení lokality
Azure Site Recovery a Backup poskytují služby pro ochranu podnikových dat a zajištění dostupnosti hello serverů a aplikací.  Můžete využít tyto služby tooperform takových scénářů, jako poskytují zálohování služby pro vaši aplikaci nebo zahájení převzetí služeb při selhání virtuálního počítače.

* [Rutiny Azure Backup](https://msdn.microsoft.com/library/mt619253.aspx)
* [Azure Site Recovery rozhraní REST API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
* [Rutiny Azure Site Recovery](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Vlastní řešení
V pracovním prostoru nebo v pracovním prostoru zákazníka, může zapouzdřit logiku integrace do toorun vlastní řešení.  Řešení může zahrnovat kteroukoliv ze hello integrace metody v tomto článku v přidání tooother prostředky tooprovide scénář dokončení správy.  Hello prostředky v řešení hello spojených tak, že pokud je odebrán hello řešení, jsou z pracovní prostor OMS hello a předplatné Azure odebrat všechny hello prostředky, které vytvořil.

Řešení může například obsahovat data služby Automation runbook toogather a proces a pak naplnit úložiště analýzy protokolů hello pomocí hello rozhraní API sady kolekcí dat protokolu HTTP.  Může také obsahovat vlastní zobrazení, které představuje a analyzuje hello shromažďovat data.  

* Vytváření vlastních řešení (už brzy)    

## <a name="next-steps"></a>Další kroky
* Referenční dokumentace hello [OMS SDK](operations-management-suite-sdk.md) technické informace o automatizaci služby OMS.  

