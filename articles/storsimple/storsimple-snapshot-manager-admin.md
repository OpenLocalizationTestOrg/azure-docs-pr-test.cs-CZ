---
title: "aaaStorSimple správy Snapshot Manager | Microsoft Docs"
description: "Poskytuje přehled a odkazy toomore informace o úlohách správy řešení StorSimple Snapshot Manager a pracovních postupů."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 1cdbb61d-bd16-4be4-ade2-ceab11508acb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2016
ms.author: v-sharos
ms.openlocfilehash: d875f2efbdeb844b412cf8d9f1f971f18da7526e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooadminister-your-storsimple-solution"></a>Pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister

## <a name="overview"></a>Přehled
Snapshot Manager zařízení StorSimple je modul snap-in konzoly Microsoft Management Console (MMC), který zjednodušuje ochrany dat a správu záloh v prostředí Microsoft Azure StorSimple. S StorSimple Snapshot Manager můžete spravovat Microsoft Azure StorSimple data v datovém centru hello a v cloudu hello jako řešení jedné integrované úložiště, proto kvůli zjednodušení zálohování procesů a snížení nákladů.

Konzola pro centrální správu Hello StorSimple Snapshot Manager umožňuje toocreate konzistentní, v okamžiku záložní kopie místní a Cloudová data. Například můžete použít konzolu hello:

* Konfigurace, zálohovat a odstraňte svazky.
* Konfigurace svazku skupiny tooensure zálohovaných dat je konzistentní s aplikací.
* Spravovat zásady zálohování tak, aby se data zálohují na předem určeného plánu.
* Vytvořte nezávislé kopie dat, které můžete ukládat v cloudu hello a použít pro obnovení po havárii.

Tento článek obsahuje tootutorials odkazy, které popisují Snapshot Manager zařízení StorSimple a jak toouse ho toocomplete úloh správy systému a pracovních postupů.

* Další informace o součástech Snapshot Manager zařízení StorSimple a architektura najdete v tématu [co je StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md) 
* toodownload StorSimple Snapshot Manager přejděte příliš[stránky pro stažení StorSimple Snapshot Manager hello](https://www.microsoft.com/download/details.aspx?id=44220).
* Postupy nasazení zařízení StorSimple Snapshot Manager, přejděte příliš[nasazení zařízení StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

> [!NOTE]
> Nelze použít StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuální pole (také označované jako StorSimple místní virtuální zařízení).


## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>Úlohy StorSimple Snapshot Manager a pracovních postupů
Můžete použít hello toomonitor Snapshot Manager zařízení StorSimple a spravovat aktuální, plánované a dokončené úlohy zálohování. Kromě toho StorSimple Snapshot Manager poskytuje katalog zálohování too64 byla dokončena. Můžete použít hello katalogu toofind a obnovit svazky nebo jednotlivé soubory. 

| Pokud chcete, aby tooDO THIS... | TENTO KURZ POUŽIJTE... |
|:--- |:--- |
| Další informace o Snapshot Manager zařízení StorSimple |[Co je StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md) |
| Nainstalujte Snapshot Manager zařízení StorSimple<br>Přeinstalujte Snapshot Manager zařízení StorSimple<br>Odebrat Snapshot Manager zařízení StorSimple |[Nasazení Snapshot Manager zařízení StorSimple](storsimple-snapshot-manager-deployment.md) |
| Použití StorSimple Snapshot Manager nabídky a funkce:<ul><li>Panel nabídek</li><li>Panel nástrojů</li><li>Podokno oboru</li><li>Podokno výsledků</li><li>Podokna akcí</li><li>Navigace klávesnice a zkratky</li></ul> |[Uživatelské rozhraní Snapshot Manager zařízení StorSimple](storsimple-use-snapshot-manager.md) |
| Použijte hello běžné konzoly MMC funkce obsažené v StorSimple Snapshot Manager:<ul><li>Zobrazení</li><li>Nové okno</li><li>Aktualizace</li><li>Export seznamu</li><li>Nápověda</li></ul> |[Použít hello konzoly MMC nabídce Akce v Snapshot Manager zařízení StorSimple](storsimple-snapshot-manager-mmc-menu.md) |
| Přidat nebo nahradit zařízení<br>Připojení zařízení<br>Ověřte importované svazku skupiny<br>Aktualizujte připojená zařízení<br>Ověřování zařízení<br>Zobrazení podrobností o zařízeních<br>Odstranění konfigurace zařízení<br>Změnit heslo k zařízení<br>Nahraďte zařízení se nezdařilo<br> |[Použití tooconnect Snapshot Manager zařízení StorSimple a správě zařízení StorSimple](storsimple-snapshot-manager-manage-devices.md) |
| Připojit svazky<br>Zobrazení informací o svazcích<br>Odstranění svazku<br>Prohledat znovu svazky<br>Konfigurace a zálohovat vytvoření základního svazku<br>Konfigurace a zálohování dynamické zrcadleného svazku |[Použijte tooview StorSimple Snapshot Manager a správa svazků](storsimple-snapshot-manager-manage-volumes.md) |
| Zobrazit svazku skupiny<br>Vytvořte skupinu svazku<br>Zálohování svazku skupiny<br>Úprava skupiny svazku<br>Odstranit skupinu svazku |[Použít toocreate StorSimple Snapshot Manager a Správa skupin svazku](storsimple-snapshot-manager-manage-volume-groups.md) |
| Vytvořit zásady zálohování <br>Upravit zásady zálohování<br>Odstranit zásady zálohování |[Použít toocreate Snapshot Manager zařízení StorSimple a spravovat zásady zálohování](storsimple-snapshot-manager-manage-backup-policies.md) |
| Zobrazit a spravovat naplánované úlohy zálohování<br>Zobrazit a spravovat poslední úlohy zálohování<br>Zobrazit a spravovat právě probíhajících úloh zálohování |[Použít tooview Snapshot Manager zařízení StorSimple a spravovat úlohy zálohování](storsimple-snapshot-manager-manage-backup-jobs.md) |
| Obnovení svazku<br>Klonování svazku nebo svazku skupiny<br>Odstranit zálohy<br>Obnovit soubor<br>Obnovení databáze hello Snapshot Manager zařízení StorSimple |[Použití StorSimple Snapshot Manager toomanage hello zálohování katalogu](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Další kroky
[Stáhnout Snapshot Manager zařízení StorSimple](https://www.microsoft.com/download/details.aspx?id=44220).

