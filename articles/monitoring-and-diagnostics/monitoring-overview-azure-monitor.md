---
title: "aaaAzure Přehled monitorování | Microsoft Docs"
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
ms.openlocfilehash: ffa304e7b158f0fceb7f60ab88fab291976aa0e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-monitor"></a>Přehled Azure monitorování
Tento článek obsahuje přehled hello Azure monitorování služby ve službě Microsoft Azure. Popisuje, co monitorování Azure nepodporuje a obsahuje ukazatele tooadditional informace o toouse Azure monitorování.  Pokud upřednostňujete video úvod, najdete v části Další odkazy na kroky v hello dolní části tohoto článku. 

## <a name="why-monitor-your-application-or-system"></a>Proč monitorování aplikace nebo systému
Cloudové aplikace jsou komplexní s mnoha přesunutí částmi. Monitorování poskytuje tooensure data, která vaše aplikace zůstává nahoru a spuštěn v dobrém stavu. Pomáhá také můžete toostave vypnout potenciální problémy a řešení potíží s uplynulou těch, které jsou. Kromě toho můžete použít monitorování hlubšímu porozumění toogain data o vaší aplikaci. Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Azure monitorování a Microsoft je další monitorování produkty
Monitorování Azure poskytuje základní úroveň infrastruktura metriky a protokoly pro většina služeb v Microsoft Azure. Služby Azure, které ještě Neumísťujte svá data do Azure monitorování se pro něj existuje hello budoucí.

Microsoft dodává další produkty a služby, které poskytují další možnosti monitorování pro vývojáře, DevOps nebo Ops IT, které mají také na místní instalace. Přehled a pochopení jak tyto různé produkty a služby spolupracují, najdete v části [monitorování v Microsoft Azure](monitoring-overview.md).

## <a name="monitoring-sources---compute"></a>Monitorování zdroje - výpočetní

![Model pro monitorování a Diagnostika pro jiný výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

Zahrnout Hello výpočetní služby 
- Cloud Services 
- Virtuální počítače 
- Sady škálování virtuálního počítače 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Aplikace – diagnostické protokoly, protokoly aplikací a metriky
Aplikace můžete spustit nad hello hostovaného operačního systému v modelu výpočetní hello. Emitování jejich vlastní sadu protokolů a metriky. Azure monitorování spoléhá na hello Azure diagnostics rozšíření (Windows nebo Linux) toocollect většina protokoly a metriky na úrovni aplikace. typy Hello

* Čítače výkonu
* Protokoly aplikací
* Protokoly událostí systému Windows
* Zdroj události rozhraní .NET
* Protokoly služby IIS
* Manifest na základě trasování událostí pro Windows
* Výpisy stavu systému
* Protokoly chyb zákazníka

Bez hello rozšíření diagnostiky jsou k dispozici pouze několik metriky jako využití procesoru. 

### <a name="host-and-guest-vm-metrics"></a>Metriky hostitele a hostů virtuálních počítačů
Hello výše uvedených výpočetní prostředky mít vyhrazený hostitelský počítač, hostovaný operační systém, které komunikují s. Hello hostitele virtuálních počítačů a hostovaný operační systém jsou ekvivalentní hello kořenové virtuálních počítačů a virtuálním počítači hosta v modelu hypervisoru hello technologie Hyper-V. Můžete shromažďovat metriky na obojí. Může taky shromažďovat diagnostické protokoly na hello hostovaný operační systém.   

### <a name="activity-log"></a>Protokol aktivit
Hello protokol aktivit (dříve označovaný jako provozní nebo protokoly auditu) můžete vyhledat informace o vašem prostředku jakém ho vidí hello infrastruktury Azure. Hello protokolu obsahuje informace, jako jsou časy, kdy jsou prostředky vytvořit nebo zničeno.  Další informace najdete v tématu [protokol aktivit přehled](monitoring-overview-activity-logs.md). 

## <a name="monitoring-sources---everything-else"></a>Monitorování zdroje - všem ostatním

![Model pro monitorování a Diagnostika pro výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Prostředku, metriky a protokolů diagnostiky
Lišit v závislosti na typu prostředku hello shromážditelného metriky a diagnostické protokoly. Například Web Apps poskytuje statistiky na hello v/v disku a procesoru v procentech. Tyto metriky neexistují pro fronty Service Bus, která poskytuje metriky jako propustnost velikost a zprávu fronty. Seznam shromážditelného metriky pro každý zdroj je k dispozici na [podporované metriky](monitoring-supported-metrics.md). 

### <a name="host-and-guest-vm-metrics"></a>Metriky hostitele a hostů virtuálních počítačů
Není nezbytně mapování 1:1 mezi prostředku a na konkrétní hostitele nebo hosta virtuálního počítače, metriky nejsou k dispozici.

### <a name="activity-log"></a>Protokol aktivit
Protokol aktivit Hello je hello stejné jako pro výpočetní prostředky.  

## <a name="uses-for-monitoring-data"></a>Používá pro monitorování dat.
Jakmile shromáždíte vaše data, můžete provést následující ho v Azure monitorování hello

### <a name="route"></a>Trasa
Dá Streamovat monitorování tooother umístění dat v reálném čase.

Příklady obsahují:

- Odesláno tooApplication statistiky, můžete použít bohatší nástroje vizualizaci a analýzu.
- Odesláno tooEvent rozbočovače, je možné směrovat nástroje toothird výrobců. 

### <a name="store-and-archive"></a>Úložiště a archivu
Některá data monitorování již uložené a k dispozici v monitorování Azure pro sadu časového intervalu. 
- Metriky se uchovávají po dobu 30 dnů. 
- Položky protokolu aktivity se uchovávají po dobu 90 dnů. 
- Diagnostické protokoly nejsou uložené ve všech. 

Pokud chcete data toostore delší než hello časových období uvedených výše, můžete použít úložiště Azure. Sledování dat je uložen v váš účet úložiště na základě zásad uchovávání informací, které nastavíte. Máte toopay pro hello místo hello trvá dat v úložišti Azure. 

Několik způsobů toouse tato data:

- Jakmile zapsána, můžete mít další nástroje v rámci nebo mimo Azure číst a zpracovávat.
- Stáhněte si hello data místně pro místní archivaci nebo změňte vaše zásady uchovávání informací v datech tookeep cloudu hello dlouhou dobu.  
- Necháte hello dat v úložišti Azure po neomezenou dobu pro účely archivu. 

### <a name="query"></a>Dotaz
Můžete použít hello monitorování rozhraní REST API Azure, křížové platformy příkazy rozhraní příkazového řádku (CLI), rutiny prostředí PowerShell nebo sady .NET SDK tooaccess hello hello dat v systému hello nebo úložiště Azure

Příklady obsahují:

* Získávání dat pro vlastní monitorování aplikací, který jste napsali
* Vytváření vlastních dotazů a odesílání tuto aplikaci dat tooa třetích stran.

### <a name="visualize"></a>Vizualizace
Vizualizace dat monitorování v grafy vám pomůže najít trendy rychlejší než vyhledávání prostřednictvím hello samotná data.  

Několik metod vizualizace patří:

* Hello použití portálu Azure
* TooAzure data trasy Application Insights
* TooMicrosoft data trasy PowerBI
* Trasy hello dat tooa třetích stran vizualizace nástroj pomocí živě Streamovat nebo tak, že nástroj hello číst z archivu v úložišti Azure


### <a name="automate"></a>Automatizace
Můžete použít monitorování upozornění na data tootrigger nebo i v celé procesy. Příklady obsahují:

* Použijte data tooautoscale výpočetních instancích nahoru nebo dolů podle zatížení aplikace.
* Odesílání e-mailů, když metriky překračuje předem určené prahové hodnoty.
* Volání webovou adresu URL (webhooku) tooexecute akce v rámci systému mimo Azure
* Spuštění sady runbook v Azure automation tooperform všechny různé úlohy

## <a name="methods-of-accessing-azure-monitor"></a>Metody přístupu k Azure monitorování
Obecně platí můžete upravit data sledování, směrování a načítání pomocí jedné z následujících metod hello. Ne všechny metody jsou k dispozici pro všechny akce nebo datové typy.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Napříč platformami rozhraní příkazového řádku (CLI)](insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Další kroky
Další informace o
- Je k dispozici na video s návodem právě Azure monitorování  
[Začínáme s Azure monitorování](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). Další video vysvětlením scénář, kde můžete použít Azure monitorování je k dispozici na [monitorování prozkoumat Microsoft Azure a Diagnostika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) a [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
- Spuštění prostřednictvím rozhraní Azure monitorování hello v [Začínáme s Azure monitorování](monitoring-get-started.md)
- Nastavit hello [rozšíření diagnostiky Azure](../azure-diagnostics.md) Pokud se pokoušíte toodiagnose problémy v rámci cloudové služby, virtuální počítač škálování virtuálního počítače nebo aplikace Service Fabric sady.
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) se pokoušíte toodiagnostic problémy v aplikaci služby webové aplikace.
- [Řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md) při použití úložiště objektů BLOB, tabulek a front
- [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a hello [Operations Management Suite](https://www.microsoft.com/oms/)
