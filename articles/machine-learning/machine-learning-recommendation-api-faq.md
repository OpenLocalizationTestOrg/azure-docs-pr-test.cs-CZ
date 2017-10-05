---
title: "Nastavení a použití Machine Learning API doporučení | Microsoft Docs"
description: "Rozhraní API služby doporučení Microsoft vytvořené s nástroji Azure Machine Learning – nejčastější dotazy"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 3851589818bb8f4309bf3c65f17b115e0dcd27fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a><span data-ttu-id="1c428-103">Nejčastější dotazy k nastavení a používání rozhraní Machine Learning Recommendations API</span><span class="sxs-lookup"><span data-stu-id="1c428-103">Setting up and using Machine Learning Recommendations API FAQ</span></span>
<span data-ttu-id="1c428-104">**Co je doporučení?**</span><span class="sxs-lookup"><span data-stu-id="1c428-104">**What is RECOMMENDATIONS?**</span></span>

> [!NOTE]
> <span data-ttu-id="1c428-105">Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="1c428-105">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="1c428-106">Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="1c428-106">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="1c428-107">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="1c428-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="1c428-108">Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="1c428-108">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="1c428-109">Pro organizace a firmy, které jsou závislé na doporučení, která prodává mezi a až prodává produktů a služeb zákazníkům doporučení v Azure Machine Learning poskytuje modul Samoobslužné služby doporučení.</span><span class="sxs-lookup"><span data-stu-id="1c428-109">For organizations and businesses that rely on recommendations to cross-sell and up-sell products and services to their customers, RECOMMENDATIONS in Azure Machine Learning provides a self-service recommendations engine.</span></span> <span data-ttu-id="1c428-110">Je implementace spolupráce filtrování, která používá matice factorization jako algoritmus jádra.</span><span class="sxs-lookup"><span data-stu-id="1c428-110">It is an implementation of collaborative filtering that uses matrix factorization as its core algorithm.</span></span> <span data-ttu-id="1c428-111">Vývojáři aplikace přístup pomocí rozhraní REST API doporučení.</span><span class="sxs-lookup"><span data-stu-id="1c428-111">Application developers can access RECOMMENDATIONS by using REST APIs.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="1c428-112">**Co můžete dělat s DOPORUČENÍMI?**</span><span class="sxs-lookup"><span data-stu-id="1c428-112">**What can I do with RECOMMENDATIONS?**</span></span>

<span data-ttu-id="1c428-113">DOPORUČENÍ používá jako vstup některou položku nebo sadu položek a vrátí seznam hodnot příslušná doporučení.</span><span class="sxs-lookup"><span data-stu-id="1c428-113">RECOMMENDATIONS takes as input an item or a set of items and returns a list of relevant recommendations.</span></span> <span data-ttu-id="1c428-114">Příklad: zákazník online prodejce klikne produktu.</span><span class="sxs-lookup"><span data-stu-id="1c428-114">For example: A customer of an online retailer clicks a product.</span></span> <span data-ttu-id="1c428-115">Online prodejce odešle produktu jako vstup pro doporučení, získá seznam produktů na oplátku a rozhodne, která z těchto produktů se zobrazí na zákazníka.</span><span class="sxs-lookup"><span data-stu-id="1c428-115">The online retailer sends that product as input to RECOMMENDATIONS, gets a list of products in return, and decides which of these products will be shown to the customer.</span></span> <span data-ttu-id="1c428-116">Můžete chtít použít doporučení k optimalizaci online úložiště nebo i k informování vaší uvnitř prodejní oddělení nebo volání center.</span><span class="sxs-lookup"><span data-stu-id="1c428-116">You may want to use RECOMMENDATIONS to optimize your online store or even to inform your inside sales department or call center.</span></span>

<span data-ttu-id="1c428-117">**Existují nějaká omezení využití?**</span><span class="sxs-lookup"><span data-stu-id="1c428-117">**Are there any usage limitations?**</span></span>

<span data-ttu-id="1c428-118">Doporučení má následující omezení využití:</span><span class="sxs-lookup"><span data-stu-id="1c428-118">Recommendations has the following usage limitations:</span></span>

* <span data-ttu-id="1c428-119">Maximální počet modelů podle předplatného: 10</span><span class="sxs-lookup"><span data-stu-id="1c428-119">Maximum number of models per subscription: 10</span></span>
* <span data-ttu-id="1c428-120">Maximální počet položek, které mohou být uloženy katalog: 100 000</span><span class="sxs-lookup"><span data-stu-id="1c428-120">Maximum number of items that a catalog can hold: 100,000</span></span>
* <span data-ttu-id="1c428-121">Maximální počet bodů využití, které jsou zachovány je ~ 5 000 000.</span><span class="sxs-lookup"><span data-stu-id="1c428-121">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="1c428-122">Nejstarší budou odstraněna, pokud nové se nahrál nebo nahlásí.</span><span class="sxs-lookup"><span data-stu-id="1c428-122">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="1c428-123">Maximální velikost dat, který lze poslat e-mailem (například import katalogu dat, data o využití import) je 200 MB</span><span class="sxs-lookup"><span data-stu-id="1c428-123">Maximum size of data that can be sent in email (for example, import catalog data, import usage data) is 200 MB</span></span>
* <span data-ttu-id="1c428-124">Počet transakcí za sekundu (TPS) pro sestavení modelu doporučení, která není aktivní je TPS ~ 2.</span><span class="sxs-lookup"><span data-stu-id="1c428-124">Number of transactions per second (TPS) for a Recommendations model build that is not active is ~2 TPS.</span></span> <span data-ttu-id="1c428-125">Až 20 TPS mohou být uloženy sestavení modelu doporučení, která je aktivní.</span><span class="sxs-lookup"><span data-stu-id="1c428-125">A Recommendations model build that is active can hold up to 20 TPS.</span></span>

## <a name="purchase-and-billing"></a><span data-ttu-id="1c428-126">Nákup a fakturace</span><span class="sxs-lookup"><span data-stu-id="1c428-126">Purchase and Billing</span></span>
<span data-ttu-id="1c428-127">**Kolik stojí doporučení v době spuštění?**</span><span class="sxs-lookup"><span data-stu-id="1c428-127">**How much does Recommendations cost during the launch period?**</span></span>

<span data-ttu-id="1c428-128">Doporučení je služba na základě předplatného.</span><span class="sxs-lookup"><span data-stu-id="1c428-128">Recommendations is a subscription-based service.</span></span> <span data-ttu-id="1c428-129">Ukládání je založena na objemu transakce za měsíc.</span><span class="sxs-lookup"><span data-stu-id="1c428-129">Charging is based on volume of transactions per month.</span></span> <span data-ttu-id="1c428-130">Můžete zkontrolovat [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) v Microsoft Azure Marketplace informace o cenách.</span><span class="sxs-lookup"><span data-stu-id="1c428-130">You can check the [offer page](https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace for pricing information.</span></span>

<span data-ttu-id="1c428-131">**Existují veškerých nákladech spojených s nutnosti doporučení sledovat a uložit aktivity uživatelů pro mě nejlepší?**</span><span class="sxs-lookup"><span data-stu-id="1c428-131">**Are there any costs associated with having Recommendations track and store user activity for me?**</span></span>

<span data-ttu-id="1c428-132">Není v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="1c428-132">Not at the moment.</span></span>

<span data-ttu-id="1c428-133">**Má doporučení k bezplatné zkušební verzi?**</span><span class="sxs-lookup"><span data-stu-id="1c428-133">**Does Recommendations have a free trial?**</span></span>

<span data-ttu-id="1c428-134">Není k dispozici bezplatná záznam, který je omezen na 10 000 transakce za měsíc.</span><span class="sxs-lookup"><span data-stu-id="1c428-134">There is a free trail which is restricted to 10,000 transactions per month.</span></span>

<span data-ttu-id="1c428-135">**Pokud bude I účtovány poplatky za doporučení?**</span><span class="sxs-lookup"><span data-stu-id="1c428-135">**When will I be billed for Recommendations?**</span></span>

<span data-ttu-id="1c428-136">Placené předplatné je libovolné předplatné, pro kterou je měsíční poplatek.</span><span class="sxs-lookup"><span data-stu-id="1c428-136">A paid subscription is any subscription for which there is a monthly fee.</span></span> <span data-ttu-id="1c428-137">Pokud si koupíte předplatné, jsou okamžitě účtovat pro použití v prvním měsíci.</span><span class="sxs-lookup"><span data-stu-id="1c428-137">When you purchase a paid subscription, you are immediately charged for the first month's use.</span></span> <span data-ttu-id="1c428-138">Budou se účtovat množství, které souvisí s nabídky na stránku odběru služby (a příslušné daně).</span><span class="sxs-lookup"><span data-stu-id="1c428-138">You are charged the amount that is associated with the offer on the subscription page (plus applicable taxes).</span></span> <span data-ttu-id="1c428-139">Dokud zrušit předplatné, provádí tento měsíční poplatek za každý měsíc na stejné datum kalendáře jako původního nákupu.</span><span class="sxs-lookup"><span data-stu-id="1c428-139">This monthly charge is made each month on the same calendar date as your original purchase until you cancel the subscription.</span></span> 

<span data-ttu-id="1c428-140">**Jak upgradovat na vyšší úroveň služby?**</span><span class="sxs-lookup"><span data-stu-id="1c428-140">**How do I upgrade to a higher tier service?**</span></span>

<span data-ttu-id="1c428-141">Můžete koupit nebo aktualizovat vaše předplatné z [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) stránky na webu Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1c428-141">You can buy or update your subscription from the [offer page](https://datamarket.azure.com/dataset/amla/recommendations) page on Microsoft Azure Marketplace.</span></span>

<span data-ttu-id="1c428-142">Při upgradu předplatné:</span><span class="sxs-lookup"><span data-stu-id="1c428-142">When you upgrade a subscription:</span></span>

* <span data-ttu-id="1c428-143">Transakce, které jsou zbývající u původního předplatného nejsou přidány do nového předplatného.</span><span class="sxs-lookup"><span data-stu-id="1c428-143">Transactions that are remaining on your old subscription are not added to your new subscription.</span></span> 
* <span data-ttu-id="1c428-144">Platíte úplné ceny pro nové předplatné, i když můžete mít nepoužívané transakcí u původního předplatného.</span><span class="sxs-lookup"><span data-stu-id="1c428-144">You pay full price for the new subscription, even though you have unused transactions on your old subscription.</span></span>

<span data-ttu-id="1c428-145">Proces upgradu předplatné:</span><span class="sxs-lookup"><span data-stu-id="1c428-145">Process to upgrade a subscription:</span></span>

* <span data-ttu-id="1c428-146">Nevigate k [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="1c428-146">Nevigate to the [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="1c428-147">Pokud už nejste přihlášení, přihlaste se k webu Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1c428-147">Sign in to the Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="1c428-148">V pravém podokně jsou uvedeny všechny dostupné plány.</span><span class="sxs-lookup"><span data-stu-id="1c428-148">In the right pane, all the available plans are listed.</span></span> <span data-ttu-id="1c428-149">Klikněte na přepínač pro plán, který chcete upgradovat.</span><span class="sxs-lookup"><span data-stu-id="1c428-149">Click the radio button for the plan you want to upgrade to.</span></span>
* <span data-ttu-id="1c428-150">Pokud chcete upgradovat, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c428-150">If you want to upgrade, click **OK**.</span></span> <span data-ttu-id="1c428-151">Pokud nechcete upgradovat, klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="1c428-151">If you do not want to upgrade, click **Cancel**.</span></span>

<span data-ttu-id="1c428-152">**Důležité** pečlivě přečtěte dialogové okno před provedením upgradu, protože nejsou k dispozici dopady na fakturaci a použití.</span><span class="sxs-lookup"><span data-stu-id="1c428-152">**Important** Carefully read the dialog box before you upgrade because there are billing and use implications.</span></span>

<span data-ttu-id="1c428-153">**Když se Moje předplatné doporučení ukončí?**</span><span class="sxs-lookup"><span data-stu-id="1c428-153">**When will my subscription to Recommendations end?**</span></span>

<span data-ttu-id="1c428-154">Vaše předplatné se ukončí, pokud ho zrušit.</span><span class="sxs-lookup"><span data-stu-id="1c428-154">Your subscription will end when you cancel it.</span></span> <span data-ttu-id="1c428-155">Pokud chcete zrušit předplatné, přečtěte si následující pokyny.</span><span class="sxs-lookup"><span data-stu-id="1c428-155">If you would like to cancel your subscriptions, see the following instructions.</span></span>

<span data-ttu-id="1c428-156">**Jak zruším Moje předplatné doporučení?**</span><span class="sxs-lookup"><span data-stu-id="1c428-156">**How do I cancel my Recommendations subscription?**</span></span>

<span data-ttu-id="1c428-157">Pokud chcete zrušit předplatné, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="1c428-157">To cancel your subscription, use the following steps.</span></span> <span data-ttu-id="1c428-158">Pokud je vaše aktuální předplatné na placené předplatné, pokračuje vaše předplatné platí až do konce aktuální fakturačního období.</span><span class="sxs-lookup"><span data-stu-id="1c428-158">If your current subscription is a paid subscription, your subscription continues in effect until the end of the current billing period.</span></span> <span data-ttu-id="1c428-159">Pokud potřebujete zrušení být účinné okamžitě, obraťte se na nás na adrese [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="1c428-159">If you need the cancellation to be effective immediately, contact us at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

<span data-ttu-id="1c428-160">**Poznámka:** je zadána žádná náhrada, pokud zrušíte do konce fakturačního období nebo nepoužívané transakcí v fakturačního období.</span><span class="sxs-lookup"><span data-stu-id="1c428-160">**Note** No refund is given if you cancel before the end of a billing period or for unused transactions in a billing period.</span></span>

* <span data-ttu-id="1c428-161">Přejděte na [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="1c428-161">Navigate to the [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="1c428-162">Pokud už nejste přihlášení, přihlaste se k webu Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1c428-162">Sign in to the Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="1c428-163">Klikněte na tlačítko **zrušit** vpravo od názvu datové sady a stav.</span><span class="sxs-lookup"><span data-stu-id="1c428-163">Click **Cancel** to the right of the dataset name and status.</span></span> <span data-ttu-id="1c428-164">Můžete použít tento odběr až do konce fakturačního období aktuální nebo dosažení limit transakcí (cokoliv nastane dříve).</span><span class="sxs-lookup"><span data-stu-id="1c428-164">You can use this subscription until the end of the current billing period or your transaction limit is reached (whichever occurs first).</span></span>

<span data-ttu-id="1c428-165">Pokud chcete zrušit předplatné, okamžitě, takže si můžete zakoupit nové předplatné, soubor lístek v [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="1c428-165">If you would like to cancel your subscription immediately so you can purchase a new subscription, file a ticket at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

## <a name="getting-started-with-recommendations"></a><span data-ttu-id="1c428-166">Začínáme s doporučeními</span><span class="sxs-lookup"><span data-stu-id="1c428-166">Getting started with Recommendations</span></span>
<span data-ttu-id="1c428-167">**Doporučení je pro mě nejlepší?**</span><span class="sxs-lookup"><span data-stu-id="1c428-167">**Is Recommendations for me?**</span></span> 

<span data-ttu-id="1c428-168">Doporučení v Machine Learning je pro organizace a firmy, které jsou závislé na doporučení, která prodává mezi a až prodává produktech či službách zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="1c428-168">Recommendations in Machine Learning is for organizations and businesses that rely on recommendations to cross-sell and up-sell products or services to their customers.</span></span> <span data-ttu-id="1c428-169">Pokud máte web zákazníka přístupem, obchodníci, vnitřní obchodníci nebo volání centra, a Pokud nabízíte katalog více než několik desítek produktů a služeb, do dolní řádku může využívat doporučení.</span><span class="sxs-lookup"><span data-stu-id="1c428-169">If you have a customer-facing website, a sales force, an inside sales force, or a call center, and if you offer a catalog of more than a few dozen products or services, your bottom line may benefit from using Recommendations.</span></span> 

<span data-ttu-id="1c428-170">Experimentování se doporučení je určena pro být celkem jednoduché.</span><span class="sxs-lookup"><span data-stu-id="1c428-170">Experimenting with Recommendations is designed to be fairly simple.</span></span> <span data-ttu-id="1c428-171">Aktuální verze založené na rozhraní API vyžaduje základní znalosti programování.</span><span class="sxs-lookup"><span data-stu-id="1c428-171">The current API-based version requires basic programming skills.</span></span> <span data-ttu-id="1c428-172">Pokud potřebujete pomoc, obraťte se na dodavatele, který vyvinul vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="1c428-172">If you need assistance, contact the vendor who developed your website.</span></span> <span data-ttu-id="1c428-173">Pokud máte interní IT oddělení nebo interní vývojáře, že byste měli mít získat doporučení, jak pro vás.</span><span class="sxs-lookup"><span data-stu-id="1c428-173">If you have an internal IT department or an in-house developer, they should be able to get Recommendations to work for you.</span></span> 

<span data-ttu-id="1c428-174">**Jaké jsou požadavky pro nastavení doporučení?**</span><span class="sxs-lookup"><span data-stu-id="1c428-174">**What are the prerequisites for setting up Recommendations?**</span></span>

<span data-ttu-id="1c428-175">Doporučení vyžaduje, abyste měli protokolu volby uživatele, protože se týká do katalogu.</span><span class="sxs-lookup"><span data-stu-id="1c428-175">Recommendations requires that you have a log of user choices as it relates to your catalog.</span></span> <span data-ttu-id="1c428-176">Pokud nemáte takové protokolu a máte weby zákazníka, můžete doporučení shromáždit aktivity uživatelů za vás.</span><span class="sxs-lookup"><span data-stu-id="1c428-176">If you don't have such a log and you do have a customer facing website, Recommendations can collect user activity for you.</span></span> 

<span data-ttu-id="1c428-177">Doporučení jsou taky vyžaduje katalog produktů a služeb.</span><span class="sxs-lookup"><span data-stu-id="1c428-177">Recommendations also requires a catalog of your products or services.</span></span> <span data-ttu-id="1c428-178">Pokud nemáte katalogu, můžete doporučení použít dat o využití skutečnou zákazníků a generovat katalog.</span><span class="sxs-lookup"><span data-stu-id="1c428-178">If you don't have the catalog, Recommendations can use the actual customer usage data and distill a catalog.</span></span> <span data-ttu-id="1c428-179">Předpokládané katalogu nebude zahrnovat položky, které nejsou hlášeny v rámci uživatelské transakce.</span><span class="sxs-lookup"><span data-stu-id="1c428-179">An implied catalog will not include items that were not reported as part of user transactions.</span></span>

<span data-ttu-id="1c428-180">**Jak nastavit doporučení poprvé?**</span><span class="sxs-lookup"><span data-stu-id="1c428-180">**How do I set up Recommendations for the first time?**</span></span>

<span data-ttu-id="1c428-181">Po [odběru](https://datamarket.azure.com/dataset/amla/recommendations) na doporučení, měli byste použít v dokumentaci rozhraní API [Azure Machine Learning doporučení - úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md) nastavit službu.</span><span class="sxs-lookup"><span data-stu-id="1c428-181">After [subscribing](https://datamarket.azure.com/dataset/amla/recommendations) to Recommendations, you should use the API documentation in the [Azure Machine Learning Recommendations - Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) to set up the service.</span></span>

<span data-ttu-id="1c428-182">**Kde najdu dokumentaci k rozhraní API?**</span><span class="sxs-lookup"><span data-stu-id="1c428-182">**Where can I find API documentation?**</span></span> 

<span data-ttu-id="1c428-183">Dokumentace rozhraní API je [Azure Machine Learning doporučení-úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md).</span><span class="sxs-lookup"><span data-stu-id="1c428-183">The API documentation is [Azure Machine Learning Recommendations -Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).</span></span>

<span data-ttu-id="1c428-184">**Jaké možnosti jsou odeslání katalogu a využití dat do doporučení?**</span><span class="sxs-lookup"><span data-stu-id="1c428-184">**What options do I have to upload catalog and usage data to Recommendations?**</span></span>

<span data-ttu-id="1c428-185">Máte dvě možnosti pro odeslání vaše data o využití a katalogu: můžete exportovat data z systému CRM nebo jiné protokoly a nahrajte ho do doporučení, nebo můžete přidat značky na váš web, který bude sledovat aktivity uživatele.</span><span class="sxs-lookup"><span data-stu-id="1c428-185">You have two options for uploading your catalog and usage data: You can export the data from your CRM system or other logs and upload it to Recommendations, or you can add tags to your website that will track user activities.</span></span> <span data-ttu-id="1c428-186">Pokud používáte metodu druhé, data se uloží v Azure.</span><span class="sxs-lookup"><span data-stu-id="1c428-186">If you use the latter method, the data will be stored in Azure.</span></span>

## <a name="maintenance-and-support"></a><span data-ttu-id="1c428-187">Údržba a podpora</span><span class="sxs-lookup"><span data-stu-id="1c428-187">Maintenance and support</span></span>
<span data-ttu-id="1c428-188">**Jak velká může Moje datové sady být?**</span><span class="sxs-lookup"><span data-stu-id="1c428-188">**How large can my data set be?**</span></span>

<span data-ttu-id="1c428-189">Každá sada dat může obsahovat až 100 000 položek katalogu a až na 2 048 MB dat o využití.</span><span class="sxs-lookup"><span data-stu-id="1c428-189">Each data set can contain up to 100,000 catalog items and up to 2048 MB of usage data.</span></span>
<span data-ttu-id="1c428-190">Kromě toho předplatné může obsahovat až 10 sady dat (modelů).</span><span class="sxs-lookup"><span data-stu-id="1c428-190">In addition, a subscription can contain up to 10 data sets (models).</span></span>

<span data-ttu-id="1c428-191">**Kde lze získat technickou podporu pro doporučení?**</span><span class="sxs-lookup"><span data-stu-id="1c428-191">**Where can I get technical support for Recommendations?**</span></span>

<span data-ttu-id="1c428-192">Technická podpora je k dispozici na [podporu Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) lokality.</span><span class="sxs-lookup"><span data-stu-id="1c428-192">Technical support is available on the [Microsoft Azure Support](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) site.</span></span>

<span data-ttu-id="1c428-193">**Kde podmínky použití**</span><span class="sxs-lookup"><span data-stu-id="1c428-193">**Where can I find the terms of use?**</span></span>

<span data-ttu-id="1c428-194">[Microsoft Azure Machine Learning podmínky doporučení rozhraní API služby](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span><span class="sxs-lookup"><span data-stu-id="1c428-194">[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span></span>

