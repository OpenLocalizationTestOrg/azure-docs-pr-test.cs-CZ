---
title: "Vytvoření zoned virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Linuxem v zóně dostupnosti pomocí Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: 5e742187295d0bd6dbc0767ee164335fc0cf9f02
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Vytvořit virtuální počítač s Linuxem v zóně dostupnosti pomocí Azure CLI

Tento článek obsahuje kroky prostřednictvím rozhraní příkazového řádku Azure k vytvoření virtuálního počítače s Linuxem v zóně Azure dostupnosti. [Zóna dostupnosti](../../availability-zones/az-overview.md) je fyzicky oddělená zóna v oblasti Azure. Zóny dostupnosti se používají k ochraně aplikací a dat před málo pravděpodobným selháním nebo ztrátou celého datového centra.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Ujistěte se, že jste nainstalovali nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlášený k účtu Azure s [az přihlášení](/cli/azure/#login).


## <a name="check-vm-sku-availability"></a>Zkontrolovat dostupnost virtuálních počítačů SKU
Dostupnost velikosti virtuálních počítačů nebo jednotky SKU, se může lišit podle oblasti a zóny. Při plánování pro používání zón, dostupnosti, můžete vytvořit seznam dostupné edice virtuálních počítačů tak, že oblast Azure a zóny. Tato možnost je zajištěno, vyberte správnou velikost virtuálního počítače a získat požadované odolnosti napříč zóny. Další informace o různých typech virtuálních počítačů a velikosti najdete v tématu [velikosti virtuálních počítačů přehled](sizes.md).

Můžete zobrazit dostupné edice virtuálních počítačů s [seznamu virtuálních počítačů az-SKU](/cli/azure/vm#az_vm_list_skus) příkaz. Následující příklad vypíše dostupné edice virtuálních počítačů v *eastus2* oblasti:

```azurecli
az vm list-skus --location eastus2 --output table
```

Výstup se podobá následující zhuštěné příkladu, který ukazuje dostupnost zóny, ve kterém je každý velikost virtuálního počítače k dispozici:

```azurecli
ResourceType      Locations  Name               Tier       Size     Zones
----------------  ---------  -----------------  ---------  -------  -------
virtualMachines   eastus2    Standard_DS1_v2    Standard   DS1_v2   1,2,3
virtualMachines   eastus2    Standard_DS2_v2    Standard   DS2_v2   1,2,3
[...]
virtualMachines   eastus2    Standard_F1s       Standard   F1s      1,2,3
virtualMachines   eastus2    Standard_F2s       Standard   F2s      1,2,3
[...]
virtualMachines   eastus2    Standard_D2s_v3    Standard   D2_v3    1,2,3
virtualMachines   eastus2    Standard_D4s_v3    Standard   D4_v3    1,2,3
[...]
virtualMachines   eastus2    Standard_E2_v3     Standard   E2_v3    1,2,3
virtualMachines   eastus2    Standard_E4_v3     Standard   E4_v3    1,2,3
```


## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#az_group_create).  

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v *eastus2* oblast. Oblast Východní USA 2 je jednou z oblasti Azure, které podporují zóny dostupnosti ve verzi Preview.

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus2
```

Skupina prostředků je zadána při vytváření nebo úpravách virtuální počítač, který v tomto článku si můžete prohlédnout.

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#az_vm_create) příkaz. 

Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností. V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* systémem Ubuntu Server. Vytvoření virtuálního počítače v zóně dostupnosti *1*. Ve výchozím nastavení, je virtuální počítač vytvořený v *Standard_DS1_v2* velikost. Tato velikost je podporovaná ve verzi preview zón dostupnosti.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

To může trvat několik minut pro vytvoření virtuálního počítače. Po vytvoření virtuálního počítače Azure CLI výstupy informace o virtuálním počítači. Poznamenejte si `zones` hodnotu, která určuje dostupnosti zónu, ve kterém je virtuální počítač spuštěný. 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Zóna pro IP adresu a spravovaný disk

Po nasazení virtuálního počítače v zóně dostupnosti IP adres a spravovaných disků na prostředky nasazených ve stejném pásmu dostupnosti. Následující příklady získat informace o těchto prostředcích.

Prvním použití [az virtuálních počítačů seznamu ip-addresses](/cli/azure/vm#az_vm_list_ip_addresses) příkaz vrátí název prostředek veřejné IP adresy v *Můjvp*. V tomto příkladu je název uložené v proměnné, která se používá v pozdější fázi.

```azurecli-interactive
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

Teď můžete získat informace o IP adrese:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

Výstup ukazuje, že IP adresa je v zóně dostupnosti jako virtuální počítač:

```azurecli-interactive
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

Stejně tak ověřte, že spravovaného disku Virtuálního počítače je v zóně dostupnosti. Použití [az virtuálních počítačů zobrazit](/cli/azure/vm#az_vm_show) příkaz vrátí id disku. V tomto příkladu je id disku uložené v proměnné, která se používá v pozdější fázi. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
Teď můžete získat informace o spravovaných disků:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

Výstup ukazuje, že spravovaný disk je ve stejné zóně dostupnosti jako virtuální počítač:

```azurecli-interactive
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/Subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2017-09-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```
 


## <a name="next-steps"></a>Další postup

V tomto článku jste se naučili vytvářet virtuální počítač v zóně dostupnosti. Přečtěte si další informace o [oblastech a dostupnosti](regions-and-availability.md) pro virtuální počítače Azure.




