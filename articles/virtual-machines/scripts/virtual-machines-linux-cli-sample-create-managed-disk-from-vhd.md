---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření spravovaného disku ze souboru VHD v účtu úložiště v hello stejného předplatného. | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze souboru VHD v účtu úložiště v hello stejného předplatného."
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
ms.openlocfilehash: 6cfb3c54a7692b0f3999c585861340c1a6b4d348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-hello-same-subscription-with-cli"></a><span data-ttu-id="81e5f-103">Vytvoření spravovaného disku ze souboru VHD v účtu úložiště v hello stejného předplatného pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="81e5f-103">Create a managed disk from a VHD file in a storage account in hello same subscription with CLI</span></span>

<span data-ttu-id="81e5f-104">Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="81e5f-104">This script creates a managed disk from a VHD file in a storage account in hello same subscription.</span></span> <span data-ttu-id="81e5f-105">Použijte tento skript tooimport specializované (není zobecněný/Sysprep) virtuálního pevného disku toomanaged operačního systému disku toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="81e5f-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD toomanaged OS disk toocreate a virtual machine.</span></span> <span data-ttu-id="81e5f-106">Nebo ho používat tooimport data virtuálního pevného disku toomanaged datový disk.</span><span class="sxs-lookup"><span data-stu-id="81e5f-106">Or, use it tooimport a data VHD toomanaged data disk.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="81e5f-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="81e5f-107">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="81e5f-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="81e5f-108">Script explanation</span></span>

<span data-ttu-id="81e5f-109">Tento skript používá tyto příkazy toocreate spravované disk z virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="81e5f-109">This script uses following commands toocreate a managed disk from a VHD.</span></span> <span data-ttu-id="81e5f-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="81e5f-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="81e5f-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="81e5f-111">Command</span></span> | <span data-ttu-id="81e5f-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="81e5f-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="81e5f-113">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="81e5f-113">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="81e5f-114">Vytvoří se spravovaným diskem pomocí identifikátoru URI virtuálního pevného disku v účtu úložiště v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="81e5f-114">Creates a managed disk using URI of a VHD in a storage account in hello same subscription</span></span> |

## <a name="next-steps"></a><span data-ttu-id="81e5f-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81e5f-115">Next steps</span></span>

[<span data-ttu-id="81e5f-116">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="81e5f-116">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="81e5f-117">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="81e5f-117">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="81e5f-118">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="81e5f-118">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
