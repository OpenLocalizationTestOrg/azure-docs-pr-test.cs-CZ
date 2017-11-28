---
title: "Úvod do Azure Advisor | Microsoft Docs"
description: "Chcete-li optimalizovat nasazení Azure pomocí Azure Advisor."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 35678142550f9f887562f311a5e7d9516495cf53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-advisor"></a><span data-ttu-id="765e8-103">Úvod do Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="765e8-103">Introduction to Azure Advisor</span></span>

<span data-ttu-id="765e8-104">Další informace o Azure Advisor a jejími klíčovými funkcemi a získejte odpovědi na nejčastější dotazy.</span><span class="sxs-lookup"><span data-stu-id="765e8-104">Learn about Azure Advisor and its key capabilities, and get answers to frequently asked questions.</span></span>

## <a name="what-is-advisor"></a><span data-ttu-id="765e8-105">Co je Advisor?</span><span class="sxs-lookup"><span data-stu-id="765e8-105">What is Advisor?</span></span>
<span data-ttu-id="765e8-106">Advisor je konzultantem přizpůsobené cloudu, která pomáhá dodržujte doporučené postupy, chcete-li optimalizovat nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="765e8-106">Advisor is a personalized cloud consultant that helps you follow best practices to optimize your Azure deployments.</span></span> <span data-ttu-id="765e8-107">Ho analyzuje konfigurace prostředků a telemetrii využití a pak doporučuje řešení, které vám pomůžou líp finanční efektivita, výkon, vysokou dostupnost a zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="765e8-107">It analyzes your resource configuration and usage telemetry and then recommends solutions that can help you improve the cost effectiveness, performance, high availability, and security of your Azure resources.</span></span>

<span data-ttu-id="765e8-108">Advisor můžete:</span><span class="sxs-lookup"><span data-stu-id="765e8-108">With Advisor, you can:</span></span>
* <span data-ttu-id="765e8-109">Získejte proaktivní, kterého lze provést akci a přizpůsobené osvědčené postupy a doporučení.</span><span class="sxs-lookup"><span data-stu-id="765e8-109">Get proactive, actionable, and personalized best practices recommendations.</span></span> 
* <span data-ttu-id="765e8-110">Zvýšit výkon, zabezpečení a vysokou dostupnost vašich prostředků, jak identifikovat příležitosti k snížit vaše celkové Azure tráví.</span><span class="sxs-lookup"><span data-stu-id="765e8-110">Improve the performance, security, and high availability of your resources, as you identify opportunities to reduce your overall Azure spend.</span></span>
* <span data-ttu-id="765e8-111">Získejte doporučení s vložené navrhovaná akce.</span><span class="sxs-lookup"><span data-stu-id="765e8-111">Get recommendations with proposed actions inline.</span></span>

<span data-ttu-id="765e8-112">Dostanete Advisor prostřednictvím [portál Azure](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="765e8-112">You can access Advisor through the [Azure portal](https://aka.ms/azureadvisordashboard).</span></span> <span data-ttu-id="765e8-113">Přihlaste se k [portál](https://portal.azure.com), vyberte **Procházet**a potom přejděte k **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="765e8-113">Sign in to the [portal](https://portal.azure.com), select **Browse**, and then scroll to **Azure Advisor**.</span></span> <span data-ttu-id="765e8-114">Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="765e8-114">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

<span data-ttu-id="765e8-115">Doporučení jsou rozděleny do čtyř kategorií:</span><span class="sxs-lookup"><span data-stu-id="765e8-115">The recommendations are divided into four categories:</span></span> 

* <span data-ttu-id="765e8-116">**Vysoká dostupnost**: K zajištění a zlepšování kontinuity důležitými obchodními aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="765e8-116">**High Availability**: To ensure and improve the continuity of your business-critical applications.</span></span> <span data-ttu-id="765e8-117">Další informace najdete v tématu [vysokou dostupnost Advisor doporučení](advisor-high-availability-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="765e8-117">For more information, see [Advisor High Availability recommendations](advisor-high-availability-recommendations.md).</span></span>

* <span data-ttu-id="765e8-118">**Zabezpečení**: ke zjištění hrozby a ohrožení zabezpečení, které mohou vést k narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="765e8-118">**Security**: To detect threats and vulnerabilities that might lead to security breaches.</span></span> <span data-ttu-id="765e8-119">Další informace najdete v tématu [doporučení zabezpečení Advisor](advisor-security-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="765e8-119">For more information, see [Advisor Security recommendations](advisor-security-recommendations.md).</span></span>

* <span data-ttu-id="765e8-120">**Výkon**: aby se zvýšila rychlost aplikací.</span><span class="sxs-lookup"><span data-stu-id="765e8-120">**Performance**: To improve the speed of your applications.</span></span> <span data-ttu-id="765e8-121">Další informace najdete v tématu [Poradce pro výkon doporučení](advisor-performance-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="765e8-121">For more information, see [Advisor Performance recommendations](advisor-performance-recommendations.md).</span></span>

* <span data-ttu-id="765e8-122">**Náklady na**: optimalizace a snížit vaše celkové Azure tráví.</span><span class="sxs-lookup"><span data-stu-id="765e8-122">**Cost**: To optimize and reduce your overall Azure spend.</span></span> <span data-ttu-id="765e8-123">Další informace najdete v tématu [doporučení služby Advisor náklady](advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="765e8-123">For more information, see [Advisor Cost recommendations](advisor-cost-recommendations.md).</span></span>

  ![Typy doporučení služby Advisor](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> <span data-ttu-id="765e8-125">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="765e8-125">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="765e8-126">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="765e8-126">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="765e8-127">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="765e8-127">This is a *one-time operation*.</span></span> <span data-ttu-id="765e8-128">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="765e8-128">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

<span data-ttu-id="765e8-129">Můžete kliknout na doporučení Další informace o něm.</span><span class="sxs-lookup"><span data-stu-id="765e8-129">You can click a recommendation to learn more about it.</span></span> <span data-ttu-id="765e8-130">Můžete si také přečíst o akcích, které můžete využít výhod příležitost nebo vyřešte problém.</span><span class="sxs-lookup"><span data-stu-id="765e8-130">You can also learn about actions that you can perform to take advantage of an opportunity or resolve an issue.</span></span> 

<span data-ttu-id="765e8-131">Advisor nabízí doporučení s vložené akce nebo odkazy na dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="765e8-131">Advisor offers recommendations with inline actions or documentation links.</span></span> <span data-ttu-id="765e8-132">Kliknutím na vložené akce vás provede "cesty s průvodcem uživatele" k implementaci.</span><span class="sxs-lookup"><span data-stu-id="765e8-132">Clicking an inline action takes you through a “guided user journey” to implement it.</span></span> <span data-ttu-id="765e8-133">Kliknutím na odkaz dokumentace bodů dokumentaci, která popisuje, jak ručně implementovat akce.</span><span class="sxs-lookup"><span data-stu-id="765e8-133">Clicking a documentation link points you to documentation that describes how to manually implement the action.</span></span> 

<span data-ttu-id="765e8-134">Advisor aktualizuje doporučení každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="765e8-134">Advisor updates recommendations hourly.</span></span> <span data-ttu-id="765e8-135">Pokud nemáte v úmyslu provést okamžitou akci na doporučení, můžete zopakovat později pro zadané časové období nebo ho zavřít.</span><span class="sxs-lookup"><span data-stu-id="765e8-135">If you don’t intend to take immediate action on a recommendation, you can snooze it for a specified time period or dismiss it.</span></span> 

## <a name="frequently-asked-questions"></a><span data-ttu-id="765e8-136">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="765e8-136">Frequently asked questions</span></span>

### <a name="how-do-i-access-advisor"></a><span data-ttu-id="765e8-137">Přístupu Advisor</span><span class="sxs-lookup"><span data-stu-id="765e8-137">How do I access Advisor?</span></span>
<span data-ttu-id="765e8-138">Dostanete Advisor prostřednictvím [portál Azure](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="765e8-138">You can access Advisor through the [Azure portal](https://aka.ms/azureadvisordashboard).</span></span> <span data-ttu-id="765e8-139">Přihlaste se k [portál](https://portal.azure.com), vyberte **Procházet**a potom přejděte k **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="765e8-139">Sign in to the [portal](https://portal.azure.com), select **Browse**, and then scroll to **Azure Advisor**.</span></span> <span data-ttu-id="765e8-140">Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="765e8-140">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

<span data-ttu-id="765e8-141">Doporučení služby Advisor můžete zobrazit také prostřednictvím okně prostředku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="765e8-141">You can also view Advisor recommendations through the virtual machine resource blade.</span></span> <span data-ttu-id="765e8-142">Vyberte virtuální počítač a poté přejděte k doporučení služby Advisor v nabídce.</span><span class="sxs-lookup"><span data-stu-id="765e8-142">Choose a virtual machine, and then scroll to Advisor recommendations in the menu.</span></span> 

### <a name="what-permissions-do-i-need-to-access-advisor"></a><span data-ttu-id="765e8-143">Uvedete, jaká oprávnění jsou nutné pro přístup k Advisor?</span><span class="sxs-lookup"><span data-stu-id="765e8-143">What permissions do I need to access Advisor?</span></span>

<span data-ttu-id="765e8-144">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="765e8-144">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="765e8-145">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="765e8-145">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="765e8-146">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="765e8-146">This is a *one-time operation*.</span></span> <span data-ttu-id="765e8-147">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="765e8-147">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

### <a name="how-often-are-advisor-recommendations-updated"></a><span data-ttu-id="765e8-148">Jak často jsou doporučení služby Advisor aktualizovat?</span><span class="sxs-lookup"><span data-stu-id="765e8-148">How often are Advisor recommendations updated?</span></span>

<span data-ttu-id="765e8-149">Doporučení služby Advisor se aktualizují každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="765e8-149">Advisor recommendations are updated hourly.</span></span>

### <a name="what-resources-does-advisor-provide-recommendations-for"></a><span data-ttu-id="765e8-150">Jaké prostředky poskytuje Advisor doporučení pro?</span><span class="sxs-lookup"><span data-stu-id="765e8-150">What resources does Advisor provide recommendations for?</span></span>

<span data-ttu-id="765e8-151">Advisor poskytuje doporučení pro virtuální počítače, skupiny dostupnosti, application Gateway, aplikační služby, servery SQL Server, databáze SQL a Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="765e8-151">Advisor provides recommendations for virtual machines, availability sets, application gateways, App Services, SQL servers, SQL databases, and Redis Cache.</span></span>

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a><span data-ttu-id="765e8-152">Můžete připomenout znovu nebo zrušit doporučení?</span><span class="sxs-lookup"><span data-stu-id="765e8-152">Can I snooze or dismiss a recommendation?</span></span>

<span data-ttu-id="765e8-153">Připomenout znovu nebo zrušit doporučení, klikněte **připomenout znovu** tlačítko nebo odkaz.</span><span class="sxs-lookup"><span data-stu-id="765e8-153">To snooze or dismiss a recommendation, click the **Snooze** button or link.</span></span> <span data-ttu-id="765e8-154">Můžete zadat dobu připomenutí období nebo vybrat možnost **nikdy** zrušíte doporučení.</span><span class="sxs-lookup"><span data-stu-id="765e8-154">You can specify a snooze time period or select **Never** to dismiss the recommendation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="765e8-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="765e8-155">Next steps</span></span>

<span data-ttu-id="765e8-156">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="765e8-156">To learn more about Advisor recommendations, see:</span></span>

* [<span data-ttu-id="765e8-157">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="765e8-157">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="765e8-158">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="765e8-158">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="765e8-159">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="765e8-159">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
* [<span data-ttu-id="765e8-160">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="765e8-160">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="765e8-161">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="765e8-161">Advisor Cost recommendations</span></span>](advisor-cost-recommendations.md)
