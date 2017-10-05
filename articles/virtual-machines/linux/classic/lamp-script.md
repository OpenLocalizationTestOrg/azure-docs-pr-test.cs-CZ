---
title: "Pomocí rozšíření CustomScript na virtuální počítač s Linuxem | Microsoft Docs"
description: "Další informace o použití rozšíření CustomScript k nasazení aplikací na virtuální počítače s Linuxem v Azure vytvořit pomocí modelu nasazení classic."
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
ms.openlocfilehash: cb1fc9a44dc9e57d9cc9f1c546ad937d67e63c2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a><span data-ttu-id="7c396-103">Nasazení aplikace LAMP pomocí rozšíření Azure CustomScript pro Linux</span><span class="sxs-lookup"><span data-stu-id="7c396-103">Deploy a LAMP app using the Azure CustomScript Extension for Linux</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="7c396-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7c396-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7c396-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="7c396-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="7c396-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7c396-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="7c396-107">Informace o nasazení svítilna zásobníku pomocí modelu Resource Manager najdete v tématu [zde](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7c396-107">For information about deploying a LAMP stack using the Resource Manager model, see [here](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="7c396-108">Rozšíření CustomScript Microsoft Azure pro Linux poskytuje způsob, jak přizpůsobit virtuálních počítačů (VM) spuštěním libovolný kód napsaný v jakékoli skriptovací jazyk, který nepodporuje virtuální počítač (například Python a Bash).</span><span class="sxs-lookup"><span data-stu-id="7c396-108">The Microsoft Azure CustomScript Extension for Linux provides a way to customize your virtual machines (VMs) by running arbitrary code written in any scripting language supported by the VM (for example, Python, and Bash).</span></span> <span data-ttu-id="7c396-109">To poskytuje flexibilní způsob k automatizaci nasazení aplikace do více počítačů.</span><span class="sxs-lookup"><span data-stu-id="7c396-109">This provides a very flexible way to automate application deployment to multiple machines.</span></span>

<span data-ttu-id="7c396-110">Můžete nasadit rozšíření CustomScript pomocí portálu Azure, prostředí Windows PowerShell nebo rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="7c396-110">You can deploy the CustomScript Extension using the Azure portal, Windows PowerShell, or the Azure Command-Line Interface (Azure CLI).</span></span>

<span data-ttu-id="7c396-111">V tomto článku, který použijeme rozhraní příkazového řádku Azure k nasazení jednoduché aplikace svítilna do virtuálního počítače s Ubuntu vytvořit pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="7c396-111">In this article we'll use the Azure CLI to deploy a simple LAMP application to an Ubuntu VM created using the classic deployment model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c396-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c396-112">Prerequisites</span></span>
<span data-ttu-id="7c396-113">V tomto příkladu nejdřív vytvořte dva virtuální počítače Azure s Ubuntu 14.04 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="7c396-113">For this example, first create two Azure VMs running Ubuntu 14.04 or later.</span></span> <span data-ttu-id="7c396-114">Virtuální počítače se označují jako *skriptu vm* a *svítilna vm*.</span><span class="sxs-lookup"><span data-stu-id="7c396-114">The VMs are called *script-vm* and *lamp-vm*.</span></span> <span data-ttu-id="7c396-115">Při vytváření virtuálních počítačů, použijte jedinečné názvy.</span><span class="sxs-lookup"><span data-stu-id="7c396-115">Use unique names when you create the VMs.</span></span> <span data-ttu-id="7c396-116">Jeden slouží ke spuštění příkazů rozhraní příkazového řádku a jeden slouží k nasazení aplikace svítilna.</span><span class="sxs-lookup"><span data-stu-id="7c396-116">One is used to run the CLI commands and one is used to deploy the LAMP app.</span></span>

<span data-ttu-id="7c396-117">Musíte taky účtu Azure Storage a klíč pro přístup k ní (získáte to z portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="7c396-117">You also need an Azure Storage account and a key to access it (you can get this from the Azure portal).</span></span>

<span data-ttu-id="7c396-118">Pokud potřebujete pomoc, vytváření virtuální počítače s Linuxem v Azure naleznete [vytvořit virtuálním počítači systémem Linux](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="7c396-118">If you need help creating Linux VMs on Azure refer to [Create a Virtual Machine Running Linux](createportal.md).</span></span>

<span data-ttu-id="7c396-119">Instalace příkazů se předpokládá, Ubuntu, ale můžete přizpůsobit instalace pro všechny podporované distro Linux.</span><span class="sxs-lookup"><span data-stu-id="7c396-119">The install commands assume Ubuntu, but you can adapt the installation for any supported Linux distro.</span></span>

<span data-ttu-id="7c396-120">Virtuální počítač skriptu vm musí mít nainstalovaný, má funkční připojení k Azure rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7c396-120">The script-vm VM needs to have Azure CLI installed, with a working connection to Azure.</span></span> <span data-ttu-id="7c396-121">Pomoc s to naleznete [instalace a konfigurace rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7c396-121">For help with this refer to [Install and Configure the Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span>

## <a name="upload-a-script"></a><span data-ttu-id="7c396-122">Odeslat skript</span><span class="sxs-lookup"><span data-stu-id="7c396-122">Upload a script</span></span>
<span data-ttu-id="7c396-123">Pomocí rozšíření CustomScript spuštění skriptu na virtuálním počítači vzdálené instalace svítilna zásobníku a vytvoření stránky PHP.</span><span class="sxs-lookup"><span data-stu-id="7c396-123">We'll use the CustomScript Extension to run a script on a remote VM to install the LAMP stack and create a PHP page.</span></span> <span data-ttu-id="7c396-124">Chcete-li skript přístup odkudkoli jsme budete ho nahrát jako Azure blob.</span><span class="sxs-lookup"><span data-stu-id="7c396-124">In order to access the script from anywhere we'll upload it as an Azure blob.</span></span>

### <a name="script-overview"></a><span data-ttu-id="7c396-125">Přehled skriptů</span><span class="sxs-lookup"><span data-stu-id="7c396-125">Script overview</span></span>
<span data-ttu-id="7c396-126">Příklad skriptu nainstaluje svítilna zásobníku do Ubuntu (včetně nastavení bezobslužné instalace MySQL), zapíše jednoduchého souboru PHP a spustí Apache.</span><span class="sxs-lookup"><span data-stu-id="7c396-126">The script example installs a LAMP stack to Ubuntu (including setting up a silent install of MySQL), writes a simple PHP file, and starts Apache.</span></span>

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a><span data-ttu-id="7c396-127">Nahrát skriptu</span><span class="sxs-lookup"><span data-stu-id="7c396-127">Upload script</span></span>
<span data-ttu-id="7c396-128">Uložte skript jako textový soubor, například *install_lamp.sh*a odešlete ji do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7c396-128">Save the script as a text file, for example *install_lamp.sh*, and then upload it to Azure Storage.</span></span> <span data-ttu-id="7c396-129">Můžete provést snadno pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7c396-129">You can do this easily with Azure CLI.</span></span> <span data-ttu-id="7c396-130">Následující příklad odešle soubor do kontejneru úložiště s názvem "skripty".</span><span class="sxs-lookup"><span data-stu-id="7c396-130">The following example uploads the file to a storage container named "scripts".</span></span> <span data-ttu-id="7c396-131">Pokud kontejner neexistuje musíte nejprve vytvořit.</span><span class="sxs-lookup"><span data-stu-id="7c396-131">If the container doesn't exist you'll need to create it first.</span></span>

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

<span data-ttu-id="7c396-132">Také vytvořte soubor JSON, který popisuje, jak se stáhnout skript z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7c396-132">Also create a JSON file that describes how to download the script from Azure Storage.</span></span> <span data-ttu-id="7c396-133">Uložit jako *public_config.json* (nahrazení "mystorage" s název účtu úložiště):</span><span class="sxs-lookup"><span data-stu-id="7c396-133">Save this as *public_config.json* (replacing "mystorage" with the name of your storage account):</span></span>

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a><span data-ttu-id="7c396-134">Nasazení rozšíření</span><span class="sxs-lookup"><span data-stu-id="7c396-134">Deploy the extension</span></span>
<span data-ttu-id="7c396-135">Teď můžete použít příkaz Další k nasazení rozšíření CustomScript Linux na vzdálený počítač pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7c396-135">Now you can use the next command to deploy the Linux CustomScript Extension to the remote VM using the Azure CLI.</span></span>

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

<span data-ttu-id="7c396-136">Předchozí příkaz stáhne a běží *install_lamp.sh* skriptu ve virtuálním počítači názvem *svítilna vm*.</span><span class="sxs-lookup"><span data-stu-id="7c396-136">The previous command downloads and runs the *install_lamp.sh* script on the VM called *lamp-vm*.</span></span>

<span data-ttu-id="7c396-137">Vzhledem k tomu, že aplikace zahrnuje webový server, musíte otevřít port naslouchání v protokolu HTTP na vzdálený počítač následujícím příkazem.</span><span class="sxs-lookup"><span data-stu-id="7c396-137">Since the app includes a web server, remember to open an HTTP listening port on the remote VM with the next command.</span></span>

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="7c396-138">Monitorování a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7c396-138">Monitoring and troubleshooting</span></span>
<span data-ttu-id="7c396-139">Můžete se podívat na tom, jak dobře vlastní skript se spustí prohlédnutím souboru protokolu na vzdálený počítač.</span><span class="sxs-lookup"><span data-stu-id="7c396-139">You can check on how well the custom script runs by looking at the log file on the remote VM.</span></span> <span data-ttu-id="7c396-140">SSH k *svítilna vm* a tail soubor protokolu následujícím příkazem.</span><span class="sxs-lookup"><span data-stu-id="7c396-140">SSH to *lamp-vm* and tail the log file with the next command.</span></span>

    cd /var/log/azure/customscript
    tail -f handler.log

<span data-ttu-id="7c396-141">Po spuštění rozšíření CustomScript můžete procházet na stránku PHP, který jste vytvořili pro informace.</span><span class="sxs-lookup"><span data-stu-id="7c396-141">After you run the CustomScript Extension, you can browse to the PHP page you created for information.</span></span> <span data-ttu-id="7c396-142">Stránka PHP, například v tomto článku je *http://lamp-vm.cloudapp.net/phpinfo.php*.</span><span class="sxs-lookup"><span data-stu-id="7c396-142">The PHP page for the example in this article is *http://lamp-vm.cloudapp.net/phpinfo.php*.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c396-143">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7c396-143">Additional resources</span></span>
<span data-ttu-id="7c396-144">Stejný základní postup můžete použít k nasazení aplikace složitější.</span><span class="sxs-lookup"><span data-stu-id="7c396-144">You can use the same basic steps to deploy more complex apps.</span></span> <span data-ttu-id="7c396-145">V tomto příkladu se instalační skript uložit jako veřejného objektu blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7c396-145">In this example the install script was saved as a public blob in Azure Storage.</span></span> <span data-ttu-id="7c396-146">Bezpečnější možnost může být jako zabezpečené objektů blob s úložiště instalační skript [zabezpečený přístup k podpisu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span><span class="sxs-lookup"><span data-stu-id="7c396-146">A more secure option would be to store the install script as a secure blob with a [Secure Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span></span>

<span data-ttu-id="7c396-147">Další zdroje pro rozhraní příkazového řádku Azure, Linux a rozšíření CustomScript jsou uvedeny dále.</span><span class="sxs-lookup"><span data-stu-id="7c396-147">Additional resources for Azure CLI, Linux and the CustomScript Extension are listed next.</span></span>

[<span data-ttu-id="7c396-148">Automatizovat úkoly vlastního nastavení virtuálního počítače s Linuxem pomocí rozšíření CustomScript</span><span class="sxs-lookup"><span data-stu-id="7c396-148">Automate Linux VM Customization Tasks Using CustomScript Extension</span></span>](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[<span data-ttu-id="7c396-149">Rozšíření Azure Linux (Githubu)</span><span class="sxs-lookup"><span data-stu-id="7c396-149">Azure Linux Extensions (GitHub)</span></span>](https://github.com/Azure/azure-linux-extensions)