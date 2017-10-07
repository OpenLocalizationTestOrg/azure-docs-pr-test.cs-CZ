---
title: "aaaUse hello rozšíření CustomScript na virtuální počítač s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toouse hello CustomScript rozšíření toodeploy aplikace na virtuální počítače s Linuxem v Azure vytvořit pomocí modelu nasazení classic hello."
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: 864a586e70093eefbabc065a3c05e1cf9e315704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-lamp-app-using-hello-azure-customscript-extension-for-linux"></a><span data-ttu-id="dded4-103">Nasazení aplikace svítilna pomocí hello rozšíření CustomScript Azure pro Linux</span><span class="sxs-lookup"><span data-stu-id="dded4-103">Deploy a LAMP app using hello Azure CustomScript Extension for Linux</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="dded4-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="dded4-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="dded4-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="dded4-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="dded4-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="dded4-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="dded4-107">Informace o nasazení svítilna zásobníku pomocí modelu Resource Manager hello najdete v tématu [zde](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dded4-107">For information about deploying a LAMP stack using hello Resource Manager model, see [here](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="dded4-108">Hello rozšíření CustomScript Microsoft Azure pro Linux poskytuje způsob toocustomize virtuálních počítačů (VM) spuštěním libovolný kód napsaný v jakékoli skriptovací jazyk podporuje hello virtuálního počítače (například Python a Bash).</span><span class="sxs-lookup"><span data-stu-id="dded4-108">hello Microsoft Azure CustomScript Extension for Linux provides a way toocustomize your virtual machines (VMs) by running arbitrary code written in any scripting language supported by hello VM (for example, Python, and Bash).</span></span> <span data-ttu-id="dded4-109">To poskytuje flexibilní způsob tooautomate počítače toomultiple nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="dded4-109">This provides a very flexible way tooautomate application deployment toomultiple machines.</span></span>

<span data-ttu-id="dded4-110">Můžete nasadit hello rozšíření CustomScript pomocí portálu Azure, prostředí Windows PowerShell nebo rozhraní příkazového řádku Azure (Azure CLI) hello hello.</span><span class="sxs-lookup"><span data-stu-id="dded4-110">You can deploy hello CustomScript Extension using hello Azure portal, Windows PowerShell, or hello Azure Command-Line Interface (Azure CLI).</span></span>

<span data-ttu-id="dded4-111">V tomto článku, který použijeme hello rozhraní příkazového řádku Azure toodeploy jednoduché aplikace tooan svítilna virtuálního počítače s Ubuntu vytvořit pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="dded4-111">In this article we'll use hello Azure CLI toodeploy a simple LAMP application tooan Ubuntu VM created using hello classic deployment model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dded4-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dded4-112">Prerequisites</span></span>
<span data-ttu-id="dded4-113">V tomto příkladu nejdřív vytvořte dva virtuální počítače Azure s Ubuntu 14.04 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="dded4-113">For this example, first create two Azure VMs running Ubuntu 14.04 or later.</span></span> <span data-ttu-id="dded4-114">Hello virtuální počítače se označují jako *skriptu vm* a *svítilna vm*.</span><span class="sxs-lookup"><span data-stu-id="dded4-114">hello VMs are called *script-vm* and *lamp-vm*.</span></span> <span data-ttu-id="dded4-115">Při vytváření virtuálních počítačů hello, použijte jedinečné názvy.</span><span class="sxs-lookup"><span data-stu-id="dded4-115">Use unique names when you create hello VMs.</span></span> <span data-ttu-id="dded4-116">Jeden je použité toorun hello rozhraní příkazového řádku a jedna je použité toodeploy hello svítilna aplikace.</span><span class="sxs-lookup"><span data-stu-id="dded4-116">One is used toorun hello CLI commands and one is used toodeploy hello LAMP app.</span></span>

<span data-ttu-id="dded4-117">Budete také potřebovat účtu Azure Storage a klíče tooaccess it (můžete získat to hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="dded4-117">You also need an Azure Storage account and a key tooaccess it (you can get this from hello Azure portal).</span></span>

<span data-ttu-id="dded4-118">Pokud potřebujete pomoc, vytváření virtuální počítače s Linuxem v Azure odkazovat příliš[vytvořit virtuálním počítači systémem Linux](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="dded4-118">If you need help creating Linux VMs on Azure refer too[Create a Virtual Machine Running Linux](createportal.md).</span></span>

<span data-ttu-id="dded4-119">Hello instalace příkazů se předpokládá, Ubuntu, ale hello instalace však můžete upravit pro všechny podporované Linux distro.</span><span class="sxs-lookup"><span data-stu-id="dded4-119">hello install commands assume Ubuntu, but you can adapt hello installation for any supported Linux distro.</span></span>

<span data-ttu-id="dded4-120">Hello skriptu vm virtuálních počítačů musí toohave nainstalované rozhraní příkazového řádku Azure s tooAzure připojení práci.</span><span class="sxs-lookup"><span data-stu-id="dded4-120">hello script-vm VM needs toohave Azure CLI installed, with a working connection tooAzure.</span></span> <span data-ttu-id="dded4-121">Pomoc s to naleznete příliš[instalace a konfigurace rozhraní příkazového řádku Azure hello](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="dded4-121">For help with this refer too[Install and Configure hello Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span>

## <a name="upload-a-script"></a><span data-ttu-id="dded4-122">Odeslat skript</span><span class="sxs-lookup"><span data-stu-id="dded4-122">Upload a script</span></span>
<span data-ttu-id="dded4-123">Jsme budete používat hello rozšíření CustomScript toorun skript v zásobníku hello svítilna sady vzdálený počítač tooinstall a vytvoření stránky PHP.</span><span class="sxs-lookup"><span data-stu-id="dded4-123">We'll use hello CustomScript Extension toorun a script on a remote VM tooinstall hello LAMP stack and create a PHP page.</span></span> <span data-ttu-id="dded4-124">Ve skriptu hello tooaccess pořadí odkudkoli jsme budete ho nahrát jako Azure blob.</span><span class="sxs-lookup"><span data-stu-id="dded4-124">In order tooaccess hello script from anywhere we'll upload it as an Azure blob.</span></span>

### <a name="script-overview"></a><span data-ttu-id="dded4-125">Přehled skriptů</span><span class="sxs-lookup"><span data-stu-id="dded4-125">Script overview</span></span>
<span data-ttu-id="dded4-126">Příklad skriptu Hello nainstaluje svítilna tooUbuntu zásobníku (včetně nastavení bezobslužné instalace MySQL), zapíše jednoduchého souboru PHP a spustí Apache.</span><span class="sxs-lookup"><span data-stu-id="dded4-126">hello script example installs a LAMP stack tooUbuntu (including setting up a silent install of MySQL), writes a simple PHP file, and starts Apache.</span></span>

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install hello LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a><span data-ttu-id="dded4-127">Nahrát skriptu</span><span class="sxs-lookup"><span data-stu-id="dded4-127">Upload script</span></span>
<span data-ttu-id="dded4-128">Uložte skript hello jako textový soubor, například *install_lamp.sh*a odešlete ji tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="dded4-128">Save hello script as a text file, for example *install_lamp.sh*, and then upload it tooAzure Storage.</span></span> <span data-ttu-id="dded4-129">Můžete provést snadno pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dded4-129">You can do this easily with Azure CLI.</span></span> <span data-ttu-id="dded4-130">Hello následujícím příkladu se uloží hello souboru tooa kontejner úložiště s názvem "skripty".</span><span class="sxs-lookup"><span data-stu-id="dded4-130">hello following example uploads hello file tooa storage container named "scripts".</span></span> <span data-ttu-id="dded4-131">Pokud hello kontejner neexistuje, budete potřebovat toocreate ho první.</span><span class="sxs-lookup"><span data-stu-id="dded4-131">If hello container doesn't exist you'll need toocreate it first.</span></span>

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

<span data-ttu-id="dded4-132">Také vytvořte soubor JSON, který popisuje, jak toodownload hello skript z Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dded4-132">Also create a JSON file that describes how toodownload hello script from Azure Storage.</span></span> <span data-ttu-id="dded4-133">Uložit jako *public_config.json* (nahrazení "mystorage" hello název účtu úložiště):</span><span class="sxs-lookup"><span data-stu-id="dded4-133">Save this as *public_config.json* (replacing "mystorage" with hello name of your storage account):</span></span>

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-hello-extension"></a><span data-ttu-id="dded4-134">Nasazení rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="dded4-134">Deploy hello extension</span></span>
<span data-ttu-id="dded4-135">Nyní můžete pomocí hello další příkaz toodeploy hello rozšíření CustomScript Linux toohello vzdálené hello virtuální počítač pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dded4-135">Now you can use hello next command toodeploy hello Linux CustomScript Extension toohello remote VM using hello Azure CLI.</span></span>

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

<span data-ttu-id="dded4-136">předchozí příkaz Hello stahuje a spouští hello *install_lamp.sh* skriptu na hello názvem virtuálního počítače *svítilna vm*.</span><span class="sxs-lookup"><span data-stu-id="dded4-136">hello previous command downloads and runs hello *install_lamp.sh* script on hello VM called *lamp-vm*.</span></span>

<span data-ttu-id="dded4-137">Vzhledem k tomu, že aplikace hello zahrnuje webový server, mějte na paměti, port naslouchání tooopen protokolu HTTP na hello vzdálené virtuální počítač s další příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="dded4-137">Since hello app includes a web server, remember tooopen an HTTP listening port on hello remote VM with hello next command.</span></span>

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="dded4-138">Monitorování a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="dded4-138">Monitoring and troubleshooting</span></span>
<span data-ttu-id="dded4-139">Můžete se podívat na tom, jak dobře hello hello vlastní skript se spustí prohlédnutím souboru protokolu hello vzdálený počítač.</span><span class="sxs-lookup"><span data-stu-id="dded4-139">You can check on how well hello custom script runs by looking at hello log file on hello remote VM.</span></span> <span data-ttu-id="dded4-140">SSH příliš*svítilna vm* a soubor protokolu poškozené databáze hello další příkazem hello.</span><span class="sxs-lookup"><span data-stu-id="dded4-140">SSH too*lamp-vm* and tail hello log file with hello next command.</span></span>

    cd /var/log/azure/customscript
    tail -f handler.log

<span data-ttu-id="dded4-141">Po spuštění hello rozšíření CustomScript můžete procházet toohello PHP stránku, kterou jste vytvořili pro informace.</span><span class="sxs-lookup"><span data-stu-id="dded4-141">After you run hello CustomScript Extension, you can browse toohello PHP page you created for information.</span></span> <span data-ttu-id="dded4-142">Hello PHP stránku hello příklad v tomto článku je *http://lamp-vm.cloudapp.net/phpinfo.php*.</span><span class="sxs-lookup"><span data-stu-id="dded4-142">hello PHP page for hello example in this article is *http://lamp-vm.cloudapp.net/phpinfo.php*.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dded4-143">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dded4-143">Additional resources</span></span>
<span data-ttu-id="dded4-144">Můžete použít stejné základní kroky toodeploy hello složitější aplikace.</span><span class="sxs-lookup"><span data-stu-id="dded4-144">You can use hello same basic steps toodeploy more complex apps.</span></span> <span data-ttu-id="dded4-145">V tomto příkladu byla uložena hello instalační skript jako veřejného objektu blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dded4-145">In this example hello install script was saved as a public blob in Azure Storage.</span></span> <span data-ttu-id="dded4-146">Bezpečnější možnost by jako zabezpečené blob s toostore hello instalační skript [zabezpečený přístup k podpisu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span><span class="sxs-lookup"><span data-stu-id="dded4-146">A more secure option would be toostore hello install script as a secure blob with a [Secure Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span></span>

<span data-ttu-id="dded4-147">Další zdroje pro rozhraní příkazového řádku Azure, Linux a hello rozšíření CustomScript jsou uvedeny dále.</span><span class="sxs-lookup"><span data-stu-id="dded4-147">Additional resources for Azure CLI, Linux and hello CustomScript Extension are listed next.</span></span>

[<span data-ttu-id="dded4-148">Automatizovat úkoly vlastního nastavení virtuálního počítače s Linuxem pomocí rozšíření CustomScript</span><span class="sxs-lookup"><span data-stu-id="dded4-148">Automate Linux VM Customization Tasks Using CustomScript Extension</span></span>](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[<span data-ttu-id="dded4-149">Rozšíření Azure Linux (Githubu)</span><span class="sxs-lookup"><span data-stu-id="dded4-149">Azure Linux Extensions (GitHub)</span></span>](https://github.com/Azure/azure-linux-extensions)