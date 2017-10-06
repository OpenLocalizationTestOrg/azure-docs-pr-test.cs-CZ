---
title: "aaaCreate virtuální sítě | Šablona Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak toocreate a virtuální sítě pomocí šablony Azure Resource Manager."
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
ms.openlocfilehash: b9c289433ff2a84bec19eac25fa28ab40d131c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a><span data-ttu-id="15a0d-103">Vytvoření virtuální sítě pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="15a0d-103">Create a virtual network using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="15a0d-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="15a0d-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="15a0d-105">Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="15a0d-106">Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="15a0d-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="15a0d-107">Tento článek vysvětluje, jak toocreate virtuální síť prostřednictvím nasazení Resource Manager hello modelu pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="15a0d-107">This article explains how toocreate a VNet through hello Resource Manager deployment model using an Azure Resource Manager template.</span></span> <span data-ttu-id="15a0d-108">Můžete také vytvořit virtuální síť pomocí Resource Manager pomocí jiných nástrojů nebo vytvoření virtuální sítě pomocí modelu nasazení classic hello výběrem jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="15a0d-108">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
- [<span data-ttu-id="15a0d-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="15a0d-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
- [<span data-ttu-id="15a0d-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="15a0d-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
- [<span data-ttu-id="15a0d-111">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="15a0d-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
- [<span data-ttu-id="15a0d-112">Šablona</span><span class="sxs-lookup"><span data-stu-id="15a0d-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
- [<span data-ttu-id="15a0d-113">Portál (Classic)</span><span class="sxs-lookup"><span data-stu-id="15a0d-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
- [<span data-ttu-id="15a0d-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="15a0d-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [<span data-ttu-id="15a0d-115">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="15a0d-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

<span data-ttu-id="15a0d-116">Se dozvíte, jak toodownload a upravit existující šablonu ARM z Githubu a nasazení hello šablony z Githubu, prostředí PowerShell a hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="15a0d-116">You will learn how toodownload and modify and existing ARM template from GitHub, and deploy hello template from GitHub, PowerShell, and hello Azure CLI.</span></span>

<span data-ttu-id="15a0d-117">Pokud jednoduše nasazujete šablony ARM hello přímo z Githubu, beze změn, přeskočte příliš[nasazení šablony z githubu](#deploy-the-arm-template-by-using-click-to-deploy).</span><span class="sxs-lookup"><span data-stu-id="15a0d-117">If you are simply deploying hello ARM template directly from GitHub, without any changes, skip too[deploy a template from github](#deploy-the-arm-template-by-using-click-to-deploy).</span></span>

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-hello-azure-resource-manager-template"></a><span data-ttu-id="15a0d-118">Stažení a pochopení šablony Azure Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="15a0d-118">Download and understand hello Azure Resource Manager template</span></span>
<span data-ttu-id="15a0d-119">Můžete stáhnout existující šablonu hello pro vytvoření virtuální sítě a dvě podsítě z Githubu, proveďte požadované změny, může být vhodné a opakovaně ji používat.</span><span class="sxs-lookup"><span data-stu-id="15a0d-119">You can download hello existing template for creating a VNet and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="15a0d-120">toodo tedy dokončit hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15a0d-120">toodo so, complete hello following steps:</span></span>

1. <span data-ttu-id="15a0d-121">Přejděte příliš[stránku hello Ukázka šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="15a0d-121">Navigate too[hello sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
2. <span data-ttu-id="15a0d-122">Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-122">Click **azuredeploy.json**, and then click **RAW**.</span></span>
3. <span data-ttu-id="15a0d-123">Uložte soubor tooa hello do místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="15a0d-123">Save hello file tooa a local folder on your computer.</span></span>
4. <span data-ttu-id="15a0d-124">Pokud jste obeznámeni s šablonami, přeskočte toostep 7.</span><span class="sxs-lookup"><span data-stu-id="15a0d-124">If you are familiar with templates, skip toostep 7.</span></span>
5. <span data-ttu-id="15a0d-125">Otevřete soubor hello jste právě uložili a prohlédněte si hello obsah v části **parametry** na řádku 5.</span><span class="sxs-lookup"><span data-stu-id="15a0d-125">Open hello file you just saved and look at hello contents under **parameters** in line 5.</span></span> <span data-ttu-id="15a0d-126">Parametry šablony ARM představují zástupce hodnot, které můžete doplnit během nasazování.</span><span class="sxs-lookup"><span data-stu-id="15a0d-126">ARM template parameters provide a placeholder for values that can be filled out during deployment.</span></span>
   
   | <span data-ttu-id="15a0d-127">Parametr</span><span class="sxs-lookup"><span data-stu-id="15a0d-127">Parameter</span></span> | <span data-ttu-id="15a0d-128">Popis</span><span class="sxs-lookup"><span data-stu-id="15a0d-128">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="15a0d-129">**location**</span><span class="sxs-lookup"><span data-stu-id="15a0d-129">**location**</span></span> |<span data-ttu-id="15a0d-130">Oblast Azure, kde bude vytvořena hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="15a0d-130">Azure region where hello VNet will be created</span></span> |
   | <span data-ttu-id="15a0d-131">**vnetName**</span><span class="sxs-lookup"><span data-stu-id="15a0d-131">**vnetName**</span></span> |<span data-ttu-id="15a0d-132">Název hello nové sítě VNet</span><span class="sxs-lookup"><span data-stu-id="15a0d-132">Name for hello new VNet</span></span> |
   | <span data-ttu-id="15a0d-133">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="15a0d-133">**addressPrefix**</span></span> |<span data-ttu-id="15a0d-134">Adresní prostor sítě VNet, hello ve formátu CIDR</span><span class="sxs-lookup"><span data-stu-id="15a0d-134">Address space for hello VNet, in CIDR format</span></span> |
   | <span data-ttu-id="15a0d-135">**subnet1Name**</span><span class="sxs-lookup"><span data-stu-id="15a0d-135">**subnet1Name**</span></span> |<span data-ttu-id="15a0d-136">Název hello první sítě VNet</span><span class="sxs-lookup"><span data-stu-id="15a0d-136">Name for hello first VNet</span></span> |
   | <span data-ttu-id="15a0d-137">**subnet1Prefix**</span><span class="sxs-lookup"><span data-stu-id="15a0d-137">**subnet1Prefix**</span></span> |<span data-ttu-id="15a0d-138">Blok CIDR pro první podsíť hello</span><span class="sxs-lookup"><span data-stu-id="15a0d-138">CIDR block for hello first subnet</span></span> |
   | <span data-ttu-id="15a0d-139">**subnet2Name**</span><span class="sxs-lookup"><span data-stu-id="15a0d-139">**subnet2Name**</span></span> |<span data-ttu-id="15a0d-140">Název hello druhé sítě VNet</span><span class="sxs-lookup"><span data-stu-id="15a0d-140">Name for hello second VNet</span></span> |
   | <span data-ttu-id="15a0d-141">**subnet2Prefix**</span><span class="sxs-lookup"><span data-stu-id="15a0d-141">**subnet2Prefix**</span></span> |<span data-ttu-id="15a0d-142">Blok CIDR pro druhou podsíť hello</span><span class="sxs-lookup"><span data-stu-id="15a0d-142">CIDR block for hello second subnet</span></span> |
   
   > [!IMPORTANT]
   > <span data-ttu-id="15a0d-143">Šablony Azure Resource Manageru udržované na webu GitHub se můžou časem změnit.</span><span class="sxs-lookup"><span data-stu-id="15a0d-143">Azure Resource Manager templates maintained in GitHub can change over time.</span></span> <span data-ttu-id="15a0d-144">Zajistěte, aby že Zkontrolujte šablonu, hello před jeho použitím.</span><span class="sxs-lookup"><span data-stu-id="15a0d-144">Make sure you check hello template before using it.</span></span>
   > 
   > 
6. <span data-ttu-id="15a0d-145">Zkontrolujte obsah hello v části **prostředky** a Všimněte si následujících hello:</span><span class="sxs-lookup"><span data-stu-id="15a0d-145">Check hello content under **resources** and notice hello following:</span></span>
   
   * <span data-ttu-id="15a0d-146">**type**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-146">**type**.</span></span> <span data-ttu-id="15a0d-147">Typ prostředku vytvořeného šablonou hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-147">Type of resource being created by hello template.</span></span> <span data-ttu-id="15a0d-148">V tomto případě se jedná o prostředek **Microsoft.Network/virtualNetworks**, který reprezentuje síť VNet.</span><span class="sxs-lookup"><span data-stu-id="15a0d-148">In this case, **Microsoft.Network/virtualNetworks**, which represent a VNet.</span></span>
   * <span data-ttu-id="15a0d-149">**name**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-149">**name**.</span></span> <span data-ttu-id="15a0d-150">Název prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-150">Name for hello resource.</span></span> <span data-ttu-id="15a0d-151">Všimněte si použití hello **[parameters('vnetName')]**, což znamená hello se název zadaný jako vstup hello uživatelem nebo ze souboru parametrů během nasazování.</span><span class="sxs-lookup"><span data-stu-id="15a0d-151">Notice hello use of **[parameters('vnetName')]**, which means hello name will provided as input by hello user or a parameter file during deployment.</span></span>
   * <span data-ttu-id="15a0d-152">**properties**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-152">**properties**.</span></span> <span data-ttu-id="15a0d-153">Seznam vlastností pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-153">List of properties for hello resource.</span></span> <span data-ttu-id="15a0d-154">Tato šablona používá během vytváření sítě VNet vlastnosti prostoru a podsítě adresy hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-154">This template uses hello address space and subnet properties during VNet creation.</span></span>
7. <span data-ttu-id="15a0d-155">Vraťte se zpět příliš[stránku hello Ukázka šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="15a0d-155">Navigate back too[hello sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
8. <span data-ttu-id="15a0d-156">Klikněte na **azuredeploy-paremeters.json** a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-156">Click **azuredeploy-paremeters.json**, and then click **RAW**.</span></span>
9. <span data-ttu-id="15a0d-157">Uložte soubor tooa hello do místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="15a0d-157">Save hello file tooa a local folder on your computer.</span></span>
10. <span data-ttu-id="15a0d-158">Otevřete hello soubor, který jste právě uložili a upravte hello hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-158">Open hello file you just saved and edit hello values for hello parameters.</span></span> <span data-ttu-id="15a0d-159">Použijte následující hodnoty menší než toodeploy hello sítě VNet popsané v hello scénáři hello:</span><span class="sxs-lookup"><span data-stu-id="15a0d-159">Use hello following values below toodeploy hello VNet described in hello scenario:</span></span>

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

11. <span data-ttu-id="15a0d-160">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="15a0d-160">Save hello file.</span></span>


## <a name="deploy-hello-template-using-powershell"></a><span data-ttu-id="15a0d-161">Nasazení šablony hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="15a0d-161">Deploy hello template using PowerShell</span></span>

<span data-ttu-id="15a0d-162">Proveďte následující kroky toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="15a0d-162">Complete hello following steps toodeploy hello template you downloaded by using PowerShell:</span></span>

1. <span data-ttu-id="15a0d-163">Instalace a konfigurace prostředí Azure PowerShell pomocí kroků hello v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="15a0d-163">Install and configure Azure PowerShell by completing hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="15a0d-164">Spusťte následující příkaz toocreate hello novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="15a0d-164">Run hello following command toocreate a new resource group:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="15a0d-165">příkaz Hello vytvoří skupinu prostředků s názvem *TestRG* v hello *střed USA* oblast azure.</span><span class="sxs-lookup"><span data-stu-id="15a0d-165">hello command creates a resource group named *TestRG* in hello *Central US* azure region.</span></span> <span data-ttu-id="15a0d-166">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="15a0d-166">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="15a0d-167">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="15a0d-167">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. <span data-ttu-id="15a0d-168">Spuštění hello následující příkaz toodeploy hello nové sítě VNet pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozích krocích:</span><span class="sxs-lookup"><span data-stu-id="15a0d-168">Run hello following command toodeploy hello new VNet using hello template and parameter files you downloaded and modified above:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    <span data-ttu-id="15a0d-169">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="15a0d-169">Expected output:</span></span>
   
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
4. <span data-ttu-id="15a0d-170">Hello spusťte následující příkaz tooview hello vlastnosti hello nové virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="15a0d-170">Run hello following command tooview hello properties of hello new VNet:</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    <span data-ttu-id="15a0d-171">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="15a0d-171">Expected output:</span></span>

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

## <a name="deploy-hello-template-using-click-to-deploy"></a><span data-ttu-id="15a0d-172">Nasazení šablony hello pomocí, klikněte na tlačítko nasadit</span><span class="sxs-lookup"><span data-stu-id="15a0d-172">Deploy hello template using click-to-deploy</span></span>

<span data-ttu-id="15a0d-173">Předem definované Azure Resource Manager šablony nahrané tooa úložiště GitHub spravován společností Microsoft a otevřete toohello komunity můžete znovu použít.</span><span class="sxs-lookup"><span data-stu-id="15a0d-173">You can reuse pre-defined Azure Resource Manager templates uploaded tooa GitHub repository maintained by Microsoft and open toohello community.</span></span> <span data-ttu-id="15a0d-174">Tyto šablony můžete nasazovat přímo z Githubu, nebo stáhli a upravili toofit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="15a0d-174">These templates can be deployed straight out of GitHub, or downloaded and modified toofit your needs.</span></span> <span data-ttu-id="15a0d-175">toodeploy šablonu, která vytvoří síť VNet se dvěma podsítěmi, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15a0d-175">toodeploy a template that creates a VNet with two subnets, complete hello following steps:</span></span>

1. <span data-ttu-id="15a0d-176">V prohlížeči přejděte příliš[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="15a0d-176">From a browser, navigate too[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
2. <span data-ttu-id="15a0d-177">Posuňte se dolů hello seznam šablon a klikněte na tlačítko **101-vnet-two-subnets**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-177">Scroll down hello list of templates, and click **101-vnet-two-subnets**.</span></span> <span data-ttu-id="15a0d-178">Zkontrolujte hello **README.md** souboru, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="15a0d-178">Check hello **README.md** file, as shown below.</span></span>

    ![Soubor READEME.md na webu GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. <span data-ttu-id="15a0d-180">Klikněte na tlačítko **nasazení tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-180">Click **Deploy tooAzure**.</span></span> <span data-ttu-id="15a0d-181">V případě potřeby zadejte své přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="15a0d-181">If necessary, enter your Azure login credentials.</span></span> 
4. <span data-ttu-id="15a0d-182">V hello **parametry** okno, zadejte hodnoty hello toouse toocreate nové sítě VNet a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-182">In hello **Parameters** blade, enter hello values you want toouse toocreate your new VNet, and then click **OK**.</span></span> <span data-ttu-id="15a0d-183">Hello následující obrázek znázorňuje hello hodnoty pro scénář hello:</span><span class="sxs-lookup"><span data-stu-id="15a0d-183">hello following figure shows hello values for hello scenario:</span></span>
   
    ![Parametry šablony ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. <span data-ttu-id="15a0d-185">Klikněte na tlačítko **skupiny prostředků** a vyberte hello mezi virtuálními tooadd skupiny prostředků, nebo klikněte na **vytvořit nový** tooadd hello virtuální síť tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="15a0d-185">Click **Resource group** and select a resource group tooadd hello VNet to, or click **Create new** tooadd hello VNet tooa new resource group.</span></span> <span data-ttu-id="15a0d-186">Hello následující obrázek znázorňuje hello prostředků nastavení skupiny pro novou skupinu prostředků s názvem **TestRG**:</span><span class="sxs-lookup"><span data-stu-id="15a0d-186">hello following figure shows hello resource group settings for a new resource group called **TestRG**:</span></span>

    ![Skupina prostředků](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. <span data-ttu-id="15a0d-188">V případě potřeby změňte hello **předplatné** a **umístění** nastavení pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="15a0d-188">If necessary, change hello **Subscription** and **Location** settings for your VNet.</span></span>
7. <span data-ttu-id="15a0d-189">Pokud nechcete, aby toosee hello virtuální síť jako dlaždici v hello **úvodní panel**, zakažte **Pin tooStartboard**.</span><span class="sxs-lookup"><span data-stu-id="15a0d-189">If you do not want toosee hello VNet as a tile in hello **Startboard**, disable **Pin tooStartboard**.</span></span>
8. <span data-ttu-id="15a0d-190">Klikněte na tlačítko **právní podmínky**, přečtěte si podmínky hello a klikněte na tlačítko **koupit** tooagree.</span><span class="sxs-lookup"><span data-stu-id="15a0d-190">Click **Legal terms**, read hello terms, and click **Buy** tooagree.</span></span> 
9. <span data-ttu-id="15a0d-191">Klikněte na tlačítko **vytvořit** toocreate hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="15a0d-191">Click **Create** toocreate hello VNet.</span></span>
   
    ![Dlaždice Odesílá se nasazení na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. <span data-ttu-id="15a0d-193">Po dokončení nasazení hello v hello portálu Azure klikněte na **další služby**, typ *virtuální sítě* hello pole filtru, který se zobrazí, pak klikněte na virtuální sítě toosee hello virtuální sítě okno.</span><span class="sxs-lookup"><span data-stu-id="15a0d-193">Once hello deployment is complete, in hello Azure portal click **More services**, type *virtual networks* in hello filter box that appears, then click Virtual networks toosee hello Virtual networks blade.</span></span> <span data-ttu-id="15a0d-194">V okně hello, klikněte na tlačítko *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="15a0d-194">In hello blade, click *TestVNet*.</span></span> <span data-ttu-id="15a0d-195">V hello *TestVNet* okně klikněte na tlačítko **podsítě** toosee hello vytvořit podsítě, jak je znázorněno v následujícím obrázku hello:</span><span class="sxs-lookup"><span data-stu-id="15a0d-195">In hello *TestVNet* blade, click **Subnets** toosee hello created subnets, as shown in hello following picture:</span></span>
    
     ![Vytvoření sítě VNet na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a><span data-ttu-id="15a0d-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15a0d-197">Next steps</span></span>

<span data-ttu-id="15a0d-198">Zjistěte, jak tooconnect:</span><span class="sxs-lookup"><span data-stu-id="15a0d-198">Learn how tooconnect:</span></span>

- <span data-ttu-id="15a0d-199">Virtuální síť virtuálních počítačů (VM) tooa ve čtení hello [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-portal.md) články.</span><span class="sxs-lookup"><span data-stu-id="15a0d-199">A virtual machine (VM) tooa virtual network by reading hello [Create a Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) or [Create a Linux VM](../virtual-machines/linux/quick-create-portal.md) articles.</span></span> <span data-ttu-id="15a0d-200">Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.</span><span class="sxs-lookup"><span data-stu-id="15a0d-200">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="15a0d-201">Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) článku.</span><span class="sxs-lookup"><span data-stu-id="15a0d-201">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="15a0d-202">Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15a0d-202">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="15a0d-203">Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-arm.md) články.</span><span class="sxs-lookup"><span data-stu-id="15a0d-203">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
