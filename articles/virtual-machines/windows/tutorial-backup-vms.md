---
title: "Zálohování virtuálních počítačů Azure Windows | Microsoft Docs"
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
ms.openlocfilehash: 8e58a2290e5034ef393f65cbcddb86e18cf4a6ec
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a><span data-ttu-id="953b7-103">Zálohovat virtuální počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="953b7-103">Back up Windows virtual machines in Azure</span></span>

<span data-ttu-id="953b7-104">Svá data můžete chránit prováděním záloh v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="953b7-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="953b7-105">Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů.</span><span class="sxs-lookup"><span data-stu-id="953b7-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="953b7-106">Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jenom určité soubory.</span><span class="sxs-lookup"><span data-stu-id="953b7-106">When you restore from a recovery point, you can restore the whole VM or just specific files.</span></span> <span data-ttu-id="953b7-107">Tento článek vysvětluje, jak obnovit jeden soubor k virtuálnímu počítači spuštěný Windows Server a službu IIS.</span><span class="sxs-lookup"><span data-stu-id="953b7-107">This article explains how to restore a single file to a VM running Windows Server and IIS.</span></span> <span data-ttu-id="953b7-108">Pokud ještě nemáte virtuální počítač používat, můžete vytvořit jeden pomocí [rychlé spuštění Windows](quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="953b7-108">If you don't already have a VM to use, you can create one using the [Windows quickstart](quick-create-portal.md).</span></span> <span data-ttu-id="953b7-109">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="953b7-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="953b7-110">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="953b7-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="953b7-111">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="953b7-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="953b7-112">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="953b7-112">Restore a file from a backup</span></span>




## <a name="backup-overview"></a><span data-ttu-id="953b7-113">Přehled služby Backup</span><span class="sxs-lookup"><span data-stu-id="953b7-113">Backup overview</span></span>

<span data-ttu-id="953b7-114">Když služba Azure Backup zahájí úlohu zálohování, aktivuje rozšíření zálohování k pořízení snímku v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="953b7-114">When the Azure Backup service initiates a backup job, it triggers the backup extension to take a point-in-time snapshot.</span></span> <span data-ttu-id="953b7-115">Použití služby Azure Backup _VMSnapshot_ rozšíření.</span><span class="sxs-lookup"><span data-stu-id="953b7-115">The Azure Backup service uses the _VMSnapshot_ extension.</span></span> <span data-ttu-id="953b7-116">Rozšíření je nainstalován během první zálohování virtuálních počítačů, pokud je virtuální počítač spuštěný.</span><span class="sxs-lookup"><span data-stu-id="953b7-116">The extension is installed during the first VM backup if the VM is running.</span></span> <span data-ttu-id="953b7-117">Pokud virtuální počítač není spuštěný, služba zálohování pořídí snímek podkladové úložiště (protože žádné aplikace zápisy dojít při zastavení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="953b7-117">If the VM is not running, the Backup service takes a snapshot of the underlying storage (since no application writes occur while the VM is stopped).</span></span>

<span data-ttu-id="953b7-118">Při pořizování snímku virtuálních počítačů Windows, služba zálohování koordinuje s Stínová kopie svazku Service (VSS) získat konzistentního snímku disky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="953b7-118">When taking a snapshot of Windows VMs, the Backup service coordinates with the Volume Shadow Copy Service (VSS) to get a consistent snapshot of the virtual machine's disks.</span></span> <span data-ttu-id="953b7-119">Jakmile služba Azure Backup používá snímku, data se přenáší do trezoru.</span><span class="sxs-lookup"><span data-stu-id="953b7-119">Once the Azure Backup service takes the snapshot, the data is transferred to the vault.</span></span> <span data-ttu-id="953b7-120">Pokud chcete maximalizovat efektivitu, služba identifikuje a přenáší pouze bloky dat, které se změnily od předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="953b7-120">To maximize efficiency, the service identifies and transfers only the blocks of data that have changed since the previous backup.</span></span>

<span data-ttu-id="953b7-121">Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="953b7-121">When the data transfer is complete, the snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="953b7-122">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="953b7-122">Create a backup</span></span>
<span data-ttu-id="953b7-123">Vytvořte jednoduché plánované denní zálohování do trezoru služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="953b7-123">Create a simple scheduled daily backup to a Recovery Services Vault.</span></span> 

1. <span data-ttu-id="953b7-124">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="953b7-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="953b7-125">V nabídce na levé straně vyberte **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="953b7-125">In the menu on the left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="953b7-126">V seznamu vyberte virtuální počítač, který chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="953b7-126">From the list, select a VM to back up.</span></span>
4. <span data-ttu-id="953b7-127">V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="953b7-127">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="953b7-128">**Povolit zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="953b7-128">The **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="953b7-129">V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název pro nový trezor.</span><span class="sxs-lookup"><span data-stu-id="953b7-129">In **Recovery Services vault**, click **Create new** and provide the name for the new vault.</span></span> <span data-ttu-id="953b7-130">Nový trezor se vytvoří ve stejné skupině prostředků a umístění jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="953b7-130">A new vault is created in the same Resource Group and location as the virtual machine.</span></span>
6. <span data-ttu-id="953b7-131">Klikněte na tlačítko **zálohování zásad**.</span><span class="sxs-lookup"><span data-stu-id="953b7-131">Click **Backup policy**.</span></span> <span data-ttu-id="953b7-132">V tomto příkladu ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="953b7-132">For this example, keep the defaults and click **OK**.</span></span>
7. <span data-ttu-id="953b7-133">Na **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**.</span><span class="sxs-lookup"><span data-stu-id="953b7-133">On the **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="953b7-134">Tím se vytvoří denní zálohování podle plánu, výchozí.</span><span class="sxs-lookup"><span data-stu-id="953b7-134">This creates a daily backup based on the default schedule.</span></span>
10. <span data-ttu-id="953b7-135">Vytvořit bod obnovení počáteční na **zálohování** okně klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="953b7-135">To create an initial recovery point, on the **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="953b7-136">Na **zálohovat nyní** okně klikněte na ikonu Kalendář, pomocí ovládacího prvku Kalendář vyberte poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="953b7-136">On the **Backup Now** blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="953b7-137">V **zálohování** okno pro virtuální počítač, zobrazí se počet bodů obnovení, které jsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="953b7-137">In the **Backup** blade for your VM, you will see the number of recovery points that are complete.</span></span>

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)
    
<span data-ttu-id="953b7-139">První zálohování trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="953b7-139">The first backup takes about 20 minutes.</span></span> <span data-ttu-id="953b7-140">Po dokončení zálohování, přejděte k další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="953b7-140">Proceed to the next part of this tutorial after your backup is finished.</span></span>

## <a name="recover-a-file"></a><span data-ttu-id="953b7-141">Obnovit soubor</span><span class="sxs-lookup"><span data-stu-id="953b7-141">Recover a file</span></span>

<span data-ttu-id="953b7-142">Pokud omylem odstraníte nebo provést změny do souboru, můžete obnovit soubor z trezoru zálohování obnovení souborů.</span><span class="sxs-lookup"><span data-stu-id="953b7-142">If you accidentally delete or make changes to a file, you can use File Recovery to recover the file from your backup vault.</span></span> <span data-ttu-id="953b7-143">Obnovení souborů používá skript, který běží na virtuální počítač, přípojný bod obnovení jako místní disk.</span><span class="sxs-lookup"><span data-stu-id="953b7-143">File Recovery uses a script that runs on the VM, to mount the recovery point as local drive.</span></span> <span data-ttu-id="953b7-144">Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení a obnovit je do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="953b7-144">These drives will remain mounted for 12 hours so that you can copy files from the recovery point and restore them to the VM.</span></span>  

<span data-ttu-id="953b7-145">V tomto příkladu ukážeme, jak k obnovení souboru bitové kopie, který se používá v výchozí webové stránky pro službu IIS.</span><span class="sxs-lookup"><span data-stu-id="953b7-145">In this example, we show how to recover the image file that is used in the default web page for IIS.</span></span> 

1. <span data-ttu-id="953b7-146">Otevřete prohlížeč a připojte na adresu IP virtuálního počítače, který zobrazit výchozí stránka služby IIS.</span><span class="sxs-lookup"><span data-stu-id="953b7-146">Open a browser and connect to the IP address of the VM to show the default IIS page.</span></span>

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-working.png)

2. <span data-ttu-id="953b7-148">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="953b7-148">Connect to the VM.</span></span>
3. <span data-ttu-id="953b7-149">Na virtuální počítač, otevřete **Průzkumníka souborů** , přejděte na \inetpub\wwwroot, odstraňte soubor **iisstart.png**.</span><span class="sxs-lookup"><span data-stu-id="953b7-149">On the VM, open **File Explorer** and navigate to \inetpub\wwwroot and delete the file **iisstart.png**.</span></span>
4. <span data-ttu-id="953b7-150">V místním počítači aktualizujte prohlížeč a zjistíte, že je pryč bitovou kopii na výchozí stránka služby IIS.</span><span class="sxs-lookup"><span data-stu-id="953b7-150">On your local computer, refresh the browser to see that the image on the default IIS page is gone.</span></span>

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-broken.png)

5. <span data-ttu-id="953b7-152">V místním počítači, otevřete novou kartu a přejděte [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="953b7-152">On your local computer, open a new tab and go the the [Azure portal](https://portal.azure.com).</span></span>
6. <span data-ttu-id="953b7-153">V nabídce na levé straně vyberte **virtuální počítače** a vyberte formuláře virtuálních počítačů v seznamu.</span><span class="sxs-lookup"><span data-stu-id="953b7-153">In the menu on the left, select **Virtual machines** and select the VM form the list.</span></span>
8. <span data-ttu-id="953b7-154">V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="953b7-154">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="953b7-155">**Zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="953b7-155">The **Backup** blade opens.</span></span> 
9. <span data-ttu-id="953b7-156">V nabídce v horní části okna vyberte **obnovení souboru**.</span><span class="sxs-lookup"><span data-stu-id="953b7-156">In the menu at the top of the blade, select **File Recovery**.</span></span> <span data-ttu-id="953b7-157">**Obnovení souboru** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="953b7-157">The **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="953b7-158">V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="953b7-158">In **Step 1: Select recovery point**, select a recovery point from the drop-down.</span></span>
11. <span data-ttu-id="953b7-159">V **krok 2: stáhnout skript a procházet a obnovit soubory**, klikněte **spustitelný soubor stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="953b7-159">In **Step 2: Download script to browse and recover files**, click the **Download Executable** button.</span></span> <span data-ttu-id="953b7-160">Uložení souboru do vaší **stáhne** složky.</span><span class="sxs-lookup"><span data-stu-id="953b7-160">Save the file to your **Downloads** folder.</span></span>
12. <span data-ttu-id="953b7-161">V místním počítači, otevřete **Průzkumníka souborů** a přejděte do vaší **stáhne** složky a zkopírujte na stažený soubor .exe.</span><span class="sxs-lookup"><span data-stu-id="953b7-161">On your local computer, open **File Explorer** and navigate to your **Downloads** folder and copy the downloaded .exe file.</span></span> <span data-ttu-id="953b7-162">Název souboru bude obsahovat předponu název vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="953b7-162">The filename will be prefixed by your VM name.</span></span> 
13. <span data-ttu-id="953b7-163">Na vašem virtuálním počítači (přes připojení RDP) vložte soubor .exe na ploše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="953b7-163">On your VM (over the RDP connection) paste the .exe file to the Desktop of your VM.</span></span> 
14. <span data-ttu-id="953b7-164">Přejděte na ploše virtuálního počítače a dvakrát klikněte na .exe.</span><span class="sxs-lookup"><span data-stu-id="953b7-164">Navigate to the desktop of your VM and double-click on the .exe.</span></span> <span data-ttu-id="953b7-165">Se spustí příkazový řádek a pak připojte jako sdílené složky, který je k dispozici bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="953b7-165">This will launch a command prompt and then mount the recovery point as a file share that you can access.</span></span> <span data-ttu-id="953b7-166">Po dokončení vytváření sdílené složky, zadejte **q** zavřete příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="953b7-166">When it is finished creating the share, type **q** to close the command prompt.</span></span>
15. <span data-ttu-id="953b7-167">Na vašem virtuálním počítači otevřete **Průzkumníka souborů** a přejděte na písmeno jednotky, která byla použita pro sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="953b7-167">On your VM, open **File Explorer** and navigate to the drive letter that was used for the file share.</span></span>
16. <span data-ttu-id="953b7-168">Přejděte do \inetpub\wwwroot a zkopírujte **iisstart.png** ze souboru sdílet a vložte jej do \inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="953b7-168">Navigate to \inetpub\wwwroot and copy **iisstart.png** from the file share and paste it into \inetpub\wwwroot.</span></span> <span data-ttu-id="953b7-169">Například F:\inetpub\wwwroot\iisstart.png zkopírujte a vložte jej do c:\inetpub\wwwroot k obnovení souboru.</span><span class="sxs-lookup"><span data-stu-id="953b7-169">For example, copy F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot to recover the file.</span></span>
17. <span data-ttu-id="953b7-170">V místním počítači otevřete kartu prohlížeče, kde jste připojeni k IP adresu virtuálního počítače zobrazující výchozí stránka služby IIS.</span><span class="sxs-lookup"><span data-stu-id="953b7-170">On your local computer, open the browser tab where you are connected to the IP address of the VM showing the IIS default page.</span></span> <span data-ttu-id="953b7-171">Stisknutím kláves CTRL + F5 aktualizujte stránku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="953b7-171">Press CTRL + F5 to refresh the browser page.</span></span> <span data-ttu-id="953b7-172">Teď byste měli vidět, že byla obnovena bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="953b7-172">You should now see that the image has been restored.</span></span>
18. <span data-ttu-id="953b7-173">V místním počítači, přejděte zpět na záložce prohlížeče pro portál Azure a v **krok 3: odpojení disky po obnovení** klikněte na tlačítko **odpojit disky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="953b7-173">On your local computer, go back to the browser tab for the Azure portal and in **Step 3: Unmount the disks after recovery** click the **Unmount Disks** button.</span></span> <span data-ttu-id="953b7-174">Pokud zapomenete tento krok, připojení k přípojný bod je automaticky zavřít po 12 hodinách.</span><span class="sxs-lookup"><span data-stu-id="953b7-174">If you forget to do this step, the connection to the mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="953b7-175">Po těchto 12 hodin budete muset stáhnout nový skript pro vytvoření nové přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="953b7-175">After those 12 hours, you need to download a new script to create a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="953b7-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="953b7-176">Next steps</span></span>

<span data-ttu-id="953b7-177">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="953b7-177">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="953b7-178">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="953b7-178">Create a backup of a VM</span></span>
> * <span data-ttu-id="953b7-179">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="953b7-179">Schedule a daily backup</span></span>
> * <span data-ttu-id="953b7-180">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="953b7-180">Restore a file from a backup</span></span>

<span data-ttu-id="953b7-181">Přechodu na v dalším kurzu se dozvíte o monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="953b7-181">Advance to the next tutorial to learn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="953b7-182">Monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="953b7-182">Monitor virtual machines</span></span>](tutorial-monitoring.md)









