---
title: "Využívat Azure spravované aplikace | Microsoft Docs"
description: "Popisuje, jak zákazník vytvoří Azure spravované aplikace z publikovaných souborů."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="consume-an-internal-managed-application"></a><span data-ttu-id="c3ccc-103">Využívat interní spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="c3ccc-103">Consume an internal managed application</span></span>

<span data-ttu-id="c3ccc-104">Můžete využívat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-104">You can consume Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="c3ccc-105">Spravované aplikace můžete například vybrat od vašeho IT oddělení, které zajišťují dodržování standardů organizace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-105">For example, you can select managed applications from your IT department that ensure compliance with organizational standards.</span></span> <span data-ttu-id="c3ccc-106">Tyto spravované aplikace jsou k dispozici prostřednictvím katalogu služeb, není v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-106">These managed applications are available through the Service Catalog, not the Azure Marketplace.</span></span>

<span data-ttu-id="c3ccc-107">Než budete pokračovat v tomto článku, musíte mít k dispozici v katalogu služeb spravované aplikace pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-107">Before proceeding with this article, you must have a managed application available in the service catalog for your subscription.</span></span> <span data-ttu-id="c3ccc-108">Pokud někdo ve vaší organizaci dosud vytvořena spravované aplikace, najdete v části [publikování spravované aplikace pro interní používání](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-108">If someone in your organization has not already created a managed application, see [Publish a managed application for internal consumption](managed-application-publishing.md).</span></span>

<span data-ttu-id="c3ccc-109">V současné době můžete rozhraní příkazového řádku Azure nebo portálu Azure využívat spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-109">Currently, you can use either Azure CLI or the Azure portal to consume a managed application.</span></span>

## <a name="create-the-managed-application-by-using-the-portal"></a><span data-ttu-id="c3ccc-110">Vytvoření spravované aplikace prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="c3ccc-110">Create the managed application by using the portal</span></span>

<span data-ttu-id="c3ccc-111">Pokud chcete nasadit spravované aplikace prostřednictvím portálu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-111">To deploy a managed application through the portal, follow these steps:</span></span>

1. <span data-ttu-id="c3ccc-112">Přejděte na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-112">Go to the Azure portal.</span></span> <span data-ttu-id="c3ccc-113">Vyhledejte **katalogu služeb spravované aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-113">Search for **Service Catalog Managed Application**.</span></span>

   ![Katalog služeb spravované aplikace](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. <span data-ttu-id="c3ccc-115">Vyberte spravované aplikaci, kterou chcete vytvořit ze seznamu dostupných řešení.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-115">Select the managed application you want to create from the list of available solutions.</span></span> <span data-ttu-id="c3ccc-116">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-116">Select **Create**.</span></span>

   ![Výběr spravované aplikace](./media/managed-application-consumption/select-offer.png)

1. <span data-ttu-id="c3ccc-118">Zadejte hodnoty pro parametry, které jsou požadovány k přidělení prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-118">Provide values for the parameters that are required to provision the resources.</span></span> <span data-ttu-id="c3ccc-119">Vyberte **– Západ střední USA** pro umístění.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-119">Select **West Central US** for location.</span></span> <span data-ttu-id="c3ccc-120">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-120">Select **OK**.</span></span>

   ![Parametry spravované aplikace](./media/managed-application-consumption/input-parameters.png)

1. <span data-ttu-id="c3ccc-122">Šablona ověří hodnoty, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-122">The template validates the values you provided.</span></span> <span data-ttu-id="c3ccc-123">V případě úspěšného ověření vyberte **OK** ke spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-123">If validation succeeds, select **OK** to start the deployment.</span></span>

   ![Ověřování spravovaných aplikací](./media/managed-application-consumption/validation.png)

<span data-ttu-id="c3ccc-125">Po dokončení nasazení s příslušnými prostředky v šabloně definovánu připravené ve skupině spravovaných prostředků, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-125">After the deployment finishes, the appropriate resources defined in the template are provisioned in the managed resource group you provided.</span></span>

## <a name="create-the-managed-application-by-using-azure-cli"></a><span data-ttu-id="c3ccc-126">Vytvoření spravované aplikace pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c3ccc-126">Create the managed application by using Azure CLI</span></span>

<span data-ttu-id="c3ccc-127">Existují dva způsoby vytvoření spravované aplikace pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-127">There are two ways to create a managed application by using Azure CLI:</span></span>

* <span data-ttu-id="c3ccc-128">Použijte příkaz pro vytvoření spravovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-128">Use the command for creating managed applications.</span></span>
* <span data-ttu-id="c3ccc-129">Použijte příkaz regulární šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-129">Use the regular template deployment command.</span></span>

### <a name="use-the-template-deployment-command"></a><span data-ttu-id="c3ccc-130">Použijte příkaz nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="c3ccc-130">Use the template deployment command</span></span>

<span data-ttu-id="c3ccc-131">Nasaďte soubor applianceMainTemplate.json vytvořený dodavatele.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-131">Deploy the applianceMainTemplate.json file that the vendor created.</span></span>

<span data-ttu-id="c3ccc-132">Pak vytvořte dvě skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-132">Then create two resource groups.</span></span> <span data-ttu-id="c3ccc-133">První skupina prostředků je, kde se má vytvořit prostředek spravované aplikace: Microsoft.Solutions/appliances.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-133">The first resource group is where the managed application resource is created: Microsoft.Solutions/appliances.</span></span> <span data-ttu-id="c3ccc-134">Druhý skupina prostředků obsahuje všechny prostředky, které jsou definované v mainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-134">The second resource group contains all the resources defined in mainTemplate.json.</span></span> <span data-ttu-id="c3ccc-135">Tato skupina prostředků je spravovaný ISV.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-135">This resource group is managed by the ISV.</span></span>

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="c3ccc-136">Použití `westcentralus` jako umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-136">Use `westcentralus` as the location of the resource group.</span></span>
>

<span data-ttu-id="c3ccc-137">Pokud chcete nasadit applianceMainTemplate.json v mainResourceGroup, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-137">To deploy applianceMainTemplate.json in mainResourceGroup, use the following command:</span></span>

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

<span data-ttu-id="c3ccc-138">Po spuštění předchozí šablony, budete vyzváni k zadání hodnot parametrů, které jsou definované v šabloně.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-138">After the preceding template runs, it prompts you for the values of the parameters that are defined in the template.</span></span> <span data-ttu-id="c3ccc-139">Kromě parametrů, které jsou potřebné k zajištění prostředků v šabloně je třeba dvě hodnoty klíčů parametr:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-139">In addition to the parameters that are needed to provision resources in a template, you need two key parameter values:</span></span>

- <span data-ttu-id="c3ccc-140">**managedResourceGroupId**: ID skupiny prostředků obsahující prostředky, které jsou definované v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-140">**managedResourceGroupId**: The ID of the resource group containing the resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="c3ccc-141">ID je ve formátu `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-141">The ID is of the form `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span></span> <span data-ttu-id="c3ccc-142">V předchozím příkladu je ID `managedResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-142">In the preceding example, it's the ID of `managedResourceGroup`.</span></span>
- <span data-ttu-id="c3ccc-143">**applianceDefinitionId**: ID definice prostředku spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-143">**applianceDefinitionId**: The ID of the managed application definition resource.</span></span> <span data-ttu-id="c3ccc-144">Tato hodnota je poskytovaný ISV.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-144">This value is provided by the ISV.</span></span>

> [!NOTE]
> <span data-ttu-id="c3ccc-145">Vydavatel musí udělit přístup ke skupině prostředků, který obsahuje definici spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-145">The publisher must grant access to the resource group that contains the managed application definition.</span></span> <span data-ttu-id="c3ccc-146">Definice prostředků se vytvoří ve vašem předplatném vydavatele.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-146">The definition resource is created in the publisher subscription.</span></span> <span data-ttu-id="c3ccc-147">Přístup pro čtení k tomuto prostředku musí tedy uživatele, skupiny uživatelů nebo aplikací v klientovi zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-147">Therefore, a user, user group, or application in the customer tenant needs read access to this resource.</span></span>

<span data-ttu-id="c3ccc-148">Po dokončení nasazení úspěšně, uvidíte, že spravované aplikace se vytvoří v mainResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-148">After the deployment finishes successfully, you see the managed application is created in mainResourceGroup.</span></span> <span data-ttu-id="c3ccc-149">StorageAccount prostředek je vytvořen v managedResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-149">The storageAccount resource is created in managedResourceGroup.</span></span>

### <a name="use-the-create-command"></a><span data-ttu-id="c3ccc-150">Použijte příkaz create</span><span class="sxs-lookup"><span data-stu-id="c3ccc-150">Use the create command</span></span>

<span data-ttu-id="c3ccc-151">Můžete použít `az managedapp create` příkaz pro vytvoření spravované aplikace z definice spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-151">You can use the `az managedapp create` command to create a managed application from the managed application definition.</span></span>

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* <span data-ttu-id="c3ccc-152">**Id definice zařízení**: ID prostředku definici spravované aplikaci vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-152">**appliance-definition-Id**: The resource ID of the managed application definition created in the preceding step.</span></span> <span data-ttu-id="c3ccc-153">Pokud chcete získat toto ID, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-153">To obtain this ID, run the following command:</span></span>

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  <span data-ttu-id="c3ccc-154">Tento příkaz vrátí definice spravovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-154">This command returns the managed application definition.</span></span> <span data-ttu-id="c3ccc-155">Je třeba hodnotu vlastnost ID.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-155">You need the value of the ID property.</span></span>

* <span data-ttu-id="c3ccc-156">**spravované id rg**: název skupiny prostředků obsahující prostředky, které jsou definované v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-156">**managed-rg-id**: The name of the resource group containing the resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="c3ccc-157">Tato skupina prostředků je skupina spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-157">This resource group is the managed resource group.</span></span> <span data-ttu-id="c3ccc-158">Je spravovaný vydavatele.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-158">It's managed by the publisher.</span></span> <span data-ttu-id="c3ccc-159">Pokud neexistuje, vytvoří se pro vás.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-159">If it doesn't exist, it's created for you.</span></span>
* <span data-ttu-id="c3ccc-160">**Skupina prostředků**: skupině prostředků, kde se má vytvořit prostředek spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-160">**resource-group**: The resource group where the managed application resource is created.</span></span> <span data-ttu-id="c3ccc-161">Microsoft.Solutions/appliance prostředků je umístěn v této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-161">The Microsoft.Solutions/appliance resource lives in this resource group.</span></span>
* <span data-ttu-id="c3ccc-162">**Parametry**: parametry, které jsou potřebné pro prostředky, které jsou definované v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-162">**parameters**: The parameters that are needed for the resources defined in applianceMainTemplate.json.</span></span>

## <a name="known-issues"></a><span data-ttu-id="c3ccc-163">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="c3ccc-163">Known issues</span></span>

<span data-ttu-id="c3ccc-164">Tato verze preview zahrnuje následující problémy:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-164">This preview release includes the following issues:</span></span>

* <span data-ttu-id="c3ccc-165">Při vytváření spravované aplikace se zobrazí 500 vnitřní chybu serveru.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-165">A 500 internal server error appears during the creation of the managed application.</span></span> <span data-ttu-id="c3ccc-166">Pokud narazíte na potíže, je pravděpodobně přerušované.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-166">If you run into this issue, it's likely to be intermittent.</span></span> <span data-ttu-id="c3ccc-167">Opakujte operaci.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-167">Retry the operation.</span></span>
* <span data-ttu-id="c3ccc-168">Novou skupinu prostředků je potřeba pro skupinu spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-168">A new resource group is needed for the managed resource group.</span></span> <span data-ttu-id="c3ccc-169">Pokud použijete existující skupinu prostředků, nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-169">If you use an existing resource group, the deployment fails.</span></span>
* <span data-ttu-id="c3ccc-170">Skupinu prostředků, která obsahuje prostředek Microsoft.Solutions/appliances musí být vytvořené v **westcentralus** umístění.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-170">The resource group that contains the Microsoft.Solutions/appliances resource must be created in the **westcentralus** location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3ccc-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3ccc-171">Next steps</span></span>

* <span data-ttu-id="c3ccc-172">Úvod do spravovaných aplikací, najdete v části [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-172">For an introduction to managed applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="c3ccc-173">Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-173">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="c3ccc-174">Informace o publikování spravované aplikace do Azure Marketplace najdete v tématu [Azure spravované aplikace na webu Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-174">For information about publishing managed applications to the Azure Marketplace, see [Azure managed applications in the Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="c3ccc-175">Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-175">For information about consuming a managed application from the Marketplace, see [Consume Azure managed applications in the Marketplace](managed-application-consume-marketplace.md).</span></span>
