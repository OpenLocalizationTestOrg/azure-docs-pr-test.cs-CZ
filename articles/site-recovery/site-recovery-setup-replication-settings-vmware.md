---
title: "aaaSet nastavení replikace pro Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toodeploy Site Recovery tooorchestrate replikace, převzetí služeb při selhání a obnovení virtuálních počítačů technologie Hyper-V v nástroji VMM cloudů tooAzure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 618e92e42411732a2a1bb75c5e5ea8a433cd7d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-replication-policy-for-vmware-tooazure"></a>Správa zásad replikace pro VMware tooAzure


## <a name="create-a-replication-policy"></a>Vytvoření zásady replikace

1. Vyberte **Spravovat** > **Infrastruktura Site Recovery**.
2. V části **Pro VMware a fyzické počítače** vyberte **Zásady replikace**.
3. Vyberte **+Zásada replikace**.

    ![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Zadejte název zásady hello.

5. V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení. Když toto omezení průběžná replikace překročí, vygenerují se upozornění.
6. V **uchování bodu obnovení**, zadejte (v hodinách) hello trvání intervalem pro uchovávání dat hello pro každý bod obnovení. Chráněné počítače může být obnovena tooany bodu v rámci časového období uchování.

    > [!NOTE]
    > Pro úložiště replikované toopremium počítače je podporováno až too24 hodin uchování. Pro úložiště replikované toostandard počítače je podporováno až too72 hodin uchování.

    > [!NOTE]
    > Zásada replikace pro navrácení služeb po obnovení se vytvoří automaticky.

7. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (v minutách) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím.

8. Klikněte na **OK**. Hello zásad by měl být vytvořen v 30 sekund too60.

![Generování zásad replikace](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Přidružení konfiguračního serveru k zásadě replikace
1. Zvolte toowhich zásady replikace hello chcete tooassociate hello konfigurační server.
2. Klikněte na **Přidružit**.
![Přidružení konfiguračního serveru](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Vyberte konfigurační server hello hello seznamu serverů.
4. Klikněte na **OK**. Hello konfigurační server by měl spojené v jedné tootwo minut.

![Přidružení konfiguračního serveru](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Úprava zásady replikace
1. Vyberte zásadu replikace hello, pro které chcete tooedit nastavení replikace.
![Úprava zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klikněte na **Upravit nastavení**.
![Úprava nastavení zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Změňte nastavení hello podle vašim potřebám.
4. Klikněte na **Uložit**. Hello zásad by měla být uložena ve dvou minut toofive, v závislosti na tom, kolik virtuálních počítačů používají tuto zásadu replikace.

![Uložení zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>Zrušení přidružení konfiguračního serveru k zásadě replikace
1. Zvolte toowhich zásady replikace hello chcete tooassociate hello konfigurační server.
2. Klikněte na **Zrušit přidružení**.
3. Vyberte konfigurační server hello hello seznamu serverů.
4. Klikněte na **OK**. v jedné tootwo minut by měl zrušit její přidružení Hello konfigurační server.

    > [!NOTE]
    > Konfigurační server nemůže zrušit přidružení, pokud je alespoň jednu položku replikovaná pomocí zásad hello. Zajistěte, aby nebyly nalezeny žádné replikované položky pomocí zásad hello před zrušíte přidružení hello konfigurační server.

## <a name="delete-a-replication-policy"></a>Odstranění zásady replikace

1. Vyberte zásadu replikace hello, které chcete toodelete.
2. Klikněte na **Odstranit**. 30 sekund too60 je potřeba odstranit zásady Hello.

    > [!NOTE]
    > Zásady replikace nelze odstranit, pokud má alespoň jeden server přidružené tooit konfigurace. Ujistěte se, že neexistují žádné replikované položky pomocí zásad hello a odstranění všech hello související konfigurační servery před odstraněním hello zásad.
