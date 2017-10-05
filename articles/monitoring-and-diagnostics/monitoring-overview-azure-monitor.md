---
title: "Přehled Azure monitorování | Microsoft Docs"
description: "Azure monitorování shromažďuje statistiky pro použití v výstrahy, webhooků, škálování a automatizace. Článek taky seznam dalších možností monitorování společnosti Microsoft."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: 619a004b9aff99be68988e1f7be3ccad400a8a0e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="overview-of-azure-monitor"></a>Přehled Azure monitorování
Tento článek obsahuje přehled služby Azure monitorování v Microsoft Azure. Popisuje, co monitorování Azure nepodporuje a poskytuje odkazy na další informace o tom, jak používat Azure monitorování.  Pokud upřednostňujete video úvod, najdete v části Další kroky odkazy v dolní části tohoto článku. 

## <a name="why-monitor-your-application-or-system"></a>Proč monitorování aplikace nebo systému
Cloudové aplikace jsou komplexní s mnoha přesunutí částmi. Monitorování poskytuje data a ujistěte se, že vaše aplikace zůstává nahoru a spuštěna v dobrém stavu. Také pomáhá stave vypnout potenciální problémy nebo vyřešit potíže s uplynulou těch, které jsou. Kromě toho můžete data monitorování a získáte přehled o hloubkové o vaší aplikaci. Dané znalosti můžete dozvíte, jak zlepšit výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Azure monitorování a Microsoft je další monitorování produkty
Monitorování Azure poskytuje základní úroveň infrastruktura metriky a protokoly pro většina služeb v Microsoft Azure. Služby Azure, které ještě Neumísťujte svá data do Azure monitorování se pro něj existuje v budoucnu.

Microsoft dodává další produkty a služby, které poskytují další možnosti monitorování pro vývojáře, DevOps nebo Ops IT, které mají také na místní instalace. Přehled a pochopení jak tyto různé produkty a služby spolupracují, najdete v části [monitorování v Microsoft Azure](monitoring-overview.md).

## <a name="monitoring-sources---compute"></a>Monitorování zdroje - výpočetní

![Model pro monitorování a Diagnostika pro jiný výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

Zahrnout výpočetní služby 
- Cloud Services 
- Virtuální počítače 
- Sady škálování virtuálního počítače 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Aplikace – diagnostické protokoly, protokoly aplikací a metriky
Aplikace můžete spustit nad hostovaného operačního systému v výpočetní modelu. Emitování jejich vlastní sadu protokolů a metriky. Azure monitorování spoléhá na rozšíření diagnostiky Azure (Windows nebo Linux) ke shromažďování většina protokoly a metriky na úrovni aplikace. Zahrnout typy

* Čítače výkonu
* Protokoly aplikací
* Protokoly událostí systému Windows
* Zdroj události rozhraní .NET
* Protokoly služby IIS
* Manifest na základě trasování událostí pro Windows
* Výpisy stavu systému
* Protokoly chyb zákazníka

Bez rozšíření diagnostiky jsou k dispozici pouze několik metriky jako využití procesoru. 

### <a name="host-and-guest-vm-metrics"></a>Metriky hostitele a hostů virtuálních počítačů
Výše uvedených výpočetní prostředky mít vyhrazený hostitelský počítač a hostovaného operačního systému komunikují s. Hostitele virtuálního počítače a hostovaný operační systém jsou ekvivalentní kořenových virtuálních počítačů a hosta virtuálního počítače v hypervisoru modelu technologie Hyper-V. Můžete shromažďovat metriky na obojí. Může taky shromažďovat diagnostické protokoly na hostovaný operační systém.   

### <a name="activity-log"></a>Protokol aktivit
Protokol aktivit (dříve označovaný jako provozní nebo protokoly auditu) můžete vyhledat informace o vašem prostředku, jak je vidět infrastruktura Azure. V protokolu obsahuje informace, jako jsou časy, kdy jsou prostředky vytvořit nebo zničeno.  Další informace najdete v tématu [protokol aktivit přehled](monitoring-overview-activity-logs.md). 

## <a name="monitoring-sources---everything-else"></a>Monitorování zdroje - všem ostatním

![Model pro monitorování a Diagnostika pro výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Prostředku, metriky a protokolů diagnostiky
Shromážditelného metriky a diagnostické protokoly lišit v závislosti na typu prostředku. Například Web Apps poskytuje statistiky na v/v disku a procesoru v procentech. Tyto metriky neexistují pro fronty Service Bus, která poskytuje metriky jako propustnost velikost a zprávu fronty. Seznam shromážditelného metriky pro každý zdroj je k dispozici na [podporované metriky](monitoring-supported-metrics.md). 

### <a name="host-and-guest-vm-metrics"></a>Metriky hostitele a hostů virtuálních počítačů
Není nezbytně mapování 1:1 mezi prostředku a na konkrétní hostitele nebo hosta virtuálního počítače, metriky nejsou k dispozici.

### <a name="activity-log"></a>Protokol aktivit
Protokol aktivit je stejné jako výpočetní prostředky.  

## <a name="uses-for-monitoring-data"></a>Používá pro monitorování dat.
Jakmile shromáždíte vaše data, můžete provést na následujícím obrázku se ho v Azure monitorování

### <a name="route"></a>Trasa
Můžete Streamovat monitorování data do jiných umístění v reálném čase.

Příklady obsahují:

- Odesílání Application Insights, abyste mohli používat širší nástroje vizualizaci a analýzu.
- Odesílat do centra událostí, takže může směrovat nástroje třetích stran. 

### <a name="store-and-archive"></a>Úložiště a archivu
Některá data monitorování již uložené a k dispozici v monitorování Azure pro sadu časového intervalu. 
- Metriky se uchovávají po dobu 30 dnů. 
- Položky protokolu aktivity se uchovávají po dobu 90 dnů. 
- Diagnostické protokoly nejsou uložené ve všech. 

Pokud chcete k ukládání dat delší než časová období uvedených výše, můžete použít úložiště Azure. Sledování dat je uložen v váš účet úložiště na základě zásad uchovávání informací, které nastavíte. Budete muset platit na místo, které zabírají data v úložišti Azure. 

Několik způsobů, jak používat tato data:

- Jakmile zapsána, můžete mít další nástroje v rámci nebo mimo Azure číst a zpracovávat.
- Stáhněte si data místně pro místní archivaci nebo změňte vaše zásady uchovávání informací v cloudu a ponechat data pro dlouhou dobu.  
- Ponechat data v Azure storage po neomezenou dobu pro účely archivu. 

### <a name="query"></a>Dotaz
Monitorování REST API služby Azure, pro různé platformy příkazy rozhraní příkazového řádku (CLI), rutiny prostředí PowerShell nebo sady .NET SDK můžete použít pro přístup k datům v systému nebo úložiště Azure

Příklady obsahují:

* Získávání dat pro vlastní monitorování aplikací, který jste napsali
* Vytváření vlastních dotazů a odesílání dat do aplikace třetích stran.

### <a name="visualize"></a>Vizualizace
Vizualizace dat monitorování v grafy vám pomůže najít trendy rychlejší než vyhledávání prostřednictvím samotná data.  

Několik metod vizualizace patří:

* Použití webu Azure Portal
* Data trasy do služby Azure Application Insights
* Data trasy k Microsoft PowerBI
* Směrování dat na nástroj třetí strany vizualizaci pomocí buď živé streamování nebo tak, že nástroj pro čtení z archivu v úložišti Azure


### <a name="automate"></a>Automatizace
Můžete použít data monitorování výstrahy aktivační události nebo dokonce celé procesy. Příklady obsahují:

* Data pro automatické škálování výpočetních instancích použijte nahoru nebo dolů podle zatížení aplikace.
* Odesílání e-mailů, když metriky překračuje předem určené prahové hodnoty.
* Volání adresu URL webu (webhooku) k provedení akce v rámci systému mimo Azure
* Spuštění sady runbook ve službě Azure automation k provedení jakékoli řadu úloh

## <a name="methods-of-accessing-azure-monitor"></a>Metody přístupu k Azure monitorování
Obecně platí můžete upravit data sledování, směrování a načítání pomocí jedné z následujících metod. Ne všechny metody jsou k dispozici pro všechny akce nebo datové typy.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Napříč platformami rozhraní příkazového řádku (CLI)](insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Další kroky
Další informace o
- Je k dispozici na video s návodem právě Azure monitorování  
[Začínáme s Azure monitorování](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). Další video vysvětlením scénář, kde můžete použít Azure monitorování je k dispozici na [monitorování prozkoumat Microsoft Azure a Diagnostika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) a [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
- Spuštění prostřednictvím rozhraní Azure monitorování v [Začínáme s Azure monitorování](monitoring-get-started.md)
- Nastavit [rozšíření diagnostiky Azure](../azure-diagnostics.md) Pokud se pokoušíte diagnostikovat problémy s cloudové služby, virtuální počítač škálování virtuálního počítače nebo aplikace Service Fabric sady.
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se pokoušíte diagnostiky problémů v aplikaci služby webové aplikace.
- [Řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md) při použití úložiště objektů BLOB, tabulek a front
- [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a [služby Operations Management Suite](https://www.microsoft.com/oms/)
