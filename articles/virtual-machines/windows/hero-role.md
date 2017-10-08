---
title: "aaaInstall IIS na vaše první virtuální počítač s Windows | Microsoft Docs"
description: "Experimentujte s první virtuální počítač s Windows pomocí instalace služby IIS a otevření portu 80 pomocí hello portálu Azure."
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
ms.openlocfilehash: 7cfed6197df058c4569d111ee88961da7c6fe0b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a><span data-ttu-id="87aa7-103">Experimentujte s instalaci role na vašem virtuálním počítači Windows</span><span class="sxs-lookup"><span data-stu-id="87aa7-103">Experiment with installing a role on your Windows VM</span></span>
<span data-ttu-id="87aa7-104">Až budete mít první virtuální počítač (VM) nahoru a spuštěna, můžete přesunout na tooinstalling softwaru a služeb.</span><span class="sxs-lookup"><span data-stu-id="87aa7-104">Once you have your first virtual machine (VM) up and running, you can move on tooinstalling software and services.</span></span> <span data-ttu-id="87aa7-105">V tomto kurzu budeme toouse správce serveru na virtuální počítač Windows serveru tooinstall hello služby IIS.</span><span class="sxs-lookup"><span data-stu-id="87aa7-105">For this tutorial, we are going toouse Server Manager on hello Windows Server VM tooinstall IIS.</span></span> <span data-ttu-id="87aa7-106">Poté vytvoříme skupinu zabezpečení sítě (NSG) pomocí hello Azure portálu tooopen port 80 tooIIS provoz.</span><span class="sxs-lookup"><span data-stu-id="87aa7-106">Then, we will create a Network Security Group (NSG) using hello Azure portal tooopen port 80 tooIIS traffic.</span></span> 

<span data-ttu-id="87aa7-107">Pokud jste ještě nevytvořili vaše první virtuální počítač, přejděte zpět příliš[vytvořit svůj první virtuální počítač Windows v hello portál Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před pokračováním v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="87aa7-107">If you haven't already created your first VM, you should go back too[Create your first Windows virtual machine in hello Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before continuing with this tutorial.</span></span>

## <a name="make-sure-hello-vm-is-running"></a><span data-ttu-id="87aa7-108">Ujistěte se, zda text hello, který je spuštěný virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="87aa7-108">Make sure hello VM is running</span></span>
1. <span data-ttu-id="87aa7-109">Otevřete hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="87aa7-109">Open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="87aa7-110">V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-110">On hello hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="87aa7-111">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="87aa7-111">Select hello virtual machine from hello list.</span></span>
3. <span data-ttu-id="87aa7-112">Pokud je stav hello **zastaveno (Deallocated)**, klikněte na tlačítko hello **spustit** na hello tlačítko **Essentials** okno hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="87aa7-112">If hello status is **Stopped (Deallocated)**, click hello **Start** button on hello **Essentials** blade of hello VM.</span></span> <span data-ttu-id="87aa7-113">Pokud je stav hello **systémem**, můžete přesunout na další krok toohello.</span><span class="sxs-lookup"><span data-stu-id="87aa7-113">If hello status is **Running**, you can move on toohello next step.</span></span>

## <a name="connect-toohello-virtual-machine-and-sign-in"></a><span data-ttu-id="87aa7-114">Připojit toohello virtuálního počítače a přihlášení</span><span class="sxs-lookup"><span data-stu-id="87aa7-114">Connect toohello virtual machine and sign in</span></span>
1. <span data-ttu-id="87aa7-115">V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-115">On hello hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="87aa7-116">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="87aa7-116">Select hello virtual machine from hello list.</span></span>
2. <span data-ttu-id="87aa7-117">V okně hello hello virtuálního počítače, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-117">On hello blade for hello virtual machine, click **Connect**.</span></span> <span data-ttu-id="87aa7-118">Tím se vytvoří a stáhne soubor Remote Desktop Protocol (soubor .rdp), který je třeba na počítači tooyour tooconnect zástupce.</span><span class="sxs-lookup"><span data-stu-id="87aa7-118">This creates and downloads a Remote Desktop Protocol file (.rdp file) that is like a shortcut tooconnect tooyour machine.</span></span> <span data-ttu-id="87aa7-119">Můžete chtít toosave hello souboru tooyour desktop pro snadný přístup.</span><span class="sxs-lookup"><span data-stu-id="87aa7-119">You might want toosave hello file tooyour desktop for easy access.</span></span> <span data-ttu-id="87aa7-120">**Otevřete** tooyour tooconnect tento soubor virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="87aa7-120">**Open** this file tooconnect tooyour VM.</span></span>
   
    ![Snímek obrazovky portálu Azure znázorňující hello jak tooconnect tooyour virtuálních počítačů](./media/hero-role/connect.png)
3. <span data-ttu-id="87aa7-122">Se zobrazí upozornění, že hello RDP je od neznámého vydavatele.</span><span class="sxs-lookup"><span data-stu-id="87aa7-122">You get a warning that hello .rdp is from an unknown publisher.</span></span> <span data-ttu-id="87aa7-123">To je normální.</span><span class="sxs-lookup"><span data-stu-id="87aa7-123">This is normal.</span></span> <span data-ttu-id="87aa7-124">V okně hello vzdálené plochy, klikněte na tlačítko **připojit** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="87aa7-124">In hello Remote Desktop window, click **Connect** toocontinue.</span></span>
   
    ![Snímek obrazovky s upozorněním na neznámého vydavatele](./media/hero-role/rdp-warn.png)
4. <span data-ttu-id="87aa7-126">V okně zabezpečení systému Windows hello hello typ hello uživatelské jméno a heslo pro hello místní účet, který jste vytvořili při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="87aa7-126">In hello Windows Security window, type hello username and password for hello local account that you created when you created hello VM.</span></span> <span data-ttu-id="87aa7-127">uživatelské jméno Hello je zadán jako *vmname*&#92; *uživatelské jméno*, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-127">hello username is entered as *vmname*&#92;*username*, then click **OK**.</span></span>
   
    ![Snímek obrazovky zadání hello název virtuálního počítače, uživatelské jméno a heslo](./media/hero-role/credentials.png)
5. <span data-ttu-id="87aa7-129">Zobrazí se upozornění certifikátu hello nelze ověřit.</span><span class="sxs-lookup"><span data-stu-id="87aa7-129">You get a warning that hello certificate cannot be verified.</span></span> <span data-ttu-id="87aa7-130">To je normální.</span><span class="sxs-lookup"><span data-stu-id="87aa7-130">This is normal.</span></span> <span data-ttu-id="87aa7-131">Klikněte na tlačítko **Ano** tooverify hello identity hello virtuální počítač a dokončete přihlášení.</span><span class="sxs-lookup"><span data-stu-id="87aa7-131">Click **Yes** tooverify hello identity of hello virtual machine and finish logging on.</span></span>
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity hello hello virtuálních počítačů](./media/hero-role/cert-warning.png)

<span data-ttu-id="87aa7-133">Pokud spustíte tootrouble při pokusu o tooconnect, najdete v části [tooa připojení řešení Vzdálená plocha systému Windows virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="87aa7-133">If you run in tootrouble when you try tooconnect, see [Troubleshoot Remote Desktop connections tooa Windows-based Azure Virtual Machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="install-iis-on-your-vm"></a><span data-ttu-id="87aa7-134">Instalace služby IIS na vašem virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="87aa7-134">Install IIS on your VM</span></span>
<span data-ttu-id="87aa7-135">Teď, když jste přihlášení toohello virtuálních počítačů, se nainstaluje role serveru, aby můžete experimentovat více.</span><span class="sxs-lookup"><span data-stu-id="87aa7-135">Now that you are logged in toohello VM, we will install a server role so that you can experiment more.</span></span>

1. <span data-ttu-id="87aa7-136">Otevřete **Správce serveru**, pokud ještě není otevřený.</span><span class="sxs-lookup"><span data-stu-id="87aa7-136">Open **Server Manager** if it isn't already open.</span></span> <span data-ttu-id="87aa7-137">Klikněte na tlačítko hello **spustit** nabídce a pak klikněte na tlačítko **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-137">Click hello **Start** menu, and then click **Server Manager**.</span></span>
2. <span data-ttu-id="87aa7-138">V **správce serveru**, vyberte **místní Server** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="87aa7-138">In **Server Manager**, select **Local Server** from hello left pane.</span></span> 
3. <span data-ttu-id="87aa7-139">V nabídce hello vyberte **spravovat** > **přidat role a funkce**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-139">In hello menu, select **Manage** > **Add Roles and Features**.</span></span>
4. <span data-ttu-id="87aa7-140">V hello přidat role a funkce Průvodce hello **typ instalace** vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-140">In hello Add Roles and Features Wizard, on hello **Installation Type** page, choose **Role-based or feature-based installation**, and then click **Next**.</span></span>
   
    ![Snímek obrazovky znázorňující hello Průvodce přidáním rolí a funkcí kartu pro typ instalace](./media/hero-role/role-wizard.png)
5. <span data-ttu-id="87aa7-142">Vyberte hello virtuálních počítačů z fondu serverů hello a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-142">Select hello VM from hello server pool and click **Next**.</span></span>
6. <span data-ttu-id="87aa7-143">Na hello **role serveru** vyberte **webového serveru (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-143">On hello **Server Roles** page, select **Web Server (IIS)**.</span></span>
   
    ![Snímek obrazovky znázorňující hello Průvodce přidáním rolí a funkcí kartu pro role serveru](./media/hero-role/add-iis.png)
7. <span data-ttu-id="87aa7-145">V hello automaticky otevírané okno o přidávání funkcí, které jsou potřebné pro službu IIS, ujistěte se, že **zahrnout nástroje pro správu** je vybrána a potom klikněte na **přidat funkce**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-145">In hello pop-up about adding features needed for IIS, make sure that **Include management tools** is selected and then click **Add Features**.</span></span> <span data-ttu-id="87aa7-146">Po hello automaticky otevírané okno se zavře a klikněte na tlačítko **Další** v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="87aa7-146">When hello pop-up closes, click **Next** in hello wizard.</span></span>
   
    ![Snímek obrazovky ukazující, automaticky otevírané okno tooconfirm přidání hello IIS role](./media/hero-role/confirm-add-feature.png)
8. <span data-ttu-id="87aa7-148">Na stránce funkce hello, klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-148">On hello features page, click **Next**.</span></span>
9. <span data-ttu-id="87aa7-149">Na hello **Role webového serveru (IIS)** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-149">On hello **Web Server Role (IIS)** page, click **Next**.</span></span> 
10. <span data-ttu-id="87aa7-150">Na hello **služby rolí** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-150">On hello **Role Services** page, click **Next**.</span></span> 
11. <span data-ttu-id="87aa7-151">Na hello **potvrzení** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-151">On hello **Confirmation** page, click **Install**.</span></span> 
12. <span data-ttu-id="87aa7-152">Po dokončení instalace hello klikněte na tlačítko **Zavřít** v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="87aa7-152">When hello installation is complete, click **Close** on hello wizard.</span></span>

## <a name="open-port-80"></a><span data-ttu-id="87aa7-153">Otevření portu 80</span><span class="sxs-lookup"><span data-stu-id="87aa7-153">Open port 80</span></span>
<span data-ttu-id="87aa7-154">V pořadí pro váš počítač tooaccept příchozí provoz přes port 80, je třeba tooadd skupinu zabezpečení sítě toohello příchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="87aa7-154">In order for your VM tooaccept inbound traffic over port 80, you need tooadd an inbound rule toohello network security group.</span></span> 

1. <span data-ttu-id="87aa7-155">Otevřete hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="87aa7-155">Open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="87aa7-156">V **virtuální počítače** hello vyberte virtuální počítač, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="87aa7-156">In **Virtual machines** select hello VM that you created.</span></span>
3. <span data-ttu-id="87aa7-157">V nastavení hello virtuální počítače, vyberte **síťových rozhraní** a pak vyberte hello existujícího síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="87aa7-157">In hello virtual machines settings, select **Network interfaces** and then select hello existing network interface.</span></span>
   
    ![Snímek obrazovky zobrazující hello nastavení virtuálního počítače pro hello síťová rozhraní](./media/hero-role/network-interface.png)
4. <span data-ttu-id="87aa7-159">V **Essentials** hello síťového rozhraní, klikněte na hello **skupinu zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-159">In **Essentials** for hello network interface, click hello **Network security group**.</span></span>
   
    ![Snímek obrazovky zobrazující hello Essentials části pro síťové rozhraní hello](./media/hero-role/select-nsg.png)
5. <span data-ttu-id="87aa7-161">V hello **Essentials** okně hello NSG, měli byste mít jeden existující výchozí pravidlo pro příchozí **výchozí povolit rdp** který vám umožní toolog v toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="87aa7-161">In hello **Essentials** blade for hello NSG, you should have one existing default inbound rule for **default-allow-rdp** which allows you toolog in toohello VM.</span></span> <span data-ttu-id="87aa7-162">Přidejte jiný příchozí pravidlo tooallow IIS provoz.</span><span class="sxs-lookup"><span data-stu-id="87aa7-162">You will add another inbound rule tooallow IIS traffic.</span></span> <span data-ttu-id="87aa7-163">Klikněte na **Příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-163">Click **Inbound security rule**.</span></span>
   
    ![Snímek obrazovky zobrazující hello Essentials části hello NSG](./media/hero-role/inbound.png)
6. <span data-ttu-id="87aa7-165">V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-165">In **Inbound security rules**, click **Add**.</span></span>
   
    ![Snímek obrazovky zobrazující hello tlačítko tooadd pravidlo zabezpečení](./media/hero-role/add-rule.png)
7. <span data-ttu-id="87aa7-167">V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-167">In **Inbound security rules**, click **Add**.</span></span> <span data-ttu-id="87aa7-168">Typ **80** v hello rozsah portů a zajistěte, aby **povolit** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="87aa7-168">Type **80** in hello port range and make sure **Allow** is selected.</span></span> <span data-ttu-id="87aa7-169">Až to budete mít, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-169">When you are done, click **OK**.</span></span>
   
    ![Snímek obrazovky zobrazující hello tlačítko tooadd pravidlo zabezpečení](./media/hero-role/port-80.png)

<span data-ttu-id="87aa7-171">Další informace o skupinách Nsg, příchozí a odchozí pravidla, najdete v části [povolit externí přístup tooyour virtuální počítač pomocí hello portálu Azure](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="87aa7-171">For more information about NSGs, inbound and outbound rules, see [Allow external access tooyour VM using hello Azure portal](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="connect-toohello-default-iis-website"></a><span data-ttu-id="87aa7-172">Připojit toohello výchozí web služby IIS</span><span class="sxs-lookup"><span data-stu-id="87aa7-172">Connect toohello default IIS website</span></span>
1. <span data-ttu-id="87aa7-173">V hello portálu Azure, klikněte na **virtuální počítače** a potom vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="87aa7-173">In hello Azure portal, click **Virtual machines** and then select your VM.</span></span>
2. <span data-ttu-id="87aa7-174">V hello **Essentials** zkopírujte vaše **veřejnou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="87aa7-174">In hello **Essentials** blade, copy your **Public IP address**.</span></span>
   
    ![Snímek obrazovky ukazující, kde toofind hello veřejnou IP adresu vašeho virtuálního počítače](./media/hero-role/ipaddress.png)
3. <span data-ttu-id="87aa7-176">Otevřete prohlížeč a v panelu Adresa hello, zadejte svoji veřejnou IP adresu takto: http://<publicIPaddress> a klikněte na tlačítko **Enter** toogo toothat adresu.</span><span class="sxs-lookup"><span data-stu-id="87aa7-176">Open a browser and in hello address bar, type in your public IP address like this: http://<publicIPaddress> and click **Enter** toogo toothat address.</span></span>
4. <span data-ttu-id="87aa7-177">Váš prohlížeč by měla otevřít hello výchozí IIS webové stránky.</span><span class="sxs-lookup"><span data-stu-id="87aa7-177">Your browser should open hello default IIS web page.</span></span> <span data-ttu-id="87aa7-178">Vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="87aa7-178">It looks something like this:</span></span>
   
    ![Snímek obrazovky se stránkou jaké hello výchozí IIS vypadá v prohlížeči](./media/hero-role/iis-default.png)

## <a name="next-steps"></a><span data-ttu-id="87aa7-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87aa7-180">Next steps</span></span>
* <span data-ttu-id="87aa7-181">Také můžete experimentovat s [připojením datového disku](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="87aa7-181">You can also experiment with [attaching a data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtual machine.</span></span> <span data-ttu-id="87aa7-182">Pomocí datových disků si můžete rozšířit úložiště pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="87aa7-182">Data disks provide more storage for your virtual machine.</span></span>

