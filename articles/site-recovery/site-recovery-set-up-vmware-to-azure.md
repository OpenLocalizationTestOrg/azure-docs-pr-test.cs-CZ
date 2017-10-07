---
title: "Nastavení prostředí pro zdroj hello (VMware tooAzure) | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikace VMware virtuálních počítačů tooAzure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: cbeeffc1061b11223e12ff3e237fa7e55b999aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-vmware-tooazure"></a>Nastavení prostředí pro zdroj hello (VMware tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fyzické tooAzure](./site-recovery-set-up-physical-to-azure.md)

Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikovat virtuální počítače běží na VMware tooAzure.

## <a name="prerequisites"></a>Požadavky

Hello článek předpokládá, že jste již vytvořili:
- Trezor služeb zotavení v hello [portál Azure](http://portal.azure.com "portál Azure").
- Vyhrazený účet do systému VMware vCenter, který lze použít pro [automatického zjišťování](./site-recovery-vmware-to-azure.md).
- Virtuální počítač, na které tooinstall hello konfigurační server.

## <a name="configuration-server-minimum-requirements"></a>Minimální požadavky na konfiguraci serveru
Hello konfigurace serverového softwaru musí být nasazené na virtuální počítače VMware s vysokou dostupností. Hello následující tabulka uvádí hello minimální požadavky na hardware, software a sítě pro konfigurační server.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Založený na protokolu HTTPS proxy servery nejsou podporovány hello konfigurační server.

## <a name="choose-your-protection-goals"></a>Volba cílů ochrany

1. V hello portálu Azure, přejděte toohello **služeb zotavení** okno trezoru a vyberte svůj trezor.
2. V nabídce hello prostředků úložiště hello přejděte příliš**Začínáme** > **Site Recovery** > **krok 1: připravte infrastrukturu**  >  **Cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **tooAzure**a zvolte **Ano, s hypervisoru VMware vSphere**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí
Nastavení hello zdrojové prostředí zahrnuje dva hlavní činnosti:

- Nainstalujte a zaregistrujte konfigurační server pomocí Site Recovery.
- Zjistit virtuální počítače na místní připojením Site Recovery tooyour místní VMware vCenter nebo vSphere EXSi hostitelů.

### <a name="step-1-install-and-register-a-configuration-server"></a>Krok 1: Instalace a zaregistrujte konfigurační server

1. Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**. V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** tooadd jeden.

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Na hello **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.
5. Stáhněte si registrační klíč trezoru hello. Když spustíte instalační program Unified musíte hello registrační klíč. Hello klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Na počítači hello používáte jako hello konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** tooinstall hello konfigurační server hello procesový server a hlavní hello cílí na serveru.

#### <a name="run-azure-site-recovery-unified-setup"></a>Spuštění Azure Site Recovery sjednocený instalační program

> [!TIP]
> Registrace serveru konfigurace selže, pokud čas hello na váš počítač systémové hodiny se liší od místního času ve více než pět minut. Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace hello.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Hello konfigurační server lze nainstalovat pomocí příkazového řádku. Další informace najdete v tématu [instalace hello konfigurační server pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).

#### <a name="add-hello-vmware-account-for-automatic-discovery"></a>Přidat účet hello VMware pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Krok 2: Přidání systému vCenter
tooallow Azure Site Recovery toodiscover virtuální počítače běžící na vašem místním prostředí, musíte tooconnect serveru VMware vCenter nebo hostitelů vSphere ESXi službou Site Recovery.

Vyberte **+ vCenter** toostart připojuje VMware vCenter server nebo hostiteli ESXi VMware vSphere.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Běžné problémy
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Další kroky
[Nastavení cílového prostředí](./site-recovery-prepare-target-vmware-to-azure.md) v Azure.
