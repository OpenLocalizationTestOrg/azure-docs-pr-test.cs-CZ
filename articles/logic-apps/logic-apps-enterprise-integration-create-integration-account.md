---
title: "aaaCreate, odkaz, odstranit nebo přesunout integrace účtu v Azure logic apps | Microsoft Docs"
description: "Jak toocreate integrační účet a propojit jej tooyour logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: fda6c91723b3e3624ee176df112ba8b6c9800273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-integration-account"></a><span data-ttu-id="c0455-103">Co je integrace účet?</span><span class="sxs-lookup"><span data-stu-id="c0455-103">What is an integration account?</span></span>

<span data-ttu-id="c0455-104">Účet integrace umožňuje aplikacím integrace enterprise toomanage artefaktů, včetně schémat, map, certifikáty, partneři a smlouvy.</span><span class="sxs-lookup"><span data-stu-id="c0455-104">An integration account allows enterprise integration apps toomanage artifacts, including schemas, maps, certificates, partners and agreements.</span></span> <span data-ttu-id="c0455-105">Všechny integrace aplikace, které vytvoříte používá integraci účet tooaccess tyto schémat, map, certifikáty a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c0455-105">Any integration app you create uses an integration account tooaccess these schemas, maps, certificates, and so on.</span></span>

## <a name="create-an-integration-account"></a><span data-ttu-id="c0455-106">Vytvoření účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c0455-106">Create an integration account</span></span>

1.  <span data-ttu-id="c0455-107">Přihlaste se toohello [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="c0455-107">Sign in toohello [Azure portal](http://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="c0455-108">Hello levé nabídce vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c0455-108">From hello left menu, select **More services**.</span></span>

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="c0455-110">Hello vyhledávacího pole zadejte "integrace" filtru.</span><span class="sxs-lookup"><span data-stu-id="c0455-110">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="c0455-111">V seznamu výsledků hello vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c0455-111">In hello results list, select **Integration Accounts**.</span></span>

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="c0455-113">V horní části hello hello stránky, zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c0455-113">At hello top of hello page, choose **Add**.</span></span>

    ![Vyberte Přidat](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. <span data-ttu-id="c0455-115">Název vašeho účtu integrace a vyberte hello předplatné Azure, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="c0455-115">Name your integration account and select hello Azure subscription that you want toouse.</span></span> <span data-ttu-id="c0455-116">Můžete buď vytvořit novou **skupiny prostředků** nebo vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c0455-116">You can either create a new **Resource group** or select an existing resource group.</span></span> <span data-ttu-id="c0455-117">Vyberte **umístění** pro hostování účtu integrace a **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="c0455-117">Then select a **Location** for hosting your integration account and a **Pricing Tier**.</span></span> 

    <span data-ttu-id="c0455-118">Až budete připraveni, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c0455-118">When you're ready, choose **Create**.</span></span>

    ![Zadejte podrobnosti pro váš účet integrace](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    <span data-ttu-id="c0455-120">Azure zřídí účtu integrace v hello vybrané umístění, které by se měla dokončit během 1 minuty.</span><span class="sxs-lookup"><span data-stu-id="c0455-120">Azure provisions your integration account  in hello selected location, which should complete within 1 minute.</span></span>

5. <span data-ttu-id="c0455-121">Obnovte stránku hello.</span><span class="sxs-lookup"><span data-stu-id="c0455-121">Refresh hello page.</span></span> <span data-ttu-id="c0455-122">Zobrazí váš nový účet integrace, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="c0455-122">You see your new integration account listed.</span></span>

    ![Zobrazí se váš nový účet integrace](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

<span data-ttu-id="c0455-124">V dalším kroku propojte hello integrace účet je vytvořený tooyour logiku aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c0455-124">Next, link hello integration account that you created tooyour logic app.</span></span> 

## <a name="link-an-integration-account-tooa-logic-app"></a><span data-ttu-id="c0455-125">Odkaz integrace účet tooa logiku aplikace</span><span class="sxs-lookup"><span data-stu-id="c0455-125">Link an integration account tooa logic app</span></span>

<span data-ttu-id="c0455-126">toogive aplikace logiky přístup toomaps, schémata, smlouvy a artefaktů ve vašem účtu integrace odkaz hello integrace účet tooyour logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0455-126">toogive your logic apps access toomaps, schemas, agreements, and other artifacts in your integration account, link hello integration account tooyour logic app.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="c0455-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0455-127">Prerequisites</span></span>

* <span data-ttu-id="c0455-128">Účet integrace</span><span class="sxs-lookup"><span data-stu-id="c0455-128">An integration account</span></span>
* <span data-ttu-id="c0455-129">Aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="c0455-129">A logic app</span></span>

> [!NOTE] 
> <span data-ttu-id="c0455-130">Zkontrolujte, zda jsou vaše integrace účet a logiku aplikace v hello *stejného umístění Azure* před zahájením.</span><span class="sxs-lookup"><span data-stu-id="c0455-130">Make sure your integration account and logic app are in hello *same Azure location* before you begin.</span></span>


1. <span data-ttu-id="c0455-131">V hello portálu Azure vyberte svou aplikaci logiky a zkontrolujte umístění svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c0455-131">In hello Azure portal, select your logic app, and check your logic app's location.</span></span>

    ![Vyberte svou aplikaci logiky, zkontrolujte umístění](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. <span data-ttu-id="c0455-133">V části **nastavení**, vyberte **integrace účet**.</span><span class="sxs-lookup"><span data-stu-id="c0455-133">Under **Settings**, select **Integration Account**.</span></span>

    ![Vyberte "Účet integrace"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. <span data-ttu-id="c0455-135">Z hello **vyberte účet, integrace** seznamu, vyberte hello integrace účtu chcete toolink tooyour logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0455-135">From hello **Select an Integration account** list, select hello integration account you want toolink tooyour logic app.</span></span> <span data-ttu-id="c0455-136">Vyberte propojení, toofinish **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c0455-136">toofinish linking, choose **Save**.</span></span>

    ![Vyberte svůj účet integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    <span data-ttu-id="c0455-138">Zobrazí se oznámení, že zobrazuje svoji integraci aplikace logiky propojené tooyour je účet a všechny artefakty ve vašem účtu integrace jsou nyní k dispozici tooyour aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="c0455-138">You get a notification that shows your integration account is linked tooyour logic app,  and that all artifacts in your integration account are now available tooyour logic app.</span></span>

    ![Aplikace logiky je propojený účet tooyour integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

<span data-ttu-id="c0455-140">Teď, když je váš účet integrace aplikace logiky propojené tooyour, můžete použít konektory hello B2B ve vašich logic apps.</span><span class="sxs-lookup"><span data-stu-id="c0455-140">Now that your integration account is linked tooyour logic app, you can use hello B2B connectors in your logic apps.</span></span> <span data-ttu-id="c0455-141">Některé běžné konektory B2B zahrnují ověření XML a plochý soubor kódování nebo dekódování.</span><span class="sxs-lookup"><span data-stu-id="c0455-141">Some common B2B connectors include XML validation and flat file encode/decode.</span></span>  

## <a name="delete-your-integration-account"></a><span data-ttu-id="c0455-142">Odstranění účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c0455-142">Delete your integration account</span></span>

1. <span data-ttu-id="c0455-143">Vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c0455-143">Select **More services**.</span></span>

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="c0455-145">Hello vyhledávacího pole zadejte "integrace" filtru.</span><span class="sxs-lookup"><span data-stu-id="c0455-145">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="c0455-146">V seznamu výsledků hello vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c0455-146">In hello results list, select **Integration Accounts**.</span></span>

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="c0455-148">Vyberte účet hello integrace, které chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="c0455-148">Select hello integration account that you want toodelete.</span></span>

    ![Vyberte účet toodelete integrace](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. <span data-ttu-id="c0455-150">V nabídce hello zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c0455-150">On hello menu, choose **Delete**.</span></span>

    ![Zvolte "Odstranit"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. <span data-ttu-id="c0455-152">Potvrzení účtu choice toodelete hello integrace.</span><span class="sxs-lookup"><span data-stu-id="c0455-152">Confirm your choice toodelete hello integration account.</span></span>

## <a name="move-your-integration-account"></a><span data-ttu-id="c0455-153">Přesunutí účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c0455-153">Move your integration account</span></span>

<span data-ttu-id="c0455-154">toomove tooanother účet integraci Azure předplatné nebo skupinu prostředků, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="c0455-154">toomove an integration account tooanother Azure subscription or resource group, follow these steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0455-155">Po přesunutí účet integrace, je nutné aktualizovat všechny skripty toouse hello nové ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="c0455-155">You must update all scripts toouse hello new resource IDs after you move an integration account.</span></span>

1. <span data-ttu-id="c0455-156">Vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c0455-156">Select **More services**.</span></span>

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="c0455-158">Hello vyhledávacího pole zadejte "integrace" filtru.</span><span class="sxs-lookup"><span data-stu-id="c0455-158">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="c0455-159">V seznamu výsledků hello vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c0455-159">In hello results list, select **Integration Accounts**.</span></span>

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. <span data-ttu-id="c0455-161">Vyberte účet hello integrace, které chcete toomove.</span><span class="sxs-lookup"><span data-stu-id="c0455-161">Select hello integration account that you want toomove.</span></span> <span data-ttu-id="c0455-162">V části **nastavení**, zvolte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c0455-162">Under **Settings**, choose **Properties**.</span></span>

    ![Vyberte účet toomove integrace.](./media/logic-apps-enterprise-integration-accounts/move.png)

5. <span data-ttu-id="c0455-165">Změnit skupinu prostředků hello nebo předplatné Azure, který je spojen s vaším účtem integrace.</span><span class="sxs-lookup"><span data-stu-id="c0455-165">Change hello resource group or Azure subscription that's associated with your integration account.</span></span>

    ![Vyberte skupiny prostředků změnu nebo změnu předplatného](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a><span data-ttu-id="c0455-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0455-167">Next Steps</span></span>
* [<span data-ttu-id="c0455-168">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="c0455-168">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

