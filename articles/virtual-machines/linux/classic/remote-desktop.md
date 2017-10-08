---
title: "aaaRemote plochy tooa virtuálního počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace vzdálené plochy tooconnect tooa virtuálních počítačů služby Microsoft Azure Linux pro model nasazení Classic hello"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.openlocfilehash: aadd6e87883cf9cacf9d198b680669d594206e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-tooconnect-tooa-microsoft-azure-linux-vm"></a>Pomocí vzdálené plochy tooconnect tooa virtuálních počítačů služby Microsoft Azure Linux
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Hello aktualizovat Resource Manager verze tohoto článku, najdete v části [zde](../use-remote-desktop.md).

## <a name="overview"></a>Přehled
Protokol RDP (Remote Desktop Protocol) je vlastní protokol, pro systém Windows. Jak jsme vzdáleně pomocí protokolu RDP tooconnect tooa virtuálního počítače s Linuxem (virtuálním počítači)?

Tyto pokyny vám poskytne hello odpovědí! Pomůže vám xrdp tooinstall a konfigurace vašeho Microsoft Azure Linux virtuálního počítače, které vám umožní připojit tooit pomocí vzdálené plochy z počítače s Windows. Linux virtuálního počítače s Ubuntu nebo OpenSUSE hello příklad v tomto návodu budeme používat.

Nástroj xrdp Hello je otevřenou zdrojového serveru RDP, který vám umožní tooconnect server Linux pomocí vzdálené plochy z počítače s Windows. RDP má lepší výkon než VNC (Computing virtuální sítě). VNC vykreslí pomocí grafiky JPEG kvality a může být pomalé, zatímco RDP je rychlý a jasný.

> [!NOTE]
> Již musí mít virtuální počítač Microsoft Azure s Linuxem. toocreate a nastavení virtuálního počítače s Linuxem, najdete v části hello [kurzu virtuální počítač Azure s Linuxem](createportal.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Vytvořit koncový bod pro vzdálené plochy
Použijeme hello výchozí koncový bod 3389 v této dokumentace pro vzdálenou plochu. Nastavit 3389 koncový bod jako `Remote Desktop` tooyour virtuálního počítače s Linuxem jako níže:

![Bitové kopie](./media/remote-desktop/endpoint-for-linux-server.png)

Pokud nevíte jak zjistit, tooset až koncového bodu pro virtuální počítač, [v tomto návodu](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Nainstalujte Gnome plochy
Připojit virtuální počítač s Linuxem tooyour prostřednictvím `putty`a nainstalujte `Gnome Desktop`.

Ubuntu použijte:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


Pro OpenSUSE použijte:

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a>Nainstalujte xrdp
Ubuntu použijte:

    #sudo apt-get install xrdp

Pro OpenSUSE použijte:

> [!NOTE]
> Aktualizujte hello OpenSUSE verzi s verzí hello, který používáte v hello následující příkaz. Následující příklad Hello je pro `OpenSUSE 13.2`.
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Spusťte xrdp a nastavte xdrp při spuštění
Pro OpenSUSE použijte:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

Pro Ubuntu, bude spuštěn xrdp a eanbled na spouštěcí až po instalaci automaticky.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Pokud používáte verzi Ubuntu později než Ubuntu 12.04LTS pomocí xfce
Protože hello aktuální verzi xrdp nepodporuje Gnome Desktop pro verze Ubuntu později než Ubuntu 12.04LTS, budeme používat `xfce` plochy místo.

tooinstall `xfce`, použijte tento příkaz:

    #sudo apt-get install xubuntu-desktop

Potom povolte `xfce` použití tohoto příkazu:

    #echo xfce4-session >~/.xsession

Upravte konfigurační soubor hello `/etc/xrdp/startwm.sh`:

    #sudo vi /etc/xrdp/startwm.sh   

Přidejte řádek hello `xfce4-session` před řádek hello `/etc/X11/Xsession`.

toorestart hello xrdp služby, použijte toto:

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Připojení virtuálním počítačům s Linuxem z počítače s Windows
V počítači s Windows spusťte klienta vzdálené plochy hello a zadejte název DNS virtuálních počítačů Linux. Nebo přejděte toohello řídicí panel vašeho virtuálního počítače v hello portál Azure a klikněte na tlačítko `Connect` tooconnect virtuálním počítačům s Linuxem. V takovém případě se zobrazí okno přihlášení hello:

![Bitové kopie](./media/remote-desktop/no2.png)

Přihlaste se pomocí hello uživatelské jméno a heslo k virtuálním počítačům s Linuxem.

## <a name="next-steps"></a>Další kroky
Další informace o používání xrdp najdete v tématu [http://www.xrdp.org/](http://www.xrdp.org/).
