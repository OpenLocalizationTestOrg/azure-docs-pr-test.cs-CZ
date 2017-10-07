---
title: "zásady zálohování řady StorSimple 8000 aaaManage | Microsoft Docs"
description: "Vysvětluje, jak můžete použít toocreate služby StorSimple Manager zařízení hello a spravovat ručního zálohování, plánů zálohování a uchovávání záloh na zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 7c56365abb6ba69d02008829ca6ae703d4632705
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-toomanage-backup-policies"></a>Použít službu StorSimple Manager zařízení hello ve službě Azure portálu toomanage zásady zálohování


## <a name="overview"></a>Přehled

Tento kurz popisuje, jak toouse hello služby StorSimple Manager zařízení **zálohování zásad** okno toocontrol zálohování procesy a uchovávání záloh pro formátujte svazky zařízení StorSimple. Také popisuje, jak toocomplete ručního zálohování.

Při zálohování svazku, můžete toocreate místní snímek nebo cloudový snímek. Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek. Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní. Pokud si zvolíte tootake místní snímky, doporučujeme trvat méně denní tooback snímky hello nejnovější stav, uchovávány denně a pak odstraňte je.

Pokud pořídíte snímek cloudu místně vázaný svazek, je zkopírovat pouze hello změnit dat toohello v cloudu, kde je s odstraněním duplicitních dat a komprimované.

## <a name="hello-backup-policy-blade"></a>okno zásady zálohování Hello

Hello **zálohování zásad** okno pro zařízení StorSimple můžete zásady zálohování toomanage a plán místních a cloudových snímků. Zásady zálohování jsou použité tooconfigure plánů zálohování a uchovávání záloh pro kolekci svazků. Zásady zálohování umožňují tootake snímek více svazků najednou. To znamená, že hello zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.

výpis tabulkové zásady zálohování Hello také umožňuje vám toofilter hello existující zásady zálohování pro jeden nebo více hello následující pole:

* **Název zásady** – hello název spojený s hello zásad. Zahrnout Hello různé typy zásad:

  * Naplánované zásady, které jsou explicitně vytvořené uživatelem hello.
  * Importovat zásady, které byly původně vytvořil v hello StorSimple Snapshot Manager. Tyto mají značku, která popisuje hello StorSimple Snapshot Manager hostitele, který hello zásady, které byly naimportovány z.

  > [!NOTE]
  > Automatická nebo výchozí zásady zálohování již není povoleno v době hello vytváření svazků.

* **Poslední úspěšná zálohy** – hello datum a čas hello poslední úspěšné zálohy pořízené touto zásadou.

* **Další zálohování** – hello datum a čas hello další naplánované zálohování, bude inicializován pomocí těchto zásad.

* **Svazky** – hello svazky, které jsou spojené s hello zásadami. Všechny svazky hello přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.

* **Plány** – hello počet plány přidružené zásady zálohování hello.

jsou často používané Hello operace, které můžete provést pro zásady zálohování:

* Přidání zásady zálohování
* Přidat nebo změnit plán
* Přidat nebo odebrat svazku
* Odstranit zásady zálohování
* Proveďte ruční zálohy

## <a name="add-a-backup-policy"></a>Přidání zásady zálohování

Přidáte zásady zálohování tooautomatically plán zálohování. Při prvním vytvoření svazku, neexistuje žádný výchozí zásady zálohování přidružené k svazku. Potřebujete tooadd a přiřadit zásady zálohování dat tooprotect svazku.

Proveďte následující kroky v hello Azure portálu tooadd zásady zálohování pro zařízení StorSimple hello. Po přidání hello zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Přidat nebo změnit plán

Můžete přidat nebo změnit plán, který je připojený tooan existující zásady zálohování v zařízení StorSimple. Proveďte následující kroky v hello Azure portálu tooadd hello nebo změna plánu.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Přidat nebo odebrat svazku

Můžete přidat nebo odebrat svazek přiřazené zásady zálohování tooa zařízení StorSimple. Proveďte následující kroky v hello Azure portálu tooadd hello nebo odeberte svazek.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Odstranit zásady zálohování

Proveďte následující kroky v hello Azure portálu toodelete zásady zálohování v zařízení StorSimple hello.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Proveďte ruční zálohy

Proveďte následující kroky hello Azure portálu toocreate na vyžádání (ručně) zálohování pro samostatný svazek hello.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Další kroky

Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

