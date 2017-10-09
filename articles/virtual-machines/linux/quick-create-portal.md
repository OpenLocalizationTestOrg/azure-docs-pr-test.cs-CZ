---
title: "Rychlý Start - aaaAzure vytvoření virtuálního počítače portál | Microsoft Docs"
description: "Rychlý start Azure – Vytvoření virtuálního počítače pomocí portálu"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 984a400c976e349a59f873210d6e04bcdea39e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-portal"></a>Vytvořte virtuální počítač s Linuxem s hello portálu Azure

Virtuální počítače Azure můžete vytvořit prostřednictvím hello portálu Azure. Tato metoda poskytuje uživatelské rozhraní v prohlížeči, pomocí kterého můžete vytvářet a konfigurovat virtuální počítače a všechny související prostředky. Tento postup rychlého spuštění prostřednictvím vytvoření virtuálního počítače a instalaci webovém serveru na hello virtuálních počítačů.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="create-ssh-key-pair"></a>Vytvoření páru klíčů SSH

Budete potřebovat toocomplete pár klíčů SSH tento rychlý start. Pokud máte existující pár klíčů SSH, můžete tento krok přeskočit.

Z prostředí Bash spusťte tento příkaz a postupujte podle hello na obrazovce pokynů. výstup příkazu Hello obsahuje název souboru hello hello soubor veřejného klíče. Kopírovat hello obsah schránky toohello hello soubor veřejného klíče.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure 

Přihlaste se toohello portál Azure na http://portal.azure.com.

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

1. Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

2. Vyberte **Compute** a potom vyberte **Ubuntu Server 16.04 LTS**. 

3. Zadejte informace o virtuálním počítači hello. Jako **Typ ověřování** vyberte **Veřejný klíč SSH**. Při vkládání v veřejný klíč SSH, postará tooremove žádné začínat ani končit mezerou prázdné. Jakmile budete hotovi, klikněte na **OK**.

    ![Zadejte základní informace o virtuální počítač v okně portálu hello](./media/quick-create-portal/create-vm-portal-basic-blade.png)

4. Vyberte velikost hello virtuálních počítačů. Vyberte další velikosti toosee **zobrazit všechny** nebo změňte hello **podporován typ disku** filtru. 

    ![Snímek obrazovky zobrazující velikosti virtuálních počítačů](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. V okně Nastavení hello, zachovat hello výchozí hodnoty a klikněte na tlačítko **OK**.

6. Na stránce Souhrn hello, klikněte na **Ok** nasazení virtuálního počítače toostart hello.

7. Hello virtuálních počítačů bude definovaného toohello řídicí panel portálu Azure. Po dokončení nasazení hello souhrnu okna hello virtuální počítač se automaticky otevře.


## <a name="connect-toovirtual-machine"></a>Připojte počítač toovirtual

Vytvoření připojení SSH s hello virtuálního počítače.

1. Klikněte na tlačítko hello **Connect** tlačítka v okně hello virtuálního počítače. Hello se zobrazí tlačítko Připojit SSH připojovací řetězec, který lze použít tooconnect toohello virtuálnímu počítači.

    ![Portál 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Hello spusťte následující příkaz toocreate na relace SSH. Nahraďte text hello, který ten, který jste zkopírovali z hello portál Azure hello připojovací řetězec.

```bash 
ssh azureuser@40.112.21.50
```

## <a name="install-nginx"></a>Instalace serveru NGINX

Použití hello následující zdroje balíčků tooupdate skriptů bash a instalovat nejnovější balíček NGINX hello. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Až budete hotoví, ukončete hello relace SSH a vrátí hello vlastnosti virtuálního počítače v hello portálu Azure.


## <a name="open-port-80-for-web-traffic"></a>Otevření portu 80 pro webový provoz 

Skupina zabezpečení sítě (NSG) zabezpečuje příchozí a odchozí provoz. Když virtuální počítač je vytvořen z hello portálu Azure, se na port 22 pro SSH připojení vytvoří pravidlo pro příchozí. Protože tento virtuální počítač hostuje webovém serveru, musí pravidlo NSG toobe vytvořenou pro port 80.

1. Na virtuálním počítači hello, klikněte na název hello hello **skupiny prostředků**.
2. Vyberte hello **skupinu zabezpečení sítě**. Hello NSG lze identifikovat pomocí hello **typ** sloupce. 
3. V levé nabídce hello v části nastavení, klikněte na tlačítko **příchozí pravidla zabezpečení**.
4. Klikněte na **Přidat**.
5. Do pole **Název** zadejte **http**. Zajistěte, aby **rozsah portů** nastavena too80 a **akce** je nastaven příliš**povolit**. 
6. Klikněte na **OK**.


## <a name="view-hello-nginx-welcome-page"></a>Zobrazení hello NGINX úvodní stránka

S NGINX nainstalovaná a port 80 otevřete tooyour virtuálních počítačů, webový server hello je nyní přístupná z hello Internetu. Otevřete webový prohlížeč a zadejte hello veřejnou IP adresu hello virtuálních počítačů. Hello veřejnou IP adresu naleznete v okně hello virtuálních počítačů v hello portálu Azure.

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, odstraňte skupinu prostředků hello, virtuální počítač a všechny související prostředky. toodo Ano, vyberte skupinu prostředků hello v okně hello virtuální počítač a klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server. toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače s Linuxem.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Linuxem](./tutorial-manage-vm.md)
