---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Export nebo zkopírování snímku jako účet úložiště tooa virtuálního pevného disku v jiné oblasti. | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Export nebo zkopírování snímku jako účet úložiště tooa virtuálního pevného disku v rámci stejného nebo jiného předplatného"
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
ms.openlocfilehash: 027c5e588c4f10d64d125c17f4c78a7d8e1ef060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-cli"></a><span data-ttu-id="99190-103">Export nebo zkopírování spravované snímků jako účet úložiště tooa virtuálního pevného disku v jiné oblasti pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="99190-103">Export/Copy managed snapshots as VHD tooa storage account in different region with CLI</span></span>

<span data-ttu-id="99190-104">Tento skript exportuje účtu úložiště spravovaného snímku tooa v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="99190-104">This script exports a managed snapshot tooa storage account in different region.</span></span> <span data-ttu-id="99190-105">Nejprve generuje hello identifikátor URI pro SAS hello snímku a používá je toocopy ho tooa účet úložiště v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="99190-105">It first generates hello SAS URI of hello snapshot and then uses it toocopy it tooa storage account in different region.</span></span> <span data-ttu-id="99190-106">Použijte tuto zálohu toomaintain skriptu spravované disky v jiné oblasti pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="99190-106">Use this script toomaintain backup of your managed disks in different region for disaster recovery.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="99190-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="99190-107">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="99190-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="99190-108">Script explanation</span></span>

<span data-ttu-id="99190-109">Tento skript používá následující příkazy toogenerate tooa účet úložiště pomocí SAS URI pořízení snímku identifikátor URI pro SAS pro spravované hello snímku a kopií.</span><span class="sxs-lookup"><span data-stu-id="99190-109">This script uses following commands toogenerate SAS URI for a managed snapshot and copies hello snapshot tooa storage account using SAS URI.</span></span> <span data-ttu-id="99190-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="99190-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="99190-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="99190-111">Command</span></span> | <span data-ttu-id="99190-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="99190-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="99190-113">AZ snímku udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="99190-113">az snapshot grant-access</span></span>](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | <span data-ttu-id="99190-114">Vygeneruje SAS jen pro čtení, použité toocopy základní účet úložiště tooa souboru virtuálního pevného disku nebo ho stáhnout tooon místní</span><span class="sxs-lookup"><span data-stu-id="99190-114">Generates read-only SAS that is used toocopy underlying VHD file tooa storage account or download it tooon-premises</span></span>  |
| [<span data-ttu-id="99190-115">kopírování objektu blob úložiště az start</span><span class="sxs-lookup"><span data-stu-id="99190-115">az storage blob copy start</span></span>](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | <span data-ttu-id="99190-116">Asynchronně zkopíruje objekt blob z jednoho tooanother účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="99190-116">Copies a blob asynchronously from one storage account tooanother</span></span> |

## <a name="next-steps"></a><span data-ttu-id="99190-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99190-117">Next steps</span></span>

[<span data-ttu-id="99190-118">Vytvoření spravovaného disku z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="99190-118">Create a managed disk from a VHD</span></span>](./../scripts/storage-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[<span data-ttu-id="99190-119">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="99190-119">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="99190-120">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="99190-120">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="99190-121">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="99190-121">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
