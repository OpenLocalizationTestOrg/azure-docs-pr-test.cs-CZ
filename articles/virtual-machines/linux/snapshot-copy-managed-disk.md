---
title: "Zkopírujte diskem spravované Azure pro Back up | Microsoft Docs"
description: "Naučte se vytvořit kopii Azure spravované disku pro back up nebo řešení potíží s disku."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: c91367ef11c9d531bebac7c069d2df586607ec29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="6e195-103">Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky</span><span class="sxs-lookup"><span data-stu-id="6e195-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="6e195-104">Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku a jeho připojení k testovací virtuální počítač k řešení.</span><span class="sxs-lookup"><span data-stu-id="6e195-104">Take a snapshot of a Managed disk for backup or create a Managed Disk from the snapshot and attach it to a test virtual machine to troubleshoot.</span></span> <span data-ttu-id="6e195-105">Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e195-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="6e195-106">Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk.</span><span class="sxs-lookup"><span data-stu-id="6e195-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> 

<span data-ttu-id="6e195-107">Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="6e195-107">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> <!--Add link to topic or blog post that explains managed disks. -->

<span data-ttu-id="6e195-108">K pořízení snímku na Disk spravovaný pomocí portálu Azure nebo Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="6e195-108">Use either the Azure portal or the Azure CLI 2.0 to take a snapshot of the Managed Disk.</span></span>

## <a name="use-azure-cli-20-to-take-a-snapshot"></a><span data-ttu-id="6e195-109">Použití Azure CLI 2.0 k pořízení snímku</span><span class="sxs-lookup"><span data-stu-id="6e195-109">Use Azure CLI 2.0 to take a snapshot</span></span>

> [!NOTE] 
> <span data-ttu-id="6e195-110">Následující příklad vyžaduje Azure CLI 2.0 nainstalovaná a přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e195-110">The following example requires the Azure CLI 2.0 installed and logged into your Azure account.</span></span>

<span data-ttu-id="6e195-111">Následující kroky ukazují, jak získat a pořízení snímku spravované disk operačního systému pomocí `az snapshot create` s `--source-disk` parametr.</span><span class="sxs-lookup"><span data-stu-id="6e195-111">The following steps show how to obtain and take a snapshot of a managed OS disk using the `az snapshot create` command with the `--source-disk` parameter.</span></span> <span data-ttu-id="6e195-112">Následující příklad předpokládá, že je virtuální počítač s názvem `myVM` vytvořené pomocí spravovaného disku operačního systému ve `myResourceGroup` skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e195-112">The following example assumes that there is a VM called `myVM` created with a managed OS disk in the `myResourceGroup` resource group.</span></span>

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

<span data-ttu-id="6e195-113">Výstup by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="6e195-113">The output should look something like:</span></span>

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a><span data-ttu-id="6e195-114">Pořízení snímku pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e195-114">Use Azure portal to take a snapshot</span></span> 

1. <span data-ttu-id="6e195-115">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6e195-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6e195-116">Spuštění v levé horní, klikněte na tlačítko **nový** a vyhledejte **snímku**.</span><span class="sxs-lookup"><span data-stu-id="6e195-116">Starting in the upper-left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="6e195-117">V okně snímku, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e195-117">In the Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="6e195-118">Zadejte **název** pro snímku.</span><span class="sxs-lookup"><span data-stu-id="6e195-118">Enter a **Name** for the snapshot.</span></span>
5. <span data-ttu-id="6e195-119">Vyberte existující [skupinu prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e195-119">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span> 
6. <span data-ttu-id="6e195-120">Vyberte umístění datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="6e195-120">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="6e195-121">Pro **zdrojový disk**, vyberte spravované Disk snímek.</span><span class="sxs-lookup"><span data-stu-id="6e195-121">For **Source disk**, select the Managed Disk to snapshot.</span></span>
8. <span data-ttu-id="6e195-122">Vyberte **typ účtu** používat k uložení snímku.</span><span class="sxs-lookup"><span data-stu-id="6e195-122">Select the **Account type** to use to store the snapshot.</span></span> <span data-ttu-id="6e195-123">Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6e195-123">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="6e195-124">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e195-124">Click **Create**.</span></span>

<span data-ttu-id="6e195-125">Pokud máte v plánu používat k vytvoření disku spravované a připojte ji virtuální počítač, který musí být vysoké provádění snímku, použijte parametr `--sku Premium_LRS` s `az snapshot create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6e195-125">If you plan to use the snapshot to create a Managed Disk and attach it a VM that needs to be high performing, use the parameter `--sku Premium_LRS` with the `az snapshot create` command.</span></span> <span data-ttu-id="6e195-126">Tím se vytvoří snímek tak, aby je uložena jako spravovaný Disk úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="6e195-126">This creates the snapshot so that it is stored as a Premium Managed Disk.</span></span> <span data-ttu-id="6e195-127">Prémiové disky spravované provést lépe, protože jsou disků SSD (Solid-State Drive), ale náklady více než standardní disky (HDD).</span><span class="sxs-lookup"><span data-stu-id="6e195-127">Premium Managed Disks perform better because they are solid-state drives (SSDs), but cost more than Standard disks (HDDs).</span></span>


