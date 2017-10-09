---
title: "aaaInstall hello službu Mobility u replikace VMware tooAzure | Microsoft Docs"
description: "Tento článek popisuje, jak tooinstall hello agenta služby Mobility pro replikaci tooAzure VMware službou Azure Site Recovery hello."
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
ms.openlocfilehash: d3b7bc9c4d84d13317e0b0b47adcf38e8c41d0bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-install-hello-mobility-service"></a>Krok 10: Instalace služby Mobility hello


Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure virtuální počítače VMware, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

Hello služba Mobility zaznamenává datové zápisy na počítači a předává je toohello procesový server. Je třeba jej nainstalovat na každém počítači, které chcete tooreplicate tooAzure.

Můžete nainstalovat ručně službu Mobility hello, pomocí nabízené instalace z hello Site Recovery procesový server, pokud je zapnutá replikace, nebo pomocí nástroje System Center Configuration Manager. Pokud používáte nabízenou instalaci, hello je nainstalovaná služba hello virtuálních počítačů při je zapnutá replikace.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Ruční instalace

1. Zkontrolujte hello [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.
2. Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu hello.
3. Pokud dáváte přednost tooinstall z hello příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-hello-process-server"></a>Nainstalujte z hello procesového serveru

Pokud chcete instalace služby Mobility toopush hello z hello procesový server, když povolíte replikaci pro virtuální počítač, je třeba účet, který mohou být využívána hello proces serveru tooaccess hello virtuálních počítačů. Hello účet se používá pouze pro hello nabízenou instalaci.

1. Měli byste mít [vytvořili účet](vmware-walkthrough-prepare-vmware.md) který lze použít pro nabízenou instalaci. Potom zadejte hello účet, že který má toouse při konfiguraci nastavení zdroje během nasazování Site Recovery.
2. Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete, aby služba Mobility hello toopush na virtuální počítače se systémem Windows nebo Linux.

## <a name="other-methods"></a>Ostatní metody

Další informace o [instalaci služby Mobility hello pomocí nástroje Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), nebo pomocí [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)
