---
title: "aaaCopy Azure spravované na disku pro Back up | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii toouse Azure spravované disku pro disk zpět nahoru nebo řešení problémů s problémy."
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
ms.openlocfilehash: 41b91c2d68eb5be9c493a66be5f7d085a70450d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="048a2-103">Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky</span><span class="sxs-lookup"><span data-stu-id="048a2-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="048a2-104">Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku hello a připojte ji tooa testovací virtuální počítač tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="048a2-104">Take a snapshot of a Managed disk for backup or create a Managed Disk from hello snapshot and attach it tooa test virtual machine tootroubleshoot.</span></span> <span data-ttu-id="048a2-105">Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="048a2-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="048a2-106">Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk.</span><span class="sxs-lookup"><span data-stu-id="048a2-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> 

<span data-ttu-id="048a2-107">Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="048a2-107">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> <!--Add link tootopic or blog post that explains managed disks. -->

<span data-ttu-id="048a2-108">Použijte buď hello Azure portal nebo hello Azure CLI 2.0 tootake snímek hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="048a2-108">Use either hello Azure portal or hello Azure CLI 2.0 tootake a snapshot of hello Managed Disk.</span></span>

## <a name="use-azure-cli-20-tootake-a-snapshot"></a><span data-ttu-id="048a2-109">Použití Azure CLI 2.0 tootake snímku</span><span class="sxs-lookup"><span data-stu-id="048a2-109">Use Azure CLI 2.0 tootake a snapshot</span></span>

> [!NOTE] 
> <span data-ttu-id="048a2-110">Hello následující příklad vyžaduje hello Azure CLI 2.0 nainstalované a přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="048a2-110">hello following example requires hello Azure CLI 2.0 installed and logged into your Azure account.</span></span>

<span data-ttu-id="048a2-111">Hello následující kroky ukazují, jak tooobtain a proveďte snímek spravované OS disk s využitím hello `az snapshot create` s hello `--source-disk` parametr.</span><span class="sxs-lookup"><span data-stu-id="048a2-111">hello following steps show how tooobtain and take a snapshot of a managed OS disk using hello `az snapshot create` command with hello `--source-disk` parameter.</span></span> <span data-ttu-id="048a2-112">Hello následující příklad předpokládá, že je virtuální počítač s názvem `myVM` vytvořené pomocí spravovaného disk operačního systému v hello `myResourceGroup` skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="048a2-112">hello following example assumes that there is a VM called `myVM` created with a managed OS disk in hello `myResourceGroup` resource group.</span></span>

```azure-cli
# take hello disk id with which toocreate a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

<span data-ttu-id="048a2-113">výstup Hello by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="048a2-113">hello output should look something like:</span></span>

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

## <a name="use-azure-portal-tootake-a-snapshot"></a><span data-ttu-id="048a2-114">Použití portálu Azure tootake snímku</span><span class="sxs-lookup"><span data-stu-id="048a2-114">Use Azure portal tootake a snapshot</span></span> 

1. <span data-ttu-id="048a2-115">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="048a2-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="048a2-116">Spuštění v levém horním hello, klikněte na tlačítko **nový** a vyhledejte **snímku**.</span><span class="sxs-lookup"><span data-stu-id="048a2-116">Starting in hello upper-left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="048a2-117">V okně hello snímku, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="048a2-117">In hello Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="048a2-118">Zadejte **název** pro vytvoření snímku hello.</span><span class="sxs-lookup"><span data-stu-id="048a2-118">Enter a **Name** for hello snapshot.</span></span>
5. <span data-ttu-id="048a2-119">Vyberte existující [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové.</span><span class="sxs-lookup"><span data-stu-id="048a2-119">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span> 
6. <span data-ttu-id="048a2-120">Vyberte umístění datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="048a2-120">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="048a2-121">Pro **zdrojový disk**, vyberte hello toosnapshot spravované disku.</span><span class="sxs-lookup"><span data-stu-id="048a2-121">For **Source disk**, select hello Managed Disk toosnapshot.</span></span>
8. <span data-ttu-id="048a2-122">Vyberte hello **typ účtu** toouse toostore hello snímku.</span><span class="sxs-lookup"><span data-stu-id="048a2-122">Select hello **Account type** toouse toostore hello snapshot.</span></span> <span data-ttu-id="048a2-123">Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.</span><span class="sxs-lookup"><span data-stu-id="048a2-123">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="048a2-124">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="048a2-124">Click **Create**.</span></span>

<span data-ttu-id="048a2-125">Pokud plánování toouse hello snímku toocreate Disk spravované a připojte ji virtuální počítač, který potřebuje toobe vysoké provádění, použijte parametr hello `--sku Premium_LRS` s hello `az snapshot create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="048a2-125">If you plan toouse hello snapshot toocreate a Managed Disk and attach it a VM that needs toobe high performing, use hello parameter `--sku Premium_LRS` with hello `az snapshot create` command.</span></span> <span data-ttu-id="048a2-126">Tím se vytvoří snímek hello tak, aby je uložena jako spravovaný Disk úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="048a2-126">This creates hello snapshot so that it is stored as a Premium Managed Disk.</span></span> <span data-ttu-id="048a2-127">Prémiové disky spravované provést lépe, protože jsou disků SSD (Solid-State Drive), ale náklady více než standardní disky (HDD).</span><span class="sxs-lookup"><span data-stu-id="048a2-127">Premium Managed Disks perform better because they are solid-state drives (SSDs), but cost more than Standard disks (HDDs).</span></span>


