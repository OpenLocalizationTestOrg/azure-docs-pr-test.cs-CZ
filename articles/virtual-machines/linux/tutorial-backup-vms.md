---
title: "Zálohování virtuálních počítačů Azure Linux | Microsoft Docs"
description: "Chraňte vaše virtuální počítače s Linuxem pomocí zálohování pomocí služby Azure Backup."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d0cbf7883a8737bcb10e9dd251c9792a12993f77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a><span data-ttu-id="b3eff-103">Zálohovat virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="b3eff-103">Back up Linux  virtual machines in Azure</span></span>

<span data-ttu-id="b3eff-104">Svá data můžete chránit prováděním záloh v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="b3eff-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="b3eff-105">Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů.</span><span class="sxs-lookup"><span data-stu-id="b3eff-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="b3eff-106">Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jenom určité soubory.</span><span class="sxs-lookup"><span data-stu-id="b3eff-106">When you restore from a recovery point, you can restore the whole VM or just specific files.</span></span> <span data-ttu-id="b3eff-107">Tento článek vysvětluje, jak k obnovení virtuálního počítače s Linuxem systémem nginx jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="b3eff-107">This article explains how to restore a single file to a Linux VM running nginx.</span></span> <span data-ttu-id="b3eff-108">Pokud ještě nemáte virtuální počítač používat, můžete vytvořit jeden pomocí [rychlý start Linux](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b3eff-108">If you don't already have a VM to use, you can create one using the [Linux quickstart](quick-create-cli.md).</span></span> <span data-ttu-id="b3eff-109">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="b3eff-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3eff-110">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b3eff-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="b3eff-111">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="b3eff-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="b3eff-112">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="b3eff-112">Restore a file from a backup</span></span>



## <a name="backup-overview"></a><span data-ttu-id="b3eff-113">Přehled služby Backup</span><span class="sxs-lookup"><span data-stu-id="b3eff-113">Backup overview</span></span>

<span data-ttu-id="b3eff-114">Když služba Azure Backup zahájí zálohu, aktivuje rozšíření zálohování k pořízení snímku v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="b3eff-114">When the Azure Backup service initiates a backup, it triggers the backup extension to take a point-in-time snapshot.</span></span> <span data-ttu-id="b3eff-115">Použití služby Azure Backup _VMSnapshotLinux_ rozšíření v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="b3eff-115">The Azure Backup service uses the _VMSnapshotLinux_ extension in Linux.</span></span> <span data-ttu-id="b3eff-116">Rozšíření je nainstalován během první zálohování virtuálních počítačů, pokud je virtuální počítač spuštěný.</span><span class="sxs-lookup"><span data-stu-id="b3eff-116">The extension is installed during the first VM backup if the VM is running.</span></span> <span data-ttu-id="b3eff-117">Pokud virtuální počítač není spuštěný, služba zálohování pořídí snímek podkladové úložiště (protože žádné aplikace zápisy dojít při zastavení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="b3eff-117">If the VM is not running, the Backup service takes a snapshot of the underlying storage (since no application writes occur while the VM is stopped).</span></span>

<span data-ttu-id="b3eff-118">Ve výchozím nastavení, zálohování Azure trvá konzistentní zálohu systému souborů pro virtuální počítač s Linuxem ale dá se provést [aplikace konzistentní zálohování pomocí skriptů před a po skript rozhraní](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span><span class="sxs-lookup"><span data-stu-id="b3eff-118">By default, Azure Backup takes a file system consistent backup for Linux VM but it can be configured to take [application consistent backup using pre-script and post-script framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span></span> <span data-ttu-id="b3eff-119">Jakmile služba Azure Backup používá snímku, data se přenáší do trezoru.</span><span class="sxs-lookup"><span data-stu-id="b3eff-119">Once the Azure Backup service takes the snapshot, the data is transferred to the vault.</span></span> <span data-ttu-id="b3eff-120">Pokud chcete maximalizovat efektivitu, služba identifikuje a přenáší pouze bloky dat, které se změnily od předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="b3eff-120">To maximize efficiency, the service identifies and transfers only the blocks of data that have changed since the previous backup.</span></span>

<span data-ttu-id="b3eff-121">Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="b3eff-121">When the data transfer is complete, the snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="b3eff-122">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="b3eff-122">Create a backup</span></span>
<span data-ttu-id="b3eff-123">Vytvořte jednoduché plánované denní zálohování do trezoru služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="b3eff-123">Create a simple scheduled daily backup to a Recovery Services Vault.</span></span> 

1. <span data-ttu-id="b3eff-124">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b3eff-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b3eff-125">V nabídce na levé straně vyberte **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-125">In the menu on the left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="b3eff-126">V seznamu vyberte virtuální počítač, který chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="b3eff-126">From the list, select a VM to back up.</span></span>
4. <span data-ttu-id="b3eff-127">V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-127">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="b3eff-128">**Povolit zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="b3eff-128">The **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="b3eff-129">V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název pro nový trezor.</span><span class="sxs-lookup"><span data-stu-id="b3eff-129">In **Recovery Services vault**, click **Create new** and provide the name for the new vault.</span></span> <span data-ttu-id="b3eff-130">Nový trezor se vytvoří ve stejné skupině prostředků a umístění jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b3eff-130">A new vault is created in the same Resource Group and location as the virtual machine.</span></span>
6. <span data-ttu-id="b3eff-131">Klikněte na tlačítko **zálohování zásad**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-131">Click **Backup policy**.</span></span> <span data-ttu-id="b3eff-132">V tomto příkladu ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-132">For this example, keep the defaults and click **OK**.</span></span>
7. <span data-ttu-id="b3eff-133">Na **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-133">On the **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="b3eff-134">Tím se vytvoří denní zálohování podle plánu, výchozí.</span><span class="sxs-lookup"><span data-stu-id="b3eff-134">This creates a daily backup based on the default schedule.</span></span>
10. <span data-ttu-id="b3eff-135">Vytvořit bod obnovení počáteční na **zálohování** okně klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-135">To create an initial recovery point, on the **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="b3eff-136">Na **zálohovat nyní** okně klikněte na ikonu Kalendář, pomocí ovládacího prvku Kalendář vyberte poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-136">On the **Backup Now** blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="b3eff-137">V **zálohování** okno pro virtuální počítač, zobrazí se počet bodů obnovení, které jsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="b3eff-137">In the **Backup** blade for your VM, you will see the number of recovery points that are complete.</span></span>

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)

<span data-ttu-id="b3eff-139">První zálohování trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="b3eff-139">The first backup takes about 20 minutes.</span></span> <span data-ttu-id="b3eff-140">Po dokončení zálohování, přejděte k další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b3eff-140">Proceed to the next part of this tutorial after your backup is finished.</span></span>

## <a name="restore-a-file"></a><span data-ttu-id="b3eff-141">Obnovit soubor</span><span class="sxs-lookup"><span data-stu-id="b3eff-141">Restore a file</span></span>

<span data-ttu-id="b3eff-142">Pokud omylem odstraníte nebo provést změny do souboru, můžete obnovit soubor z trezoru zálohování obnovení souborů.</span><span class="sxs-lookup"><span data-stu-id="b3eff-142">If you accidentally delete or make changes to a file, you can use File Recovery to recover the file from your backup vault.</span></span> <span data-ttu-id="b3eff-143">Obnovení souborů používá skript, který běží na virtuální počítač, přípojný bod obnovení jako místní disk.</span><span class="sxs-lookup"><span data-stu-id="b3eff-143">File Recovery uses a script that runs on the VM, to mount the recovery point as local drive.</span></span> <span data-ttu-id="b3eff-144">Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení a obnovit je do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b3eff-144">These drives will remain mounted for 12 hours so that you can copy files from the recovery point and restore them to the VM.</span></span>  

<span data-ttu-id="b3eff-145">V tomto příkladu ukážeme, jak obnovit /var/www/html/index.nginx-debian.html výchozí nginx webové stránky.</span><span class="sxs-lookup"><span data-stu-id="b3eff-145">In this example, we show how to recover the default nginx web page /var/www/html/index.nginx-debian.html.</span></span> <span data-ttu-id="b3eff-146">Veřejná IP adresa naše virtuálního počítače v tomto příkladu je *13.69.75.209*.</span><span class="sxs-lookup"><span data-stu-id="b3eff-146">The public IP address of our VM in this example is *13.69.75.209*.</span></span> <span data-ttu-id="b3eff-147">Můžete najít IP adresu virtuální počítač pomocí:</span><span class="sxs-lookup"><span data-stu-id="b3eff-147">You can find the IP address of your vm using:</span></span>

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. <span data-ttu-id="b3eff-148">V místním počítači otevřete prohlížeč a zadejte veřejnou IP adresu vašeho virtuálního počítače zobrazíte výchozí nginx webové stránky.</span><span class="sxs-lookup"><span data-stu-id="b3eff-148">On your local computer, open a browser and type in the public IP address of your VM to see the default nginx web page.</span></span>

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

1. <span data-ttu-id="b3eff-150">SSH ke svému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="b3eff-150">SSH into your VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
2. <span data-ttu-id="b3eff-151">Odstraňte /var/www/html/index.nginx-debian.html.</span><span class="sxs-lookup"><span data-stu-id="b3eff-151">Delete /var/www/html/index.nginx-debian.html.</span></span>

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. <span data-ttu-id="b3eff-152">V místním počítači aktualizujte stránku prohlížeče zasažení kombinaci kláves CTRL + F5 zobrazíte této stránce nginx výchozí je pryč.</span><span class="sxs-lookup"><span data-stu-id="b3eff-152">On your local computer, refresh the browser by hitting CTRL + F5 to see that default nginx page is gone.</span></span>

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-broken.png)
    
1. <span data-ttu-id="b3eff-154">V místním počítači, přihlaste se k [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b3eff-154">On your local computer, sign in to the [Azure portal](https://portal.azure.com/).</span></span>
6. <span data-ttu-id="b3eff-155">V nabídce na levé straně vyberte **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-155">In the menu on the left, select **Virtual machines**.</span></span> 
7. <span data-ttu-id="b3eff-156">V seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b3eff-156">From the list, select the VM.</span></span>
8. <span data-ttu-id="b3eff-157">V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-157">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="b3eff-158">**Zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="b3eff-158">The **Backup** blade opens.</span></span> 
9. <span data-ttu-id="b3eff-159">V nabídce v horní části okna vyberte **obnovení souboru**.</span><span class="sxs-lookup"><span data-stu-id="b3eff-159">In the menu at the top of the blade, select **File Recovery**.</span></span> <span data-ttu-id="b3eff-160">**Obnovení souboru** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="b3eff-160">The **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="b3eff-161">V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b3eff-161">In **Step 1: Select recovery point**, select a recovery point from the drop-down.</span></span>
11. <span data-ttu-id="b3eff-162">V **krok 2: stáhnout skript a procházet a obnovit soubory**, klikněte **spustitelný soubor stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3eff-162">In **Step 2: Download script to browse and recover files**, click the **Download Executable** button.</span></span> <span data-ttu-id="b3eff-163">Uložte stažený soubor do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="b3eff-163">Save the downloaded file to your local computer.</span></span>
7. <span data-ttu-id="b3eff-164">Klikněte na tlačítko **stáhnout skript** ke stažení souboru skriptu místně.</span><span class="sxs-lookup"><span data-stu-id="b3eff-164">Click **Download script** to download the script file locally.</span></span>
8. <span data-ttu-id="b3eff-165">Otevřete Bash řádku a zadejte následující příkaz, nahraďte *Linux_myVM_05-05-2017.sh* se správnou cestu a název souboru pro skript, který jste stáhli, *azureuser* s uživatelským jménem pro virtuální počítač a *13.69.75.209* s veřejnou IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b3eff-165">Open a Bash prompt and type the following, replacing *Linux_myVM_05-05-2017.sh* with the correct path and filename for the script that you downloaded, *azureuser* with the username for the VM and *13.69.75.209* with the public IP address for your VM.</span></span>
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. <span data-ttu-id="b3eff-166">V místním počítači otevřete připojení SSH pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b3eff-166">On your local computer, open an SSH connection to the VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
    
10. <span data-ttu-id="b3eff-167">Na virtuální počítač, přidejte oprávnění k souboru skriptu ke spouštění.</span><span class="sxs-lookup"><span data-stu-id="b3eff-167">On your VM, add execute permissions to the script file.</span></span>

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. <span data-ttu-id="b3eff-168">Na vašem virtuálním počítači spusťte skript pro přípojný bod obnovení jako systému souborů.</span><span class="sxs-lookup"><span data-stu-id="b3eff-168">On your VM, run the script to mount the recovery point as a filesystem.</span></span>

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. <span data-ttu-id="b3eff-169">Výstup ze skriptu vám cestu pro přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="b3eff-169">The output from the script gives you the path for the mount point.</span></span> <span data-ttu-id="b3eff-170">Výstup vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="b3eff-170">The output looks similar to this:</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
                         
    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

12. <span data-ttu-id="b3eff-171">Na virtuální počítač zkopírujte z přípojného bodu zpět do které jste odstranili soubor nginx výchozí webové stránky.</span><span class="sxs-lookup"><span data-stu-id="b3eff-171">On your VM, copy the nginx default web page from the mount point back to where you deleted the file.</span></span>

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. <span data-ttu-id="b3eff-172">V místním počítači otevřete kartu prohlížeče, kde jste připojeni k IP adresu virtuálního počítače výchozí stránkou nginx.</span><span class="sxs-lookup"><span data-stu-id="b3eff-172">On your local computer, open the browser tab where you are connected to the IP address of the VM showing the nginx default page.</span></span> <span data-ttu-id="b3eff-173">Stisknutím kláves CTRL + F5 aktualizujte stránku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b3eff-173">Press CTRL + F5 to refresh the browser page.</span></span> <span data-ttu-id="b3eff-174">Teď byste měli vidět, že výchozí stránky znovu funguje.</span><span class="sxs-lookup"><span data-stu-id="b3eff-174">You should now see that the default page is working again.</span></span>

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

18. <span data-ttu-id="b3eff-176">V místním počítači, přejděte zpět na záložce prohlížeče pro portál Azure a v **krok 3: odpojení disky po obnovení** klikněte na tlačítko **odpojit disky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3eff-176">On your local computer, go back to the browser tab for the Azure portal and in **Step 3: Unmount the disks after recovery** click the **Unmount Disks** button.</span></span> <span data-ttu-id="b3eff-177">Pokud zapomenete tento krok, připojení k přípojný bod je automaticky zavřít po 12 hodinách.</span><span class="sxs-lookup"><span data-stu-id="b3eff-177">If you forget to do this step, the connection to the mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="b3eff-178">Po těchto 12 hodin budete muset stáhnout nový skript pro vytvoření nové přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="b3eff-178">After those 12 hours, you need to download a new script to create a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b3eff-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3eff-179">Next steps</span></span>

<span data-ttu-id="b3eff-180">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="b3eff-180">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3eff-181">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b3eff-181">Create a backup of a VM</span></span>
> * <span data-ttu-id="b3eff-182">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="b3eff-182">Schedule a daily backup</span></span>
> * <span data-ttu-id="b3eff-183">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="b3eff-183">Restore a file from a backup</span></span>

<span data-ttu-id="b3eff-184">Přechodu na v dalším kurzu se dozvíte o monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b3eff-184">Advance to the next tutorial to learn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3eff-185">Monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b3eff-185">Monitor virtual machines</span></span>](tutorial-monitoring.md)

