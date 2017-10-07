---
title: "režim zařízení StorSimple hello aaaChange | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple hello a vysvětluje, jak toouse Windows Powershellu pro StorSimple toochange hello režim zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 299fd380a83bcd06780c97937f4064f0791b440d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-device-mode-on-your-storsimple-device"></a>Změnit režim hello zařízení na zařízení StorSimple
Tento článek obsahuje stručný popis hello různé režimy, ve kterých může fungovat i zařízení StorSimple. Zařízení StorSimple, můžou fungovat v tři režimy: Normální, údržbu a obnovení. 

Po přečtení tohoto článku, budete vědět:

* Jaké jsou režimy zařízení StorSimple hello
* Jak toofigure, na kterém režimu hello zařízení StorSimple je v
* Jak toochange z režimu Normální toomaintenance a *naopak*

Hello výše úlohy správy lze provést pouze prostřednictvím rozhraní Windows PowerShell hello zařízení StorSimple.

## <a name="about-storsimple-device-modes"></a>O režimech zařízení StorSimple
Zařízení StorSimple můžou fungovat v režimu Normální, údržby nebo obnovení. Každá z těchto režimů stručně je popsána níže.

### <a name="normal-mode"></a>Normálním režimu
To je definován jako hello normální provozní režim pro zařízení StorSimple plně nakonfigurované. Ve výchozím nastavení musí být zařízení v normálním režimu.

### <a name="maintenance-mode"></a>Režim údržby
Někdy hello zařízení StorSimple může být nutné toobe umístěn do režimu údržby. Tento režim na zařízení hello vám umožní tooperform údržby a instalovat rušivý aktualizace, například těch, které související toodisk firmwaru.

Hello systému můžete uvést do režimu údržby pouze prostřednictvím hello Windows Powershellu pro StorSimple. V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky. Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo hello Clusterová služba. Oba řadiče hello se restartují, když zadáte nebo ukončit tento režim. Při ukončení režimu údržby hello se všechny služby hello bude pokračovat a musí být v pořádku. Může to trvat několik minut.

> [!NOTE]
> **Režim údržby je podporována pouze na zařízení správně funguje. Není podporována u zařízení, ve které jedna nebo obě hello řadičů nefungují.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Obnovení režimu
Obnovení režimu lze popsat jako "Bezpečný režim pro Windows s podporou sítě". Režimu obnovení, zapojí týmu Microsoft Support hello a umožňuje jim tooperform diagnostiky systému hello. primární cílem Hello režimu obnovení je tooretrieve hello systémové protokoly.

Pokud váš systém přejde do režimu obnovení, měli byste požádat Microsoft Support pro další kroky. Další informace, přejděte příliš[obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **Nelze umístit hello zařízení v režimu obnovení. Pokud zařízení hello je ve špatném stavu, režimu obnovení pokusí tooget hello zařízení ve stavu, ve kterém můžete zkontrolovat Microsoft Support pracovníky ho.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Určení režimu zařízení StorSimple
#### <a name="toodetermine-hello-current-device-mode"></a>aktuální režim zařízení toodetermine hello
1. Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Podívejte se na uvítací zprávu banner v nabídce konzoly sériového portu hello hello zařízení. Tato zpráva explicitně určuje, zda text hello zařízení je v režimu údržby nebo obnovení. Pokud zpráva hello neobsahuje žádné konkrétní informace týkající se režimu toohello systému, hello zařízení je v normálním režimu.

## <a name="change-hello-storsimple-device-mode"></a>Změnit režim zařízení StorSimple hello
Můžete umístit zařízení StorSimple hello do údržby režimu (od normální režim) tooperform údržby nebo nainstalovat aktualizace režimu údržby. Proveďte následující postupy tooenter nebo ukončení režimu údržby hello.

> [!IMPORTANT]
> Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku díky přístupu k hello **stavu hardwaru** na hello **údržby** stránku hello portál Azure classic. Pokud jeden nebo oba řadiče hello nejsou v pořádku, požádejte o další kroky hello Microsoft Support. Další informace, přejděte příliš[obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="tooenter-maintenance-mode"></a>tooenter režimu údržby
1. Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**. Po zobrazení výzvy zadejte hello **hesla správce zařízení**. výchozí heslo Hello je: `Password1`.
3. Hello příkazového řádku zadejte 
   
    `Enter-HcsMaintenanceMode`
4. Zobrazí se upozornění oznamující, že režimu údržby se přerušit všechny vstupně-výstupní požadavky a severu hello připojení toohello portál Azure classic a zobrazí se výzva k potvrzení. Typ **Y** tooenter režimu údržby.
5. Oba řadiče se restartuje. Po dokončení restartování hello se zobrazí další zprávu oznamující, že toto zařízení hello je v režimu údržby.

#### <a name="tooexit-maintenance-mode"></a>tooexit režimu údržby
1. Přihlaste se toohello konzoly sériového portu zařízení. Ověřte ze hello zpráva hlavičky, která vaše zařízení je v režimu údržby.
2. Hello příkazového řádku zadejte:
   
    `Exit-HcsMaintenanceMode`
3. Zobrazí se zpráva s upozorněním a potvrzovací zpráva. Typ **Y** tooexit režimu údržby.
4. Oba řadiče se restartuje. Po dokončení restartování hello se zobrazí další zprávu oznamující, že toto zařízení hello je v normálním režimu.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[režim aktualizace normální a údržby](storsimple-update-device.md) zařízení StorSimple.

