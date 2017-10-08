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
# <a name="using-remote-desktop-tooconnect-tooa-microsoft-azure-linux-vm"></a><span data-ttu-id="acba6-103">Pomocí vzdálené plochy tooconnect tooa virtuálních počítačů služby Microsoft Azure Linux</span><span class="sxs-lookup"><span data-stu-id="acba6-103">Using Remote Desktop tooconnect tooa Microsoft Azure Linux VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="acba6-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="acba6-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="acba6-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="acba6-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="acba6-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="acba6-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="acba6-107">Hello aktualizovat Resource Manager verze tohoto článku, najdete v části [zde](../use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="acba6-107">For hello updated Resource Manager version of this article, see [here](../use-remote-desktop.md).</span></span>

## <a name="overview"></a><span data-ttu-id="acba6-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="acba6-108">Overview</span></span>
<span data-ttu-id="acba6-109">Protokol RDP (Remote Desktop Protocol) je vlastní protokol, pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="acba6-109">RDP (Remote Desktop Protocol) is a proprietary protocol used for Windows.</span></span> <span data-ttu-id="acba6-110">Jak jsme vzdáleně pomocí protokolu RDP tooconnect tooa virtuálního počítače s Linuxem (virtuálním počítači)?</span><span class="sxs-lookup"><span data-stu-id="acba6-110">How can we use RDP tooconnect tooa Linux VM (virtual machine) remotely?</span></span>

<span data-ttu-id="acba6-111">Tyto pokyny vám poskytne hello odpovědí!</span><span class="sxs-lookup"><span data-stu-id="acba6-111">This guidance will give you hello answer!</span></span> <span data-ttu-id="acba6-112">Pomůže vám xrdp tooinstall a konfigurace vašeho Microsoft Azure Linux virtuálního počítače, které vám umožní připojit tooit pomocí vzdálené plochy z počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="acba6-112">It will help you tooinstall and config xrdp on your Microsoft Azure Linux VM, which lets you connect tooit with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="acba6-113">Linux virtuálního počítače s Ubuntu nebo OpenSUSE hello příklad v tomto návodu budeme používat.</span><span class="sxs-lookup"><span data-stu-id="acba6-113">We will use Linux VM running Ubuntu or OpenSUSE as hello example in this guidance.</span></span>

<span data-ttu-id="acba6-114">Nástroj xrdp Hello je otevřenou zdrojového serveru RDP, který vám umožní tooconnect server Linux pomocí vzdálené plochy z počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="acba6-114">hello xrdp tool is an open source RDP server that allows you tooconnect your Linux server with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="acba6-115">RDP má lepší výkon než VNC (Computing virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="acba6-115">RDP has better performance than VNC (Virtual Network Computing).</span></span> <span data-ttu-id="acba6-116">VNC vykreslí pomocí grafiky JPEG kvality a může být pomalé, zatímco RDP je rychlý a jasný.</span><span class="sxs-lookup"><span data-stu-id="acba6-116">VNC renders using JPEG-quality graphics and can be slow, whereas RDP is fast and crystal clear.</span></span>

> [!NOTE]
> <span data-ttu-id="acba6-117">Již musí mít virtuální počítač Microsoft Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="acba6-117">You must already have an Microsoft Azure VM running Linux.</span></span> <span data-ttu-id="acba6-118">toocreate a nastavení virtuálního počítače s Linuxem, najdete v části hello [kurzu virtuální počítač Azure s Linuxem](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="acba6-118">toocreate and set up a Linux VM, see hello [Azure Linux VM tutorial](createportal.md).</span></span>
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a><span data-ttu-id="acba6-119">Vytvořit koncový bod pro vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="acba6-119">Create an endpoint for Remote Desktop</span></span>
<span data-ttu-id="acba6-120">Použijeme hello výchozí koncový bod 3389 v této dokumentace pro vzdálenou plochu. Nastavit 3389 koncový bod jako `Remote Desktop` tooyour virtuálního počítače s Linuxem jako níže:</span><span class="sxs-lookup"><span data-stu-id="acba6-120">We will use hello default endpoint 3389 for Remote Desktop in this doc. Set up 3389 endpoint as `Remote Desktop` tooyour Linux VM like below:</span></span>

![Bitové kopie](./media/remote-desktop/endpoint-for-linux-server.png)

<span data-ttu-id="acba6-122">Pokud nevíte jak zjistit, tooset až koncového bodu pro virtuální počítač, [v tomto návodu](setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="acba6-122">If you don't know how tooset up an endpoint for your VM, see [this guidance](setup-endpoints.md).</span></span>

## <a name="install-gnome-desktop"></a><span data-ttu-id="acba6-123">Nainstalujte Gnome plochy</span><span class="sxs-lookup"><span data-stu-id="acba6-123">Install Gnome Desktop</span></span>
<span data-ttu-id="acba6-124">Připojit virtuální počítač s Linuxem tooyour prostřednictvím `putty`a nainstalujte `Gnome Desktop`.</span><span class="sxs-lookup"><span data-stu-id="acba6-124">Connect tooyour Linux VM through `putty`, and install `Gnome Desktop`.</span></span>

<span data-ttu-id="acba6-125">Ubuntu použijte:</span><span class="sxs-lookup"><span data-stu-id="acba6-125">For Ubuntu, use:</span></span>

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


<span data-ttu-id="acba6-126">Pro OpenSUSE použijte:</span><span class="sxs-lookup"><span data-stu-id="acba6-126">For OpenSUSE, use:</span></span>

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a><span data-ttu-id="acba6-127">Nainstalujte xrdp</span><span class="sxs-lookup"><span data-stu-id="acba6-127">Install xrdp</span></span>
<span data-ttu-id="acba6-128">Ubuntu použijte:</span><span class="sxs-lookup"><span data-stu-id="acba6-128">For Ubuntu, use:</span></span>

    #sudo apt-get install xrdp

<span data-ttu-id="acba6-129">Pro OpenSUSE použijte:</span><span class="sxs-lookup"><span data-stu-id="acba6-129">For OpenSUSE, use:</span></span>

> [!NOTE]
> <span data-ttu-id="acba6-130">Aktualizujte hello OpenSUSE verzi s verzí hello, který používáte v hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="acba6-130">Update hello OpenSUSE version with hello version you are using in hello following command.</span></span> <span data-ttu-id="acba6-131">Následující příklad Hello je pro `OpenSUSE 13.2`.</span><span class="sxs-lookup"><span data-stu-id="acba6-131">hello example below is for `OpenSUSE 13.2`.</span></span>
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a><span data-ttu-id="acba6-132">Spusťte xrdp a nastavte xdrp při spuštění</span><span class="sxs-lookup"><span data-stu-id="acba6-132">Start xrdp and set xdrp service at boot-up</span></span>
<span data-ttu-id="acba6-133">Pro OpenSUSE použijte:</span><span class="sxs-lookup"><span data-stu-id="acba6-133">For OpenSUSE, use:</span></span>

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

<span data-ttu-id="acba6-134">Pro Ubuntu, bude spuštěn xrdp a eanbled na spouštěcí až po instalaci automaticky.</span><span class="sxs-lookup"><span data-stu-id="acba6-134">For Ubuntu, xrdp will be started and eanbled at boot-up automatically after installation.</span></span>

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a><span data-ttu-id="acba6-135">Pokud používáte verzi Ubuntu později než Ubuntu 12.04LTS pomocí xfce</span><span class="sxs-lookup"><span data-stu-id="acba6-135">Using xfce if you are using an Ubuntu version later than Ubuntu 12.04LTS</span></span>
<span data-ttu-id="acba6-136">Protože hello aktuální verzi xrdp nepodporuje Gnome Desktop pro verze Ubuntu později než Ubuntu 12.04LTS, budeme používat `xfce` plochy místo.</span><span class="sxs-lookup"><span data-stu-id="acba6-136">Because hello current version of xrdp does not support Gnome Desktop for  Ubuntu versions later than Ubuntu 12.04LTS, we will use `xfce` Desktop instead.</span></span>

<span data-ttu-id="acba6-137">tooinstall `xfce`, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="acba6-137">tooinstall `xfce`, use this command:</span></span>

    #sudo apt-get install xubuntu-desktop

<span data-ttu-id="acba6-138">Potom povolte `xfce` použití tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="acba6-138">Then enable `xfce` using this command:</span></span>

    #echo xfce4-session >~/.xsession

<span data-ttu-id="acba6-139">Upravte konfigurační soubor hello `/etc/xrdp/startwm.sh`:</span><span class="sxs-lookup"><span data-stu-id="acba6-139">Edit hello config file `/etc/xrdp/startwm.sh`:</span></span>

    #sudo vi /etc/xrdp/startwm.sh   

<span data-ttu-id="acba6-140">Přidejte řádek hello `xfce4-session` před řádek hello `/etc/X11/Xsession`.</span><span class="sxs-lookup"><span data-stu-id="acba6-140">Add hello line `xfce4-session` before hello line `/etc/X11/Xsession`.</span></span>

<span data-ttu-id="acba6-141">toorestart hello xrdp služby, použijte toto:</span><span class="sxs-lookup"><span data-stu-id="acba6-141">toorestart hello xrdp service, use this:</span></span>

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a><span data-ttu-id="acba6-142">Připojení virtuálním počítačům s Linuxem z počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="acba6-142">Connect your Linux VM from a Windows machine</span></span>
<span data-ttu-id="acba6-143">V počítači s Windows spusťte klienta vzdálené plochy hello a zadejte název DNS virtuálních počítačů Linux.</span><span class="sxs-lookup"><span data-stu-id="acba6-143">In a Windows machine, start hello Remote Desktop client and input your Linux VM DNS name.</span></span> <span data-ttu-id="acba6-144">Nebo přejděte toohello řídicí panel vašeho virtuálního počítače v hello portál Azure a klikněte na tlačítko `Connect` tooconnect virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="acba6-144">Or go toohello Dashboard of your VM in hello Azure portal and click `Connect` tooconnect your Linux VM.</span></span> <span data-ttu-id="acba6-145">V takovém případě se zobrazí okno přihlášení hello:</span><span class="sxs-lookup"><span data-stu-id="acba6-145">In that case, you see hello login window:</span></span>

![Bitové kopie](./media/remote-desktop/no2.png)

<span data-ttu-id="acba6-147">Přihlaste se pomocí hello uživatelské jméno a heslo k virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="acba6-147">Log in with hello user name and password of your Linux VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="acba6-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acba6-148">Next steps</span></span>
<span data-ttu-id="acba6-149">Další informace o používání xrdp najdete v tématu [http://www.xrdp.org/](http://www.xrdp.org/).</span><span class="sxs-lookup"><span data-stu-id="acba6-149">For more information about using xrdp, see [http://www.xrdp.org/](http://www.xrdp.org/).</span></span>
