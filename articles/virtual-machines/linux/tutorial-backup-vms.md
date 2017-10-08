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
# <a name="back-up-linux--virtual-machines-in-azure"></a>Zálohovat virtuální počítače s Linuxem v Azure

Svá data můžete chránit prováděním záloh v pravidelných intervalech. Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit hello celý virtuální počítač nebo jenom určité soubory. Tento článek vysvětluje, jak toorestore jednoho souboru tooa nginx spuštěného virtuálního počítače s Linuxem. Pokud ještě nemáte toouse virtuálních počítačů, můžete jeden vytvořit pomocí hello [rychlý start Linux](quick-create-cli.md). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy



## <a name="backup-overview"></a>Přehled služby Backup

V případě hello služba Azure Backup zahájí zálohu, aktivuje hello rozšíření zálohování tootake snímku v daném okamžiku. Hello služby Azure Backup používá hello _VMSnapshotLinux_ rozšíření v systému Linux. rozšíření Hello je nainstalován během hello první zálohování virtuálních počítačů, pokud běží hello virtuálních počítačů. Pokud hello virtuální počítač není spuštěný, hello služby zálohování pořídí snímek hello základní úložiště (protože žádné zápisy aplikace dochází při hello zastavení virtuálního počítače).

Ve výchozím nastavení, zálohování Azure přebírá konzistentní zálohu systému souborů pro virtuální počítač s Linuxem, ale může být nakonfigurované tootake [aplikace konzistentní zálohování pomocí skriptů před a po skript rozhraní](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). Jakmile hello služby Azure Backup používá hello snímku, je hello data přenášená toohello trezoru. toomaximize efektivitu, služba hello identifikuje a přenáší pouze hello bloky dat, které se změnily od hello předchozí zálohy.

Po dokončení přenosu dat hello hello snímek odebrán a vytvoří bod obnovení.


## <a name="create-a-backup"></a>Vytvoření zálohy
Vytvořte jednoduchý naplánované denní zálohování tooa trezoru služeb zotavení. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce hello na levé straně hello vyberte **virtuální počítače**. 
3. Hello seznamu vyberte virtuální počítač tooback nahoru.
4. V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**. Hello **povolit zálohování** otevře se okno.
5. V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název hello hello nový trezor. Nový trezor se vytvoří v hello stejnou skupinu prostředků a umístění jako virtuální počítač hello.
6. Klikněte na tlačítko **zálohování zásad**. V tomto příkladu zachovat hello výchozí hodnoty a klikněte na tlačítko **OK**.
7. Na hello **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**. Tím se vytvoří denní zálohování podle plánu výchozí hello.
10. toocreate bod počáteční obnovení na hello **zálohování** okně klikněte na tlačítko **zálohovat nyní**.
11. Na hello **zálohovat nyní** okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.
12. V hello **zálohování** okno pro virtuální počítač, zobrazí se hello počet bodů obnovení, které jsou dokončeny.

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)

první zálohy Hello trvá asi 20 minut. Po dokončení zálohování, pokračujte toohello další části tohoto kurzu.

## <a name="restore-a-file"></a>Obnovit soubor

Pokud omylem odstraníte nebo aby se změny tooa soubor, můžete použít soubor hello toorecover obnovení souborů z trezoru záloh. Obnovení souborů používá skript, který běží na hello virtuálních počítačů, bod obnovení hello toomount jako místní disk. Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení hello a obnovit je toohello virtuálních počítačů.  

V tomto příkladu ukážeme, jak toorecover hello výchozí nginx webové stránky /var/www/html/index.nginx-debian.html. Hello veřejnou IP adresu naše virtuálního počítače v tomto příkladu je *13.69.75.209*. Můžete najít hello IP adresu virtuální počítač pomocí:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. V místním počítači otevřete prohlížeč a zadejte v hello veřejnou IP adresu virtuálního počítače toosee hello výchozí nginx webové stránky.

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

1. SSH ke svému virtuálnímu počítači.

    ```bash
    ssh 13.69.75.209
    ```
2. Odstraňte /var/www/html/index.nginx-debian.html.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. V místním počítači, aktualizujte hello prohlížeče zasažení CTRL + F5 toosee, která výchozí stránka nginx je pryč.

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-broken.png)
    
1. V místním počítači, přihlášení toohello [portál Azure](https://portal.azure.com/).
6. V nabídce hello na levé straně hello vyberte **virtuální počítače**. 
7. Hello seznamu vyberte hello virtuálních počítačů.
8. V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**. Hello **zálohování** otevře se okno. 
9. V nabídce hello hello horní části okna hello vyberte **obnovení souboru**. Hello **obnovení souboru** otevře se okno.
10. V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu hello.
11. V **krok 2: stáhnout skript toobrowse a obnovit soubory**, klikněte na tlačítko hello **spustitelný soubor stáhnout** tlačítko. Uložte hello stažený soubor tooyour místního počítače.
7. Klikněte na tlačítko **stáhnout skript** toodownload hello soubor skriptu místně.
8. Otevřete Bash řádku a zadejte hello následující, nahraďte *Linux_myVM_05-05-2017.sh* s hello opravte cestu a název souboru hello skript, který jste stáhli, *azureuser* s hello uživatelské jméno pro hello virtuálních počítačů a *13.69.75.209* s hello veřejnou IP adresu pro virtuální počítač.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. V místním počítači otevřete toohello připojení SSH virtuálních počítačů.

    ```bash
    ssh 13.69.75.209
    ```
    
10. Na virtuální počítač, přidejte provést soubor skriptu toohello oprávnění.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. Na vašem virtuálním počítači spusťte bod obnovení hello skriptu toomount hello jako systému souborů.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. Hello výstupu z hello skriptu poskytuje že Hello cestu pro hello přípojného bodu. výstup Hello vypadá podobně jako toothis:

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

12. Na virtuální počítač, zkopírujte hello nginx výchozí webové stránky z hello přípojného bodu back toowhere odstranit soubor hello.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. V místním počítači, otevřete kartu hello prohlížeče, kde jste připojeni toohello IP adresu hello virtuálních počítačů hello nginx výchozí stránkou. Stiskněte kombinaci kláves CTRL + F5 toorefresh hello prohlížeči stránky. Teď byste měli vidět tento hello znovu funguje výchozí stránky.

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

18. V místním počítači, přejděte zpět toohello kartu prohlížeče pro hello portál Azure a v **krok 3: odpojení hello disky po obnovení** klikněte na tlačítko hello **odpojit disky** tlačítko. Pokud zapomenete toodo tento krok, se po 12 hodinách automaticky zavřít hello připojení toohello přípojný bod. Po těchto 12 hodin je třeba toodownload nový skript toocreate nové přípojný bod.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy

Posunutí další kurz toolearn toohello o monitorování virtuálních počítačů.

> [!div class="nextstepaction"]
> [Monitorování virtuálních počítačů](tutorial-monitoring.md)

