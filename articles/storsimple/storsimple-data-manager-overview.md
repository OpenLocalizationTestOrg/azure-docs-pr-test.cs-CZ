---
title: "Přehled Azure StorSimple Data Manager aaaMicrosoft | Microsoft Docs"
description: "Poskytuje přehled hello služby StorSimple Manager dat (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 5d29f7d26be9f2b36857526bdea770d991cece6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>Přehled Data Manager zařízení StorSimple (soukromém náhledu).

## <a name="overview"></a>Přehled

Microsoft Azure StorSimple je řešení hybridní cloudové úložiště, které adresy hello složitosti nestrukturovaných dat běžně spojovaných se sdílenými složkami. StorSimple používá úložiště v cloudu jako rozšíření hello místní řešení a automaticky úrovně dat napříč hello místní úložiště a úložiště v cloudu. Integrovat ochranu dat, místní a cloudových snímků, eliminuje nutnost hello rozrůstající infrastruktury úložiště. Archivace a zotavení po havárii je také bezproblémové hello cloudu, který funguje jako mimo pracoviště.

Hello služba transformace dat, která Představujeme v tomto dokumentu, umožní vám tooseamlessly přístup hello StorSimple dat v cloudu hello. Tato služba poskytuje rozhraní API tooextract data ze zařízení StorSimple a je k dispozici tooother Azure services v formáty, které můžete snadno využívat. Hello formátů podporovaných v této verzi preview jsou Azure BLOB a prostředky Azure Media Services. Tato transformace umožňuje vám tooeasily přenosová až službám, jako je Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search toooperate data na místní zařízení řady StorSimple 8000.

Podrobný Blokový diagram ilustrující to jsou uvedeny níže.

![Vysokoúrovňový diagram](./media//storsimple-data-manager-overview/high-level-diagram.png)

Tento dokument popisuje, jak zaregistrovat privátní Preview verzi této služby. Také vysvětluje, jak lze pomocí této aplikace toowrite služby, které používají StorSimple data a jinými službami Azure v cloudu hello.

## <a name="sign-up-for-data-manager-preview"></a>Zaregistrujte si Data Manager preview
Ještě než si zaregistrujete služby hello Data Manager, zkontrolujte hello následující požadavky.

### <a name="prerequisites"></a>Požadavky

Toto cvičení předpokládá, že máte
* Aktivní předplatné Azure.
* zaregistrované zařízení řady StorSimple 8000 tooa přístup
* všechny hello klíče přidružené zařízení řady StorSimple 8000 hello.

### <a name="sign-up"></a>Registrace

Hello StorSimple Data Manager je v privátní Preview verzi. Proveďte následující kroky toosign pro privátní Preview verzi této služby hello:

1.  Přihlaste se k hello portál Azure s příponou hello StorSimple Manager dat na: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Použijte toolog vaše přihlašovací údaje Azure v.

2.  Klikněte na tlačítko hello  **+**  toocreate ikonu služby. Klikněte na tlačítko **úložiště** a pak klikněte na **najdete v článku všechny** v okně hello, které se otevře.

    ![Ikona Data Manager StorSimple vyhledávání](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. Zobrazí ikona hello StorSimple Data Manager.

    ![Vyberte ikonu pro StorSimple Data Manager](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. Klikněte na ikonu StorSimple Manager dat a pak klikněte na **vytvořit**. Vyberte předplatné hello má tooenable hello privátní Preview verzi a potom klikněte na **zapsat se!**

    ![Zapsat se](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Tím se odešle požadavek tooonboard můžete. Nemůžeme se zaváděním je co nejdříve. Když je vaše předplatné povolená, můžete vytvořit služby StorSimple Data Manager.

6. tooeasily přístup služby StorSimple Manager dat hello, klikněte na ikonu hvězdičky toopin hello ho tooyour Oblíbené položky.

    ![Správci StorSimple Data Access](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Další kroky

[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md).
