---
title: "aaaSet až koncových bodů na klasické virtuální počítač s Linuxem | Microsoft Docs"
description: "Přečtěte si informace tooset až koncové body pro virtuální počítač s Linuxem v hello Azure classic portálu tooallow komunikaci s virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 1c959d10dd1e20228fa4a20e1cc0205c1d12f185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a><span data-ttu-id="e3b7a-103">Jak tooset až koncových bodů na klasické virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="e3b7a-103">How tooset up endpoints on a Linux classic virtual machine in Azure</span></span>
<span data-ttu-id="e3b7a-104">Všechny virtuální počítače Linux, které vytvoříte v Azure pomocí modelu nasazení classic hello můžete automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači v hello že stejné cloudové služby nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-104">All Linux virtual machines that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="e3b7a-105">Počítače na hello internetové nebo jiné virtuální sítě však vyžadují koncové body toodirect hello příchozích síťových přenosů tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-105">However, computers on hello Internet or other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="e3b7a-106">Tento článek je také k dispozici pro [virtuální počítače s Windows](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e3b7a-106">This article is also available for [Windows virtual machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3b7a-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e3b7a-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e3b7a-108">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="e3b7a-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="e3b7a-110">V hello **Resource Manager** modelu nasazení koncové body jsou konfigurováni pomocí **skupiny zabezpečení sítě (Nsg)**.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-110">In hello **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="e3b7a-111">Další informace najdete v tématu [otevírání portů a koncové body](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e3b7a-111">For more information, see [Opening ports and endpoints](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="e3b7a-112">Když vytvoříte virtuální počítač s Linuxem v hello portálu Azure, koncový bod pro Secure Shell (SSH) je obvykle vám vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-112">When you create a Linux virtual machine in hello Azure portal, an endpoint for Secure Shell (SSH) is typically created for you automatically.</span></span> <span data-ttu-id="e3b7a-113">Při vytváření hello virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-113">You can configure additional endpoints while creating hello virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="e3b7a-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3b7a-114">Next steps</span></span>
* <span data-ttu-id="e3b7a-115">Můžete také vytvořit koncový bod virtuálního počítače pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e3b7a-115">You can also create a VM endpoint by using hello [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="e3b7a-116">Spustit hello **vytvořit koncový bod virtuálního počítače azure** příkaz.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-116">Run hello **azure vm endpoint create** command.</span></span>
* <span data-ttu-id="e3b7a-117">Pokud jste vytvořili virtuální počítač v modelu nasazení Resource Manager hello, můžete použít hello rozhraní příkazového řádku Azure v režimu Resource Manager příliš[vytvoření skupin zabezpečení sítě](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toohello toocontrol přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e3b7a-117">If you created a virtual machine in hello Resource Manager deployment model, you can use hello Azure CLI in Resource Manager mode too[create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol traffic toohello VM.</span></span>
