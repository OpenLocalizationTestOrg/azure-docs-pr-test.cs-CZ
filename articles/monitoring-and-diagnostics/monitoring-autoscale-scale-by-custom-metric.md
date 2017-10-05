---
title: "Začínáme s automatické škálování podle vlastní metriky v Azure | Microsoft Docs"
description: "Zjistěte, jak se škálovat prostředek podle vlastní metriky v Azure."
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
ms.openlocfilehash: de8f7acadc282e4b81c657b1723f00fd3e5fd4f2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a><span data-ttu-id="0a2de-103">Začínáme s automatické škálování podle vlastní metriky v Azure</span><span class="sxs-lookup"><span data-stu-id="0a2de-103">Get started with auto scale by custom metric in Azure</span></span>
<span data-ttu-id="0a2de-104">Tento článek popisuje postup škálování prostředku podle vlastní metriky v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0a2de-104">This article describes how to scale your resource by a custom metric in Azure portal.</span></span>

<span data-ttu-id="0a2de-105">Azure monitorování automatického škálování se vztahuje pouze na virtuální počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service.</span><span class="sxs-lookup"><span data-stu-id="0a2de-105">Azure Monitor auto scale applies only to Virtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="0a2de-106">Umožňuje Začínáme</span><span class="sxs-lookup"><span data-stu-id="0a2de-106">Lets get started</span></span>
<span data-ttu-id="0a2de-107">Tento článek předpokládá, že máte webovou aplikaci pomocí nástroje application insights nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="0a2de-107">This article assumes that you have a web app with application insights configured.</span></span> <span data-ttu-id="0a2de-108">Pokud nemáte již, můžete [nastavte Application Insights pro váš web ASP.NET][1]</span><span class="sxs-lookup"><span data-stu-id="0a2de-108">If you don't have one already, you can [set up Application Insights for your ASP.NET website][1]</span></span>

- <span data-ttu-id="0a2de-109">Otevřete [portálu Azure][2]</span><span class="sxs-lookup"><span data-stu-id="0a2de-109">Open [Azure portal][2]</span></span>
- <span data-ttu-id="0a2de-110">Klikněte na ikonu monitorování Azure v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="0a2de-110">Click on Azure Monitor icon in the left navigation pane.</span></span>
  <span data-ttu-id="0a2de-111">![Spustit sledování Azure][3]</span><span class="sxs-lookup"><span data-stu-id="0a2de-111">![Launch Azure Monitor][3]</span></span>
- <span data-ttu-id="0a2de-112">Klikněte na nastavení automatického škálování, chcete-li zobrazit všechny prostředky, na které automatického škálování se vztahuje, společně s jeho aktuální stav škálování ![zjistit automatického měřítka v Azure monitorování][4]</span><span class="sxs-lookup"><span data-stu-id="0a2de-112">Click on Autoscale setting to view all the resources for which auto scale is applicable, along with its current autoscale status ![Discover auto scale in Azure monitor][4]</span></span>
- <span data-ttu-id="0a2de-113">Otevřete okno 'Škálování' v Azure monitorování a vyberte prostředek, který chcete škálovat</span><span class="sxs-lookup"><span data-stu-id="0a2de-113">Open 'Autoscale' blade in Azure Monitor and select a resource you want to scale</span></span>
> <span data-ttu-id="0a2de-114">Poznámka: Následující postup použijte plán služby app service přidružené k webové aplikaci, která má Statistika aplikace nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="0a2de-114">Note: The steps below use an app service plan associated with a web app that has app insights configured.</span></span>
- <span data-ttu-id="0a2de-115">V okně Nastavení škálování pro prostředek Všimněte si, že je aktuální počet instancí 1.</span><span class="sxs-lookup"><span data-stu-id="0a2de-115">In the scale setting blade for the resource, notice that the current instance count is 1.</span></span> <span data-ttu-id="0a2de-116">Klikněte na 'Povolit škálování'.</span><span class="sxs-lookup"><span data-stu-id="0a2de-116">Click on 'Enable autoscale'.</span></span>
  <span data-ttu-id="0a2de-117">![Nastavení škálování pro novou webovou aplikaci][5]</span><span class="sxs-lookup"><span data-stu-id="0a2de-117">![Scale setting for new web app][5]</span></span>
- <span data-ttu-id="0a2de-118">Zadejte název pro nastavení škálování a klikněte na "Přidat pravidlo".</span><span class="sxs-lookup"><span data-stu-id="0a2de-118">Provide a name for the scale setting, and the click on "Add a rule".</span></span> <span data-ttu-id="0a2de-119">Všimněte si, že pravidlo škálování možnostech, které se otevře jako kontext podokno v pravé straně.</span><span class="sxs-lookup"><span data-stu-id="0a2de-119">Notice the scale rule options that opens as a context pane in the right hand side.</span></span> <span data-ttu-id="0a2de-120">Ve výchozím nastavení nastaví možnost škálování vašeho počet instancí 1, pokud percetage procesoru prostředku je vyšší než 70 %.</span><span class="sxs-lookup"><span data-stu-id="0a2de-120">By default, it sets the option to scale your instance count by 1 if the CPU percetage of the resource exceeds 70%.</span></span> <span data-ttu-id="0a2de-121">Změnit metriky zdroje v horní části na "Application Insights", vyberte prostředek Statistika aplikace v rozevírací nabídce 'prostředků a pak vyberte vlastní metrika na základě na kterou chcete škálovat.</span><span class="sxs-lookup"><span data-stu-id="0a2de-121">Change the metric source at the top to "Application Insights", select the app insights resource in the 'Resource' dropdown and then select the custom metric based on which you want to scale.</span></span>
  <span data-ttu-id="0a2de-122">![Škálování podle vlastní metriky][6]</span><span class="sxs-lookup"><span data-stu-id="0a2de-122">![Scale by custom metric][6]</span></span>
- <span data-ttu-id="0a2de-123">Podobně jako výše, přidejte pravidlo škálování, který bude škálovat v a snížit počet škálování o 1, pokud vlastní metrika je pod prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0a2de-123">Similar to the step above, add a scale rule that will scale in and decrease the scale count by 1 if the custom metric is below a threshold.</span></span>
  <span data-ttu-id="0a2de-124">![Škálování podle využití procesoru][7]</span><span class="sxs-lookup"><span data-stu-id="0a2de-124">![Scale based on cpu][7]</span></span>
- <span data-ttu-id="0a2de-125">Nastavit vy instance omezení.</span><span class="sxs-lookup"><span data-stu-id="0a2de-125">Set the you instance limits.</span></span> <span data-ttu-id="0a2de-126">Například pokud chcete změnit měřítko mezi instancemi 2 až 5 v závislosti na vlastní metriky kolísání, nastavte minimální na "2", maximální "5" a "default" na "2"</span><span class="sxs-lookup"><span data-stu-id="0a2de-126">For example, if you want to scale between 2-5 instances depending on the custom metric fluctuations, set 'minimum' to '2', 'maximum' to '5' and 'default' to '2'</span></span>
> <span data-ttu-id="0a2de-127">Poznámka: V případě, že je potíže při čtení metrika prostředků a aktuální kapacita je nižší než výchozí kapacity, pak pro zajištění dostupnosti prostředků, automatické škálování bude škálovat na výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0a2de-127">Note: In case there is a problem reading the resource metrics and the current capacity is below the default capacity, then to ensure the availability of the resource, Autoscale will scale out to the default value.</span></span> <span data-ttu-id="0a2de-128">Pokud je aktuální kapacita již vyšší než výchozí kapacita, nebude v škálovat škálování.</span><span class="sxs-lookup"><span data-stu-id="0a2de-128">If the current capacity is already higher than default capacity, Autoscale will not scale in.</span></span>
- <span data-ttu-id="0a2de-129">Klikněte na 'uložit.</span><span class="sxs-lookup"><span data-stu-id="0a2de-129">Click on 'Save'</span></span>

<span data-ttu-id="0a2de-130">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="0a2de-130">Congratulations.</span></span> <span data-ttu-id="0a2de-131">Jste teď úspěšně vytvořili vaší škálování nastavení na automatické škálování webové aplikace založené na vlastní metriku.</span><span class="sxs-lookup"><span data-stu-id="0a2de-131">You now now succesfully created your scale setting to auto scale your web app based on a custom metric.</span></span>

> <span data-ttu-id="0a2de-132">Poznámka: Stejný postup platí pro začít pracovat s rolí služby VMSS nebo cloud.</span><span class="sxs-lookup"><span data-stu-id="0a2de-132">Note: The same steps are applicable to get started with a VMSS or cloud service role.</span></span>

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
