---
title: "aaaStorSimple správu service Manager | Microsoft Docs"
description: "Zjistěte, jak hello toomanage hello zařízení StorSimple pomocí služby StorSimple Manager v portálu Azure classic."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 695ebbb785590a0e3d6de4c73125f65b16dcb776
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooadminister-your-storsimple-device"></a>Použít tooadminister služby StorSimple Manager hello zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek popisuje hello rozhraní služby StorSimple Manager, včetně tooconnect tooit hello různé možnosti, které jsou k dispozici a odkazy na konkrétní pracovní postupy toohello, které je možné provádět prostřednictvím tohoto uživatelského rozhraní. V tomto návodu je použít tooboth; Hello StorSimple fyzické a virtuální zařízení hello.

Po přečtení tohoto článku, se naučíte:

* Připojit tooStorSimple Manager service
* Přejděte hello StorSimple Manager uživatelského rozhraní
* Spravovat prostřednictvím hello služby StorSimple Manager zařízení StorSimple

## <a name="connect-toostorsimple-manager-service"></a>Připojit tooStorSimple Manager service
Hello služby StorSimple Manager běží v Microsoft Azure a připojí zařízení StorSimple toomultiple. Můžete použít centrální klasický portál Microsoft Azure spuštěné v prohlížeči toomanage těchto zařízení. tooconnect toohello služby StorSimple Manager hello následující.

#### <a name="tooconnect-toohello-service"></a>tooconnect toohello služby
1. Přejděte příliš[https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. Pomocí svých přihlašovacích údajů účtu Microsoft, přihlaste na portál classic toohello Microsoft Azure (v pravém horním hello hello podokna umístění).
3. Posuňte se dolů hello levé navigační podokně tooaccess hello StorSimple Manager service.

## <a name="navigate-storsimple-manager-service-ui"></a>Přejděte služby StorSimple Manager uživatelského rozhraní
Hello navigační hierarchie pro služby StorSimple Manager hello uživatelského rozhraní se zobrazí v hello následující tabulka.

* **StorSimple Manager** cílová stránka přejdete toohello uživatelského rozhraní stránky na úrovni služby použít tooall zařízení v rámci služby.
* **Zařízení** stránky přejdete toohello úrovni – zařízení uživatelského rozhraní stránky použít tooa konkrétní zařízení.
* **Kontejnery svazků** stránky přejdete toohello svazku stránky, která obsahuje všechny svazky hello související se zařízením.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Navigační hierarchii služby StorSimple Manager
| Cílová stránka | Stránky na úrovni služby | Stránky na úrovni zařízení | Stránky na úrovni zařízení |
| --- | --- | --- | --- |
| Služba StorSimple Manager |Řídicí panel služby |Řídicí panel zařízení | |
| Zařízení → |Monitorování | | |
| Zálohování katalogu |Containers→ svazku |Svazky | |
| Konfigurace (služba) |Zásady zálohování | | |
| Úlohy |Konfigurace (zařízení) | | |
| Výstrahy |Údržby | | |

![Dostupné video](./media/storsimple-manager-service-administration/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které vás provede uživatelské rozhraní služby StorSimple Manager hello, [zde](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Spravovat pomocí služby StorSimple Manager zařízení StorSimple
Hello následující tabulka zobrazuje souhrn všech hello běžné úlohy správy a komplexní pracovní postupy, které lze provést v rámci hello služby StorSimple Manager uživatelského rozhraní. Tyto úlohy jsou uspořádány podle hello uživatelského rozhraní stránky, na kterých se spouští.

Další informace o každém pracovním postupu klikněte na tlačítko hello vhodný postup uvedený v tabulce hello.

#### <a name="storsimple-manager-workflows"></a>Pracovní postupy StorSimple Manager
| Pokud chcete, aby toodo to... | Stránka uživatelského rozhraní přejděte toothis... | Pomocí tohoto postupu. |
| --- | --- | --- |
| Vytvoření služby</br>Odstranění služby</br>Získat registrační klíč služby</br>Znovu vygenerovat registrační klíč služby |Služba StorSimple Manager |[Nasazení služby StorSimple Manager](storsimple-manage-service.md) |
| Změna šifrovacího klíče dat služby hello</br>Protokoly operací hello zobrazení |Řídicí panel StorSimple Manager service → |[Pomocí řídicího panelu služby StorSimple Manager hello](storsimple-service-dashboard.md) |
| Deaktivace zařízení</br>Odstranění zařízení |Zařízení StorSimple Manager služby → |[Deaktivovat nebo odstranit zařízení](storsimple-deactivate-and-delete-device.md) |
| Další informace o havárii obnovení a zařízení převzetí služeb při selhání</br>Převzetí služeb při selhání tooa fyzického zařízení</br>Převzetí služeb při selhání tooa virtuálního zařízení</br>Obchodní kontinuity zotavení po havárii (BCDR) |Zařízení StorSimple Manager služby → |[Převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-device-failover-disaster-recovery.md) |
| Zálohování seznamu pro svazek clusteru</br>Vyberte zálohovací sklad</br>Odstranit zálohovacího skladu |Katalog zálohování StorSimple Manager service → |[Spravovat zálohy](storsimple-manage-backup-catalog.md) |
| Klonování svazku |Katalog zálohování StorSimple Manager service → |[Klonování svazku](storsimple-clone-volume.md) |
| Obnovení zálohovacího skladu |Katalog zálohování StorSimple Manager service → |[Obnovení zálohovacího skladu](storsimple-restore-from-backup-set.md) |
| Informace o účtech úložiště</br>Přidání účtu úložiště</br>Upravit účet úložiště</br>Odstranění účtu úložiště</br>Střídání klíče účtů úložiště |Konfigurace → služby StorSimple Manager |[Správa účtů úložiště](storsimple-manage-storage-accounts.md) |
| O šablonách pásma</br>Přidání šablony šířky pásma</br>Upravit šablonu šířky pásma</br>Odstranit šablonu šířky pásma</br>Použít výchozí šablonu šířky pásma</br>Vytvořit šablonu celodenní šířky pásma, která se spouští v zadanou dobu |Konfigurace → služby StorSimple Manager |[Správa šablon šířky pásma](storsimple-manage-bandwidth-templates.md) |
| Informace o záznamech řízení přístupu</br>Vytvoření záznamů řízení přístupu</br>Upravit záznam řízení přístupu</br>Odstranit záznam řízení přístupu |Konfigurace → služby StorSimple Manager |[Spravovat přístup k záznamům v ovládací prvek](storsimple-manage-acrs.md) |
| Zobrazení podrobností o úloze</br>Zrušení úlohy |Úlohy služby → StorSimple Manager |[Správa úloh](storsimple-manage-jobs.md) |
| Zobrazování oznámení o výstrahách</br>Správa výstrah</br>Zkontrolujte výstrahy |Výstrahy služeb → StorSimple Manager |[Zobrazovat a spravovat výstrahy StorSimple](storsimple-manage-alerts.md) |
| Zobrazit připojené iniciátory</br>Najít sériové číslo zařízení hello</br>Nalezen element hello target IQN |Zařízení StorSimple Manager služby → → řídicí panel |[Řídicí panel zařízení StorSimple hello](storsimple-device-dashboard.md) |
| Vytvářet monitorování grafy |Zařízení StorSimple Manager služby → monitorování → |[Monitorování zařízení StorSimple](storsimple-monitor-device.md) |
| Přidat kontejner svazků</br>Upravit kontejneru svazků</br>Odstranit kontejner svazků |Kontejnery svazků → zařízení StorSimple Manager service → |[Správa kontejnerů svazků](storsimple-manage-volume-containers.md) |
| Přidání svazku</br>Upravit svazku</br>Svazek převést do režimu offline</br>Odstranění svazku</br>Monitorování svazku |Svazky zařízení StorSimple Manager service → zařízení → kontejnery svazků → |[Správa svazků](storsimple-manage-volumes.md) |
| Upravit nastavení zařízení</br>Upravit nastavení času</br>Upravit nastavení DNS.md</br>Nakonfigurujte rozhraní sítě |Zařízení StorSimple Manager služby → nakonfigurovat → |[Upravte konfiguraci zařízení pro zařízení StorSimple](storsimple-modify-device-config.md) |
| Nastavení proxy serveru webové zobrazení |Zařízení StorSimple Manager služby → nakonfigurovat → |[Konfigurace webového proxy serveru pro zařízení](storsimple-configure-web-proxy.md) |
| Změnit heslo správce zařízení</br>Změnit heslo Snapshot Manager zařízení StorSimple |Zařízení StorSimple Manager služby → nakonfigurovat → |[Změna hesel zařízení StorSimple](storsimple-change-passwords.md) |
| Konfigurace vzdálené správy |Zařízení StorSimple Manager služby → nakonfigurovat → |[Vzdálené připojení zařízení StorSimple tooyour](storsimple-remote-connect.md) |
| Konfigurace nastavení výstrah |Zařízení StorSimple Manager služby → nakonfigurovat → |[Zobrazovat a spravovat výstrahy StorSimple](storsimple-manage-alerts.md) |
| Konfigurace protokolů CHAP pro vaše zařízení StorSimple |Zařízení StorSimple Manager služby → nakonfigurovat → |[Konfigurace protokolů CHAP pro vaše zařízení StorSimple](storsimple-configure-chap.md) |
| Přidání zásady zálohování</br>Přidat nebo změnit plán</br>Odstranit zásady zálohování</br>Proveďte ruční zálohy</br>Vytvořit vlastní zásady zálohování s více svazků a plány |Zásady zálohování StorSimple Manager service → zařízení → |[Správa zásad zálohování](storsimple-manage-backup-policies.md) |
| Zastavit řadiče zařízení</br>Restartujte řadiče zařízení</br>Vypnutí řadiče zařízení</br>Obnovit výchozí hodnoty. toofactory vaše zařízení</br>(Vyšší jsou pouze místní zařízení) |Zařízení StorSimple Manager služby → → údržby |[Správa řadiče zařízení StorSimple](storsimple-manage-device-controller.md) |
| Další informace o hardwarových součástí StorSimple</br>Monitorování stavu hardwaru</br>(Vyšší jsou pouze místní zařízení) |Zařízení StorSimple Manager služby → → údržby |[Monitorování hardwarových součástí](storsimple-monitor-hardware-status.md) |
| Vytvoření balíčku pro podporu |Zařízení StorSimple Manager služby → → údržby |[Vytvoření a Správa balíčku pro podporu](storsimple-create-manage-support-package.md) |
| Instalovat aktualizace softwaru |Zařízení StorSimple Manager služby → → údržby |[Aktualizace zařízení](storsimple-update-device.md) |

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s hello každodenní operace zařízení StorSimple nebo s žádným z jeho hardwarové součásti, podívejte se na:

* [Řešení potíží s provozní zařízení](storsimple-troubleshoot-operational-device.md)
* [Použít StorSimple monitorování kláves](storsimple-monitoring-indicators.md)

Pokud nelze vyřešit problémy hello a potřebujete toocreate žádosti o službu, naleznete příliš[obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

