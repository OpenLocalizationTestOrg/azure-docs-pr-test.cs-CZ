---
title: "aaaConvert Azure tooa sady škálování virtuálního počítače | Microsoft Docs"
description: "Vytvořit a nasadit škálování virtuálního počítače Linux Azure s hello rozhraní příkazového řádku Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
ms.openlocfilehash: e228282ac4855cef589b8500e74e9d461f9aed84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-an-existing-azure-virtual-machine-tooa-scale-set"></a>Převést existující sady škálování virtuálního počítače Azure tooa

Tento kurz ukazuje, jak tooconvert toouse 2.0 rozhraní příkazového řádku Azure virtuální počítač tooa škálovací sady virtuálních počítačů. Také zjistíte, jak nastavit konfiguraci hello tooautomate hello virtuální počítače ve škálovací hello. Další informace o tom, jak tooinstall rozhraní příkazového řádku Azure 2.0, najdete v části [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-hello-vm"></a>Krok 1 - Deprovision hello virtuálních počítačů

Pomocí SSH tooconnect toohello virtuálních počítačů.

Deprovision hello virtuálních počítačů pomocí souborů toodelete agenta virtuálního počítače Azure hello a data. Podrobný přehled o zrušení zřízení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-hello-vm"></a>Krok 2 – Vytvoření bitové kopie hello virtuálních počítačů

Podrobný přehled o zachycení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).

Deallocate hello virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate):

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Generalize hello virtuálního počítače s [az virtuálních počítačů zobecní](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Vytvoření image z hello prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-hello-scale-set"></a>Krok 3 – vytvoření sadě škálování hello

Získat hello **id** hello bitové kopie.

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

Vytvořte virtuální počítač z bitové kopie prostředku s [vytvořit az vmss](/cli/azure/vmss#create):

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

Tento příkaz také připojit datový disk 10gb. Mějte na paměti, který v závislosti na hello virtuálních počítačů velikost zvolenou (jsme použili **Standard_DS1_v2**), se liší hello počet datových disků povolená. Další informace najdete v tématu hello [velikostí virtuálních počítačů](sizes.md).

Po dokončení nastavení škálování hello připojte tooit. Získat seznam IP adres pro instance hello SSH s [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info):

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Teď se můžete připojit toohello virtuální počítač instance tooinitialize hello datový disk

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-hello-data-disk"></a>Krok 4 – inicializovat hello datový disk

Při připojené toohello virtuálního počítače, oddílu hello disk s `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

Zápis systému souborů toohello oddílu s hello `mkfs` příkaz:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Připojte nový disk hello tak, aby se v hello operačního systému:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

Hello disk může být nyní přistupuje prostřednictvím přípojný bod datadrive hello, který lze ověřit pomocí `ls /datadrive/`.

Ukončení relace SSH hello.


## <a name="step-5---configure-firewall"></a>Krok 5: Konfigurace brány firewall

Děrování díru prostřednictvím brány firewall webový server toohello hello hostované hello škálovací sada. Pokud byla vytvořena hello škálovací sadu, byla vytvořena i nástroj pro vyrovnávání zatížení a ho používají **SSH** toohello jednotlivé virtuální počítače. tooopen port, budete potřebovat dva kusy informace, které můžete získat pomocí rozhraní příkazového řádku Azure.

* **Fond adres IP front-endu**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Fond back-end IP adres**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

S těmito dva názvy, můžete otevřít port **80**.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>Krok 6 – automatické konfiguraci

datový disk Hello musí toobe nakonfigurované na každou instanci virtuálního počítače. Jsme můžete automatizovat konfiguraci hello hello virtuálního počítače s hello **CustomScript** rozšíření.

Nejprve vytvořte *.sh* skript, který obsahuje příkazy formát disku hello.

```sh
#!/bin/bash

# Setup hello data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

V dalším kroku nahrát tento skript soubor toowhere hello **CustomScript** rozšíření k němu přístup. Je k dispozici kopii [zde](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).

Vytvořte místní soubor s názvem **settings.json** a put hello následující bloku JSON v ní. Hello `flieUris` vlastnost musí být nastavená toowhere nahranému v souboru skriptu.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

Nasazení tohoto měřítka tooyour příkaz s hello **CustomScript** rozšíření, odkazující na hello **settings.json** souboru jsme právě vytvořili.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Toto rozšíření se automaticky spustí na všechny aktuální instance a všechny instance později vytvořené škálování.

## <a name="step-7---configure-autoscale-rules"></a>Krok 7: Konfigurace pravidel automatického škálování

Škálování pravidla se v současné době nelze nastavit v rozhraní příkazového řádku Azure. Použití hello [portál Azure](https://portal.azure.com) tooconfigure škálování.

## <a name="step-8---management-tasks"></a>Krok 8 – úlohy správy

V průběhu cyklu hello škálovací sady hello, může být nutné toorun jedné nebo více úloh správy. Kromě toho může být vhodné toocreate skripty, které automatizují různé úlohy životního cyklu a hello rozhraní příkazového řádku Azure poskytuje rychlý způsob toodo tyto úlohy. Tady jsou několik běžných úloh.

### <a name="get-connection-info"></a>Získání informací o připojení

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a>Nastavit počet instancí (ruční škálování)

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a>Odstraňte skupinu prostředků

Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky
toolearn informace o některých škálování virtuálních počítačů hello nastavit funkce zavedená v tomto kurzu, najdete v části hello následující informace:

- [Přehled sady škálování virtuálního počítače Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Azure Load Balancer – přehled](../../load-balancer/load-balancer-overview.md)
- [Řízení toku provozu sítě s skupin zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md)