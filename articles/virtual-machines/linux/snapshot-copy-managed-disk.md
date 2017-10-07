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
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky
Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku hello a připojte ji tooa testovací virtuální počítač tootroubleshoot. Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů. Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk. 

Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/). <!--Add link tootopic or blog post that explains managed disks. -->

Použijte buď hello Azure portal nebo hello Azure CLI 2.0 tootake snímek hello spravované disku.

## <a name="use-azure-cli-20-tootake-a-snapshot"></a>Použití Azure CLI 2.0 tootake snímku

> [!NOTE] 
> Hello následující příklad vyžaduje hello Azure CLI 2.0 nainstalované a přihlášení k účtu Azure.

Hello následující kroky ukazují, jak tooobtain a proveďte snímek spravované OS disk s využitím hello `az snapshot create` s hello `--source-disk` parametr. Hello následující příklad předpokládá, že je virtuální počítač s názvem `myVM` vytvořené pomocí spravovaného disk operačního systému v hello `myResourceGroup` skupinu prostředků.

```azure-cli
# take hello disk id with which toocreate a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

výstup Hello by měl vypadat podobně jako:

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

## <a name="use-azure-portal-tootake-a-snapshot"></a>Použití portálu Azure tootake snímku 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Spuštění v levém horním hello, klikněte na tlačítko **nový** a vyhledejte **snímku**.
3. V okně hello snímku, klikněte na tlačítko **vytvořit**.
4. Zadejte **název** pro vytvoření snímku hello.
5. Vyberte existující [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové. 
6. Vyberte umístění datového centra Azure.  
7. Pro **zdrojový disk**, vyberte hello toosnapshot spravované disku.
8. Vyberte hello **typ účtu** toouse toostore hello snímku. Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.
9. Klikněte na možnost **Vytvořit**.

Pokud plánování toouse hello snímku toocreate Disk spravované a připojte ji virtuální počítač, který potřebuje toobe vysoké provádění, použijte parametr hello `--sku Premium_LRS` s hello `az snapshot create` příkaz. Tím se vytvoří snímek hello tak, aby je uložena jako spravovaný Disk úrovně Premium. Prémiové disky spravované provést lépe, protože jsou disků SSD (Solid-State Drive), ale náklady více než standardní disky (HDD).


