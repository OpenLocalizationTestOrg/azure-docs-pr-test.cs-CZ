---
title: "Vytvoření plně kvalifikovaný název domény pro virtuální počítač s Linuxem na portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény, pro správce prostředků na základě virtuálního počítače na portálu Azure."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49bfec791fcca3feabc4eb280cefd7faada0ea31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a><span data-ttu-id="cc08e-103">V portálu Azure vytvořit platný plně kvalifikovaný název domény pro virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="cc08e-103">Create a fully qualified domain name in the Azure portal for a Linux VM</span></span>

<span data-ttu-id="cc08e-104">Když vytvoříte virtuální počítač (VM) v [portál Azure](https://portal.azure.com), se automaticky vytvoří prostředek veřejné IP pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cc08e-104">When you create a virtual machine (VM) in the [Azure portal](https://portal.azure.com), a public IP resource for the virtual machine is automatically created.</span></span> <span data-ttu-id="cc08e-105">Tuto IP adresu použijete vzdálený přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="cc08e-105">You use this IP address to remotely access the VM.</span></span> <span data-ttu-id="cc08e-106">I když na portál nevytvoří [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, můžete přidat po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cc08e-106">Although the portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can add one once the VM is created.</span></span> <span data-ttu-id="cc08e-107">Tento článek ukazuje postup vytvoření název DNS nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="cc08e-107">This article demonstrates the steps to create a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="cc08e-108">Vytvoření plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="cc08e-108">Create a FQDN</span></span>
<span data-ttu-id="cc08e-109">Tento článek předpokládá, že jste již vytvořili virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cc08e-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="cc08e-110">V případě potřeby můžete [vytvoření virtuálního počítače na portálu](quick-create-portal.md) nebo [pomocí Azure CLI](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cc08e-110">If needed, you can [create a VM in the portal](quick-create-portal.md) or [with the Azure CLI](quick-create-cli.md).</span></span> <span data-ttu-id="cc08e-111">Po nastavení a spuštění virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="cc08e-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="cc08e-112">Nyní můžete přihlásit vzdáleně k virtuálnímu počítači pomocí tohoto názvu DNS, jako se `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="cc08e-112">You can now connect remotely to the VM using this DNS name such as with `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc08e-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc08e-113">Next steps</span></span>
<span data-ttu-id="cc08e-114">Nyní, když virtuální počítač veřejný název IP a DNS, můžete nasadit společné architektury aplikace nebo služby, jako je například nginx, MongoDB, Docker, atd.</span><span class="sxs-lookup"><span data-stu-id="cc08e-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as nginx, MongoDB, Docker, etc.</span></span>

<span data-ttu-id="cc08e-115">Také další informace o [pomocí Resource Manager](../../azure-resource-manager/resource-group-overview.md) tipy k sestavení Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="cc08e-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

