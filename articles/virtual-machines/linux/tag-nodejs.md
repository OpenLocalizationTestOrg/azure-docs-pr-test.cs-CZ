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
ms.openlocfilehash: f643001c85e127ae39e9869ffdc689bcac232ccb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="b6e7f-103">Postup označení virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="b6e7f-103">How to tag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="b6e7f-104">Tento článek popisuje různé způsoby, jak označit virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-104">This article describes different ways to tag a Linux virtual machine in Azure through the Resource Manager deployment model.</span></span> <span data-ttu-id="b6e7f-105">Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="b6e7f-106">Azure aktuálně podporuje až 15 značky pro každý prostředků a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-106">Azure currently supports up to 15 tags per resource and resource group.</span></span> <span data-ttu-id="b6e7f-107">Značky mohou umístit na prostředku v době vytvoření nebo přidat do existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-107">Tags may be placed on a resource at the time of creation or added to an existing resource.</span></span> <span data-ttu-id="b6e7f-108">Poznámka: značky jsou podporovány pro prostředky vytvořené pomocí modelu nasazení Resource Manager jenom.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-108">Please note, tags are supported for resources created via the Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="b6e7f-109">Označování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b6e7f-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="b6e7f-110">Pokud chcete začít, [instalace a konfigurace rozhraní příkazového řádku Azure](../../xplat-cli-azure-resource-manager.md) a zkontrolujte, zda jsou v režimu Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="b6e7f-110">To begin, [install and configure the Azure CLI](../../xplat-cli-azure-resource-manager.md) and make sure you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="b6e7f-111">Můžete zobrazit všechny vlastnosti pro daný virtuální počítač, včetně značky, použití tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="b6e7f-111">You can view all properties for a given Virtual Machine, including the tags, using this command:</span></span>

        azure vm show -g MyResourceGroup -n MyTestVM

<span data-ttu-id="b6e7f-112">Chcete-li přidat novou značku virtuálních počítačů prostřednictvím rozhraní příkazového řádku Azure, můžete použít `azure vm set` příkazu společně s parametrem značka **-t**:</span><span class="sxs-lookup"><span data-stu-id="b6e7f-112">To add a new VM tag through the Azure CLI, you can use the `azure vm set` command along with the tag parameter **-t**:</span></span>

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

<span data-ttu-id="b6e7f-113">Chcete-li odebrat všechny značky, můžete použít **– T** parametr ve `azure vm set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-113">To remove all tags, you can use the **–T** parameter in the `azure vm set` command.</span></span>

        azure vm set – g MyResourceGroup –n MyTestVM -T


<span data-ttu-id="b6e7f-114">Teď, když jsme bylo použito značky naše prostředky Azure CLI a portálu, Podívejme se na podrobnosti využití zobrazíte značky v portálu fakturace.</span><span class="sxs-lookup"><span data-stu-id="b6e7f-114">Now that we have applied tags to our resources Azure CLI and the Portal, let’s take a look at the usage details to see the tags in the billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="b6e7f-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6e7f-115">Next steps</span></span>
* <span data-ttu-id="b6e7f-116">Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [pomocí značek k uspořádání prostředků Azure][Using Tags to organize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="b6e7f-116">To learn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].</span></span>
* <span data-ttu-id="b6e7f-117">Jak značky vám může pomoci spravovat používání prostředků Azure, najdete v tématu [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure][Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="b6e7f-117">To see how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
