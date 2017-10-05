---
title: "Převést na sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Vytvořit a nasadit škálování virtuálního počítače Linux Azure nastavit pomocí Azure CLI."
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
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a>Převést stávající virtuální počítač Azure na škálovací sadě

V tomto kurzu se dozvíte, jak používat Azure CLI 2.0 převést virtuální počítač na škálovací sadu virtuálních počítačů. Můžete také informace o automatizaci konfigurace virtuálních počítačů v sadě škálování. Další informace o tom, jak nainstalovat Azure CLI 2.0, naleznete v části [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-the-vm"></a>Krok 1 – zrušení zřízení virtuálního počítače

Použití SSH se připojit k virtuálnímu počítači.

Zrušení zřízení virtuálního počítače pomocí agenta virtuálního počítače Azure se odstranit soubory a data. Podrobný přehled o zrušení zřízení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a>Krok 2 – Vytvoření bitové kopie virtuálního počítače

Podrobný přehled o zachycení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).

Zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate):

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Generalize virtuálního počítače s [az virtuálních počítačů zobecní](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Vytvoření image z prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a>Krok 3 – vytvoření sady škálování

Získat **id** bitové kopie.

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

Tento příkaz také připojit datový disk 10gb. Mějte na paměti, který v závislosti na virtuální počítač velikost zvolenou (jsme použili **Standard_DS1_v2**), počet datových disků povolená, se liší. Další informace najdete v článku [velikostí virtuálních počítačů](sizes.md).

Po dokončení nastavení měřítka připojte se k němu. Získat seznam IP adres pro instance pro SSH s [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info):

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Nyní můžete připojit k instanci virtuálního počítače k chybě při inicializaci datový disk

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a>Krok 4 – inicializovat datový disk

Při připojení k virtuálnímu počítači, oddílu disku spolu s `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

Zápis systém souborů k oddílu s `mkfs` příkaz:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Připojte nový disk, takže bude přístupný v operačním systému:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

Disk může být nyní přistupuje prostřednictvím datadrive přípojný bod, který lze ověřit pomocí `ls /datadrive/`.

Ukončení relace SSH.


## <a name="step-5---configure-firewall"></a>Krok 5: Konfigurace brány firewall

Děrování díru přes bránu firewall, aby webový server hostované byly sadou škálování. Pokud byly sadou škálování byl vytvořen, byla vytvořena i nástroj pro vyrovnávání zatížení a ho používají **SSH** pro jednotlivé virtuální počítače. Chcete-li otevřít port, je třeba dva kusy informace, které můžete získat pomocí rozhraní příkazového řádku Azure.

* **Fond adres IP front-endu**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Fond back-end IP adres**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

S těmito dva názvy, můžete otevřít port **80**.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>Krok 6 – automatické konfiguraci

Datový disk musí být nakonfigurované na každou instanci virtuálního počítače. Jsme můžete automatizovat konfiguraci virtuálního počítače s **CustomScript** rozšíření.

Nejprve vytvořte *.sh* skript, který obsahuje příkazy formátu disku.

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

V dalším kroku nahrát daného souboru skriptu k umístění, kde **CustomScript** rozšíření k němu přístup. Je k dispozici kopii [zde](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).

Vytvořte místní soubor s názvem **settings.json** a uveďte následující blok JSON. `flieUris` Musí být vlastnost nastavena, kde byla nahrát váš soubor skriptu do.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

Nasadit tento příkaz vaší škálování s **CustomScript** rozšíření, odkazující **settings.json** souboru jsme právě vytvořili.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Toto rozšíření se automaticky spustí na všechny aktuální instance a všechny instance později vytvořené škálování.

## <a name="step-7---configure-autoscale-rules"></a>Krok 7: Konfigurace pravidel automatického škálování

Škálování pravidla se v současné době nelze nastavit v rozhraní příkazového řádku Azure. Použití [portál Azure](https://portal.azure.com) ke konfiguraci automatického škálování.

## <a name="step-8---management-tasks"></a>Krok 8 – úlohy správy

V průběhu cyklu škálovací sady můžete spustit jeden nebo více úloh správy. Kromě toho můžete chtít vytvořit skripty, které automatizují různé úlohy životního cyklu a rozhraní příkazového řádku Azure poskytuje rychlý způsob, jak provést tyto úlohy. Tady jsou několik běžných úloh.

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
Další informace o některých funkcí sady škálování virtuálního počítače byla zavedená v tomto kurzu, přečtěte si následující informace:

- [Přehled sady škálování virtuálního počítače Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Azure Load Balancer – přehled](../../load-balancer/load-balancer-overview.md)
- [Řízení toku provozu sítě s skupin zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md)