---
title: "aaaHow tootag se virtuální počítač Azure Linux | Microsoft Docs"
description: "Informace o označování se virtuální počítač Azure Linux vytvořené v Azure pomocí modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: 456b226af4495c3b446cb79c99cf9494dde9fca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="81ea8-103">Jak tootag virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="81ea8-103">How tootag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="81ea8-104">Tento článek popisuje různé způsoby tootag virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="81ea8-104">This article describes different ways tootag a Linux virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="81ea8-105">Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů.</span><span class="sxs-lookup"><span data-stu-id="81ea8-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="81ea8-106">Azure aktuálně podporuje až too15 značky na prostředků a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="81ea8-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="81ea8-107">Značky mohou být uvedeny na prostředku v době vytvoření hello nebo přidat tooan existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="81ea8-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="81ea8-108">Poznámka: značky jsou podporovány pro vytvořené prostřednictvím hello modelu nasazení Resource Manager pouze prostředky.</span><span class="sxs-lookup"><span data-stu-id="81ea8-108">Please note, tags are supported for resources created via hello Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="81ea8-109">Označování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="81ea8-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="81ea8-110">toobegin, budete potřebovat hello nejnovější [2.0 rozhraní příkazového řádku Azure (Preview)](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="81ea8-110">toobegin, you need hello latest [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="81ea8-111">Můžete také provést tyto kroky hello [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="81ea8-111">You can also perform these steps with hello [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="81ea8-112">Můžete zobrazit všechny vlastnosti pro daný virtuální počítač, včetně hello značky, použití tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="81ea8-112">You can view all properties for a given Virtual Machine, including hello tags, using this command:</span></span>

        az vm show --resource-group MyResourceGroup --name MyTestVM

<span data-ttu-id="81ea8-113">tooadd novou značku virtuálních počítačů prostřednictvím hello rozhraní příkazového řádku Azure, můžete použít hello `azure vm update` příkazu společně s hello značky parametr **– nastavte**:</span><span class="sxs-lookup"><span data-stu-id="81ea8-113">tooadd a new VM tag through hello Azure CLI, you can use hello `azure vm update` command along with hello tag parameter **--set**:</span></span>

        az vm update --resource-group MyResourceGroup --name MyTestVM –-set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2

<span data-ttu-id="81ea8-114">tooremove značky, můžete použít hello **– odebrat** parametr v hello `azure vm update` příkaz.</span><span class="sxs-lookup"><span data-stu-id="81ea8-114">tooremove tags, you can use hello **--remove** parameter in hello `azure vm update` command.</span></span>

        az vm update –-resource-group MyResourceGroup –-name MyTestVM --remove tags.myNewTagName1


<span data-ttu-id="81ea8-115">Teď, když jsme provedli značky tooour prostředky Azure CLI a hello portál, Podívejme se na toosee podrobnosti o využití hello hello značky hello fakturace portálu.</span><span class="sxs-lookup"><span data-stu-id="81ea8-115">Now that we have applied tags tooour resources Azure CLI and hello Portal, let’s take a look at hello usage details toosee hello tags in hello billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="81ea8-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81ea8-116">Next steps</span></span>
* <span data-ttu-id="81ea8-117">toolearn Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [tooorganize pomocí značky vašich prostředků Azure] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="81ea8-117">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="81ea8-118">toosee způsobu značky vám může pomoci spravovat používání prostředky Azure, najdete v [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="81ea8-118">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
