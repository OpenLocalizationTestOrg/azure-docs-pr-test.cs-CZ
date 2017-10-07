---
title: "Nastavení prostředí pro zdroj hello (fyzické servery tooAzure) | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikace fyzických serverů s Windows nebo Linuxem do Azure."
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
ms.openlocfilehash: d4702265bf36910015685d2bba99d6e577531bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-physical-server-tooazure"></a>Nastavení prostředí pro zdroj hello (tooAzure fyzického serveru)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fyzické tooAzure](./site-recovery-set-up-physical-to-azure.md)

Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikace fyzických serverů s Windows nebo Linuxem do Azure.

## <a name="prerequisites"></a>Požadavky

Hello článek předpokládá, že už máte:
1. Trezor služeb zotavení v hello [portál Azure](http://portal.azure.com "portál Azure").
3. Fyzický počítač, na které tooinstall hello konfiguračním serveru.

### <a name="configuration-server-minimum-requirements"></a>Minimální požadavky na konfiguraci serveru
Hello následující tabulka uvádí hello minimální požadavky na hardware, software a sítě pro konfigurační server.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Založený na protokolu HTTPS proxy servery nejsou podporovány hello konfigurační server.

## <a name="choose-your-protection-goals"></a>Volba cílů ochrany

1. V hello portálu Azure, přejděte toohello **služeb zotavení** trezory okno a vyberte svůj trezor.
2. V hello **prostředků** nabídky hello trezoru, klikněte na tlačítko **Začínáme** > **Site Recovery** > **krok 1: Příprava Infrastruktura** > **cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **tooAzure** a **není virtualizované/jiné**a potom klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

1. V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** tooadd jeden.

  ![Nastavení zdroje](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. V hello **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.
5. Stáhněte si registrační klíč trezoru hello. Když spustíte instalační program Unified musíte hello registrační klíč. Hello klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. Na počítači hello používáte jako hello konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** tooinstall hello konfigurační server hello procesový server a hlavní hello cílí na serveru.

#### <a name="run-azure-site-recovery-unified-setup"></a>Spuštění Azure Site Recovery sjednocený instalační program

> [!TIP]
> Konfigurace serveru registrace selže, pokud čas hello na systémových hodin vašeho počítače je delší než 5 minut z místního času. Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace hello.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Hello konfigurační server lze nainstalovat pomocí příkazového řádku. Další informace najdete v tématu [instalace konfigurační server pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Běžné problémy

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Další kroky

Dalším krokem zahrnuje [nastavení cílového prostředí](./site-recovery-prepare-target-physical-to-azure.md) v Azure.
