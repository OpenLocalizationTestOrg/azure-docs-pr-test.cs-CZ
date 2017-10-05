---
title: "Vytvořte partnery pro business-to-business (B2B) zprávy – Azure Logic Apps | Microsoft Docs"
description: "Informace o postupu přidání partnery ke svému účtu integrace s Logic Apps a Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a><span data-ttu-id="10f3f-103">Přidat nebo aktualizovat partnery ve smlouvách business-to-business v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="10f3f-103">Add or update partners in business-to-business agreements in your workflow</span></span>

<span data-ttu-id="10f3f-104">Partneři jsou entity, které se účastní transakce (B2B) business-to-business výměnu zpráv mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="10f3f-104">Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other.</span></span> <span data-ttu-id="10f3f-105">Před vytvořením partnery, které představují jste a jiné organizaci v tyto transakce, musí oba sdílet informace, které identifikuje a ověří zprávy odeslané podle sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="10f3f-105">Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other.</span></span> <span data-ttu-id="10f3f-106">Po popisují tyto podrobnosti a budete připravení zahájit relaci vaší firmy, můžete vytvořit partnerům ve vašem účtu integrace představují oba.</span><span class="sxs-lookup"><span data-stu-id="10f3f-106">After you discuss these details and are ready to start your business relationship, you can create partners in your integration account to represent you both.</span></span>

## <a name="what-roles-do-partners-have-in-your-integration-account"></a><span data-ttu-id="10f3f-107">Ve vašem účtu integrace jaké role mají partnery?</span><span class="sxs-lookup"><span data-stu-id="10f3f-107">What roles do partners have in your integration account?</span></span>

<span data-ttu-id="10f3f-108">Zadat podrobnosti o zprávy vyměňují mezi partnery, vytvořte smlouvy mezi těmito partnery.</span><span class="sxs-lookup"><span data-stu-id="10f3f-108">To define details about the messages exchanged between partners, you create agreements between those partners.</span></span> <span data-ttu-id="10f3f-109">Ale předtím, než vytvoříte smlouvu, je třeba přidat alespoň dva partneři ke svému účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="10f3f-109">However, before you can create an agreement, you must have added at least two partners to your integration account.</span></span> <span data-ttu-id="10f3f-110">Vaše organizace musí být součástí smlouvy jako **hostitele partnera**.</span><span class="sxs-lookup"><span data-stu-id="10f3f-110">Your organization must be part of the agreement as the **host partner**.</span></span> <span data-ttu-id="10f3f-111">Druhého partnera nebo **hosta partnera** představuje organizace, která výměny zpráv ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="10f3f-111">The other partner, or **guest partner** represents the organization that exchanges messages with your organization.</span></span> <span data-ttu-id="10f3f-112">Partner hostů může být jiné společnosti nebo i oddělení ve vaší vlastní organizaci.</span><span class="sxs-lookup"><span data-stu-id="10f3f-112">The guest partner can be another company, or even a department in your own organization.</span></span>

<span data-ttu-id="10f3f-113">Po přidání těchto partnerů, můžete vytvořit smlouvu.</span><span class="sxs-lookup"><span data-stu-id="10f3f-113">After you add these partners, you can create an agreement.</span></span>

<span data-ttu-id="10f3f-114">Příjem a odesílání nastavení jsou orientované z hlediska hostované partnera.</span><span class="sxs-lookup"><span data-stu-id="10f3f-114">Receive and Send settings are oriented from the point of view of the Hosted Partner.</span></span> <span data-ttu-id="10f3f-115">Nastavení škálování v smlouvu například určit, jak hostované partnera obdrží zpráv odeslaných z hosta partnera.</span><span class="sxs-lookup"><span data-stu-id="10f3f-115">For example, the receive settings in an agreement determine how the hosted partner receives messages sent from a guest partner.</span></span> <span data-ttu-id="10f3f-116">Nastavení odesílání na smlouvě, uvádí, jak hostované partnera odešle zprávy do hostovaného partnera.</span><span class="sxs-lookup"><span data-stu-id="10f3f-116">Likewise, the send settings on the agreement indicate how the hosted partner sends messages to the guest partner.</span></span>

## <a name="how-to-create-a-partner"></a><span data-ttu-id="10f3f-117">Postup vytvoření partnera?</span><span class="sxs-lookup"><span data-stu-id="10f3f-117">How to create a partner?</span></span>

1. <span data-ttu-id="10f3f-118">Na portálu Azure vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="10f3f-118">In the Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="10f3f-119">Do vyhledávacího pole filtru zadejte **integrace**, pak vyberte **účty pro integraci** v seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="10f3f-119">In the filter search box, enter **integration**, then select **Integration Accounts** in the results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="10f3f-120">Vyberte účet integrace, ve které chcete přidat vaši partneři.</span><span class="sxs-lookup"><span data-stu-id="10f3f-120">Select the integration account where you want to add your partners.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="10f3f-121">Vyberte **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="10f3f-121">Select the **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. <span data-ttu-id="10f3f-122">V okně partnery zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="10f3f-122">In the Partners blade, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. <span data-ttu-id="10f3f-123">Zadejte název svého partnera, a pak vyberte **kvalifikátor**.</span><span class="sxs-lookup"><span data-stu-id="10f3f-123">Enter a name for your partner, then select a **Qualifier**.</span></span> <span data-ttu-id="10f3f-124">Nakonec zadejte **hodnotu** k identifikaci dokumenty, které jsou do svých aplikací.</span><span class="sxs-lookup"><span data-stu-id="10f3f-124">Finally, enter a **Value** to help identify documents that come into your apps.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. <span data-ttu-id="10f3f-125">Chcete-li sledovat průběh pro proces vytváření partnera, vyberte *zvonku* ikonu oznámení.</span><span class="sxs-lookup"><span data-stu-id="10f3f-125">To see the progress for your partner creation process, select the *bell* notification icon.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. <span data-ttu-id="10f3f-126">Chcete-li potvrdit, že byly úspěšně přidány nové partnerům, vyberte **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="10f3f-126">To confirm that your new partners were successfully added, select the **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    <span data-ttu-id="10f3f-127">Po výběru dlaždice partnery, se zobrazí také nově přidané partnery v okně partnery.</span><span class="sxs-lookup"><span data-stu-id="10f3f-127">After you select the Partners tile, you'll also see  newly added partners in the Partners blade.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a><span data-ttu-id="10f3f-128">Úprava existující partnery ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="10f3f-128">How to edit existing partners in your integration account</span></span>

1. <span data-ttu-id="10f3f-129">Vyberte **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="10f3f-129">Select the **Partners** tile.</span></span>
2. <span data-ttu-id="10f3f-130">Po otevření okna partnery, vyberte partnera, se kterým chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="10f3f-130">After the Partners blade opens, select the partner you want to edit.</span></span>
3. <span data-ttu-id="10f3f-131">Na **aktualizace partnera** dlaždici, provedené změny.</span><span class="sxs-lookup"><span data-stu-id="10f3f-131">On the **Update Partner** tile, make your changes.</span></span>
4. <span data-ttu-id="10f3f-132">Jakmile jste hotovi, zvolte **Uložit**, nebo zrušit změny, vyberte **zahodit**.</span><span class="sxs-lookup"><span data-stu-id="10f3f-132">After you're done, choose **Save**, or to cancel your changes, select **Discard**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a><span data-ttu-id="10f3f-133">Postup odstranění partnera</span><span class="sxs-lookup"><span data-stu-id="10f3f-133">How to delete a partner</span></span>

1. <span data-ttu-id="10f3f-134">Vyberte **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="10f3f-134">Select the **Partners** tile.</span></span>
2. <span data-ttu-id="10f3f-135">Po otevření okna partnera vyberte partnera, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="10f3f-135">After the Partner blade opens, select the partner that you want to delete.</span></span>
3. <span data-ttu-id="10f3f-136">Zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="10f3f-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a><span data-ttu-id="10f3f-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="10f3f-137">Next steps</span></span>
* [<span data-ttu-id="10f3f-138">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="10f3f-138">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

