---
title: "Správa aaaDevice službou Azure IoT Hub | Microsoft Docs"
description: "Přehled správy zařízení ve službě Azure IoT Hub: životní cyklus firemních zařízení a vzorce správy zařízení, jako je restartování, obnovení do výrobního nastavení, aktualizace firmwaru, konfigurace, dvojčata zařízení, dotazy a úlohy."
services: iot-hub
documentationcenter: 
author: bzurcher
manager: timlt
editor: 
ms.assetid: a367e715-55f6-4593-bd68-7863cbf0eb81
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: briz
ms.openlocfilehash: 7e22fb6eb3c541a513b16a047c7c3ef557255532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-device-management-with-iot-hub"></a>Přehled správy zařízení ve službě IoT Hub
## <a name="introduction"></a>Úvod
Azure IoT Hub nabízí funkce hello a model rozšiřitelnosti, které umožňují zařízení a řešením pro správu serveru back-end vývojáři toobuild robustní zařízení. Zařízení v rozsahu od omezené senzory a microcontrollers jednoúčelové, toopowerful brány, které směrovat komunikaci pro skupiny zařízení.  Kromě toho hello případy použití a požadavcích pro IoT operátory lišit výrazně napříč odvětví.  Bez ohledu na případný rozdíl Správa zařízení pomocí služby IoT Hub poskytuje možnosti hello, vzory a kód knihovny toocater tooa různého typu zařízení a koncoví uživatelé.

Je zásadní součástí vytváření řešení IoT úspěšné celopodnikové tooprovide strategie pro jak pracovat s operátory hello průběžnou správu jejich kolekce zařízení. Operátory IoT vyžadují jednoduché a spolehlivé nástrojů a aplikací, které je toofocus na hello povolit další strategických aspektů svou práci. Tento článek obsahuje:

* Stručný přehled Azure IoT Hub přístup toodevice správy.
* Popis obecných principů správy zařízení
* Popis životní cyklus zařízení hello.
* Přehled běžných schémat správy zařízení

## <a name="device-management-principles"></a>Principy správy zařízení
Přináší IoT s ním jedinečnou sadu problémy při správě zařízení a každé podnikové řešení, musí řešit hello následující zásady:

![Grafické znázornění principů správy zařízení][img-dm_principles]

* **Škálování a automatizace**: řešení IoT vyžadují jednoduché nástroje, které můžete automatizovat rutinní úlohy a povolení poměrně malý operací personál toomanage miliony zařízení. Každodenní, operátory očekávat operations toohandle zařízení vzdáleně, hromadně, a tooonly upozorněni případných problémech, které vyžadují jejich pozornost direct.
* **Průhlednost a kompatibility**: ekosystém zařízení hello je mimořádně různých. Nástroje pro správu musí být šité na míru tooaccommodate velkého množství tříd zařízení, platformy a protokoly. Operátory musí být schopný toosupport mnoho typů zařízení, z hello nejvíce omezené vložených jedním proces čipy, toopowerful a plně funkční počítače.
* **Znalost kontextu:** Prostředí služby IoT prostředí jsou dynamická a neustále se mění. Spolehlivost služby je prvořadá. Operace správy zařízení je třeba vzít v účtu hello následující faktory tooensure této údržby výpadek nebude ovlivnily provoz kritické obchodní nebo vytvořte nebezpečná podmínky:
    * Časová období údržby SLA
    * Stavy sítě a napájení
    * Podmínky využívání
    * Zeměpisná poloha zařízení
* **Služby role mnoho**: podpora pro hello jedinečný pracovních postupů a procesů IoT rolí je zásadní. Hello provozní personál musí obchodu pracovat hello zadané omezení interní IT oddělení.  Najdou musí také udržitelná způsoby toosurface v reálném čase zařízení operations informace toosupervisors a jiné firmy správcovské role.

## <a name="device-lifecycle"></a>Životní cyklus zařízení
Není sadu fáze správy Obecné zařízení, které jsou společné projekty IoT tooall enterprise. V Azure IoT jsou pět fází v rámci hello životní cyklus zařízení:

![Hello pět fází životního cyklu zařízení Azure IoT: plánování, konfiguraci, monitorování, zřízení vyřadit z provozu][img-device_lifecycle]

V každém z těchto pět fází existuje několik požadavků operátor zařízení, které by měly být splněny tooprovide kompletního řešení:

* **Plánování**: Povolit operátory toocreate schéma metadat zařízení, která umožňuje jejich tooeasily a přesně dotaz pro a cílovou skupinu a zařízení pro hromadné operace správy. Hello zařízení twin toostore můžete tato zařízení metadata hello tvar značky a vlastnosti.
  
    *Další čtení*: [začít pracovat s dvojčata zařízení][lnk-twins-getstarted], [pochopit dvojčata zařízení][lnk-twins-devguide], [jak Vlastnosti twin zařízení toouse][lnk-twin-properties].
* **Zřízení**: bezpečně zřídit nové zařízení tooIoT rozbočovače a povolit operátory tooimmediately zjistit možnosti zařízení.  Použijte hello IoT Hub identity registru toocreate flexibilní zařízení identity a přihlašovací údaje a provést tuto operaci hromadné provádíte pomocí úlohy. Sestavení zařízení tooreport jejich funkce a podmínky prostřednictvím vlastnosti zařízení v dvojče zařízení hello.
  
    *Další čtení*: [Správa identit zařízení][lnk-identity-registry], [hromadné správu identit zařízení][lnk-bulk-identity], [Jak dvojče zařízení toouse vlastnosti][lnk-twin-properties].
* **Konfigurace**: usnadnění hromadné změny konfigurace a firmware aktualizací toodevices při zachování stavu a zabezpečení. Tyto operace správy zařízení provádějte hromadně pomocí požadovaných vlastností nebo pomocí přímých metod a vysílacích úloh.
  
    *Další čtení*: [použít přímé metody][lnk-c2d-methods], [vyvolat přímé metodu na zařízení][lnk-methods-devguide], [jak Vlastnosti twin zařízení toouse][lnk-twin-properties], [plán a všesměrového vysílání úlohy][lnk-jobs], [plánování úloh na několika zařízeních] [lnk-jobs-devguide].
* **Monitorování**: monitorování stavu kolekce celkové zařízení, stav hello probíhající operace a tooissues výstrahy operátory, které mohou vyžadovat jejich pozornost.  Použijte hello zařízení twin tooallow zařízení tooreport v reálném čase provozních podmínek a stav operace aktualizace. Vytváření sestav výkonné řídicí panel tento prostor hello nejvíce okamžitou problémy pomocí dotazů twin zařízení.
  
    *Další čtení*: [jak dvojče zařízení toouse vlastnosti][lnk-twin-properties], [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query-language].
* **Vyřazení**: nahraďte nebo vyřadit z provozu zařízení po selhání, upgrade cyklus, nebo na konci hello životnosti hello služby.  Použijte hello zařízení twin toomaintain informace o zařízení, pokud hello fyzické zařízení, která má být nahrazen nebo archivovaných Pokud postupně vyřazuje z provozu. Použijte hello registru identit služby IoT Hub pro bezpečně odvolání identit zařízení a přihlašovací údaje.
  
    *Další čtení*: [jak dvojče zařízení toouse vlastnosti][lnk-twin-properties], [Správa identit zařízení][lnk-identity-registry].

## <a name="device-management-patterns"></a>Schémata správy zařízení
IoT Hub umožňuje hello následující sada schémat správy zařízení.  Hello [kurzy správy zařízení] [ lnk-get-started] jak ukazují podrobněji tooextend tyto vzory toofit přesný scénář a jak nové vzory toodesign založené na tyto základní šablony.

* **Restartovat** -hello back-end aplikačním informuje hello zařízení prostřednictvím přímé metody, aby se zahájilo se restartování.  Hello zařízení používá hello hlášené vlastnosti tooupdate hello restartování stav hello zařízení.
  
    ![Grafické znázornění schématu restartování ve správě zařízení][img-reboot_pattern]
* **Obnovení továrního nastavení** -hello back-end aplikačním informuje hello zařízení prostřednictvím přímé metody, inicioval obnovení továrního nastavení.  Hello zařízení používá hello ohlásil, že stav zařízení hello obnovit vlastnosti tooupdate hello tovární nastavení.
  
    ![Grafické znázornění schématu obnovení výrobního nastavení][img-facreset_pattern]
* **Konfigurace** -back-end aplikace hello používá hello potřeby vlastnosti tooconfigure softwaru spuštěné na zařízení hello.  Hello zařízení používá hello hlášené vlastnosti tooupdate stav konfigurace hello zařízení.
  
    ![Grafické znázornění schématu konfigurace ve správě zařízení][img-config_pattern]
* **Aktualizace firmwaru** -hello back-end aplikačním informuje hello zařízení prostřednictvím přímé metody, inicioval aktualizaci firmwaru.  zařízení Hello zahájí proces zahrnující více kroků toodownload hello firmware bitové kopie, použít bitovou kopii hello firmware a nakonec znovu připojit toohello služby IoT Hub.  V rámci hello vícekrokový proces hello zařízení používá hello hlášené vlastnosti tooupdate hello průběh a stav hello zařízení.
  
    ![Grafické znázornění schématu aktualizace firmwaru ve správě zařízení][img-fwupdate_pattern]
* **Vytváření sestav průběh a stav** -back-end hello řešení spouští zařízení twin dotazy, napříč sady zařízení, tooreport na dobrý stav a průběh akce běžící v zařízeních hello.
  
    ![Grafické znázornění schématu informování o průběhu a stavu ve správě zařízení][img-report_progress_pattern]

## <a name="next-steps"></a>Další kroky
Možnosti Hello, vzory a knihovny kódu, které poskytuje služby IoT Hub pro správu zařízení, povolit toocreate IoT aplikace, které odpovídají enterprise IoT operátor požadavky v rámci každé fáze životního cyklu zařízení.

toocontinue získávání informací o hello funkce správy zařízení IoT hub, najdete v části hello [Začínáme se správou zařízení] [ lnk-get-started] kurzu.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-node-node-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md
