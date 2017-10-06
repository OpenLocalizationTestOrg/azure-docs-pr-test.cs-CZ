---
title: "aaaLoad vyrovnávání na víc konfigurací IP adres pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí rozhraní příkazového řádku Azure | Správce prostředků."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: annahar
ms.openlocfilehash: df81e1b8193f274bad435d6b506c7be824117416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations"></a><span data-ttu-id="8bddc-103">Víc konfigurací IP adres pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8bddc-103">Load balancing on multiple IP configurations</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8bddc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8bddc-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="8bddc-105">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8bddc-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="8bddc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bddc-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="8bddc-107">Tento článek popisuje, jak toouse Vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="8bddc-107">This article describes how toouse Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="8bddc-108">V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="8bddc-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="8bddc-109">Každý z hello sekundární síťové adaptéry mají dvě konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="8bddc-109">Each of hello secondary NICs have two IP configurations.</span></span> <span data-ttu-id="8bddc-110">Každý virtuální počítač hostuje weby contoso.com a fabrikam.com. Každý web je vázané tooone hello konfigurace protokolu IP v síťovém adaptéru hello sekundární.</span><span class="sxs-lookup"><span data-stu-id="8bddc-110">Each VM hosts both websites contoso.com and fabrikam.com. Each website is bound tooone of hello IP configurations on hello secondary NIC.</span></span> <span data-ttu-id="8bddc-111">Můžeme použít nástroj pro vyrovnávání zatížení Azure tooexpose dvě front-end IP adresy, jednu pro každý web, toodistribute provoz toohello odpovídající konfiguraci protokolu IP pro web hello.</span><span class="sxs-lookup"><span data-stu-id="8bddc-111">We use Azure Load Balancer tooexpose two frontend IP addresses, one for each website, toodistribute traffic toohello respective IP configuration for hello website.</span></span> <span data-ttu-id="8bddc-112">Tento scénář používá hello stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.</span><span class="sxs-lookup"><span data-stu-id="8bddc-112">This scenario uses hello same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a><span data-ttu-id="8bddc-114">Vyrovnávat tooload kroky na víc konfigurací IP adres</span><span class="sxs-lookup"><span data-stu-id="8bddc-114">Steps tooload balance on multiple IP configurations</span></span>

<span data-ttu-id="8bddc-115">Postupujte podle kroků hello tooachieve hello scénář popsaný v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="8bddc-115">Follow hello steps below tooachieve hello scenario outlined in this article:</span></span>

1. <span data-ttu-id="8bddc-116">[Instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) hello rozhraní příkazového řádku Azure postupem hello v hello propojené článku a v protokolu ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="8bddc-116">[Install and Configure hello Azure CLI](../cli-install-nodejs.md) hello Azure CLI by following hello steps in hello linked article and log into your Azure account.</span></span>
2. <span data-ttu-id="8bddc-117">[Vytvořte skupinu prostředků](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) názvem *contosofabrikam* jak bylo popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="8bddc-117">[Create a resource group](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) called *contosofabrikam* as described above.</span></span>

    ```azurecli
    azure group create contosofabrikam westcentralus
    ```

3. <span data-ttu-id="8bddc-118">[Vytvořit skupinu dostupnosti](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) toofor hello dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8bddc-118">[Create an availability set](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) toofor hello two VMs.</span></span> <span data-ttu-id="8bddc-119">V tomto scénáři použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8bddc-119">For this scenario, use hello following command:</span></span>

    ```azurecli
    azure availset create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. <span data-ttu-id="8bddc-120">[Vytvoření virtuální sítě](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) názvem *myVNet* podsíť s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="8bddc-120">[Create a virtual network](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) called *myVNet* and a subnet called *mySubnet*:</span></span>

    ```azurecli
    azure network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus

    azure network vnet subnet create --resource-group contosofabrikam --vnet-name myVnet --name mySubnet --address-prefix 10.0.0.0/24
    ```

5. <span data-ttu-id="8bddc-121">[Vytvořit nástroj pro vyrovnávání zatížení hello](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) názvem *mylb*:</span><span class="sxs-lookup"><span data-stu-id="8bddc-121">[Create hello load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) called *mylb*:</span></span>

    ```azurecli
    azure network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. <span data-ttu-id="8bddc-122">Vytvořte dvě dynamické veřejné IP adresy pro konfigurace IP front-endu hello nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="8bddc-122">Create two dynamic public IP addresses for hello frontend IP configurations of your load balancer:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. <span data-ttu-id="8bddc-123">Vytvoření hello dva front-endové konfigurace protokolu IP, *contosofe* a *fabrikamfe* v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="8bddc-123">Create hello two frontend IP configurations, *contosofe* and *fabrikamfe* respectively:</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. <span data-ttu-id="8bddc-124">Vytvoření váš back-end fondu adres - *contosopool* a *fabrikampool*, [testu](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, a pravidla - Vyrovnávání zatížení *HTTPc* a *HTTPf*:</span><span class="sxs-lookup"><span data-stu-id="8bddc-124">Create your backend address pools - *contosopool* and *fabrikampool*, a [probe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, and your load balancing rules - *HTTPc* and *HTTPf*:</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    azure network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. <span data-ttu-id="8bddc-125">Spuštění hello následující příkaz níže a potom zkontrolujte výstup hello příliš[ověřit nástroj pro vyrovnávání zatížení](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) byla vytvořena správně:</span><span class="sxs-lookup"><span data-stu-id="8bddc-125">Run hello following command below and then check hello output too[verify your load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) was created correctly:</span></span>

    ```azurecli
    azure network lb show --resource-group contosofabrikam --name mylb
    ```

10. <span data-ttu-id="8bddc-126">[Vytvoření veřejné IP adresy](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, a [účet úložiště](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* pro první virtuální počítač VM1, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="8bddc-126">[Create a public IP](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, and [storage account](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* for your first virtual machine VM1 as shown below:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. <span data-ttu-id="8bddc-127">[Vytvořit hello síťových rozhraní](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) pro VM1 a přidejte druhý konfiguraci IP adresy, *VM1 ipconfig2*, a [vytvořit hello virtuálních počítačů](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="8bddc-127">[Create hello network interfaces](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) for VM1 and add a second IP configuration, *VM1-ipconfig2*, and [create hello VM](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) as shown below:</span></span>

    ```azurecli
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. <span data-ttu-id="8bddc-128">Opakujte kroky 10 11 pro druhý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="8bddc-128">Repeat steps 10-11 for your second VM:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. <span data-ttu-id="8bddc-129">Nakonec musíte nakonfigurovat DNS prostředků záznamů toopoint toohello příslušných front-endovou IP adresu hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8bddc-129">Finally, you must configure DNS resource records toopoint toohello respective frontend IP address of hello Load Balancer.</span></span> <span data-ttu-id="8bddc-130">Může hostovat vaše domény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="8bddc-130">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="8bddc-131">Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="8bddc-131">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bddc-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bddc-132">Next steps</span></span>
- <span data-ttu-id="8bddc-133">Další informace o tom, jak Vyrovnávání zatížení toocombine služby v Azure v [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="8bddc-133">Learn more about how toocombine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="8bddc-134">Zjistěte, jak můžete použít různé typy protokolů v Azure toomanage a řešení vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="8bddc-134">Learn how you can use different types of logs in Azure toomanage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
