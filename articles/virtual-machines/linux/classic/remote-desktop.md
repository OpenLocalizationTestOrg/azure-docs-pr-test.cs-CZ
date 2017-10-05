---
title: "Vzdálená plocha pro virtuální počítač s Linuxem | Microsoft Docs"
description: "Postup instalace a konfigurace vzdálené plochy pro připojení k virtuálnímu počítači Microsoft Azure Linux pro model nasazení Classic"
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
ms.openlocfilehash: 68031d548bdbeda9a83d1bceaaea7c5bbcab3188
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a><span data-ttu-id="74ad4-103">Použití Vzdálené plochy pro připojení k virtuálnímu počítači Microsoft Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="74ad4-103">Using Remote Desktop to connect to a Microsoft Azure Linux VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="74ad4-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="74ad4-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="74ad4-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="74ad4-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="74ad4-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="74ad4-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="74ad4-107">Aktualizovaná verze Resource Manager v tomto článku, najdete v části [zde](../use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="74ad4-107">For the updated Resource Manager version of this article, see [here](../use-remote-desktop.md).</span></span>

## <a name="overview"></a><span data-ttu-id="74ad4-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="74ad4-108">Overview</span></span>
<span data-ttu-id="74ad4-109">Protokol RDP (Remote Desktop Protocol) je vlastní protokol, pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="74ad4-109">RDP (Remote Desktop Protocol) is a proprietary protocol used for Windows.</span></span> <span data-ttu-id="74ad4-110">Jak jsme pomocí protokolu RDP pro vzdálené připojení k virtuální počítač s Linuxem (virtuálním počítači)?</span><span class="sxs-lookup"><span data-stu-id="74ad4-110">How can we use RDP to connect to a Linux VM (virtual machine) remotely?</span></span>

<span data-ttu-id="74ad4-111">Tyto pokyny vám poskytne odpověď!</span><span class="sxs-lookup"><span data-stu-id="74ad4-111">This guidance will give you the answer!</span></span> <span data-ttu-id="74ad4-112">Pomůže k instalaci a konfiguraci xrdp na vašem Microsoft Azure Linux virtuálním počítači, který vám umožní připojit se k němu pomocí vzdálené plochy z počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="74ad4-112">It will help you to install and config xrdp on your Microsoft Azure Linux VM, which lets you connect to it with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="74ad4-113">Linux virtuálního počítače s Ubuntu nebo OpenSUSE jako v příkladu v tomto návodu budeme používat.</span><span class="sxs-lookup"><span data-stu-id="74ad4-113">We will use Linux VM running Ubuntu or OpenSUSE as the example in this guidance.</span></span>

<span data-ttu-id="74ad4-114">Nástroj xrdp je serveru RDP s otevřeným zdrojem, která umožňuje připojit Linux server pomocí vzdálené plochy z počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="74ad4-114">The xrdp tool is an open source RDP server that allows you to connect your Linux server with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="74ad4-115">RDP má lepší výkon než VNC (Computing virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="74ad4-115">RDP has better performance than VNC (Virtual Network Computing).</span></span> <span data-ttu-id="74ad4-116">VNC vykreslí pomocí grafiky JPEG kvality a může být pomalé, zatímco RDP je rychlý a jasný.</span><span class="sxs-lookup"><span data-stu-id="74ad4-116">VNC renders using JPEG-quality graphics and can be slow, whereas RDP is fast and crystal clear.</span></span>

> [!NOTE]
> <span data-ttu-id="74ad4-117">Již musí mít virtuální počítač Microsoft Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="74ad4-117">You must already have an Microsoft Azure VM running Linux.</span></span> <span data-ttu-id="74ad4-118">Vytvoření a nastavení virtuálního počítače s Linuxem najdete v tématu [kurzu virtuální počítač Azure s Linuxem](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="74ad4-118">To create and set up a Linux VM, see the [Azure Linux VM tutorial](createportal.md).</span></span>
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a><span data-ttu-id="74ad4-119">Vytvořit koncový bod pro vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="74ad4-119">Create an endpoint for Remote Desktop</span></span>
<span data-ttu-id="74ad4-120">Budeme používat výchozí koncový bod 3389 pro vzdálené plochy v této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="74ad4-120">We will use the default endpoint 3389 for Remote Desktop in this doc.</span></span> <span data-ttu-id="74ad4-121">Nastavit 3389 koncový bod jako `Remote Desktop` k virtuálním počítačům s Linuxem jako níže:</span><span class="sxs-lookup"><span data-stu-id="74ad4-121">Set up 3389 endpoint as `Remote Desktop` to your Linux VM like below:</span></span>

![Bitové kopie](./media/remote-desktop/endpoint-for-linux-server.png)

<span data-ttu-id="74ad4-123">Pokud nevíte jak nastavit koncový bod pro virtuální počítač, najdete v části [v tomto návodu](setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="74ad4-123">If you don't know how to set up an endpoint for your VM, see [this guidance](setup-endpoints.md).</span></span>

## <a name="install-gnome-desktop"></a><span data-ttu-id="74ad4-124">Nainstalujte Gnome plochy</span><span class="sxs-lookup"><span data-stu-id="74ad4-124">Install Gnome Desktop</span></span>
<span data-ttu-id="74ad4-125">Připojení k virtuálním počítačům s Linuxem pomocí `putty`a nainstalujte `Gnome Desktop`.</span><span class="sxs-lookup"><span data-stu-id="74ad4-125">Connect to your Linux VM through `putty`, and install `Gnome Desktop`.</span></span>

<span data-ttu-id="74ad4-126">Ubuntu použijte:</span><span class="sxs-lookup"><span data-stu-id="74ad4-126">For Ubuntu, use:</span></span>

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


<span data-ttu-id="74ad4-127">Pro OpenSUSE použijte:</span><span class="sxs-lookup"><span data-stu-id="74ad4-127">For OpenSUSE, use:</span></span>

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a><span data-ttu-id="74ad4-128">Nainstalujte xrdp</span><span class="sxs-lookup"><span data-stu-id="74ad4-128">Install xrdp</span></span>
<span data-ttu-id="74ad4-129">Ubuntu použijte:</span><span class="sxs-lookup"><span data-stu-id="74ad4-129">For Ubuntu, use:</span></span>

    #sudo apt-get install xrdp

<span data-ttu-id="74ad4-130">Pro OpenSUSE použijte:</span><span class="sxs-lookup"><span data-stu-id="74ad4-130">For OpenSUSE, use:</span></span>

> [!NOTE]
> <span data-ttu-id="74ad4-131">Aktualizujte OpenSUSE verze na verzi, kterou používáte v následujícím příkazu.</span><span class="sxs-lookup"><span data-stu-id="74ad4-131">Update the OpenSUSE version with the version you are using in the following command.</span></span> <span data-ttu-id="74ad4-132">Následující příklad je pro `OpenSUSE 13.2`.</span><span class="sxs-lookup"><span data-stu-id="74ad4-132">The example below is for `OpenSUSE 13.2`.</span></span>
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a><span data-ttu-id="74ad4-133">Spusťte xrdp a nastavte xdrp při spuštění</span><span class="sxs-lookup"><span data-stu-id="74ad4-133">Start xrdp and set xdrp service at boot-up</span></span>
<span data-ttu-id="74ad4-134">Pro OpenSUSE použijte:</span><span class="sxs-lookup"><span data-stu-id="74ad4-134">For OpenSUSE, use:</span></span>

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

<span data-ttu-id="74ad4-135">Pro Ubuntu, bude spuštěn xrdp a eanbled na spouštěcí až po instalaci automaticky.</span><span class="sxs-lookup"><span data-stu-id="74ad4-135">For Ubuntu, xrdp will be started and eanbled at boot-up automatically after installation.</span></span>

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a><span data-ttu-id="74ad4-136">Pokud používáte verzi Ubuntu později než Ubuntu 12.04LTS pomocí xfce</span><span class="sxs-lookup"><span data-stu-id="74ad4-136">Using xfce if you are using an Ubuntu version later than Ubuntu 12.04LTS</span></span>
<span data-ttu-id="74ad4-137">Vzhledem k tomu, že aktuální verze xrdp nepodporuje Gnome Desktop pro verze Ubuntu později než Ubuntu 12.04LTS, budeme používat `xfce` plochy místo.</span><span class="sxs-lookup"><span data-stu-id="74ad4-137">Because the current version of xrdp does not support Gnome Desktop for  Ubuntu versions later than Ubuntu 12.04LTS, we will use `xfce` Desktop instead.</span></span>

<span data-ttu-id="74ad4-138">Chcete-li nainstalovat `xfce`, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="74ad4-138">To install `xfce`, use this command:</span></span>

    #sudo apt-get install xubuntu-desktop

<span data-ttu-id="74ad4-139">Potom povolte `xfce` použití tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="74ad4-139">Then enable `xfce` using this command:</span></span>

    #echo xfce4-session >~/.xsession

<span data-ttu-id="74ad4-140">Upravte konfigurační soubor `/etc/xrdp/startwm.sh`:</span><span class="sxs-lookup"><span data-stu-id="74ad4-140">Edit the config file `/etc/xrdp/startwm.sh`:</span></span>

    #sudo vi /etc/xrdp/startwm.sh   

<span data-ttu-id="74ad4-141">Přidejte řádek `xfce4-session` před řádek `/etc/X11/Xsession`.</span><span class="sxs-lookup"><span data-stu-id="74ad4-141">Add the line `xfce4-session` before the line `/etc/X11/Xsession`.</span></span>

<span data-ttu-id="74ad4-142">Chcete-li restartovat službu xrdp, použijte toto:</span><span class="sxs-lookup"><span data-stu-id="74ad4-142">To restart the xrdp service, use this:</span></span>

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a><span data-ttu-id="74ad4-143">Připojení virtuálním počítačům s Linuxem z počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="74ad4-143">Connect your Linux VM from a Windows machine</span></span>
<span data-ttu-id="74ad4-144">V počítači s Windows spusťte klienta vzdálené plochy a zadejte název DNS virtuálních počítačů Linux.</span><span class="sxs-lookup"><span data-stu-id="74ad4-144">In a Windows machine, start the Remote Desktop client and input your Linux VM DNS name.</span></span> <span data-ttu-id="74ad4-145">Přejděte na řídicí panel virtuálního počítače na portálu Azure a klikněte na tlačítko `Connect` připojení virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="74ad4-145">Or go to the Dashboard of your VM in the Azure portal and click `Connect` to connect your Linux VM.</span></span> <span data-ttu-id="74ad4-146">V takovém případě se zobrazí okno přihlášení:</span><span class="sxs-lookup"><span data-stu-id="74ad4-146">In that case, you see the login window:</span></span>

![Bitové kopie](./media/remote-desktop/no2.png)

<span data-ttu-id="74ad4-148">Přihlaste se pomocí uživatelské jméno a heslo k virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="74ad4-148">Log in with the user name and password of your Linux VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74ad4-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74ad4-149">Next steps</span></span>
<span data-ttu-id="74ad4-150">Další informace o používání xrdp najdete v tématu [http://www.xrdp.org/](http://www.xrdp.org/).</span><span class="sxs-lookup"><span data-stu-id="74ad4-150">For more information about using xrdp, see [http://www.xrdp.org/](http://www.xrdp.org/).</span></span>
