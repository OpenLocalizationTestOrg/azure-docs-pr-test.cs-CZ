---
title: "Testování vaši nabídku šablony řešení pro Marketplace | Microsoft Docs"
description: "Pochopit, jak otestovat vaši nabídku řešení šablony pro Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: ef8f9b5e-b98c-49f3-913f-cdf772c14c12
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/04/2015
ms.author: hascipio; v-divte
ms.openlocfilehash: da1fc4713fd1d832c7ba91226f72cbef63b241bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="test-your-solution-template-offer-in-staging"></a><span data-ttu-id="9bbcf-103">Otestovat vaši nabídku šablony řešení v pracovní</span><span class="sxs-lookup"><span data-stu-id="9bbcf-103">Test your solution template offer in staging</span></span>
<span data-ttu-id="9bbcf-104">Pracovní znamená nasazení vaši nabídku v soukromé "izolovaného", kde můžete otestovat a ověřit jeho funkčnost před odesláním do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-104">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it to production.</span></span> <span data-ttu-id="9bbcf-105">Nabídka se zobrazí v pracovním stejně, jako by zákazníkovi, který je nasazena.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-105">The offer appears in staging just as it would to a customer who has deployed it.</span></span> <span data-ttu-id="9bbcf-106">Chcete-li být vložena do přípravy, musí mít certifikovanou vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-106">Your offer must be certified to be pushed to staging.</span></span>

<span data-ttu-id="9bbcf-107">Po nabídka je připravený, můžete zobrazit a otestovat nabídku v [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9bbcf-107">After the offer is staged, you can view and test the offer in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="9bbcf-108">Použijte následující postup push vaši nabídku pro pracovní a otestovat ho [portálu Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="9bbcf-108">Follow the steps below to push your offer to staging and test it in the [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="9bbcf-109">Přejděte na [publikování portál](https://publish.windowsazure.com) > **šablony řešení** kartě > vaši nabídku > **publikovat** > **nabízená pracovní**.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-109">Go to the [Publishing Portal](https://publish.windowsazure.com) > **Solution Templates** tab > your offer > **Publish** > **Push to Staging**.</span></span>
2. <span data-ttu-id="9bbcf-110">Zadejte seznam předplatných Azure, které budete používat pro zobrazení náhledu a otestovat vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-110">Provide the list of Azure subscriptions that you will use to preview and test your offer.</span></span>
3. <span data-ttu-id="9bbcf-111">Přihlaste se k portálu Azure preview pomocí ID odběru, který jste použili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-111">Sign in to the Azure preview portal by using the subscription ID that you used in the previous step.</span></span>
4. <span data-ttu-id="9bbcf-112">Provádění alespoň jeden zaokrouhlit testování na portálu Azure preview na bodech uvedených níže:</span><span class="sxs-lookup"><span data-stu-id="9bbcf-112">Carry out at least one round of testing in the Azure preview portal on the points mentioned below:</span></span>
   * <span data-ttu-id="9bbcf-113">Ujistěte se, že marketingové obsah se zobrazí správně v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-113">Make sure that marketing content shows up correctly in the Azure Marketplace.</span></span>
   * <span data-ttu-id="9bbcf-114">Topologie nasazení začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-114">End-to-end deployment of the topology.</span></span>
   * <span data-ttu-id="9bbcf-115">Proveďte test výkonnosti a zátěžové testování.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-115">Perform performance testing and stress testing.</span></span>
   * <span data-ttu-id="9bbcf-116">Ujistěte se, že vaše topologie dodržuje osvědčené postupy.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-116">Ensure that your topology adheres to the best practices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bbcf-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bbcf-117">Next steps</span></span>
<span data-ttu-id="9bbcf-118">Pokud jste s výsledky spokojeni, pak můžete přejít k publikování fázi konečné nabídka **krok 4**: [nasazení vaši nabídku Marketplace](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="9bbcf-118">If you are satisfied with the results, then you can proceed to the final offer publishing phase, **Step 4**:  [Deploying your offer to the Marketplace](marketplace-publishing-push-to-production.md).</span></span> <span data-ttu-id="9bbcf-119">Jinak proveďte změny v vaši nabídku a požádat o certifikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-119">Otherwise, make changes in your offer and request certification again.</span></span>

> [!NOTE]
> <span data-ttu-id="9bbcf-120">Certifikace pro marketing změny obsahu, není nutné.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-120">For marketing content changes, certification is not required.</span></span>
> 
> 

<span data-ttu-id="9bbcf-121">V tématu [Začínáme: postup publikování nabídky pro Azure Marketplace](marketplace-publishing-getting-started.md) průvodce všechny úlohy vydavatele.</span><span class="sxs-lookup"><span data-stu-id="9bbcf-121">See [Getting started: How to publish an offer to the Azure Marketplace](marketplace-publishing-getting-started.md) for a guide to all publisher tasks.</span></span>

