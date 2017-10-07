---
title: "aaaManage záznamy řízení přístupu pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak řízení přístupu toomanage záznamy toodetermine (ACRs), které hostitele může připojit tooa svazek na hello pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 608fdf72413761ce3c9c4bf297a748489c415685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-access-control-records-for-storsimple-virtual-array"></a>Pomocí Správce zařízení StorSimple toomanage záznamy řízení přístupu pro pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Záznamy řízení přístupu (ACRs) umožňují toospecify, které hostitele může připojit tooa svazek na hello pole virtuální zařízení StorSimple (také označované jako hello místní virtuální zařízení StorSimple). ACRs jsou nastavené určité svazku tooa a obsahovat hello iSCSI (IQN) kvalifikované názvy hostitelů hello. Když se hostitel pokusí tooconnect tooa svazek, zařízení hello ověří hello ACR přidružený tento svazek pro název IQN hello, a pokud je nalezena shoda, pak hello připojení. Hello **záznamy řízení přístupu** okno v rámci hello **konfigurace** části služby Správce zařízení zobrazí všechny záznamy řízení přístupu hello s hello odpovídající IQN hello hostitelů.

![Spravovat přístup k záznamům v ovládací prvek](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Tento kurz vysvětluje hello následující běžné úlohy související s ACR:

* Získání názvu IQN hello
* Přidání záznamu o řízení přístupu
* Upravit záznam řízení přístupu
* Odstranit záznam řízení přístupu

> [!IMPORTANT]
> 
> * Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.
> * Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.


## <a name="get-hello-iqn"></a>Získání názvu IQN hello

Proveďte následující kroky tooget hello názvu IQN hostitele se systémem Windows Server 2012 Windows hello.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Přidat ACR

Používáte **záznamy řízení přístupu** okno v rámci hello **konfigurace** část vaší tooadd služby StorSimple Manager zařízení ACRs. Obvykle přidružíte jednu ACR jeden svazek.

Informace o přiřazení ACR svazku, přejděte příliš[přidat svazek](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.


Proveďte následující kroky tooadd ACR hello.

#### <a name="tooadd-an-acr"></a>tooadd ACR

1. Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.
2. V hello **záznamy řízení přístupu** okně klikněte na tlačítko **přidat**.
3. V hello **přidat ACR** okně hello následující:
   
    1. Zadejte **Název** záznamu ACR.
    
    2. V části **název iniciátoru iSCSI**, zadejte název IQN hello hostitele s Windows. hello tooget názvu IQN hostitele vašeho systému Windows Server hello následující:
   
    3. Na hostiteli s Windows spusťte iniciátor iSCSI společnosti Microsoft hello. V okně hello při vlastnosti iniciátoru iSCSI na hello **konfigurace** , vyberte a zkopírujte řetězec hello z hello **název iniciátoru** pole.
    Vložte tento řetězec hello **IQN** pole hello **přidat ACR** okno.
   
    6. Klikněte na tlačítko **přidat** tooadd hello ACR.  
   
        ![Přidat záznamy řízení přístupu](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. Hello tabulkové výpis je aktualizovaný tooreflect přidání.

## <a name="edit-an-acr"></a>Upravit ACR

Použít hello **záznamy řízení přístupu** okno v rámci hello **konfigurace** části služby Správce zařízení v Azure portálu tooedit ACRs hello.

> [!NOTE]
> ACR, který je aktuálně používán, byste neměli upravovat. tooedit přidruženého ACR svazku, který je aktuálně používán, byste měli nejdřív vzít hello svazek offline.


Proveďte následující kroky tooedit ACR hello.

#### <a name="tooedit-an-acr"></a>tooedit ACR

1. Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** části **záznamy řízení přístupu**.
2. V hello **záznamy řízení přístupu** okno z hello tabulkové seznam hello záznamy řízení přístupu, klikněte dvakrát na hello ACR chcete toomodify.
3. V hello **upravit záznamy řízení přístupu** okně hello následující:
   
    1. Zadejte hello IQN pro hello ACR.
   
    2. Klikněte na tlačítko **Uložit** hello horní části okna toosave hello hello upravit ACR. Zobrazí následující potvrzující zpráva hello:
   
        ![Úpravy záznamů o řízení přístupu](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Odstranit záznam řízení přístupu

Použít hello **konfigurace** stránku hello Azure portálu toodelete ACRs.

> [!NOTE]
> 
> * Nedoporučuje se mazat ACR, který je aktuálně používán. toodelete přidruženého ACR svazku, který je aktuálně používán, byste měli nejdřív vzít hello svazek offline.
> * Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.


Proveďte následující kroky toodelete záznam řízení přístupu hello.

#### <a name="toodelete-an-access-control-record"></a>toodelete záznam řízení přístupu

1. Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** části **záznamy řízení přístupu**.

2. V hello **záznamy řízení přístupu** okno z hello tabulkové seznam hello záznamy řízení přístupu, klikněte dvakrát na hello ACR chcete toodelete.

3. V okně hello úpravy přístupu řízení záznamy, klikněte na tlačítko **odstranit**.
   
    ![Odstranit ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **odstranit** toocontinue s hello odstranění. tabulkový výčet Hello je aktualizovaný tooreflect hello odstranění.
   
   ![Zpráva upozornění](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Další kroky

* Další informace o [přidání svazků a konfigurace ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

