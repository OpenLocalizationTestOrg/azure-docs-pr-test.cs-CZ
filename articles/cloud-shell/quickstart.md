---
title: "Azure rychlý start prostředí cloudu (Preview) | Microsoft Docs"
description: "Rychlý úvodní kurz pro Azure cloudové prostředí"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 75676eb0ab784e2adbfd27b170c1dee5599b74ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="quickstart-for-using-the-azure-cloud-shell"></a>Rychlý úvodní kurz pro používání prostředí cloudové služby Azure

Tento dokument podrobně popisuje postup používání prostředí cloudové služby Azure v [portál Azure](https://ms.portal.azure.com/).

## <a name="start-cloud-shell"></a>Spusťte prostředí cloudu
1. Spusťte **cloudové prostředí** z horním navigačním panelu portálu Azure <br>
![](media/shell-icon.png)
2. Vyberte předplatné, chcete-li vytvořit účet úložiště a sdílenou složku Azure
3. Vyberte "Vytvoření úložiště"

> [!TIP]
> Pro Azure CLI 2.0 se automaticky ověřeni v každé relaci..

### <a name="set-your-subscription"></a>Nastavení předplatného
1. Seznam odběrů, ke kterým máte přístup k: <br>
`az account list`
2. Nastavte upřednostňované předplatného: <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> Vaše předplatné se uloží pro budoucí relace pomocí `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte novou skupinu prostředků v WestUS s názvem "MyRG": <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem
Vytvoření virtuálního počítače s Ubuntu v nové skupiny prostředků. Azure CLI 2.0 bude vytvoření klíčů SSH a instalace virtuálních počítačů s nimi. <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> Veřejné a soukromé klíče používá k ověření svého virtuálního počítače jsou umístěny v `/User/.ssh/id_rsa` a `/User/.ssh/id_rsa.pub` pomocí Azure CLI 2.0 ve výchozím nastavení. Vaše složky .ssh je trvalé připojené Azure sdílené složky na obrázku 5 GB.

Vaše uživatelské jméno pro tento virtuální počítač bude vaše uživatelské jméno použité v prostředí cloudu ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH do virtuálním počítačům s Linuxem
1. Vyhledejte název virtuálního počítače v panelu vyhledávání v portálu Azure
2. Klikněte na tlačítko "Připojit" a spusťte:`ssh username@ipaddress`

![](media/sshcmd-copy.png)

Při navazování připojení SSH, měli byste vidět úvodní řádku Ubuntu.
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Čištění 
Odstranění skupiny prostředků a všechny prostředky v něm: <br>
Spusťte `az group delete -n MyRG`.

## <a name="next-steps"></a>Další kroky
[Další informace o zachování úložiště v prostředí cloudu](persisting-shell-storage.md) <br>
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/) <br>
[Další informace o Azure File storage](../storage/files/storage-files-introduction.md) <br>