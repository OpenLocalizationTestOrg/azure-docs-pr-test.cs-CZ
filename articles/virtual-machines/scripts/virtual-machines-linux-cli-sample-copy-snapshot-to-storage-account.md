---
title: "Ukázka skriptu Azure CLI - Export nebo zkopírování snímku jako virtuální pevný disk na účet úložiště v jiné oblasti. | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Export nebo zkopírování snímku jako virtuální pevný disk na účet úložiště ve stejné nebo jiné předplatné"
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
ms.openlocfilehash: a6ea65aba91641ece415db4df6ae9c17b42a0954
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a><span data-ttu-id="b53de-103">Export nebo zkopírování spravované snímků jako virtuální pevný disk na účet úložiště v jiné oblasti pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b53de-103">Export/Copy managed snapshots as VHD to a storage account in different region with CLI</span></span>

<span data-ttu-id="b53de-104">Tento skript exporty spravované snímku na účet úložiště v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b53de-104">This script exports a managed snapshot to a storage account in different region.</span></span> <span data-ttu-id="b53de-105">Nejprve generuje identifikátor URI SAS snímku a používá je zkopírovat do účtu úložiště v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b53de-105">It first generates the SAS URI of the snapshot and then uses it to copy it to a storage account in different region.</span></span> <span data-ttu-id="b53de-106">Tento skript lze použijte k udržování zálohování spravované disky v jiné oblasti pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="b53de-106">Use this script to maintain backup of your managed disks in different region for disaster recovery.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b53de-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b53de-107">Sample script</span></span>

<span data-ttu-id="b53de-108">[!code-azurecli[hlavní](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "kopírování snímku")]</span><span class="sxs-lookup"><span data-stu-id="b53de-108">[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="b53de-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b53de-109">Script explanation</span></span>

<span data-ttu-id="b53de-110">Tento skript používá tyto příkazy ke generování identifikátor URI pro SAS pro spravované snímek a zkopíruje snímku na účet úložiště pomocí SAS URI.</span><span class="sxs-lookup"><span data-stu-id="b53de-110">This script uses following commands to generate SAS URI for a managed snapshot and copies the snapshot to a storage account using SAS URI.</span></span> <span data-ttu-id="b53de-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="b53de-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b53de-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b53de-112">Command</span></span> | <span data-ttu-id="b53de-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b53de-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b53de-114">AZ snímku udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="b53de-114">az snapshot grant-access</span></span>](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | <span data-ttu-id="b53de-115">Vygeneruje SAS jen pro čtení, který slouží ke kopírování podkladový soubor virtuálního pevného disku na účet úložiště nebo stáhnout do místní</span><span class="sxs-lookup"><span data-stu-id="b53de-115">Generates read-only SAS that is used to copy underlying VHD file to a storage account or download it to on-premises</span></span>  |
| [<span data-ttu-id="b53de-116">kopírování objektu blob úložiště az start</span><span class="sxs-lookup"><span data-stu-id="b53de-116">az storage blob copy start</span></span>](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | <span data-ttu-id="b53de-117">Asynchronně zkopíruje objekt blob z jednoho účtu úložiště do druhého</span><span class="sxs-lookup"><span data-stu-id="b53de-117">Copies a blob asynchronously from one storage account to another</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b53de-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b53de-118">Next steps</span></span>

[<span data-ttu-id="b53de-119">Vytvoření spravovaného disku z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="b53de-119">Create a managed disk from a VHD</span></span>](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[<span data-ttu-id="b53de-120">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="b53de-120">Create a virtual machine from a managed disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="b53de-121">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b53de-121">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b53de-122">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b53de-122">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
