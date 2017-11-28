---
title: "Připojit virtuální počítače s Linuxem v cloudové službě | Microsoft Docs"
description: "Připojte virtuální počítače Linux vytvořené pomocí modelu nasazení classic do cloudové služby Azure nebo virtuální sítě."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: e222645509640b104410f87e4bcd22834c8d9ec1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a><span data-ttu-id="523f9-103">Připojit virtuální počítače Linux vytvořené pomocí modelu nasazení classic pomocí virtuální sítě nebo cloudové služby</span><span class="sxs-lookup"><span data-stu-id="523f9-103">Connect Linux virtual machines created with the classic deployment model with a virtual network or cloud service</span></span>
> [!IMPORTANT]
> <span data-ttu-id="523f9-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="523f9-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="523f9-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="523f9-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="523f9-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="523f9-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="523f9-107">Virtuální počítače Linux vytvořené pomocí modelu nasazení classic jsou vždy umístěny v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="523f9-107">Linux virtual machines created with the classic deployment model are always placed in a cloud service.</span></span> <span data-ttu-id="523f9-108">Cloudové služby slouží jako kontejner a poskytuje jedinečný veřejný název DNS, veřejnou IP adresu a sada koncových bodů pro přístup k virtuálnímu počítači přes Internet.</span><span class="sxs-lookup"><span data-stu-id="523f9-108">The cloud service acts as a container and provides a unique public DNS name, a public IP address, and a set of endpoints to access the virtual machine over the Internet.</span></span> <span data-ttu-id="523f9-109">Cloudové služby může být ve virtuální síti, ale není povinné.</span><span class="sxs-lookup"><span data-stu-id="523f9-109">The cloud service can be in a virtual network, but that's not a requirement.</span></span> <span data-ttu-id="523f9-110">Můžete také [připojit virtuální počítače s Windows pomocí virtuální sítě nebo cloudové služby](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="523f9-110">You can also [connect Windows virtual machines with a virtual network or cloud service](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="523f9-111">Pokud cloudové služby není ve virtuální síti, se nazývá *samostatné* Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="523f9-111">If a cloud service isn't in a virtual network, it's called a *standalone* cloud service.</span></span> <span data-ttu-id="523f9-112">Virtuální počítače v cloudové službě samostatné komunikovat s jinými virtuálními počítači pomocí veřejné názvy DNS jinými virtuálními počítači a provoz se přenáší přes Internet.</span><span class="sxs-lookup"><span data-stu-id="523f9-112">The virtual machines in a standalone cloud service communicate with other virtual machines by using the other virtual machines’ public DNS names, and the traffic travels over the Internet.</span></span> <span data-ttu-id="523f9-113">Pokud cloudové služby ve virtuální síti, virtuální počítače v dané cloudové služby může komunikovat s všechny ostatní virtuální počítače ve virtuální síti bez odeslání veškerou komunikaci přes Internet.</span><span class="sxs-lookup"><span data-stu-id="523f9-113">If a cloud service is in a virtual network, the virtual machines in that cloud service can communicate with all other virtual machines in the virtual network without sending any traffic over the Internet.</span></span>

<span data-ttu-id="523f9-114">Pokud jste v rámci stejné cloudové služby samostatné virtuální počítače, můžete nadále používat vyrovnávání zatížení a skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="523f9-114">If you place your virtual machines in the same standalone cloud service, you can still use load balancing and availability sets.</span></span> <span data-ttu-id="523f9-115">Podrobnosti najdete v tématu [virtuální počítače Vyrovnávání zatížení](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [Správa dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="523f9-115">For details, see [Load balancing virtual machines](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Manage the availability of virtual machines](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="523f9-116">Nelze však uspořádat virtuální počítače v podsítích nebo připojení k síti na pracovišti samostatné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="523f9-116">However, you can't organize the virtual machines on subnets or connect a standalone cloud service to your on-premises network.</span></span> <span data-ttu-id="523f9-117">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="523f9-117">Here's an example:</span></span>

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a><span data-ttu-id="523f9-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="523f9-118">Next steps</span></span>
<span data-ttu-id="523f9-119">Po vytvoření virtuálního počítače, je vhodné [přidat datový disk](attach-disk.md) aby umístění pro uložení dat služby a úlohy.</span><span class="sxs-lookup"><span data-stu-id="523f9-119">After you create a virtual machine, it's a good idea to [add a data disk](attach-disk.md) so your services and workloads have a location to store data.</span></span>
