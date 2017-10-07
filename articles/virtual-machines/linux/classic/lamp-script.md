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
# <a name="deploy-a-lamp-app-using-hello-azure-customscript-extension-for-linux"></a>Nasazení aplikace svítilna pomocí hello rozšíření CustomScript Azure pro Linux
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o nasazení svítilna zásobníku pomocí modelu Resource Manager hello najdete v tématu [zde](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hello rozšíření CustomScript Microsoft Azure pro Linux poskytuje způsob toocustomize virtuálních počítačů (VM) spuštěním libovolný kód napsaný v jakékoli skriptovací jazyk podporuje hello virtuálního počítače (například Python a Bash). To poskytuje flexibilní způsob tooautomate počítače toomultiple nasazení aplikace.

Můžete nasadit hello rozšíření CustomScript pomocí portálu Azure, prostředí Windows PowerShell nebo rozhraní příkazového řádku Azure (Azure CLI) hello hello.

V tomto článku, který použijeme hello rozhraní příkazového řádku Azure toodeploy jednoduché aplikace tooan svítilna virtuálního počítače s Ubuntu vytvořit pomocí modelu nasazení classic hello.

## <a name="prerequisites"></a>Požadavky
V tomto příkladu nejdřív vytvořte dva virtuální počítače Azure s Ubuntu 14.04 nebo novějším. Hello virtuální počítače se označují jako *skriptu vm* a *svítilna vm*. Při vytváření virtuálních počítačů hello, použijte jedinečné názvy. Jeden je použité toorun hello rozhraní příkazového řádku a jedna je použité toodeploy hello svítilna aplikace.

Budete také potřebovat účtu Azure Storage a klíče tooaccess it (můžete získat to hello portál Azure).

Pokud potřebujete pomoc, vytváření virtuální počítače s Linuxem v Azure odkazovat příliš[vytvořit virtuálním počítači systémem Linux](createportal.md).

Hello instalace příkazů se předpokládá, Ubuntu, ale hello instalace však můžete upravit pro všechny podporované Linux distro.

Hello skriptu vm virtuálních počítačů musí toohave nainstalované rozhraní příkazového řádku Azure s tooAzure připojení práci. Pomoc s to naleznete příliš[instalace a konfigurace rozhraní příkazového řádku Azure hello](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Odeslat skript
Jsme budete používat hello rozšíření CustomScript toorun skript v zásobníku hello svítilna sady vzdálený počítač tooinstall a vytvoření stránky PHP. Ve skriptu hello tooaccess pořadí odkudkoli jsme budete ho nahrát jako Azure blob.

### <a name="script-overview"></a>Přehled skriptů
Příklad skriptu Hello nainstaluje svítilna tooUbuntu zásobníku (včetně nastavení bezobslužné instalace MySQL), zapíše jednoduchého souboru PHP a spustí Apache.

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

### <a name="upload-script"></a>Nahrát skriptu
Uložte skript hello jako textový soubor, například *install_lamp.sh*a odešlete ji tooAzure úložiště. Můžete provést snadno pomocí rozhraní příkazového řádku Azure. Hello následujícím příkladu se uloží hello souboru tooa kontejner úložiště s názvem "skripty". Pokud hello kontejner neexistuje, budete potřebovat toocreate ho první.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Také vytvořte soubor JSON, který popisuje, jak toodownload hello skript z Azure Storage. Uložit jako *public_config.json* (nahrazení "mystorage" hello název účtu úložiště):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-hello-extension"></a>Nasazení rozšíření hello
Nyní můžete pomocí hello další příkaz toodeploy hello rozšíření CustomScript Linux toohello vzdálené hello virtuální počítač pomocí rozhraní příkazového řádku Azure.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

předchozí příkaz Hello stahuje a spouští hello *install_lamp.sh* skriptu na hello názvem virtuálního počítače *svítilna vm*.

Vzhledem k tomu, že aplikace hello zahrnuje webový server, mějte na paměti, port naslouchání tooopen protokolu HTTP na hello vzdálené virtuální počítač s další příkaz hello.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Monitorování a řešení potíží
Můžete se podívat na tom, jak dobře hello hello vlastní skript se spustí prohlédnutím souboru protokolu hello vzdálený počítač. SSH příliš*svítilna vm* a soubor protokolu poškozené databáze hello další příkazem hello.

    cd /var/log/azure/customscript
    tail -f handler.log

Po spuštění hello rozšíření CustomScript můžete procházet toohello PHP stránku, kterou jste vytvořili pro informace. Hello PHP stránku hello příklad v tomto článku je *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Další zdroje
Můžete použít stejné základní kroky toodeploy hello složitější aplikace. V tomto příkladu byla uložena hello instalační skript jako veřejného objektu blob ve službě Azure Storage. Bezpečnější možnost by jako zabezpečené blob s toostore hello instalační skript [zabezpečený přístup k podpisu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Další zdroje pro rozhraní příkazového řádku Azure, Linux a hello rozšíření CustomScript jsou uvedeny dále.

[Automatizovat úkoly vlastního nastavení virtuálního počítače s Linuxem pomocí rozšíření CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Rozšíření Azure Linux (Githubu)](https://github.com/Azure/azure-linux-extensions)