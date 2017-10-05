---
title: "Nastavení koncových bodů na klasické virtuální počítač s Linuxem | Microsoft Docs"
description: "Naučte se nastavit koncové body pro virtuální počítač s Linuxem v portálu Azure classic a povolit komunikaci se virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: 4fd8b847b0f60648d1661ce5a8667c641e616ed4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a><span data-ttu-id="bec66-103">Jak nastavit koncové body na klasické virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="bec66-103">How to set up endpoints on a Linux classic virtual machine in Azure</span></span>
<span data-ttu-id="bec66-104">Všechny virtuální počítače Linux, které vytvoříte v Azure pomocí modelu nasazení classic může automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači ve stejné cloudové služby nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bec66-104">All Linux virtual machines that you create in Azure using the classic deployment model can automatically communicate over a private network channel with other virtual machines in the same cloud service or virtual network.</span></span> <span data-ttu-id="bec66-105">Počítače v Internetu nebo k jiným virtuálním sítím však vyžadují koncové body směrovat příchozí síťový provoz do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bec66-105">However, computers on the Internet or other virtual networks require endpoints to direct the inbound network traffic to a virtual machine.</span></span> <span data-ttu-id="bec66-106">Tento článek je také k dispozici pro [virtuální počítače s Windows](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bec66-106">This article is also available for [Windows virtual machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bec66-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bec66-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bec66-108">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="bec66-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="bec66-109">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bec66-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="bec66-110">V **Resource Manager** modelu nasazení koncové body jsou konfigurováni pomocí **skupiny zabezpečení sítě (Nsg)**.</span><span class="sxs-lookup"><span data-stu-id="bec66-110">In the **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="bec66-111">Další informace najdete v tématu [otevírání portů a koncové body](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bec66-111">For more information, see [Opening ports and endpoints](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="bec66-112">Když vytvoříte virtuální počítač s Linuxem na portálu Azure, koncový bod pro Secure Shell (SSH) je obvykle vám vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="bec66-112">When you create a Linux virtual machine in the Azure portal, an endpoint for Secure Shell (SSH) is typically created for you automatically.</span></span> <span data-ttu-id="bec66-113">Při vytváření virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.</span><span class="sxs-lookup"><span data-stu-id="bec66-113">You can configure additional endpoints while creating the virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="bec66-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bec66-114">Next steps</span></span>
* <span data-ttu-id="bec66-115">Můžete také vytvořit koncový bod virtuálního počítače pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="bec66-115">You can also create a VM endpoint by using the [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="bec66-116">Spustit **vytvořit koncový bod virtuálního počítače azure** příkaz.</span><span class="sxs-lookup"><span data-stu-id="bec66-116">Run the **azure vm endpoint create** command.</span></span>
* <span data-ttu-id="bec66-117">Pokud jste vytvořili virtuální počítač v modelu nasazení Resource Manager, můžete použít rozhraní příkazového řádku Azure v režimu Resource Manager k [vytvoření skupin zabezpečení sítě](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) pro řízení přenosu do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bec66-117">If you created a virtual machine in the Resource Manager deployment model, you can use the Azure CLI in Resource Manager mode to [create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) to control traffic to the VM.</span></span>
