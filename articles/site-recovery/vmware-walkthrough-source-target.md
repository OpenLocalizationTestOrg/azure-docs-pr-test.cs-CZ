---
title: "aaaSet hello zdroj a cíl pro tooAzure replikace VMware s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci virtuálních počítačů VMware tooAzure úložiště s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ef33a44bc5da17afb0442be63f576925f5b9a8b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-vmware-replication-tooazure"></a>Krok 8: Nastavení hello zdroj a cíl pro tooAzure replikace VMware

Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure virtuální počítače VMware, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nastavení konfigurace serveru hello, zaregistrovat v trezoru hello a vyhledání virtuálních počítačů.

1. Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.
5. Stáhněte si registrační klíč trezoru hello. Tuto funkci potřebujete po spuštění Unified instalace. Hello klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a>Zaregistrujte konfigurační server hello v trezoru hello

Následující hello před start a potom spusťte tooinstall hello konfigurační server, hello procesový server a hlavní cílový server hello sjednocený instalační program.
    - Vám zajistí rychlý přehled video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Ujistěte se, že hello systémové hodiny synchronizované s na konfigurační server hello virtuálních počítačů, [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
    - Spusťte instalační program jako místní správce na konfigurační server hello virtuálních počítačů.
    - Ujistěte se, že je povolená TLS 1.0 na hello virtuálních počítačů.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Hello konfigurační server lze také nainstalovat [z příkazového řádku hello](http://aka.ms/installconfigsrv).



## <a name="connect-toovmware-servers"></a>Připojte tooVMware servery

tooallow Azure Site Recovery toodiscover virtuální počítače běžící na vašem místním prostředí, musíte tooconnect serveru VMware vCenter nebo hostitelů vSphere ESXi službou Site Recovery. Než začnete, vezměte na vědomí následující hello:

- Pokud přidáte hello vCenter server nebo hostitelů vSphere-tooSite obnovení pomocí účtu bez oprávnění správce na serveru hello, potřebuje účet hello těchto oprávnění povoleno:
    - Datacenter, úložiště, složka, hostitele, sítě, prostředků, virtuální počítač, vSphere distribuované přepínače.
    - Hello vCenter server potřebuje oprávnění úložišť zobrazení.
- Když přidáte tooSite servery VMware obnovení, může trvat 15 minut nebo déle pro ně tooappear hello portálu.

### <a name="add-hello-account-for-automatic-discovery"></a>Přidat účet hello pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Nastavit připojení

Připojte tooservers následujícím způsobem:

1. Vyberte **+ vCenter** toostart připojuje VMware vCenter server nebo hostiteli ESXi VMware vSphere.
2. V **přidat vCenter**, zadejte popisný název pro hello vSphere hostitele nebo vCenter server a pak zadejte hello IP adresu nebo plně kvalifikovaný název domény serveru hello.
3. Pokud jsou vaše servery VMware nakonfigurovaná toolisten pro požadavky na jiném portu nechte hello port 443. Vyberte hello účtu, který je tooconnect toohello VMware vCenter nebo vSphere ESXi serveru. Klikněte na **OK**.
4. Obnovení lokality se připojuje tooVMware servery pomocí hello zadané nastavení a vyhledá virtuální počítače.

> [!NOTE]
> Pokud přidáváte server nebo hostitele pomocí účtu, který nemá oprávnění správce na serveru vCenter nebo hostitelů hello, ujistěte se, zda má účet hello těchto oprávnění povoleno: Datacenter, úložiště, složku, hostitele, sítě a prostředků, virtuální počítač a vSphere distribuované přepínače. Kromě toho hello VMware vCenter server potřebuje oprávnění povolená zobrazení hello úložiště.


## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Než nastavíte hello cílové prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, a vyberte hello chcete toouse předplatného Azure.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/vmware-walkthrough-source-target/gs-target.png)
4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, toocreate správce prostředků účtu nebo síti vložené.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)
