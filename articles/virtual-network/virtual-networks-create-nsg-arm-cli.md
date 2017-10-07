---
title: "aaaCreate skupin zabezpečení - sítě, Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin zabezpečení sítě pomocí hello 2.0 rozhraní příkazového řádku Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30b1d60676331bf5e2bbbb046c747477be9d3338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-20"></a><span data-ttu-id="17e72-103">Vytvořit síť pomocí Azure CLI 2.0 hello skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17e72-103">Create network security groups using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="17e72-104">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="17e72-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="17e72-105">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="17e72-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="17e72-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="17e72-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="17e72-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="17e72-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="17e72-108">Ukázka Hello Azure CLI 2.0 příkazy následující očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello předchozí.</span><span class="sxs-lookup"><span data-stu-id="17e72-108">hello sample Azure CLI 2.0 commands following expect a simple environment already created based on hello scenario preceding.</span></span> 

## <a name="create-hello-nsg-for-hello-frontend-subnet"></a><span data-ttu-id="17e72-109">Vytvoření hello skupina NSG pro hello `FrontEnd` podsítě</span><span class="sxs-lookup"><span data-stu-id="17e72-109">Create hello NSG for hello `FrontEnd` subnet</span></span>

<span data-ttu-id="17e72-110">toocreate skupinu NSG s názvem *NSG front-endu* podle hello scénář předchozí, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="17e72-110">toocreate an NSG named *NSG-FrontEnd* based on hello scenario preceding, follow hello steps following.</span></span>

1. <span data-ttu-id="17e72-111">Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="17e72-111">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="17e72-112">Vytvořit skupinu NSG pomocí hello [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-112">Create an NSG using hello [az network nsg create](/cli/azure/network/nsg#create) command.</span></span> 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    <span data-ttu-id="17e72-113">Parametry:</span><span class="sxs-lookup"><span data-stu-id="17e72-113">Parameters:</span></span>
   
   * <span data-ttu-id="17e72-114">`--resource-group`: Název skupiny prostředků hello, kde se má vytvořit hello NSG.</span><span class="sxs-lookup"><span data-stu-id="17e72-114">`--resource-group`: Name of hello resource group where hello NSG is created.</span></span> <span data-ttu-id="17e72-115">V našem scénáři je to *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="17e72-115">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="17e72-116">`--location`: Oblast azure, kde hello nová skupina NSG je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="17e72-116">`--location`: Azure region where hello new NSG is created.</span></span> <span data-ttu-id="17e72-117">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="17e72-117">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="17e72-118">`--name`: Název hello nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="17e72-118">`--name`: Name for hello new NSG.</span></span> <span data-ttu-id="17e72-119">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="17e72-119">For our scenario, *NSG-FrontEnd*.</span></span>

    <span data-ttu-id="17e72-120">Hello očekává, že výstup je poměrně bit informace včetně seznam všech pravidel výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="17e72-120">hello expected output is quite a bit of information including a list of all hello default rules.</span></span> <span data-ttu-id="17e72-121">Hello následující příklad ukazuje hello výchozí pravidla pomocí filtru dotazu JMESPATH hello `table` výstupní formát:</span><span class="sxs-lookup"><span data-stu-id="17e72-121">hello following example shows hello default rules using a JMESPATH query filter with hello `table` output format:</span></span>

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   <span data-ttu-id="17e72-122">Výstup:</span><span class="sxs-lookup"><span data-stu-id="17e72-122">Output:</span></span>

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs tooall VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs tooInternet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. <span data-ttu-id="17e72-123">Vytvořte pravidlo, které umožňuje přístup tooport 3389 (RDP) z hello Internet s hello [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-123">Create a rule that allows access tooport 3389 (RDP) from hello Internet with hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17e72-124">V závislosti na prostředí hello používáte, může být nutné toomodify hello `*` znak v argumentech hello následující tak, aby nebyla tooexpand hello argument před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="17e72-124">Depending on hello shell you are using, you might need toomodify hello `*` character in hello arguments following so as not tooexpand hello argument before execution.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    <span data-ttu-id="17e72-125">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="17e72-125">Expected output:</span></span>
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    <span data-ttu-id="17e72-126">Parametry:</span><span class="sxs-lookup"><span data-stu-id="17e72-126">Parameters:</span></span>

    * <span data-ttu-id="17e72-127">`--resource-group testrg`: hello toouse skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="17e72-127">`--resource-group testrg`: hello resource group toouse.</span></span> <span data-ttu-id="17e72-128">Všimněte si, že se jedná o velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="17e72-128">Note that it is case-insensitive.</span></span>
    * <span data-ttu-id="17e72-129">`--nsg-name NSG-FrontEnd`: Název hello NSG, ve které hello bude vytvořeno pravidlo.</span><span class="sxs-lookup"><span data-stu-id="17e72-129">`--nsg-name NSG-FrontEnd`: Name of hello NSG in which hello rule is created.</span></span>
    * <span data-ttu-id="17e72-130">`--name rdp-rule`: Název pro nové pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="17e72-130">`--name rdp-rule`: Name for hello new rule.</span></span>
    * <span data-ttu-id="17e72-131">`--access Allow`: Úroveň přístupu pro pravidlo hello (zakázat nebo povolit).</span><span class="sxs-lookup"><span data-stu-id="17e72-131">`--access Allow`: Access level for hello rule (Deny or Allow).</span></span>
    * <span data-ttu-id="17e72-132">`--protocol Tcp`: Protokol (Tcp, Udp nebo *).</span><span class="sxs-lookup"><span data-stu-id="17e72-132">`--protocol Tcp`: Protocol (Tcp, Udp, or *).</span></span>
    * <span data-ttu-id="17e72-133">`--direction Inbound`: Směr hello připojení (příchozí nebo odchozí).</span><span class="sxs-lookup"><span data-stu-id="17e72-133">`--direction Inbound`: Direction of hello connection (Inbound or Outbound).</span></span>
    * <span data-ttu-id="17e72-134">`--priority 100`: Prioritu pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="17e72-134">`--priority 100`: Priority for hello rule.</span></span>
    * <span data-ttu-id="17e72-135">`--source-address-prefix Internet`: Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="17e72-135">`--source-address-prefix Internet`: Source address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="17e72-136">`--source-port-range "*"`: Zdrojové portu nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="17e72-136">`--source-port-range "*"`: Source port or port range.</span></span> <span data-ttu-id="17e72-137">Port, který otevírá hello připojení.</span><span class="sxs-lookup"><span data-stu-id="17e72-137">Port that opened hello connection.</span></span>
    * <span data-ttu-id="17e72-138">`--destination-address-prefix "*"`: Předpona cílové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="17e72-138">`--destination-address-prefix "*"`: Destination address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="17e72-139">`--destination-port-range 3389`: Cílový port nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="17e72-139">`--destination-port-range 3389`: Destination port or port range.</span></span> <span data-ttu-id="17e72-140">Port, který obdrží požadavek na připojení hello.</span><span class="sxs-lookup"><span data-stu-id="17e72-140">Port that receives hello connection request.</span></span>



4. <span data-ttu-id="17e72-141">Vytvořte pravidlo, které umožňuje přístup tooport 80 (HTTP) z hello Internet **vytvořit pravidla nsg sítě az** příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-141">Create a rule that allows access tooport 80 (HTTP) from hello Internet **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    <span data-ttu-id="17e72-142">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="17e72-142">Expected putput:</span></span>
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. <span data-ttu-id="17e72-143">Vazby hello NSG toohello **front-endu** podsíť s hello [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-143">Bind hello NSG toohello **FrontEnd** subnet with hello [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command.</span></span>
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    <span data-ttu-id="17e72-144">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="17e72-144">Expected output:</span></span>
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-hello-nsg-for-hello-backend-subnet"></a><span data-ttu-id="17e72-145">Vytvoření hello skupina NSG pro hello `BackEnd` podsítě</span><span class="sxs-lookup"><span data-stu-id="17e72-145">Create hello NSG for hello `BackEnd` subnet</span></span>
<span data-ttu-id="17e72-146">toocreate skupinu NSG s názvem *NSG back-end* podle hello scénář předchozí, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="17e72-146">toocreate an NSG named *NSG-BackEnd* based on hello scenario preceding, follow hello steps following.</span></span>

1. <span data-ttu-id="17e72-147">Vytvoření hello `NSG-BackEnd` NSG s **vytvořit az sítě nsg**.</span><span class="sxs-lookup"><span data-stu-id="17e72-147">Create hello `NSG-BackEnd` NSG with **az network nsg create**.</span></span>
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    <span data-ttu-id="17e72-148">Jako v kroku 2, předchozí hello očekává, že výstup je poměrně rozsáhlé, včetně výchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="17e72-148">As in step 2, preceding, hello expected output is quite large, including default rules.</span></span>
   
2. <span data-ttu-id="17e72-149">Vytvořte pravidlo, které umožňuje přístup tooport 1433 (SQL) z hello `FrontEnd` podsíť s hello **vytvořit pravidla nsg sítě az** příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-149">Create a rule that allows access tooport 1433 (SQL) from hello `FrontEnd` subnet with hello **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    <span data-ttu-id="17e72-150">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="17e72-150">Expected output:</span></span>

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. <span data-ttu-id="17e72-151">Vytvořte pravidlo, které odepře přístup toohello Internetu pomocí hello **vytvořit pravidla nsg sítě az** příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-151">Create a rule that denies access toohello Internet using hello **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    <span data-ttu-id="17e72-152">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="17e72-152">Expected putput:</span></span>
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. <span data-ttu-id="17e72-153">Vazby hello NSG toohello `BackEnd` podsíť pomocí zápisu hello **az sítě vnet podsíť sadu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="17e72-153">Bind hello NSG toohello `BackEnd` subnet using hello **az network vnet subnet set** command.</span></span>
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    <span data-ttu-id="17e72-154">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="17e72-154">Expected output:</span></span>
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```
