---
title: "aaaCreate partnery pro business-to-business (B2B) zprávy – Azure Logic Apps | Microsoft Docs"
description: "Zjistěte, jak integrace tooyour partnery tooadd účet s hello Enterprise integračního balíčku a Logic Apps"
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
ms.openlocfilehash: 8dc70a8f441fcf228ed178029dcdbac940d794b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a><span data-ttu-id="7d853-103">Přidat nebo aktualizovat partnery ve smlouvách business-to-business v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="7d853-103">Add or update partners in business-to-business agreements in your workflow</span></span>

<span data-ttu-id="7d853-104">Partneři jsou entity, které se účastní transakce (B2B) business-to-business výměnu zpráv mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="7d853-104">Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other.</span></span> <span data-ttu-id="7d853-105">Před vytvořením partnery, které představují jste a jiné organizaci v tyto transakce, musí oba sdílet informace, které identifikuje a ověří zprávy odeslané podle sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="7d853-105">Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other.</span></span> <span data-ttu-id="7d853-106">Po popisují tyto podrobnosti a jsou připravené toostart vaší firmy vztahu, můžete vytvořit partnery ve vaší toorepresent účet integrace oba.</span><span class="sxs-lookup"><span data-stu-id="7d853-106">After you discuss these details and are ready toostart your business relationship, you can create partners in your integration account toorepresent you both.</span></span>

## <a name="what-roles-do-partners-have-in-your-integration-account"></a><span data-ttu-id="7d853-107">Ve vašem účtu integrace jaké role mají partnery?</span><span class="sxs-lookup"><span data-stu-id="7d853-107">What roles do partners have in your integration account?</span></span>

<span data-ttu-id="7d853-108">toodefine podrobnosti o hello zprávy vyměňují mezi partnery, vytvořte smlouvy mezi těmito partnery.</span><span class="sxs-lookup"><span data-stu-id="7d853-108">toodefine details about hello messages exchanged between partners, you create agreements between those partners.</span></span> <span data-ttu-id="7d853-109">Ale předtím, než vytvoříte smlouvu, je třeba přidat alespoň dva partneři tooyour integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="7d853-109">However, before you can create an agreement, you must have added at least two partners tooyour integration account.</span></span> <span data-ttu-id="7d853-110">Vaše organizace musí být součástí smlouvy hello jako hello **hostitele partnera**.</span><span class="sxs-lookup"><span data-stu-id="7d853-110">Your organization must be part of hello agreement as hello **host partner**.</span></span> <span data-ttu-id="7d853-111">Hello druhého partnera nebo **hosta partnera** představuje hello organizace, která výměny zpráv ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="7d853-111">hello other partner, or **guest partner** represents hello organization that exchanges messages with your organization.</span></span> <span data-ttu-id="7d853-112">Hello hosta partnera může být jiné společnosti nebo i oddělení ve vaší vlastní organizaci.</span><span class="sxs-lookup"><span data-stu-id="7d853-112">hello guest partner can be another company, or even a department in your own organization.</span></span>

<span data-ttu-id="7d853-113">Po přidání těchto partnerů, můžete vytvořit smlouvu.</span><span class="sxs-lookup"><span data-stu-id="7d853-113">After you add these partners, you can create an agreement.</span></span>

<span data-ttu-id="7d853-114">Příjem a odesílání nastavení jsou orientované z hello z hlediska hello Hosted partnera.</span><span class="sxs-lookup"><span data-stu-id="7d853-114">Receive and Send settings are oriented from hello point of view of hello Hosted Partner.</span></span> <span data-ttu-id="7d853-115">Například hello přijímat nastavení v smlouvu určit, jak hello hostované partnera obdrží zpráv odeslaných z hosta partnera.</span><span class="sxs-lookup"><span data-stu-id="7d853-115">For example, hello receive settings in an agreement determine how hello hosted partner receives messages sent from a guest partner.</span></span> <span data-ttu-id="7d853-116">Nastavení odesílání hello hello smlouvy, označuje, jak hello hostované partnera odešle zprávy toohello hosta partnera.</span><span class="sxs-lookup"><span data-stu-id="7d853-116">Likewise, hello send settings on hello agreement indicate how hello hosted partner sends messages toohello guest partner.</span></span>

## <a name="how-toocreate-a-partner"></a><span data-ttu-id="7d853-117">Jak toocreate partnera?</span><span class="sxs-lookup"><span data-stu-id="7d853-117">How toocreate a partner?</span></span>

1. <span data-ttu-id="7d853-118">V hello portálu Azure, vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="7d853-118">In hello Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="7d853-119">Hello filtru vyhledávacího pole zadejte **integrace**, pak vyberte **účty pro integraci** v seznamu výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="7d853-119">In hello filter search box, enter **integration**, then select **Integration Accounts** in hello results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="7d853-120">Vyberte účet integrace hello místo tooadd vaši partneři.</span><span class="sxs-lookup"><span data-stu-id="7d853-120">Select hello integration account where you want tooadd your partners.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="7d853-121">Vyberte hello **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7d853-121">Select hello **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. <span data-ttu-id="7d853-122">V okně partnery hello zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7d853-122">In hello Partners blade, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. <span data-ttu-id="7d853-123">Zadejte název svého partnera, a pak vyberte **kvalifikátor**.</span><span class="sxs-lookup"><span data-stu-id="7d853-123">Enter a name for your partner, then select a **Qualifier**.</span></span> <span data-ttu-id="7d853-124">Nakonec zadejte **hodnotu** toohelp identifikovat dokumenty, které jsou do svých aplikací.</span><span class="sxs-lookup"><span data-stu-id="7d853-124">Finally, enter a **Value** toohelp identify documents that come into your apps.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. <span data-ttu-id="7d853-125">průběh hello toosee pro proces vytváření partnera, vyberte hello *zvonku* ikonu oznámení.</span><span class="sxs-lookup"><span data-stu-id="7d853-125">toosee hello progress for your partner creation process, select hello *bell* notification icon.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. <span data-ttu-id="7d853-126">tooconfirm, které vaše nové partnery byly úspěšně přidány, vyberte hello **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7d853-126">tooconfirm that your new partners were successfully added, select hello **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    <span data-ttu-id="7d853-127">Po výběru dlaždice hello partnery, se zobrazí také nově přidané partnery v okně partnery hello.</span><span class="sxs-lookup"><span data-stu-id="7d853-127">After you select hello Partners tile, you'll also see  newly added partners in hello Partners blade.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-tooedit-existing-partners-in-your-integration-account"></a><span data-ttu-id="7d853-128">Jak existující tooedit partnerům ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="7d853-128">How tooedit existing partners in your integration account</span></span>

1. <span data-ttu-id="7d853-129">Vyberte hello **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7d853-129">Select hello **Partners** tile.</span></span>
2. <span data-ttu-id="7d853-130">Po otevření okna partnery hello vyberte partnera hello chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="7d853-130">After hello Partners blade opens, select hello partner you want tooedit.</span></span>
3. <span data-ttu-id="7d853-131">Na hello **aktualizace partnera** dlaždici, provedené změny.</span><span class="sxs-lookup"><span data-stu-id="7d853-131">On hello **Update Partner** tile, make your changes.</span></span>
4. <span data-ttu-id="7d853-132">Jakmile jste hotovi, zvolte **Uložit**, nebo vyberte změny, toocancel **zahodit**.</span><span class="sxs-lookup"><span data-stu-id="7d853-132">After you're done, choose **Save**, or toocancel your changes, select **Discard**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-toodelete-a-partner"></a><span data-ttu-id="7d853-133">Jak toodelete partnera</span><span class="sxs-lookup"><span data-stu-id="7d853-133">How toodelete a partner</span></span>

1. <span data-ttu-id="7d853-134">Vyberte hello **partnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7d853-134">Select hello **Partners** tile.</span></span>
2. <span data-ttu-id="7d853-135">Po otevření okna partnera hello vyberte hello partnera, které chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="7d853-135">After hello Partner blade opens, select hello partner that you want toodelete.</span></span>
3. <span data-ttu-id="7d853-136">Zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7d853-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a><span data-ttu-id="7d853-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d853-137">Next steps</span></span>
* [<span data-ttu-id="7d853-138">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="7d853-138">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

