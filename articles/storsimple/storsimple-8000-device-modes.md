---
title: "režim zařízení StorSimple aaaChange | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple hello a vysvětluje, jak toouse Windows Powershellu pro StorSimple toochange hello režim zařízení."
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
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 058ca6cc38954bce3679cc21b39d341b10cb4dfb
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


### <a name="recovery-mode"></a>Obnovení režimu

Obnovení režimu lze popsat jako "Bezpečný režim pro Windows s podporou sítě". Režimu obnovení, zapojí týmu Microsoft Support hello a umožňuje jim tooperform diagnostiky systému hello. primární cílem Hello režimu obnovení je tooretrieve hello systémové protokoly.

Pokud váš systém přejde do režimu obnovení, měli byste požádat Microsoft Support pro další kroky. Další informace, přejděte příliš[obraťte se na podporu společnosti Microsoft](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **Nelze umístit hello zařízení v režimu obnovení. Pokud zařízení hello je ve špatném stavu, režimu obnovení pokusí tooget hello zařízení ve stavu, ve kterém můžete zkontrolovat Microsoft Support pracovníky ho.**

## <a name="determine-storsimple-device-mode"></a>Určení režimu zařízení StorSimple

#### <a name="toodetermine-hello-current-device-mode"></a>aktuální režim zařízení toodetermine hello

1. Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Podívejte se na uvítací zprávu banner v nabídce konzoly sériového portu hello hello zařízení. Tato zpráva explicitně určuje, zda text hello zařízení je v režimu údržby nebo obnovení. Pokud zpráva hello neobsahuje žádné konkrétní informace týkající se režimu toohello systému, hello zařízení je v normálním režimu.

## <a name="change-hello-storsimple-device-mode"></a>Změnit režim zařízení StorSimple hello

Můžete umístit zařízení StorSimple hello do údržby režimu (od normální režim) tooperform údržby nebo nainstalovat aktualizace režimu údržby. Proveďte následující postupy tooenter nebo ukončení režimu údržby hello.

> [!IMPORTANT]
> Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku díky přístupu k hello **nastavení zařízení > stavu hardwaru** pro zařízení v hello portálu Azure. Pokud jeden nebo oba řadiče hello nejsou v pořádku, požádejte o další kroky hello Microsoft Support. Další informace, přejděte příliš[obraťte se na podporu společnosti Microsoft](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="tooenter-maintenance-mode"></a>tooenter režimu údržby

1. Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**. Po zobrazení výzvy zadejte hello **hesla správce zařízení**. výchozí heslo Hello je: `Password1`.
3. Hello příkazového řádku zadejte 
   
    `Enter-HcsMaintenanceMode`
4. Zobrazí se upozornění oznamující, že režimu údržby se přerušit všechny vstupně-výstupní požadavky a severu hello připojení toohello portál Azure a zobrazí se výzva k potvrzení. Typ **Y** tooenter režimu údržby.
5. Oba řadiče se restartuje. Po dokončení restartování hello hello konzoly sériového portu banner označí, že toto zařízení hello je v režimu údržby. Ukázkový výstup najdete níž.

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="tooexit-maintenance-mode"></a>tooexit režimu údržby

1. Přihlaste se toohello konzoly sériového portu zařízení. Ověřte ze hello zpráva hlavičky, která vaše zařízení je v režimu údržby.
2. Hello příkazového řádku zadejte:
   
    `Exit-HcsMaintenanceMode`
3. Zobrazí se zpráva s upozorněním a potvrzovací zpráva. Typ **Y** tooexit režimu údržby.
4. Oba řadiče se restartuje. Po dokončení restartování hello hello konzoly sériového portu banner značí, že toto zařízení hello je v normálním režimu. Ukázkový výstup najdete níž.

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure tooinstall on each controller could result in data corruption. Exiting maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooexit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[režim aktualizace normální a údržby](storsimple-update-device.md) zařízení StorSimple.

