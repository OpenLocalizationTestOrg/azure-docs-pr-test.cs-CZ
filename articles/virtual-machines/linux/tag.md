---
title: "Postup označení se virtuální počítač Azure Linux | Microsoft Docs"
description: "Informace o označování se virtuální počítač Azure Linux vytvořené v Azure pomocí modelu nasazení Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: da3ff0de2a5d6ac8994b7c16b758f976228a53b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="3fe49-103">Postup označení virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="3fe49-103">How to tag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="3fe49-104">Tento článek popisuje různé způsoby, jak označit virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3fe49-104">This article describes different ways to tag a Linux virtual machine in Azure through the Resource Manager deployment model.</span></span> <span data-ttu-id="3fe49-105">Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů.</span><span class="sxs-lookup"><span data-stu-id="3fe49-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="3fe49-106">Azure aktuálně podporuje až 15 značky pro každý prostředků a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="3fe49-106">Azure currently supports up to 15 tags per resource and resource group.</span></span> <span data-ttu-id="3fe49-107">Značky mohou umístit na prostředku v době vytvoření nebo přidat do existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="3fe49-107">Tags may be placed on a resource at the time of creation or added to an existing resource.</span></span> <span data-ttu-id="3fe49-108">Poznámka: značky jsou podporovány pro prostředky vytvořené pomocí modelu nasazení Resource Manager jenom.</span><span class="sxs-lookup"><span data-stu-id="3fe49-108">Please note, tags are supported for resources created via the Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="3fe49-109">Označování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="3fe49-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="3fe49-110">Chcete-li začít, je třeba nejnovější [2.0 rozhraní příkazového řádku Azure (Preview)](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3fe49-110">To begin, you need the latest [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="3fe49-111">K provedení těchto kroků můžete také využít [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3fe49-111">You can also perform these steps with the [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="3fe49-112">Můžete zobrazit všechny vlastnosti pro daný virtuální počítač, včetně značky, použití tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="3fe49-112">You can view all properties for a given Virtual Machine, including the tags, using this command:</span></span>

        az vm show --resource-group MyResourceGroup --name MyTestVM

<span data-ttu-id="3fe49-113">Chcete-li přidat novou značku virtuálních počítačů prostřednictvím rozhraní příkazového řádku Azure, můžete použít `azure vm update` příkazu společně s parametrem značka **– nastavte**:</span><span class="sxs-lookup"><span data-stu-id="3fe49-113">To add a new VM tag through the Azure CLI, you can use the `azure vm update` command along with the tag parameter **--set**:</span></span>

        az vm update --resource-group MyResourceGroup --name MyTestVM –-set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2

<span data-ttu-id="3fe49-114">Chcete-li odebrat značky, můžete použít **– odebrat** parametr ve `azure vm update` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3fe49-114">To remove tags, you can use the **--remove** parameter in the `azure vm update` command.</span></span>

        az vm update –-resource-group MyResourceGroup –-name MyTestVM --remove tags.myNewTagName1


<span data-ttu-id="3fe49-115">Teď, když jsme bylo použito značky naše prostředky Azure CLI a portálu, Podívejme se na podrobnosti využití zobrazíte značky v portálu fakturace.</span><span class="sxs-lookup"><span data-stu-id="3fe49-115">Now that we have applied tags to our resources Azure CLI and the Portal, let’s take a look at the usage details to see the tags in the billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="3fe49-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3fe49-116">Next steps</span></span>
* <span data-ttu-id="3fe49-117">Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [pomocí značek k uspořádání prostředků Azure][Using Tags to organize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="3fe49-117">To learn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].</span></span>
* <span data-ttu-id="3fe49-118">Jak značky vám může pomoci spravovat používání prostředků Azure, najdete v tématu [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure][Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="3fe49-118">To see how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
