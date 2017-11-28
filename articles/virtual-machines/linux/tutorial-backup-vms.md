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
ms.openlocfilehash: 7c00392d5185a2f067f2ee2717529dcbde1e71f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a><span data-ttu-id="bf65f-103">Zálohovat virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="bf65f-103">Back up Linux  virtual machines in Azure</span></span>

<span data-ttu-id="bf65f-104">Svá data můžete chránit prováděním záloh v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="bf65f-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="bf65f-105">Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="bf65f-106">Při obnovení z bodu obnovení můžete obnovit hello celý virtuální počítač nebo jenom určité soubory.</span><span class="sxs-lookup"><span data-stu-id="bf65f-106">When you restore from a recovery point, you can restore hello whole VM or just specific files.</span></span> <span data-ttu-id="bf65f-107">Tento článek vysvětluje, jak toorestore jednoho souboru tooa nginx spuštěného virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="bf65f-107">This article explains how toorestore a single file tooa Linux VM running nginx.</span></span> <span data-ttu-id="bf65f-108">Pokud ještě nemáte toouse virtuálních počítačů, můžete jeden vytvořit pomocí hello [rychlý start Linux](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="bf65f-108">If you don't already have a VM toouse, you can create one using hello [Linux quickstart](quick-create-cli.md).</span></span> <span data-ttu-id="bf65f-109">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="bf65f-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bf65f-110">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bf65f-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="bf65f-111">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="bf65f-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="bf65f-112">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="bf65f-112">Restore a file from a backup</span></span>



## <a name="backup-overview"></a><span data-ttu-id="bf65f-113">Přehled služby Backup</span><span class="sxs-lookup"><span data-stu-id="bf65f-113">Backup overview</span></span>

<span data-ttu-id="bf65f-114">V případě hello služba Azure Backup zahájí zálohu, aktivuje hello rozšíření zálohování tootake snímku v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="bf65f-114">When hello Azure Backup service initiates a backup, it triggers hello backup extension tootake a point-in-time snapshot.</span></span> <span data-ttu-id="bf65f-115">Hello služby Azure Backup používá hello _VMSnapshotLinux_ rozšíření v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="bf65f-115">hello Azure Backup service uses hello _VMSnapshotLinux_ extension in Linux.</span></span> <span data-ttu-id="bf65f-116">rozšíření Hello je nainstalován během hello první zálohování virtuálních počítačů, pokud běží hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-116">hello extension is installed during hello first VM backup if hello VM is running.</span></span> <span data-ttu-id="bf65f-117">Pokud hello virtuální počítač není spuštěný, hello služby zálohování pořídí snímek hello základní úložiště (protože žádné zápisy aplikace dochází při hello zastavení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="bf65f-117">If hello VM is not running, hello Backup service takes a snapshot of hello underlying storage (since no application writes occur while hello VM is stopped).</span></span>

<span data-ttu-id="bf65f-118">Ve výchozím nastavení, zálohování Azure přebírá konzistentní zálohu systému souborů pro virtuální počítač s Linuxem, ale může být nakonfigurované tootake [aplikace konzistentní zálohování pomocí skriptů před a po skript rozhraní](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span><span class="sxs-lookup"><span data-stu-id="bf65f-118">By default, Azure Backup takes a file system consistent backup for Linux VM but it can be configured tootake [application consistent backup using pre-script and post-script framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span></span> <span data-ttu-id="bf65f-119">Jakmile hello služby Azure Backup používá hello snímku, je hello data přenášená toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="bf65f-119">Once hello Azure Backup service takes hello snapshot, hello data is transferred toohello vault.</span></span> <span data-ttu-id="bf65f-120">toomaximize efektivitu, služba hello identifikuje a přenáší pouze hello bloky dat, které se změnily od hello předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="bf65f-120">toomaximize efficiency, hello service identifies and transfers only hello blocks of data that have changed since hello previous backup.</span></span>

<span data-ttu-id="bf65f-121">Po dokončení přenosu dat hello hello snímek odebrán a vytvoří bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf65f-121">When hello data transfer is complete, hello snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="bf65f-122">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="bf65f-122">Create a backup</span></span>
<span data-ttu-id="bf65f-123">Vytvořte jednoduchý naplánované denní zálohování tooa trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="bf65f-123">Create a simple scheduled daily backup tooa Recovery Services Vault.</span></span> 

1. <span data-ttu-id="bf65f-124">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bf65f-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="bf65f-125">V nabídce hello na levé straně hello vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-125">In hello menu on hello left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="bf65f-126">Hello seznamu vyberte virtuální počítač tooback nahoru.</span><span class="sxs-lookup"><span data-stu-id="bf65f-126">From hello list, select a VM tooback up.</span></span>
4. <span data-ttu-id="bf65f-127">V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-127">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="bf65f-128">Hello **povolit zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="bf65f-128">hello **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="bf65f-129">V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název hello hello nový trezor.</span><span class="sxs-lookup"><span data-stu-id="bf65f-129">In **Recovery Services vault**, click **Create new** and provide hello name for hello new vault.</span></span> <span data-ttu-id="bf65f-130">Nový trezor se vytvoří v hello stejnou skupinu prostředků a umístění jako virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="bf65f-130">A new vault is created in hello same Resource Group and location as hello virtual machine.</span></span>
6. <span data-ttu-id="bf65f-131">Klikněte na tlačítko **zálohování zásad**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-131">Click **Backup policy**.</span></span> <span data-ttu-id="bf65f-132">V tomto příkladu zachovat hello výchozí hodnoty a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-132">For this example, keep hello defaults and click **OK**.</span></span>
7. <span data-ttu-id="bf65f-133">Na hello **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-133">On hello **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="bf65f-134">Tím se vytvoří denní zálohování podle plánu výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="bf65f-134">This creates a daily backup based on hello default schedule.</span></span>
10. <span data-ttu-id="bf65f-135">toocreate bod počáteční obnovení na hello **zálohování** okně klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-135">toocreate an initial recovery point, on hello **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="bf65f-136">Na hello **zálohovat nyní** okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-136">On hello **Backup Now** blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="bf65f-137">V hello **zálohování** okno pro virtuální počítač, zobrazí se hello počet bodů obnovení, které jsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="bf65f-137">In hello **Backup** blade for your VM, you will see hello number of recovery points that are complete.</span></span>

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)

<span data-ttu-id="bf65f-139">první zálohy Hello trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="bf65f-139">hello first backup takes about 20 minutes.</span></span> <span data-ttu-id="bf65f-140">Po dokončení zálohování, pokračujte toohello další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bf65f-140">Proceed toohello next part of this tutorial after your backup is finished.</span></span>

## <a name="restore-a-file"></a><span data-ttu-id="bf65f-141">Obnovit soubor</span><span class="sxs-lookup"><span data-stu-id="bf65f-141">Restore a file</span></span>

<span data-ttu-id="bf65f-142">Pokud omylem odstraníte nebo aby se změny tooa soubor, můžete použít soubor hello toorecover obnovení souborů z trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="bf65f-142">If you accidentally delete or make changes tooa file, you can use File Recovery toorecover hello file from your backup vault.</span></span> <span data-ttu-id="bf65f-143">Obnovení souborů používá skript, který běží na hello virtuálních počítačů, bod obnovení hello toomount jako místní disk.</span><span class="sxs-lookup"><span data-stu-id="bf65f-143">File Recovery uses a script that runs on hello VM, toomount hello recovery point as local drive.</span></span> <span data-ttu-id="bf65f-144">Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení hello a obnovit je toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-144">These drives will remain mounted for 12 hours so that you can copy files from hello recovery point and restore them toohello VM.</span></span>  

<span data-ttu-id="bf65f-145">V tomto příkladu ukážeme, jak toorecover hello výchozí nginx webové stránky /var/www/html/index.nginx-debian.html.</span><span class="sxs-lookup"><span data-stu-id="bf65f-145">In this example, we show how toorecover hello default nginx web page /var/www/html/index.nginx-debian.html.</span></span> <span data-ttu-id="bf65f-146">Hello veřejnou IP adresu naše virtuálního počítače v tomto příkladu je *13.69.75.209*.</span><span class="sxs-lookup"><span data-stu-id="bf65f-146">hello public IP address of our VM in this example is *13.69.75.209*.</span></span> <span data-ttu-id="bf65f-147">Můžete najít hello IP adresu virtuální počítač pomocí:</span><span class="sxs-lookup"><span data-stu-id="bf65f-147">You can find hello IP address of your vm using:</span></span>

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. <span data-ttu-id="bf65f-148">V místním počítači otevřete prohlížeč a zadejte v hello veřejnou IP adresu virtuálního počítače toosee hello výchozí nginx webové stránky.</span><span class="sxs-lookup"><span data-stu-id="bf65f-148">On your local computer, open a browser and type in hello public IP address of your VM toosee hello default nginx web page.</span></span>

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

1. <span data-ttu-id="bf65f-150">SSH ke svému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="bf65f-150">SSH into your VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
2. <span data-ttu-id="bf65f-151">Odstraňte /var/www/html/index.nginx-debian.html.</span><span class="sxs-lookup"><span data-stu-id="bf65f-151">Delete /var/www/html/index.nginx-debian.html.</span></span>

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. <span data-ttu-id="bf65f-152">V místním počítači, aktualizujte hello prohlížeče zasažení CTRL + F5 toosee, která výchozí stránka nginx je pryč.</span><span class="sxs-lookup"><span data-stu-id="bf65f-152">On your local computer, refresh hello browser by hitting CTRL + F5 toosee that default nginx page is gone.</span></span>

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-broken.png)
    
1. <span data-ttu-id="bf65f-154">V místním počítači, přihlášení toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bf65f-154">On your local computer, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
6. <span data-ttu-id="bf65f-155">V nabídce hello na levé straně hello vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-155">In hello menu on hello left, select **Virtual machines**.</span></span> 
7. <span data-ttu-id="bf65f-156">Hello seznamu vyberte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-156">From hello list, select hello VM.</span></span>
8. <span data-ttu-id="bf65f-157">V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-157">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="bf65f-158">Hello **zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="bf65f-158">hello **Backup** blade opens.</span></span> 
9. <span data-ttu-id="bf65f-159">V nabídce hello hello horní části okna hello vyberte **obnovení souboru**.</span><span class="sxs-lookup"><span data-stu-id="bf65f-159">In hello menu at hello top of hello blade, select **File Recovery**.</span></span> <span data-ttu-id="bf65f-160">Hello **obnovení souboru** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="bf65f-160">hello **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="bf65f-161">V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="bf65f-161">In **Step 1: Select recovery point**, select a recovery point from hello drop-down.</span></span>
11. <span data-ttu-id="bf65f-162">V **krok 2: stáhnout skript toobrowse a obnovit soubory**, klikněte na tlačítko hello **spustitelný soubor stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bf65f-162">In **Step 2: Download script toobrowse and recover files**, click hello **Download Executable** button.</span></span> <span data-ttu-id="bf65f-163">Uložte hello stažený soubor tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="bf65f-163">Save hello downloaded file tooyour local computer.</span></span>
7. <span data-ttu-id="bf65f-164">Klikněte na tlačítko **stáhnout skript** toodownload hello soubor skriptu místně.</span><span class="sxs-lookup"><span data-stu-id="bf65f-164">Click **Download script** toodownload hello script file locally.</span></span>
8. <span data-ttu-id="bf65f-165">Otevřete Bash řádku a zadejte hello následující, nahraďte *Linux_myVM_05-05-2017.sh* s hello opravte cestu a název souboru hello skript, který jste stáhli, *azureuser* s hello uživatelské jméno pro hello virtuálních počítačů a *13.69.75.209* s hello veřejnou IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bf65f-165">Open a Bash prompt and type hello following, replacing *Linux_myVM_05-05-2017.sh* with hello correct path and filename for hello script that you downloaded, *azureuser* with hello username for hello VM and *13.69.75.209* with hello public IP address for your VM.</span></span>
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. <span data-ttu-id="bf65f-166">V místním počítači otevřete toohello připojení SSH virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-166">On your local computer, open an SSH connection toohello VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
    
10. <span data-ttu-id="bf65f-167">Na virtuální počítač, přidejte provést soubor skriptu toohello oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bf65f-167">On your VM, add execute permissions toohello script file.</span></span>

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. <span data-ttu-id="bf65f-168">Na vašem virtuálním počítači spusťte bod obnovení hello skriptu toomount hello jako systému souborů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-168">On your VM, run hello script toomount hello recovery point as a filesystem.</span></span>

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. <span data-ttu-id="bf65f-169">Hello výstupu z hello skriptu poskytuje že Hello cestu pro hello přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="bf65f-169">hello output from hello script gives you hello path for hello mount point.</span></span> <span data-ttu-id="bf65f-170">výstup Hello vypadá podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="bf65f-170">hello output looks similar toothis:</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting toorecovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of hello recovery point toothis machine...
                         
    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

12. <span data-ttu-id="bf65f-171">Na virtuální počítač, zkopírujte hello nginx výchozí webové stránky z hello přípojného bodu back toowhere odstranit soubor hello.</span><span class="sxs-lookup"><span data-stu-id="bf65f-171">On your VM, copy hello nginx default web page from hello mount point back toowhere you deleted hello file.</span></span>

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. <span data-ttu-id="bf65f-172">V místním počítači, otevřete kartu hello prohlížeče, kde jste připojeni toohello IP adresu hello virtuálních počítačů hello nginx výchozí stránkou.</span><span class="sxs-lookup"><span data-stu-id="bf65f-172">On your local computer, open hello browser tab where you are connected toohello IP address of hello VM showing hello nginx default page.</span></span> <span data-ttu-id="bf65f-173">Stiskněte kombinaci kláves CTRL + F5 toorefresh hello prohlížeči stránky.</span><span class="sxs-lookup"><span data-stu-id="bf65f-173">Press CTRL + F5 toorefresh hello browser page.</span></span> <span data-ttu-id="bf65f-174">Teď byste měli vidět tento hello znovu funguje výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="bf65f-174">You should now see that hello default page is working again.</span></span>

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

18. <span data-ttu-id="bf65f-176">V místním počítači, přejděte zpět toohello kartu prohlížeče pro hello portál Azure a v **krok 3: odpojení hello disky po obnovení** klikněte na tlačítko hello **odpojit disky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bf65f-176">On your local computer, go back toohello browser tab for hello Azure portal and in **Step 3: Unmount hello disks after recovery** click hello **Unmount Disks** button.</span></span> <span data-ttu-id="bf65f-177">Pokud zapomenete toodo tento krok, se po 12 hodinách automaticky zavřít hello připojení toohello přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="bf65f-177">If you forget toodo this step, hello connection toohello mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="bf65f-178">Po těchto 12 hodin je třeba toodownload nový skript toocreate nové přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="bf65f-178">After those 12 hours, you need toodownload a new script toocreate a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="bf65f-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf65f-179">Next steps</span></span>

<span data-ttu-id="bf65f-180">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="bf65f-180">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bf65f-181">Vytvoření zálohy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bf65f-181">Create a backup of a VM</span></span>
> * <span data-ttu-id="bf65f-182">Naplánovat denní zálohování</span><span class="sxs-lookup"><span data-stu-id="bf65f-182">Schedule a daily backup</span></span>
> * <span data-ttu-id="bf65f-183">Obnovte soubor ze zálohy</span><span class="sxs-lookup"><span data-stu-id="bf65f-183">Restore a file from a backup</span></span>

<span data-ttu-id="bf65f-184">Posunutí další kurz toolearn toohello o monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bf65f-184">Advance toohello next tutorial toolearn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bf65f-185">Monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bf65f-185">Monitor virtual machines</span></span>](tutorial-monitoring.md)

