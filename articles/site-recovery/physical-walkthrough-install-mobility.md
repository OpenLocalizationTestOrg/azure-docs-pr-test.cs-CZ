---
title: "aaaInstall hello službu Mobility u fyzického serveru tooAzure replikace | Microsoft Docs"
description: "Tento článek popisuje, jak tooinstall hello agenta služby Mobility na fyzických serverech replikace tooAzure službou Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 48fd2c0ffe67875ed446c8167c2ae7f90d3f537c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-install-hello-mobility-service"></a>Krok 9: Instalace služby Mobility hello


Tento článek popisuje, jak tooinstall hello službu mobility při replikaci místní tooAzure fyzických serverů Windows nebo Linuxem, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

Hello služba Mobility zaznamenává datové zápisy na počítači a předává je toohello procesový server. Je třeba jej nainstalovat na každém serveru, které chcete tooreplicate tooAzure.

Služba Mobility hello můžete nainstalovat ručně, nebo pomocí nabízené instalace z hello Site Recovery zpracovat, server, pokud je zapnutá replikace, nebo pomocí nástroje, jako je například System Center Configuration Manager. Pokud používáte nabízenou instalaci, hello je nainstalována služba na serveru hello při povolení replikace.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Ruční instalace

1. Zkontrolujte hello [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.
2. Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu hello.
3. Pokud dáváte přednost tooinstall z hello příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-hello-process-server"></a>Nainstalujte z hello procesového serveru

Pokud chcete toopush hello instalace služby Mobility ze serveru hello procesu, když povolíte replikaci pro počítač, je třeba účet, který lze použít hello proces serveru tooaccess hello počítačem. Hello účet se používá pouze pro hello nabízenou instalaci.

1. Pokud jste nevytvořili účet, to udělat pomocí těchto pokynů:

    - Můžete použít domény nebo místní účet
    - Pro Windows Pokud nepoužíváte účet domény, budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo, informace v hello zaregistrovat v rámci **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD hello **LocalAccountTokenFilterPolicy**, s hodnotou 1.
    - Pokud chcete položku registru hello tooadd pro Windows z rozhraní příkazového řádku, zadejte:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - Pro systémy Linux hello účet by měl být kořenový na zdrojovém serveru se Linux hello.

2. Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete, aby služba Mobility hello toopush na virtuální počítače se systémem Windows nebo Linux.

## <a name="other-installation-methods"></a>Jiné metody instalace

- [Další informace o](site-recovery-install-mobility-service-using-sccm.md) instalaci služby Mobility hello pomocí nástroje Configuration Manager
- [Další informace o](site-recovery-automate-mobility-service-install.md) instalace s Azure Automation DSC.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 10: povolení replikace](physical-walkthrough-enable-replication.md)
