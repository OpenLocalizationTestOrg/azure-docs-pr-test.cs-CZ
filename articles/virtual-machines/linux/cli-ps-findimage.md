---
title: "Vyberte bitové kopie virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku Azure k určení vydavatele, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
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
ms.openlocfilehash: e0c27a7ee9e9a7ab1a3b004e070fa556b56a36a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a><span data-ttu-id="b999b-103">Postup nalezení bitové kopie virtuálního počítače s Linuxem v Azure Marketplace pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b999b-103">How to find Linux VM images in the Azure Marketplace with the Azure CLI</span></span>
<span data-ttu-id="b999b-104">Toto téma popisuje, jak pomocí Azure CLI 2.0 najít Image virtuálních počítačů v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b999b-104">This topic describes how to use the Azure CLI 2.0 to find VM images in the Azure Marketplace.</span></span> <span data-ttu-id="b999b-105">Tyto informace slouží k určení image pořízenou prostřednictvím Marketplace, při vytváření virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="b999b-105">Use this information to specify a Marketplace image when you create a Linux VM.</span></span>

<span data-ttu-id="b999b-106">Ujistěte se, že jste nainstalovali nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a jsou přihlášení k účtu Azure (`az login`).</span><span class="sxs-lookup"><span data-stu-id="b999b-106">Make sure that you installed the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and are logged in to an Azure account (`az login`).</span></span>

## <a name="terminology"></a><span data-ttu-id="b999b-107">Terminologie</span><span class="sxs-lookup"><span data-stu-id="b999b-107">Terminology</span></span>

<span data-ttu-id="b999b-108">Obrázky Marketplace jsou určené v rozhraní příkazového řádku a dalších nástrojů Azure podle hierarchie:</span><span class="sxs-lookup"><span data-stu-id="b999b-108">Marketplace images are identified in the CLI and other Azure tools according to a hierarchy:</span></span>

* <span data-ttu-id="b999b-109">**Vydavatel** -organizace, který vytvořil bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="b999b-109">**Publisher** - The organization that created the image.</span></span> <span data-ttu-id="b999b-110">Příklad: kanonickém tvaru</span><span class="sxs-lookup"><span data-stu-id="b999b-110">Example: Canonical</span></span>
* <span data-ttu-id="b999b-111">**Nabízejí** -skupina související bitové kopie vytvořené vydavatelem.</span><span class="sxs-lookup"><span data-stu-id="b999b-111">**Offer** - A group of related images created by a publisher.</span></span> <span data-ttu-id="b999b-112">Příklad: Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="b999b-112">Example: Ubuntu Server</span></span>
* <span data-ttu-id="b999b-113">**Skladová položka** – instanci nabídku, jako je například hlavní verze distribuce.</span><span class="sxs-lookup"><span data-stu-id="b999b-113">**SKU** - An instance of an offer, such as a major release of a distribution.</span></span> <span data-ttu-id="b999b-114">Příklad: 16.04-LTS</span><span class="sxs-lookup"><span data-stu-id="b999b-114">Example: 16.04-LTS</span></span>
* <span data-ttu-id="b999b-115">**Verze** -číslo verze SKU bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b999b-115">**Version** - The version number of an image SKU.</span></span> <span data-ttu-id="b999b-116">Při zadávání bitovou kopii, můžete nahradit číslo verze se "nejnovější", který vybere nejnovější verzi rozdělení.</span><span class="sxs-lookup"><span data-stu-id="b999b-116">When specifying the image, you can replace the version number with "latest", which selects the latest version of the distribution.</span></span>

<span data-ttu-id="b999b-117">Pokud chcete zadat image pořízenou prostřednictvím Marketplace, obvykle použít image *URN*.</span><span class="sxs-lookup"><span data-stu-id="b999b-117">To specify a Marketplace image, you typically use the image *URN*.</span></span> <span data-ttu-id="b999b-118">Název URN kombinuje tyto hodnoty oddělené dvojtečkou (:): *vydavatele*:*nabízejí*:*Sku*:*verze*.</span><span class="sxs-lookup"><span data-stu-id="b999b-118">The URN combines these values, separated by the colon (:) character: *Publisher*:*Offer*:*Sku*:*Version*.</span></span> 


## <a name="list-popular-images"></a><span data-ttu-id="b999b-119">Seznam oblíbených obrázků</span><span class="sxs-lookup"><span data-stu-id="b999b-119">List popular images</span></span>

<span data-ttu-id="b999b-120">Spustit [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list) příkazu, aniž by `--all` možnost, podívejte se do seznamu oblíbených Image virtuálních počítačů v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b999b-120">Run the [az vm image list](/cli/azure/vm/image#list) command, without the `--all` option, to see a list of popular VM images in the Azure Marketplace.</span></span> <span data-ttu-id="b999b-121">Například spusťte následující příkaz, který zobrazí seznam oblíbených bitové kopie v mezipaměti ve formátu tabulky:</span><span class="sxs-lookup"><span data-stu-id="b999b-121">For example, run the following command to display a cached list of popular images in table format:</span></span>

```azurecli
az vm image list --output table
```

<span data-ttu-id="b999b-122">Výstup obsahuje název URN (hodnota v *Urn* sloupec), který se používá k určení bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="b999b-122">The output includes the URN (the value in the *Urn* column), which you use to specify the image.</span></span> <span data-ttu-id="b999b-123">Při vytváření virtuálního počítače s jedním z těchto oblíbených bitových kopií Marketplace, můžete alternativně zadat název URN alias, například *UbuntuLTS*.</span><span class="sxs-lookup"><span data-stu-id="b999b-123">When creating a VM with one of these popular Marketplace images, you can alternatively specify the URN alias, such as *UbuntuLTS*.</span></span>

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
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

## <a name="find-specific-images"></a><span data-ttu-id="b999b-124">Vyhledání konkrétních imagí</span><span class="sxs-lookup"><span data-stu-id="b999b-124">Find specific images</span></span>

<span data-ttu-id="b999b-125">Chcete-li vyhledat konkrétní image virtuálního počítače na Marketplace, použijte `az vm image list` s `--all` možnost.</span><span class="sxs-lookup"><span data-stu-id="b999b-125">To find a specific VM image in the Marketplace, use the `az vm image list` command with the `--all` option.</span></span> <span data-ttu-id="b999b-126">Tato verze příkazu trvá delší dobu a lze vrátit zdlouhavé výstup, takže obvykle filtrovat seznam podle `--publisher` nebo jiný parametr.</span><span class="sxs-lookup"><span data-stu-id="b999b-126">This version of the command takes some time to complete and can return lengthy output, so you usually filter the list by `--publisher` or another parameter.</span></span> 

<span data-ttu-id="b999b-127">Například následující příkaz zobrazí všechny Debian nabídky (Nezapomeňte, že bez `--all` přepnout, prochází pouze místní mezipaměti běžné bitových kopií):</span><span class="sxs-lookup"><span data-stu-id="b999b-127">For example, the following command displays all Debian offers (remember that without the `--all` switch, it only searches the local cache of common images):</span></span>

```azurecli
az vm image list --offer Debian --all --output table 

```

<span data-ttu-id="b999b-128">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="b999b-128">Partial output:</span></span> 
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

<span data-ttu-id="b999b-129">Použití filtrů podobné s `--location`, `--publisher`, a `--sku` možnosti.</span><span class="sxs-lookup"><span data-stu-id="b999b-129">Apply similar filters with the `--location`, `--publisher`, and `--sku` options.</span></span> <span data-ttu-id="b999b-130">Termínem shodují jen částečně můžete provést i na filtrem, třeba hledání `--offer Deb` najít všechny Debian Image.</span><span class="sxs-lookup"><span data-stu-id="b999b-130">You can even perform partial matches on a filter, such as searching for `--offer Deb` to find all Debian images.</span></span>

<span data-ttu-id="b999b-131">Pokud nezadáte konkrétního umístění s `--location` možnost hodnoty `westus` jsou vráceny ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b999b-131">If you don't specify a particular location with the `--location` option, the values for `westus` are returned by default.</span></span> <span data-ttu-id="b999b-132">(Spuštěním nastavte jiné výchozí umístění `az configure --defaults location=<location>`.)</span><span class="sxs-lookup"><span data-stu-id="b999b-132">(Set a different default location by running `az configure --defaults location=<location>`.)</span></span>

<span data-ttu-id="b999b-133">Například následující příkaz zobrazí všechny Debian SKU 8 v `westeurope`:</span><span class="sxs-lookup"><span data-stu-id="b999b-133">For example, the following command lists all Debian 8 SKUs in `westeurope`:</span></span>

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

<span data-ttu-id="b999b-134">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="b999b-134">Partial output:</span></span>

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

## <a name="navigate-the-images"></a><span data-ttu-id="b999b-135">Přejděte bitové kopie</span><span class="sxs-lookup"><span data-stu-id="b999b-135">Navigate the images</span></span> 
<span data-ttu-id="b999b-136">Jiný způsob, jak najít bitovou kopii v umístění je spuštění [bitové kopie virtuálních počítačů az seznamu vydavatelů](/cli/azure/vm/image#list-publishers), [bitové kopie virtuálních počítačů az seznamu nabízí](/cli/azure/vm/image#list-offers), a [bitové kopie virtuálních počítačů az seznamu SKU](/cli/azure/vm/image#list-skus) příkazy v pořadí.</span><span class="sxs-lookup"><span data-stu-id="b999b-136">Another way to find an image in a location is to run the [az vm image list-publishers](/cli/azure/vm/image#list-publishers), [az vm image list-offers](/cli/azure/vm/image#list-offers), and [az vm image list-skus](/cli/azure/vm/image#list-skus) commands in sequence.</span></span> <span data-ttu-id="b999b-137">Pomocí těchto příkazů určit tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b999b-137">With these commands, you determine these values:</span></span>

1. <span data-ttu-id="b999b-138">Vypsat vydavatele imagí.</span><span class="sxs-lookup"><span data-stu-id="b999b-138">List the image publishers.</span></span>
2. <span data-ttu-id="b999b-139">Pro daného vydavatele vypsat jeho nabídky.</span><span class="sxs-lookup"><span data-stu-id="b999b-139">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="b999b-140">Pro danou nabídku vypsat její skladovou jednotku (SKU).</span><span class="sxs-lookup"><span data-stu-id="b999b-140">For a given offer, list their SKUs.</span></span>


<span data-ttu-id="b999b-141">Například následující příkaz seznam vydavatelů bitové kopie v umístění západní USA:</span><span class="sxs-lookup"><span data-stu-id="b999b-141">For example, the following command lists the image publishers in the West US location:</span></span>

```azurecli
az vm image list-publishers --location westus --output table
```

<span data-ttu-id="b999b-142">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="b999b-142">Partial output:</span></span>

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
<span data-ttu-id="b999b-143">Tyto informace použijte k vyhledání nabídky od konkrétní vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b999b-143">Use this information to find offers from a specific publisher.</span></span> <span data-ttu-id="b999b-144">Například pokud Canonical vydavatele bitové kopie v umístění západní USA, najít jejich nabídky spuštěním `azure vm image list-offers`.</span><span class="sxs-lookup"><span data-stu-id="b999b-144">For example, if Canonical is an image publisher in the West US location, find their offers by running `azure vm image list-offers`.</span></span> <span data-ttu-id="b999b-145">Předejte umístění a vydavatele jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b999b-145">Pass the location and the publisher as in the following example:</span></span>

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

<span data-ttu-id="b999b-146">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b999b-146">Output:</span></span>

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
<span data-ttu-id="b999b-147">Uvidíte, že v oblasti západní USA Canonical publikuje **UbuntuServer** nabídku Azure.</span><span class="sxs-lookup"><span data-stu-id="b999b-147">You see that in the West US region, Canonical publishes the **UbuntuServer** offer on Azure.</span></span> <span data-ttu-id="b999b-148">Ale co skladové jednotky (SKU)?</span><span class="sxs-lookup"><span data-stu-id="b999b-148">But what SKUs?</span></span> <span data-ttu-id="b999b-149">Chcete-li získat tyto hodnoty, spusťte `azure vm image list-skus` a nastavte umístění, vydavatele a nabídky, které byly zjištěny:</span><span class="sxs-lookup"><span data-stu-id="b999b-149">To get those values, run `azure vm image list-skus` and set the location, publisher, and offer that you have discovered:</span></span>

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

<span data-ttu-id="b999b-150">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b999b-150">Output:</span></span>

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

<span data-ttu-id="b999b-151">Nakonec použijte `az vm image list` příkazu najděte na konkrétní verzi verze SKU chcete například **16.04 LTS**:</span><span class="sxs-lookup"><span data-stu-id="b999b-151">Finally, use the `az vm image list` command to find a specific version of the SKU you want, for example, **16.04-LTS**:</span></span>

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

<span data-ttu-id="b999b-152">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b999b-152">Output:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="b999b-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b999b-153">Next steps</span></span>
<span data-ttu-id="b999b-154">Nyní můžete přesněji bitovou kopii, kterou chcete použít provedením Poznámka URN hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b999b-154">Now you can choose precisely the image you want to use by taking note of the URN value.</span></span> <span data-ttu-id="b999b-155">Předat tuto hodnotu s `--image` parametr při vytváření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b999b-155">Pass this value with the `--image` parameter when you create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="b999b-156">Mějte na paměti, že můžete volitelně nahradit číslo verze v název URN "poslední zálohy".</span><span class="sxs-lookup"><span data-stu-id="b999b-156">Remember that you can optionally replace the version number in the URN with "latest".</span></span> <span data-ttu-id="b999b-157">Tato verze je vždy nejnovější verzi rozdělení.</span><span class="sxs-lookup"><span data-stu-id="b999b-157">This version is always the latest version of the distribution.</span></span> <span data-ttu-id="b999b-158">Pokud chcete rychle vytvořit virtuální počítač pomocí informací o URN, přečtěte si téma [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure](tutorial-manage-vm.md).</span><span class="sxs-lookup"><span data-stu-id="b999b-158">To create a virtual machine quickly by using the URN information, see [Create and Manage Linux VMs with the Azure CLI](tutorial-manage-vm.md).</span></span>
