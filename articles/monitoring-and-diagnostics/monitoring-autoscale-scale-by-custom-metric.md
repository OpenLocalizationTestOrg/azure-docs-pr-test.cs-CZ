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
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a><span data-ttu-id="c72cb-103">Začínáme s automatické škálování podle vlastní metriky v Azure</span><span class="sxs-lookup"><span data-stu-id="c72cb-103">Get started with auto scale by custom metric in Azure</span></span>
<span data-ttu-id="c72cb-104">Tento článek popisuje, jak tooscale prostředku podle vlastní metriky v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c72cb-104">This article describes how tooscale your resource by a custom metric in Azure portal.</span></span>

<span data-ttu-id="c72cb-105">Azure monitorování automatického škálování se vztahují pouze tooVirtual počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service.</span><span class="sxs-lookup"><span data-stu-id="c72cb-105">Azure Monitor auto scale applies only tooVirtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="c72cb-106">Umožňuje Začínáme</span><span class="sxs-lookup"><span data-stu-id="c72cb-106">Lets get started</span></span>
<span data-ttu-id="c72cb-107">Tento článek předpokládá, že máte webovou aplikaci pomocí nástroje application insights nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="c72cb-107">This article assumes that you have a web app with application insights configured.</span></span> <span data-ttu-id="c72cb-108">Pokud nemáte již, můžete [nastavte Application Insights pro váš web ASP.NET][1]</span><span class="sxs-lookup"><span data-stu-id="c72cb-108">If you don't have one already, you can [set up Application Insights for your ASP.NET website][1]</span></span>

- <span data-ttu-id="c72cb-109">Otevřete [portálu Azure][2]</span><span class="sxs-lookup"><span data-stu-id="c72cb-109">Open [Azure portal][2]</span></span>
- <span data-ttu-id="c72cb-110">Klikněte na ikonu monitorování Azure v levém navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="c72cb-110">Click on Azure Monitor icon in hello left navigation pane.</span></span>
  <span data-ttu-id="c72cb-111">![Spustit sledování Azure][3]</span><span class="sxs-lookup"><span data-stu-id="c72cb-111">![Launch Azure Monitor][3]</span></span>
- <span data-ttu-id="c72cb-112">Klikněte na nastavení tooview všechny hello prostředky, na které automatického škálování se vztahuje, společně s jeho aktuální stav škálování škálování ![zjistit automatického měřítka v Azure monitorování][4]</span><span class="sxs-lookup"><span data-stu-id="c72cb-112">Click on Autoscale setting tooview all hello resources for which auto scale is applicable, along with its current autoscale status ![Discover auto scale in Azure monitor][4]</span></span>
- <span data-ttu-id="c72cb-113">Otevřete okno 'Škálování' v Azure monitorování a vyberte prostředek, který chcete tooscale</span><span class="sxs-lookup"><span data-stu-id="c72cb-113">Open 'Autoscale' blade in Azure Monitor and select a resource you want tooscale</span></span>
> <span data-ttu-id="c72cb-114">Poznámka: následující postup hello použít plán služby app service přidružené k webové aplikaci, která má Statistika aplikace nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="c72cb-114">Note: hello steps below use an app service plan associated with a web app that has app insights configured.</span></span>
- <span data-ttu-id="c72cb-115">V okně Nastavení hello škálování pro prostředek hello Všimněte si, že je aktuální počet instancí hello 1.</span><span class="sxs-lookup"><span data-stu-id="c72cb-115">In hello scale setting blade for hello resource, notice that hello current instance count is 1.</span></span> <span data-ttu-id="c72cb-116">Klikněte na 'Povolit škálování'.</span><span class="sxs-lookup"><span data-stu-id="c72cb-116">Click on 'Enable autoscale'.</span></span>
  <span data-ttu-id="c72cb-117">![Nastavení škálování pro novou webovou aplikaci][5]</span><span class="sxs-lookup"><span data-stu-id="c72cb-117">![Scale setting for new web app][5]</span></span>
- <span data-ttu-id="c72cb-118">Zadejte název pro nastavení rozsahu hello a hello klikněte na "Přidat pravidlo".</span><span class="sxs-lookup"><span data-stu-id="c72cb-118">Provide a name for hello scale setting, and hello click on "Add a rule".</span></span> <span data-ttu-id="c72cb-119">Všimněte si hello škálování pravidlo možnosti, které se otevře jako kontext podokno v hello pravé straně.</span><span class="sxs-lookup"><span data-stu-id="c72cb-119">Notice hello scale rule options that opens as a context pane in hello right hand side.</span></span> <span data-ttu-id="c72cb-120">Ve výchozím nastavení nastaví tooscale možnost hello instanci počet o 1, pokud percetage procesoru hello hello prostředku překročí 70 %.</span><span class="sxs-lookup"><span data-stu-id="c72cb-120">By default, it sets hello option tooscale your instance count by 1 if hello CPU percetage of hello resource exceeds 70%.</span></span> <span data-ttu-id="c72cb-121">Zdroj metriky hello změny v horní části hello příliš "Application Insights", vyberte hello prostředek Statistika aplikace v rozevírací hello 'prostředků a pak vyberte hello vlastní metriku na základě ve které chcete tooscale.</span><span class="sxs-lookup"><span data-stu-id="c72cb-121">Change hello metric source at hello top too"Application Insights", select hello app insights resource in hello 'Resource' dropdown and then select hello custom metric based on which you want tooscale.</span></span>
  <span data-ttu-id="c72cb-122">![Škálování podle vlastní metriky][6]</span><span class="sxs-lookup"><span data-stu-id="c72cb-122">![Scale by custom metric][6]</span></span>
- <span data-ttu-id="c72cb-123">Podobně jako toohello krok výše, přidejte pravidlo škálování, které bude škálovat v a snížit počet škálování hello o 1, pokud vlastní metrika hello je pod prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c72cb-123">Similar toohello step above, add a scale rule that will scale in and decrease hello scale count by 1 if hello custom metric is below a threshold.</span></span>
  <span data-ttu-id="c72cb-124">![Škálování podle využití procesoru][7]</span><span class="sxs-lookup"><span data-stu-id="c72cb-124">![Scale based on cpu][7]</span></span>
- <span data-ttu-id="c72cb-125">Nastavit hello instance omezení.</span><span class="sxs-lookup"><span data-stu-id="c72cb-125">Set hello you instance limits.</span></span> <span data-ttu-id="c72cb-126">Například pokud chcete tooscale mezi instancemi 2 až 5 v závislosti na vlastní metriky kolísání hello, nastavte příliš "2", "minimální", maximální' příliš "5" a "default" příliš 2</span><span class="sxs-lookup"><span data-stu-id="c72cb-126">For example, if you want tooscale between 2-5 instances depending on hello custom metric fluctuations, set 'minimum' too'2', 'maximum' too'5' and 'default' too'2'</span></span>
> <span data-ttu-id="c72cb-127">Poznámka: V případě, že je potíže při čtení hello metrika prostředků a kapacity aktuální hello je nižší než hello výchozí kapacitu, pak tooensure hello dostupnost prostředků hello škálování bude škálovat toohello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c72cb-127">Note: In case there is a problem reading hello resource metrics and hello current capacity is below hello default capacity, then tooensure hello availability of hello resource, Autoscale will scale out toohello default value.</span></span> <span data-ttu-id="c72cb-128">Pokud aktuální kapacita hello již vyšší než výchozí kapacita, nebude v škálovat škálování.</span><span class="sxs-lookup"><span data-stu-id="c72cb-128">If hello current capacity is already higher than default capacity, Autoscale will not scale in.</span></span>
- <span data-ttu-id="c72cb-129">Klikněte na 'uložit.</span><span class="sxs-lookup"><span data-stu-id="c72cb-129">Click on 'Save'</span></span>

<span data-ttu-id="c72cb-130">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="c72cb-130">Congratulations.</span></span> <span data-ttu-id="c72cb-131">Je teď úspěšně vytvořili vaší tooauto nastavení škálování škálovat vaší webové aplikace založené na vlastní metriku.</span><span class="sxs-lookup"><span data-stu-id="c72cb-131">You now now succesfully created your scale setting tooauto scale your web app based on a custom metric.</span></span>

> <span data-ttu-id="c72cb-132">Poznámka: hello stejné kroky jsou příslušné tooget spuštění s rolí služby VMSS nebo cloud.</span><span class="sxs-lookup"><span data-stu-id="c72cb-132">Note: hello same steps are applicable tooget started with a VMSS or cloud service role.</span></span>

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
