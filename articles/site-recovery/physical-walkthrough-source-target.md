---
title: "aaaSet hello zdroj a cíl pro fyzický server tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci úložiště tooAzure fyzických serverů s hello služba Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e75ef23174d77509bf54380eba8f251a7337a45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-hello-source-and-target-for-physical-server-replication-tooazure"></a>Krok 7: Nastavení hello zdroj a cíl pro tooAzure replikace fyzického serveru

Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure fyzických serverů pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nastavení konfigurace serveru hello, zaregistrovat v trezoru hello a zjistit počítače.

1. Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.
5. Stáhněte si registrační klíč trezoru hello. Tuto funkci potřebujete po spuštění Unified instalace. Hello klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a>Zaregistrujte konfigurační server hello v trezoru hello

Následující hello před start a potom spusťte tooinstall hello konfigurační server, hello procesový server a hlavní cílový server hello sjednocený instalační program. Hello video popisuje vytvoření hello komponenty pro virtuální počítač VMware tooAzure replikace. Hello stejný postup je však platný pro replikaci tooAzure fyzický server.

- Ujistěte se, že hello systémové hodiny synchronizované s na konfigurační server hello virtuálních počítačů, [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
- Spusťte instalační program jako místní správce na počítači serveru konfigurace hello.
- Ujistěte se, že je povolená TLS 1.0 na hello virtuálních počítačů.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Hello konfigurační server lze také nainstalovat [z příkazového řádku hello](http://aka.ms/installconfigsrv).




## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Než nastavíte hello cílové prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, a vyberte hello chcete toouse předplatného Azure.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/physical-walkthrough-source-target/gs-target.png)

4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, toocreate správce prostředků účtu nebo síti vložené.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)
