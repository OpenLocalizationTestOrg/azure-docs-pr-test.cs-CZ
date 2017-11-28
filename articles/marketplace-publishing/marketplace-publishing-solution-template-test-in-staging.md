---
title: "aaaTesting šablony řešení nabízejí pro hello Marketplace | Microsoft Docs"
description: "Pochopte, jak tootest šablony řešení nabízejí pro hello Azure Marketplace."
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
ms.openlocfilehash: 9c195c465d2fc6aa349e4bbcc348e5325f32850d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-solution-template-offer-in-staging"></a><span data-ttu-id="d9430-103">Otestovat vaši nabídku šablony řešení v pracovní</span><span class="sxs-lookup"><span data-stu-id="d9430-103">Test your solution template offer in staging</span></span>
<span data-ttu-id="d9430-104">Pracovní znamená nasazení vaši nabídku v soukromé "izolovaného", kde můžete otestovat a ověřit jeho funkčnost před odesláním ji tooproduction.</span><span class="sxs-lookup"><span data-stu-id="d9430-104">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it tooproduction.</span></span> <span data-ttu-id="d9430-105">Nabídka Hello se zobrazí v pracovním stejně, jako by tooa zákazníkovi, který je nasazena.</span><span class="sxs-lookup"><span data-stu-id="d9430-105">hello offer appears in staging just as it would tooa customer who has deployed it.</span></span> <span data-ttu-id="d9430-106">Vaši nabídku musí být toostaging certifikovaných toobe poslat.</span><span class="sxs-lookup"><span data-stu-id="d9430-106">Your offer must be certified toobe pushed toostaging.</span></span>

<span data-ttu-id="d9430-107">Po hello nabídka je připravený, můžete zobrazit a otestovat hello nabídka v hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d9430-107">After hello offer is staged, you can view and test hello offer in hello [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="d9430-108">Postupujte podle kroků hello toopush vaše nabídka toostaging a testování v hello [portálu Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="d9430-108">Follow hello steps below toopush your offer toostaging and test it in hello [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="d9430-109">Přejděte toohello [publikování portál](https://publish.windowsazure.com) > **šablony řešení** kartě > vaši nabídku > **publikovat** > **Push tooStaging** .</span><span class="sxs-lookup"><span data-stu-id="d9430-109">Go toohello [Publishing Portal](https://publish.windowsazure.com) > **Solution Templates** tab > your offer > **Publish** > **Push tooStaging**.</span></span>
2. <span data-ttu-id="d9430-110">Zadejte hello seznam předplatných Azure, že budete používat toopreview a otestovat vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="d9430-110">Provide hello list of Azure subscriptions that you will use toopreview and test your offer.</span></span>
3. <span data-ttu-id="d9430-111">Přihlaste se na portálu Azure preview toohello pomocí hello ID odběru, který jste použili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="d9430-111">Sign in toohello Azure preview portal by using hello subscription ID that you used in hello previous step.</span></span>
4. <span data-ttu-id="d9430-112">Provádění alespoň jeden zaokrouhlit testování na portálu Azure preview hello na hello bodů uvedených níže:</span><span class="sxs-lookup"><span data-stu-id="d9430-112">Carry out at least one round of testing in hello Azure preview portal on hello points mentioned below:</span></span>
   * <span data-ttu-id="d9430-113">Ujistěte se, že marketingové obsah se zobrazí správně v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d9430-113">Make sure that marketing content shows up correctly in hello Azure Marketplace.</span></span>
   * <span data-ttu-id="d9430-114">Koncové nasazení hello topologie.</span><span class="sxs-lookup"><span data-stu-id="d9430-114">End-to-end deployment of hello topology.</span></span>
   * <span data-ttu-id="d9430-115">Proveďte test výkonnosti a zátěžové testování.</span><span class="sxs-lookup"><span data-stu-id="d9430-115">Perform performance testing and stress testing.</span></span>
   * <span data-ttu-id="d9430-116">Ujistěte se, že vaše topologie dodržuje osvědčené postupy toohello.</span><span class="sxs-lookup"><span data-stu-id="d9430-116">Ensure that your topology adheres toohello best practices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9430-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9430-117">Next steps</span></span>
<span data-ttu-id="d9430-118">Pokud budete spokojeni s výsledky hello, pak můžete pokračovat toohello konečné nabídka publikování fázi **krok 4**: [nasazení vaší toohello nabídku Marketplace](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="d9430-118">If you are satisfied with hello results, then you can proceed toohello final offer publishing phase, **Step 4**:  [Deploying your offer toohello Marketplace](marketplace-publishing-push-to-production.md).</span></span> <span data-ttu-id="d9430-119">Jinak proveďte změny v vaši nabídku a požádat o certifikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="d9430-119">Otherwise, make changes in your offer and request certification again.</span></span>

> [!NOTE]
> <span data-ttu-id="d9430-120">Certifikace pro marketing změny obsahu, není nutné.</span><span class="sxs-lookup"><span data-stu-id="d9430-120">For marketing content changes, certification is not required.</span></span>
> 
> 

<span data-ttu-id="d9430-121">V tématu [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md) pro úlohy Průvodce tooall vydavatele.</span><span class="sxs-lookup"><span data-stu-id="d9430-121">See [Getting started: How toopublish an offer toohello Azure Marketplace](marketplace-publishing-getting-started.md) for a guide tooall publisher tasks.</span></span>

