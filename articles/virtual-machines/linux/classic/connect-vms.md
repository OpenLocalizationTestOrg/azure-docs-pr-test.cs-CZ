---
title: "aaaConnect virtuální počítače s Linuxem v cloudové službě | Microsoft Docs"
description: "Připojte virtuální počítače s Linuxem, které jsou vytvořené pomocí tooan modelu nasazení classic hello cloudové služby Azure nebo virtuální sítě."
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
ms.openlocfilehash: 323baf04390d53ffb2810e24a24d175c08e60cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-linux-virtual-machines-created-with-hello-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a><span data-ttu-id="248dc-103">Připojit virtuální počítače Linux vytvořené pomocí modelu nasazení classic hello pomocí virtuální sítě nebo cloudové služby</span><span class="sxs-lookup"><span data-stu-id="248dc-103">Connect Linux virtual machines created with hello classic deployment model with a virtual network or cloud service</span></span>
> [!IMPORTANT]
> <span data-ttu-id="248dc-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="248dc-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="248dc-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="248dc-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="248dc-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="248dc-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="248dc-107">Virtuální počítače Linux vytvořené pomocí modelu nasazení classic hello jsou vždy umístěny v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="248dc-107">Linux virtual machines created with hello classic deployment model are always placed in a cloud service.</span></span> <span data-ttu-id="248dc-108">Cloudová služba Hello slouží jako kontejner a poskytuje jedinečný veřejný název DNS, veřejnou IP adresu a sada koncových bodů tooaccess hello virtuálního počítače přes hello Internet.</span><span class="sxs-lookup"><span data-stu-id="248dc-108">hello cloud service acts as a container and provides a unique public DNS name, a public IP address, and a set of endpoints tooaccess hello virtual machine over hello Internet.</span></span> <span data-ttu-id="248dc-109">Hello cloudové služby může být ve virtuální síti, ale není povinné.</span><span class="sxs-lookup"><span data-stu-id="248dc-109">hello cloud service can be in a virtual network, but that's not a requirement.</span></span> <span data-ttu-id="248dc-110">Můžete také [připojit virtuální počítače s Windows pomocí virtuální sítě nebo cloudové služby](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="248dc-110">You can also [connect Windows virtual machines with a virtual network or cloud service](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="248dc-111">Pokud cloudové služby není ve virtuální síti, se nazývá *samostatné* Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="248dc-111">If a cloud service isn't in a virtual network, it's called a *standalone* cloud service.</span></span> <span data-ttu-id="248dc-112">Hello virtuální počítače v cloudové službě samostatné komunikovat s jinými virtuálními počítači pomocí hello veřejné názvy DNS jiné virtuální počítače a provoz hello přenášena přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="248dc-112">hello virtual machines in a standalone cloud service communicate with other virtual machines by using hello other virtual machines’ public DNS names, and hello traffic travels over hello Internet.</span></span> <span data-ttu-id="248dc-113">Pokud cloudové služby ve virtuální síti, hello virtuální počítače v tomto cloudové služby může komunikovat s všechny ostatní virtuální počítače ve virtuální síti hello bez odeslání veškerou komunikaci přes hello Internet.</span><span class="sxs-lookup"><span data-stu-id="248dc-113">If a cloud service is in a virtual network, hello virtual machines in that cloud service can communicate with all other virtual machines in hello virtual network without sending any traffic over hello Internet.</span></span>

<span data-ttu-id="248dc-114">Zadáte-li virtuální počítače v hello stejné samostatnou cloudovou službu, můžete nadále používat, Vyrovnávání zatížení a sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="248dc-114">If you place your virtual machines in hello same standalone cloud service, you can still use load balancing and availability sets.</span></span> <span data-ttu-id="248dc-115">Podrobnosti najdete v tématu [virtuální počítače Vyrovnávání zatížení](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [spravovat hello dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="248dc-115">For details, see [Load balancing virtual machines](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Manage hello availability of virtual machines](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="248dc-116">Nelze však uspořádání hello virtuální počítače v podsítích nebo připojit samostatné cloudové služby tooyour místní síť.</span><span class="sxs-lookup"><span data-stu-id="248dc-116">However, you can't organize hello virtual machines on subnets or connect a standalone cloud service tooyour on-premises network.</span></span> <span data-ttu-id="248dc-117">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="248dc-117">Here's an example:</span></span>

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a><span data-ttu-id="248dc-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="248dc-118">Next steps</span></span>
<span data-ttu-id="248dc-119">Po vytvoření virtuálního počítače, je vhodné příliš[přidat datový disk](attach-disk.md) aby dat toostore umístění služby a úlohy.</span><span class="sxs-lookup"><span data-stu-id="248dc-119">After you create a virtual machine, it's a good idea too[add a data disk](attach-disk.md) so your services and workloads have a location toostore data.</span></span>
