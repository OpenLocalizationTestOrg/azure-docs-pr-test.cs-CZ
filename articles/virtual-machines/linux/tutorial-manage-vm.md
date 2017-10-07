---
title: "aaaCreate a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvořit a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 05f7c1cf860f809bc13f110778d3bddd619ac6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-linux-vms-with-hello-azure-cli"></a>Vytvořit a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure

Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí. Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit a připojit tooa virtuálních počítačů
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz. 

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v hello *eastus* oblast. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

Skupina prostředků Hello je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvoření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create) příkaz. 

Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností. V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* systémem Ubuntu Server. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

Jednou hello, kterou virtuální počítač byl vytvořen, hello rozhraní příkazového řádku Azure výstupy informace o hello virtuálních počítačů. Poznamenejte si hello `publicIpAddress`, tato adresa může být použité tooaccess hello virtuální počítač... 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-toovm"></a>Připojit tooVM

Nyní můžete přihlásit toohello virtuálních počítačů pomocí protokolu SSH. Nahraďte hello příklad IP adresu hello `publicIpAddress` si poznamenali v předchozím kroku hello.

```bash
ssh 52.174.34.95
```

Po dokončení s hello virtuálního počítače, zavřete relace SSH hello. 

```bash
exit
```

## <a name="understand-vm-images"></a>Pochopení Image virtuálních počítačů

Hello Azure marketplace obsahuje celou řadu imagí, které se dají použít toocreate virtuálních počítačů. V předchozích krocích hello byl vytvořen virtuální počítač pomocí Ubuntu obrázku. V tomto kroku hello rozhraní příkazového řádku Azure je použité toosearch hello marketplace CentOS obrázek, který je pak používá toodeploy druhý virtuální počítač.  

toosee seznam hello nejčastěji používaná bitové kopie, použijte hello [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list) příkaz.

```azurecli-interactive 
az vm image list --output table
```

výstup příkazu Hello vrátí hello nejoblíbenější Image virtuálních počítačů v Azure.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

Úplný seznam můžete zobrazit tak, že přidáte hello `--all` argument. seznam obrázků Hello lze také filtrovat podle `--publisher` nebo `–-offer`. V tomto příkladu hello seznam je filtrovaný pro všechny Image s nabídku odpovídající *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Částečné výstup:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

toodeploy virtuálního počítače pomocí konkrétní image, poznamenejte si hodnotu hello v hello *Urn* sloupce. Při zadávání hello bitové kopie, číslo verze bitové kopie hello lze nahradit "nejnovější", který vybere hello nejnovější verzi distribučních hello. V tomto příkladu hello `--image` argument je použité toospecify hello nejnovější verze CentOS 6.5 bitové kopie.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>Pochopení velikosti virtuálních počítačů

Velikost virtuálního počítače určuje hello množství výpočetní prostředky, například procesoru, grafického procesoru a paměti, které jsou vytvářeny k dispozici toohello virtuálního počítače. Třeba virtuální počítače toobe odpovídající velikost pro hello očekává pracovní zatížení. Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.

### <a name="vm-sizes"></a>Velikosti virtuálních počítačů

Následující tabulka Hello rozděluje velikosti do případy použití.  

| Typ                     | Velikosti           |    Popis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Obecné účely](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7| Vyrovnáváním procesoru do paměti. Ideální pro vývojáře nebo test a řešení pro malé toomedium aplikacím a datům.  |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)   | Služby FS, F             | Vysoké využití procesoru do paměti. Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.        |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Vysoká paměti na core. Výborně hodí pro relačních databází, střední toolarge mezipaměti a analýzy v paměti.                 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md)      | Ls                | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat.                                                         |
| [GPU](sizes-gpu.md)          | VS, NC            | Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.       |
| [Vysoký výkon](sizes-hpc.md) | H, A8 11          | Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA). 


### <a name="find-available-vm-sizes"></a>Najít dostupných velikostí virtuálních počítačů

toosee seznam virtuálních počítačů velikostí k dispozici v určité oblasti, použijte hello [seznam velikosti virtuálních počítačů az](/cli/azure/vm#list-sizes) příkaz. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Částečné výstup:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Vytvoření virtuálního počítače s určitou velikost

V hello předchozí příklad vytvoření virtuálního počítače velikost nebylo zadáno, výsledkem je výchozí velikost. Velikost virtuálního počítače lze vybrat pomocí čas vytvoření [vytvořit virtuální počítač az](/cli/azure/vm#create) a hello `--size` argument. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>Změna velikosti virtuálního počítače

Po nasazení virtuálního počítače, může být změněnou tooincrease nebo snížit přidělení prostředků.

Před změnou velikosti virtuálního počítače, zkontrolujte, zda hello požadovaná velikost je k dispozici na aktuální Azure cluster hello. Hello [az virtuálních počítačů seznamu-virtuálních počítačů-změny velikosti options](/cli/azure/vm#list-vm-resize-options) příkaz vrátí hello seznam velikostí. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
V případě, že velikost je k dispozici potřeby hello hello virtuálních počítačů velikost lze změnit ze stavu na zapnuté, ale po restartu během operace hello. Použití hello [změnit velikost virtuálního počítače az]( /cli/azure/vm#resize) příkaz tooperform hello změny velikosti.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

V případě potřeby hello velikost není v aktuální clusteru hello hello virtuálních počítačů potřeb, které může dojít toobe navrácena před hello změnit velikost operace. Použití hello [az OM deallocate]( /cli/azure/vm#deallocate) příkaz toostop a navrátit hello virtuálních počítačů. Všimněte si, když hello virtuální počítač je zapnutý zpět, může odeberou všechna data na dočasné disku hello. Hello veřejnou IP adresu také změní, pokud se používá statickou IP adresu. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

Jakmile navrácena, může dojít, změny velikosti hello. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

Po změně velikosti hello, lze spustit hello virtuálních počítačů.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>Stavy spotřeby virtuálních počítačů

Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie. Tento stav představuje hello aktuální stav hello virtuálních počítačů z hlediska hello hello hypervisoru. 

### <a name="power-states"></a>Stavy spotřeby.

| Stav napájení | Popis
|----|----|
| Spouštění | Označuje, že se spouští hello virtuálního počítače. |
| Běžící (Spuštěno) | Označuje, že aby hello virtuální počítač běží. |
| Zastavování | Označuje, že Probíhá zastavování virtuálního počítače hello. | 
| Zastaveno | Označuje, že tento virtuální počítač hello je zastavena. Virtuální počítače ve stavu Zastaveno hello stále platit poplatky výpočty.  |
| Rušení přidělování | Označuje, že tento virtuální počítač hello je navrácena. |
| Zrušeno | Označuje, že hello virtuálního počítače je odebrán z hello hypervisoru, ale stále k dispozici v rovině řízení hello. Virtuální počítače v hello Deallocated stavu nevznikají poplatky za výpočty. |
| - | Označuje, že stav napájení hello hello virtuálního počítače neznámý. |

### <a name="find-power-state"></a>Najít stav napájení

Stav hello tooretrieve konkrétní virtuální počítač, použijte hello [az virtuální počítač získat zobrazení instance](/cli/azure/vm#get-instance-view) příkaz. Zda toospecify být platný název pro virtuální počítač a skupinu prostředků. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Výstup:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Úlohy správy

Během hello životního cyklu virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače. Kromě toho můžete toocreate skripty tooautomate opakovaných nebo komplexní úlohy. Pomocí hello rozhraní příkazového řádku Azure, mnoho běžné úlohy správy lze spustit z příkazového řádku hello nebo ve skriptech. 

### <a name="get-ip-address"></a>Získat IP adresu

Tento příkaz vrátí hello privátní a veřejné IP adresy virtuálního počítače.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Zastavit virtuální počítač

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Spustit virtuální počítač

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Odstraňte skupinu prostředků

Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:

> [!div class="checklist"]
> * Vytvořit a připojit tooa virtuálních počítačů
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače

Posunutí další kurz toolearn toohello o disky virtuálních počítačů.  

> [!div class="nextstepaction"]
> [Vytvoření a správa virtuálních počítačů disky](./tutorial-manage-disks.md)
