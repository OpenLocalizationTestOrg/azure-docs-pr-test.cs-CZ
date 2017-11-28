---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - kopírování (přesunout) spravované toosame disky nebo jiné předplatné | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku - toosame disky kopírování (přesunout), spravovat nebo jiném předplatném."
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: b1fa207bd6e05d7094be08855e7823e3b7686013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-toosame-or-different-subscription-with-cli"></a><span data-ttu-id="5b7ed-103">Zkopírujte toosame spravované disky nebo jiné předplatné pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b7ed-103">Copy managed disks toosame or different subscription with CLI</span></span>

<span data-ttu-id="5b7ed-104">Tento skript zkopíruje toosame spravovaného disku nebo jiného předplatného, ale v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="5b7ed-104">This script copies a managed disk toosame or different subscription but in hello same region.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5b7ed-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5b7ed-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a><span data-ttu-id="5b7ed-106">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5b7ed-106">Script explanation</span></span>

<span data-ttu-id="5b7ed-107">Tento skript používá následující příkazy toocreate nový disk spravované hello cílové předplatné pomocí hello Id zdroje hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="5b7ed-107">This script uses following commands toocreate a new managed disk in hello target subscription using hello Id of hello source managed disk.</span></span> <span data-ttu-id="5b7ed-108">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5b7ed-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5b7ed-109">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5b7ed-109">Command</span></span> | <span data-ttu-id="5b7ed-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5b7ed-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5b7ed-111">Zobrazit az disku</span><span class="sxs-lookup"><span data-stu-id="5b7ed-111">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="5b7ed-112">Získá všechny vlastnosti hello spravovaného disku pomocí hello název a prostředků vlastnosti skupiny hello spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="5b7ed-112">Gets all hello properties of a managed disk using hello name and resource group properties of hello managed disk.</span></span> <span data-ttu-id="5b7ed-113">Vlastnost ID je použité toocopy hello spravované disku toodifferent předplatné.</span><span class="sxs-lookup"><span data-stu-id="5b7ed-113">Id property is used toocopy hello managed disk toodifferent subscription.</span></span>  |
| [<span data-ttu-id="5b7ed-114">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="5b7ed-114">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="5b7ed-115">Kopíruje se spravovaným diskem vytvořením nového spravovaného disku v jiném předplatném. pomocí Id a název nadřazené hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="5b7ed-115">Copies a managed disk by creating a new managed disk in different subscription using Id and name hello parent managed disk.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="5b7ed-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b7ed-116">Next steps</span></span>

[<span data-ttu-id="5b7ed-117">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="5b7ed-117">Create a virtual machine from a managed disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="5b7ed-118">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5b7ed-118">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5b7ed-119">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b7ed-119">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
