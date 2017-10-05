---
title: "Vytvoření virtuální sítě Azure (klasické) s několika podsítěmi | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť s více podsítěmi v Azure (klasické)."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 95c2f4fe40590a8d809f634fb5b2c92d07421bb0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a><span data-ttu-id="b9ea6-103">Vytvoření virtuální sítě (klasické) s několika podsítěmi</span><span class="sxs-lookup"><span data-stu-id="b9ea6-103">Create a virtual network (classic) with multiple subnets</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9ea6-104">Azure má dva [různé modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) pro vytváření a práci s prostředky: Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-104">Azure has two [different deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) for creating and working with resources: Resource Manager and classic.</span></span> <span data-ttu-id="b9ea6-105">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-105">This article covers using the classic deployment model.</span></span> <span data-ttu-id="b9ea6-106">Společnost Microsoft doporučuje vytvoření většina nové virtuální sítě prostřednictvím [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-106">Microsoft recommends creating most new virtual networks through the [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) deployment model.</span></span>

<span data-ttu-id="b9ea6-107">V tomto kurzu zjistěte, jak vytvořit základní virtuální síť Azure (klasické) s oddělené veřejné a privátní podsítě.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-107">In this tutorial, learn how to create a basic Azure virtual network (classic) that has separate public and private subnets.</span></span> <span data-ttu-id="b9ea6-108">Můžete vytvořit prostředky Azure, jako jsou virtuální počítače a cloudové služby v podsíti.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-108">You can create Azure resources, like Virtual machines and Cloud services in a subnet.</span></span> <span data-ttu-id="b9ea6-109">Prostředky vytvořené ve virtuální sítě (klasické) mohou komunikovat navzájem a s prostředky v jiných sítích připojené k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-109">Resources created in virtual networks (classic) can communicate with each other, and with resources in other networks connected to a virtual network.</span></span>

<span data-ttu-id="b9ea6-110">Další informace o všech [virtuální sítě](virtual-network-manage-network.md) a [podsíť](virtual-network-manage-subnet.md) nastavení.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-110">Learn more about all [virtual network](virtual-network-manage-network.md) and [subnet](virtual-network-manage-subnet.md) settings.</span></span>

> [!WARNING]
> <span data-ttu-id="b9ea6-111">Virtuální sítě (klasické) se okamžitě odstraní Azure při [předplatné je zakázané](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-111">Virtual networks (classic) are immediately deleted by Azure when a [subscription is disabled](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span></span> <span data-ttu-id="b9ea6-112">Bez ohledu na to, jestli existují prostředky ve virtuální síti, se odstraní virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-112">Virtual networks (classic) are deleted regardless of whether resources exist in the virtual network.</span></span> <span data-ttu-id="b9ea6-113">Pokud později znovu povolíte předplatné, je nutné znovu vytvořit prostředky, které existovaly ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-113">If you later re-enable the subscription, resources that existed in the virtual network must be recreated.</span></span>

<span data-ttu-id="b9ea6-114">Vytvoříte virtuální sítě (klasické) pomocí [portál Azure](#portal), [rozhraní příkazového řádku Azure (CLI) 1.0](#azure-cli), nebo [prostředí PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-114">You can create a virtual network (classic) by using the [Azure portal](#portal), the [Azure command-line interface (CLI) 1.0](#azure-cli), or [PowerShell](#powershell).</span></span>

## <a name="portal"></a><span data-ttu-id="b9ea6-115">Portál</span><span class="sxs-lookup"><span data-stu-id="b9ea6-115">Portal</span></span>

1. <span data-ttu-id="b9ea6-116">V internetovém prohlížeči, přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-116">In an Internet browser, go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b9ea6-117">Přihlaste se pomocí vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-117">Log in using your [Azure account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="b9ea6-118">Pokud nemáte účet Azure, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-118">If you don't have an Azure account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="b9ea6-119">Klikněte na tlačítko **+ nový** na portálu.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-119">Click **+New** in the portal.</span></span>
3. <span data-ttu-id="b9ea6-120">Zadejte *virtuální síť* v **vyhledávání na webu Marketplace** pole v horní části **nový** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-120">Enter *Virtual network* in the **Search the Marketplace** box at the top of the **New** blade that appears.</span></span>  <span data-ttu-id="b9ea6-121">Klikněte na tlačítko **virtuální síť** při zobrazí ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-121">Click **Virtual network** when it appears in the search results.</span></span>
4. <span data-ttu-id="b9ea6-122">Vyberte **Classic** v **vybrat model nasazení** pole **virtuální sítě** okno, které se zobrazí, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-122">Select **Classic** in the **Select a deployment model** box in the **Virtual Network** blade that appears, then click **Create**.</span></span> 
5. <span data-ttu-id="b9ea6-123">Zadejte následující hodnoty **vytvořit virtuální síť (klasická)** okna a potom klikněte na **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-123">Enter the following values on the **Create virtual network (classic)** blade and then click **Create**:</span></span>

    |<span data-ttu-id="b9ea6-124">Nastavení</span><span class="sxs-lookup"><span data-stu-id="b9ea6-124">Setting</span></span>|<span data-ttu-id="b9ea6-125">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b9ea6-125">Value</span></span>|
    |---|---|
    |<span data-ttu-id="b9ea6-126">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b9ea6-126">Name</span></span>|<span data-ttu-id="b9ea6-127">myVnet</span><span class="sxs-lookup"><span data-stu-id="b9ea6-127">myVnet</span></span>|
    |<span data-ttu-id="b9ea6-128">Adresní prostor</span><span class="sxs-lookup"><span data-stu-id="b9ea6-128">Address space</span></span>|<span data-ttu-id="b9ea6-129">10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b9ea6-129">10.0.0.0/16</span></span>|
    |<span data-ttu-id="b9ea6-130">Název podsítě</span><span class="sxs-lookup"><span data-stu-id="b9ea6-130">Subnet name</span></span>|<span data-ttu-id="b9ea6-131">Veřejné</span><span class="sxs-lookup"><span data-stu-id="b9ea6-131">Public</span></span>|
    |<span data-ttu-id="b9ea6-132">Rozsah adres podsítě</span><span class="sxs-lookup"><span data-stu-id="b9ea6-132">Subnet address range</span></span>|<span data-ttu-id="b9ea6-133">10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b9ea6-133">10.0.0.0/24</span></span>|
    |<span data-ttu-id="b9ea6-134">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="b9ea6-134">Resource group</span></span>|<span data-ttu-id="b9ea6-135">Nechte **vytvořit nový** vybrané a potom zadejte **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-135">Leave **Create new** selected, and then enter **myResourceGroup**.</span></span>|
    |<span data-ttu-id="b9ea6-136">Předplatné a umístění</span><span class="sxs-lookup"><span data-stu-id="b9ea6-136">Subscription and location</span></span>|<span data-ttu-id="b9ea6-137">Vyberte vaše předplatné a umístění.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-137">Select your subscription and location.</span></span>

    <span data-ttu-id="b9ea6-138">Pokud jste Azure ještě nepoužívali, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (také označuje jako *oblasti*).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-138">If you're new to Azure, learn more about [resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (also referred to as *regions*).</span></span>
4. <span data-ttu-id="b9ea6-139">Na portálu můžete vytvořit jenom jednu podsíť, když vytvoříte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-139">In the portal, you can create only one subnet when you create a virtual network.</span></span> <span data-ttu-id="b9ea6-140">V tomto kurzu vytvoříte druhou podsíť po vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-140">In this tutorial, you create a second subnet after you create the virtual network.</span></span> <span data-ttu-id="b9ea6-141">Můžete vytvořit později přístupné z Internetu prostředky v **veřejné** podsítě.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-141">You might later create Internet-accessible resources in the **Public** subnet.</span></span> <span data-ttu-id="b9ea6-142">Můžete také vytvořit prostředky, které nejsou přístupné z Internetu v **privátní** podsítě.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-142">You also might create resources that aren't accessible from the Internet in the **Private** subnet.</span></span> <span data-ttu-id="b9ea6-143">Chcete-li vytvořit druhou podsíť, zadejte **myVnet** v **vyhledávání prostředků** pole v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-143">To create the second subnet, enter **myVnet** in the **Search resources** box at the top of the page.</span></span> <span data-ttu-id="b9ea6-144">Klikněte na tlačítko **myVnet** při zobrazí ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-144">Click **myVnet** when it appears in the search results.</span></span>
5. <span data-ttu-id="b9ea6-145">Klikněte na tlačítko **podsítě** (v **nastavení** část) na **vytvořit virtuální síť (klasická)** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-145">Click **Subnets** (in the **SETTINGS** section) on the **Create virtual network (classic)** blade that appears.</span></span>
6. <span data-ttu-id="b9ea6-146">Klikněte na tlačítko **+ přidat** na **myVnet - podsítě** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-146">Click **+Add** on the **myVnet - Subnets** blade that appears.</span></span>
7. <span data-ttu-id="b9ea6-147">Zadejte **privátní** pro **název** na **přidat podsíť** okno.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-147">Enter **Private** for **Name** on the **Add subnet** blade.</span></span> <span data-ttu-id="b9ea6-148">Zadejte **10.0.1.0/24** pro **rozsahu adres**.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-148">Enter **10.0.1.0/24** for **Address range**.</span></span>  <span data-ttu-id="b9ea6-149">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-149">Click **OK**.</span></span>
8. <span data-ttu-id="b9ea6-150">Na **myVnet - podsítě** okně uvidíte **veřejné** a **privátní** podsítě, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-150">On the **myVnet - Subnets** blade, you can see the **Public** and **Private** subnets that you created.</span></span>
9. <span data-ttu-id="b9ea6-151">**Volitelné**: Po dokončení tohoto kurzu můžete chtít odstranit prostředky, které jste vytvořili, tak, aby vám zbytečně nenabíhaly poplatky za používání:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-151">**Optional**: When you finish this tutorial, you might want to delete the resources that you created, so that you don't incur usage charges:</span></span>
    - <span data-ttu-id="b9ea6-152">Klikněte na tlačítko **přehled** na **myVnet** okno.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-152">Click **Overview** on the **myVnet** blade.</span></span>
    - <span data-ttu-id="b9ea6-153">Klikněte **odstranit** ikonu na **myVnet** okno.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-153">Click the **Delete** icon on the **myVnet** blade.</span></span>
    - <span data-ttu-id="b9ea6-154">Potvrďte odstranění, klikněte na tlačítko **Ano** v **virtuální sítě odstranit** pole.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-154">To confirm the deletion, click **Yes** in the **Delete virtual network** box.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="b9ea6-155">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b9ea6-155">Azure CLI</span></span>

1. <span data-ttu-id="b9ea6-156">Můžete buď [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), nebo pomocí rozhraní příkazového řádku v prostředí cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-156">You can either [install and configure the Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), or use the CLI within the Azure Cloud Shell.</span></span> <span data-ttu-id="b9ea6-157">Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-157">The Azure Cloud Shell is a free Bash shell that you can run directly within the Azure portal.</span></span> <span data-ttu-id="b9ea6-158">Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-158">It has the Azure CLI preinstalled and configured to use with your account.</span></span> <span data-ttu-id="b9ea6-159">Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `azure <command> --help`.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-159">To get help for CLI commands, type `azure <command> --help`.</span></span> 
2. <span data-ttu-id="b9ea6-160">V relaci příkazového řádku přihlaste se do Azure pomocí příkazu, který následuje dále.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-160">In a CLI session, log in to Azure with the command that follows.</span></span> <span data-ttu-id="b9ea6-161">Pokud kliknete na tlačítko **vyzkoušet** do pole níže, otevře se prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-161">If you click **Try it** in the box below, a Cloud Shell opens.</span></span> <span data-ttu-id="b9ea6-162">K předplatnému Azure, se můžete přihlásit bez zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-162">You can log in to your Azure subscription, without entering the following command:</span></span>

    ```azurecli-interactive
    azure login
    ```

3. <span data-ttu-id="b9ea6-163">K zajištění, že rozhraní příkazového řádku je v režimu správy služby, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-163">To ensure the CLI is in Service Management mode, enter the following command:</span></span>

    ```azurecli-interactive
    azure config mode asm
    ```

4. <span data-ttu-id="b9ea6-164">Vytvoření virtuální sítě s privátní podsítě:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-164">Create a virtual network with a private subnet:</span></span>

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. <span data-ttu-id="b9ea6-165">Vytvoření veřejné podsítě v rámci virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-165">Create a public subnet within the virtual network:</span></span>

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. <span data-ttu-id="b9ea6-166">Zkontrolujte virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-166">Review the virtual network and subnets:</span></span>

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. <span data-ttu-id="b9ea6-167">**Volitelné**: můžete chtít odstranit prostředky, které jste vytvořili po dokončení tohoto kurzu, tak, aby vám zbytečně nenabíhaly poplatky za používání:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-167">**Optional**: You might want to delete the resources that you created when you finish this tutorial, so that you don't incur usage charges:</span></span>

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> <span data-ttu-id="b9ea6-168">I když nelze zadat skupinu prostředků pro vytvoření virtuální sítě (klasické) pomocí rozhraní příkazového řádku, Azure vytvoří virtuální síť ve skupině prostředků s názvem *výchozí sítě*.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-168">Though you can't specify a resource group to create a virtual network (classic) in using the CLI, Azure creates the virtual network in a resource group named *Default-Networking*.</span></span>

## <a name="powershell"></a><span data-ttu-id="b9ea6-169">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b9ea6-169">PowerShell</span></span>

1. <span data-ttu-id="b9ea6-170">Nainstalujte nejnovější verzi prostředí PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) modulu.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-170">Install the latest version of the PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module.</span></span> <span data-ttu-id="b9ea6-171">Pokud s Azure PowerShellem začínáte, podívejte se na [Přehled Azure PowerShellu](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-171">If you're new to Azure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="b9ea6-172">Spusťte relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-172">Start a PowerShell session.</span></span>
3. <span data-ttu-id="b9ea6-173">V PowerShellu se přihlaste k Azure zadáním příkazu `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-173">In PowerShell, log in to Azure by entering the `Add-AzureAccount` command.</span></span>
4. <span data-ttu-id="b9ea6-174">Změnit následující cestu a název souboru, podle potřeby, a poté exportovat existující konfigurační soubor sítě:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-174">Change the following path and filename, as appropriate, then export your existing network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. <span data-ttu-id="b9ea6-175">Pokud chcete vytvořit virtuální síť s veřejné a privátní podsítě, pomocí libovolného textového editoru přidejte **VirtualNetworkSite** prvek, který následuje do konfiguračního souboru sítě.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-175">To create a virtual network with public and private subnets, use any text editor to add the **VirtualNetworkSite** element that follows to the network configuration file.</span></span>

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    <span data-ttu-id="b9ea6-176">Přečtěte si celý [schéma konfiguračního souboru sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-176">Review the full [network configuration file schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

6. <span data-ttu-id="b9ea6-177">Import konfiguračního souboru sítě:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-177">Import the network configuration file:</span></span>

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > <span data-ttu-id="b9ea6-178">Import konfiguračního souboru změněné sítě může způsobit změny existující virtuální sítě (klasické) v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-178">Importing a changed network configuration file can cause changes to existing virtual networks (classic) in your subscription.</span></span> <span data-ttu-id="b9ea6-179">Ujistěte se, můžete přidat pouze předchozí virtuální sítě a změníte nebo odeberte všechny existující virtuální sítě ze svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-179">Ensure you only add the previous virtual network and that you don't change or remove any existing virtual networks from your subscription.</span></span> 

7. <span data-ttu-id="b9ea6-180">Zkontrolujte virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="b9ea6-180">Review the virtual network and subnets:</span></span>

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. <span data-ttu-id="b9ea6-181">**Volitelné**: můžete chtít odstranit prostředky, které jste vytvořili po dokončení tohoto kurzu, tak, aby vám zbytečně nenabíhaly poplatky za používání.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-181">**Optional**: You might want to delete the resources that you created when you finish this tutorial, so that you don't incur usage charges.</span></span> <span data-ttu-id="b9ea6-182">Pokud chcete odstranit virtuální síť, dokončení kroky 4 až 6 znovu, tento čas odebrání **VirtualNetworkSite** elementu, které jste přidali v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-182">To delete the virtual network, complete steps 4-6 again, this time removing the **VirtualNetworkSite** element you added in step 5.</span></span>
 
> [!NOTE]
> <span data-ttu-id="b9ea6-183">I když nelze zadat skupinu prostředků pro vytvoření virtuální sítě (klasické) pomocí prostředí PowerShell, Azure vytvoří virtuální síť ve skupině prostředků s názvem *výchozí sítě*.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-183">Though you can't specify a resource group to create a virtual network (classic) in using PowerShell, Azure creates the virtual network in a resource group named *Default-Networking*.</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="b9ea6-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9ea6-184">Next steps</span></span>

- <span data-ttu-id="b9ea6-185">Další informace o všech virtuální síť a podsíť nastavení najdete v tématu [spravovat virtuální sítě](virtual-network-manage-network.md) a [spravovat podsítě virtuální sítě](virtual-network-manage-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-185">To learn about all virtual network and subnet settings, see [Manage virtual networks](virtual-network-manage-network.md) and [Manage virtual network subnets](virtual-network-manage-subnet.md).</span></span> <span data-ttu-id="b9ea6-186">Máte různé možnosti pro splňují odlišné požadavky pomocí virtuální sítě a podsítě v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-186">You have various options for using virtual networks and subnets in a production environment to meet different requirements.</span></span>
- <span data-ttu-id="b9ea6-187">Filtrovat podsíť příchozí a odchozí provoz, vytvoření a použití [skupin zabezpečení sítě](virtual-networks-nsg.md) k podsítím.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-187">To filter inbound and outbound subnet traffic, create and apply [network security groups](virtual-networks-nsg.md) to subnets.</span></span>
- <span data-ttu-id="b9ea6-188">Vytvoření [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuálního počítače a připojte ho k existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-188">Create a [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or a [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtual machine, and then connect it to an existing virtual network.</span></span>
- <span data-ttu-id="b9ea6-189">Pro připojení dvou virtuálních sítí ve stejné oblasti Azure, vytvořte [partnerský vztah virtuální sítě](create-peering-different-deployment-models.md) mezi virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-189">To connect two virtual networks in the same Azure location, create a  [virtual network peering](create-peering-different-deployment-models.md) between the virtual networks.</span></span> <span data-ttu-id="b9ea6-190">Virtuální síť (Resource Manager) může rovnocenných počítačů k virtuální síti (klasické), ale nemůžete vytvořit partnerský vztah mezi dvěma virtuálními sítěmi (klasické).</span><span class="sxs-lookup"><span data-stu-id="b9ea6-190">You can peer a virtual network (Resource Manager) to a virtual network (classic), but you cannot create a peering between two virtual networks (classic).</span></span>
- <span data-ttu-id="b9ea6-191">Připojit virtuální síť k místní síti pomocí [brány VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) okruh.</span><span class="sxs-lookup"><span data-stu-id="b9ea6-191">Connect the virtual network to an on-premises network by using a [VPN Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.</span></span>
