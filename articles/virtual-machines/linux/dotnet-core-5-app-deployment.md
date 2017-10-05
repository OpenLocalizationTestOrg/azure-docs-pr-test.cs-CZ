---
title: "Automatizace nasazení aplikací pomocí rozšíření virtuálního počítače | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 9fc8b1ba-60f5-410b-8190-9f1ff885e50e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f972fef75aa8e13af7dab908c2b0e2ec28f1324
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="cdf46-103">Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="cdf46-103">Application deployment with Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="cdf46-104">Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, nasazení skutečné aplikace, které musí vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="cdf46-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, the actual application deployment needs to be addressed.</span></span> <span data-ttu-id="cdf46-105">Nasazení aplikace v tomto poli odkazuje na instalaci binárních souborů skutečné aplikace do prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cdf46-105">Application deployment here is referring to installing the actual application binaries onto Azure resources.</span></span> <span data-ttu-id="cdf46-106">Pro ukázku Hudba úložiště, rozhraní .net jádro, NGINX a Supervisor musí být nainstalovaná a nakonfigurovaná na každém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="cdf46-106">For the Music Store sample, .Net Core, NGINX, and Supervisor need to be installed and configured on each virtual machine.</span></span> <span data-ttu-id="cdf46-107">Binární soubory Hudba úložiště je potřeba nainstalovat na virtuální počítač a předem vytvořené databáze hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="cdf46-107">The Music Store binaries need to be installed onto the virtual machine, and the Music Store database pre-created.</span></span>

<span data-ttu-id="cdf46-108">Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat nasazení aplikace a konfigurace virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="cdf46-108">This document details how Virtual Machine extensions can automate application deployment and configuration to Azure virtual machines.</span></span> <span data-ttu-id="cdf46-109">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cdf46-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="cdf46-110">Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cdf46-110">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="cdf46-111">Úplnou šablonu naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="cdf46-111">The complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="cdf46-112">Konfigurační skript</span><span class="sxs-lookup"><span data-stu-id="cdf46-112">Configuration script</span></span>
<span data-ttu-id="cdf46-113">Rozšíření virtuálního počítače jsou specializované programy, které provést před virtuálními počítači poskytnout automatizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cdf46-113">Virtual Machine extensions are specialized programs that execute against virtual machines to provide configuration automation.</span></span> <span data-ttu-id="cdf46-114">Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cdf46-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="cdf46-115">Rozšíření vlastních skriptů slouží ke spouštění všech skriptů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="cdf46-115">A custom script extension can be used to run any script against a virtual machine.</span></span> <span data-ttu-id="cdf46-116">Pomocí ukázky hudby úložiště je rozšíření vlastních skriptů pro konfiguraci Ubuntu virtuální počítače a instalaci aplikace Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="cdf46-116">With the Music Store sample, it is up to the custom script extension to configure the Ubuntu virtual machines and install the Music Store application.</span></span> 

<span data-ttu-id="cdf46-117">Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte skript, který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="cdf46-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine the script that is run.</span></span> <span data-ttu-id="cdf46-118">Tento skript nakonfiguruje Ubuntu virtuální počítač pro hostování aplikace Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="cdf46-118">This script configures the Ubuntu virtual machine to host the Music Store application.</span></span> <span data-ttu-id="cdf46-119">Při spuštění skriptu nainstaluje všechny, potřebný software, nainstalujte aplikaci store hudba od správy zdrojového kódu a Příprava databáze.</span><span class="sxs-lookup"><span data-stu-id="cdf46-119">When run, the script installs all needed software, install the Music store application from source control, and prepare the database.</span></span> 

<span data-ttu-id="cdf46-120">Další informace o hostování .net základní aplikace v systému Linux najdete v tématu [publikovat do provozního prostředí Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span><span class="sxs-lookup"><span data-stu-id="cdf46-120">To learn more about hosting a .Net Core application on Linux, see [Publish to a Linux production environment](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span></span>

> <span data-ttu-id="cdf46-121">Tato ukázka je pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="cdf46-121">This sample is for demonstration purposes.</span></span>
> 
> 

```bash
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a><span data-ttu-id="cdf46-122">Skript rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cdf46-122">VM Script Extension</span></span>
<span data-ttu-id="cdf46-123">Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu, přiložením rozšíření prostředků do šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cdf46-123">VM Extensions can be run against a virtual machine at build time by including the extension resource in the Azure Resource Manager template.</span></span> <span data-ttu-id="cdf46-124">Rozšíření můžete přidat pomocí Průvodce přidáním prostředku Visual Studio, nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="cdf46-124">The extension can be added with the Visual Studio Add Resource wizard, or by inserting valid JSON into the template.</span></span> <span data-ttu-id="cdf46-125">Skript rozšíření prostředků je vnořit prostředek virtuálního počítače; To můžete vidět v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="cdf46-125">The Script Extension resource is nested inside the Virtual Machine resource; this can be seen in the following example.</span></span>

<span data-ttu-id="cdf46-126">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span><span class="sxs-lookup"><span data-stu-id="cdf46-126">Follow this link to see the JSON sample within the Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span></span> 

<span data-ttu-id="cdf46-127">Všimněte si v níže JSON, aby tento skript je uložený na webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="cdf46-127">Notice in the below JSON that the script is stored in GitHub.</span></span> <span data-ttu-id="cdf46-128">Tento skript by také může být uložený v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="cdf46-128">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="cdf46-129">Šablony Azure Resource Manager umožnit řetězec provádění skriptu, který má vytvořený tak, aby hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="cdf46-129">Also, Azure Resource Manager templates allow the script execution string to constructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="cdf46-130">V takovém případě se data zadaná při nasazování šablony a tyto hodnoty lze při provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="cdf46-130">In this case data is provided when deploying the templates, and these values can then be used when executing the script.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

<span data-ttu-id="cdf46-131">Další informace o použití rozšíření vlastních skriptů, najdete v části [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cdf46-131">For more information on using the custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="cdf46-132">Další krok</span><span class="sxs-lookup"><span data-stu-id="cdf46-132">Next Step</span></span>
<hr>

[<span data-ttu-id="cdf46-133">Prozkoumejte šablony více Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="cdf46-133">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

