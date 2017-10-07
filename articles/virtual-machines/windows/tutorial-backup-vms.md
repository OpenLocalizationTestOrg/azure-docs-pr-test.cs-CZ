---
title: "aaaBackup virtuálních počítačích Windows Azure | Microsoft Docs"
description: "Virtuální počítače Windows Chraňte pomocí zálohování pomocí služby Azure Backup."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1cd3e1940a83aacd160cba3c8613b63b6f3c11d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a><span data-ttu-id="851fa-103">Zálohovat virtuální počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="851fa-103">Back up Windows virtual machines in Azure</span></span>

<span data-ttu-id="851fa-104">Svá data můžete chránit prováděním záloh v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="851fa-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="851fa-105">Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů.</span><span class="sxs-lookup"><span data-stu-id="851fa-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="851fa-106">Při obnovení z bodu obnovení můžete obnovit hello celý virtuální počítač nebo jenom určité soubory.</span><span class="sxs-lookup"><span data-stu-id="851fa-106">When you restore from a recovery point, you can restore hello whole VM or just specific files.</span></span> <span data-ttu-id="851fa-107">Tento článek vysvětluje, jak toorestore jednoho souboru tooa virtuální počítač spuštěný Windows Server a službu IIS.</span><span class="sxs-lookup"><span data-stu-id="851fa-107">This article explains how toorestore a single file tooa VM running Windows Server and IIS.</span></span> <span data-ttu-id="851fa-108">Pokud ještě nemáte toouse virtuálních počítačů, můžete jeden vytvořit pomocí hello [rychlé spuštění Windows](quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="851fa-108">If you don't already have a VM toouse, you can create one using hello [Windows quickstart](quick-create-portal.md).</span></span> <span data-ttu-id="851fa-109">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="851fa-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="851fa-110">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="851fa-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="851fa-111">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="851fa-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="851fa-112">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="851fa-112">Restore a file from a backup</span></span>




## <a name="backup-overview"></a><span data-ttu-id="851fa-113">Přehled služby Backup</span><span class="sxs-lookup"><span data-stu-id="851fa-113">Backup overview</span></span>

<span data-ttu-id="851fa-114">V případě hello služba Azure Backup zahájí úlohu zálohování, aktivuje hello rozšíření zálohování tootake snímku v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="851fa-114">When hello Azure Backup service initiates a backup job, it triggers hello backup extension tootake a point-in-time snapshot.</span></span> <span data-ttu-id="851fa-115">Hello služby Azure Backup používá hello _VMSnapshot_ rozšíření.</span><span class="sxs-lookup"><span data-stu-id="851fa-115">hello Azure Backup service uses hello _VMSnapshot_ extension.</span></span> <span data-ttu-id="851fa-116">rozšíření Hello je nainstalován během hello první zálohování virtuálních počítačů, pokud běží hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="851fa-116">hello extension is installed during hello first VM backup if hello VM is running.</span></span> <span data-ttu-id="851fa-117">Pokud hello virtuální počítač není spuštěný, hello služby zálohování pořídí snímek hello základní úložiště (protože žádné zápisy aplikace dochází při hello zastavení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="851fa-117">If hello VM is not running, hello Backup service takes a snapshot of hello underlying storage (since no application writes occur while hello VM is stopped).</span></span>

<span data-ttu-id="851fa-118">Při pořizování snímku virtuálních počítačů Windows, služba Backup hello koordinuje pomocí služby Stínová kopie svazku (VSS) tooget hello konzistentního snímku disky hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="851fa-118">When taking a snapshot of Windows VMs, hello Backup service coordinates with hello Volume Shadow Copy Service (VSS) tooget a consistent snapshot of hello virtual machine's disks.</span></span> <span data-ttu-id="851fa-119">Jakmile hello služby Azure Backup používá hello snímku, je hello data přenášená toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="851fa-119">Once hello Azure Backup service takes hello snapshot, hello data is transferred toohello vault.</span></span> <span data-ttu-id="851fa-120">toomaximize efektivitu, služba hello identifikuje a přenáší pouze hello bloky dat, které se změnily od hello předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="851fa-120">toomaximize efficiency, hello service identifies and transfers only hello blocks of data that have changed since hello previous backup.</span></span>

<span data-ttu-id="851fa-121">Po dokončení přenosu dat hello hello snímek odebrán a vytvoří bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="851fa-121">When hello data transfer is complete, hello snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="851fa-122">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="851fa-122">Create a backup</span></span>
<span data-ttu-id="851fa-123">Vytvořte jednoduchý naplánované denní zálohování tooa trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="851fa-123">Create a simple scheduled daily backup tooa Recovery Services Vault.</span></span> 

1. <span data-ttu-id="851fa-124">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="851fa-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="851fa-125">V nabídce hello na levé straně hello vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="851fa-125">In hello menu on hello left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="851fa-126">Hello seznamu vyberte virtuální počítač tooback nahoru.</span><span class="sxs-lookup"><span data-stu-id="851fa-126">From hello list, select a VM tooback up.</span></span>
4. <span data-ttu-id="851fa-127">V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="851fa-127">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="851fa-128">Hello **povolit zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="851fa-128">hello **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="851fa-129">V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název hello hello nový trezor.</span><span class="sxs-lookup"><span data-stu-id="851fa-129">In **Recovery Services vault**, click **Create new** and provide hello name for hello new vault.</span></span> <span data-ttu-id="851fa-130">Nový trezor se vytvoří v hello stejnou skupinu prostředků a umístění jako virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="851fa-130">A new vault is created in hello same Resource Group and location as hello virtual machine.</span></span>
6. <span data-ttu-id="851fa-131">Klikněte na tlačítko **zálohování zásad**.</span><span class="sxs-lookup"><span data-stu-id="851fa-131">Click **Backup policy**.</span></span> <span data-ttu-id="851fa-132">V tomto příkladu zachovat hello výchozí hodnoty a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="851fa-132">For this example, keep hello defaults and click **OK**.</span></span>
7. <span data-ttu-id="851fa-133">Na hello **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**.</span><span class="sxs-lookup"><span data-stu-id="851fa-133">On hello **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="851fa-134">Tím se vytvoří denní zálohování podle plánu výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="851fa-134">This creates a daily backup based on hello default schedule.</span></span>
10. <span data-ttu-id="851fa-135">toocreate bod počáteční obnovení na hello **zálohování** okně klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="851fa-135">toocreate an initial recovery point, on hello **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="851fa-136">Na hello **zálohovat nyní** okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="851fa-136">On hello **Backup Now** blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="851fa-137">V hello **zálohování** okno pro virtuální počítač, zobrazí se hello počet bodů obnovení, které jsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="851fa-137">In hello **Backup** blade for your VM, you will see hello number of recovery points that are complete.</span></span>

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)
    
<span data-ttu-id="851fa-139">první zálohy Hello trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="851fa-139">hello first backup takes about 20 minutes.</span></span> <span data-ttu-id="851fa-140">Po dokončení zálohování, pokračujte toohello další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="851fa-140">Proceed toohello next part of this tutorial after your backup is finished.</span></span>

## <a name="recover-a-file"></a><span data-ttu-id="851fa-141">Obnovit soubor</span><span class="sxs-lookup"><span data-stu-id="851fa-141">Recover a file</span></span>

<span data-ttu-id="851fa-142">Pokud omylem odstraníte nebo aby se změny tooa soubor, můžete použít soubor hello toorecover obnovení souborů z trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="851fa-142">If you accidentally delete or make changes tooa file, you can use File Recovery toorecover hello file from your backup vault.</span></span> <span data-ttu-id="851fa-143">Obnovení souborů používá skript, který běží na hello virtuálních počítačů, bod obnovení hello toomount jako místní disk.</span><span class="sxs-lookup"><span data-stu-id="851fa-143">File Recovery uses a script that runs on hello VM, toomount hello recovery point as local drive.</span></span> <span data-ttu-id="851fa-144">Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení hello a obnovit je toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="851fa-144">These drives will remain mounted for 12 hours so that you can copy files from hello recovery point and restore them toohello VM.</span></span>  

<span data-ttu-id="851fa-145">V tomto příkladu ukážeme, jak toorecover hello soubor bitové kopie, který se používá v hello výchozí webové stránky pro službu IIS.</span><span class="sxs-lookup"><span data-stu-id="851fa-145">In this example, we show how toorecover hello image file that is used in hello default web page for IIS.</span></span> 

1. <span data-ttu-id="851fa-146">Otevřete prohlížeč a připojte toohello IP adresu hello virtuálních počítačů tooshow hello výchozí IIS stránky.</span><span class="sxs-lookup"><span data-stu-id="851fa-146">Open a browser and connect toohello IP address of hello VM tooshow hello default IIS page.</span></span>

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-working.png)

2. <span data-ttu-id="851fa-148">Připojte toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="851fa-148">Connect toohello VM.</span></span>
3. <span data-ttu-id="851fa-149">Na hello virtuálních počítačů, otevřete **Průzkumníka souborů** přejděte too\inetpub\wwwroot a odstranit soubor hello **iisstart.png**.</span><span class="sxs-lookup"><span data-stu-id="851fa-149">On hello VM, open **File Explorer** and navigate too\inetpub\wwwroot and delete hello file **iisstart.png**.</span></span>
4. <span data-ttu-id="851fa-150">V místním počítači je pryč toosee aktualizace hello prohlížeče, který hello bitové kopie na stránce služby IIS výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="851fa-150">On your local computer, refresh hello browser toosee that hello image on hello default IIS page is gone.</span></span>

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-broken.png)

5. <span data-ttu-id="851fa-152">V místním počítači, otevřete novou kartu a přejděte hello hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="851fa-152">On your local computer, open a new tab and go hello hello [Azure portal](https://portal.azure.com).</span></span>
6. <span data-ttu-id="851fa-153">V nabídce hello na levé straně hello vyberte **virtuální počítače** a seznam hello formuláře vyberte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="851fa-153">In hello menu on hello left, select **Virtual machines** and select hello VM form hello list.</span></span>
8. <span data-ttu-id="851fa-154">V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="851fa-154">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="851fa-155">Hello **zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="851fa-155">hello **Backup** blade opens.</span></span> 
9. <span data-ttu-id="851fa-156">V nabídce hello hello horní části okna hello vyberte **obnovení souboru**.</span><span class="sxs-lookup"><span data-stu-id="851fa-156">In hello menu at hello top of hello blade, select **File Recovery**.</span></span> <span data-ttu-id="851fa-157">Hello **obnovení souboru** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="851fa-157">hello **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="851fa-158">V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="851fa-158">In **Step 1: Select recovery point**, select a recovery point from hello drop-down.</span></span>
11. <span data-ttu-id="851fa-159">V **krok 2: stáhnout skript toobrowse a obnovit soubory**, klikněte na tlačítko hello **spustitelný soubor stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="851fa-159">In **Step 2: Download script toobrowse and recover files**, click hello **Download Executable** button.</span></span> <span data-ttu-id="851fa-160">Uložte soubor tooyour hello **stáhne** složky.</span><span class="sxs-lookup"><span data-stu-id="851fa-160">Save hello file tooyour **Downloads** folder.</span></span>
12. <span data-ttu-id="851fa-161">V místním počítači, otevřete **Průzkumníka souborů** a přejděte tooyour **stáhne** hello složky a zkopírujte stažený soubor .exe.</span><span class="sxs-lookup"><span data-stu-id="851fa-161">On your local computer, open **File Explorer** and navigate tooyour **Downloads** folder and copy hello downloaded .exe file.</span></span> <span data-ttu-id="851fa-162">Název souboru Hello bude obsahovat předponu název vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="851fa-162">hello filename will be prefixed by your VM name.</span></span> 
13. <span data-ttu-id="851fa-163">Na vašem virtuálním počítači (přes hello připojení RDP) vložte toohello souboru .exe hello ploše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="851fa-163">On your VM (over hello RDP connection) paste hello .exe file toohello Desktop of your VM.</span></span> 
14. <span data-ttu-id="851fa-164">Přejděte toohello ploše virtuálního počítače a dvakrát klikněte na hello .exe.</span><span class="sxs-lookup"><span data-stu-id="851fa-164">Navigate toohello desktop of your VM and double-click on hello .exe.</span></span> <span data-ttu-id="851fa-165">Se spustí příkazový řádek a pak připojte bod obnovení hello jako sdílené složky, který je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="851fa-165">This will launch a command prompt and then mount hello recovery point as a file share that you can access.</span></span> <span data-ttu-id="851fa-166">Po dokončení vytváření hello sdílenou složku, zadejte **q** tooclose hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="851fa-166">When it is finished creating hello share, type **q** tooclose hello command prompt.</span></span>
15. <span data-ttu-id="851fa-167">Na vašem virtuálním počítači otevřete **Průzkumníka souborů** a přejděte toohello písmeno jednotky, která byla použita pro sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="851fa-167">On your VM, open **File Explorer** and navigate toohello drive letter that was used for hello file share.</span></span>
16. <span data-ttu-id="851fa-168">Přejděte too\inetpub\wwwroot a zkopírujte **iisstart.png** ze souboru hello sdílet a vložte jej do \inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="851fa-168">Navigate too\inetpub\wwwroot and copy **iisstart.png** from hello file share and paste it into \inetpub\wwwroot.</span></span> <span data-ttu-id="851fa-169">Například F:\inetpub\wwwroot\iisstart.png zkopírujte a vložte jej do souboru hello toorecover c:\inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="851fa-169">For example, copy F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot toorecover hello file.</span></span>
17. <span data-ttu-id="851fa-170">V místním počítači, otevřete kartu hello prohlížeče, kde jste připojeni toohello IP adresu hello virtuálních počítačů hello IIS výchozí stránkou.</span><span class="sxs-lookup"><span data-stu-id="851fa-170">On your local computer, open hello browser tab where you are connected toohello IP address of hello VM showing hello IIS default page.</span></span> <span data-ttu-id="851fa-171">Stiskněte kombinaci kláves CTRL + F5 toorefresh hello prohlížeči stránky.</span><span class="sxs-lookup"><span data-stu-id="851fa-171">Press CTRL + F5 toorefresh hello browser page.</span></span> <span data-ttu-id="851fa-172">Teď byste měli vidět tento hello obnovení bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="851fa-172">You should now see that hello image has been restored.</span></span>
18. <span data-ttu-id="851fa-173">V místním počítači, přejděte zpět toohello kartu prohlížeče pro hello portál Azure a v **krok 3: odpojení hello disky po obnovení** klikněte na tlačítko hello **odpojit disky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="851fa-173">On your local computer, go back toohello browser tab for hello Azure portal and in **Step 3: Unmount hello disks after recovery** click hello **Unmount Disks** button.</span></span> <span data-ttu-id="851fa-174">Pokud zapomenete toodo tento krok, se po 12 hodinách automaticky zavřít hello připojení toohello přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="851fa-174">If you forget toodo this step, hello connection toohello mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="851fa-175">Po těchto 12 hodin je třeba toodownload nový skript toocreate nové přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="851fa-175">After those 12 hours, you need toodownload a new script toocreate a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="851fa-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="851fa-176">Next steps</span></span>

<span data-ttu-id="851fa-177">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="851fa-177">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="851fa-178">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="851fa-178">Create a backup of a VM</span></span>
> * <span data-ttu-id="851fa-179">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="851fa-179">Schedule a daily backup</span></span>
> * <span data-ttu-id="851fa-180">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="851fa-180">Restore a file from a backup</span></span>

<span data-ttu-id="851fa-181">Posunutí další kurz toolearn toohello o monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="851fa-181">Advance toohello next tutorial toolearn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="851fa-182">Monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="851fa-182">Monitor virtual machines</span></span>](tutorial-monitoring.md)









