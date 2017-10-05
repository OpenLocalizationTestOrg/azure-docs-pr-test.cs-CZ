---
title: "Rychlý start Azure – Vytvoření virtuálního počítače s Windows pomocí portálu | Dokumentace Microsoftu"
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
ms.openlocfilehash: 31ac18add9c3fd956e0d37b1e0c1a510265c22e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a><span data-ttu-id="232b6-103">Vytvoření virtuálního počítače s Windows pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="232b6-103">Create a Windows virtual machine with the Azure portal</span></span>

<span data-ttu-id="232b6-104">Virtuální počítače Azure je možné vytvářet na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="232b6-104">Azure virtual machines can be created through the Azure portal.</span></span> <span data-ttu-id="232b6-105">Tato metoda poskytuje uživatelské rozhraní v prohlížeči, pomocí kterého můžete vytvářet a konfigurovat virtuální počítače a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="232b6-105">This method provides a browser-based user interface for creating and configuring virtual machines and all related resources.</span></span> <span data-ttu-id="232b6-106">Tento Rychlý start prochází jednotlivé kroky k vytvoření virtuálního počítače a instalaci webového serveru na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="232b6-106">This Quickstart steps through creating a virtual machine and installing a webserver on the VM.</span></span>

<span data-ttu-id="232b6-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="232b6-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="232b6-108">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="232b6-108">Log in to Azure</span></span>

<span data-ttu-id="232b6-109">Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="232b6-109">Log in to the Azure portal at http://portal.azure.com.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="232b6-110">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="232b6-110">Create virtual machine</span></span>

1. <span data-ttu-id="232b6-111">Klikněte na tlačítko **Nový** v levém horním rohu portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="232b6-111">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="232b6-112">Vyberte **Compute** a potom vyberte **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="232b6-112">Select **Compute**, and then select **Windows Server 2016 Datacenter**.</span></span> 

3. <span data-ttu-id="232b6-113">Zadejte informace o virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="232b6-113">Enter the virtual machine information.</span></span> <span data-ttu-id="232b6-114">Uživatelské jméno a heslo, které tady zadáte, se používá k přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="232b6-114">The user name and password entered here is used to log in to the virtual machine.</span></span> <span data-ttu-id="232b6-115">Jakmile budete hotovi, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="232b6-115">When complete, click **OK**.</span></span>

    ![Zadání základních informací o virtuálním počítači v okně portálu](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. <span data-ttu-id="232b6-117">Vyberte velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="232b6-117">Select a size for the VM.</span></span> <span data-ttu-id="232b6-118">Pokud chcete zobrazit další velikosti, vyberte **Zobrazit všechny** nebo změňte filtr **Podporovaný typ disku**.</span><span class="sxs-lookup"><span data-stu-id="232b6-118">To see more sizes, select **View all** or change the **Supported disk type** filter.</span></span> 

    ![Snímek obrazovky zobrazující velikosti virtuálních počítačů](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. <span data-ttu-id="232b6-120">V okně Nastavení ponechte výchozí nastavení a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="232b6-120">On the settings blade, keep the defaults and click **OK**.</span></span>

6. <span data-ttu-id="232b6-121">Na stránce Souhrn kliknutím na **Ok** spusťte nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="232b6-121">On the summary page, click **Ok** to start the virtual machine deployment.</span></span>

7. <span data-ttu-id="232b6-122">Virtuální počítač se připne na řídicí panel webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="232b6-122">The VM will be pinned to the Azure portal dashboard.</span></span> <span data-ttu-id="232b6-123">Po dokončení nasazení se automaticky otevře okno souhrnu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="232b6-123">Once the deployment has completed, the VM summary blade automatically opens.</span></span>


## <a name="connect-to-virtual-machine"></a><span data-ttu-id="232b6-124">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="232b6-124">Connect to virtual machine</span></span>

<span data-ttu-id="232b6-125">Vytvořte připojení ke vzdálené ploše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="232b6-125">Create a remote desktop connection to the virtual machine.</span></span>

1. <span data-ttu-id="232b6-126">Klikněte na tlačítko **Připojit** ve vlastnostech virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="232b6-126">Click the **Connect** button on the virtual machine properties.</span></span> <span data-ttu-id="232b6-127">Vytvoří a stáhne se soubor protokolu RDP (Remote Desktop Protocol) – soubor .rdp.</span><span class="sxs-lookup"><span data-stu-id="232b6-127">A Remote Desktop Protocol file (.rdp file) is created and downloaded.</span></span>

    ![Portál 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. <span data-ttu-id="232b6-129">Chcete-li se připojit k virtuálnímu počítači, otevřete stažený soubor protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="232b6-129">To connect to your VM, open the downloaded RDP file.</span></span> <span data-ttu-id="232b6-130">Pokud se zobrazí výzva, klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="232b6-130">If prompted, click **Connect**.</span></span> <span data-ttu-id="232b6-131">Na počítači Mac budete potřebovat klienta protokolu RDP, jako je například tento [Klient vzdálené plochy](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) na Mac App Storu.</span><span class="sxs-lookup"><span data-stu-id="232b6-131">On a Mac, you need an RDP client such as this [Remote Desktop Client](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) from the Mac App Store.</span></span>

3. <span data-ttu-id="232b6-132">Zadejte uživatelské jméno a heslo, které jste zadali při vytváření virtuálního počítače, a potom klikněte na **Ok**.</span><span class="sxs-lookup"><span data-stu-id="232b6-132">Enter the user name and password you specified when creating the virtual machine, then click **Ok**.</span></span>

4. <span data-ttu-id="232b6-133">Během procesu přihlášení se může zobrazit upozornění certifikátu.</span><span class="sxs-lookup"><span data-stu-id="232b6-133">You may receive a certificate warning during the sign-in process.</span></span> <span data-ttu-id="232b6-134">Klikněte na **Ano** nebo **Pokračovat** a pokračujte v připojení.</span><span class="sxs-lookup"><span data-stu-id="232b6-134">Click **Yes** or **Continue** to proceed with the connection.</span></span>


## <a name="install-iis-using-powershell"></a><span data-ttu-id="232b6-135">Instalace služby IIS pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="232b6-135">Install IIS using PowerShell</span></span>

<span data-ttu-id="232b6-136">Na virtuálním počítači spusťte relaci PowerShellu a následujícím příkazem nainstalujte službu IIS.</span><span class="sxs-lookup"><span data-stu-id="232b6-136">On the virtual machine, start a PowerShell session and run the following command to install IIS.</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="232b6-137">Až budete hotovi, ukončete relaci RDP a vraťte se do vlastností virtuálního počítače na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="232b6-137">When done, exit the RDP session and return the VM properties in the Azure portal.</span></span>

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="232b6-138">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="232b6-138">Open port 80 for web traffic</span></span> 

<span data-ttu-id="232b6-139">Skupina zabezpečení sítě (NSG) zabezpečuje příchozí a odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="232b6-139">A Network security group (NSG) secures inbound and outbound traffic.</span></span> <span data-ttu-id="232b6-140">Když se virtuální počítač vytvoří na webu Azure Portal, pro připojení ke vzdálené ploše se vytvoří příchozí pravidlo na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="232b6-140">When a VM is created from the Azure portal, an inbound rule is created on port 3389 for RDP connections.</span></span> <span data-ttu-id="232b6-141">Protože je tento virtuální počítač hostitelem webového serveru, je potřeba vytvořit pravidlo NSG pro port 80.</span><span class="sxs-lookup"><span data-stu-id="232b6-141">Because this VM hosts a webserver, an NSG rule needs to be created for port 80.</span></span>

1. <span data-ttu-id="232b6-142">Na virtuálním počítači klikněte na název **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="232b6-142">On the virtual machine, click the name of the **Resource group**.</span></span>
2. <span data-ttu-id="232b6-143">Vyberte **skupinu zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="232b6-143">Select the **network security group**.</span></span> <span data-ttu-id="232b6-144">NSG můžete identifikovat pomocí sloupce **Typ**.</span><span class="sxs-lookup"><span data-stu-id="232b6-144">The NSG can be identified using the **Type** column.</span></span> 
3. <span data-ttu-id="232b6-145">V nabídce vlevo v části Nastavení klikněte na **Příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="232b6-145">On the left-hand menu, under settings, click **Inbound security rules**.</span></span>
4. <span data-ttu-id="232b6-146">Klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="232b6-146">Click on **Add**.</span></span>
5. <span data-ttu-id="232b6-147">Do pole **Název** zadejte **http**.</span><span class="sxs-lookup"><span data-stu-id="232b6-147">In **Name**, type **http**.</span></span> <span data-ttu-id="232b6-148">Zkontrolujte, že **Rozsah portů** je nastavený na 80 a **Akce** je nastavená na **Povolit**.</span><span class="sxs-lookup"><span data-stu-id="232b6-148">Make sure **Port range** is set to 80 and **Action** is set to **Allow**.</span></span> 
6. <span data-ttu-id="232b6-149">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="232b6-149">Click **OK**.</span></span>


## <a name="view-the-iis-welcome-page"></a><span data-ttu-id="232b6-150">Zobrazení úvodní stránky služby IIS</span><span class="sxs-lookup"><span data-stu-id="232b6-150">View the IIS welcome page</span></span>

<span data-ttu-id="232b6-151">Když je teď služba IIS nainstalovaná a port 80 k virtuálnímu počítači otevřený, webový server je přístupný z internetu.</span><span class="sxs-lookup"><span data-stu-id="232b6-151">With IIS installed, and port 80 open to your VM, the webserver can now be accessed from the internet.</span></span> <span data-ttu-id="232b6-152">Otevřete webový prohlížeč a zadejte veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="232b6-152">Open a web browser, and enter the public IP address of the VM.</span></span> <span data-ttu-id="232b6-153">Veřejnou IP adresu najdete v okně virtuálního počítače na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="232b6-153">the public IP address can be found on the VM blade in the Azure portal.</span></span>

![Výchozí web služby IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="232b6-155">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="232b6-155">Clean up resources</span></span>

<span data-ttu-id="232b6-156">Pokud už je nepotřebujete, odstraňte skupinu prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="232b6-156">When no longer needed, delete the resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="232b6-157">Provedete to tak, že v okně virtuálního počítače vyberete skupinu prostředků a kliknete na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="232b6-157">To do so, select the resource group from the virtual machine blade and click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="232b6-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="232b6-158">Next steps</span></span>

<span data-ttu-id="232b6-159">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="232b6-159">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="232b6-160">Další informace o virtuálních počítačích Azure najdete v kurzu pro virtuální počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="232b6-160">To learn more about Azure virtual machines, continue to the tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="232b6-161">Kurzy pro virtuální počítače Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="232b6-161">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
