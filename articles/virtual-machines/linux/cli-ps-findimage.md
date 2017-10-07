---
title: "bitové kopie virtuálního počítače s Linuxem aaaSelect s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello toodetermine hello vydavatel rozhraní příkazového řádku Azure, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/24/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0b115b8654bc156b5bfadba53a6b002a105acb68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-linux-vm-images-in-hello-azure-marketplace-with-hello-azure-cli"></a>Jak toofind virtuálního počítače s Linuxem Image v nástroji hello Azure Marketplace s hello rozhraní příkazového řádku Azure
Toto téma popisuje, jak toouse hello Image virtuálních počítačů Azure CLI 2.0 toofind v hello Azure Marketplace. Pomocí této informace toospecify image pořízenou prostřednictvím Marketplace, při vytváření virtuálního počítače s Linuxem.

Zajistěte, aby nainstalovat nejnovější hello [Azure CLI 2.0](/cli/azure/install-az-cli2) a jsou zaznamenány v tooan účet Azure (`az login`).

## <a name="terminology"></a>Terminologie

Obrázky Marketplace jsou určené v hello rozhraní příkazového řádku a další nástroje Azure podle tooa hierarchie:

* **Vydavatel** -hello organizace, který vytvořili hello image. Příklad: kanonickém tvaru
* **Nabízejí** -skupina související bitové kopie vytvořené vydavatelem. Příklad: Ubuntu Server
* **Skladová položka** – instanci nabídku, jako je například hlavní verze distribuce. Příklad: 16.04-LTS
* **Verze** -hello číslo verze SKU bitové kopie. Při zadávání hello bitové kopie, můžete nahradit hello číslo verze se "nejnovější", který vybere hello nejnovější verzi distribučních hello.

toospecify image pořízenou prostřednictvím Marketplace, obvykle použijete hello image *URN*. Tyto hodnoty oddělené dvojtečkou (:) znakem hello kombinuje Hello URN: *vydavatele*:*nabízejí*:*Sku*:*verze*. 


## <a name="list-popular-images"></a>Seznam oblíbených obrázků

Spustit hello [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list) příkazu, aniž by hello `--all` možnost, toosee Image seznam oblíbených virtuálních počítačů v nástroji hello Azure Marketplace. Například spusťte následující příkaz toodisplay hello seznam oblíbených bitové kopie v mezipaměti ve formátu tabulky:

```azurecli
az vm image list --output table
```

výstup Hello zahrnuje hello URN (hello hodnota v hello *Urn* sloupec), které použijete toospecify hello image. Při vytváření virtuálního počítače s jedním z těchto oblíbených bitových kopií Marketplace, můžete alternativně zadat hello URN alias, například *UbuntuLTS*.

```
You are viewing an offline list of images, use --all tooretrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Vyhledání konkrétních imagí

toofind konkrétní image virtuálních počítačů v hello Marketplace, použijte hello `az vm image list` s hello `--all` možnost. Tato verze příkazu hello trvá některé toocomplete čas a vrátit zdlouhavé výstup, takže obvykle filtrovat seznam hello podle `--publisher` nebo jiný parametr. 

Například hello následující příkaz zobrazí všechny Debian nabídky (Nezapomeňte, že bez hello `--all` přepnout, prochází pouze místní mezipaměti hello běžné bitových kopií):

```azurecli
az vm image list --offer Debian --all --output table 

```

Částečné výstup: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
...
```

Použití filtrů podobné s hello `--location`, `--publisher`, a `--sku` možnosti. Termínem shodují jen částečně můžete provést i na filtrem, třeba hledání `--offer Deb` toofind všechny Debian Image.

Pokud nezadáte konkrétního umístění s hello `--location` možnost hello hodnoty pro `westus` jsou vráceny ve výchozím nastavení. (Spuštěním nastavte jiné výchozí umístění `az configure --defaults location=<location>`.)

Například následující příkaz hello uvádí všechny Debian SKU 8 v `westeurope`:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Částečné výstup:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```

## <a name="navigate-hello-images"></a>Přejděte hello obrázků 
Jiný způsob toofind bitovou kopii v umístění je toorun hello [bitové kopie virtuálních počítačů az seznamu vydavatelů](/cli/azure/vm/image#list-publishers), [bitové kopie virtuálních počítačů az seznamu nabízí](/cli/azure/vm/image#list-offers), a [bitové kopie virtuálních počítačů az seznamu SKU](/cli/azure/vm/image#list-skus) příkazy v pořadí. Pomocí těchto příkazů určit tyto hodnoty:

1. Seznam vydavatelů image hello.
2. Pro daného vydavatele vypsat jeho nabídky.
3. Pro danou nabídku vypsat její skladovou jednotku (SKU).


Například hello následující příkaz seznam vydavatelů hello bitové kopie v hello západní USA umístění:

```azurecli
az vm image list-publishers --location westus --output table
```

Částečné výstup:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Pomocí této informace toofind nabízí z konkrétní vydavatele. Například pokud Canonical vydavatele bitové kopie v hello umístění západní USA, najít jejich nabídky spuštěním `azure vm image list-offers`. Předejte hello umístění a vydavatele hello jako hello následující ukázka:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Výstup:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Uvidíte, že v oblasti západní USA hello publikuje Canonical hello **UbuntuServer** nabídku Azure. Ale co SKU? Spusťte tyto hodnoty tooget `azure vm image list-skus` a nastavte hello umístění, vydavatele a nabídky, které byly zjištěny:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Výstup:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Nakonec použijte hello `az vm image list` příkaz toofind na konkrétní verzi hello SKU chcete například **16.04 LTS**:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Výstup:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```
## <a name="next-steps"></a>Další kroky
Teď můžete zvolit přesněji hello obraz chcete toouse podle trvá poznamenejte hello URN hodnotu. Předat tuto hodnotu s hello `--image` parametr při vytváření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. Mějte na paměti, že můžete volitelně nahradit hello číslo verze v hello URN "poslední zálohy". Tato verze je vždy hello nejnovější verzi distribučních hello. toocreate virtuální počítač rychle pomocí hello URN informace najdete v tématu [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello](tutorial-manage-vm.md).
