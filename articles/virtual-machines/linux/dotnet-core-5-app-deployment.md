---
title: "Nasazení aplikací pomocí rozšíření virtuálního počítače aaaAutomating | Microsoft Docs"
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
ms.openlocfilehash: 38a02a4271d6b9ba02a473a51794a7bd90ca3a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a>Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače s Linuxem

Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, musí nasazení skutečné aplikace hello toobe řešit. Nasazení aplikace zde odkazuje tooinstalling hello skutečné aplikačních binárních souborů do prostředků Azure. Pro ukázku hello Hudba úložiště, rozhraní .net Core, NGINX a Supervisor potřebovat toobe nainstalovaná a nakonfigurovaná na každém virtuálním počítači. Hello Hudba úložiště binární soubory nutné toobe nainstaluje do hello virtuálního počítače a hello Hudba úložiště databáze předem vytvořené.

Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat aplikace nasazení a konfigurace tooAzure virtuálních počítačů. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru. úplnou šablonu Hello naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurační skript
Rozšíření virtuálního počítače jsou specializované programy, které spuštění u virtuálních počítačů tooprovide konfigurace automatizace. Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace. Rozšíření vlastních skriptů lze použít toorun žádný skript u virtuálního počítače. Pomocí hello Ukázky hudby úložiště je toohello rozšíření vlastních skriptů tooconfigure hello Ubuntu virtuální počítače a nainstalovat aplikaci Store Hudba hello. 

Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte hello skript, který je spuštěn. Tento skript nakonfiguruje hello Ubuntu virtuální počítač toohost hello aplikace Hudba úložiště. Při spuštění skriptu hello nainstaluje všechny potřebné software, nainstalujte aplikaci store Hudba hello od správy zdrojového kódu a příprava hello databáze. 

toolearn Další informace o hostování .net základní aplikace pro systémy Linux, najdete v části [publikovat tooa Linux provozním prostředí](https://docs.asp.net/en/latest/publishing/linuxproduction.html).

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
Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu včetně hello rozšíření prostředků v hello šablony Azure Resource Manageru. rozšíření Hello lze přidat pomocí Průvodce přidáním prostředku Visual Studio hello nebo vložením platný kód JSON do šablony hello. Hello skriptu rozšíření prostředků je vnořit hello prostředek virtuálního počítače; To si můžete prohlédnout ve hello následující ukázka.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Všimněte si v hello níže formátu JSON, který hello skript je uložený na webu GitHub. Tento skript by také může být uložený v úložišti objektů Blob Azure. Šablony Azure Resource Manager povolit dále tooconstructed řetězec provádění skriptu hello tak, že hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu. V takovém případě se data poskytuje při nasazování šablony hello a tyto hodnoty lze při provádění skriptu hello.

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

Další informace o použití rozšíření vlastních skriptů hello najdete v tématu [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-step"></a>Další krok
<hr>

[Prozkoumejte šablony více Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates)

