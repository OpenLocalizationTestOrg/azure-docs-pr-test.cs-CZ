---
title: "Monitorování v Microsoft Azure | Microsoft Docs"
description: "Možnosti, když chcete monitorovat nic v Microsoft Azure. Azure monitorování, analýzy protokolů Application Insights"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: d4a94a92585420cf92018084437422fd0c66fa2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Přehled monitorování v Microsoft Azure
Tento článek obsahuje přehled nástrojů, které jsou k dispozici pro monitorování Microsoft Azure. Platí pro 
- monitorování aplikace běžící v Microsoft Azure 
- nástroje nebo služby, které běží mimo Azure, která mohou monitorovat objekty ve službě Azure. 

Popisuje různé produkty a služby, které jsou k dispozici a jak pracují společně. Může pomoci určit, které nástroje jsou pro vás nejvhodnější v jaké případech.  

## <a name="why-use-monitoring-and-diagnostics"></a>Proč používat monitorovací a diagnostické?

Problémy s výkonem v cloudové aplikace může mít vliv na vaši firmu. S více vzájemně propojena součástmi a často verzích může dojít, degradations kdykoli. A pokud vyvíjíte aplikace, uživatelé obvykle zjistit problémy, které nebyl nalezen v testování. Měli vědět o tyto problémy okamžitě a mít nástroje pro diagnostiku a řešení problémů. Microsoft Azure obsahuje řadu nástrojů pro identifikaci těchto problémů.

## <a name="how-do-i-monitor-my-azure-cloud-apps"></a>Jak se monitorování Moje Azure cloudových aplikací?

Není řadu nástrojů pro monitorování aplikací Azure a služby. Některé z jejich funkcí překrývat. Toto je částečně historických důvodů a částečně kvůli stírá mezi vývojovým týmem a operace aplikace. 

Zde jsou hlavní nástroje:

-   **Azure monitorování** je základní nástroj pro monitorování služby spuštěné v Azure. Nabízí data na úrovni infrastruktury o propustnost služby a okolního prostředí. Pokud spravujete své aplikace v Azure, rozhodování o škálování směrem nahoru nebo dolů prostředky, pak monitorování Azure vám dává používáte ke spuštění.

-   **Application Insights** lze použít pro vývoj a jako výrobní řešení monitorování. Funguje tak, že instalaci balíčku do vaší aplikace a proto nabízí více interní zobrazení co se děje. Jeho data zahrnují odezvy závislostí, výjimek trasování, ladění snímky, profily spuštění. Poskytuje výkonné inteligentní nástroje pro analýzu všech tuto telemetrii do vám pomůže při ladění aplikace a které vám pomohou pochopit, co uživatelé dělají s ním. Můžete zjistit, zda Špička při odezvy je z důvodu něco v aplikaci nebo některé externí resourcing problém. Pokud používáte Visual Studio a aplikace se na chyby, vám může přesměrováni vpravo na problém řádky kódu, můžete ji opravit.  

-   **Analýza protokolu** je pro uživatele, kteří potřebují k optimalizaci výkonu a plán údržby na aplikace běžící v produkčním prostředí. Pracuje v Azure. Shromažďuje a agreguje data z mnoha zdrojů, i když s zpožděním 10 až 15 minut. Poskytuje komplexní řešení pro správu IT pro Azure, místní a cloudové infrastruktuře jiných výrobců (například Amazon Web Services). Poskytuje bohatší nástroje k analýze dat napříč více zdrojů, umožňuje komplexní dotazy napříč všechny protokoly a může aktivně upozornit na zadaných podmínek.  Můžete dokonce shromáždění vlastních dat do své centrální úložiště, takže se můžete dotazovat a vizualizovat ho. 

-   **System Center Operations Manager (SCOM)** je pro správu a monitorování instalace velké cloudu. Je již obeznámeni s ním jako nástroj pro správu pro místní server systému Windows a na základě technologie Hyper-V-cloudy, ale můžete také integrovat a spravovat aplikace Azure. Kromě jiných věcí ho můžete nainstalovat na existující živé aplikace Application Insights.  Pokud aplikace přestane fungovat, zde zjistíte v sekundách. Všimněte si, že analýzy protokolů nenahrazuje SCOM. Funguje dobře ve spojení s ním.  


## <a name="accessing-monitoring-in-the-azure-portal"></a>Přístup k monitorování na portálu Azure
Monitorování všech služeb Azure jsou nyní k dispozici v jednom podokně uživatelského rozhraní. Další informace o tom, jak získat přístup k této oblasti najdete v tématu [Začínáme s Azure monitorování](monitoring-get-started.md). 

Monitorování funkce pro konkrétní prostředky můžete také přejít pomocí zvýrazňování tyto prostředky a podrobnějším informacím o jejich možnosti monitorování. 

## <a name="examples-of-when-to-use-which-tool"></a>Příklady použití který nástroj 

Následující části vysvětlují některé základní scénáře a které nástroje by měl použít společně. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Scénář 1 – oprava chyb v aplikaci Azure ve vývoji   

**Nejlepší možnost je společně použít Application Insights, sledování Azure a Visual Studio**

Azure teď poskytuje potenciál ladicího programu sady Visual Studio v cloudu. Konfigurace monitorování Azure k odesílání telemetrie Application insights. Povolte sady Visual Studio Application Insights SDK do aplikace zahrnout. Jednou ve službě Application Insights můžete použít aplikaci mapy zjistit vizuálně části spuštěné aplikace, které jsou není v pořádku, či nikoli. Pro ty části, které nejsou v pořádku chyby a výjimky jsou již k dispozici pro zkoumání. Různé analytics ve službě Application Insights vám pomůže přejít hlubší. Pokud si nejste jisti o této chybě, můžete pro trasování do kódu a kódu pin bod problém další ladicího programu sady Visual Studio. 

Další informace najdete v tématu [monitorování webové aplikace](../application-insights/app-insights-azure-web-apps.md) a odkazovat na obsah na levé straně pokyny pro různé typy aplikací a jazyků.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Scénář 2 – ladění webové aplikace služby Azure .NET pro chyby, které se zobrazí jenom v produkčním prostředí 

> [!NOTE]
> Tyto funkce jsou ve verzi preview. 

**Nejlepší možnost je použít Application Insights a pokud možné Visual Studio pro úplné ladění prostředí.**

Ladění aplikace pomocí ladicího programu Application Insights snímku. Dojde-li k určitým prahem chyby s provozním součásti, systém automaticky zaznamená telemetrie v systému windows času názvem "snímků." Velikost zachytit je bezpečné pro přístup z cloudu produkční, protože je dostatečně malé, nechcete mít vliv na výkon, ale dostatečně významné umožňující trasování.  Systém můžete zaznamenat několik snímků. Můžete prohlížet bod v čase na portálu Azure nebo použijte sadu Visual Studio pro úplné prostředí. Pomocí sady Visual Studio můžou vývojáři provede tento snímek jako kdyby byly ladění v reálném čase. Lokální proměnné, parametry, paměti a rámce jsou všechny dostupné. Vývojáři musí mít udělen přístup k těmto datům produkční prostřednictvím role RBAC.  

Další informace najdete v tématu [snímku ladění](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Scénář 3 – ladění aplikace Azure, která používá kontejnery nebo mikroslužeb 

**Stejné jako scénář 1. Pomocí Application Insights, sledování Azure a Visual Studio společně** Application Insights podporuje taky shromažďování telemetrie z procesů běžících v rámci kontejnery a mikroslužeb (Kubernetes, Docker, Azure Service Fabric). Další informace najdete [najdete v tomto videu o ladění kontejnerů a mikroslužeb](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Scénář 4 – oprava problémů s výkonem v aplikaci Azure

[Application Insights profileru](../application-insights/app-insights-profiler.md) slouží k řešení těchto typů problémů. Můžete identifikovat a řešit problémy s výkonem pro aplikace spuštěné v App Services (webové aplikace, Logic Apps, mobilní aplikace, aplikace API) a další výpočetní prostředky, jako jsou virtuální počítače, virtuální počítače sady škálování (VMSS), cloudové služby a Service Fabric. 

> [!NOTE]
> Schopnost profil virtuální počítače, škálovací sady virtuálních počítačů (VMSS), cloudové služby a služby prostředků infrastruktury je ve verzi preview.   

Kromě toho proaktivně upozornění e-mailem o určitých typů chyb, jako je například časů načtení stránky pomalé, od nástroj inteligentní detekce.  Nepotřebujete provádět žádnou konfiguraci na tento nástroj. Další informace najdete v tématu [Inteligentní detekce - anomálie výkonu](../application-insights/app-insights-proactive-performance-diagnostics.md) a [Inteligentní detekce - anomálie výkonu](https://azure.microsoft.com/blog/Enhancments-ApplicationInsights-SmartDetection/preview).



## <a name="next-steps"></a>Další kroky
Další informace o

* [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Začínáme s Azure monitorování](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) Pokud se pokoušíte diagnostikovat problémy s cloudové služby, virtuální počítač, virtuální počítač škálování aplikace Fabric nastaveny nebo služby.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se pokoušíte diagnostiky problémů v aplikaci služby webové aplikace.
* [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a [Operations Management Suite](https://www.microsoft.com/oms/) produkční řešení monitorování