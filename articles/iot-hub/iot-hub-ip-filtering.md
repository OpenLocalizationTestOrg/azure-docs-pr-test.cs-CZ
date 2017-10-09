---
title: "filtry připojení aaaAzure IoT Hub IP | Microsoft Docs"
description: "Jak toouse IP filtrování tooblock připojení z konkrétní IP adresy pro tooyour Azure IoT hub. Můžete blokovat připojení z jednotlivých nebo rozsahy IP adres."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 45e5906a494561b6108895d86d6a04fc3b52b8fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ip-filters"></a>Použití filtrů IP

Zabezpečení je důležitým aspektem jakékoli řešení IoT podle Azure IoT Hub. Někdy můžete potřebovat tooexplicitly zadejte hello IP adresy, ze kterých se zařízení můžou připojit jako součást konfigurace zabezpečení. Hello _filtr IP_ funkce vám umožní tooconfigure pravidla odmítat nebo přijímat přenosy z konkrétní adresy IPv4.

## <a name="when-toouse"></a>Když toouse

Existují dvě určité případy použití po koncové body centra IoT hello užitečné tooblock pro určité IP adresy:

- Služby IoT hub by měl přijímat přenosy pouze ze zadaného rozsahu IP adres a všem ostatním odmítnout. Například používáte služby IoT hub s [Azure Express trasy] toocreate privátní připojení mezi služby IoT hub a v místní infrastruktuře.
- Je nutné tooreject provoz z IP adresy, které byly identifikovány jako podezřelou správcem hello IoT hub.

## <a name="how-filter-rules-are-applied"></a>Používání pravidel filtru

pravidla filtru IP Hello se používají v hello úrovně služby IoT Hub. Proto platí pravidla filtru IP hello tooall připojení ze zařízení a back-end aplikace používající libovolný protokol pro podporované.

Všechny pokus o připojení z IP adresy, který odpovídá pravidlu předávaní IP ve službě IoT hub přijímá neoprávněným stavový kód 401 a popis. zpráva odpovědi Hello není zmiňuje hello IP pravidlo.

## <a name="default-setting"></a>Výchozí nastavení

Ve výchozím nastavení, hello **filtr IP** mřížky hello portálu služby IoT hub je prázdný. Toto výchozí nastavení znamená, že vaše Centrum přijímá připojení jakékoli IP adresy. Toto výchozí nastavení je ekvivalentní tooa pravidlo, které přijímá rozsah IP adres hello 0.0.0.0/0.

![Nastavení filtru IP výchozí služby IoT Hub][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>Přidat nebo upravit pravidlo filtru IP

Když přidáte pravidlo filtru IP, budete vyzváni k hello následující hodnoty:

- **Název pravidla filtru IP** , musí být jedinečný, velká a malá písmena, alfanumerické řetězec až too128 znaků. Pouze plus alfanumerických znaků ASCII 7bitového hello `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` jsou podmínky přijaty.
- Vyberte **odmítnout** nebo **přijmout** jako hello **akce** pravidla filtru IP hello.
- Zadejte adresu samostatného protokolu IPv4, nebo blok IP adres v notaci CIDR. V CIDR zápis 192.168.100.0/22 představuje hello 1024 IPv4 adresy z 192.168.100.0 too192.168.103.255.

![Přidat Centrum IoT tooan pravidlo filtru IP][img-ip-filter-add-rule]

Po uložení hello pravidlo se zobrazí výstraha upozorněním, že je daná aktualizace hello v průběhu.

![Oznámení o ukládání pravidlo filtru IP][img-ip-filter-save-new-rule]

Hello **přidat** možnost je vypnuta, když se dostanete hello maximálně 10 pravidla filtru IP.

Dvojitým kliknutím na soubor hello řádek, který obsahuje pravidlo hello můžete upravit existující pravidlo.

> [!NOTE]
> Odmítat IP adresy můžete zabránit jiné služby Azure (například Azure Stream Analytics, virtuální počítače Azure nebo hello zařízení Explorer hello portálu) v interakci s centrem IoT hello.

> [!WARNING]
> Pokud používáte Azure Stream Analytics (ASA) tooread zpráv ze služby IoT hub s povoleným filtrováním IP, použijte název hello kompatibilní s centrem událostí a koncový bod služby IoT Hub v hello ASA připojovací řetězec.

## <a name="delete-an-ip-filter-rule"></a>Odstranění pravidla filtru IP

toodelete pravidlo filtru IP vyberte jeden nebo více pravidel v mřížce hello a klikněte na **odstranit**.

![Odstranění pravidla filtru IP centra IoT][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>Vyhodnocení pravidla filtru IP

Pravidla filtru IP jsou použity v zadaném pořadí a že odpovídá hello IP adresa je rozhodující pro hello první pravidlo hello přijmout nebo odmítnout akce.

Například pokud chcete tooaccept adresy v rozsahu 192.168.100.0/22 hello a všem ostatním odmítnout, hello prvního pravidla v mřížce hello musí přijmout 192.168.100.0/22 rozsah adres hello. Další pravidla Hello by měl zamítnout všechny adresy pomocí 0.0.0.0/0 rozsah hello.

Můžete změnit pořadí hello pravidel filtru IP v mřížce hello klepnutím hello tři tečky vertikálně při spuštění hello řádku a pomocí přetažení a vyřadit.

filtrovat nových IP toosave pravidlo pořadí, klikněte na tlačítko **Uložit**.

![Změňte pořadí hello pravidel filtru IP centra IoT][img-ip-filter-rule-order]

## <a name="next-steps"></a>Další kroky

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

- [Monitorování operací][lnk-monitor]
- [Metriky služby IoT Hub][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express trasy]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md