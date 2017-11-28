---
title: "aaaCreate plně kvalifikovaný název domény pro virtuální počítač s Windows v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény pro správce prostředků na základě virtuálního počítače v hello portálu Azure."
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
ms.openlocfilehash: 67c817ec97073803e513bc41ebde67b75ced565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-windows-vm"></a><span data-ttu-id="d238f-103">Vytvoření platný plně kvalifikovaný název domény v hello portál Azure pro virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="d238f-103">Create a fully qualified domain name in hello Azure portal for a Windows VM</span></span>

<span data-ttu-id="d238f-104">Když vytvoříte virtuální počítač (VM) v hello [portál Azure](https://portal.azure.com), se automaticky vytvoří prostředek veřejné IP pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="d238f-104">When you create a virtual machine (VM) in hello [Azure portal](https://portal.azure.com), a public IP resource for hello virtual machine is automatically created.</span></span> <span data-ttu-id="d238f-105">Můžete použít tuto IP adresu tooremotely přístup hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d238f-105">You use this IP address tooremotely access hello VM.</span></span> <span data-ttu-id="d238f-106">Přestože portál hello nevytvoří [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, můžete vytvořit jeden po hello virtuální počítač je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="d238f-106">Although hello portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can create one once hello VM is created.</span></span> <span data-ttu-id="d238f-107">Tento článek ukazuje hello kroky toocreate název DNS nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="d238f-107">This article demonstrates hello steps toocreate a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="d238f-108">Vytvoření plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="d238f-108">Create a FQDN</span></span>
<span data-ttu-id="d238f-109">Tento článek předpokládá, že jste již vytvořili virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d238f-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="d238f-110">V případě potřeby můžete [vytvoření virtuálního počítače na portálu hello](quick-create-portal.md) nebo [s prostředím Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="d238f-110">If needed, you can [create a VM in hello portal](quick-create-portal.md) or [with Azure PowerShell](quick-create-powershell.md).</span></span> <span data-ttu-id="d238f-111">Po nastavení a spuštění virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d238f-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="d238f-112">Nyní můžete přihlásit vzdáleně toohello virtuálního počítače pomocí tento název DNS, jako pro protokol RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="d238f-112">You can now connect remotely toohello VM using this DNS name such as for Remote Desktop Protocol (RDP).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d238f-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d238f-113">Next steps</span></span>
<span data-ttu-id="d238f-114">Nyní, když virtuální počítač veřejný název IP a DNS, můžete nasadit společné architektury aplikace nebo služby, například služby IIS, SQL nebo SharePoint.</span><span class="sxs-lookup"><span data-stu-id="d238f-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as IIS, SQL, or SharePoint.</span></span>

<span data-ttu-id="d238f-115">Také další informace o [pomocí Resource Manager](../../azure-resource-manager/resource-group-overview.md) tipy k sestavení Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="d238f-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

