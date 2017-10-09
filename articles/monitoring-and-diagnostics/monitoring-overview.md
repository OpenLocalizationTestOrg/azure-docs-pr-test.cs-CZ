---
title: aaaMonitoring v Microsoft Azure | Microsoft Docs
description: "Možnosti, když chcete toomonitor ničím v Microsoft Azure. Azure monitorování, analýzy protokolů Application Insights"
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
ms.openlocfilehash: f5a4f32525c52613f01a913e09a9fe3fbcbaeb21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Přehled monitorování v Microsoft Azure
Tento článek obsahuje přehled nástrojů, které jsou k dispozici pro monitorování Microsoft Azure. Platí příliš
- monitorování aplikace běžící v Microsoft Azure 
- nástroje nebo služby, které běží mimo Azure, která mohou monitorovat objekty ve službě Azure. 

Popisuje hello různé produkty a služby, které jsou k dispozici a jak pracují společně. Se vám může pomoci nástroje, které jsou pro vás nejvhodnější v případech, jaké toodetermine.  

## <a name="why-use-monitoring-and-diagnostics"></a>Proč používat monitorovací a diagnostické?

Problémy s výkonem v cloudové aplikace může mít vliv na vaši firmu. S více vzájemně propojena součástmi a často verzích může dojít, degradations kdykoli. A pokud vyvíjíte aplikace, uživatelé obvykle zjistit problémy, které nebyl nalezen v testování. Měli vědět o tyto problémy okamžitě a mít nástroje pro diagnostiku a řešení problémů hello. Microsoft Azure obsahuje řadu nástrojů pro identifikaci těchto problémů.

## <a name="how-do-i-monitor-my-azure-cloud-apps"></a>Jak se monitorování Moje Azure cloudových aplikací?

Není řadu nástrojů pro monitorování aplikací Azure a služby. Některé z jejich funkcí překrývat. Toto je částečně historických důvodů a částečně kvůli toohello stírá mezi vývojovým týmem a operace aplikace. 

Zde jsou hlavní nástroje hello:

-   **Azure monitorování** je základní nástroj pro monitorování služby spuštěné v Azure. Nabízí data na úrovni infrastruktury o hello propustnost služby a které obaluje prostředí hello. Pokud spravujete své aplikace v Azure, rozhodování, zda tooscale nahoru nebo dolů prostředky, pak monitorování Azure vám dává můžete použít toostart.

-   **Application Insights** lze použít pro vývoj a jako výrobní řešení monitorování. Funguje tak, že instalaci balíčku do vaší aplikace a proto nabízí více interní zobrazení co se děje. Jeho data zahrnují odezvy závislostí, výjimek trasování, ladění snímky, profily spuštění. Poskytuje výkonné inteligentní nástroje pro analýzu tuto telemetrii obou toohelp ladění toohelp chápete, co uživatelé dělají s ním a aplikace. Můžete zjistit, zda je kvůli Špička při odezvy toosomething v aplikaci nebo některé externí resourcing problém. Pokud používáte Visual Studio a aplikace hello je při selhání, můžete děláte správné toohello problém řádky kódu, můžete ji opravit.  

-   **Analýza protokolu** je pro uživatele, kteří potřebují tootune výkon a plán údržby na aplikace běžící v produkčním prostředí. Pracuje v Azure. Shromažďuje a agreguje data z mnoha zdrojů, když se zpožděním 10 minut too15. Poskytuje komplexní řešení pro správu IT pro Azure, místní a cloudové infrastruktuře jiných výrobců (například Amazon Web Services). Poskytuje tooanalyze data širší nástroje napříč více zdrojů, umožňuje komplexní dotazy napříč všechny protokoly a může aktivně upozornit na zadaných podmínek.  Můžete dokonce shromáždění vlastních dat do své centrální úložiště, takže se můžete dotazovat a vizualizovat ho. 

-   **System Center Operations Manager (SCOM)** je pro správu a monitorování instalace velké cloudu. Je již obeznámeni s ním jako nástroj pro správu pro místní server systému Windows a na základě technologie Hyper-V-cloudy, ale můžete také integrovat a spravovat aplikace Azure. Kromě jiných věcí ho můžete nainstalovat na existující živé aplikace Application Insights.  Pokud aplikace přestane fungovat, zde zjistíte v sekundách. Všimněte si, že analýzy protokolů nenahrazuje SCOM. Funguje dobře ve spojení s ním.  


## <a name="accessing-monitoring-in-hello-azure-portal"></a>Přístup k monitorování v hello portálu Azure
Monitorování všech služeb Azure jsou nyní k dispozici v jednom podokně uživatelského rozhraní. Další informace o tom, tooaccess této oblasti, najdete v části [Začínáme s Azure monitorování](monitoring-get-started.md). 

Monitorování funkce pro konkrétní prostředky můžete také přejít pomocí zvýrazňování tyto prostředky a podrobnějším informacím o jejich možnosti monitorování. 

## <a name="examples-of-when-toouse-which-tool"></a>Příklady toouse, který nástroj 

Následující části Hello zobrazit některé základní scénáře a které nástroje by měl použít společně. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Scénář 1 – oprava chyb v aplikaci Azure ve vývoji   

**Hello nejlepší možnost je společně toouse Application Insights, sledování Azure a Visual Studio**

Azure teď poskytuje hello úplné možností ladicího programu sady Visual Studio hello v cloudu hello. Konfigurace monitorování Azure toosend telemetrie tooApplication statistiky. Povolte hello tooinclude Visual Studio Application Insights SDK v aplikaci. Jakmile se ve službě Application Insights můžete použít toodiscover mapy aplikace hello vizuálně jsou části spuštěné aplikace, které není v pořádku, nebo ne. Pro ty části, které nejsou v pořádku chyby a výjimky jsou již k dispozici pro zkoumání. Můžete použít různé analýzy na portálu služby Application Insights toogo hlubší hello. Pokud si nejste jisti o hello chybě, můžete do kódu a kódu pin bodu problém další tootrace ladicího programu sady Visual Studio hello. 

Další informace najdete v tématu [monitorování webové aplikace](../application-insights/app-insights-azure-web-apps.md) a toohello tabulky obsahu na levé straně hello pokyny pro různé typy aplikací a jazyků.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Scénář 2 – ladění webové aplikace služby Azure .NET pro chyby, které se zobrazí jenom v produkčním prostředí 

> [!NOTE]
> Tyto funkce jsou ve verzi preview. 

**Hello nejlepší možnost je toouse Application Insights a pokud je to možné úplné sadě Visual Studio pro hello ladění prostředí.**

Ladicí program snímku Application Insights toodebug hello používají vaši aplikaci. V případě určitým prahem chyby s provozním součásti systému hello automaticky zaznamená telemetrie v systému windows času názvem "snímků." Hello velikost zachytit je bezpečné pro přístup do cloudu produkční vzhledem k tomu, že je malá dostatek není tooaffect výkon, ale dostatečně významné tooallow trasování.  Hello systému můžete zaznamenat několik snímků. Můžete podívejte se na bod v čase v hello portál Azure nebo použijte sadu Visual Studio pro úplné prostředí hello. Pomocí sady Visual Studio můžou vývojáři provede tento snímek jako kdyby byly ladění v reálném čase. Lokální proměnné, parametry, paměti a rámce jsou všechny dostupné. Vývojáři musí mít udělen přístup k datům produkční toothis prostřednictvím role RBAC.  

Další informace najdete v tématu [snímku ladění](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Scénář 3 – ladění aplikace Azure, která používá kontejnery nebo mikroslužeb 

**Stejné jako scénář 1. Pomocí Application Insights, sledování Azure a Visual Studio společně** Application Insights podporuje taky shromažďování telemetrie z procesů běžících v rámci kontejnery a mikroslužeb (Kubernetes, Docker, Azure Service Fabric). Další informace najdete [najdete v tomto videu o ladění kontejnerů a mikroslužeb](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Scénář 4 – oprava problémů s výkonem v aplikaci Azure

Hello [Application Insights profileru](../application-insights/app-insights-profiler.md) je navrženou toohelp řešení těchto typů problémů. Můžete identifikovat a řešit problémy s výkonem pro aplikace spuštěné v App Services (webové aplikace, Logic Apps, mobilní aplikace, aplikace API) a další výpočetní prostředky, jako jsou virtuální počítače, virtuální počítače sady škálování (VMSS), cloudové služby a Service Fabric. 

> [!NOTE]
> Možnost tooprofile virtuální počítače, škálovací sady virtuálních počítačů (VMSS), cloudové služby a služby prostředků infrastruktury je ve verzi preview.   

Kromě toho proaktivně upozornění e-mailem o určitých typů chyb, jako je například časů načtení stránky pomalé, od nástroj inteligentní detekce hello.  Nepotřebujete toodo žádnou konfiguraci na tento nástroj. Další informace najdete v tématu [Inteligentní detekce - anomálie výkonu](../application-insights/app-insights-proactive-performance-diagnostics.md) a [Inteligentní detekce - anomálie výkonu](https://azure.microsoft.com/blog/Enhancments-ApplicationInsights-SmartDetection/preview).



## <a name="next-steps"></a>Další kroky
Další informace o

* [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Začínáme s Azure monitorování](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) Pokud se pokoušíte toodiagnose problémy v rámci cloudové služby, virtuální počítač, virtuální počítač škálování aplikace Fabric nastaveny nebo služby.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) se pokoušíte toodiagnostic problémy v aplikaci služby webové aplikace.
* [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a hello [Operations Management Suite](https://www.microsoft.com/oms/) produkční řešení monitorování