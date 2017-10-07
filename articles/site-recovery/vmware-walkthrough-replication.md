---
title: "aaaSet se zásada replikace pro virtuální počítač VMware tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset si zásadu replikace při replikaci virtuálních počítačů VMware tooAzure úložiště"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 12870f3f98a4dd412221b817581403cd4bf7a76d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-tooazure"></a>Krok 9: Nastavení zásad replikace pro virtuální počítač VMware tooAzure replikace


Tento článek popisuje, jak tooset si zásadu replikace při replikaci virtuálních počítačů VMware tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="configure-a-policy"></a>Konfigurace zásad

Vám zajistí rychlý přehled videa, než začnete:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klikněte na tlačítko toocreate novou zásadu replikace, **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
2. V **vytvořit zásadu replikace**, zadejte název zásady.
3. V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení. Tato hodnota určuje, jak často jsou vytvořeny body obnovení data. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
4. V **uchování bodu obnovení**, určit, jak dlouho (v hodinách) pro každý bod obnovení je okno uchování hello. Replikované virtuální počítače mohou být obnovena tooany bodu v okně. Až too24 čas uchování je podporována pro počítače replikují toopremium úložiště a 72 hodin, než standardní úložiště.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** toocreate hello zásad.

    ![Zásady replikace](./media/vmware-walkthrough-replication/gs-replication2.png)
8. Když vytvoříte novou zásadu je přidružená automaticky hello konfigurační server. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například pokud hello zásady replikace je **rep zásad** bude hello navrácení služeb po obnovení zásad **rep. zásady navrácení**. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 10: instalace služby Mobility hello](vmware-walkthrough-install-mobility.md)
