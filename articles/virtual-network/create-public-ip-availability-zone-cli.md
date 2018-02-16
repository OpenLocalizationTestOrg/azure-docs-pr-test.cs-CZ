---
title: "Vytvoření zoned veřejnou IP adresu pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvořte veřejnou IP adresu v zóně dostupnosti pomocí Azure CLI."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ef93d43bbd0c58950027810c8c335d9076574326
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-cli"></a><span data-ttu-id="202c5-103">Vytvoření veřejné IP adresy v zóně dostupnosti pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="202c5-103">Create a public IP address in an availability zone with the Azure CLI</span></span>

<span data-ttu-id="202c5-104">Můžete nasadit na veřejnou IP adresu v zóně Azure dostupnosti (preview).</span><span class="sxs-lookup"><span data-stu-id="202c5-104">You can deploy a public IP address in an Azure availability zone (preview).</span></span> <span data-ttu-id="202c5-105">Dostupnosti zóna je fyzicky oddělená zónu v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="202c5-105">An availability zone is a physically separate zone in an Azure region.</span></span> <span data-ttu-id="202c5-106">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="202c5-106">Learn how to:</span></span>

> * <span data-ttu-id="202c5-107">Vytvoření veřejné IP adresy v zóně dostupnosti</span><span class="sxs-lookup"><span data-stu-id="202c5-107">Create a public IP address in an availability zone</span></span>
> * <span data-ttu-id="202c5-108">Určete související prostředky, které jsou vytvořeny v zóně dostupnosti</span><span class="sxs-lookup"><span data-stu-id="202c5-108">Identify related resources created in the availability zone</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

<span data-ttu-id="202c5-109">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure novější než verze 2.0.17.</span><span class="sxs-lookup"><span data-stu-id="202c5-109">If you choose to install and use the CLI locally, this tutorial requires that you are running a version of the Azure CLI greater than version 2.0.17.</span></span> <span data-ttu-id="202c5-110">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="202c5-110">To find the version, run `az --version`.</span></span> <span data-ttu-id="202c5-111">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="202c5-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

> [!NOTE]
> <span data-ttu-id="202c5-112">Dostupnost zóny jsou ve verzi preview a jsou připraveny pro váš vývojový a testovací scénáře.</span><span class="sxs-lookup"><span data-stu-id="202c5-112">Availability zones are in preview and are ready for your development and test scenarios.</span></span> <span data-ttu-id="202c5-113">Podpora je k dispozici pro vyberte prostředků Azure a oblasti a rodiny velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="202c5-113">Support is available for select Azure resources and regions, and VM size families.</span></span> <span data-ttu-id="202c5-114">Další informace o tom, jak začít pracovat a které prostředky Azure, oblasti a rodiny velikost virtuálního počítače můžete zkusit dostupnost zóny s, najdete v části [přehled dostupnosti zón](https://docs.microsoft.com/azure/availability-zones/az-overview).</span><span class="sxs-lookup"><span data-stu-id="202c5-114">For more information on how to get started, and which Azure resources, regions, and VM size families you can try availability zones with, see [Overview of Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview).</span></span> <span data-ttu-id="202c5-115">Pokud budete potřebovat podporu, můžete kontaktovat [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) nebo [otevřít lístek podpory Azure](../azure-supportability/how-to-create-azure-support-request.md).</span><span class="sxs-lookup"><span data-stu-id="202c5-115">For support, you can reach out on [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) or [open an Azure support ticket](../azure-supportability/how-to-create-azure-support-request.md).</span></span>

## <a name="create-a-zonal-public-ip-address"></a><span data-ttu-id="202c5-116">Vytvoření oblastmi veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="202c5-116">Create a zonal public IP address</span></span>

<span data-ttu-id="202c5-117">Vytvoření veřejné IP adresy v zóně dostupnosti pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="202c5-117">Create a public IP address in an availability zone using the following command:</span></span>


```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIp --zone 2 --location westeurope
```
> [!NOTE]
> <span data-ttu-id="202c5-118">Při přiřazování veřejné IP adresy standardní SKU k síťovému rozhraní virtuálního počítače je potřeba explicitně povolit plánovaný provoz pomocí [skupiny zabezpečení sítě](security-overview.md#network-security-groups).</span><span class="sxs-lookup"><span data-stu-id="202c5-118">When you assign a standard SKU public IP address to a virtual machine’s network interface, you must explicitly allow the intended traffic with a [network security group](security-overview.md#network-security-groups).</span></span> <span data-ttu-id="202c5-119">Komunikace s prostředkem nebude možná, dokud nevytvoříte a nepřiřadíte skupinu zabezpečení sítě a explicitně nepovolíte požadovaný provoz.</span><span class="sxs-lookup"><span data-stu-id="202c5-119">Communication with the resource fails until you create and associate a network security group and explicitly allow the desired traffic.</span></span>

## <a name="get-zone-information-about-a-public-ip-address"></a><span data-ttu-id="202c5-120">Získat informace o zóně o veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="202c5-120">Get zone information about a public IP address</span></span>

<span data-ttu-id="202c5-121">Získáte informace o zóně veřejné IP adresy pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="202c5-121">Get the zone information of a public IP address using the following command:</span></span>

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIp
```

## <a name="next-steps"></a><span data-ttu-id="202c5-122">Další postup</span><span class="sxs-lookup"><span data-stu-id="202c5-122">Next steps</span></span>

- <span data-ttu-id="202c5-123">Další informace o [dostupnost zóny](https://docs.microsoft.com/azure/availability-zones/az-overview)</span><span class="sxs-lookup"><span data-stu-id="202c5-123">Learn more about [availability zones](https://docs.microsoft.com/azure/availability-zones/az-overview)</span></span>
- <span data-ttu-id="202c5-124">Další informace o [veřejné IP adresy](../virtual-network/virtual-network-public-ip-address.md)</span><span class="sxs-lookup"><span data-stu-id="202c5-124">Learn more about [public IP addresses](../virtual-network/virtual-network-public-ip-address.md)</span></span> 
