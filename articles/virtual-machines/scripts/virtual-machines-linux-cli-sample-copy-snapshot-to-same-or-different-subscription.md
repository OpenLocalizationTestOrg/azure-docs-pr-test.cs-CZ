---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného pomocí rozhraní příkazového řádku"
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
ms.openlocfilehash: f214ab1fc1cb2cb42479d82e455f20a8cc55c83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-toosame-or-different-subscription-with-cli"></a><span data-ttu-id="0ef07-103">Kopírování snímku toosame spravovaného disku nebo jiného předplatného pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0ef07-103">Copy snapshot of a managed disk toosame or different subscription with CLI</span></span>

<span data-ttu-id="0ef07-104">Tento skript zkopíruje snímek toosame spravovaného disku nebo jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="0ef07-104">This script copies a snapshot of a managed disk toosame or different subscription.</span></span> <span data-ttu-id="0ef07-105">Použijte tento skript toomove předplatné toodifferent snímku v hello stejné oblasti jako hello nadřazené snímku.</span><span class="sxs-lookup"><span data-stu-id="0ef07-105">Use this script toomove a snapshot toodifferent subscription in hello same region as hello parent snapshot.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="0ef07-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0ef07-106">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="0ef07-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="0ef07-107">Script explanation</span></span>

<span data-ttu-id="0ef07-108">Tento skript používá následující příkazy toocreate snímku hello cílové předplatné pomocí hello Id hello zdroj snímku.</span><span class="sxs-lookup"><span data-stu-id="0ef07-108">This script uses following commands toocreate a snapshot in hello target subscription using hello Id of hello source snapshot.</span></span> <span data-ttu-id="0ef07-109">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0ef07-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0ef07-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="0ef07-110">Command</span></span> | <span data-ttu-id="0ef07-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0ef07-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0ef07-112">Zobrazit az snímku</span><span class="sxs-lookup"><span data-stu-id="0ef07-112">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="0ef07-113">Získá všechny vlastnosti hello snímku pomocí názvu hello a vlastnosti skupiny prostředků hello snímku.</span><span class="sxs-lookup"><span data-stu-id="0ef07-113">Gets all hello properties of a snapshot using hello name and resource group properties of hello snapshot.</span></span> <span data-ttu-id="0ef07-114">Vlastnost ID je použité toocopy hello snímku toodifferent předplatné.</span><span class="sxs-lookup"><span data-stu-id="0ef07-114">Id property is used toocopy hello snapshot toodifferent subscription.</span></span>  |
| [<span data-ttu-id="0ef07-115">Vytvoření snímku az</span><span class="sxs-lookup"><span data-stu-id="0ef07-115">az snapshot create</span></span>](https://docs.microsoft.com/cli/azure/snapshot#create) | <span data-ttu-id="0ef07-116">Kopie snímků vytvořením snímek pomocí jiného předplatného hello Id a název hello nadřazené snímku.</span><span class="sxs-lookup"><span data-stu-id="0ef07-116">Copies a snapshot by creating a snapshot in different subscription using hello Id and name of hello parent snapshot.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="0ef07-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ef07-117">Next steps</span></span>

[<span data-ttu-id="0ef07-118">Vytvoření virtuálního počítače ze snímku</span><span class="sxs-lookup"><span data-stu-id="0ef07-118">Create a virtual machine from a snapshot</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="0ef07-119">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0ef07-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0ef07-120">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0ef07-120">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
