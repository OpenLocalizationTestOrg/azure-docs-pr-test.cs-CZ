---
title: "aaaSet hello zdroj a cíl pro tooAzure replikace technologie Hyper-V (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci virtuálních počítačů Hyper-V tooAzure úložiště s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 105b90e6ac053d5b842c54a36c460a26d0f5c2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-replication-tooazure"></a>Krok 8: Nastavení hello zdroj a cíl pro tooAzure replikace technologie Hyper-V

Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure technologie Hyper-V virtuální počítače (bez System Center VMM), pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nastavení lokality hello technologie Hyper-V, nainstalujte hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure hello na hostitele Hyper-V a registraci hello lokality v trezoru hello.

1. V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**. Klikněte na tlačítko tooadd nové lokality Hyper-V jako kontejner pro hostitele Hyper-V nebo clusterů, **serveru technologie Hyper-V +**.

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. V **web vytvořit Hyper-V**, zadejte název lokality hello. Pak klikněte na **OK**. Nyní, vyberte lokalitu hello jste vytvořili a klikněte na tlačítko **+ technologie Hyper-V Server** tooadd toohello serveru lokality.

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. V **přidat Server** > **typ serveru**, zkontrolujte, zda **technologie Hyper-V server** se zobrazí.

    - Zajistěte, aby tento server hello technologie Hyper-V chcete tooadd odpovídá hello [požadavky](#on-premises-prerequisites), a je možné tooaccess hello zadaných adres URL.
    - Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello. Spusťte tento soubor tooinstall hello zprostředkovatele a hello agenta služeb zotavení na každém hostiteli technologie Hyper-V.

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-hello-provider-and-agent"></a>Instalace hello zprostředkovatel a agent

1. Spusťte instalační soubor zprostředkovatele hello na každém hostiteli jste přidali lokality toohello technologie Hyper-V. Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru. Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V **instalace**, přijmout nebo úpravě hello výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.
4. V **nastavení trezoru**, klikněte na tlačítko **Procházet** tooselect hello trezoru klíčů soubor, který jste stáhli. Zadejte předplatné Azure Site Recovery hello, hello název trezoru, a patří hello technologie Hyper-V serveru toowhich hello technologie Hyper-V.

    ![Registrace serveru](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. V **nastavení proxy serveru**, zadejte, jak hello hello zprostředkovatel, který běží na hostitelích technologie Hyper-V připojí tooAzure Site Recovery přes internet.

    * Pokud chcete hello zprostředkovatele tooconnect přímo vyberte **připojit přímo bez proxy tooAzure Site Recovery**.
    * Pokud váš stávající proxy server vyžaduje ověření, nebo chcete použít pro připojení poskytovatele hello toouse vlastní proxy server, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.
    * Pokud používáte proxy server:
        - Zadejte hello adresu, port a přihlašovací údaje
        - Ujistěte se, zda text hello adresy URL popisované v hello [požadavky](#prerequisites) jsou povoleny přes proxy server hello.

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Po dokončení instalace, klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.

    ![Umístění instalace](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru hello technologie Hyper-V a hello server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.


## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Zadejte hello účtu úložiště Azure pro replikaci a hello toowhich síť Azure virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.
2. Vyberte hello předplatné a skupina prostředků hello, ve kterém chcete toocreate hello virtuální počítače Azure po převzetí služeb při selhání. Zvolte hello nasazení modelu, že chcete toouse v Azure (klasický nebo prostředek management) pro hello virtuálních počítačů.

3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    - Pokud nemáte účet úložiště, klikněte na tlačítko **+ úložiště** toocreate vložené založené na správci prostředků účtu. 
    - Pokud nemáte síť Azure, klikněte na tlačítko **+ síť** toocreate vložené sítě pomocí Správce prostředků.






## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)
