---
title: "Azure ukázkový skript prostředí PowerShell - Export nebo zkopírování snímku jako virtuální pevný disk na účet úložiště v jiné oblasti. | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Export nebo zkopírování snímku jako virtuální pevný disk na účet úložiště ve stejné oblasti jiné"
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
ms.openlocfilehash: 80f83f4b66165ddd2bdd0edca2be5d026cb11a9b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a><span data-ttu-id="8ff8e-103">Export nebo zkopírování spravované snímků jako virtuální pevný disk na účet úložiště v jiné oblasti pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ff8e-103">Export/Copy managed snapshots as VHD to a storage account in different region with PowerShell</span></span>

<span data-ttu-id="8ff8e-104">Tento skript exporty spravované snímku na účet úložiště v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-104">This script exports a managed snapshot to a storage account in different region.</span></span> <span data-ttu-id="8ff8e-105">Nejprve generuje identifikátor URI SAS snímku a používá je zkopírovat do účtu úložiště v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-105">It first generates the SAS URI of the snapshot and then uses it to copy it to a storage account in different region.</span></span> <span data-ttu-id="8ff8e-106">Tento skript lze použijte k udržování zálohování spravované disky v jiné oblasti pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-106">Use this script to maintain backup of your managed disks in different region for disaster recovery.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8ff8e-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8ff8e-107">Sample script</span></span>

<span data-ttu-id="8ff8e-108">[!code-powershell[hlavní](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "kopírování snímku")]</span><span class="sxs-lookup"><span data-stu-id="8ff8e-108">[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="8ff8e-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8ff8e-109">Script explanation</span></span>

<span data-ttu-id="8ff8e-110">Tento skript používá tyto příkazy ke generování identifikátor URI pro SAS pro spravované snímek a zkopíruje snímku na účet úložiště pomocí SAS URI.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-110">This script uses following commands to generate SAS URI for a managed snapshot and copies the snapshot to a storage account using SAS URI.</span></span> <span data-ttu-id="8ff8e-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8ff8e-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8ff8e-112">Command</span></span> | <span data-ttu-id="8ff8e-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8ff8e-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8ff8e-114">Udělení AzureRmSnapshotAccess</span><span class="sxs-lookup"><span data-stu-id="8ff8e-114">Grant-AzureRmSnapshotAccess</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="8ff8e-115">Identifikátor URI pro SAS se generuje pro snímek, který se používá ke zkopírování na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-115">Generates SAS URI for a snapshot that is used to copy it to a storage account.</span></span> |
| [<span data-ttu-id="8ff8e-116">Nové AzureStorageContext</span><span class="sxs-lookup"><span data-stu-id="8ff8e-116">New-AzureStorageContext</span></span>](/powershell/module/azure.storage/New-AzureStorageContext) | <span data-ttu-id="8ff8e-117">Vytvoří kontext účtu úložiště pomocí názvu účtu a klíč.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-117">Creates a storage account context using the account name and key.</span></span> <span data-ttu-id="8ff8e-118">Tento kontext lze použít k provedení operace čtení a zápisu v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8ff8e-118">This context can be used to perform read/write operations on the storage account.</span></span> |
| [<span data-ttu-id="8ff8e-119">Počáteční AzureStorageBlobCopy</span><span class="sxs-lookup"><span data-stu-id="8ff8e-119">Start-AzureStorageBlobCopy</span></span>](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | <span data-ttu-id="8ff8e-120">Zkopíruje základní virtuální pevný disk snímku na účet úložiště</span><span class="sxs-lookup"><span data-stu-id="8ff8e-120">Copies the underlying VHD of a snapshot to a storage account</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8ff8e-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ff8e-121">Next steps</span></span>

[<span data-ttu-id="8ff8e-122">Vytvoření spravovaného disku z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="8ff8e-122">Create a managed disk from a VHD</span></span>](./../scripts/storage-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[<span data-ttu-id="8ff8e-123">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="8ff8e-123">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="8ff8e-124">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8ff8e-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8ff8e-125">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8ff8e-125">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>