---
title: "Rychlý Start - aaaAzure vytvořit portál virtuálních počítačů Windows | Microsoft Docs"
description: "Rychlý start Azure – Vytvoření virtuálního počítače s Windows pomocí portálu"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5646ad51244db6d214c0121d1f7cc45c59f9a78b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="63280-103">Vytvoření virtuálního počítače s Windows pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="63280-103">Create a Windows virtual machine with hello Azure portal</span></span>

<span data-ttu-id="63280-104">Virtuální počítače Azure můžete vytvořit prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63280-104">Azure virtual machines can be created through hello Azure portal.</span></span> <span data-ttu-id="63280-105">Tato metoda poskytuje uživatelské rozhraní v prohlížeči, pomocí kterého můžete vytvářet a konfigurovat virtuální počítače a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="63280-105">This method provides a browser-based user interface for creating and configuring virtual machines and all related resources.</span></span> <span data-ttu-id="63280-106">Tento postup rychlého spuštění prostřednictvím vytvoření virtuálního počítače a instalaci webovém serveru na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="63280-106">This Quickstart steps through creating a virtual machine and installing a webserver on hello VM.</span></span>

<span data-ttu-id="63280-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="63280-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="63280-108">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="63280-108">Log in tooAzure</span></span>

<span data-ttu-id="63280-109">Přihlaste se toohello portál Azure na http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="63280-109">Log in toohello Azure portal at http://portal.azure.com.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="63280-110">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="63280-110">Create virtual machine</span></span>

1. <span data-ttu-id="63280-111">Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63280-111">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="63280-112">Vyberte **Compute** a potom vyberte **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="63280-112">Select **Compute**, and then select **Windows Server 2016 Datacenter**.</span></span> 

3. <span data-ttu-id="63280-113">Zadejte informace o virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="63280-113">Enter hello virtual machine information.</span></span> <span data-ttu-id="63280-114">Hello uživatelské jméno a heslo zadané v tomto poli je použité toolog toohello virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="63280-114">hello user name and password entered here is used toolog in toohello virtual machine.</span></span> <span data-ttu-id="63280-115">Jakmile budete hotovi, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="63280-115">When complete, click **OK**.</span></span>

    ![Zadejte základní informace o virtuální počítač v okně portálu hello](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. <span data-ttu-id="63280-117">Vyberte velikost hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="63280-117">Select a size for hello VM.</span></span> <span data-ttu-id="63280-118">Vyberte další velikosti toosee **zobrazit všechny** nebo změňte hello **podporován typ disku** filtru.</span><span class="sxs-lookup"><span data-stu-id="63280-118">toosee more sizes, select **View all** or change hello **Supported disk type** filter.</span></span> 

    ![Snímek obrazovky zobrazující velikosti virtuálních počítačů](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. <span data-ttu-id="63280-120">V okně Nastavení hello, zachovat hello výchozí hodnoty a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="63280-120">On hello settings blade, keep hello defaults and click **OK**.</span></span>

6. <span data-ttu-id="63280-121">Na stránce Souhrn hello, klikněte na **Ok** nasazení virtuálního počítače toostart hello.</span><span class="sxs-lookup"><span data-stu-id="63280-121">On hello summary page, click **Ok** toostart hello virtual machine deployment.</span></span>

7. <span data-ttu-id="63280-122">Hello virtuálních počítačů bude definovaného toohello řídicí panel portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63280-122">hello VM will be pinned toohello Azure portal dashboard.</span></span> <span data-ttu-id="63280-123">Po dokončení nasazení hello souhrnu okna hello virtuální počítač se automaticky otevře.</span><span class="sxs-lookup"><span data-stu-id="63280-123">Once hello deployment has completed, hello VM summary blade automatically opens.</span></span>


## <a name="connect-toovirtual-machine"></a><span data-ttu-id="63280-124">Připojte počítač toovirtual</span><span class="sxs-lookup"><span data-stu-id="63280-124">Connect toovirtual machine</span></span>

<span data-ttu-id="63280-125">Vytvoření virtuálního počítače toohello připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="63280-125">Create a remote desktop connection toohello virtual machine.</span></span>

1. <span data-ttu-id="63280-126">Klikněte na tlačítko hello **Connect** na vlastnosti virtuálního počítače hello tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63280-126">Click hello **Connect** button on hello virtual machine properties.</span></span> <span data-ttu-id="63280-127">Vytvoří a stáhne se soubor protokolu RDP (Remote Desktop Protocol) – soubor .rdp.</span><span class="sxs-lookup"><span data-stu-id="63280-127">A Remote Desktop Protocol file (.rdp file) is created and downloaded.</span></span>

    ![Portál 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. <span data-ttu-id="63280-129">tooconnect tooyour virtuální počítač, otevřete hello stáhnout soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="63280-129">tooconnect tooyour VM, open hello downloaded RDP file.</span></span> <span data-ttu-id="63280-130">Pokud se zobrazí výzva, klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="63280-130">If prompted, click **Connect**.</span></span> <span data-ttu-id="63280-131">V systému Mac, je nutné klientem RDP, jako je tato [klient služby Vzdálená plocha](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) z hello Mac App Storu.</span><span class="sxs-lookup"><span data-stu-id="63280-131">On a Mac, you need an RDP client such as this [Remote Desktop Client](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) from hello Mac App Store.</span></span>

3. <span data-ttu-id="63280-132">Zadejte hello uživatelské jméno a heslo, které jste zadali při vytváření hello virtuálního počítače a potom klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="63280-132">Enter hello user name and password you specified when creating hello virtual machine, then click **Ok**.</span></span>

4. <span data-ttu-id="63280-133">Může se zobrazit upozornění certifikátu během procesu přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="63280-133">You may receive a certificate warning during hello sign-in process.</span></span> <span data-ttu-id="63280-134">Klikněte na tlačítko **Ano** nebo **pokračovat** tooproceed hello připojení.</span><span class="sxs-lookup"><span data-stu-id="63280-134">Click **Yes** or **Continue** tooproceed with hello connection.</span></span>


## <a name="install-iis-using-powershell"></a><span data-ttu-id="63280-135">Instalace služby IIS pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="63280-135">Install IIS using PowerShell</span></span>

<span data-ttu-id="63280-136">Hello virtuálního počítače spusťte relaci prostředí PowerShell a spusťte následující příkaz tooinstall IIS hello.</span><span class="sxs-lookup"><span data-stu-id="63280-136">On hello virtual machine, start a PowerShell session and run hello following command tooinstall IIS.</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="63280-137">Až budete hotoví, ukončete relaci protokolu RDP hello a vrátí hello vlastnosti virtuálního počítače v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63280-137">When done, exit hello RDP session and return hello VM properties in hello Azure portal.</span></span>

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="63280-138">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="63280-138">Open port 80 for web traffic</span></span> 

<span data-ttu-id="63280-139">Skupina zabezpečení sítě (NSG) zabezpečuje příchozí a odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="63280-139">A Network security group (NSG) secures inbound and outbound traffic.</span></span> <span data-ttu-id="63280-140">Když virtuální počítač je vytvořen z hello portálu Azure, vytvoří se příchozí pravidlo na portu 3389 pro připojení RDP.</span><span class="sxs-lookup"><span data-stu-id="63280-140">When a VM is created from hello Azure portal, an inbound rule is created on port 3389 for RDP connections.</span></span> <span data-ttu-id="63280-141">Protože tento virtuální počítač hostuje webovém serveru, musí pravidlo NSG toobe vytvořenou pro port 80.</span><span class="sxs-lookup"><span data-stu-id="63280-141">Because this VM hosts a webserver, an NSG rule needs toobe created for port 80.</span></span>

1. <span data-ttu-id="63280-142">Na virtuálním počítači hello, klikněte na název hello hello **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="63280-142">On hello virtual machine, click hello name of hello **Resource group**.</span></span>
2. <span data-ttu-id="63280-143">Vyberte hello **skupinu zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="63280-143">Select hello **network security group**.</span></span> <span data-ttu-id="63280-144">Hello NSG lze identifikovat pomocí hello **typ** sloupce.</span><span class="sxs-lookup"><span data-stu-id="63280-144">hello NSG can be identified using hello **Type** column.</span></span> 
3. <span data-ttu-id="63280-145">V levé nabídce hello v části nastavení, klikněte na tlačítko **příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="63280-145">On hello left-hand menu, under settings, click **Inbound security rules**.</span></span>
4. <span data-ttu-id="63280-146">Klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="63280-146">Click on **Add**.</span></span>
5. <span data-ttu-id="63280-147">Do pole **Název** zadejte **http**.</span><span class="sxs-lookup"><span data-stu-id="63280-147">In **Name**, type **http**.</span></span> <span data-ttu-id="63280-148">Zajistěte, aby **rozsah portů** nastavena too80 a **akce** je nastaven příliš**povolit**.</span><span class="sxs-lookup"><span data-stu-id="63280-148">Make sure **Port range** is set too80 and **Action** is set too**Allow**.</span></span> 
6. <span data-ttu-id="63280-149">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="63280-149">Click **OK**.</span></span>


## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="63280-150">Zobrazení hello úvodní stránka služby IIS</span><span class="sxs-lookup"><span data-stu-id="63280-150">View hello IIS welcome page</span></span>

<span data-ttu-id="63280-151">Pomocí služby IIS nainstalovaná a port 80 otevřete tooyour virtuálních počítačů, webový server hello je nyní přístupná z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="63280-151">With IIS installed, and port 80 open tooyour VM, hello webserver can now be accessed from hello internet.</span></span> <span data-ttu-id="63280-152">Otevřete webový prohlížeč a zadejte hello veřejnou IP adresu hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="63280-152">Open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="63280-153">Hello veřejnou IP adresu naleznete v okně hello virtuálních počítačů v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63280-153">hello public IP address can be found on hello VM blade in hello Azure portal.</span></span>

![Výchozí web služby IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="63280-155">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="63280-155">Clean up resources</span></span>

<span data-ttu-id="63280-156">Pokud již nepotřebujete, odstraňte skupinu prostředků hello, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="63280-156">When no longer needed, delete hello resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="63280-157">toodo Ano, vyberte skupinu prostředků hello v okně hello virtuální počítač a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="63280-157">toodo so, select hello resource group from hello virtual machine blade and click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63280-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63280-158">Next steps</span></span>

<span data-ttu-id="63280-159">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="63280-159">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="63280-160">toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="63280-160">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="63280-161">Kurzy pro virtuální počítače Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="63280-161">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
