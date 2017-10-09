---
title: "aaaManage záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak řízení přístupu toouse záznamy toodetermine (ACRs), které hostitele může připojit tooa svazek v zařízení StorSimple hello."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: cf532206e2c0bc49da853663ba34ae993ec2981d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a>Použití služby StorSimple Manager hello, toomanage záznamy řízení přístupu

## <a name="overview"></a>Přehled
Záznamy řízení přístupu (ACRs) umožňují toospecify, které hostitele může připojit tooa svazek v zařízení StorSimple hello. ACRs jsou nastavené určité svazku tooa a obsahovat hello iSCSI (IQN) kvalifikované názvy hostitelů hello. Když se hostitel pokusí tooconnect tooa svazku, zkontroluje zařízení hello hello ACR přidruženého tento svazek pro název IQN hello a pokud je nalezen, pak hello připojení. Hello záznamy řízení přístupu v hello **konfigurace** části okně vaší služby StorSimple Manager zařízení zobrazí všechny záznamy řízení přístupu hello s hello odpovídající IQN hello hostitelů.

Tento kurz vysvětluje hello následující běžné úlohy související s ACR:

* Přidání záznamu o řízení přístupu
* Upravit záznam řízení přístupu
* Odstranit záznam řízení přístupu

> [!IMPORTANT]
> * Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.
> * Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.

## <a name="get-hello-iqn"></a>Získání názvu IQN hello

Proveďte následující kroky tooget hello názvu IQN hostitele se systémem Windows Server 2012 Windows hello.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Přidání záznamu o řízení přístupu
Použít hello **konfigurace** v hello Správce zařízení StorSimple služby okno tooadd ACRs části. Obvykle přidružíte jednu ACR jeden svazek.

Proveďte následující kroky tooadd ACR hello.

#### <a name="tooadd-an-acr"></a>tooadd ACR

1. Přejděte tooyour service Manager zařízení StorSimple, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.
2. V hello **záznamy řízení přístupu** okně klikněte na tlačítko **+ přidat ACR**.

    ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. V hello **přidat ACR** okně hello následující kroky:

    1. Zadáte název acr.
    
    2. Zadejte název IQN hello hostiteli systému Windows Server v části **iSCSI Initiator název IQN ()**.

    3. Klikněte na tlačítko **přidat** toocreate hello ACR.

        ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  Hello nově přidaní že ACR se zobrazí v tabulkovém seznam ACRs hello.

    ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Upravit záznam řízení přístupu
Použít hello **konfigurace** v hello Správce zařízení StorSimple služby okno tooedit ACRs části.

> [!NOTE]
> Doporučujeme proto upravovat pouze ACRs, které nejsou aktuálně používá. tooedit přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.

Proveďte následující kroky tooedit ACR hello.

#### <a name="tooedit-an-access-control-record"></a>tooedit záznam řízení přístupu
1.  Přejděte tooyour service Manager zařízení StorSimple, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/createacr1.png)

2. V hello tabulkové výpis hello záznamy řízení přístupu, klikněte na tlačítko a vyberte hello ACR chcete toomodify.

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr1.png)

3. V hello **záznam řízení přístupu upravit** okno, zadejte jiný hostitelský odpovídající tooanother IQN.

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr2.png)

4. Klikněte na **Uložit**. Po zobrazení výzvy k potvrzení klikněte na **Ano**. 

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr3.png)

5. Budete upozorněni, když je aktualizována hello ACR. tabulkový výčet Hello rovněž aktualizuje tooreflect hello změnu.

   
## <a name="delete-an-access-control-record"></a>Odstranit záznam řízení přístupu
Použít hello **konfigurace** v hello Správce zařízení StorSimple služby okno toodelete ACRs části.

> [!NOTE]
> Lze odstranit pouze ACRs, které nejsou aktuálně používá. toodelete přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.

Proveďte následující kroky toodelete záznam řízení přístupu hello.

#### <a name="toodelete-an-access-control-record"></a>toodelete záznam řízení přístupu
1.  Přejděte tooyour service Manager zařízení StorSimple, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/createacr1.png)

2. V hello tabulkové výpis hello záznamy řízení přístupu, klikněte na tlačítko a vyberte hello ACR chcete toodelete.

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a vyberte **odstranit**.

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Po zobrazení výzvy k potvrzení, zkontrolujte hello informace a pak klikněte na tlačítko **odstranit**.

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. Upozornění se zobrazí po dokončení odstranění hello. tabulkový výčet Hello je aktualizovaný tooreflect hello odstranění.

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [správu svazků zařízení StorSimple](storsimple-8000-manage-volumes-u2.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-8000-manager-service-administration.md).

