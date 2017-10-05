---
title: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejném předplatném | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze souboru VHD v účtu úložiště v rámci stejného předplatného"
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
ms.openlocfilehash: 448636e87db126defc804a613bb61ff19a086ad9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a><span data-ttu-id="13e83-103">Vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejném předplatném pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="13e83-103">Create a managed disk from a VHD file in a storage account in the same subscription with CLI</span></span>

<span data-ttu-id="13e83-104">Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="13e83-104">This script creates a managed disk from a VHD file in a storage account in the same subscription.</span></span> <span data-ttu-id="13e83-105">Pomocí tohoto skriptu k importu specializované (není zobecněný/Sysprep) virtuálního pevného disku na disk spravovaný operačního systému k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="13e83-105">Use this script to import a specialized (not generalized/sysprepped) VHD to managed OS disk to create a virtual machine.</span></span> <span data-ttu-id="13e83-106">Nebo použijte k importu dat virtuálního pevného disku na disk spravovaný data.</span><span class="sxs-lookup"><span data-stu-id="13e83-106">Or, use it to import a data VHD to managed data disk.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="13e83-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="13e83-107">Sample script</span></span>

<span data-ttu-id="13e83-108">[!code-azurecli[hlavní](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "vytvořit spravovaného disku z virtuálního pevného disku")]</span><span class="sxs-lookup"><span data-stu-id="13e83-108">[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="13e83-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="13e83-109">Script explanation</span></span>

<span data-ttu-id="13e83-110">Tento skript používá následující příkazy k vytvoření spravovaného disku z virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="13e83-110">This script uses following commands to create a managed disk from a VHD.</span></span> <span data-ttu-id="13e83-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="13e83-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="13e83-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="13e83-112">Command</span></span> | <span data-ttu-id="13e83-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="13e83-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="13e83-114">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="13e83-114">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="13e83-115">Vytvoří se spravovaným diskem pomocí identifikátoru URI virtuálního pevného disku v účtu úložiště v rámci stejného předplatného</span><span class="sxs-lookup"><span data-stu-id="13e83-115">Creates a managed disk using URI of a VHD in a storage account in the same subscription</span></span> |

## <a name="next-steps"></a><span data-ttu-id="13e83-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13e83-116">Next steps</span></span>

[<span data-ttu-id="13e83-117">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="13e83-117">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="13e83-118">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="13e83-118">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="13e83-119">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="13e83-119">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
