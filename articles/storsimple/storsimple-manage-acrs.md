---
title: "aaaManage záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak řízení přístupu toouse záznamy toodetermine (ACRs), které hostitele může připojit tooa svazek v zařízení StorSimple hello."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a1e718c2679301b34221a233557a1eaae869a94f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a>Použití služby StorSimple Manager hello, toomanage záznamy řízení přístupu
## <a name="overview"></a>Přehled
Záznamy řízení přístupu (ACRs) umožňují toospecify, které hostitele může připojit tooa svazek v zařízení StorSimple hello. ACRs jsou nastavené určité svazku tooa a obsahovat hello iSCSI (IQN) kvalifikované názvy hostitelů hello. Když se hostitel pokusí tooconnect tooa svazku, zkontroluje zařízení hello hello ACR přidruženého tento svazek pro název IQN hello a pokud je nalezen, pak hello připojení. řízení přístupu Hello zaznamenává část hello **konfigurace** stránka zobrazuje všechny záznamy řízení přístupu hello hello odpovídající IQN hello hostitelů.

Tento kurz vysvětluje hello následující běžné úlohy související s ACR:

* Přidání záznamu o řízení přístupu 
* Upravit záznam řízení přístupu 
* Odstranit záznam řízení přístupu 

> [!IMPORTANT]
> * Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello. 
> * Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.
> 
> 

## <a name="add-an-access-control-record"></a>Přidání záznamu o řízení přístupu
Použít službu StorSimple Manager hello **konfigurace** stránka tooadd ACRs. Obvykle přidružíte jednu ACR jeden svazek.

Proveďte následující kroky tooadd ACR hello.

#### <a name="tooadd-an-access-control-record"></a>tooadd záznam řízení přístupu
1. Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko hello **konfigurace** kartě.
2. V tabulkovém výpis pod hello **záznamy řízení přístupu**, dodávky **název** acr.
3. Zadejte název IQN hello hostitele s Windows v rámci **název iniciátoru iSCSI**. hello tooget názvu IQN hostitele vašeho systému Windows Server hello následující:
   
   * Na hostiteli s Windows spusťte iniciátor iSCSI společnosti Microsoft hello.
   * V hello **vlastnosti iniciátoru iSCSI** okně na hello **konfigurace** , vyberte a zkopírujte řetězec hello z hello **název iniciátoru** pole.
   * Vložte tento řetězec hello **název iniciátoru iSCSI** pole v tabulce ACRs hello v hello portál Azure classic.
4. Klikněte na tlačítko **Uložit** toosave hello nově vytvořený ACR. Hello tabulkové výpis bude možné aktualizované tooreflect přidání.

## <a name="edit-an-access-control-record"></a>Upravit záznam řízení přístupu
Použít hello **konfigurace** stránku hello ACRs Azure tooedit portálu classic. 

> [!NOTE]
> Můžete upravit pouze ACRs, které nejsou aktuálně používá. tooedit přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.
> 
> 

Proveďte následující kroky tooedit ACR hello.

#### <a name="tooedit-an-access-control-record"></a>tooedit záznam řízení přístupu
1. Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko hello **konfigurace** kartě.
2. V tabulkovém seznamu hello záznamů hello řízení přístupu, pozastavte ukazatel myši nad hello ACR chcete toomodify.
3. Zadejte nový název nebo název IQN hello ACR.
4. Klikněte na tlačítko **Uložit** toosave hello upravit ACR. Hello tabulkové výpis bude možné aktualizované tooreflect tuto změnu.

## <a name="delete-an-access-control-record"></a>Odstranit záznam řízení přístupu
Použít hello **konfigurace** stránku hello ACRs Azure toodelete portálu classic. 

> [!NOTE]
> Lze odstranit pouze ACRs, které nejsou aktuálně používá. toodelete přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.
> 
> 

Proveďte následující kroky toodelete záznam řízení přístupu hello.

#### <a name="toodelete-an-access-control-record"></a>toodelete záznam řízení přístupu
1. Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko hello **konfigurace** kartě.
2. V hello tabulkové výpis hello záznamy řízení přístupu (ACRs), přejděte myší hello ACR chcete toodelete.
3. Ikony odstranění (**x**) se zobrazí v hello extrémně pravém sloupci pro hello ACR, kterou jste vybrali. Klikněte na tlačítko hello **x** ikonu toodelete hello ACR.
4. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** toocontinue s hello odstranění. tabulkový výčet Hello bude aktualizované tooreflect hello odstranění.

## <a name="next-steps"></a>Další kroky
* Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

