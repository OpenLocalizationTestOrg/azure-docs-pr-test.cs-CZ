---
title: "Víc konfigurací IP adres pomocí rozhraní příkazového řádku Azure pro vyrovnávání zatížení | Microsoft Docs"
description: "Zjistěte, jak přiřadit více IP adres virtuálního počítače pomocí rozhraní příkazového řádku Azure | Správce prostředků."
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
ms.openlocfilehash: bd15713752ea01ad403d8e3dcfed0c9a7adcc7fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="load-balancing-on-multiple-ip-configurations"></a><span data-ttu-id="2da02-103">Víc konfigurací IP adres pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2da02-103">Load balancing on multiple IP configurations</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2da02-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2da02-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="2da02-105">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2da02-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="2da02-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2da02-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="2da02-107">Tento článek popisuje, jak používat nástroj pro vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="2da02-107">This article describes how to use Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="2da02-108">V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="2da02-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="2da02-109">Každé sekundární síťové karty má dvě konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="2da02-109">Each of the secondary NICs have two IP configurations.</span></span> <span data-ttu-id="2da02-110">Každý virtuální počítač hostuje weby contoso.com a fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="2da02-110">Each VM hosts both websites contoso.com and fabrikam.com.</span></span> <span data-ttu-id="2da02-111">Každý web je vázána na jednu z konfigurace protokolu IP v sekundárním síťovém adaptéru.</span><span class="sxs-lookup"><span data-stu-id="2da02-111">Each website is bound to one of the IP configurations on the secondary NIC.</span></span> <span data-ttu-id="2da02-112">Nástroj pro vyrovnávání zatížení Azure používáme ke zveřejnění dvě front-end IP adresy, jednu pro každý web, distribuovat provoz do příslušných konfiguraci protokolu IP pro web.</span><span class="sxs-lookup"><span data-stu-id="2da02-112">We use Azure Load Balancer to expose two frontend IP addresses, one for each website, to distribute traffic to the respective IP configuration for the website.</span></span> <span data-ttu-id="2da02-113">Tento scénář používá stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.</span><span class="sxs-lookup"><span data-stu-id="2da02-113">This scenario uses the same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a><span data-ttu-id="2da02-115">Postup na víc konfigurací IP adres Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2da02-115">Steps to load balance on multiple IP configurations</span></span>

<span data-ttu-id="2da02-116">Použijte následující postup k dosažení scénáři uvedeném v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="2da02-116">Follow the steps below to achieve the scenario outlined in this article:</span></span>

1. <span data-ttu-id="2da02-117">[Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) rozhraní příkazového řádku Azure podle pokynů v článku propojené a přihlaste k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="2da02-117">[Install and Configure the Azure CLI](../cli-install-nodejs.md) the Azure CLI by following the steps in the linked article and log into your Azure account.</span></span>
2. <span data-ttu-id="2da02-118">[Vytvořte skupinu prostředků](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) názvem *contosofabrikam* jak bylo popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="2da02-118">[Create a resource group](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) called *contosofabrikam* as described above.</span></span>

    ```azurecli
    azure group create contosofabrikam westcentralus
    ```

3. <span data-ttu-id="2da02-119">[Vytvořit skupinu dostupnosti](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) k pro dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2da02-119">[Create an availability set](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) to for the two VMs.</span></span> <span data-ttu-id="2da02-120">V tomto scénáři použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2da02-120">For this scenario, use the following command:</span></span>

    ```azurecli
    azure availset create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. <span data-ttu-id="2da02-121">[Vytvoření virtuální sítě](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) názvem *myVNet* podsíť s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="2da02-121">[Create a virtual network](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) called *myVNet* and a subnet called *mySubnet*:</span></span>

    ```azurecli
    azure network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus

    azure network vnet subnet create --resource-group contosofabrikam --vnet-name myVnet --name mySubnet --address-prefix 10.0.0.0/24
    ```

5. <span data-ttu-id="2da02-122">[Vytvořit nástroj pro vyrovnávání zatížení](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) názvem *mylb*:</span><span class="sxs-lookup"><span data-stu-id="2da02-122">[Create the load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) called *mylb*:</span></span>

    ```azurecli
    azure network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. <span data-ttu-id="2da02-123">Vytvořte dvě dynamické veřejné IP adresy pro konfigurace IP front-endu nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="2da02-123">Create two dynamic public IP addresses for the frontend IP configurations of your load balancer:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. <span data-ttu-id="2da02-124">Vytvořte dvě konfigurace IP front-endu *contosofe* a *fabrikamfe* v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="2da02-124">Create the two frontend IP configurations, *contosofe* and *fabrikamfe* respectively:</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. <span data-ttu-id="2da02-125">Vytvoření váš back-end fondu adres - *contosopool* a *fabrikampool*, [testu](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, a pravidla - Vyrovnávání zatížení *HTTPc* a *HTTPf*:</span><span class="sxs-lookup"><span data-stu-id="2da02-125">Create your backend address pools - *contosopool* and *fabrikampool*, a [probe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, and your load balancing rules - *HTTPc* and *HTTPf*:</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    azure network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. <span data-ttu-id="2da02-126">Spusťte následující příkaz níže a potom zkontrolujte výstup do [ověřit nástroj pro vyrovnávání zatížení](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) byla vytvořena správně:</span><span class="sxs-lookup"><span data-stu-id="2da02-126">Run the following command below and then check the output to [verify your load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) was created correctly:</span></span>

    ```azurecli
    azure network lb show --resource-group contosofabrikam --name mylb
    ```

10. <span data-ttu-id="2da02-127">[Vytvoření veřejné IP adresy](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, a [účet úložiště](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* pro první virtuální počítač VM1, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="2da02-127">[Create a public IP](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, and [storage account](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* for your first virtual machine VM1 as shown below:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. <span data-ttu-id="2da02-128">[Vytvořit rozhraní sítě](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) pro VM1 a přidejte druhý konfiguraci IP adresy, *VM1 ipconfig2*, a [vytvořit virtuální počítač](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="2da02-128">[Create the network interfaces](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) for VM1 and add a second IP configuration, *VM1-ipconfig2*, and [create the VM](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) as shown below:</span></span>

    ```azurecli
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. <span data-ttu-id="2da02-129">Opakujte kroky 10 11 pro druhý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="2da02-129">Repeat steps 10-11 for your second VM:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. <span data-ttu-id="2da02-130">Nakonec je nutné nakonfigurovat záznamy prostředků DNS tak, aby odkazoval na příslušné front-endovou IP adresu služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2da02-130">Finally, you must configure DNS resource records to point to the respective frontend IP address of the Load Balancer.</span></span> <span data-ttu-id="2da02-131">Může hostovat vaše domény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="2da02-131">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="2da02-132">Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="2da02-132">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2da02-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2da02-133">Next steps</span></span>
- <span data-ttu-id="2da02-134">Další informace o tom, jak kombinací v Azure v rámci služby Vyrovnávání zatížení [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="2da02-134">Learn more about how to combine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="2da02-135">Zjistěte, jak můžete použít různé typy protokolů v Azure ke správě a odstraňování potíží pro vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="2da02-135">Learn how you can use different types of logs in Azure to manage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
