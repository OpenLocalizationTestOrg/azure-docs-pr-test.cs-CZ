---
title: "aaaAzure ukázka skriptu prostředí PowerShell - Export nebo zkopírování snímku jako účet úložiště tooa virtuálního pevného disku v jiné oblasti. | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Export nebo zkopírování snímku jako účet úložiště tooa virtuální pevný disk ve stejné oblasti jiné"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: c18ad4fa0bf12033fafe941a807e7b4c8d233a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-powershell"></a><span data-ttu-id="ee62d-103">Export nebo zkopírování spravované snímků jako účet úložiště tooa virtuálního pevného disku v jiné oblasti pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ee62d-103">Export/Copy managed snapshots as VHD tooa storage account in different region with PowerShell</span></span>

<span data-ttu-id="ee62d-104">Tento skript exportuje účtu úložiště spravovaného snímku tooa v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="ee62d-104">This script exports a managed snapshot tooa storage account in different region.</span></span> <span data-ttu-id="ee62d-105">Nejprve generuje hello identifikátor URI pro SAS hello snímku a používá je toocopy ho tooa účet úložiště v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="ee62d-105">It first generates hello SAS URI of hello snapshot and then uses it toocopy it tooa storage account in different region.</span></span> <span data-ttu-id="ee62d-106">Použijte tuto zálohu toomaintain skriptu spravované disky v jiné oblasti pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="ee62d-106">Use this script toomaintain backup of your managed disks in different region for disaster recovery.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ee62d-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ee62d-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="ee62d-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ee62d-108">Script explanation</span></span>

<span data-ttu-id="ee62d-109">Tento skript používá následující příkazy toogenerate tooa účet úložiště pomocí SAS URI pořízení snímku identifikátor URI pro SAS pro spravované hello snímku a kopií.</span><span class="sxs-lookup"><span data-stu-id="ee62d-109">This script uses following commands toogenerate SAS URI for a managed snapshot and copies hello snapshot tooa storage account using SAS URI.</span></span> <span data-ttu-id="ee62d-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ee62d-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ee62d-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ee62d-111">Command</span></span> | <span data-ttu-id="ee62d-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ee62d-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ee62d-113">Udělení AzureRmSnapshotAccess</span><span class="sxs-lookup"><span data-stu-id="ee62d-113">Grant-AzureRmSnapshotAccess</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="ee62d-114">Vygeneruje SAS URI pro snímek, který je použité toocopy ho tooa účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ee62d-114">Generates SAS URI for a snapshot that is used toocopy it tooa storage account.</span></span> |
| [<span data-ttu-id="ee62d-115">Nové AzureStorageContext</span><span class="sxs-lookup"><span data-stu-id="ee62d-115">New-AzureStorageContext</span></span>](/powershell/module/azure.storage/New-AzureStorageContext) | <span data-ttu-id="ee62d-116">Vytvoří kontext účtu úložiště pomocí hello název účtu a klíč.</span><span class="sxs-lookup"><span data-stu-id="ee62d-116">Creates a storage account context using hello account name and key.</span></span> <span data-ttu-id="ee62d-117">Tento kontext může být použité tooperform operace čtení a zápisu v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ee62d-117">This context can be used tooperform read/write operations on hello storage account.</span></span> |
| [<span data-ttu-id="ee62d-118">Počáteční AzureStorageBlobCopy</span><span class="sxs-lookup"><span data-stu-id="ee62d-118">Start-AzureStorageBlobCopy</span></span>](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | <span data-ttu-id="ee62d-119">Kopie hello základní virtuální pevný disk účtu úložiště tooa snímku</span><span class="sxs-lookup"><span data-stu-id="ee62d-119">Copies hello underlying VHD of a snapshot tooa storage account</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ee62d-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee62d-120">Next steps</span></span>

[<span data-ttu-id="ee62d-121">Vytvoření spravovaného disku z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="ee62d-121">Create a managed disk from a VHD</span></span>](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[<span data-ttu-id="ee62d-122">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="ee62d-122">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="ee62d-123">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ee62d-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="ee62d-124">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ee62d-124">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
