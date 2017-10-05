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
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a>Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače s Linuxem

Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, nasazení skutečné aplikace, které musí vzít v úvahu. Nasazení aplikace v tomto poli odkazuje na instalaci binárních souborů skutečné aplikace do prostředků Azure. Pro ukázku Hudba úložiště, rozhraní .net jádro, NGINX a Supervisor musí být nainstalovaná a nakonfigurovaná na každém virtuálním počítači. Binární soubory Hudba úložiště je potřeba nainstalovat na virtuální počítač a předem vytvořené databáze hudba úložiště.

Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat nasazení aplikace a konfigurace virtuálních počítačů Azure. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru. Úplnou šablonu naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurační skript
Rozšíření virtuálního počítače jsou specializované programy, které provést před virtuálními počítači poskytnout automatizace konfigurace. Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace. Rozšíření vlastních skriptů slouží ke spouštění všech skriptů na virtuálním počítači. Pomocí ukázky hudby úložiště je rozšíření vlastních skriptů pro konfiguraci Ubuntu virtuální počítače a instalaci aplikace Hudba úložiště. 

Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte skript, který je spuštěn. Tento skript nakonfiguruje Ubuntu virtuální počítač pro hostování aplikace Hudba úložiště. Při spuštění skriptu nainstaluje všechny, potřebný software, nainstalujte aplikaci store hudba od správy zdrojového kódu a Příprava databáze. 

Další informace o hostování .net základní aplikace v systému Linux najdete v tématu [publikovat do provozního prostředí Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html).

> Tato ukázka je pro demonstrační účely.
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

## <a name="vm-script-extension"></a>Skript rozšíření virtuálního počítače
Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu, přiložením rozšíření prostředků do šablony Azure Resource Manageru. Rozšíření můžete přidat pomocí Průvodce přidáním prostředku Visual Studio, nebo vložením platný kód JSON do šablony. Skript rozšíření prostředků je vnořit prostředek virtuálního počítače; To můžete vidět v následujícím příkladu.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Všimněte si v níže JSON, aby tento skript je uložený na webu GitHub. Tento skript by také může být uložený v úložišti objektů Blob Azure. Šablony Azure Resource Manager umožnit řetězec provádění skriptu, který má vytvořený tak, aby hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu. V takovém případě se data zadaná při nasazování šablony a tyto hodnoty lze při provádění skriptu.

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

Další informace o použití rozšíření vlastních skriptů, najdete v části [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-step"></a>Další krok
<hr>

[Prozkoumejte šablony více Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates)

