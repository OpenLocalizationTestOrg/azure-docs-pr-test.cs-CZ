---
title: "aaaGet začít s automatické škálování podle vlastní metriky v Azure | Microsoft Docs"
description: "Zjistěte, jak tooscale prostředku podle vlastní metriky v Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: d3e268ec322698d0d367361cd9c156b21e0fb6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Začínáme s automatické škálování podle vlastní metriky v Azure
Tento článek popisuje, jak tooscale prostředku podle vlastní metriky v portálu Azure.

Azure monitorování automatického škálování se vztahují pouze tooVirtual počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service. 

# <a name="lets-get-started"></a>Umožňuje Začínáme
Tento článek předpokládá, že máte webovou aplikaci pomocí nástroje application insights nakonfigurované. Pokud nemáte již, můžete [nastavte Application Insights pro váš web ASP.NET][1]

- Otevřete [portálu Azure][2]
- Klikněte na ikonu monitorování Azure v levém navigačním podokně hello.
  ![Spustit sledování Azure][3]
- Klikněte na nastavení tooview všechny hello prostředky, na které automatického škálování se vztahuje, společně s jeho aktuální stav škálování škálování ![zjistit automatického měřítka v Azure monitorování][4]
- Otevřete okno 'Škálování' v Azure monitorování a vyberte prostředek, který chcete tooscale
> Poznámka: následující postup hello použít plán služby app service přidružené k webové aplikaci, která má Statistika aplikace nakonfigurovaná.
- V okně Nastavení hello škálování pro prostředek hello Všimněte si, že je aktuální počet instancí hello 1. Klikněte na 'Povolit škálování'.
  ![Nastavení škálování pro novou webovou aplikaci][5]
- Zadejte název pro nastavení rozsahu hello a hello klikněte na "Přidat pravidlo". Všimněte si hello škálování pravidlo možnosti, které se otevře jako kontext podokno v hello pravé straně. Ve výchozím nastavení nastaví tooscale možnost hello instanci počet o 1, pokud percetage procesoru hello hello prostředku překročí 70 %. Zdroj metriky hello změny v horní části hello příliš "Application Insights", vyberte hello prostředek Statistika aplikace v rozevírací hello 'prostředků a pak vyberte hello vlastní metriku na základě ve které chcete tooscale.
  ![Škálování podle vlastní metriky][6]
- Podobně jako toohello krok výše, přidejte pravidlo škálování, které bude škálovat v a snížit počet škálování hello o 1, pokud vlastní metrika hello je pod prahovou hodnotu.
  ![Škálování podle využití procesoru][7]
- Nastavit hello instance omezení. Například pokud chcete tooscale mezi instancemi 2 až 5 v závislosti na vlastní metriky kolísání hello, nastavte příliš "2", "minimální", maximální' příliš "5" a "default" příliš 2
> Poznámka: V případě, že je potíže při čtení hello metrika prostředků a kapacity aktuální hello je nižší než hello výchozí kapacitu, pak tooensure hello dostupnost prostředků hello škálování bude škálovat toohello výchozí hodnotu. Pokud aktuální kapacita hello již vyšší než výchozí kapacita, nebude v škálovat škálování.
- Klikněte na 'uložit.

Blahopřejeme. Je teď úspěšně vytvořili vaší tooauto nastavení škálování škálovat vaší webové aplikace založené na vlastní metriku.

> Poznámka: hello stejné kroky jsou příslušné tooget spuštění s rolí služby VMSS nebo cloud.

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
