---
title: "aaaSet nahoru a používání hello Machine Learning doporučení API | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 980bf1a36f3291275d9ef0fee9b4446f7e0cbecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a><span data-ttu-id="c5771-103">Nejčastější dotazy k nastavení a používání rozhraní Machine Learning Recommendations API</span><span class="sxs-lookup"><span data-stu-id="c5771-103">Setting up and using Machine Learning Recommendations API FAQ</span></span>
<span data-ttu-id="c5771-104">**Co je doporučení?**</span><span class="sxs-lookup"><span data-stu-id="c5771-104">**What is RECOMMENDATIONS?**</span></span>

> [!NOTE]
> <span data-ttu-id="c5771-105">Měli byste začít používat hello kognitivní služby API doporučení místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="c5771-105">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="c5771-106">Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="c5771-106">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="c5771-107">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="c5771-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="c5771-108">Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="c5771-108">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="c5771-109">Pro organizace a firmy, které jsou závislé na doporučení toocross prodává a až prodává produktů a služeb tootheir zákazníků doporučení v Azure Machine Learning poskytuje modul Samoobslužné služby doporučení.</span><span class="sxs-lookup"><span data-stu-id="c5771-109">For organizations and businesses that rely on recommendations toocross-sell and up-sell products and services tootheir customers, RECOMMENDATIONS in Azure Machine Learning provides a self-service recommendations engine.</span></span> <span data-ttu-id="c5771-110">Je implementace spolupráce filtrování, která používá matice factorization jako algoritmus jádra.</span><span class="sxs-lookup"><span data-stu-id="c5771-110">It is an implementation of collaborative filtering that uses matrix factorization as its core algorithm.</span></span> <span data-ttu-id="c5771-111">Vývojáři aplikace přístup pomocí rozhraní REST API doporučení.</span><span class="sxs-lookup"><span data-stu-id="c5771-111">Application developers can access RECOMMENDATIONS by using REST APIs.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="c5771-112">**Co můžete dělat s DOPORUČENÍMI?**</span><span class="sxs-lookup"><span data-stu-id="c5771-112">**What can I do with RECOMMENDATIONS?**</span></span>

<span data-ttu-id="c5771-113">DOPORUČENÍ používá jako vstup některou položku nebo sadu položek a vrátí seznam hodnot příslušná doporučení.</span><span class="sxs-lookup"><span data-stu-id="c5771-113">RECOMMENDATIONS takes as input an item or a set of items and returns a list of relevant recommendations.</span></span> <span data-ttu-id="c5771-114">Příklad: zákazník online prodejce klikne produktu.</span><span class="sxs-lookup"><span data-stu-id="c5771-114">For example: A customer of an online retailer clicks a product.</span></span> <span data-ttu-id="c5771-115">online prodejce Hello odešle jako vstupní tooRECOMMENDATIONS produktu, získá seznam produktů na oplátku a rozhodne, která z těchto produktů se zobrazí toohello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c5771-115">hello online retailer sends that product as input tooRECOMMENDATIONS, gets a list of products in return, and decides which of these products will be shown toohello customer.</span></span> <span data-ttu-id="c5771-116">Můžete chtít toouse doporučení toooptimize online úložiště nebo dokonce i tooinform vaše uvnitř prodejní oddělení nebo volání center.</span><span class="sxs-lookup"><span data-stu-id="c5771-116">You may want toouse RECOMMENDATIONS toooptimize your online store or even tooinform your inside sales department or call center.</span></span>

<span data-ttu-id="c5771-117">**Existují nějaká omezení využití?**</span><span class="sxs-lookup"><span data-stu-id="c5771-117">**Are there any usage limitations?**</span></span>

<span data-ttu-id="c5771-118">Doporučení má následující omezení využití hello:</span><span class="sxs-lookup"><span data-stu-id="c5771-118">Recommendations has hello following usage limitations:</span></span>

* <span data-ttu-id="c5771-119">Maximální počet modelů podle předplatného: 10</span><span class="sxs-lookup"><span data-stu-id="c5771-119">Maximum number of models per subscription: 10</span></span>
* <span data-ttu-id="c5771-120">Maximální počet položek, které mohou být uloženy katalog: 100 000</span><span class="sxs-lookup"><span data-stu-id="c5771-120">Maximum number of items that a catalog can hold: 100,000</span></span>
* <span data-ttu-id="c5771-121">maximální počet bodů využití, které jsou zachovány Hello je ~ 5 000 000.</span><span class="sxs-lookup"><span data-stu-id="c5771-121">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="c5771-122">Pokud nové se nahrál nebo nahlásí se odstraní nejstarší Hello.</span><span class="sxs-lookup"><span data-stu-id="c5771-122">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="c5771-123">Maximální velikost dat, který lze poslat e-mailem (například import katalogu dat, data o využití import) je 200 MB</span><span class="sxs-lookup"><span data-stu-id="c5771-123">Maximum size of data that can be sent in email (for example, import catalog data, import usage data) is 200 MB</span></span>
* <span data-ttu-id="c5771-124">Počet transakcí za sekundu (TPS) pro sestavení modelu doporučení, která není aktivní je TPS ~ 2.</span><span class="sxs-lookup"><span data-stu-id="c5771-124">Number of transactions per second (TPS) for a Recommendations model build that is not active is ~2 TPS.</span></span> <span data-ttu-id="c5771-125">Až too20 TPS mohou být uloženy sestavení modelu doporučení, která je aktivní.</span><span class="sxs-lookup"><span data-stu-id="c5771-125">A Recommendations model build that is active can hold up too20 TPS.</span></span>

## <a name="purchase-and-billing"></a><span data-ttu-id="c5771-126">Nákup a fakturace</span><span class="sxs-lookup"><span data-stu-id="c5771-126">Purchase and Billing</span></span>
<span data-ttu-id="c5771-127">**Kolik doporučení náklady během doby spuštění hello?**</span><span class="sxs-lookup"><span data-stu-id="c5771-127">**How much does Recommendations cost during hello launch period?**</span></span>

<span data-ttu-id="c5771-128">Doporučení je služba na základě předplatného.</span><span class="sxs-lookup"><span data-stu-id="c5771-128">Recommendations is a subscription-based service.</span></span> <span data-ttu-id="c5771-129">Ukládání je založena na objemu transakce za měsíc.</span><span class="sxs-lookup"><span data-stu-id="c5771-129">Charging is based on volume of transactions per month.</span></span> <span data-ttu-id="c5771-130">Můžete zkontrolovat hello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) v Microsoft Azure Marketplace informace o cenách.</span><span class="sxs-lookup"><span data-stu-id="c5771-130">You can check hello [offer page](https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace for pricing information.</span></span>

<span data-ttu-id="c5771-131">**Existují veškerých nákladech spojených s nutnosti doporučení sledovat a uložit aktivity uživatelů pro mě nejlepší?**</span><span class="sxs-lookup"><span data-stu-id="c5771-131">**Are there any costs associated with having Recommendations track and store user activity for me?**</span></span>

<span data-ttu-id="c5771-132">Není momentálně hello.</span><span class="sxs-lookup"><span data-stu-id="c5771-132">Not at hello moment.</span></span>

<span data-ttu-id="c5771-133">**Má doporučení k bezplatné zkušební verzi?**</span><span class="sxs-lookup"><span data-stu-id="c5771-133">**Does Recommendations have a free trial?**</span></span>

<span data-ttu-id="c5771-134">Není k dispozici bezplatná záznam, který je s omezeným přístupem too10 000 transakce za měsíc.</span><span class="sxs-lookup"><span data-stu-id="c5771-134">There is a free trail which is restricted too10,000 transactions per month.</span></span>

<span data-ttu-id="c5771-135">**Pokud bude I účtovány poplatky za doporučení?**</span><span class="sxs-lookup"><span data-stu-id="c5771-135">**When will I be billed for Recommendations?**</span></span>

<span data-ttu-id="c5771-136">Placené předplatné je libovolné předplatné, pro kterou je měsíční poplatek.</span><span class="sxs-lookup"><span data-stu-id="c5771-136">A paid subscription is any subscription for which there is a monthly fee.</span></span> <span data-ttu-id="c5771-137">Pokud si koupíte předplatné, okamžitě účtujeme pro hello nejprve použijte měsíci.</span><span class="sxs-lookup"><span data-stu-id="c5771-137">When you purchase a paid subscription, you are immediately charged for hello first month's use.</span></span> <span data-ttu-id="c5771-138">Budou se účtovat hello množství, které souvisí s hello nabídky na stránku odběru hello (a příslušné daně).</span><span class="sxs-lookup"><span data-stu-id="c5771-138">You are charged hello amount that is associated with hello offer on hello subscription page (plus applicable taxes).</span></span> <span data-ttu-id="c5771-139">Tento měsíční poplatek je probíhají každý měsíc hello stejná data jako původního nákupu kalendář až do zrušení předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="c5771-139">This monthly charge is made each month on hello same calendar date as your original purchase until you cancel hello subscription.</span></span> 

<span data-ttu-id="c5771-140">**Jak mohu provést upgrade tooa vyšší úroveň služby?**</span><span class="sxs-lookup"><span data-stu-id="c5771-140">**How do I upgrade tooa higher tier service?**</span></span>

<span data-ttu-id="c5771-141">Můžete koupit nebo aktualizovat vaše předplatné z hello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) stránky na webu Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c5771-141">You can buy or update your subscription from hello [offer page](https://datamarket.azure.com/dataset/amla/recommendations) page on Microsoft Azure Marketplace.</span></span>

<span data-ttu-id="c5771-142">Při upgradu předplatné:</span><span class="sxs-lookup"><span data-stu-id="c5771-142">When you upgrade a subscription:</span></span>

* <span data-ttu-id="c5771-143">Transakce, které jsou zbývající u původního předplatného nebyly přidány tooyour nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="c5771-143">Transactions that are remaining on your old subscription are not added tooyour new subscription.</span></span> 
* <span data-ttu-id="c5771-144">Platíte úplné ceny hello nové předplatné, i když můžete mít nepoužívané transakcí u původního předplatného.</span><span class="sxs-lookup"><span data-stu-id="c5771-144">You pay full price for hello new subscription, even though you have unused transactions on your old subscription.</span></span>

<span data-ttu-id="c5771-145">Proces tooupgrade předplatné:</span><span class="sxs-lookup"><span data-stu-id="c5771-145">Process tooupgrade a subscription:</span></span>

* <span data-ttu-id="c5771-146">Nevigate toohello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="c5771-146">Nevigate toohello [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="c5771-147">Pokud už nejste přihlášení, přihlaste toohello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c5771-147">Sign in toohello Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="c5771-148">V pravém podokně hello jsou uvedeny všechny dostupné plány hello.</span><span class="sxs-lookup"><span data-stu-id="c5771-148">In hello right pane, all hello available plans are listed.</span></span> <span data-ttu-id="c5771-149">Klikněte na přepínač hello pro hello plánu, který chcete tooupgrade k.</span><span class="sxs-lookup"><span data-stu-id="c5771-149">Click hello radio button for hello plan you want tooupgrade to.</span></span>
* <span data-ttu-id="c5771-150">Pokud chcete tooupgrade, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5771-150">If you want tooupgrade, click **OK**.</span></span> <span data-ttu-id="c5771-151">Pokud nechcete, aby tooupgrade, klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="c5771-151">If you do not want tooupgrade, click **Cancel**.</span></span>

<span data-ttu-id="c5771-152">**Důležité** pečlivě čtení hello dialogové okno před provedením upgradu, protože nejsou k dispozici dopady na fakturaci a použití.</span><span class="sxs-lookup"><span data-stu-id="c5771-152">**Important** Carefully read hello dialog box before you upgrade because there are billing and use implications.</span></span>

<span data-ttu-id="c5771-153">**Když se ukončí tooRecommendations Moje předplatné?**</span><span class="sxs-lookup"><span data-stu-id="c5771-153">**When will my subscription tooRecommendations end?**</span></span>

<span data-ttu-id="c5771-154">Vaše předplatné se ukončí, pokud ho zrušit.</span><span class="sxs-lookup"><span data-stu-id="c5771-154">Your subscription will end when you cancel it.</span></span> <span data-ttu-id="c5771-155">Pokud chcete toocancel vašich předplatných, najdete v části hello pokynů.</span><span class="sxs-lookup"><span data-stu-id="c5771-155">If you would like toocancel your subscriptions, see hello following instructions.</span></span>

<span data-ttu-id="c5771-156">**Jak zruším Moje předplatné doporučení?**</span><span class="sxs-lookup"><span data-stu-id="c5771-156">**How do I cancel my Recommendations subscription?**</span></span>

<span data-ttu-id="c5771-157">toocancel, které vaše předplatné hello použijte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="c5771-157">toocancel your subscription, use hello following steps.</span></span> <span data-ttu-id="c5771-158">Pokud je vaše aktuální předplatné na placené předplatné, vaše předplatné pokračuje v platnosti až hello konce hello aktuální fakturační období.</span><span class="sxs-lookup"><span data-stu-id="c5771-158">If your current subscription is a paid subscription, your subscription continues in effect until hello end of hello current billing period.</span></span> <span data-ttu-id="c5771-159">Pokud třeba hello zrušení toobe efektivní okamžitě, obraťte se na nás na adrese [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="c5771-159">If you need hello cancellation toobe effective immediately, contact us at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

<span data-ttu-id="c5771-160">**Poznámka:** je zadána žádná náhrada, pokud zrušíte hello konce fakturačního období nebo nepoužívané transakcí v fakturačního období.</span><span class="sxs-lookup"><span data-stu-id="c5771-160">**Note** No refund is given if you cancel before hello end of a billing period or for unused transactions in a billing period.</span></span>

* <span data-ttu-id="c5771-161">Přejděte toohello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="c5771-161">Navigate toohello [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="c5771-162">Pokud už nejste přihlášení, přihlaste toohello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c5771-162">Sign in toohello Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="c5771-163">Klikněte na tlačítko **zrušit** toohello napravo od hello název datové sady a stav.</span><span class="sxs-lookup"><span data-stu-id="c5771-163">Click **Cancel** toohello right of hello dataset name and status.</span></span> <span data-ttu-id="c5771-164">Toto předplatné můžete používat, dokud je dosaženo hello konce hello aktuální fakturační období nebo limit transakcí (cokoliv nastane dříve).</span><span class="sxs-lookup"><span data-stu-id="c5771-164">You can use this subscription until hello end of hello current billing period or your transaction limit is reached (whichever occurs first).</span></span>

<span data-ttu-id="c5771-165">Pokud chcete toocancel vaše předplatné, okamžitě, takže si můžete zakoupit nové předplatné, soubor lístek v [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="c5771-165">If you would like toocancel your subscription immediately so you can purchase a new subscription, file a ticket at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

## <a name="getting-started-with-recommendations"></a><span data-ttu-id="c5771-166">Začínáme s doporučeními</span><span class="sxs-lookup"><span data-stu-id="c5771-166">Getting started with Recommendations</span></span>
<span data-ttu-id="c5771-167">**Doporučení je pro mě nejlepší?**</span><span class="sxs-lookup"><span data-stu-id="c5771-167">**Is Recommendations for me?**</span></span> 

<span data-ttu-id="c5771-168">Doporučení v Machine Learning je pro organizace a firmy, které jsou závislé na doporučení toocross prodává až prodává produktech či službách tootheir zákazníky a.</span><span class="sxs-lookup"><span data-stu-id="c5771-168">Recommendations in Machine Learning is for organizations and businesses that rely on recommendations toocross-sell and up-sell products or services tootheir customers.</span></span> <span data-ttu-id="c5771-169">Pokud máte web zákazníka přístupem, obchodníci, vnitřní obchodníci nebo volání centra, a Pokud nabízíte katalog více než několik desítek produktů a služeb, do dolní řádku může využívat doporučení.</span><span class="sxs-lookup"><span data-stu-id="c5771-169">If you have a customer-facing website, a sales force, an inside sales force, or a call center, and if you offer a catalog of more than a few dozen products or services, your bottom line may benefit from using Recommendations.</span></span> 

<span data-ttu-id="c5771-170">Experimentování se doporučení je navrženou toobe docela jednoduché.</span><span class="sxs-lookup"><span data-stu-id="c5771-170">Experimenting with Recommendations is designed toobe fairly simple.</span></span> <span data-ttu-id="c5771-171">aktuální verze založené na rozhraní API Hello vyžaduje základní znalosti programování.</span><span class="sxs-lookup"><span data-stu-id="c5771-171">hello current API-based version requires basic programming skills.</span></span> <span data-ttu-id="c5771-172">Pokud potřebujete pomoc, obraťte se na dodavatele hello, který vyvinul vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="c5771-172">If you need assistance, contact hello vendor who developed your website.</span></span> <span data-ttu-id="c5771-173">Pokud máte interní IT oddělení nebo interní vývojáře, měly by být možné tooget toowork doporučení pro vás.</span><span class="sxs-lookup"><span data-stu-id="c5771-173">If you have an internal IT department or an in-house developer, they should be able tooget Recommendations toowork for you.</span></span> 

<span data-ttu-id="c5771-174">**Jaké jsou požadavky hello k nastavení doporučení?**</span><span class="sxs-lookup"><span data-stu-id="c5771-174">**What are hello prerequisites for setting up Recommendations?**</span></span>

<span data-ttu-id="c5771-175">Doporučení vyžaduje, abyste měli protokolu volby uživatele, protože se týká tooyour katalogu.</span><span class="sxs-lookup"><span data-stu-id="c5771-175">Recommendations requires that you have a log of user choices as it relates tooyour catalog.</span></span> <span data-ttu-id="c5771-176">Pokud nemáte takové protokolu a máte weby zákazníka, můžete doporučení shromáždit aktivity uživatelů za vás.</span><span class="sxs-lookup"><span data-stu-id="c5771-176">If you don't have such a log and you do have a customer facing website, Recommendations can collect user activity for you.</span></span> 

<span data-ttu-id="c5771-177">Doporučení jsou taky vyžaduje katalog produktů a služeb.</span><span class="sxs-lookup"><span data-stu-id="c5771-177">Recommendations also requires a catalog of your products or services.</span></span> <span data-ttu-id="c5771-178">Pokud nemáte hello katalogu, můžete doporučení použít data o využití hello skutečnou zákazníků a generovat katalog.</span><span class="sxs-lookup"><span data-stu-id="c5771-178">If you don't have hello catalog, Recommendations can use hello actual customer usage data and distill a catalog.</span></span> <span data-ttu-id="c5771-179">Předpokládané katalogu nebude zahrnovat položky, které nejsou hlášeny v rámci uživatelské transakce.</span><span class="sxs-lookup"><span data-stu-id="c5771-179">An implied catalog will not include items that were not reported as part of user transactions.</span></span>

<span data-ttu-id="c5771-180">**Jak nastavím doporučení pro hello poprvé?**</span><span class="sxs-lookup"><span data-stu-id="c5771-180">**How do I set up Recommendations for hello first time?**</span></span>

<span data-ttu-id="c5771-181">Po [odběru](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, měli byste použít dokumentaci hello rozhraní API v hello [Azure Machine Learning doporučení - úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md) tooset až hello služby.</span><span class="sxs-lookup"><span data-stu-id="c5771-181">After [subscribing](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, you should use hello API documentation in hello [Azure Machine Learning Recommendations - Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) tooset up hello service.</span></span>

<span data-ttu-id="c5771-182">**Kde najdu dokumentaci k rozhraní API?**</span><span class="sxs-lookup"><span data-stu-id="c5771-182">**Where can I find API documentation?**</span></span> 

<span data-ttu-id="c5771-183">Hello dokumentaci k rozhraní API je [Azure Machine Learning doporučení-úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md).</span><span class="sxs-lookup"><span data-stu-id="c5771-183">hello API documentation is [Azure Machine Learning Recommendations -Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).</span></span>

<span data-ttu-id="c5771-184">**Co dělat možnosti je nutné tooRecommendations tooupload katalogu a využití dat?**</span><span class="sxs-lookup"><span data-stu-id="c5771-184">**What options do I have tooupload catalog and usage data tooRecommendations?**</span></span>

<span data-ttu-id="c5771-185">Máte dvě možnosti pro odeslání vaše data o využití a katalogu: můžete exportovat hello data ze systému CRM nebo jiné protokoly a nahrajte ho tooRecommendations, nebo můžete přidat web tooyour značky, který bude sledovat aktivity uživatele.</span><span class="sxs-lookup"><span data-stu-id="c5771-185">You have two options for uploading your catalog and usage data: You can export hello data from your CRM system or other logs and upload it tooRecommendations, or you can add tags tooyour website that will track user activities.</span></span> <span data-ttu-id="c5771-186">Pokud používáte hello druhá metoda, hello data se uloží v Azure.</span><span class="sxs-lookup"><span data-stu-id="c5771-186">If you use hello latter method, hello data will be stored in Azure.</span></span>

## <a name="maintenance-and-support"></a><span data-ttu-id="c5771-187">Údržba a podpora</span><span class="sxs-lookup"><span data-stu-id="c5771-187">Maintenance and support</span></span>
<span data-ttu-id="c5771-188">**Jak velká může Moje datové sady být?**</span><span class="sxs-lookup"><span data-stu-id="c5771-188">**How large can my data set be?**</span></span>

<span data-ttu-id="c5771-189">Každá sada dat může obsahovat až too100 000 položek katalogu a až too2048 MB dat o využití.</span><span class="sxs-lookup"><span data-stu-id="c5771-189">Each data set can contain up too100,000 catalog items and up too2048 MB of usage data.</span></span>
<span data-ttu-id="c5771-190">Kromě toho předplatné může obsahovat až too10 datové sady (modelů).</span><span class="sxs-lookup"><span data-stu-id="c5771-190">In addition, a subscription can contain up too10 data sets (models).</span></span>

<span data-ttu-id="c5771-191">**Kde lze získat technickou podporu pro doporučení?**</span><span class="sxs-lookup"><span data-stu-id="c5771-191">**Where can I get technical support for Recommendations?**</span></span>

<span data-ttu-id="c5771-192">Technická podpora je k dispozici na hello [podporu Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) lokality.</span><span class="sxs-lookup"><span data-stu-id="c5771-192">Technical support is available on hello [Microsoft Azure Support](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) site.</span></span>

<span data-ttu-id="c5771-193">**Kde hello podmínky použití**</span><span class="sxs-lookup"><span data-stu-id="c5771-193">**Where can I find hello terms of use?**</span></span>

<span data-ttu-id="c5771-194">[Microsoft Azure Machine Learning podmínky doporučení rozhraní API služby](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span><span class="sxs-lookup"><span data-stu-id="c5771-194">[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span></span>

