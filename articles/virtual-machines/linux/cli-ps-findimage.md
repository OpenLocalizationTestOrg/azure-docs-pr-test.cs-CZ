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
# <a name="how-toofind-linux-vm-images-in-hello-azure-marketplace-with-hello-azure-cli"></a><span data-ttu-id="b1f7e-103">Jak toofind virtuálního počítače s Linuxem Image v nástroji hello Azure Marketplace s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b1f7e-103">How toofind Linux VM images in hello Azure Marketplace with hello Azure CLI</span></span>
<span data-ttu-id="b1f7e-104">Toto téma popisuje, jak toouse hello Image virtuálních počítačů Azure CLI 2.0 toofind v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-104">This topic describes how toouse hello Azure CLI 2.0 toofind VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="b1f7e-105">Pomocí této informace toospecify image pořízenou prostřednictvím Marketplace, při vytváření virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-105">Use this information toospecify a Marketplace image when you create a Linux VM.</span></span>

<span data-ttu-id="b1f7e-106">Zajistěte, aby nainstalovat nejnovější hello [Azure CLI 2.0](/cli/azure/install-az-cli2) a jsou zaznamenány v tooan účet Azure (`az login`).</span><span class="sxs-lookup"><span data-stu-id="b1f7e-106">Make sure that you installed hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and are logged in tooan Azure account (`az login`).</span></span>

## <a name="terminology"></a><span data-ttu-id="b1f7e-107">Terminologie</span><span class="sxs-lookup"><span data-stu-id="b1f7e-107">Terminology</span></span>

<span data-ttu-id="b1f7e-108">Obrázky Marketplace jsou určené v hello rozhraní příkazového řádku a další nástroje Azure podle tooa hierarchie:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-108">Marketplace images are identified in hello CLI and other Azure tools according tooa hierarchy:</span></span>

* <span data-ttu-id="b1f7e-109">**Vydavatel** -hello organizace, který vytvořili hello image.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-109">**Publisher** - hello organization that created hello image.</span></span> <span data-ttu-id="b1f7e-110">Příklad: kanonickém tvaru</span><span class="sxs-lookup"><span data-stu-id="b1f7e-110">Example: Canonical</span></span>
* <span data-ttu-id="b1f7e-111">**Nabízejí** -skupina související bitové kopie vytvořené vydavatelem.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-111">**Offer** - A group of related images created by a publisher.</span></span> <span data-ttu-id="b1f7e-112">Příklad: Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="b1f7e-112">Example: Ubuntu Server</span></span>
* <span data-ttu-id="b1f7e-113">**Skladová položka** – instanci nabídku, jako je například hlavní verze distribuce.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-113">**SKU** - An instance of an offer, such as a major release of a distribution.</span></span> <span data-ttu-id="b1f7e-114">Příklad: 16.04-LTS</span><span class="sxs-lookup"><span data-stu-id="b1f7e-114">Example: 16.04-LTS</span></span>
* <span data-ttu-id="b1f7e-115">**Verze** -hello číslo verze SKU bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-115">**Version** - hello version number of an image SKU.</span></span> <span data-ttu-id="b1f7e-116">Při zadávání hello bitové kopie, můžete nahradit hello číslo verze se "nejnovější", který vybere hello nejnovější verzi distribučních hello.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-116">When specifying hello image, you can replace hello version number with "latest", which selects hello latest version of hello distribution.</span></span>

<span data-ttu-id="b1f7e-117">toospecify image pořízenou prostřednictvím Marketplace, obvykle použijete hello image *URN*.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-117">toospecify a Marketplace image, you typically use hello image *URN*.</span></span> <span data-ttu-id="b1f7e-118">Tyto hodnoty oddělené dvojtečkou (:) znakem hello kombinuje Hello URN: *vydavatele*:*nabízejí*:*Sku*:*verze*.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-118">hello URN combines these values, separated by hello colon (:) character: *Publisher*:*Offer*:*Sku*:*Version*.</span></span> 


## <a name="list-popular-images"></a><span data-ttu-id="b1f7e-119">Seznam oblíbených obrázků</span><span class="sxs-lookup"><span data-stu-id="b1f7e-119">List popular images</span></span>

<span data-ttu-id="b1f7e-120">Spustit hello [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list) příkazu, aniž by hello `--all` možnost, toosee Image seznam oblíbených virtuálních počítačů v nástroji hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-120">Run hello [az vm image list](/cli/azure/vm/image#list) command, without hello `--all` option, toosee a list of popular VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="b1f7e-121">Například spusťte následující příkaz toodisplay hello seznam oblíbených bitové kopie v mezipaměti ve formátu tabulky:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-121">For example, run hello following command toodisplay a cached list of popular images in table format:</span></span>

```azurecli
az vm image list --output table
```

<span data-ttu-id="b1f7e-122">výstup Hello zahrnuje hello URN (hello hodnota v hello *Urn* sloupec), které použijete toospecify hello image.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-122">hello output includes hello URN (hello value in hello *Urn* column), which you use toospecify hello image.</span></span> <span data-ttu-id="b1f7e-123">Při vytváření virtuálního počítače s jedním z těchto oblíbených bitových kopií Marketplace, můžete alternativně zadat hello URN alias, například *UbuntuLTS*.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-123">When creating a VM with one of these popular Marketplace images, you can alternatively specify hello URN alias, such as *UbuntuLTS*.</span></span>

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

## <a name="find-specific-images"></a><span data-ttu-id="b1f7e-124">Vyhledání konkrétních imagí</span><span class="sxs-lookup"><span data-stu-id="b1f7e-124">Find specific images</span></span>

<span data-ttu-id="b1f7e-125">toofind konkrétní image virtuálních počítačů v hello Marketplace, použijte hello `az vm image list` s hello `--all` možnost.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-125">toofind a specific VM image in hello Marketplace, use hello `az vm image list` command with hello `--all` option.</span></span> <span data-ttu-id="b1f7e-126">Tato verze příkazu hello trvá některé toocomplete čas a vrátit zdlouhavé výstup, takže obvykle filtrovat seznam hello podle `--publisher` nebo jiný parametr.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-126">This version of hello command takes some time toocomplete and can return lengthy output, so you usually filter hello list by `--publisher` or another parameter.</span></span> 

<span data-ttu-id="b1f7e-127">Například hello následující příkaz zobrazí všechny Debian nabídky (Nezapomeňte, že bez hello `--all` přepnout, prochází pouze místní mezipaměti hello běžné bitových kopií):</span><span class="sxs-lookup"><span data-stu-id="b1f7e-127">For example, hello following command displays all Debian offers (remember that without hello `--all` switch, it only searches hello local cache of common images):</span></span>

```azurecli
az vm image list --offer Debian --all --output table 

```

<span data-ttu-id="b1f7e-128">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-128">Partial output:</span></span> 
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

<span data-ttu-id="b1f7e-129">Použití filtrů podobné s hello `--location`, `--publisher`, a `--sku` možnosti.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-129">Apply similar filters with hello `--location`, `--publisher`, and `--sku` options.</span></span> <span data-ttu-id="b1f7e-130">Termínem shodují jen částečně můžete provést i na filtrem, třeba hledání `--offer Deb` toofind všechny Debian Image.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-130">You can even perform partial matches on a filter, such as searching for `--offer Deb` toofind all Debian images.</span></span>

<span data-ttu-id="b1f7e-131">Pokud nezadáte konkrétního umístění s hello `--location` možnost hello hodnoty pro `westus` jsou vráceny ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-131">If you don't specify a particular location with hello `--location` option, hello values for `westus` are returned by default.</span></span> <span data-ttu-id="b1f7e-132">(Spuštěním nastavte jiné výchozí umístění `az configure --defaults location=<location>`.)</span><span class="sxs-lookup"><span data-stu-id="b1f7e-132">(Set a different default location by running `az configure --defaults location=<location>`.)</span></span>

<span data-ttu-id="b1f7e-133">Například následující příkaz hello uvádí všechny Debian SKU 8 v `westeurope`:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-133">For example, hello following command lists all Debian 8 SKUs in `westeurope`:</span></span>

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

<span data-ttu-id="b1f7e-134">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-134">Partial output:</span></span>

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

## <a name="navigate-hello-images"></a><span data-ttu-id="b1f7e-135">Přejděte hello obrázků</span><span class="sxs-lookup"><span data-stu-id="b1f7e-135">Navigate hello images</span></span> 
<span data-ttu-id="b1f7e-136">Jiný způsob toofind bitovou kopii v umístění je toorun hello [bitové kopie virtuálních počítačů az seznamu vydavatelů](/cli/azure/vm/image#list-publishers), [bitové kopie virtuálních počítačů az seznamu nabízí](/cli/azure/vm/image#list-offers), a [bitové kopie virtuálních počítačů az seznamu SKU](/cli/azure/vm/image#list-skus) příkazy v pořadí.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-136">Another way toofind an image in a location is toorun hello [az vm image list-publishers](/cli/azure/vm/image#list-publishers), [az vm image list-offers](/cli/azure/vm/image#list-offers), and [az vm image list-skus](/cli/azure/vm/image#list-skus) commands in sequence.</span></span> <span data-ttu-id="b1f7e-137">Pomocí těchto příkazů určit tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-137">With these commands, you determine these values:</span></span>

1. <span data-ttu-id="b1f7e-138">Seznam vydavatelů image hello.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-138">List hello image publishers.</span></span>
2. <span data-ttu-id="b1f7e-139">Pro daného vydavatele vypsat jeho nabídky.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-139">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="b1f7e-140">Pro danou nabídku vypsat její skladovou jednotku (SKU).</span><span class="sxs-lookup"><span data-stu-id="b1f7e-140">For a given offer, list their SKUs.</span></span>


<span data-ttu-id="b1f7e-141">Například hello následující příkaz seznam vydavatelů hello bitové kopie v hello západní USA umístění:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-141">For example, hello following command lists hello image publishers in hello West US location:</span></span>

```azurecli
az vm image list-publishers --location westus --output table
```

<span data-ttu-id="b1f7e-142">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-142">Partial output:</span></span>

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
<span data-ttu-id="b1f7e-143">Pomocí této informace toofind nabízí z konkrétní vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-143">Use this information toofind offers from a specific publisher.</span></span> <span data-ttu-id="b1f7e-144">Například pokud Canonical vydavatele bitové kopie v hello umístění západní USA, najít jejich nabídky spuštěním `azure vm image list-offers`.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-144">For example, if Canonical is an image publisher in hello West US location, find their offers by running `azure vm image list-offers`.</span></span> <span data-ttu-id="b1f7e-145">Předejte hello umístění a vydavatele hello jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-145">Pass hello location and hello publisher as in hello following example:</span></span>

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

<span data-ttu-id="b1f7e-146">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-146">Output:</span></span>

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
<span data-ttu-id="b1f7e-147">Uvidíte, že v oblasti západní USA hello publikuje Canonical hello **UbuntuServer** nabídku Azure.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-147">You see that in hello West US region, Canonical publishes hello **UbuntuServer** offer on Azure.</span></span> <span data-ttu-id="b1f7e-148">Ale co SKU? Spusťte tyto hodnoty tooget `azure vm image list-skus` a nastavte hello umístění, vydavatele a nabídky, které byly zjištěny:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-148">But what SKUs? tooget those values, run `azure vm image list-skus` and set hello location, publisher, and offer that you have discovered:</span></span>

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

<span data-ttu-id="b1f7e-149">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-149">Output:</span></span>

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

<span data-ttu-id="b1f7e-150">Nakonec použijte hello `az vm image list` příkaz toofind na konkrétní verzi hello SKU chcete například **16.04 LTS**:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-150">Finally, use hello `az vm image list` command toofind a specific version of hello SKU you want, for example, **16.04-LTS**:</span></span>

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

<span data-ttu-id="b1f7e-151">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b1f7e-151">Output:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="b1f7e-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1f7e-152">Next steps</span></span>
<span data-ttu-id="b1f7e-153">Teď můžete zvolit přesněji hello obraz chcete toouse podle trvá poznamenejte hello URN hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-153">Now you can choose precisely hello image you want toouse by taking note of hello URN value.</span></span> <span data-ttu-id="b1f7e-154">Předat tuto hodnotu s hello `--image` parametr při vytváření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-154">Pass this value with hello `--image` parameter when you create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="b1f7e-155">Mějte na paměti, že můžete volitelně nahradit hello číslo verze v hello URN "poslední zálohy".</span><span class="sxs-lookup"><span data-stu-id="b1f7e-155">Remember that you can optionally replace hello version number in hello URN with "latest".</span></span> <span data-ttu-id="b1f7e-156">Tato verze je vždy hello nejnovější verzi distribučních hello.</span><span class="sxs-lookup"><span data-stu-id="b1f7e-156">This version is always hello latest version of hello distribution.</span></span> <span data-ttu-id="b1f7e-157">toocreate virtuální počítač rychle pomocí hello URN informace najdete v tématu [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello](tutorial-manage-vm.md).</span><span class="sxs-lookup"><span data-stu-id="b1f7e-157">toocreate a virtual machine quickly by using hello URN information, see [Create and Manage Linux VMs with hello Azure CLI](tutorial-manage-vm.md).</span></span>
