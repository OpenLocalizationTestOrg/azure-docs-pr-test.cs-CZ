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
# <a name="back-up-windows-virtual-machines-in-azure"></a>Zálohovat virtuální počítače s Windows v Azure

Svá data můžete chránit prováděním záloh v pravidelných intervalech. Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit hello celý virtuální počítač nebo jenom určité soubory. Tento článek vysvětluje, jak toorestore jednoho souboru tooa virtuální počítač spuštěný Windows Server a službu IIS. Pokud ještě nemáte toouse virtuálních počítačů, můžete jeden vytvořit pomocí hello [rychlé spuštění Windows](quick-create-portal.md). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy




## <a name="backup-overview"></a>Přehled služby Backup

V případě hello služba Azure Backup zahájí úlohu zálohování, aktivuje hello rozšíření zálohování tootake snímku v daném okamžiku. Hello služby Azure Backup používá hello _VMSnapshot_ rozšíření. rozšíření Hello je nainstalován během hello první zálohování virtuálních počítačů, pokud běží hello virtuálních počítačů. Pokud hello virtuální počítač není spuštěný, hello služby zálohování pořídí snímek hello základní úložiště (protože žádné zápisy aplikace dochází při hello zastavení virtuálního počítače).

Při pořizování snímku virtuálních počítačů Windows, služba Backup hello koordinuje pomocí služby Stínová kopie svazku (VSS) tooget hello konzistentního snímku disky hello virtuálního počítače. Jakmile hello služby Azure Backup používá hello snímku, je hello data přenášená toohello trezoru. toomaximize efektivitu, služba hello identifikuje a přenáší pouze hello bloky dat, které se změnily od hello předchozí zálohy.

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

## <a name="recover-a-file"></a>Obnovit soubor

Pokud omylem odstraníte nebo aby se změny tooa soubor, můžete použít soubor hello toorecover obnovení souborů z trezoru záloh. Obnovení souborů používá skript, který běží na hello virtuálních počítačů, bod obnovení hello toomount jako místní disk. Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení hello a obnovit je toohello virtuálních počítačů.  

V tomto příkladu ukážeme, jak toorecover hello soubor bitové kopie, který se používá v hello výchozí webové stránky pro službu IIS. 

1. Otevřete prohlížeč a připojte toohello IP adresu hello virtuálních počítačů tooshow hello výchozí IIS stránky.

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-working.png)

2. Připojte toohello virtuálních počítačů.
3. Na hello virtuálních počítačů, otevřete **Průzkumníka souborů** přejděte too\inetpub\wwwroot a odstranit soubor hello **iisstart.png**.
4. V místním počítači je pryč toosee aktualizace hello prohlížeče, který hello bitové kopie na stránce služby IIS výchozí hello.

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-broken.png)

5. V místním počítači, otevřete novou kartu a přejděte hello hello [portál Azure](https://portal.azure.com).
6. V nabídce hello na levé straně hello vyberte **virtuální počítače** a seznam hello formuláře vyberte hello virtuálních počítačů.
8. V okně hello virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**. Hello **zálohování** otevře se okno. 
9. V nabídce hello hello horní části okna hello vyberte **obnovení souboru**. Hello **obnovení souboru** otevře se okno.
10. V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu hello.
11. V **krok 2: stáhnout skript toobrowse a obnovit soubory**, klikněte na tlačítko hello **spustitelný soubor stáhnout** tlačítko. Uložte soubor tooyour hello **stáhne** složky.
12. V místním počítači, otevřete **Průzkumníka souborů** a přejděte tooyour **stáhne** hello složky a zkopírujte stažený soubor .exe. Název souboru Hello bude obsahovat předponu název vašeho virtuálního počítače. 
13. Na vašem virtuálním počítači (přes hello připojení RDP) vložte toohello souboru .exe hello ploše virtuálního počítače. 
14. Přejděte toohello ploše virtuálního počítače a dvakrát klikněte na hello .exe. Se spustí příkazový řádek a pak připojte bod obnovení hello jako sdílené složky, který je k dispozici. Po dokončení vytváření hello sdílenou složku, zadejte **q** tooclose hello příkazového řádku.
15. Na vašem virtuálním počítači otevřete **Průzkumníka souborů** a přejděte toohello písmeno jednotky, která byla použita pro sdílenou složku hello.
16. Přejděte too\inetpub\wwwroot a zkopírujte **iisstart.png** ze souboru hello sdílet a vložte jej do \inetpub\wwwroot. Například F:\inetpub\wwwroot\iisstart.png zkopírujte a vložte jej do souboru hello toorecover c:\inetpub\wwwroot.
17. V místním počítači, otevřete kartu hello prohlížeče, kde jste připojeni toohello IP adresu hello virtuálních počítačů hello IIS výchozí stránkou. Stiskněte kombinaci kláves CTRL + F5 toorefresh hello prohlížeči stránky. Teď byste měli vidět tento hello obnovení bitové kopie.
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









