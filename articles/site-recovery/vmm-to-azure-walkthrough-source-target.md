---
title: "aaaSet hello zdroj a cíl pro tooAzure replikace technologie Hyper-V (s nástrojem System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci virtuálních počítačů technologie Hyper-V v úložišti tooAzure cloudy VMM s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: 3f8c5386cb64527c775aef636980bac098ee9905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-with-vmm-replication-tooazure"></a>Krok 8: Nastavení hello zdroj a cíl pro tooAzure replikace technologie Hyper-V (s nástrojem VMM)

Po [vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md) a určení, co jste tooreplicate, použijte tento článek tooconfigure zdroj a cíl nastavení při replikaci virtuálních počítačů Hyper-V místní v System Center Virtual Machine Manager (VMM) tooAzure cloudy, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

S1. Klikněte na **Připravit infrastrukturu** > **Zdroj**.

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. V **připravit zdroj**, klikněte na tlačítko **+ VMM** tooadd serveru VMM.

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a tímto serverem VMM hello splňuje hello [požadavky a adresy URL požadavky na](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.
5. Stáhněte si registrační klíč hello. Budete ho potřebovat, když spustíte instalaci. Hello klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-hello-provider-on-hello-vmm-server"></a>Nainstalujte hello zprostředkovatele na serveru VMM hello

1. Spusťte instalační soubor zprostředkovatele hello na serveru VMM hello.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V **instalace**, přijmout nebo úpravě hello výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. Po dokončení instalace klikněte na tlačítko **zaregistrovat** tooregister hello serveru VMM v trezoru hello.
5. V hello **nastavení trezoru** klikněte na tlačítko **Procházet** tooselect hello soubor klíče trezoru. Zadejte předplatné Azure Site Recovery hello a název trezoru hello.

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. V **připojení k Internetu**, zadejte, jak hello hello zprostředkovatel, který běží na serveru VMM hello připojí tooSite obnovení přes internet.

   * Pokud chcete hello zprostředkovatele tooconnect přímo, vyberte **připojit přímo bez proxy tooAzure Site Recovery**.
   * Pokud váš stávající proxy server vyžaduje ověření, nebo chcete toouse vlastní proxy server, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.
   * Pokud používáte vlastní proxy server, zadejte hello adresu, port a přihlašovací údaje.
   * Pokud používáte proxy server, které by už mít povolené hello adresy URL popisované v [požadavky](#on-premises-prerequisites).
   * Pokud používáte vlastní proxy server, účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí hello zadané přihlašovací údaje pro proxy. Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. nastavení účtu VMM RunAs Hello lze upravit v konzole VMM hello. V **nastavení**, rozbalte položku **zabezpečení** > **účty spustit jako**a potom upravte hello heslo pro DRAProxyAccount. Služba VMM hello toorestart budete potřebovat, aby toto nastavení projevilo.

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat hello. Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery hello. Uchovávejte tento certifikát v bezpečí. Když spustíte převzetí služeb při selhání tooAzure budete je potřebovat toodecrypt, pokud je povolené šifrování dat.
8. V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello. V konfiguraci clusteru zadejte název role clusteru VMM hello.
9. Povolit **synchronizovat metadata cloudu**, pokud chcete toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello. Tuto akci stačí toohappen jednou na každém serveru. Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello. Klikněte na tlačítko **zaregistrovat** toocomplete hello procesu.

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. Spustí se registrace. Po dokončení registrace hello serveru zobrazuje v **infrastruktura Site Recovery** > **servery VMM**.


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a>Nainstalujte agenta služeb zotavení Azure hello na hostitele Hyper-V

1. Poté, co jste nastavili hello poskytovatele, musíte pro agenta služeb zotavení Azure hello toodownload hello instalační soubor. Spusťte instalační program na každém serveru technologie Hyper-V v cloudu VMM hello.

    ![Lokality Hyper-V](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. Na stránce **Kontrola předpokladů** klikněte na **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. V **nastavení instalace**přijměte nebo změňte umístění instalace hello a umístění mezipaměti hello. Hello mezipaměti můžete nakonfigurovat na jednotce, která má minimálně 5 GB dostupného úložiště, ale doporučujeme mezipaměti jednotku s 600 GB nebo více volného místa. Pak klikněte na **Nainstalovat**.
4. Po dokončení instalace klikněte na tlačítko **Zavřít** toofinish.

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Hello agenta služeb zotavení Microsoft Azure si můžete nainstalovat z příkazového řádku pomocí hello následující příkaz:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a>Nastavení Internetu proxy přístup tooSite obnovení z hostitelů Hyper-V

agent služeb zotavení Hello spuštěné v hostitelích technologie Hyper-V potřebuje tooAzure přístup k Internetu pro replikaci virtuálního počítače. Pokud získáváte přístup k hello Internetu prostřednictvím proxy serveru, nastavte ho takto:

1. Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na hostiteli Hyper-V hello. Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in hello, klikněte na **změnit vlastnosti**.
3. Na hello **konfiguraci proxy serveru** zadejte informace o proxy serveru.

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. Zkontrolujte, že agent hello přístup hello adresy URL popisované v hello [požadavky](#on-premises-prerequisites).

## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello
Zadejte toobe účet úložiště Azure hello používá pro replikaci a hello toowhich síť Azure virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, vyberte předplatné hello a hello skupinu prostředků, kam chcete hello toocreate při selhání virtuálního počítače. Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management) pro hello při selhání virtuálního počítače.

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. Pokud jste ještě nevytvořili účet úložiště, a chcete toocreate jeden pomocí Resource Manager, klikněte na tlačítko **+ účet úložiště** toodo přímo tady.  Na hello **vytvořit účet úložiště** okně zadejte název účtu, typ, předplatné a umístění. Hello účet by měl být v hello stejné umístění jako hello trezoru služeb zotavení.

   ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * Pokud chcete účet úložiště pomocí klasického modelu hello toocreate, udělat na hello portálu Azure. [Další informace](../storage/common/storage-create-storage-account.md)
   * Pokud pro replikovaná data používáte účet úložiště premium, nastavte na účet další standardní úložiště toostore protokoly replikace, které zaznamenání dat tooon místní probíhající změny.
5. Pokud jste ještě nevytvořili síť Azure a chcete toocreate jeden pomocí Resource Manager, klikněte na tlačítko **+ síť** toodo přímo tady. Na hello **vytvořit virtuální síť** okně zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění. Hello síť musí být ve hello stejné umístění jako hello trezoru služeb zotavení.

   ![Síť](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   Pokud chcete toocreate síť pomocí klasického modelu hello, udělat na hello portálu Azure. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).





## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 9: nakonfigurování mapování sítě](vmm-to-azure-walkthrough-network-mapping.md)
