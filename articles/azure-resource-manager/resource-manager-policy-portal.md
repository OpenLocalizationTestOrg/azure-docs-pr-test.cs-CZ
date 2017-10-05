---
title: "Portál Azure pro zásady prostředků | Microsoft Docs"
description: "Popisuje, jak pomocí portálu Azure vytvořit a spravovat zásady Resource Manager. Zásady můžete použít na předplatné nebo prostředek skupiny."
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
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a><span data-ttu-id="3637c-104">Pomocí portálu Azure přiřadit a spravovat zásady prostředků</span><span class="sxs-lookup"><span data-stu-id="3637c-104">Use Azure portal to assign and manage resource policies</span></span>
<span data-ttu-id="3637c-105">Portál Azure umožňuje přiřadit zásady prostředků skupiny prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="3637c-105">The Azure portal enables you to assign resource policies to resource groups and subscriptions.</span></span> <span data-ttu-id="3637c-106">Uživatelské rozhraní usnadňuje vyberte zásadu, kterou chcete přiřadit a zadejte hodnoty parametrů pro tuto zásadu přizpůsobit nastavení zásad.</span><span class="sxs-lookup"><span data-stu-id="3637c-106">The user interface makes it easy to select the policy that you want to assign, and specify parameter values for that policy to customize the policy settings.</span></span> 

<span data-ttu-id="3637c-107">Přiřazení zásad prostřednictvím portálu, definice zásady již musí existovat v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="3637c-107">To assign a policy through the portal, the policy definition must already exist in your subscription.</span></span> <span data-ttu-id="3637c-108">Vaše předplatné má několik předdefinovaných zásad definice, které jsou připravené k přiřazení skupiny prostředků nebo předplatných.</span><span class="sxs-lookup"><span data-stu-id="3637c-108">Your subscription has several built-in policy definitions that are ready for you to assign to resource groups or subscriptions.</span></span> <span data-ttu-id="3637c-109">Zobrazí tyto předdefinované zásady a všechny vlastní zásady, které jste definovali při použití portálu k přiřazení zásad.</span><span class="sxs-lookup"><span data-stu-id="3637c-109">You see these built-in policies and any custom policies you have defined when using the portal to assign policies.</span></span> <span data-ttu-id="3637c-110">Úvod do zásad a jak definovat vlastní zásady, najdete v části [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="3637c-110">For an introduction to policies and how to define customized policy, see [Resource policy overview](resource-manager-policy.md).</span></span>

<span data-ttu-id="3637c-111">Zásady jsou zdědí všechny podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="3637c-111">Policies are inherited by all child resources.</span></span> <span data-ttu-id="3637c-112">Ano Pokud je zásada pro skupinu prostředků, se vztahuje na všechny prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="3637c-112">So, if a policy is applied to a resource group, it is applicable to all the resources in that resource group.</span></span> <span data-ttu-id="3637c-113">V tomto článku termín **oboru** odkazuje na skupinu prostředků nebo předplatného, které je přiřazen zásady.</span><span class="sxs-lookup"><span data-stu-id="3637c-113">In this article, the term **scope** refers to the resource group or subscription that is assigned the policy.</span></span> 

<span data-ttu-id="3637c-114">Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.</span><span class="sxs-lookup"><span data-stu-id="3637c-114">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

## <a name="assign-a-policy"></a><span data-ttu-id="3637c-115">Přiřadit zásady</span><span class="sxs-lookup"><span data-stu-id="3637c-115">Assign a policy</span></span>

1. <span data-ttu-id="3637c-116">Přiřazení zásad na skupinu prostředků nebo předplatného, vyberte tuto skupinu prostředků nebo předplatného.</span><span class="sxs-lookup"><span data-stu-id="3637c-116">To assign a policy to either a resource group or subscription, select that resource group or subscription.</span></span> <span data-ttu-id="3637c-117">V nastavení, vyberte **zásady**.</span><span class="sxs-lookup"><span data-stu-id="3637c-117">In the settings, select **Policies**.</span></span>

   ![Vyberte zásady](./media/resource-manager-policy-portal/select-policies.png)

2. <span data-ttu-id="3637c-119">Chcete-li vytvořit přiřazení zásady pro tento rozsah, vyberte **přidat přiřazení**.</span><span class="sxs-lookup"><span data-stu-id="3637c-119">To create a policy assignment for this scope, select **Add assignment**.</span></span>

   ![Přidat přiřazení](./media/resource-manager-policy-portal/add-assignment.png)

3. <span data-ttu-id="3637c-121">Vyberte zásadu, kterou chcete přiřadit.</span><span class="sxs-lookup"><span data-stu-id="3637c-121">Select the policy you want to assign.</span></span> <span data-ttu-id="3637c-122">I když nepřidali jste žádné definice zásady pro vaše předplatné, zobrazí předdefinované zásady, které jsou k dispozici pro přiřazení.</span><span class="sxs-lookup"><span data-stu-id="3637c-122">Even if you have not added any policy definitions to your subscription, you see the built-in policies that are available for assignment.</span></span> <span data-ttu-id="3637c-123">Tyto integrované zásady zahrnují mnoho běžných scénářů.</span><span class="sxs-lookup"><span data-stu-id="3637c-123">These built-in policies cover many common scenarios.</span></span>

   ![Vyberte definice](./media/resource-manager-policy-portal/select-definition.png)

4. <span data-ttu-id="3637c-125">Po výběru zásady, najdete v části Popis zásady a parametry pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="3637c-125">After selecting a policy, you see a description of the policy, and any parameters for that policy.</span></span> <span data-ttu-id="3637c-126">Například na následujícím obrázku **povolené umístění** parametr, který je vyžadován pro zásadu, která k dispozici umístění.</span><span class="sxs-lookup"><span data-stu-id="3637c-126">For example, the following image shows the **Allowed locations** parameter, which is required for the policy that restricts the available locations.</span></span>

   ![Zobrazit parametry](./media/resource-manager-policy-portal/show-parameters.png)

5. <span data-ttu-id="3637c-128">V uživatelském rozhraní vyberte hodnoty pro parametry zásad (jako je například umístění, které lze použít pro nasazení) zadejte.</span><span class="sxs-lookup"><span data-stu-id="3637c-128">Through the user interface, select the values to specify for the policy parameters (such as the locations that can be used for deployment).</span></span>

   ![Vyberte hodnoty parametru](./media/resource-manager-policy-portal/select-parameters.png)

6. <span data-ttu-id="3637c-130">Zadejte hodnoty pro ostatní parametry.</span><span class="sxs-lookup"><span data-stu-id="3637c-130">Provide values for the other parameters.</span></span> <span data-ttu-id="3637c-131">Obor je automaticky přiřadit na základě v okně, které jste vybrali při spuštění přiřazení zásady.</span><span class="sxs-lookup"><span data-stu-id="3637c-131">The scope is automatically assigned based on the blade you selected when starting the policy assignment.</span></span> <span data-ttu-id="3637c-132">Až to budete mít, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="3637c-132">Select **OK** when done.</span></span>

   ![Definujte parametry](./media/resource-manager-policy-portal/define-parameters.png)

  <span data-ttu-id="3637c-134">Přiřadili jste zásadu pro zadaný obor.</span><span class="sxs-lookup"><span data-stu-id="3637c-134">You have assigned the policy to the specified scope.</span></span>

## <a name="view-policy-assignments"></a><span data-ttu-id="3637c-135">Zobrazení přiřazení zásad.</span><span class="sxs-lookup"><span data-stu-id="3637c-135">View policy assignments</span></span>

<span data-ttu-id="3637c-136">Po přiřazení zásady, najdete v seznamu zásad pro tento obor.</span><span class="sxs-lookup"><span data-stu-id="3637c-136">After assigning a policy, you see it in the list of policies for that scope.</span></span> <span data-ttu-id="3637c-137">**Podrobnosti** karta zobrazuje souhrnné údaje o přiřazení zásad.</span><span class="sxs-lookup"><span data-stu-id="3637c-137">The **Details** tab shows a summary of the policy assignment.</span></span>

![Zobrazit podrobnosti](./media/resource-manager-policy-portal/show-details.png)

<span data-ttu-id="3637c-139">**Pravidlo přiřazení** kartě se zobrazují ve formátu JSON pro definici zásady.</span><span class="sxs-lookup"><span data-stu-id="3637c-139">The **Assignment rule** tab shows the JSON for the policy definition.</span></span>

![Zobrazit pravidlo přiřazení](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a><span data-ttu-id="3637c-141">Změnit existující přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="3637c-141">Change an existing policy assignment</span></span>

<span data-ttu-id="3637c-142">Chcete-li změnit zásadu, vyberte **upravit přiřazení** nebo **odstranit**</span><span class="sxs-lookup"><span data-stu-id="3637c-142">To change a policy, select **Edit assignment** or **Delete**</span></span>

![upravit nebo odstranit přiřazení](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a><span data-ttu-id="3637c-144">Přiřazení vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="3637c-144">Assign custom policies</span></span>

<span data-ttu-id="3637c-145">Pokud jste definovali vlastní zásady v rámci vašeho předplatného, tyto zásady jsou k dispozici pro přiřazení prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="3637c-145">If you have defined custom policies in your subscription, those policies are available for assignment through the portal.</span></span> <span data-ttu-id="3637c-146">Tyto zásady jsou uvedena **[vlastní]**</span><span class="sxs-lookup"><span data-stu-id="3637c-146">Those policies are prefaced with **[Custom]**</span></span>

![vlastní zásady](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a><span data-ttu-id="3637c-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3637c-148">Next steps</span></span>
* <span data-ttu-id="3637c-149">Další informace o syntaxi JSON pro definování zásad najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="3637c-149">To learn about the JSON syntax for defining policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="3637c-150">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="3637c-150">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="3637c-151">Schéma zásad je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="3637c-151">The policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

