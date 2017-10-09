---
title: "aaaAzure portál pro zásady prostředků | Microsoft Docs"
description: "Popisuje, jak toouse Azure portálu toocreate a spravovat zásady Resource Manager. Zásady můžete použít na hello předplatné nebo prostředek skupiny."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: ce6413386317e532b63761a24458b85c996af4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-tooassign-and-manage-resource-policies"></a><span data-ttu-id="9b806-104">Pomocí portálu Azure tooassign a spravovat zásady prostředků</span><span class="sxs-lookup"><span data-stu-id="9b806-104">Use Azure portal tooassign and manage resource policies</span></span>
<span data-ttu-id="9b806-105">Hello portál Azure umožňuje vám tooassign zásady tooresource skupiny prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="9b806-105">hello Azure portal enables you tooassign resource policies tooresource groups and subscriptions.</span></span> <span data-ttu-id="9b806-106">Hello uživatelské rozhraní umožňuje snadno tooselect hello zásady má tooassign a zadejte hodnoty parametrů pro nastavení této zásady hello toocustomize zásad.</span><span class="sxs-lookup"><span data-stu-id="9b806-106">hello user interface makes it easy tooselect hello policy that you want tooassign, and specify parameter values for that policy toocustomize hello policy settings.</span></span> 

<span data-ttu-id="9b806-107">tooassign zásadu prostřednictvím portálu hello definice zásady hello již musí existovat v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="9b806-107">tooassign a policy through hello portal, hello policy definition must already exist in your subscription.</span></span> <span data-ttu-id="9b806-108">Vaše předplatné má několik předdefinovaných zásad definice, které jsou připravené pro vás tooassign tooresource skupiny nebo předplatných.</span><span class="sxs-lookup"><span data-stu-id="9b806-108">Your subscription has several built-in policy definitions that are ready for you tooassign tooresource groups or subscriptions.</span></span> <span data-ttu-id="9b806-109">Zobrazí tyto předdefinované zásady a jakékoli vlastní zásady, které jste definovali při použití zásady portálu tooassign hello.</span><span class="sxs-lookup"><span data-stu-id="9b806-109">You see these built-in policies and any custom policies you have defined when using hello portal tooassign policies.</span></span> <span data-ttu-id="9b806-110">Toopolicies úvod a jak toodefine vlastní zásady najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="9b806-110">For an introduction toopolicies and how toodefine customized policy, see [Resource policy overview](resource-manager-policy.md).</span></span>

<span data-ttu-id="9b806-111">Zásady jsou zdědí všechny podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="9b806-111">Policies are inherited by all child resources.</span></span> <span data-ttu-id="9b806-112">Proto pokud je zásada použitá tooa skupinu prostředků, je použít tooall hello prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="9b806-112">So, if a policy is applied tooa resource group, it is applicable tooall hello resources in that resource group.</span></span> <span data-ttu-id="9b806-113">V tomto článku hello termín **oboru** odkazuje toohello skupinu prostředků nebo předplatného, které je přiřazen hello zásad.</span><span class="sxs-lookup"><span data-stu-id="9b806-113">In this article, hello term **scope** refers toohello resource group or subscription that is assigned hello policy.</span></span> 

<span data-ttu-id="9b806-114">Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.</span><span class="sxs-lookup"><span data-stu-id="9b806-114">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

## <a name="assign-a-policy"></a><span data-ttu-id="9b806-115">Přiřadit zásady</span><span class="sxs-lookup"><span data-stu-id="9b806-115">Assign a policy</span></span>

1. <span data-ttu-id="9b806-116">tooassign tooeither zásad skupiny prostředků nebo předplatného, vyberte tuto skupinu prostředků nebo předplatného.</span><span class="sxs-lookup"><span data-stu-id="9b806-116">tooassign a policy tooeither a resource group or subscription, select that resource group or subscription.</span></span> <span data-ttu-id="9b806-117">V nastavení hello vyberte **zásady**.</span><span class="sxs-lookup"><span data-stu-id="9b806-117">In hello settings, select **Policies**.</span></span>

   ![Vyberte zásady](./media/resource-manager-policy-portal/select-policies.png)

2. <span data-ttu-id="9b806-119">Vyberte toocreate přiřazení zásady pro tento rozsah **přidat přiřazení**.</span><span class="sxs-lookup"><span data-stu-id="9b806-119">toocreate a policy assignment for this scope, select **Add assignment**.</span></span>

   ![Přidat přiřazení](./media/resource-manager-policy-portal/add-assignment.png)

3. <span data-ttu-id="9b806-121">Vyberte zásady hello chcete tooassign.</span><span class="sxs-lookup"><span data-stu-id="9b806-121">Select hello policy you want tooassign.</span></span> <span data-ttu-id="9b806-122">I když nepřidali jste žádné zásady definice tooyour předplatné, zobrazí hello integrovaných zásad, které jsou k dispozici pro přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9b806-122">Even if you have not added any policy definitions tooyour subscription, you see hello built-in policies that are available for assignment.</span></span> <span data-ttu-id="9b806-123">Tyto integrované zásady zahrnují mnoho běžných scénářů.</span><span class="sxs-lookup"><span data-stu-id="9b806-123">These built-in policies cover many common scenarios.</span></span>

   ![Vyberte definice](./media/resource-manager-policy-portal/select-definition.png)

4. <span data-ttu-id="9b806-125">Po výběru zásady, zobrazí popis zásad hello a parametry pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="9b806-125">After selecting a policy, you see a description of hello policy, and any parameters for that policy.</span></span> <span data-ttu-id="9b806-126">Například hello následující obrázek ukazuje hello **povolené umístění** parametr, který je vyžadován pro hello zásad, které omezují hello dostupná umístění.</span><span class="sxs-lookup"><span data-stu-id="9b806-126">For example, hello following image shows hello **Allowed locations** parameter, which is required for hello policy that restricts hello available locations.</span></span>

   ![Zobrazit parametry](./media/resource-manager-policy-portal/show-parameters.png)

5. <span data-ttu-id="9b806-128">Hello uživatelském rozhraní vyberte hello toospecify hodnoty pro parametry zásad hello (například hello umístění, které lze použít pro nasazení).</span><span class="sxs-lookup"><span data-stu-id="9b806-128">Through hello user interface, select hello values toospecify for hello policy parameters (such as hello locations that can be used for deployment).</span></span>

   ![Vyberte hodnoty parametru](./media/resource-manager-policy-portal/select-parameters.png)

6. <span data-ttu-id="9b806-130">Zadejte hodnoty pro hello další parametry.</span><span class="sxs-lookup"><span data-stu-id="9b806-130">Provide values for hello other parameters.</span></span> <span data-ttu-id="9b806-131">Hello je automaticky přiřazen obor podle hello okna, se při spuštění přiřazení zásady hello.</span><span class="sxs-lookup"><span data-stu-id="9b806-131">hello scope is automatically assigned based on hello blade you selected when starting hello policy assignment.</span></span> <span data-ttu-id="9b806-132">Až to budete mít, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b806-132">Select **OK** when done.</span></span>

   ![Definujte parametry](./media/resource-manager-policy-portal/define-parameters.png)

  <span data-ttu-id="9b806-134">Přiřadili jste hello zásad toohello zadaný obor.</span><span class="sxs-lookup"><span data-stu-id="9b806-134">You have assigned hello policy toohello specified scope.</span></span>

## <a name="view-policy-assignments"></a><span data-ttu-id="9b806-135">Zobrazení přiřazení zásad.</span><span class="sxs-lookup"><span data-stu-id="9b806-135">View policy assignments</span></span>

<span data-ttu-id="9b806-136">Po přiřazení zásady, najdete v seznamu hello zásad pro tento obor.</span><span class="sxs-lookup"><span data-stu-id="9b806-136">After assigning a policy, you see it in hello list of policies for that scope.</span></span> <span data-ttu-id="9b806-137">Hello **podrobnosti** karta zobrazuje souhrnné údaje o přiřazení zásady hello.</span><span class="sxs-lookup"><span data-stu-id="9b806-137">hello **Details** tab shows a summary of hello policy assignment.</span></span>

![Zobrazit podrobnosti](./media/resource-manager-policy-portal/show-details.png)

<span data-ttu-id="9b806-139">Hello **pravidlo přiřazení** kartě se zobrazují hello JSON definice zásady hello.</span><span class="sxs-lookup"><span data-stu-id="9b806-139">hello **Assignment rule** tab shows hello JSON for hello policy definition.</span></span>

![Zobrazit pravidlo přiřazení](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a><span data-ttu-id="9b806-141">Změnit existující přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="9b806-141">Change an existing policy assignment</span></span>

<span data-ttu-id="9b806-142">Vyberte zásadu, toochange **upravit přiřazení** nebo **odstranit**</span><span class="sxs-lookup"><span data-stu-id="9b806-142">toochange a policy, select **Edit assignment** or **Delete**</span></span>

![upravit nebo odstranit přiřazení](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a><span data-ttu-id="9b806-144">Přiřazení vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="9b806-144">Assign custom policies</span></span>

<span data-ttu-id="9b806-145">Pokud jste definovali vlastní zásady v rámci vašeho předplatného, tyto zásady jsou k dispozici pro přiřazení prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="9b806-145">If you have defined custom policies in your subscription, those policies are available for assignment through hello portal.</span></span> <span data-ttu-id="9b806-146">Tyto zásady jsou uvedena **[vlastní]**</span><span class="sxs-lookup"><span data-stu-id="9b806-146">Those policies are prefaced with **[Custom]**</span></span>

![vlastní zásady](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a><span data-ttu-id="9b806-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b806-148">Next steps</span></span>
* <span data-ttu-id="9b806-149">toolearn o hello syntaxe JSON pro definování zásad, najdete v části [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="9b806-149">toolearn about hello JSON syntax for defining policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="9b806-150">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="9b806-150">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="9b806-151">schéma zásad Hello je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="9b806-151">hello policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

