---
title: "aaaSet až koncových bodů na klasické virtuální počítač s Windows | Microsoft Docs"
description: "Přečtěte si informace tooset až koncové body pro virtuální počítač Windows v hello Azure classic portálu tooallow komunikaci s virtuálního počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: e817076f16d3a245a8d19add7b2f2cf5e3baa17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a><span data-ttu-id="1e26f-103">Jak tooset až koncových bodů na klasické virtuální počítač Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="1e26f-103">How tooset up endpoints on a classic Windows virtual machine in Azure</span></span>
<span data-ttu-id="1e26f-104">Všechny systémy Windows, které virtuální počítače vytvořené v Azure pomocí modelu nasazení classic hello automaticky komunikovat přes kanál s jinými virtuálními počítači v privátní síti hello stejné cloudové služby nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1e26f-104">All Windows virtual machines that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="1e26f-105">Počítače na hello internetové nebo jiné virtuální sítě však vyžadují koncové body toodirect hello příchozích síťových přenosů tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e26f-105">However, computers on hello Internet or other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="1e26f-106">Tento článek je také k dispozici pro [virtuální počítače s Linuxem](../../linux/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="1e26f-106">This article is also available for [Linux virtual machines](../../linux/classic/setup-endpoints.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e26f-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1e26f-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1e26f-108">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="1e26f-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="1e26f-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="1e26f-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="1e26f-110">V hello **Resource Manager** modelu nasazení koncové body jsou konfigurováni pomocí **skupiny zabezpečení sítě (Nsg)**.</span><span class="sxs-lookup"><span data-stu-id="1e26f-110">In hello **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="1e26f-111">Další informace najdete v tématu [povolit externí přístup tooyour virtuální počítač pomocí portálu Azure hello](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e26f-111">For more information, see [Allow external access tooyour VM using hello Azure portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="1e26f-112">Při vytváření virtuálního počítače s Windows v hello portálu Azure, běžné koncové body, jako jsou ty, které pro vzdálenou plochu a vzdálenou komunikaci Windows PowerShell jsou obvykle vám vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="1e26f-112">When you create a Windows virtual machine in hello Azure portal, common endpoints like those for Remote Desktop and Windows PowerShell Remoting are typically created for you automatically.</span></span> <span data-ttu-id="1e26f-113">Při vytváření hello virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.</span><span class="sxs-lookup"><span data-stu-id="1e26f-113">You can configure additional endpoints while creating hello virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="1e26f-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e26f-114">Next steps</span></span>
* <span data-ttu-id="1e26f-115">toouse tooset rutiny prostředí Azure PowerShell až koncový bod virtuálního počítače najdete v části [přidat AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e26f-115">toouse an Azure PowerShell cmdlet tooset up a VM endpoint, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).</span></span>
* <span data-ttu-id="1e26f-116">toouse toomanage rutiny prostředí Azure PowerShell ACL na koncový bod, najdete v části [seznamy Správa řízení přístupu (ACL) pro koncové body pomocí prostředí PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1e26f-116">toouse an Azure PowerShell cmdlet toomanage an ACL on an endpoint, see [Managing access control lists (ACLs) for endpoints by using PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).</span></span>
* <span data-ttu-id="1e26f-117">Pokud jste vytvořili virtuální počítač v modelu nasazení Resource Manager hello, můžete použít Azure PowerShell příliš[vytvoření skupin zabezpečení sítě](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toohello toocontrol přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e26f-117">If you created a virtual machine in hello Resource Manager deployment model, you can use Azure PowerShell too[create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol traffic toohello VM.</span></span>
