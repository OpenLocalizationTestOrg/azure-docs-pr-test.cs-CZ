---
title: "aaaCreate virtuální sítě Azure (klasické) s několika podsítěmi | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální sítě (klasické) s několika podsítěmi v Azure."
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
ms.openlocfilehash: cc7b9ad08d5c26dba09584762bedf614d2847032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a><span data-ttu-id="884a6-103">Vytvoření virtuální sítě (klasické) s několika podsítěmi</span><span class="sxs-lookup"><span data-stu-id="884a6-103">Create a virtual network (classic) with multiple subnets</span></span>

> [!IMPORTANT]
> <span data-ttu-id="884a6-104">Azure má dva [různé modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) pro vytváření a práci s prostředky: Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="884a6-104">Azure has two [different deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) for creating and working with resources: Resource Manager and classic.</span></span> <span data-ttu-id="884a6-105">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="884a6-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="884a6-106">Společnost Microsoft doporučuje vytvoření většina nové virtuální sítě prostřednictvím hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="884a6-106">Microsoft recommends creating most new virtual networks through hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) deployment model.</span></span>

<span data-ttu-id="884a6-107">V tomto kurzu zjistěte, jak toocreate základní virtuální síť Azure (klasický) s oddělit veřejné a privátní podsítě.</span><span class="sxs-lookup"><span data-stu-id="884a6-107">In this tutorial, learn how toocreate a basic Azure virtual network (classic) that has separate public and private subnets.</span></span> <span data-ttu-id="884a6-108">Můžete vytvořit prostředky Azure, jako jsou virtuální počítače a cloudové služby v podsíti.</span><span class="sxs-lookup"><span data-stu-id="884a6-108">You can create Azure resources, like Virtual machines and Cloud services in a subnet.</span></span> <span data-ttu-id="884a6-109">Prostředky vytvořené ve virtuální sítě (klasické) mohou komunikovat navzájem a s prostředky v jiných sítích připojených tooa virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="884a6-109">Resources created in virtual networks (classic) can communicate with each other, and with resources in other networks connected tooa virtual network.</span></span>

<span data-ttu-id="884a6-110">Další informace o všech [virtuální sítě](virtual-network-manage-network.md) a [podsíť](virtual-network-manage-subnet.md) nastavení.</span><span class="sxs-lookup"><span data-stu-id="884a6-110">Learn more about all [virtual network](virtual-network-manage-network.md) and [subnet](virtual-network-manage-subnet.md) settings.</span></span>

> [!WARNING]
> <span data-ttu-id="884a6-111">Virtuální sítě (klasické) se okamžitě odstraní Azure při [předplatné je zakázané](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span><span class="sxs-lookup"><span data-stu-id="884a6-111">Virtual networks (classic) are immediately deleted by Azure when a [subscription is disabled](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span></span> <span data-ttu-id="884a6-112">Bez ohledu na to, jestli existují prostředky ve virtuální síti hello se odstraní virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="884a6-112">Virtual networks (classic) are deleted regardless of whether resources exist in hello virtual network.</span></span> <span data-ttu-id="884a6-113">Pokud později znovu povolíte hello předplatné, je nutné znovu vytvořit prostředky, které existovaly ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="884a6-113">If you later re-enable hello subscription, resources that existed in hello virtual network must be recreated.</span></span>

<span data-ttu-id="884a6-114">Virtuální síť (klasická) můžete vytvořit pomocí hello [portál Azure](#portal), hello [rozhraní příkazového řádku Azure (CLI) 1.0](#azure-cli), nebo [prostředí PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="884a6-114">You can create a virtual network (classic) by using hello [Azure portal](#portal), hello [Azure command-line interface (CLI) 1.0](#azure-cli), or [PowerShell](#powershell).</span></span>

## <a name="portal"></a><span data-ttu-id="884a6-115">Portál</span><span class="sxs-lookup"><span data-stu-id="884a6-115">Portal</span></span>

1. <span data-ttu-id="884a6-116">V internetovém prohlížeči, přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="884a6-116">In an Internet browser, go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="884a6-117">Přihlaste se pomocí vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="884a6-117">Log in using your [Azure account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="884a6-118">Pokud nemáte účet Azure, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="884a6-118">If you don't have an Azure account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="884a6-119">Klikněte na tlačítko **+ nový** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="884a6-119">Click **+New** in hello portal.</span></span>
3. <span data-ttu-id="884a6-120">Zadejte *virtuální síť* v hello **vyhledávání hello Marketplace** pole hello horní části hello **nový** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="884a6-120">Enter *Virtual network* in hello **Search hello Marketplace** box at hello top of hello **New** blade that appears.</span></span>  <span data-ttu-id="884a6-121">Klikněte na tlačítko **virtuální síť** při zobrazí ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="884a6-121">Click **Virtual network** when it appears in hello search results.</span></span>
4. <span data-ttu-id="884a6-122">Vyberte **Classic** v hello **vybrat model nasazení** pole v hello **virtuální sítě** okno, které se zobrazí, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="884a6-122">Select **Classic** in hello **Select a deployment model** box in hello **Virtual Network** blade that appears, then click **Create**.</span></span> 
5. <span data-ttu-id="884a6-123">Zadejte následující hodnoty na hello hello **vytvořit virtuální síť (klasická)** okna a pak klikněte na tlačítko **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="884a6-123">Enter hello following values on hello **Create virtual network (classic)** blade and then click **Create**:</span></span>

    |<span data-ttu-id="884a6-124">Nastavení</span><span class="sxs-lookup"><span data-stu-id="884a6-124">Setting</span></span>|<span data-ttu-id="884a6-125">Hodnota</span><span class="sxs-lookup"><span data-stu-id="884a6-125">Value</span></span>|
    |---|---|
    |<span data-ttu-id="884a6-126">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="884a6-126">Name</span></span>|<span data-ttu-id="884a6-127">myVnet</span><span class="sxs-lookup"><span data-stu-id="884a6-127">myVnet</span></span>|
    |<span data-ttu-id="884a6-128">Adresní prostor</span><span class="sxs-lookup"><span data-stu-id="884a6-128">Address space</span></span>|<span data-ttu-id="884a6-129">10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="884a6-129">10.0.0.0/16</span></span>|
    |<span data-ttu-id="884a6-130">Název podsítě</span><span class="sxs-lookup"><span data-stu-id="884a6-130">Subnet name</span></span>|<span data-ttu-id="884a6-131">Veřejné</span><span class="sxs-lookup"><span data-stu-id="884a6-131">Public</span></span>|
    |<span data-ttu-id="884a6-132">Rozsah adres podsítě</span><span class="sxs-lookup"><span data-stu-id="884a6-132">Subnet address range</span></span>|<span data-ttu-id="884a6-133">10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="884a6-133">10.0.0.0/24</span></span>|
    |<span data-ttu-id="884a6-134">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="884a6-134">Resource group</span></span>|<span data-ttu-id="884a6-135">Nechte **vytvořit nový** vybrané a potom zadejte **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="884a6-135">Leave **Create new** selected, and then enter **myResourceGroup**.</span></span>|
    |<span data-ttu-id="884a6-136">Předplatné a umístění</span><span class="sxs-lookup"><span data-stu-id="884a6-136">Subscription and location</span></span>|<span data-ttu-id="884a6-137">Vyberte vaše předplatné a umístění.</span><span class="sxs-lookup"><span data-stu-id="884a6-137">Select your subscription and location.</span></span>

    <span data-ttu-id="884a6-138">Pokud jste nový tooAzure, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (nazývaná také jen tooas *oblasti*).</span><span class="sxs-lookup"><span data-stu-id="884a6-138">If you're new tooAzure, learn more about [resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (also referred tooas *regions*).</span></span>
4. <span data-ttu-id="884a6-139">Hello portálu můžete vytvořit pouze jednu podsíť při vytváření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="884a6-139">In hello portal, you can create only one subnet when you create a virtual network.</span></span> <span data-ttu-id="884a6-140">V tomto kurzu vytvoříte druhou podsíť po vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="884a6-140">In this tutorial, you create a second subnet after you create hello virtual network.</span></span> <span data-ttu-id="884a6-141">Můžete vytvořit prostředky přístupné z Internetu později v hello **veřejné** podsítě.</span><span class="sxs-lookup"><span data-stu-id="884a6-141">You might later create Internet-accessible resources in hello **Public** subnet.</span></span> <span data-ttu-id="884a6-142">Můžete také vytvořit prostředky, které nejsou přístupné z Internetu hello v hello **privátní** podsítě.</span><span class="sxs-lookup"><span data-stu-id="884a6-142">You also might create resources that aren't accessible from hello Internet in hello **Private** subnet.</span></span> <span data-ttu-id="884a6-143">toocreate hello druhou podsíť, zadejte **myVnet** v hello **vyhledávání prostředků** pole v horní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="884a6-143">toocreate hello second subnet, enter **myVnet** in hello **Search resources** box at hello top of hello page.</span></span> <span data-ttu-id="884a6-144">Klikněte na tlačítko **myVnet** při zobrazí ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="884a6-144">Click **myVnet** when it appears in hello search results.</span></span>
5. <span data-ttu-id="884a6-145">Klikněte na tlačítko **podsítě** (v hello **nastavení** část) na hello **vytvořit virtuální síť (klasická)** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="884a6-145">Click **Subnets** (in hello **SETTINGS** section) on hello **Create virtual network (classic)** blade that appears.</span></span>
6. <span data-ttu-id="884a6-146">Klikněte na tlačítko **+ přidat** na hello **myVnet - podsítě** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="884a6-146">Click **+Add** on hello **myVnet - Subnets** blade that appears.</span></span>
7. <span data-ttu-id="884a6-147">Zadejte **privátní** pro **název** na hello **přidat podsíť** okno.</span><span class="sxs-lookup"><span data-stu-id="884a6-147">Enter **Private** for **Name** on hello **Add subnet** blade.</span></span> <span data-ttu-id="884a6-148">Zadejte **10.0.1.0/24** pro **rozsahu adres**.</span><span class="sxs-lookup"><span data-stu-id="884a6-148">Enter **10.0.1.0/24** for **Address range**.</span></span>  <span data-ttu-id="884a6-149">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="884a6-149">Click **OK**.</span></span>
8. <span data-ttu-id="884a6-150">Na hello **myVnet - podsítě** okně uvidíte hello **veřejné** a **privátní** podsítě, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="884a6-150">On hello **myVnet - Subnets** blade, you can see hello **Public** and **Private** subnets that you created.</span></span>
9. <span data-ttu-id="884a6-151">**Volitelné**: Po dokončení tohoto kurzu můžete toodelete hello prostředky, které jste vytvořili, tak, aby vám zbytečně nenabíhaly poplatky za používání:</span><span class="sxs-lookup"><span data-stu-id="884a6-151">**Optional**: When you finish this tutorial, you might want toodelete hello resources that you created, so that you don't incur usage charges:</span></span>
    - <span data-ttu-id="884a6-152">Klikněte na tlačítko **přehled** na hello **myVnet** okno.</span><span class="sxs-lookup"><span data-stu-id="884a6-152">Click **Overview** on hello **myVnet** blade.</span></span>
    - <span data-ttu-id="884a6-153">Klikněte na tlačítko hello **odstranit** ikonu na hello **myVnet** okno.</span><span class="sxs-lookup"><span data-stu-id="884a6-153">Click hello **Delete** icon on hello **myVnet** blade.</span></span>
    - <span data-ttu-id="884a6-154">tooconfirm hello odstranění, klikněte na tlačítko **Ano** v hello **virtuální sítě odstranit** pole.</span><span class="sxs-lookup"><span data-stu-id="884a6-154">tooconfirm hello deletion, click **Yes** in hello **Delete virtual network** box.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="884a6-155">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="884a6-155">Azure CLI</span></span>

1. <span data-ttu-id="884a6-156">Můžete buď [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), nebo použijte hello rozhraní příkazového řádku v rámci hello prostředí cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="884a6-156">You can either [install and configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), or use hello CLI within hello Azure Cloud Shell.</span></span> <span data-ttu-id="884a6-157">Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="884a6-157">hello Azure Cloud Shell is a free Bash shell that you can run directly within hello Azure portal.</span></span> <span data-ttu-id="884a6-158">Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="884a6-158">It has hello Azure CLI preinstalled and configured toouse with your account.</span></span> <span data-ttu-id="884a6-159">tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `azure <command> --help`.</span><span class="sxs-lookup"><span data-stu-id="884a6-159">tooget help for CLI commands, type `azure <command> --help`.</span></span> 
2. <span data-ttu-id="884a6-160">V relaci příkazového řádku přihlaste se tooAzure s hello příkaz, který následuje.</span><span class="sxs-lookup"><span data-stu-id="884a6-160">In a CLI session, log in tooAzure with hello command that follows.</span></span> <span data-ttu-id="884a6-161">Pokud kliknete na tlačítko **vyzkoušet** pole hello: otevře prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="884a6-161">If you click **Try it** in hello box below, a Cloud Shell opens.</span></span> <span data-ttu-id="884a6-162">Tooyour předplatné, můžete přihlásit bez zadávání hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="884a6-162">You can log in tooyour Azure subscription, without entering hello following command:</span></span>

    ```azurecli-interactive
    azure login
    ```

3. <span data-ttu-id="884a6-163">hello tooensure, rozhraní příkazového řádku je v režimu správy služby, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="884a6-163">tooensure hello CLI is in Service Management mode, enter hello following command:</span></span>

    ```azurecli-interactive
    azure config mode asm
    ```

4. <span data-ttu-id="884a6-164">Vytvoření virtuální sítě s privátní podsítě:</span><span class="sxs-lookup"><span data-stu-id="884a6-164">Create a virtual network with a private subnet:</span></span>

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. <span data-ttu-id="884a6-165">Vytvoření veřejné podsítě v rámci virtuální sítě hello:</span><span class="sxs-lookup"><span data-stu-id="884a6-165">Create a public subnet within hello virtual network:</span></span>

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. <span data-ttu-id="884a6-166">Projděte si hello virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="884a6-166">Review hello virtual network and subnets:</span></span>

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. <span data-ttu-id="884a6-167">**Volitelné**: můžete chtít toodelete hello prostředky, které jste vytvořili po dokončení tohoto kurzu, tak, aby vám zbytečně nenabíhaly poplatky za používání:</span><span class="sxs-lookup"><span data-stu-id="884a6-167">**Optional**: You might want toodelete hello resources that you created when you finish this tutorial, so that you don't incur usage charges:</span></span>

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> <span data-ttu-id="884a6-168">I když toocreate skupiny prostředků virtuální sítě (klasické) nelze zadat pomocí hello CLI, Azure vytvoří hello virtuální sítě ve skupině prostředků s názvem *výchozí sítě*.</span><span class="sxs-lookup"><span data-stu-id="884a6-168">Though you can't specify a resource group toocreate a virtual network (classic) in using hello CLI, Azure creates hello virtual network in a resource group named *Default-Networking*.</span></span>

## <a name="powershell"></a><span data-ttu-id="884a6-169">PowerShell</span><span class="sxs-lookup"><span data-stu-id="884a6-169">PowerShell</span></span>

1. <span data-ttu-id="884a6-170">Nainstalujte nejnovější verzi hello prostředí PowerShell hello [Azure](https://www.powershellgallery.com/packages/Azure) modulu.</span><span class="sxs-lookup"><span data-stu-id="884a6-170">Install hello latest version of hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module.</span></span> <span data-ttu-id="884a6-171">Pokud jste nový tooAzure prostředí PowerShell, najdete v části [Přehled prostředí Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="884a6-171">If you're new tooAzure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="884a6-172">Spusťte relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="884a6-172">Start a PowerShell session.</span></span>
3. <span data-ttu-id="884a6-173">V prostředí PowerShell přihlásit tooAzure zadáním hello `Add-AzureAccount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="884a6-173">In PowerShell, log in tooAzure by entering hello `Add-AzureAccount` command.</span></span>
4. <span data-ttu-id="884a6-174">Změnit hello následující cestu a název souboru, podle potřeby, exportovat pak existující konfigurační soubor sítě:</span><span class="sxs-lookup"><span data-stu-id="884a6-174">Change hello following path and filename, as appropriate, then export your existing network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. <span data-ttu-id="884a6-175">toocreate virtuální síť s podsítí veřejné a privátní, použijte všechny textového editoru tooadd hello **VirtualNetworkSite** prvek, který následuje toohello sítě konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="884a6-175">toocreate a virtual network with public and private subnets, use any text editor tooadd hello **VirtualNetworkSite** element that follows toohello network configuration file.</span></span>

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

    <span data-ttu-id="884a6-176">Úplná kontrola hello [schéma konfiguračního souboru sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="884a6-176">Review hello full [network configuration file schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

6. <span data-ttu-id="884a6-177">Importujte konfigurační soubor hello sítě:</span><span class="sxs-lookup"><span data-stu-id="884a6-177">Import hello network configuration file:</span></span>

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > <span data-ttu-id="884a6-178">Import konfiguračního souboru změněné sítě může způsobit změny tooexisting virtuální sítě (klasické) v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="884a6-178">Importing a changed network configuration file can cause changes tooexisting virtual networks (classic) in your subscription.</span></span> <span data-ttu-id="884a6-179">Ujistěte se, můžete přidat pouze hello předchozí virtuální sítě a změníte nebo odeberte všechny existující virtuální sítě ze svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="884a6-179">Ensure you only add hello previous virtual network and that you don't change or remove any existing virtual networks from your subscription.</span></span> 

7. <span data-ttu-id="884a6-180">Projděte si hello virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="884a6-180">Review hello virtual network and subnets:</span></span>

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. <span data-ttu-id="884a6-181">**Volitelné**: můžete chtít toodelete hello prostředky, které jste vytvořili po dokončení tohoto kurzu, tak, aby vám zbytečně nenabíhaly poplatky za používání.</span><span class="sxs-lookup"><span data-stu-id="884a6-181">**Optional**: You might want toodelete hello resources that you created when you finish this tutorial, so that you don't incur usage charges.</span></span> <span data-ttu-id="884a6-182">toodelete hello virtuální sítě, dokončení kroky 4 až 6 znovu, tento čas odebírání hello **VirtualNetworkSite** elementu, které jste přidali v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="884a6-182">toodelete hello virtual network, complete steps 4-6 again, this time removing hello **VirtualNetworkSite** element you added in step 5.</span></span>
 
> [!NOTE]
> <span data-ttu-id="884a6-183">Když toocreate skupiny prostředků virtuální sítě (klasické) nelze zadat pomocí prostředí PowerShell, Azure vytvoří hello virtuální síť ve skupině prostředků s názvem *výchozí sítě*.</span><span class="sxs-lookup"><span data-stu-id="884a6-183">Though you can't specify a resource group toocreate a virtual network (classic) in using PowerShell, Azure creates hello virtual network in a resource group named *Default-Networking*.</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="884a6-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="884a6-184">Next steps</span></span>

- <span data-ttu-id="884a6-185">toolearn o všechny virtuální sítě a podsítě nastavení, najdete v části [spravovat virtuální sítě](virtual-network-manage-network.md) a [spravovat podsítě virtuální sítě](virtual-network-manage-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="884a6-185">toolearn about all virtual network and subnet settings, see [Manage virtual networks](virtual-network-manage-network.md) and [Manage virtual network subnets](virtual-network-manage-subnet.md).</span></span> <span data-ttu-id="884a6-186">Máte různé možnosti pro použití virtuálními sítěmi a podsítěmi v jiné požadavky produkční prostředí toomeet.</span><span class="sxs-lookup"><span data-stu-id="884a6-186">You have various options for using virtual networks and subnets in a production environment toomeet different requirements.</span></span>
- <span data-ttu-id="884a6-187">toofilter příchozí a odchozí přenosy podsítěmi, vytvoření a použití [skupin zabezpečení sítě](virtual-networks-nsg.md) toosubnets.</span><span class="sxs-lookup"><span data-stu-id="884a6-187">toofilter inbound and outbound subnet traffic, create and apply [network security groups](virtual-networks-nsg.md) toosubnets.</span></span>
- <span data-ttu-id="884a6-188">Vytvoření [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuálního počítače a připojte ho tooan existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="884a6-188">Create a [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or a [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtual machine, and then connect it tooan existing virtual network.</span></span>
- <span data-ttu-id="884a6-189">tooconnect dvou virtuálních sítí v hello stejného umístění Azure, vytvořte [partnerský vztah virtuální sítě](create-peering-different-deployment-models.md) mezi virtuálními sítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="884a6-189">tooconnect two virtual networks in hello same Azure location, create a  [virtual network peering](create-peering-different-deployment-models.md) between hello virtual networks.</span></span> <span data-ttu-id="884a6-190">Mohou párově virtuální sítě (Resource Manager) tooa virtuální sítě (klasické), ale nemůžete vytvořit partnerský vztah mezi dvěma virtuálními sítěmi (klasické).</span><span class="sxs-lookup"><span data-stu-id="884a6-190">You can peer a virtual network (Resource Manager) tooa virtual network (classic), but you cannot create a peering between two virtual networks (classic).</span></span>
- <span data-ttu-id="884a6-191">Připojit pomocí hello virtuální sítě tooan do místní sítě [brány VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) okruh.</span><span class="sxs-lookup"><span data-stu-id="884a6-191">Connect hello virtual network tooan on-premises network by using a [VPN Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.</span></span>
