---
title: "Vytvoření, odkaz, odstranit nebo přesunout integrace účtu v Azure logic apps | Microsoft Docs"
description: "Postup vytvoření účtu integrace a propojit s logic apps"
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
ms.openlocfilehash: 716e7b5bab8725dea0fd2b760d0e46e8e892c5b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-an-integration-account"></a><span data-ttu-id="c93a9-103">Co je integrace účet?</span><span class="sxs-lookup"><span data-stu-id="c93a9-103">What is an integration account?</span></span>

<span data-ttu-id="c93a9-104">Účet integrace umožňuje integračních aplikací ke správě artefaktů, včetně schémat, map, certifikáty, partneři a smlouvy enterprise.</span><span class="sxs-lookup"><span data-stu-id="c93a9-104">An integration account allows enterprise integration apps to manage artifacts, including schemas, maps, certificates, partners and agreements.</span></span> <span data-ttu-id="c93a9-105">Všechny integrace aplikace, které vytvoříte použije integrace účet pro přístup tyto schémat, map, certifikáty a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c93a9-105">Any integration app you create uses an integration account to access these schemas, maps, certificates, and so on.</span></span>

## <a name="create-an-integration-account"></a><span data-ttu-id="c93a9-106">Vytvoření účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c93a9-106">Create an integration account</span></span>

1.  <span data-ttu-id="c93a9-107">Přihlaste se na web [Azure Portal](http://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="c93a9-107">Sign in to the [Azure portal](http://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="c93a9-108">V nabídce vlevo vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-108">From the left menu, select **More services**.</span></span>

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="c93a9-110">Do vyhledávacího pole zadejte "integrace" filtru.</span><span class="sxs-lookup"><span data-stu-id="c93a9-110">In the search box, type "integration" for your filter.</span></span> <span data-ttu-id="c93a9-111">V seznamu výsledků vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-111">In the results list, select **Integration Accounts**.</span></span>

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="c93a9-113">V horní části stránky, zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-113">At the top of the page, choose **Add**.</span></span>

    ![Vyberte Přidat](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. <span data-ttu-id="c93a9-115">Název účtu integrace a vyberte předplatné Azure, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="c93a9-115">Name your integration account and select the Azure subscription that you want to use.</span></span> <span data-ttu-id="c93a9-116">Můžete buď vytvořit novou **skupiny prostředků** nebo vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c93a9-116">You can either create a new **Resource group** or select an existing resource group.</span></span> <span data-ttu-id="c93a9-117">Vyberte **umístění** pro hostování účtu integrace a **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-117">Then select a **Location** for hosting your integration account and a **Pricing Tier**.</span></span> 

    <span data-ttu-id="c93a9-118">Až budete připraveni, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-118">When you're ready, choose **Create**.</span></span>

    ![Zadejte podrobnosti pro váš účet integrace](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    <span data-ttu-id="c93a9-120">Azure zřídí účtu integrace ve vybraném umístění, které by se měla dokončit během 1 minuty.</span><span class="sxs-lookup"><span data-stu-id="c93a9-120">Azure provisions your integration account  in the selected location, which should complete within 1 minute.</span></span>

5. <span data-ttu-id="c93a9-121">Aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="c93a9-121">Refresh the page.</span></span> <span data-ttu-id="c93a9-122">Zobrazí váš nový účet integrace, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="c93a9-122">You see your new integration account listed.</span></span>

    ![Zobrazí se váš nový účet integrace](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

<span data-ttu-id="c93a9-124">V dalším kroku propojte integrace účet, který jste vytvořili pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c93a9-124">Next, link the integration account that you created to your logic app.</span></span> 

## <a name="link-an-integration-account-to-a-logic-app"></a><span data-ttu-id="c93a9-125">Účet integrace propojit aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="c93a9-125">Link an integration account to a logic app</span></span>

<span data-ttu-id="c93a9-126">Umožnit logiky aplikace přístup k mapám, schémata, smlouvy a artefaktů ve vašem účtu integrace propojit účet integrace do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="c93a9-126">To give your logic apps access to maps, schemas, agreements, and other artifacts in your integration account, link the integration account to your logic app.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="c93a9-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c93a9-127">Prerequisites</span></span>

* <span data-ttu-id="c93a9-128">Účet integrace</span><span class="sxs-lookup"><span data-stu-id="c93a9-128">An integration account</span></span>
* <span data-ttu-id="c93a9-129">Aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="c93a9-129">A logic app</span></span>

> [!NOTE] 
> <span data-ttu-id="c93a9-130">Ujistěte se, že integrace účet a logiku aplikace jsou *stejného umístění Azure* před zahájením.</span><span class="sxs-lookup"><span data-stu-id="c93a9-130">Make sure your integration account and logic app are in the *same Azure location* before you begin.</span></span>


1. <span data-ttu-id="c93a9-131">Na portálu Azure vyberte svou aplikaci logiky a zkontrolujte umístění svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c93a9-131">In the Azure portal, select your logic app, and check your logic app's location.</span></span>

    ![Vyberte svou aplikaci logiky, zkontrolujte umístění](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. <span data-ttu-id="c93a9-133">V části **nastavení**, vyberte **integrace účet**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-133">Under **Settings**, select **Integration Account**.</span></span>

    ![Vyberte "Účet integrace"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. <span data-ttu-id="c93a9-135">Z **vyberte účet, integrace** seznamu, vyberte integrace účet, který chcete propojit do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="c93a9-135">From the **Select an Integration account** list, select the integration account you want to link to your logic app.</span></span> <span data-ttu-id="c93a9-136">K dokončení propojení, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-136">To finish linking, choose **Save**.</span></span>

    ![Vyberte svůj účet integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    <span data-ttu-id="c93a9-138">Zobrazí se oznámení, že zobrazuje svoji integraci do aplikace logiky je propojený účet a všechny artefakty ve vašem účtu integrace jsou nyní k dispozici pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c93a9-138">You get a notification that shows your integration account is linked to your logic app,  and that all artifacts in your integration account are now available to your logic app.</span></span>

    ![Aplikace logiky je propojený s vaším účtem integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

<span data-ttu-id="c93a9-140">Teď, když je propojené účtu integrace aplikace logiky, můžete použít konektory B2B ve vašich logic apps.</span><span class="sxs-lookup"><span data-stu-id="c93a9-140">Now that your integration account is linked to your logic app, you can use the B2B connectors in your logic apps.</span></span> <span data-ttu-id="c93a9-141">Některé běžné konektory B2B zahrnují ověření XML a plochý soubor kódování nebo dekódování.</span><span class="sxs-lookup"><span data-stu-id="c93a9-141">Some common B2B connectors include XML validation and flat file encode/decode.</span></span>  

## <a name="delete-your-integration-account"></a><span data-ttu-id="c93a9-142">Odstranění účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c93a9-142">Delete your integration account</span></span>

1. <span data-ttu-id="c93a9-143">Vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-143">Select **More services**.</span></span>

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="c93a9-145">Do vyhledávacího pole zadejte "integrace" filtru.</span><span class="sxs-lookup"><span data-stu-id="c93a9-145">In the search box, type "integration" for your filter.</span></span> <span data-ttu-id="c93a9-146">V seznamu výsledků vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-146">In the results list, select **Integration Accounts**.</span></span>

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="c93a9-148">Vyberte integrační účet, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="c93a9-148">Select the integration account that you want to delete.</span></span>

    ![Vyberte účet integrace odstranit](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. <span data-ttu-id="c93a9-150">V nabídce zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-150">On the menu, choose **Delete**.</span></span>

    ![Zvolte "Odstranit"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. <span data-ttu-id="c93a9-152">Potvrďte volbu Odstranit účet integrace.</span><span class="sxs-lookup"><span data-stu-id="c93a9-152">Confirm your choice to delete the integration account.</span></span>

## <a name="move-your-integration-account"></a><span data-ttu-id="c93a9-153">Přesunutí účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c93a9-153">Move your integration account</span></span>

<span data-ttu-id="c93a9-154">Pokud chcete přesunout účet integrace Azure jiné předplatné nebo prostředek skupiny, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="c93a9-154">To move an integration account to another Azure subscription or resource group, follow these steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c93a9-155">Je nutné aktualizovat všechny skripty používat nový prostředek ID po přesunutí účet integrace.</span><span class="sxs-lookup"><span data-stu-id="c93a9-155">You must update all scripts to use the new resource IDs after you move an integration account.</span></span>

1. <span data-ttu-id="c93a9-156">Vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-156">Select **More services**.</span></span>

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="c93a9-158">Do vyhledávacího pole zadejte "integrace" filtru.</span><span class="sxs-lookup"><span data-stu-id="c93a9-158">In the search box, type "integration" for your filter.</span></span> <span data-ttu-id="c93a9-159">V seznamu výsledků vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-159">In the results list, select **Integration Accounts**.</span></span>

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. <span data-ttu-id="c93a9-161">Vyberte integrační účet, který chcete přesunout.</span><span class="sxs-lookup"><span data-stu-id="c93a9-161">Select the integration account that you want to move.</span></span> <span data-ttu-id="c93a9-162">V části **nastavení**, zvolte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c93a9-162">Under **Settings**, choose **Properties**.</span></span>

    ![Vyberte účet integrace přesunout.](./media/logic-apps-enterprise-integration-accounts/move.png)

5. <span data-ttu-id="c93a9-165">Změňte skupinu prostředků nebo předplatného Azure, který je spojen s vaším účtem integrace.</span><span class="sxs-lookup"><span data-stu-id="c93a9-165">Change the resource group or Azure subscription that's associated with your integration account.</span></span>

    ![Vyberte skupiny prostředků změnu nebo změnu předplatného](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a><span data-ttu-id="c93a9-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c93a9-167">Next Steps</span></span>
* [<span data-ttu-id="c93a9-168">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="c93a9-168">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

