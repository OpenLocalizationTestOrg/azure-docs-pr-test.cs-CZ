---
title: "Pochopení Azure útrat | Microsoft Docs"
description: "Popisuje, jak funguje Azure útrat a jak ho odebrat"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: genli
ms.openlocfilehash: a2743ef34bde0faabb3afd2ace27acddd59d3d70
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a><span data-ttu-id="817a8-103">Pochopení omezení a jak ho odebrat útraty u Azure</span><span class="sxs-lookup"><span data-stu-id="817a8-103">Understand Azure spending limit and how to remove it</span></span>

<span data-ttu-id="817a8-104">Limit útraty u Azure je omezena na kolik můžete tráví vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="817a8-104">Azure spending limit is a limit on how much your Azure subscription can spend.</span></span> <span data-ttu-id="817a8-105">Všechny nové zákazníci, kteří si zaregistrovat zkušební nabídku nebo nabídky, které zahrnuje kredity přes několik měsíců mají limit útraty ve výchozím nastavení zapnuto.</span><span class="sxs-lookup"><span data-stu-id="817a8-105">All new customers who sign up for the trial offer or offers that includes credits over multiple months have the spending limit turned on by default.</span></span> <span data-ttu-id="817a8-106">Limit útraty je $0.</span><span class="sxs-lookup"><span data-stu-id="817a8-106">The spending limit is $0.</span></span> <span data-ttu-id="817a8-107">Nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="817a8-107">It can’t be changed.</span></span> <span data-ttu-id="817a8-108">Tento limit útraty není dostupný pro typy předplatného, jako jsou předplatná s průběžnými platbami a plány závazků.</span><span class="sxs-lookup"><span data-stu-id="817a8-108">The spending limit isn’t available for subscription types such as Pay-As-You-Go subscriptions and commitment plans.</span></span> <span data-ttu-id="817a8-109">Najdete v článku [úplný seznam Azure nabízí a dostupnost limit útraty](https://azure.microsoft.com/support/legal/offer-details/).</span><span class="sxs-lookup"><span data-stu-id="817a8-109">See the [full list of Azure offers and the availability of the spending limit](https://azure.microsoft.com/support/legal/offer-details/).</span></span>

## <a name="what-happens-when-i-reach-the-spending-limit"></a><span data-ttu-id="817a8-110">Co se stane, když se dosáhne limitu útraty?</span><span class="sxs-lookup"><span data-stu-id="817a8-110">What happens when I reach the spending limit?</span></span>

<span data-ttu-id="817a8-111">Při použití výsledky v poplatky, které vyčerpat měsíční množství zahrnutý do vaší nabídky, jsou zakázané služby, které jste nasadili pro zbytek tohoto měsíce fakturace deaktivujeme.</span><span class="sxs-lookup"><span data-stu-id="817a8-111">When your usage results in charges that exhaust the monthly amounts included in your offer, the services that you deployed are disabled for the rest of that billing month.</span></span> <span data-ttu-id="817a8-112">Například se přestane provozovat služba Cloud Services, kterou jste nasadili, a virtuální počítače Azure se zastaví a zruší se jejich přidělení.</span><span class="sxs-lookup"><span data-stu-id="817a8-112">For example, Cloud Services that you deployed are removed from production and your Azure virtual machines are stopped and de-allocated.</span></span> <span data-ttu-id="817a8-113">Pokud chcete zabránit případné deaktivaci služeb, můžete se rozhodnout limit útraty odebrat.</span><span class="sxs-lookup"><span data-stu-id="817a8-113">To prevent your services from being disabled, you can choose to remove your spending limit.</span></span> <span data-ttu-id="817a8-114">Pokud vaše služby jsou zakázané, data v účtech úložiště a databáze jsou k dispozici způsobem jen pro čtení pro správce.</span><span class="sxs-lookup"><span data-stu-id="817a8-114">When your services are disabled, the data in your storage accounts and databases are available in a read-only manner for administrators.</span></span> <span data-ttu-id="817a8-115">Na začátku dalšího měsíce fakturace, pokud vaši nabídku zahrnuje kredity přes několik měsíců, předplatné bude znovu zapnout.</span><span class="sxs-lookup"><span data-stu-id="817a8-115">At the beginning of the next billing month, if your offer includes credits over multiple months, your subscription will be re-enabled.</span></span> <span data-ttu-id="817a8-116">Pak můžete znovu nasadit cloudové služby a mají plný přístup ke své účty úložiště a databáze.</span><span class="sxs-lookup"><span data-stu-id="817a8-116">Then you can redeploy your Cloud Services and have full access to your storage accounts and databases.</span></span>

<span data-ttu-id="817a8-117">Po skončení zkušební verze dosáhne limitu útraty, můžete znovu povolit odběr a má automaticky [upgradovat na naše standardní nabídka průběžné platby](billing-upgrade-azure-subscription.md) do 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="817a8-117">After the free trial subscription reaches the spending limit, you can re-enable the subscription and have it automatically [upgrade to our standard Pay-As-You-Go offer](billing-upgrade-azure-subscription.md) within 90 days.</span></span>

<span data-ttu-id="817a8-118">Oznámení se zobrazí při dosáhl limitu útraty pro vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="817a8-118">You receive notifications when you hit the spending limit for your offer.</span></span> <span data-ttu-id="817a8-119">Přihlaste se k [centra účtů Azure](https://account.windowsazure.com), vyberte **účet**a potom vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="817a8-119">Log on to the [Azure Account Center](https://account.windowsazure.com), select **ACCOUNT**, and then select **subscriptions**.</span></span> <span data-ttu-id="817a8-120">Zobrazí oznámení o odběry, které bylo dosaženo limitu útraty.</span><span class="sxs-lookup"><span data-stu-id="817a8-120">You see notifications about subscriptions that have reached the spending limit.</span></span>

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a><span data-ttu-id="817a8-121">Věcí, které vám budou účtovat i v případě, že máte limitu útraty a automaticky povoleno</span><span class="sxs-lookup"><span data-stu-id="817a8-121">Things you are charged for even if you have a spending limit enabled</span></span>

<span data-ttu-id="817a8-122">Některé služby Azure a [Marketplace zakoupí](https://azure.microsoft.com/marketplace/) může platit poplatky v části způsob platby (kopie) i v případě, že je nastaven limit útraty.</span><span class="sxs-lookup"><span data-stu-id="817a8-122">Some Azure services and [Marketplace purchases](https://azure.microsoft.com/marketplace/) can incur charges under the payment method (CC) even if a spending limit is set.</span></span> <span data-ttu-id="817a8-123">Příklady jsou licence Visual studio, Azure Active Directory premium, plánům podpory a většina třetích stran značky služby za účelem prodeje přes Marketplace.</span><span class="sxs-lookup"><span data-stu-id="817a8-123">Examples are Visual studio licenses, Azure Active Directory premium, support plans, and most third-party branded services sold through the Marketplace.</span></span>


## <a name="when-not-to-use-the-spending-limit"></a><span data-ttu-id="817a8-124">Kdy limit útraty nepoužívat</span><span class="sxs-lookup"><span data-stu-id="817a8-124">When not to use the spending limit</span></span>

<span data-ttu-id="817a8-125">Limit útraty vám může bránit v nasazení nebo použití některých služeb webu Marketplace a společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="817a8-125">The spending limit could prevent you from deploying or using certain marketplace and Microsoft services.</span></span> <span data-ttu-id="817a8-126">Tady jsou scénáře, kdy byste měli limit útraty pro svoje předplatné odebrat.</span><span class="sxs-lookup"><span data-stu-id="817a8-126">Here are the scenarios where you should remove the spending limit on your subscription.</span></span>

- <span data-ttu-id="817a8-127">Chcete nasadit image Microsoftu, třeba Oracle, a služby, jako je Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="817a8-127">You plan to deploy first party images like Oracle and services such as Visual Studio Team Services.</span></span> <span data-ttu-id="817a8-128">V tomto scénáři způsobí, že je delší než limit útraty téměř okamžitě a způsobí, že vaše předplatné se zakáže.</span><span class="sxs-lookup"><span data-stu-id="817a8-128">This scenario causes you to exceed your spending limit almost immediately and causes your subscription to be disabled.</span></span>

- <span data-ttu-id="817a8-129">Používáte ale služby, které nejde přerušit.</span><span class="sxs-lookup"><span data-stu-id="817a8-129">You have services that cannot be disrupted.</span></span>

- <span data-ttu-id="817a8-130">Nechcete ztratit nastavení služeb a prostředků, jako jsou virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="817a8-130">You have services and resources with settings like virtual IP addresses that you don't want to lose.</span></span> <span data-ttu-id="817a8-131">Tato nastavení jsou ztraceny, když jsou navrácena službám a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="817a8-131">These settings are lost when the services and resources are deallocated.</span></span>


## <a name="remove-the-spending-limit"></a><span data-ttu-id="817a8-132">Odebrání limitu útraty</span><span class="sxs-lookup"><span data-stu-id="817a8-132">Remove the spending limit</span></span>

<span data-ttu-id="817a8-133">Limit útraty můžete odebrat kdykoli za předpokladu, že je k vašemu předplatnému přidružen platný způsob platby.</span><span class="sxs-lookup"><span data-stu-id="817a8-133">You can remove the spending limit at any time as long as there's a valid payment method associated with your subscription.</span></span> <span data-ttu-id="817a8-134">U nabídek s kreditem na několik měsíců můžete limit útraty také znovu aktivovat na začátku dalšího fakturačního cyklu.</span><span class="sxs-lookup"><span data-stu-id="817a8-134">For offers that have credit over multiple months, you can also re-enable the spending limit at the beginning of your next billing cycle.</span></span>

<span data-ttu-id="817a8-135">Pokud chcete odebrat limit útraty, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="817a8-135">To remove your spending limit, follow these steps:</span></span>

1. <span data-ttu-id="817a8-136">Přihlaste se na [centra účtů Azure](https://account.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="817a8-136">Log on to the [Azure Account Center](https://account.windowsazure.com).</span></span>

2. <span data-ttu-id="817a8-137">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="817a8-137">Select a subscription.</span></span>

3. <span data-ttu-id="817a8-138">Pokud je předplatné vyčerpané, klikněte na toto oznámení: U vašeho předplatného jste dosáhli limitu útraty a automaticky se deaktivovalo, aby vám nezačaly nabíhat poplatky.</span><span class="sxs-lookup"><span data-stu-id="817a8-138">If the subscription is disabled due to the Spending Limit being reached, click this notification: "Subscription reached the Spending Limit and has been disabled to prevent charges."</span></span> <span data-ttu-id="817a8-139">Jinak, klikněte na tlačítko **odebrat útrat** v **stav PŘEDPLATNÉHO** oblasti.</span><span class="sxs-lookup"><span data-stu-id="817a8-139">Otherwise, click **Remove spending limit** in the **SUBSCRIPTION STATUS** area.</span></span>

4. <span data-ttu-id="817a8-140">Vyberte vhodnou možnost.</span><span class="sxs-lookup"><span data-stu-id="817a8-140">Select an option that is appropriate for you.</span></span>

|<span data-ttu-id="817a8-141">Možnost</span><span class="sxs-lookup"><span data-stu-id="817a8-141">Option</span></span>|<span data-ttu-id="817a8-142">Efekt</span><span class="sxs-lookup"><span data-stu-id="817a8-142">Effect</span></span>|
|-------|-----|
|<span data-ttu-id="817a8-143">Odebrat limit útraty neomezeně</span><span class="sxs-lookup"><span data-stu-id="817a8-143">Remove spending limit indefinitely</span></span>|<span data-ttu-id="817a8-144">Odebere limit útraty. Na začátku dalšího fakturačního období se automaticky neaktivuje.</span><span class="sxs-lookup"><span data-stu-id="817a8-144">Removes the spending limit without turning it on automatically at the start of the next billing period.</span></span>|
|<span data-ttu-id="817a8-145">Odebrat limit útraty pro aktuální fakturační období</span><span class="sxs-lookup"><span data-stu-id="817a8-145">Remove spending limit for the current billing period</span></span>|<span data-ttu-id="817a8-146">Odebere limit útraty. Na začátku dalšího fakturačního období se automaticky znovu aktivuje.</span><span class="sxs-lookup"><span data-stu-id="817a8-146">Removes the spending limit so that it turns back on automatically at the start of the next billing period.</span></span>|

## <a name="need-help-contact-support"></a><span data-ttu-id="817a8-147">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="817a8-147">Need help?</span></span> <span data-ttu-id="817a8-148">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="817a8-148">Contact support.</span></span>
<span data-ttu-id="817a8-149">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="817a8-149">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>
