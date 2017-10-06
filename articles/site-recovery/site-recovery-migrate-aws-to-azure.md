---
title: "aaaMigrate virtuální počítače z AWS tooAzure | Microsoft Docs"
description: "Tento článek popisuje, jak toomigrate virtuální počítače běží v tooAzure Amazon Web Services (AWS) pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: c99b781ec9cca5b8f9a847d3fc48408062b120b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-tooazure-with-azure-site-recovery"></a>Migrovat virtuální počítače v tooAzure Amazon Web Services (AWS) s Azure Site Recovery

Tento článek popisuje, jak toomigrate AWS Windows instance tooAzure virtuálních počítačů s hello [Azure Site Recovery](site-recovery-overview.md) služby.

Migrace je efektivně převzetí služeb při selhání z AWS tooAzure. Nelze navrácení služeb po obnovení počítače tooAWS a neexistuje žádný probíhající replikace. Tento článek popisuje hello kroky pro migraci v hello portál Azure a jsou založené na hello pokyny pro [replikace fyzický počítač tooAzure](site-recovery-vmware-to-azure.md).

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Podporované operační systémy

Obnovení lokality lze použít toomigrate EC2 instancí s některým z následujících operačních systémů hello:

- Windows (pouze 64bitová verze)
    - Windows Server 2008 R2 SP1 + (Citrix PV ovladače nebo pouze ovladače AWS PV. **Nejsou podporovány instance systémem RedHat PV ovladače**) Windows serveru 2012 systému Windows Server 2012 R2
- Linux (pouze 64bitová verze)
    - Red Hat Enterprise Linux 6.7 (pouze HVM virtualizované instance)

## <a name="prerequisites"></a>Požadavky

Zde je nutné pro toto nasazení:

* **Konfigurační server**: virtuální počítač Amazon EC2, systém Windows Server 2012 R2 je nasazen jako hello konfigurační server. Ve výchozím nastavení hello součásti Azure Site Recovery (procesový server a hlavní cílový server) jsou nainstalovány při nasazování hello konfigurační server. Tento článek popisuje hello kroky pro migraci v hello portál Azure a jsou založené na hello pokyny [Další informace](site-recovery-components.md)

* **Instance EC2**: hello Amazon EC2 instancí virtuálních počítačů, které chcete toomigrate.

## <a name="deployment-steps"></a>Kroky nasazení

1. Vytvořte trezor služby Recovery Services.
2. Hello skupiny zabezpečení vašich instancí EC2 by měl mít nakonfigurovaná pravidla tooallow komunikace mezi instance EC2 hello chcete toomigrate a hello instanci, na který budete toodeploy hello konfigurační Server.

3. Na hello stejné Amazon virtuální privátní Cloud jako vaše instance EC2 nasadit server konfigurace automatické obnovení systému. Odkazovat hello VMware / fyzické tooAzure požadavky pro nasazení požadavky na konfiguraci serveru.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Jakmile konfigurační server je nasazena v AWS a zaregistrována trezoru služeb zotavení, byste měli vidět hello konfigurační server a procesový server v rámci infrastruktury Site Recovery, jak je uvedeno níže:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. Poté, co nasadíte hello konfigurační server (to může trvat až too15 minustes pro něj tooappear), ověřte, že může komunikovat se službou hello virtuálních počítačů, které chcete toomigrate.

6. [Nakonfigurování nastavení replikace](site-recovery-setup-replication-settings-vmware.md).

7. Povolení replikace: povolení replikace pro virtuální počítače chcete toomigrate hello. Může zjišťovat hello EC2 instancí pomocí hello privátní IP adresy, které můžete získat z konzoly EC2 hello.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. Poté, co byly chráněny hello EC2 instance a tooAzure hello replikace skončí, [spustit testovací převzetí služeb](site-recovery-test-failover-to-azure.md) toovalidate výkon aplikace v Azure.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Pro každý virtuální počítač spusťte převzetí služeb při selhání z AWS tooAzure. Volitelně můžete vytvořit plán obnovení a převzetí služeb, toomigrate spustit více virtuálních počítačů z AWS tooAzure. [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.

10. Pro migraci nepotřebujete toocommit převzetí služeb při selhání. Místo toho vybrat možnost provedení migrace hello pro každý počítač má toomigrate. Hello dokončení migrace akce dokončí se proces migrace hello odebere replikaci pro počítač hello a zastaví Site Recovery fakturace pro počítač hello.

    ![Migrace](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Další kroky

- [Příprava migrované počítače tooenable replikace](site-recovery-azure-to-azure-after-migration.md) tooanother oblast potřebovat obnovení po havárii.
- Začněte chránit svoje úlohy pomocí [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md).
