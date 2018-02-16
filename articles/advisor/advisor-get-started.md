---
title: "Začínáme s Azure Advisor | Microsoft Docs"
description: "Začínáme s Azure Advisor."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: dc89cd29e1e8038f0ff317ff6acee332218ebce7
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="5a568-103">Začínáme se službou Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="5a568-104">Zjistěte, jak přístup Advisor prostřednictvím portálu Azure, získat doporučení a implementujte doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-104">Learn how to access Advisor through the Azure portal, get recommendations, and implement recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="5a568-105">Doporučení Advisoru</span><span class="sxs-lookup"><span data-stu-id="5a568-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="5a568-106">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5a568-106">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="5a568-107">V levém podokně klikněte na **Advisor**.</span><span class="sxs-lookup"><span data-stu-id="5a568-107">In the left pane, click **Advisor**.</span></span>  <span data-ttu-id="5a568-108">Pokud nevidíte Advisor v levém podokně, klikněte na tlačítko **další služby**.</span><span class="sxs-lookup"><span data-stu-id="5a568-108">If you do not see Advisor in the left pane, click **More services**.</span></span>  <span data-ttu-id="5a568-109">V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Advisor**.</span><span class="sxs-lookup"><span data-stu-id="5a568-109">In the service menu pane, under **Monitoring and Management**, click **Advisor**.</span></span>
 <span data-ttu-id="5a568-110">Se zobrazí řídicí panel služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="5a568-110">The Advisor dashboard is displayed.</span></span>

   ![Přístup k Azure Advisor pomocí portálu Azure](./media/advisor-get-started/advisor-portal-menu.png) 

4. <span data-ttu-id="5a568-112">Řídicí panel služby Advisor se zobrazí souhrn vaše doporučení pro všechny vybrané odběry.</span><span class="sxs-lookup"><span data-stu-id="5a568-112">The Advisor dashboard will display a summary of your recommendations for all selected subscriptions.</span></span>  <span data-ttu-id="5a568-113">Je možné, že odběry, které chcete doporučení, který se má zobrazit pro používání odběru filtrovat rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="5a568-113">You can choose the subscriptions that you want recommendations to be displayed for using the subscription filter dropdown.</span></span>

5. <span data-ttu-id="5a568-114">Chcete-li získat doporučení pro určitou kategorii, klikněte na jednu z karty: **vysokou dostupnost**, **zabezpečení**, **výkonu**, nebo **náklady**.</span><span class="sxs-lookup"><span data-stu-id="5a568-114">To get recommendations for a specific category, click one of the tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="5a568-115">Pomocí nástroje Poradce pro Azure s předplatným, předplatné *vlastníka* musí spustit Poradce pro řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="5a568-115">To use Azure Advisor with a subscription, a subscription *Owner* must launch the Advisor dashboard.</span></span>  <span data-ttu-id="5a568-116">Tato akce registruje Poradce pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="5a568-116">This action registers the subscription with Advisor.</span></span>  <span data-ttu-id="5a568-117">Od tohoto okamžiku na libovolné předplatné, *vlastníka*, *Přispěvatel*, nebo *čtečky* přístup doporučením Poradce pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="5a568-117">From that point on, any subscription *Owner*, *Contributor*, or *Reader* can access the Advisor recommendations for the subscription.</span></span>  

  ![Řídicí panel Azure Advisor](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="5a568-119">Získat podrobnosti o doporučení služby Advisor a implementovat řešení</span><span class="sxs-lookup"><span data-stu-id="5a568-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="5a568-120">Doporučení můžete vybrat v Advisor k zobrazení dalších podrobností – například akce doporučení a zasažené prostředky – a implementovat řešení doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-120">You can select a recommendation in Advisor to view additional details – such as the recommendation actions and impacted resources – and to implement the solution to the recommendation.</span></span>  

1. <span data-ttu-id="5a568-121">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="5a568-121">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="5a568-122">Vyberte kategorii doporučení, která zobrazí seznam doporučení v rámci této kategorie, nebo vyberte **všechny** zobrazíte všechna doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-122">Select a recommendation category to display the list of recommendations within that category, or select the **All** tab to view all your recommendations.</span></span>

3. <span data-ttu-id="5a568-123">Klikněte na tlačítko doporučení, které chcete zkontrolovat podrobně.</span><span class="sxs-lookup"><span data-stu-id="5a568-123">Click a recommendation that you want to review in detail.</span></span>

4. <span data-ttu-id="5a568-124">Zkontrolujte informace o doporučení a prostředky, které doporučení platí pro.</span><span class="sxs-lookup"><span data-stu-id="5a568-124">Review the information about the recommendation and the resources that the recommendation applies to.</span></span>

5. <span data-ttu-id="5a568-125">Klikněte na **Doporučená akce** provést doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-125">Click on the **Recommended Action** to implement the recommendation.</span></span>

## <a name="filter-advisor-recommendations"></a><span data-ttu-id="5a568-126">Filtrovat doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-126">Filter Advisor recommendations</span></span>

<span data-ttu-id="5a568-127">Můžete filtrovat doporučení, která přejít k podrobnostem a co je pro vás důležité.</span><span class="sxs-lookup"><span data-stu-id="5a568-127">You can filter recommendations to drill down to what is most important to you.</span></span>  <span data-ttu-id="5a568-128">Můžete filtrovat podle předplatného, typ prostředku nebo stavu doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-128">You can filter by subscription, resource type, or recommendation status.</span></span>  

1. <span data-ttu-id="5a568-129">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="5a568-129">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2.  <span data-ttu-id="5a568-130">Pomocí rozevíracích seznamů na řídicím panelu služby Advisor můžete filtrovat podle předplatného, typ prostředku nebo stavu doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-130">Use the dropdowns on the Advisor dashboard to filter by subscription, resource type, or recommendation status.</span></span>

    ![Kritéria vyhledávací filtr služby Advisor](./media/advisor-get-started/advisor-filters.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="5a568-132">Připomenout znovu nebo zrušit doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-132">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="5a568-133">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="5a568-133">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="5a568-134">Přejděte na doporučení, které chcete připomenout znovu nebo zrušit.</span><span class="sxs-lookup"><span data-stu-id="5a568-134">Navigate to the recommendation you want to snooze or dismiss.</span></span>

3. <span data-ttu-id="5a568-135">Klikněte na tlačítko doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-135">Click the recommendation.</span></span>

4. <span data-ttu-id="5a568-136">Klikněte na tlačítko **připomenout znovu**.</span><span class="sxs-lookup"><span data-stu-id="5a568-136">Click **Snooze**.</span></span> 

5. <span data-ttu-id="5a568-137">Zadejte připomenutí časové období, nebo vyberte **nikdy** zrušíte doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-137">Specify a snooze time period, or select **Never** to dismiss the recommendation.</span></span>

## <a name="exclude-subscriptions-or-resource-groups-from-advisor"></a><span data-ttu-id="5a568-138">Vyloučit z Poradce pro předplatné nebo skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5a568-138">Exclude subscriptions or resource groups from Advisor</span></span>

<span data-ttu-id="5a568-139">Můžete mít skupiny prostředků nebo odběrů, pro které nechcete přijmout doporučení služby Advisor – například "test" prostředky.</span><span class="sxs-lookup"><span data-stu-id="5a568-139">You may have resource groups or subscriptions for which you do not want to receive Advisor recommendations – such as ‘test’ resources.</span></span>  <span data-ttu-id="5a568-140">Můžete nakonfigurovat Advisor pouze generovat doporučení pro konkrétní předplatné a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5a568-140">You can configure Advisor to only generate recommendations for specific subscriptions and resource groups.</span></span>

> [!NOTE]
> <span data-ttu-id="5a568-141">Zahrnout nebo vyloučit z Poradce pro předplatné nebo skupinu prostředků, musíte být vlastníkem předplatného.</span><span class="sxs-lookup"><span data-stu-id="5a568-141">To include or exclude a subscription or resource group from Advisor, you must be a subscription Owner.</span></span>  <span data-ttu-id="5a568-142">Pokud nemáte požadovaná oprávnění pro předplatné nebo skupinu prostředků, je možnost zahrnout nebo vyloučit je zakázána v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5a568-142">If you do not have the required permissions for a subscription or resource group, the option to include or exclude it is disabled in the user interface.</span></span>

1. <span data-ttu-id="5a568-143">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="5a568-143">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="5a568-144">Klikněte na tlačítko **konfigurace** na panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="5a568-144">Click **Configure** in the action bar.</span></span>

3. <span data-ttu-id="5a568-145">Zrušte zaškrtnutí políčka všechny odběry nebo nechcete dostávat doporučení služby Advisor pro skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5a568-145">Uncheck any subscriptions or resource groups you do not want to receive Advisor recommendations for.</span></span>

    ![Advisor nakonfigurovat příklad prostředků](./media/advisor-get-started/advisor-configure-resources.png)

4. <span data-ttu-id="5a568-147">Klikněte **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a568-147">Click the **Apply** button.</span></span>

## <a name="configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation"></a><span data-ttu-id="5a568-148">Nakonfigurujte pravidlo průměrné využití procesoru doporučení nízkou využití virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5a568-148">Configure the average CPU utilization rule for the low usage virtual machine recommendation</span></span>

<span data-ttu-id="5a568-149">Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a568-149">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="5a568-150">Virtuální počítače, jejichž průměrné využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a568-150">Virtual machines whose average CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="5a568-151">Pokud chcete být agresivnější zjistit nízkém zatížení virtuálních počítačů, můžete upravit průměrná pravidlo využití procesoru na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="5a568-151">If you would like to be more aggressive at identifying low usage virtual machines, you can adjust the average CPU utilization rule on a per subscription basis.</span></span>  <span data-ttu-id="5a568-152">Pravidlo průměrné využití procesoru můžete nastavit na 5 %, 10 %, 15 % nebo 20 %.</span><span class="sxs-lookup"><span data-stu-id="5a568-152">The average CPU utilization rule can be set to 5%, 10%, 15%, or 20%.</span></span>

> [!NOTE]
> <span data-ttu-id="5a568-153">Upravit průměrná pravidlo využití procesoru pro identifikaci nízkém zatížení virtuálních počítačů, musí být předplatné *vlastníka*.</span><span class="sxs-lookup"><span data-stu-id="5a568-153">To adjust the average CPU utilization rule for identifying low usage virtual machines, you must be a subscription *Owner*.</span></span>  <span data-ttu-id="5a568-154">Pokud nemáte požadovaná oprávnění pro předplatné nebo skupinu prostředků, bude v uživatelském rozhraní zakázána možnost zahrnout nebo vyloučit.</span><span class="sxs-lookup"><span data-stu-id="5a568-154">If you do not have the required permissions for a subscription or resource group, the option to include or exclude it will be disabled in the user interface.</span></span> 

1. <span data-ttu-id="5a568-155">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="5a568-155">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="5a568-156">Klikněte na tlačítko **konfigurace** na panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="5a568-156">Click **Configure** in the action bar.</span></span>

3. <span data-ttu-id="5a568-157">Klikněte **pravidla** kartě.</span><span class="sxs-lookup"><span data-stu-id="5a568-157">Click the **Rules** tab.</span></span>

4. <span data-ttu-id="5a568-158">Vyberte předplatná, kterou chcete upravit pravidlo průměrné využití procesoru pro a pak klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="5a568-158">Select the subscriptions you’d like to adjust the average CPU utilization rule for, and then click **Edit**.</span></span>

5. <span data-ttu-id="5a568-159">Vyberte požadovanou hodnotu průměrné využití procesoru a klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="5a568-159">Select the desired average CPU utilization value, and click **Apply**.</span></span>

6. <span data-ttu-id="5a568-160">Klikněte na tlačítko **aktualizovat doporučení** aktualizovat vaše stávající doporučení použít nové pravidlo průměrné využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="5a568-160">Click **Refresh recommendations** to update your existing recommendations to use the new average CPU utilization rule.</span></span> 

   ![Příklad pravidla doporučení nakonfigurovat Advisor](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-your-advisor-recommendations"></a><span data-ttu-id="5a568-162">Stáhněte si vaše doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-162">Download your Advisor recommendations</span></span>

<span data-ttu-id="5a568-163">Advisor umožňuje stáhnout souhrn vaše doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-163">Advisor enables you to download a summary of your recommendations.</span></span>  <span data-ttu-id="5a568-164">Můžete si stáhnout vaše doporučení jako soubor ve formátu PDF nebo soubor CSV.</span><span class="sxs-lookup"><span data-stu-id="5a568-164">You can download your recommendations as a PDF file or a CSV file.</span></span>  <span data-ttu-id="5a568-165">Stahování vaše doporučení umožňuje snadno sdílet se svými kolegy nebo provádět vlastní analýzu nad data doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-165">Downloading your recommendations enables you to easily share with your colleagues or perform your own analysis on top of the recommendation data.</span></span>

1. <span data-ttu-id="5a568-166">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="5a568-166">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="5a568-167">Klikněte na tlačítko **stáhnout jako sdílený svazek clusteru** nebo **stáhnout ve formátu PDF** na panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="5a568-167">Click **Download as CSV** or **Download as PDF** on the action bar.</span></span>

<span data-ttu-id="5a568-168">Možnost stažení respektuje všechny filtry, které jste použili k řídicímu panelu Advisor.</span><span class="sxs-lookup"><span data-stu-id="5a568-168">The download option respects any filters you have applied to the Advisor dashboard.</span></span>  <span data-ttu-id="5a568-169">Pokud vyberete možnost stažení při zobrazení konkrétní doporučení kategorie nebo doporučení, zahrnuje stažené souhrn pouze informace pro tuto kategorii nebo doporučení.</span><span class="sxs-lookup"><span data-stu-id="5a568-169">If you select the download option while viewing a specific recommendation category or recommendation, the downloaded summary only includes information for that category or recommendation.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5a568-170">Další postup</span><span class="sxs-lookup"><span data-stu-id="5a568-170">Next steps</span></span>

<span data-ttu-id="5a568-171">Další informace o službě Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="5a568-171">To learn more about Advisor, see:</span></span>
* [<span data-ttu-id="5a568-172">Úvod do Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-172">Introduction to Azure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="5a568-173">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-173">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="5a568-174">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-174">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="5a568-175">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="5a568-175">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="5a568-176">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5a568-176">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
