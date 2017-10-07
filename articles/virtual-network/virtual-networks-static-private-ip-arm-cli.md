---
title: "aaaConfigure privátních IP adres pro virtuální počítače - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače pomocí rozhraní příkazového řádku Azure (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0e278e6ac63c0cda061cf70ab0edfaff5491c03b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-20"></a><span data-ttu-id="5a25d-103">Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5a25d-103">Configure private IP addresses for a virtual machine using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="5a25d-104">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5a25d-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="5a25d-105">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5a25d-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="5a25d-106">[Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="5a25d-106">[Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="5a25d-107">[Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="5a25d-107">[Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="5a25d-108">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="5a25d-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="5a25d-109">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5a25d-109">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> <span data-ttu-id="5a25d-110">Hello vzorové Azure CLI 2.0 příkazy níže očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="5a25d-110">hello sample Azure CLI 2.0 commands below expect a simple environment already created.</span></span> <span data-ttu-id="5a25d-111">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5a25d-111">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-cli.md).</span></span>

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="5a25d-112">Při vytváření virtuálního počítače zadat statickou privátní IP adresu</span><span class="sxs-lookup"><span data-stu-id="5a25d-112">Specify a static private IP address when creating a VM</span></span>

<span data-ttu-id="5a25d-113">virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5a25d-113">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="5a25d-114">Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5a25d-114">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="5a25d-115">Vytvoření veřejné IP adresy pro hello virtuálních počítačů s hello [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5a25d-115">Create a public IP for hello VM with hello [az network public-ip create](/cli/azure/network/public-ip#create) command.</span></span> <span data-ttu-id="5a25d-116">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="5a25d-116">hello list shown after hello output explains hello parameters used.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a25d-117">Můžete chtít nebo potřebovat toouse různé hodnoty pro vaše argumenty v tomto a dalších krocích v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a25d-117">You may want or need toouse different values for your arguments in this and subsequent steps, depending upon your environment.</span></span>
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    <span data-ttu-id="5a25d-118">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5a25d-118">Expected output:</span></span>
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * <span data-ttu-id="5a25d-119">`--resource-group`: Název skupiny prostředků hello toocreate veřejné IP hello.</span><span class="sxs-lookup"><span data-stu-id="5a25d-119">`--resource-group`: Name of hello resource group in which toocreate hello public IP.</span></span>
   * <span data-ttu-id="5a25d-120">`--name`: Název veřejné IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="5a25d-120">`--name`: Name of hello public IP.</span></span>
   * <span data-ttu-id="5a25d-121">`--location`: Oblast azure, ve které toocreate hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5a25d-121">`--location`: Azure region in which toocreate hello public IP.</span></span>

3. <span data-ttu-id="5a25d-122">Spustit hello [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create) příkaz toocreate síťový adaptér se statickou privátní IP.</span><span class="sxs-lookup"><span data-stu-id="5a25d-122">Run hello [az network nic create](/cli/azure/network/nic#create) command toocreate a NIC with a static private IP.</span></span> <span data-ttu-id="5a25d-123">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="5a25d-123">hello list shown after hello output explains hello parameters used.</span></span> 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="5a25d-124">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5a25d-124">Expected output:</span></span>
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    <span data-ttu-id="5a25d-125">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5a25d-125">Parameters:</span></span>

    * <span data-ttu-id="5a25d-126">`--private-ip-address`: Statickou privátní IP adresou pro hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5a25d-126">`--private-ip-address`: Static private IP address for hello NIC.</span></span>
    * <span data-ttu-id="5a25d-127">`--vnet-name`: Název hello sítě vnet, ve které toocreate hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5a25d-127">`--vnet-name`: Name of hello VNet in wihch toocreate hello NIC.</span></span>
    * <span data-ttu-id="5a25d-128">`--subnet`: Název hello podsítě, ve které toocreate hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5a25d-128">`--subnet`: Name of hello subnet in which toocreate hello NIC.</span></span>

4. <span data-ttu-id="5a25d-129">Spustit hello [vytvoření virtuálního počítače azure](/cli/azure/vm/nic#create) hello toocreate příkaz virtuálního počítače pomocí hello veřejná IP adresa a síťovou kartu vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="5a25d-129">Run hello [azure vm create](/cli/azure/vm/nic#create) command toocreate hello VM using hello public IP and NIC created above.</span></span> <span data-ttu-id="5a25d-130">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="5a25d-130">hello list shown after hello output explains hello parameters used.</span></span>
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    <span data-ttu-id="5a25d-131">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5a25d-131">Expected output:</span></span>
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   <span data-ttu-id="5a25d-132">Parametry než hello základní [vytvořit virtuální počítač az](/cli/azure/vm#create) parametry.</span><span class="sxs-lookup"><span data-stu-id="5a25d-132">Parameters other than hello basic [az vm create](/cli/azure/vm#create) parameters.</span></span>

   * <span data-ttu-id="5a25d-133">`--nics`: Název hello seskupování toowhich hello virtuální počítač je připojený.</span><span class="sxs-lookup"><span data-stu-id="5a25d-133">`--nics`: Name of hello NIC toowhich hello VM is attached.</span></span>
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="5a25d-134">Načíst statickou privátní IP adresu informace pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="5a25d-134">Retrieve static private IP address information for a VM</span></span>

<span data-ttu-id="5a25d-135">Spusťte následující příkaz rozhraní příkazového řádku Azure hello tooview hello statickou privátní IP adresu, která jste vytvořili, a sledovat hello hodnoty pro *alokační metody privátní IP* a *privátní IP adresa*:</span><span class="sxs-lookup"><span data-stu-id="5a25d-135">tooview hello static private IP address that you created, run hello following Azure CLI command and observe hello values for *Private IP alloc-method* and *Private IP address*:</span></span>

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

<span data-ttu-id="5a25d-136">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5a25d-136">Expected output:</span></span>

```json
"192.168.1.101"
```

<span data-ttu-id="5a25d-137">toodisplay hello konkrétně pro konkrétní IP informace hello síťovou kartu pro tento virtuální počítač, dotaz hello síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="5a25d-137">toodisplay hello specific IP information of hello NIC for that VM, query hello NIC specifically:</span></span>

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

<span data-ttu-id="5a25d-138">výstup Hello se něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="5a25d-138">hello output is something like:</span></span>

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="5a25d-139">Odeberte statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5a25d-139">Remove a static private IP address from a VM</span></span>

<span data-ttu-id="5a25d-140">Statickou privátní IP adresu nelze odebrat z síťový adaptér v Azure CLI pro nasazení resource manager.</span><span class="sxs-lookup"><span data-stu-id="5a25d-140">You cannot remove a static private IP address from a NIC in Azure CLI for resource manager deployments.</span></span> <span data-ttu-id="5a25d-141">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5a25d-141">You must:</span></span>
- <span data-ttu-id="5a25d-142">Vytvořte novou síťovou kartu, která používá dynamický IP</span><span class="sxs-lookup"><span data-stu-id="5a25d-142">Create a new NIC that uses a dynamic IP</span></span>
- <span data-ttu-id="5a25d-143">Nastavit hello síťový adaptér na hello hello virtuálních počítačů nově vytvořený síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5a25d-143">Set hello NIC on hello VM do hello newly created NIC.</span></span> 

<span data-ttu-id="5a25d-144">toochange hello síťovou kartu pro hello virtuálních počítačů použita v hello příkazů výše, postupujte podle kroků hello níže.</span><span class="sxs-lookup"><span data-stu-id="5a25d-144">toochange hello NIC for hello VM used in hello commands above, follow hello steps below.</span></span>

1. <span data-ttu-id="5a25d-145">Spustit hello **síťových adaptérů sítě azure vytvořit** příkaz toocreate nový síťový adaptér pomocí dynamické přidělování IP adres s novou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5a25d-145">Run hello **azure network nic create** command toocreate a new NIC using dynamic IP allocation with a new IP address.</span></span> <span data-ttu-id="5a25d-146">Všimněte si, protože není zadána žádná IP adresa, metoda přidělení hello **dynamické**.</span><span class="sxs-lookup"><span data-stu-id="5a25d-146">Note that because no IP address is specified, hello allocation method is **Dynamic**.</span></span>

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    <span data-ttu-id="5a25d-147">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5a25d-147">Expected output:</span></span>

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. <span data-ttu-id="5a25d-148">Spustit hello **sada virtuálních počítačů azure** příkaz toochange hello síťový adaptér používá hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a25d-148">Run hello **azure vm set** command toochange hello NIC used by hello VM.</span></span>
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    <span data-ttu-id="5a25d-149">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5a25d-149">Expected output:</span></span>
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > <span data-ttu-id="5a25d-150">Pokud hello virtuálního počítače je dostatečně velké na to toohave více než jeden síťový adaptér, spusťte hello **odstranit síťových adaptérů sítě azure** příkaz toodelete hello staré síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5a25d-150">If hello VM is large enough toohave more than one NIC, run hello **azure network nic delete** command toodelete hello old NIC.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="5a25d-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a25d-151">Next steps</span></span>
* <span data-ttu-id="5a25d-152">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="5a25d-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="5a25d-153">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="5a25d-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="5a25d-154">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="5a25d-154">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

