---
title: "aaaConsume Azure spravované aplikace | Microsoft Docs"
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
ms.openlocfilehash: b8510086eb05304c0e351a391b7e0cf34a467568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-internal-managed-application"></a><span data-ttu-id="44557-103">Využívat interní spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="44557-103">Consume an internal managed application</span></span>

<span data-ttu-id="44557-104">Můžete využívat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="44557-104">You can consume Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="44557-105">Spravované aplikace můžete například vybrat od vašeho IT oddělení, které zajišťují dodržování standardů organizace.</span><span class="sxs-lookup"><span data-stu-id="44557-105">For example, you can select managed applications from your IT department that ensure compliance with organizational standards.</span></span> <span data-ttu-id="44557-106">Tyto spravované aplikace jsou k dispozici prostřednictvím hello katalogu služeb, hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="44557-106">These managed applications are available through hello Service Catalog, not hello Azure Marketplace.</span></span>

<span data-ttu-id="44557-107">Než budete pokračovat v tomto článku, musíte mít k dispozici v katalogu služeb hello spravované aplikace pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="44557-107">Before proceeding with this article, you must have a managed application available in hello service catalog for your subscription.</span></span> <span data-ttu-id="44557-108">Pokud někdo ve vaší organizaci dosud vytvořena spravované aplikace, najdete v části [publikování spravované aplikace pro interní používání](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="44557-108">If someone in your organization has not already created a managed application, see [Publish a managed application for internal consumption](managed-application-publishing.md).</span></span>

<span data-ttu-id="44557-109">V současné době můžete použít rozhraní příkazového řádku Azure nebo hello Azure portálu tooconsume spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="44557-109">Currently, you can use either Azure CLI or hello Azure portal tooconsume a managed application.</span></span>

## <a name="create-hello-managed-application-by-using-hello-portal"></a><span data-ttu-id="44557-110">Vytvoření aplikace hello spravované pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="44557-110">Create hello managed application by using hello portal</span></span>

<span data-ttu-id="44557-111">toodeploy spravované aplikace prostřednictvím portálu hello, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="44557-111">toodeploy a managed application through hello portal, follow these steps:</span></span>

1. <span data-ttu-id="44557-112">Přejděte toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44557-112">Go toohello Azure portal.</span></span> <span data-ttu-id="44557-113">Vyhledejte **katalogu služeb spravované aplikace**.</span><span class="sxs-lookup"><span data-stu-id="44557-113">Search for **Service Catalog Managed Application**.</span></span>

   ![Katalog služeb spravované aplikace](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. <span data-ttu-id="44557-115">Vyberte hello spravované aplikace, které chcete toocreate z hello seznam dostupných řešení.</span><span class="sxs-lookup"><span data-stu-id="44557-115">Select hello managed application you want toocreate from hello list of available solutions.</span></span> <span data-ttu-id="44557-116">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="44557-116">Select **Create**.</span></span>

   ![Výběr spravované aplikace](./media/managed-application-consumption/select-offer.png)

1. <span data-ttu-id="44557-118">Zadejte hodnoty pro parametry hello, které jsou prostředky požadované tooprovision hello.</span><span class="sxs-lookup"><span data-stu-id="44557-118">Provide values for hello parameters that are required tooprovision hello resources.</span></span> <span data-ttu-id="44557-119">Vyberte **– Západ střední USA** pro umístění.</span><span class="sxs-lookup"><span data-stu-id="44557-119">Select **West Central US** for location.</span></span> <span data-ttu-id="44557-120">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="44557-120">Select **OK**.</span></span>

   ![Parametry spravované aplikace](./media/managed-application-consumption/input-parameters.png)

1. <span data-ttu-id="44557-122">Šablona Hello ověří hello hodnoty, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="44557-122">hello template validates hello values you provided.</span></span> <span data-ttu-id="44557-123">V případě úspěšného ověření vyberte **OK** toostart hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="44557-123">If validation succeeds, select **OK** toostart hello deployment.</span></span>

   ![Ověřování spravovaných aplikací](./media/managed-application-consumption/validation.png)

<span data-ttu-id="44557-125">Po dokončení nasazení hello hello definovaná v šabloně hello odpovídající prostředky jsou zřízené v hello spravovaných prostředků skupinu, kterou jste zadali.</span><span class="sxs-lookup"><span data-stu-id="44557-125">After hello deployment finishes, hello appropriate resources defined in hello template are provisioned in hello managed resource group you provided.</span></span>

## <a name="create-hello-managed-application-by-using-azure-cli"></a><span data-ttu-id="44557-126">Vytvoření aplikace hello spravované pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="44557-126">Create hello managed application by using Azure CLI</span></span>

<span data-ttu-id="44557-127">Existují dva způsoby toocreate spravované aplikace pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="44557-127">There are two ways toocreate a managed application by using Azure CLI:</span></span>

* <span data-ttu-id="44557-128">Pro vytvoření spravované aplikace můžete použijte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="44557-128">Use hello command for creating managed applications.</span></span>
* <span data-ttu-id="44557-129">Příkaz hello regulární šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="44557-129">Use hello regular template deployment command.</span></span>

### <a name="use-hello-template-deployment-command"></a><span data-ttu-id="44557-130">Použijte příkaz nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="44557-130">Use hello template deployment command</span></span>

<span data-ttu-id="44557-131">Nasaďte soubor applianceMainTemplate.json hello hello dodavatele vytvořen.</span><span class="sxs-lookup"><span data-stu-id="44557-131">Deploy hello applianceMainTemplate.json file that hello vendor created.</span></span>

<span data-ttu-id="44557-132">Pak vytvořte dvě skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="44557-132">Then create two resource groups.</span></span> <span data-ttu-id="44557-133">Hello první skupina prostředků je kde hello spravované aplikace je vytvořen prostředek: Microsoft.Solutions/appliances.</span><span class="sxs-lookup"><span data-stu-id="44557-133">hello first resource group is where hello managed application resource is created: Microsoft.Solutions/appliances.</span></span> <span data-ttu-id="44557-134">Skupina prostředků druhý Hello obsahuje všechny prostředky hello definované v mainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="44557-134">hello second resource group contains all hello resources defined in mainTemplate.json.</span></span> <span data-ttu-id="44557-135">Tato skupina prostředků je spravovaný hello ISV.</span><span class="sxs-lookup"><span data-stu-id="44557-135">This resource group is managed by hello ISV.</span></span>

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="44557-136">Použití `westcentralus` jako hello umístění skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="44557-136">Use `westcentralus` as hello location of hello resource group.</span></span>
>

<span data-ttu-id="44557-137">toodeploy applianceMainTemplate.json v mainResourceGroup hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="44557-137">toodeploy applianceMainTemplate.json in mainResourceGroup, use hello following command:</span></span>

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

<span data-ttu-id="44557-138">Po hello předcházející spustí šablony budete vyzváni k hello hodnoty hello parametrů, které jsou definované v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="44557-138">After hello preceding template runs, it prompts you for hello values of hello parameters that are defined in hello template.</span></span> <span data-ttu-id="44557-139">Kromě toohello parametry, které jsou potřeba tooprovision prostředky v šabloně, budete potřebovat dvě hodnoty klíčů parametr:</span><span class="sxs-lookup"><span data-stu-id="44557-139">In addition toohello parameters that are needed tooprovision resources in a template, you need two key parameter values:</span></span>

- <span data-ttu-id="44557-140">**managedResourceGroupId**: hello ID prostředků hello prostředků skupiny obsahující hello definované v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="44557-140">**managedResourceGroupId**: hello ID of hello resource group containing hello resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="44557-141">Hello ID má podobu hello `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span><span class="sxs-lookup"><span data-stu-id="44557-141">hello ID is of hello form `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span></span> <span data-ttu-id="44557-142">V předchozím příkladu hello, to je hello ID `managedResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="44557-142">In hello preceding example, it's hello ID of `managedResourceGroup`.</span></span>
- <span data-ttu-id="44557-143">**applianceDefinitionId**: hello ID hello spravované aplikace definice prostředků.</span><span class="sxs-lookup"><span data-stu-id="44557-143">**applianceDefinitionId**: hello ID of hello managed application definition resource.</span></span> <span data-ttu-id="44557-144">Tato hodnota je poskytovaný hello ISV.</span><span class="sxs-lookup"><span data-stu-id="44557-144">This value is provided by hello ISV.</span></span>

> [!NOTE]
> <span data-ttu-id="44557-145">Vydavatel Hello musí udělit přístup toohello prostředků skupiny, která obsahuje definice hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="44557-145">hello publisher must grant access toohello resource group that contains hello managed application definition.</span></span> <span data-ttu-id="44557-146">Hello definice prostředků se vytvoří v předplatném hello vydavatele.</span><span class="sxs-lookup"><span data-stu-id="44557-146">hello definition resource is created in hello publisher subscription.</span></span> <span data-ttu-id="44557-147">Proto uživatele, skupiny uživatelů nebo aplikací v klientovi zákazníka hello musí prostředků toothis přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="44557-147">Therefore, a user, user group, or application in hello customer tenant needs read access toothis resource.</span></span>

<span data-ttu-id="44557-148">Po nasazení hello skončí úspěšně, zobrazí hello spravované aplikace je vytvořena v mainResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="44557-148">After hello deployment finishes successfully, you see hello managed application is created in mainResourceGroup.</span></span> <span data-ttu-id="44557-149">Hello storageAccount prostředek vytvoří v managedResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="44557-149">hello storageAccount resource is created in managedResourceGroup.</span></span>

### <a name="use-hello-create-command"></a><span data-ttu-id="44557-150">Použití hello vytvoření příkazu</span><span class="sxs-lookup"><span data-stu-id="44557-150">Use hello create command</span></span>

<span data-ttu-id="44557-151">Můžete použít hello `az managedapp create` příkaz toocreate spravované aplikace z hello spravované definice aplikace.</span><span class="sxs-lookup"><span data-stu-id="44557-151">You can use hello `az managedapp create` command toocreate a managed application from hello managed application definition.</span></span>

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* <span data-ttu-id="44557-152">**Id definice zařízení**: ID prostředku hello hello spravované definice aplikace vytvořené v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="44557-152">**appliance-definition-Id**: hello resource ID of hello managed application definition created in hello preceding step.</span></span> <span data-ttu-id="44557-153">tooobtain toto ID, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="44557-153">tooobtain this ID, run hello following command:</span></span>

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  <span data-ttu-id="44557-154">Tento příkaz vrátí definice hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="44557-154">This command returns hello managed application definition.</span></span> <span data-ttu-id="44557-155">Je nutné hello hodnotu vlastnosti ID hello.</span><span class="sxs-lookup"><span data-stu-id="44557-155">You need hello value of hello ID property.</span></span>

* <span data-ttu-id="44557-156">**spravované id rg**: název hello hello prostředků obsahující hello prostředků skupiny definované v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="44557-156">**managed-rg-id**: hello name of hello resource group containing hello resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="44557-157">Tato skupina prostředků je skupina hello spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="44557-157">This resource group is hello managed resource group.</span></span> <span data-ttu-id="44557-158">Je spravovaný hello vydavatele.</span><span class="sxs-lookup"><span data-stu-id="44557-158">It's managed by hello publisher.</span></span> <span data-ttu-id="44557-159">Pokud neexistuje, vytvoří se pro vás.</span><span class="sxs-lookup"><span data-stu-id="44557-159">If it doesn't exist, it's created for you.</span></span>
* <span data-ttu-id="44557-160">**Skupina prostředků**: Po vytvoření skupiny prostředků hello kde hello spravovaných prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="44557-160">**resource-group**: hello resource group where hello managed application resource is created.</span></span> <span data-ttu-id="44557-161">Hello Microsoft.Solutions/appliance prostředků je umístěn v této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="44557-161">hello Microsoft.Solutions/appliance resource lives in this resource group.</span></span>
* <span data-ttu-id="44557-162">**Parametry**: hello parametry, které jsou potřebné pro prostředky hello definované v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="44557-162">**parameters**: hello parameters that are needed for hello resources defined in applianceMainTemplate.json.</span></span>

## <a name="known-issues"></a><span data-ttu-id="44557-163">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="44557-163">Known issues</span></span>

<span data-ttu-id="44557-164">Tato verze preview zahrnuje hello následující problémy:</span><span class="sxs-lookup"><span data-stu-id="44557-164">This preview release includes hello following issues:</span></span>

* <span data-ttu-id="44557-165">Během vytváření hello hello spravované aplikace se zobrazí 500 vnitřní chybu serveru.</span><span class="sxs-lookup"><span data-stu-id="44557-165">A 500 internal server error appears during hello creation of hello managed application.</span></span> <span data-ttu-id="44557-166">Pokud narazíte na potíže, je pravděpodobně toobe přerušované.</span><span class="sxs-lookup"><span data-stu-id="44557-166">If you run into this issue, it's likely toobe intermittent.</span></span> <span data-ttu-id="44557-167">Opakujte operaci hello.</span><span class="sxs-lookup"><span data-stu-id="44557-167">Retry hello operation.</span></span>
* <span data-ttu-id="44557-168">Novou skupinu prostředků je potřeba pro skupinu hello spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="44557-168">A new resource group is needed for hello managed resource group.</span></span> <span data-ttu-id="44557-169">Pokud použijete existující skupinu prostředků, hello nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="44557-169">If you use an existing resource group, hello deployment fails.</span></span>
* <span data-ttu-id="44557-170">Hello skupinu prostředků, která obsahuje hello Microsoft.Solutions/appliances prostředek musí být vytvořen v hello **westcentralus** umístění.</span><span class="sxs-lookup"><span data-stu-id="44557-170">hello resource group that contains hello Microsoft.Solutions/appliances resource must be created in hello **westcentralus** location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44557-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44557-171">Next steps</span></span>

* <span data-ttu-id="44557-172">Úvod toomanaged aplikace naleznete v [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44557-172">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="44557-173">Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="44557-173">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="44557-174">Informace o publikování toohello spravovaných aplikací Azure Marketplace najdete v tématu [Azure spravované aplikace v hello Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="44557-174">For information about publishing managed applications toohello Azure Marketplace, see [Azure managed applications in hello Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="44557-175">Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="44557-175">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
