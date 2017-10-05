---
title: "Vytvoření plně kvalifikovaný název domény pro virtuální počítač s Windows v portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény, pro správce prostředků na základě virtuálního počítače na portálu Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2d5a555cd873222efcdb29e8eb3aaf128a24414b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a><span data-ttu-id="13238-103">V portálu Azure vytvořit platný plně kvalifikovaný název domény pro virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="13238-103">Create a fully qualified domain name in the Azure portal for a Windows VM</span></span>

<span data-ttu-id="13238-104">Když vytvoříte virtuální počítač (VM) v [portál Azure](https://portal.azure.com), se automaticky vytvoří prostředek veřejné IP pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="13238-104">When you create a virtual machine (VM) in the [Azure portal](https://portal.azure.com), a public IP resource for the virtual machine is automatically created.</span></span> <span data-ttu-id="13238-105">Tuto IP adresu použijete vzdálený přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="13238-105">You use this IP address to remotely access the VM.</span></span> <span data-ttu-id="13238-106">I když na portál nevytvoří [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, můžete vytvořit jeden po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="13238-106">Although the portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can create one once the VM is created.</span></span> <span data-ttu-id="13238-107">Tento článek ukazuje postup vytvoření název DNS nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="13238-107">This article demonstrates the steps to create a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="13238-108">Vytvoření plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="13238-108">Create a FQDN</span></span>
<span data-ttu-id="13238-109">Tento článek předpokládá, že jste již vytvořili virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="13238-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="13238-110">V případě potřeby můžete [vytvoření virtuálního počítače na portálu](quick-create-portal.md) nebo [s prostředím Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="13238-110">If needed, you can [create a VM in the portal](quick-create-portal.md) or [with Azure PowerShell](quick-create-powershell.md).</span></span> <span data-ttu-id="13238-111">Po nastavení a spuštění virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="13238-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="13238-112">Teď můžete vzdáleně připojit k virtuálnímu počítači pomocí tento název DNS, jako pro protokol RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="13238-112">You can now connect remotely to the VM using this DNS name such as for Remote Desktop Protocol (RDP).</span></span>

## <a name="next-steps"></a><span data-ttu-id="13238-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13238-113">Next steps</span></span>
<span data-ttu-id="13238-114">Nyní, když virtuální počítač veřejný název IP a DNS, můžete nasadit společné architektury aplikace nebo služby, například služby IIS, SQL nebo SharePoint.</span><span class="sxs-lookup"><span data-stu-id="13238-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as IIS, SQL, or SharePoint.</span></span>

<span data-ttu-id="13238-115">Také další informace o [pomocí Resource Manager](../../azure-resource-manager/resource-group-overview.md) tipy k sestavení Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="13238-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

