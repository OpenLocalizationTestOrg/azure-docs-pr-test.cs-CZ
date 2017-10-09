---
title: "aaaSet hello zdroj a cíl pro tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak tooset až hello zdrojové a cílové při replikaci virtuálních počítačů Hyper-V toosecondary VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 451cb4413ca5c09777a7faf512b1c8ea43e695f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-hello-replication-source-and-target"></a>Krok 6: Nastavení hello replikace zdroje a cíle


Po vytvoření služby zotavení trezoru pro Hyper-V replikace tooa sekundární lokalita VMM s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek tooset se hello zdroj a cíl replikace umístění. 

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nainstalujte hello zprostředkovatele Azure Site Recovery na serverech VMM a zjišťovat a zaregistrujte server v trezoru hello.

1. Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. V **připravit zdroj**, klikněte na tlačítko **+ VMM** tooadd serveru VMM.

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a tímto serverem VMM hello splňuje hello [požadavky](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.
5. Stáhněte si registrační klíč hello. Budete ho potřebovat, když spustíte instalaci. Hello klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. Nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello. Nepotřebujete tooexplicitly nic instalovat na hostitelských serverech technologie Hyper-V.


## <a name="install-hello-azure-site-recovery-provider"></a>Nainstalujte zprostředkovatele Azure Site Recovery hello

1. Spusťte instalační soubor zprostředkovatele hello na každý server VMM. Pokud VMM je nasazen v clusteru, proveďte následující hello poprvé, co nainstalujete hello:
    -  Nainstalujte poskytovatele hello na aktivním uzlu a dokončit hello instalace tooregister hello VMM server v trezoru hello.
    - Potom nainstalujte hello zprostředkovatele na hello dalších uzlů. Uzly clusteru, měly být spuštěny hello stejnou verzi hello zprostředkovatele.
2. Instalační program spustí několik kontrol požadovaných součástí a požadavky služby VMM hello toostop oprávnění. Hello služby VMM se restartuje automaticky po dokončení instalace. Pokud instalujete na clusteru VMM, můžete se výzvami toostop hello Clusterové role.
3. V **Microsoft Update**, že se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update toospecify je volitelný.
4. V **instalace**, přijmout nebo upravte výchozí umístění instalace hello a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. Po dokončení instalace klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. V **název trezoru**, ověřte název hello hello úložiště, ve které hello bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. V **připojení k Internetu**, určete, jak hello zprostředkovatel, který běží na serveru VMM hello připojuje tooAzure.

    ![Nastavení internetu](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - Můžete zadat tohoto zprostředkovatele hello by se měly připojit přímo toohello Internetu, nebo prostřednictvím proxy serveru.
   - Zadejte nastavení proxy serveru v případě potřeby.
   - Pokud používáte proxy server, účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí hello zadané přihlašovací údaje pro proxy. Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Hello nastavení účtu spustit jako lze upravit v konzole VMM hello > **nastavení** > **zabezpečení** > **účty spustit jako**. Restartujte změny tooupdate služby VMM hello.
8. V **registrační klíč**, vyberte klíč hello stáhli z Azure Site Recovery a zkopírovali toohello serveru VMM.
9. nastavení šifrování Hello se používá pouze při replikaci virtuálních počítačů Hyper-V v tooAzure cloudy VMM. Pokud replikujete tooa sekundární lokality nepoužívá.
10. V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello. V konfiguraci clusteru zadejte název role clusteru VMM hello.
11. V **synchronizovat metadata cloudu**vyberte, jestli chcete, aby toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello. Tuto akci stačí toohappen jednou na každém serveru. Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello.
12. Klikněte na tlačítko **Další** toocomplete hello procesu. Po registraci načte Azure Site Recovery metadata ze serveru VMM hello. Hello server se zobrazí na hello **servery VMM** karty v hello **servery** stránky v trezoru hello.

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. Po hello server je k dispozici v konzole Site Recovery hello v **zdroj** > **připravit zdroj** vyberte hello VMM server a vyberte hello cloudu, ve které hello technologie Hyper-V se hostitel nachází. Pak klikněte na **OK**.

Hello zprostředkovatele můžete také nainstalovat z příkazového řádku hello:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Vyberte hello cílovém serveru VMM a cloud:

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a vyberte hello cílovém serveru VMM má toouse.
2. Zobrazí se cloudy na serveru hello, jež jsou synchronizovány se Site Recovery. Vyberte cílový cloud hello.

   ![cíl](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).
