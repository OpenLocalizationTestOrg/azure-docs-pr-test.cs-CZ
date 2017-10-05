---
title: "Instalace služby IIS na vaše první virtuální počítač s Windows | Microsoft Docs"
description: "Experimentujte s prvním virtuálním počítači Windows pomocí instalace služby IIS a otevírání portu 80 pomocí portálu Azure."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6334ea45-db6b-4e08-abb5-1f98927ebc34
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cynthn
ms.openlocfilehash: b11ce1eab0c26a802c31bc418cdf725cbc4fba30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a><span data-ttu-id="21894-103">Experimentujte s instalaci role na vašem virtuálním počítači Windows</span><span class="sxs-lookup"><span data-stu-id="21894-103">Experiment with installing a role on your Windows VM</span></span>
<span data-ttu-id="21894-104">Až budete mít první virtuální počítač (VM) fungovaly, můžete přesunout instalaci softwaru a služeb.</span><span class="sxs-lookup"><span data-stu-id="21894-104">Once you have your first virtual machine (VM) up and running, you can move on to installing software and services.</span></span> <span data-ttu-id="21894-105">V tomto kurzu přidáme nainstalovat službu IIS pomocí Správce serverů na virtuální počítač Windows serveru.</span><span class="sxs-lookup"><span data-stu-id="21894-105">For this tutorial, we are going to use Server Manager on the Windows Server VM to install IIS.</span></span> <span data-ttu-id="21894-106">Poté vytvoříme skupinu zabezpečení sítě (NSG) pomocí portálu Azure otevřete port 80 k provozu služby IIS.</span><span class="sxs-lookup"><span data-stu-id="21894-106">Then, we will create a Network Security Group (NSG) using the Azure portal to open port 80 to IIS traffic.</span></span> 

<span data-ttu-id="21894-107">Pokud jste ještě nevytvořili vaše první virtuální počítač, by měl přejdete zpět do [vytvořit svůj první virtuální počítač Windows v portálu Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před pokračováním v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="21894-107">If you haven't already created your first VM, you should go back to [Create your first Windows virtual machine in the Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before continuing with this tutorial.</span></span>

## <a name="make-sure-the-vm-is-running"></a><span data-ttu-id="21894-108">Ujistěte se, že je virtuální počítač spuštěný.</span><span class="sxs-lookup"><span data-stu-id="21894-108">Make sure the VM is running</span></span>
1. <span data-ttu-id="21894-109">Otevřete web [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="21894-109">Open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="21894-110">V nabídce centra klikněte na **Virtual Machines**.</span><span class="sxs-lookup"><span data-stu-id="21894-110">On the hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="21894-111">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="21894-111">Select the virtual machine from the list.</span></span>
3. <span data-ttu-id="21894-112">Pokud je stav **zastaveno (Deallocated)**, klikněte na tlačítko **spustit** tlačítko **Essentials** okno virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="21894-112">If the status is **Stopped (Deallocated)**, click the **Start** button on the **Essentials** blade of the VM.</span></span> <span data-ttu-id="21894-113">Pokud je stav **systémem**, můžete přesunout další krok.</span><span class="sxs-lookup"><span data-stu-id="21894-113">If the status is **Running**, you can move on to the next step.</span></span>

## <a name="connect-to-the-virtual-machine-and-sign-in"></a><span data-ttu-id="21894-114">Připojit k virtuálnímu počítači a přihlášení</span><span class="sxs-lookup"><span data-stu-id="21894-114">Connect to the virtual machine and sign in</span></span>
1. <span data-ttu-id="21894-115">V nabídce centra klikněte na **Virtual Machines**.</span><span class="sxs-lookup"><span data-stu-id="21894-115">On the hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="21894-116">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="21894-116">Select the virtual machine from the list.</span></span>
2. <span data-ttu-id="21894-117">V okně pro virtuální počítač klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="21894-117">On the blade for the virtual machine, click **Connect**.</span></span> <span data-ttu-id="21894-118">Tím se vytvoří a stáhne soubor .rdp (Remote Desktop Protocol), který představuje zástupce připojení k vašemu počítači.</span><span class="sxs-lookup"><span data-stu-id="21894-118">This creates and downloads a Remote Desktop Protocol file (.rdp file) that is like a shortcut to connect to your machine.</span></span> <span data-ttu-id="21894-119">Můžete si ho uložit na plochu, abyste k němu měli snadno přístup.</span><span class="sxs-lookup"><span data-stu-id="21894-119">You might want to save the file to your desktop for easy access.</span></span> <span data-ttu-id="21894-120">**Otevřením** tohoto souboru se připojíte ke svému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="21894-120">**Open** this file to connect to your VM.</span></span>
   
    ![Snímek obrazovky webu Azure Portal znázorňující způsob připojení k virtuálnímu počítači](./media/hero-role/connect.png)
3. <span data-ttu-id="21894-122">Zobrazí se upozornění, že soubor .rdp je od neznámého vydavatele.</span><span class="sxs-lookup"><span data-stu-id="21894-122">You get a warning that the .rdp is from an unknown publisher.</span></span> <span data-ttu-id="21894-123">To je normální.</span><span class="sxs-lookup"><span data-stu-id="21894-123">This is normal.</span></span> <span data-ttu-id="21894-124">Pokračujte kliknutím na **Připojit** v okně Připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="21894-124">In the Remote Desktop window, click **Connect** to continue.</span></span>
   
    ![Snímek obrazovky s upozorněním na neznámého vydavatele](./media/hero-role/rdp-warn.png)
4. <span data-ttu-id="21894-126">V okně zabezpečení systému Windows zadejte uživatelské jméno a heslo pro místní účet, který jste vytvořili při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="21894-126">In the Windows Security window, type the username and password for the local account that you created when you created the VM.</span></span> <span data-ttu-id="21894-127">Zadejte uživatelské jméno v této podobě: *název_virtuálního_počítače*&#92;*uživatelské_jméno* a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="21894-127">The username is entered as *vmname*&#92;*username*, then click **OK**.</span></span>
   
    ![Snímek obrazovky zadání názvu virtuálního počítače, uživatelské jméno a heslo](./media/hero-role/credentials.png)
5. <span data-ttu-id="21894-129">Zobrazí se upozornění, že certifikát nelze ověřit.</span><span class="sxs-lookup"><span data-stu-id="21894-129">You get a warning that the certificate cannot be verified.</span></span> <span data-ttu-id="21894-130">To je normální.</span><span class="sxs-lookup"><span data-stu-id="21894-130">This is normal.</span></span> <span data-ttu-id="21894-131">Kliknutím na **Ano** ověřte identitu virtuálního počítače a dokončete přihlášení.</span><span class="sxs-lookup"><span data-stu-id="21894-131">Click **Yes** to verify the identity of the virtual machine and finish logging on.</span></span>
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity virtuálního počítače](./media/hero-role/cert-warning.png)

<span data-ttu-id="21894-133">Pokud budete mít s připojením problémy, projděte si téma [Poradce při potížích s připojením k virtuálnímu počítači s Windows v Azure pomocí Vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21894-133">If you run in to trouble when you try to connect, see [Troubleshoot Remote Desktop connections to a Windows-based Azure Virtual Machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="install-iis-on-your-vm"></a><span data-ttu-id="21894-134">Instalace služby IIS na vašem virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="21894-134">Install IIS on your VM</span></span>
<span data-ttu-id="21894-135">Teď jste přihlášení k virtuálnímu počítači a můžeme nainstalovat role serveru, abyste mohli více experimentovat.</span><span class="sxs-lookup"><span data-stu-id="21894-135">Now that you are logged in to the VM, we will install a server role so that you can experiment more.</span></span>

1. <span data-ttu-id="21894-136">Otevřete **Správce serveru**, pokud ještě není otevřený.</span><span class="sxs-lookup"><span data-stu-id="21894-136">Open **Server Manager** if it isn't already open.</span></span> <span data-ttu-id="21894-137">Klikněte na tlačítko **spustit** nabídce a pak klikněte na tlačítko **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="21894-137">Click the **Start** menu, and then click **Server Manager**.</span></span>
2. <span data-ttu-id="21894-138">V okně **Správce serveru** vyberte v levém podokně **Místní server**.</span><span class="sxs-lookup"><span data-stu-id="21894-138">In **Server Manager**, select **Local Server** from the left pane.</span></span> 
3. <span data-ttu-id="21894-139">V nabídce vyberte **Spravovat** > **Přidat role a funkce**.</span><span class="sxs-lookup"><span data-stu-id="21894-139">In the menu, select **Manage** > **Add Roles and Features**.</span></span>
4. <span data-ttu-id="21894-140">Přidat role a funkce Průvodce na **typ instalace** vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21894-140">In the Add Roles and Features Wizard, on the **Installation Type** page, choose **Role-based or feature-based installation**, and then click **Next**.</span></span>
   
    ![Snímek obrazovky ukazující na kartě Průvodce přidáním rolí a funkcí pro typ instalace](./media/hero-role/role-wizard.png)
5. <span data-ttu-id="21894-142">Vyberte virtuální počítač ve fondu serverů a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21894-142">Select the VM from the server pool and click **Next**.</span></span>
6. <span data-ttu-id="21894-143">Na stránce **Role serveru** vyberte **Webový server (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="21894-143">On the **Server Roles** page, select **Web Server (IIS)**.</span></span>
   
    ![Snímek obrazovky ukazující na kartě Průvodce přidáním rolí a funkcí pro role serveru](./media/hero-role/add-iis.png)
7. <span data-ttu-id="21894-145">V místním okně pro přidávání funkcí potřebných pro službu IIS ověřte, že je zaškrtnuté **Zahrnout nástroje pro správu** , a potom klikněte na **Přidat funkce**.</span><span class="sxs-lookup"><span data-stu-id="21894-145">In the pop-up about adding features needed for IIS, make sure that **Include management tools** is selected and then click **Add Features**.</span></span> <span data-ttu-id="21894-146">Po zavření místního okna klikněte v průvodci na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21894-146">When the pop-up closes, click **Next** in the wizard.</span></span>
   
    ![Snímek obrazovky ukazující, automaticky otevírané okno pro potvrzení přidání role služby IIS](./media/hero-role/confirm-add-feature.png)
8. <span data-ttu-id="21894-148">Na stránce funkcí klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21894-148">On the features page, click **Next**.</span></span>
9. <span data-ttu-id="21894-149">Na stránce **Web Server Role (IIS)** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21894-149">On the **Web Server Role (IIS)** page, click **Next**.</span></span> 
10. <span data-ttu-id="21894-150">Na stránce **Služby rolí** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21894-150">On the **Role Services** page, click **Next**.</span></span> 
11. <span data-ttu-id="21894-151">Na stránce **Potvrzení** klikněte na **Instalovat**.</span><span class="sxs-lookup"><span data-stu-id="21894-151">On the **Confirmation** page, click **Install**.</span></span> 
12. <span data-ttu-id="21894-152">Po dokončení instalace klikněte v průvodci na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="21894-152">When the installation is complete, click **Close** on the wizard.</span></span>

## <a name="open-port-80"></a><span data-ttu-id="21894-153">Otevření portu 80</span><span class="sxs-lookup"><span data-stu-id="21894-153">Open port 80</span></span>
<span data-ttu-id="21894-154">Aby váš virtuální počítač přijímal příchozí přenos přes port 80, musíte do skupiny zabezpečení sítě přidat příchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="21894-154">In order for your VM to accept inbound traffic over port 80, you need to add an inbound rule to the network security group.</span></span> 

1. <span data-ttu-id="21894-155">Otevřete web [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="21894-155">Open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="21894-156">V **virtuální počítače** vyberte virtuální počítač, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="21894-156">In **Virtual machines** select the VM that you created.</span></span>
3. <span data-ttu-id="21894-157">V nastavení virtuálního počítače, vyberte **síťových rozhraní** a pak vyberte do existujícího síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="21894-157">In the virtual machines settings, select **Network interfaces** and then select the existing network interface.</span></span>
   
    ![Snímek obrazovky nastavení virtuálního počítače pro rozhraní sítě](./media/hero-role/network-interface.png)
4. <span data-ttu-id="21894-159">V **Essentials** síťového rozhraní, klikněte na **skupinu zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="21894-159">In **Essentials** for the network interface, click the **Network security group**.</span></span>
   
    ![Snímek obrazovky zobrazující části základní údaje pro síťové rozhraní](./media/hero-role/select-nsg.png)
5. <span data-ttu-id="21894-161">V okně **Základní údaje** pro NSG by mělo být jedno výchozí příchozí rule pro **default-allow-rdp**, které umožňuje přihlásit se k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="21894-161">In the **Essentials** blade for the NSG, you should have one existing default inbound rule for **default-allow-rdp** which allows you to log in to the VM.</span></span> <span data-ttu-id="21894-162">Teď přidáte další pravidlo, které povolí provoz IIS.</span><span class="sxs-lookup"><span data-stu-id="21894-162">You will add another inbound rule to allow IIS traffic.</span></span> <span data-ttu-id="21894-163">Klikněte na **Příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="21894-163">Click **Inbound security rule**.</span></span>
   
    ![Snímek obrazovky zobrazující části základní údaje skupiny nsg](./media/hero-role/inbound.png)
6. <span data-ttu-id="21894-165">V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="21894-165">In **Inbound security rules**, click **Add**.</span></span>
   
    ![Snímek obrazovky tlačítka Přidat pravidlo zabezpečení](./media/hero-role/add-rule.png)
7. <span data-ttu-id="21894-167">V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="21894-167">In **Inbound security rules**, click **Add**.</span></span> <span data-ttu-id="21894-168">Do pole pro rozsah portů zadejte **80** a zkontrolujte, že **Povolit** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="21894-168">Type **80** in the port range and make sure **Allow** is selected.</span></span> <span data-ttu-id="21894-169">Až to budete mít, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="21894-169">When you are done, click **OK**.</span></span>
   
    ![Snímek obrazovky tlačítka Přidat pravidlo zabezpečení](./media/hero-role/port-80.png)

<span data-ttu-id="21894-171">Další informace o skupinách NSG a příchozích a odchozích pravidlech najdete v tématu [Povolení externího přístupu k virtuálnímu počítači pomocí webu Azure Portal](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="21894-171">For more information about NSGs, inbound and outbound rules, see [Allow external access to your VM using the Azure portal](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="connect-to-the-default-iis-website"></a><span data-ttu-id="21894-172">Připojení k výchozímu webu IIS</span><span class="sxs-lookup"><span data-stu-id="21894-172">Connect to the default IIS website</span></span>
1. <span data-ttu-id="21894-173">Na webu Azure Portal klikněte na **Virtuální počítače** a vyberte svůj virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="21894-173">In the Azure portal, click **Virtual machines** and then select your VM.</span></span>
2. <span data-ttu-id="21894-174">V okně **Základní údaje** zkopírujte **veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="21894-174">In the **Essentials** blade, copy your **Public IP address**.</span></span>
   
    ![Snímek obrazovky ukazující, kde najít veřejnou IP adresu vašeho virtuálního počítače](./media/hero-role/ipaddress.png)
3. <span data-ttu-id="21894-176">Otevřete prohlížeč, do adresního řádku zadejte svou veřejnou IP adresu, která vypadá takto: http://<publicIPaddress>, a kliknutím na **Enter** přejděte na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="21894-176">Open a browser and in the address bar, type in your public IP address like this: http://<publicIPaddress> and click **Enter** to go to that address.</span></span>
4. <span data-ttu-id="21894-177">Váš prohlížeč by měla otevřít výchozí webové stránky služby IIS.</span><span class="sxs-lookup"><span data-stu-id="21894-177">Your browser should open the default IIS web page.</span></span> <span data-ttu-id="21894-178">Vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="21894-178">It looks something like this:</span></span>
   
    ![Snímek obrazovky ukazující, jak výchozí stránka služby IIS vypadá v prohlížeči](./media/hero-role/iis-default.png)

## <a name="next-steps"></a><span data-ttu-id="21894-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21894-180">Next steps</span></span>
* <span data-ttu-id="21894-181">Také můžete experimentovat s [připojením datového disku](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) k vašemu virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="21894-181">You can also experiment with [attaching a data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to your virtual machine.</span></span> <span data-ttu-id="21894-182">Pomocí datových disků si můžete rozšířit úložiště pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="21894-182">Data disks provide more storage for your virtual machine.</span></span>

