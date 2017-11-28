---
title: "aaaCreate plně kvalifikovaný název domény pro virtuální počítač s Linuxem v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény pro správce prostředků na základě virtuálního počítače v hello portálu Azure."
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
ms.openlocfilehash: 1494a0cb1caa62069c72096a739aee111ac8b383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-linux-vm"></a><span data-ttu-id="aaee8-103">Vytvoření platný plně kvalifikovaný název domény v hello portál Azure pro virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="aaee8-103">Create a fully qualified domain name in hello Azure portal for a Linux VM</span></span>

<span data-ttu-id="aaee8-104">Když vytvoříte virtuální počítač (VM) v hello [portál Azure](https://portal.azure.com), se automaticky vytvoří prostředek veřejné IP pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="aaee8-104">When you create a virtual machine (VM) in hello [Azure portal](https://portal.azure.com), a public IP resource for hello virtual machine is automatically created.</span></span> <span data-ttu-id="aaee8-105">Můžete použít tuto IP adresu tooremotely přístup hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aaee8-105">You use this IP address tooremotely access hello VM.</span></span> <span data-ttu-id="aaee8-106">Přestože portál hello nevytvoří [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, můžete přidat až hello virtuální počítač je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="aaee8-106">Although hello portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can add one once hello VM is created.</span></span> <span data-ttu-id="aaee8-107">Tento článek ukazuje hello kroky toocreate název DNS nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="aaee8-107">This article demonstrates hello steps toocreate a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="aaee8-108">Vytvoření plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="aaee8-108">Create a FQDN</span></span>
<span data-ttu-id="aaee8-109">Tento článek předpokládá, že jste již vytvořili virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aaee8-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="aaee8-110">V případě potřeby můžete [vytvoření virtuálního počítače na portálu hello](quick-create-portal.md) nebo [s hello rozhraní příkazového řádku Azure](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="aaee8-110">If needed, you can [create a VM in hello portal](quick-create-portal.md) or [with hello Azure CLI](quick-create-cli.md).</span></span> <span data-ttu-id="aaee8-111">Po nastavení a spuštění virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="aaee8-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="aaee8-112">Nyní můžete přihlásit vzdáleně toohello název virtuálního počítače pomocí této služby DNS, jako s `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="aaee8-112">You can now connect remotely toohello VM using this DNS name such as with `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aaee8-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aaee8-113">Next steps</span></span>
<span data-ttu-id="aaee8-114">Nyní, když virtuální počítač veřejný název IP a DNS, můžete nasadit společné architektury aplikace nebo služby, jako je například nginx, MongoDB, Docker, atd.</span><span class="sxs-lookup"><span data-stu-id="aaee8-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as nginx, MongoDB, Docker, etc.</span></span>

<span data-ttu-id="aaee8-115">Také další informace o [pomocí Resource Manager](../../azure-resource-manager/resource-group-overview.md) tipy k sestavení Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="aaee8-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

