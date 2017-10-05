---
title: "Vytvoření virtuální sítě | Šablona Azure Resource Manageru | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81602766848a91331c8d811ea1c8ec3ffae44b96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a><span data-ttu-id="5bf8a-103">Vytvoření virtuální sítě pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5bf8a-103">Create a virtual network using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="5bf8a-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="5bf8a-105">Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="5bf8a-106">Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="5bf8a-107">Tento článek vysvětluje, jak vytvořit virtuální síť pomocí modelu nasazení Resource Manager pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-107">This article explains how to create a VNet through the Resource Manager deployment model using an Azure Resource Manager template.</span></span> <span data-ttu-id="5bf8a-108">Virtuální síť můžete vytvořit také prostřednictvím modelu nasazení Resource Manager pomocí jiných nástrojů nebo prostřednictvím modelu nasazení Classic. Pokud to chcete provést, vyberte odpovídající možnost z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-108">You can also create a VNet through Resource Manager using other tools or create a VNet through the classic deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
- [<span data-ttu-id="5bf8a-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5bf8a-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
- [<span data-ttu-id="5bf8a-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5bf8a-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
- [<span data-ttu-id="5bf8a-111">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5bf8a-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
- [<span data-ttu-id="5bf8a-112">Šablona</span><span class="sxs-lookup"><span data-stu-id="5bf8a-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
- [<span data-ttu-id="5bf8a-113">Portál (Classic)</span><span class="sxs-lookup"><span data-stu-id="5bf8a-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
- [<span data-ttu-id="5bf8a-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="5bf8a-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [<span data-ttu-id="5bf8a-115">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="5bf8a-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

<span data-ttu-id="5bf8a-116">Dozvíte se, jak stáhnout a upravit existující šablonu ARM z GitHubu a jak ji nasadit z GitHubu, prostředí PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-116">You will learn how to download and modify and existing ARM template from GitHub, and deploy the template from GitHub, PowerShell, and the Azure CLI.</span></span>

<span data-ttu-id="5bf8a-117">Pokud šablonu ARM jednoduše nasazujete přímo z GitHubu, beze změn, přejděte k části [nasazení šablony z githubu](#deploy-the-arm-template-by-using-click-to-deploy).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-117">If you are simply deploying the ARM template directly from GitHub, without any changes, skip to [deploy a template from github](#deploy-the-arm-template-by-using-click-to-deploy).</span></span>

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-the-azure-resource-manager-template"></a><span data-ttu-id="5bf8a-118">Stažení a pochopení šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="5bf8a-118">Download and understand the Azure Resource Manager template</span></span>
<span data-ttu-id="5bf8a-119">Můžete stáhnout existující šablonu pro vytvoření virtuální sítě a dvě podsítě z Githubu, proveďte požadované změny, může být vhodné a opakovaně ji používat.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-119">You can download the existing template for creating a VNet and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="5bf8a-120">Uděláte to tak, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-120">To do so, complete the following steps:</span></span>

1. <span data-ttu-id="5bf8a-121">Přejděte na stránku [vzorové šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-121">Navigate to [the sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
2. <span data-ttu-id="5bf8a-122">Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-122">Click **azuredeploy.json**, and then click **RAW**.</span></span>
3. <span data-ttu-id="5bf8a-123">Uložte soubor do místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-123">Save the file to a a local folder on your computer.</span></span>
4. <span data-ttu-id="5bf8a-124">Pokud jste obeznámeni s šablonami, přejděte ke kroku 7.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-124">If you are familiar with templates, skip to step 7.</span></span>
5. <span data-ttu-id="5bf8a-125">Otevřete soubor, který jste právě stáhli, a prohlédněte si jeho obsah v části **parameters** na řádku 5.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-125">Open the file you just saved and look at the contents under **parameters** in line 5.</span></span> <span data-ttu-id="5bf8a-126">Parametry šablony ARM představují zástupce hodnot, které můžete doplnit během nasazování.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-126">ARM template parameters provide a placeholder for values that can be filled out during deployment.</span></span>
   
   | <span data-ttu-id="5bf8a-127">Parametr</span><span class="sxs-lookup"><span data-stu-id="5bf8a-127">Parameter</span></span> | <span data-ttu-id="5bf8a-128">Popis</span><span class="sxs-lookup"><span data-stu-id="5bf8a-128">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="5bf8a-129">**location**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-129">**location**</span></span> |<span data-ttu-id="5bf8a-130">Oblast Azure, ve které bude síť VNet vytvořena</span><span class="sxs-lookup"><span data-stu-id="5bf8a-130">Azure region where the VNet will be created</span></span> |
   | <span data-ttu-id="5bf8a-131">**vnetName**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-131">**vnetName**</span></span> |<span data-ttu-id="5bf8a-132">Název nové sítě VNet</span><span class="sxs-lookup"><span data-stu-id="5bf8a-132">Name for the new VNet</span></span> |
   | <span data-ttu-id="5bf8a-133">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-133">**addressPrefix**</span></span> |<span data-ttu-id="5bf8a-134">Adresní prostor sítě VNet ve formátu CIDR</span><span class="sxs-lookup"><span data-stu-id="5bf8a-134">Address space for the VNet, in CIDR format</span></span> |
   | <span data-ttu-id="5bf8a-135">**subnet1Name**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-135">**subnet1Name**</span></span> |<span data-ttu-id="5bf8a-136">Název první sítě VNet</span><span class="sxs-lookup"><span data-stu-id="5bf8a-136">Name for the first VNet</span></span> |
   | <span data-ttu-id="5bf8a-137">**subnet1Prefix**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-137">**subnet1Prefix**</span></span> |<span data-ttu-id="5bf8a-138">Blok CIDR pro první podsíť</span><span class="sxs-lookup"><span data-stu-id="5bf8a-138">CIDR block for the first subnet</span></span> |
   | <span data-ttu-id="5bf8a-139">**subnet2Name**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-139">**subnet2Name**</span></span> |<span data-ttu-id="5bf8a-140">Název druhé sítě VNet</span><span class="sxs-lookup"><span data-stu-id="5bf8a-140">Name for the second VNet</span></span> |
   | <span data-ttu-id="5bf8a-141">**subnet2Prefix**</span><span class="sxs-lookup"><span data-stu-id="5bf8a-141">**subnet2Prefix**</span></span> |<span data-ttu-id="5bf8a-142">Blok CIDR pro druhou podsíť</span><span class="sxs-lookup"><span data-stu-id="5bf8a-142">CIDR block for the second subnet</span></span> |
   
   > [!IMPORTANT]
   > <span data-ttu-id="5bf8a-143">Šablony Azure Resource Manageru udržované na webu GitHub se můžou časem změnit.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-143">Azure Resource Manager templates maintained in GitHub can change over time.</span></span> <span data-ttu-id="5bf8a-144">Před použitím proto šablonu vždy nejprve zkontrolujte.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-144">Make sure you check the template before using it.</span></span>
   > 
   > 
6. <span data-ttu-id="5bf8a-145">Prohlédněte si obsah v části **resources** a všimněte si následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-145">Check the content under **resources** and notice the following:</span></span>
   
   * <span data-ttu-id="5bf8a-146">**type**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-146">**type**.</span></span> <span data-ttu-id="5bf8a-147">Typ prostředku vytvořeného šablonou.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-147">Type of resource being created by the template.</span></span> <span data-ttu-id="5bf8a-148">V tomto případě se jedná o prostředek **Microsoft.Network/virtualNetworks**, který reprezentuje síť VNet.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-148">In this case, **Microsoft.Network/virtualNetworks**, which represent a VNet.</span></span>
   * <span data-ttu-id="5bf8a-149">**name**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-149">**name**.</span></span> <span data-ttu-id="5bf8a-150">Název prostředku.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-150">Name for the resource.</span></span> <span data-ttu-id="5bf8a-151">Všimněte si volání **[parameters('vnetName')]**, které znamená, že název bude dodán jako vstup uživatelem nebo ze souboru parametrů během nasazování.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-151">Notice the use of **[parameters('vnetName')]**, which means the name will provided as input by the user or a parameter file during deployment.</span></span>
   * <span data-ttu-id="5bf8a-152">**properties**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-152">**properties**.</span></span> <span data-ttu-id="5bf8a-153">Seznam vlastností prostředku.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-153">List of properties for the resource.</span></span> <span data-ttu-id="5bf8a-154">Tato šablona používá během vytváření sítě VNet vlastnosti adresního prostoru a podsítě.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-154">This template uses the address space and subnet properties during VNet creation.</span></span>
7. <span data-ttu-id="5bf8a-155">Přejděte zpět na stránku [vzorové šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-155">Navigate back to [the sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
8. <span data-ttu-id="5bf8a-156">Klikněte na **azuredeploy-paremeters.json** a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-156">Click **azuredeploy-paremeters.json**, and then click **RAW**.</span></span>
9. <span data-ttu-id="5bf8a-157">Uložte soubor do místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-157">Save the file to a a local folder on your computer.</span></span>
10. <span data-ttu-id="5bf8a-158">Otevřete soubor, který jste právě uložili, a upravte hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-158">Open the file you just saved and edit the values for the parameters.</span></span> <span data-ttu-id="5bf8a-159">Pro nasazení sítě VNet popsané v tomto scénáři použijte níže uvedené následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-159">Use the following values below to deploy the VNet described in the scenario:</span></span>

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. <span data-ttu-id="5bf8a-160">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-160">Save the file.</span></span>


## <a name="deploy-the-template-using-powershell"></a><span data-ttu-id="5bf8a-161">Šablonu nasadit pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5bf8a-161">Deploy the template using PowerShell</span></span>

<span data-ttu-id="5bf8a-162">Proveďte následující kroky k nasazení šablony, které jste stáhli pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-162">Complete the following steps to deploy the template you downloaded by using PowerShell:</span></span>

1. <span data-ttu-id="5bf8a-163">Instalace a konfigurace prostředí Azure PowerShell pomocí kroků v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-163">Install and configure Azure PowerShell by completing the steps in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="5bf8a-164">Spuštěním následujícího příkazu vytvoříte novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-164">Run the following command to create a new resource group:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="5bf8a-165">Příkaz vytvoří skupinu prostředků s názvem *TestRG* v *střed USA* oblast azure.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-165">The command creates a resource group named *TestRG* in the *Central US* azure region.</span></span> <span data-ttu-id="5bf8a-166">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-166">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="5bf8a-167">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-167">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. <span data-ttu-id="5bf8a-168">Spusťte následující příkaz, který nasadíte novou síť VNet pomocí šablony a parametr soubory stáhli a upravili v předchozích krocích:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-168">Run the following command to deploy the new VNet using the template and parameter files you downloaded and modified above:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    <span data-ttu-id="5bf8a-169">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-169">Expected output:</span></span>
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. <span data-ttu-id="5bf8a-170">Spusťte následující příkaz zobrazíte vlastnosti nové sítě vnet:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-170">Run the following command to view the properties of the new VNet:</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    <span data-ttu-id="5bf8a-171">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-171">Expected output:</span></span>

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-the-template-using-click-to-deploy"></a><span data-ttu-id="5bf8a-172">Šablonu nasadit pomocí, klikněte na tlačítko nasadit</span><span class="sxs-lookup"><span data-stu-id="5bf8a-172">Deploy the template using click-to-deploy</span></span>

<span data-ttu-id="5bf8a-173">Můžete opakovaně používat předdefinované šablony Azure Resource Manager nahrán do úložiště GitHub spravován společností Microsoft a otevřené celé komunitě.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-173">You can reuse pre-defined Azure Resource Manager templates uploaded to a GitHub repository maintained by Microsoft and open to the community.</span></span> <span data-ttu-id="5bf8a-174">Tyto šablony je možné nasazovat přímo z Githubu, nebo stáhnout a upravit tak, aby vyhovovaly vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-174">These templates can be deployed straight out of GitHub, or downloaded and modified to fit your needs.</span></span> <span data-ttu-id="5bf8a-175">Pokud chcete nasadit šablonu, která vytvoří síť VNet se dvěma podsítěmi, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-175">To deploy a template that creates a VNet with two subnets, complete the following steps:</span></span>

1. <span data-ttu-id="5bf8a-176">V prohlížeči přejděte na stránku [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-176">From a browser, navigate to [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
2. <span data-ttu-id="5bf8a-177">Přejděte dolů v seznamu šablon a klikněte na šablonu **101-vnet-two-subnets**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-177">Scroll down the list of templates, and click **101-vnet-two-subnets**.</span></span> <span data-ttu-id="5bf8a-178">Podívejte se do souboru **README.md** znázorněného níže.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-178">Check the **README.md** file, as shown below.</span></span>

    ![Soubor READEME.md na webu GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. <span data-ttu-id="5bf8a-180">Klikněte na **Deploy to Azure** (Nasadit do Azure).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-180">Click **Deploy to Azure**.</span></span> <span data-ttu-id="5bf8a-181">V případě potřeby zadejte své přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-181">If necessary, enter your Azure login credentials.</span></span> 
4. <span data-ttu-id="5bf8a-182">V okně **Parametry** zadejte hodnoty, které chcete použít k vytvoření nové sítě VNet, a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-182">In the **Parameters** blade, enter the values you want to use to create your new VNet, and then click **OK**.</span></span> <span data-ttu-id="5bf8a-183">Následující obrázek znázorňuje hodnoty pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-183">The following figure shows the values for the scenario:</span></span>
   
    ![Parametry šablony ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. <span data-ttu-id="5bf8a-185">Klikněte na **Skupina prostředků** a vyberte skupinu prostředků, do které chcete přidat síť VNet, případně klikněte na **Vytvořit novou** a přidejte síť VNet do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-185">Click **Resource group** and select a resource group to add the VNet to, or click **Create new** to add the VNet to a new resource group.</span></span> <span data-ttu-id="5bf8a-186">Následující obrázek znázorňuje prostředku nastavení skupiny pro novou skupinu prostředků s názvem **TestRG**:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-186">The following figure shows the resource group settings for a new resource group called **TestRG**:</span></span>

    ![Skupina prostředků](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. <span data-ttu-id="5bf8a-188">V případě potřeby změňte nastavení **Předplatné** a **Umístění** sítě VNet.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-188">If necessary, change the **Subscription** and **Location** settings for your VNet.</span></span>
7. <span data-ttu-id="5bf8a-189">Pokud nechcete tuto síť VNet zobrazovat jako dlaždici v zobrazení **Úvodní panel**, zakažte možnost **Připnout na Úvodní panel**.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-189">If you do not want to see the VNet as a tile in the **Startboard**, disable **Pin to Startboard**.</span></span>
8. <span data-ttu-id="5bf8a-190">Klikněte na tlačítko **právní podmínky**, přečtěte si podmínky a klikněte na tlačítko **koupit** souhlasit.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-190">Click **Legal terms**, read the terms, and click **Buy** to agree.</span></span> 
9. <span data-ttu-id="5bf8a-191">Kliknutím na **Vytvořit** vytvořte síť VNet.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-191">Click **Create** to create the VNet.</span></span>
   
    ![Dlaždice Odesílá se nasazení na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. <span data-ttu-id="5bf8a-193">Po dokončení v portálu Azure klikněte na nasazení **další služby**, typ *virtuální sítě* do pole filtr, který se zobrazí, pak klikněte na virtuální sítě najdete v okně virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-193">Once the deployment is complete, in the Azure portal click **More services**, type *virtual networks* in the filter box that appears, then click Virtual networks to see the Virtual networks blade.</span></span> <span data-ttu-id="5bf8a-194">V okně klikněte na tlačítko *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-194">In the blade, click *TestVNet*.</span></span> <span data-ttu-id="5bf8a-195">V *TestVNet* okně klikněte na tlačítko **podsítě** zobrazíte vytvořený podsítě, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-195">In the *TestVNet* blade, click **Subnets** to see the created subnets, as shown in the following picture:</span></span>
    
     ![Vytvoření sítě VNet na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a><span data-ttu-id="5bf8a-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5bf8a-197">Next steps</span></span>

<span data-ttu-id="5bf8a-198">Zjistěte, jak připojit:</span><span class="sxs-lookup"><span data-stu-id="5bf8a-198">Learn how to connect:</span></span>

- <span data-ttu-id="5bf8a-199">Virtuální počítač k virtuální síti pomocí informací v článcích [Vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [Vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-199">A virtual machine (VM) to a virtual network by reading the [Create a Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) or [Create a Linux VM](../virtual-machines/linux/quick-create-portal.md) articles.</span></span> <span data-ttu-id="5bf8a-200">Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-200">Instead of creating a VNet and subnet in the steps of the articles, you can select an existing VNet and subnet to connect a VM to.</span></span>
- <span data-ttu-id="5bf8a-201">Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-201">The virtual network to other virtual networks by reading the [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="5bf8a-202">Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5bf8a-202">The virtual network to an on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="5bf8a-203">Informace najdete v článcích [Připojení virtuální sítě k místní síti pomocí sítě VPN typu Site-to-Site](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [Propojení virtuální sítě s okruhem ExpressRoute](../expressroute/expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5bf8a-203">Learn how by reading the [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
