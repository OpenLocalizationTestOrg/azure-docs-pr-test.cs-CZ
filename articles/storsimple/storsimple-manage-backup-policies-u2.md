---
title: "aaaManage zásady zálohování pro vaše zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít toocreate služby StorSimple Manager hello a spravovat ručního zálohování, plánů zálohování a uchovávání záloh."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 7b01f29a8d8a096d9890c8406557021317b9baff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies-update-2"></a>Použít hello StorSimple Manager service toomanage zásady zálohování (Update 2)
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Přehled
Tento kurz popisuje, jak toouse hello služby StorSimple Manager **zásady zálohování** stránka toocontrol procesy zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple. Také popisuje, jak toocomplete ručního zálohování.

Při zálohování svazku, můžete toocreate místní snímek nebo cloudový snímek. Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek. Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní. Pokud si zvolíte tootake místní snímky, doporučujeme trvat méně denní tooback snímky hello nejnovější stav, uchovávány denně a pak odstraňte je.

Pokud pořídíte snímek cloudu místně vázaný svazek, je zkopírovat pouze hello změnit dat toohello v cloudu, kde je s odstraněním duplicitních dat a komprimované. 

## <a name="hello-backup-policies-page"></a>Zásady zálohování stránku Hello
Hello **zásady zálohování** stránka vám umožní zásady zálohování toomanage a plán místních a cloudových snímků. (Zásady zálohování jsou použité tooconfigure plánů zálohování a uchovávání záloh pro kolekci svazků). Zásady zálohování umožňují tootake snímek více svazků najednou. To znamená, že hello zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání. Hello **zásady zálohování** zobrazuje stránku hello zásady zálohování, jejich typy, hello přidružené svazky, hello počet záloh uchována a hello možnost tooenable tyto zásady.

Hello **zásady zálohování** stránka vám také umožní toofilter hello existující zásady zálohování pro jednu nebo více hello následující pole:

* **Název zásady** – hello název spojený s hello zásad. Zahrnout Hello různé typy zásad:
  
  * Naplánované zásady, které jsou explicitně vytvořené uživatelem hello.
  * Automatické zásady, které vytvářejí, když hello výchozí zálohování pro tuto možnost svazek byl povolen v době hello vytváření svazků. Tyto zásady jsou pojmenované jako *VolumeName*_výchozí kde *VolumeName* odkazuje toohello název hello svazek StorSimple nakonfiguroval hello uživatel v hello portál Azure classic. Zásady automatické Hello za následek denní cloudových snímků od okamžiku zařízení 22:30.
  * Importovat zásady, které byly původně vytvořil v hello StorSimple Snapshot Manager. Tyto mají značku, která popisuje hello StorSimple Snapshot Manager hostitele, který hello zásady, které byly naimportovány z.
* **Svazky** – hello svazky, které jsou spojené s hello zásadami. Všechny svazky hello přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.
* **Poslední úspěšná zálohy** – hello datum a čas hello poslední úspěšné zálohy pořízené touto zásadou.
* **Další zálohování** – hello datum a čas hello další naplánované zálohování, bude inicializován pomocí těchto zásad.
* **Plány** – hello počet plány přidružené zásady zálohování hello.

jsou často používané Hello operace, které můžete provádět z této stránky:

* Přidání zásady zálohování 
* Přidat nebo změnit plán 
* Odstranit zásady zálohování 
* Proveďte ruční zálohy 
* Vytvořit vlastní zásady zálohování s více svazků a plány 

## <a name="add-a-backup-policy"></a>Přidání zásady zálohování
Přidáte zásady zálohování tooautomatically plán zálohování. Proveďte následující kroky v hello Azure classic portálu tooadd zásady zálohování pro zařízení StorSimple hello. Po přidání hello zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Dostupné video](./media/storsimple-manage-backup-policies-u2/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které ukazuje, jak toocreate místní nebo cloudové zálohování zásad, [zde](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Přidat nebo změnit plán
Můžete přidat nebo změnit plán, který je připojený tooan existující zásady zálohování v zařízení StorSimple. Proveďte následující kroky v hello Azure classic portálu tooadd hello nebo změna plánu.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Odstranit zásady zálohování
Proveďte následující kroky v hello Azure classic portálu toodelete zásady zálohování v zařízení StorSimple hello.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Proveďte ruční zálohy
Proveďte následující kroky hello Azure classic portálu toocreate na vyžádání (ručně) zálohování pro samostatný svazek hello.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Vytvořit vlastní zásady zálohování s více svazků a plány
Proveďte následující kroky v hello Azure classic portálu toocreate vlastní zásady zálohování, který má více svazků a plány hello.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

