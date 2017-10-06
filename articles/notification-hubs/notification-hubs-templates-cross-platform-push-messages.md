---
title: aaaTemplates
description: "Toto téma vysvětluje šablon pro Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 0149f0c7473e5a4b952905bc8217582b58db2a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="templates"></a>Šablony
## <a name="overview"></a>Přehled
Šablony povolit klienta aplikace toospecify hello přesný formátu hello oznámení, že ho chce tooreceive. Pomocí šablon, aplikace můžou realizovat několik různých výhod, jako třeba hello následující:

* Back-end bez ohledu na platformy
* Přizpůsobené oznámení
* Nezávislost verze klienta
* Snadno lokalizace

Tato část obsahuje dva podrobné příklady, jak toouse šablony toosend bez ohledu na platformu oznámení cílení na všechna zařízení napříč platformami a toopersonalize vysílání oznámení tooeach zařízení.

## <a name="using-templates-cross-platform"></a>Pomocí šablony a platformy
Hello standardním způsobem toosend nabízených oznámení je toosend pro každý oznámení, že toobe pošle služby oznámení konkrétní datové části tooplatform, (WNS, APNS). Například toosend výstrahy tooAPNS, datová část hello je objekt Json hello následující formulář:

    {"aps": {"alert" : "Hello!" }}

toosend podobná informační zpráva v aplikaci pro Windows Store, hello datovou část XML je následující:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

Podobně jako datové části můžete vytvořit pro platformy (Android) GCM a MPNS (Windows Phone).

Tento požadavek vynutí hello aplikace back-end tooproduce různých datových částí pro každou platformu a efektivně je zodpovědná za součástí prezentační vrstvy hello hello aplikace hello back-end. Některé aspekty patří lokalizace a grafické rozložení (hlavně u aplikací pro Windows Store, které obsahují oznámení pro různé typy dlaždic).

funkce šablony Hello Notification Hubs umožňuje aplikace toocreate speciální registrace klienta, nazývá šablona registrace, které obsahují, kromě toohello sadu značky, šablony. Hello Notification Hubs šablony funkce umožňuje tooassociate aplikace klientské zařízení se šablonami, zda pracujete s instalací (doporučeno) nebo registrací. Zadána hello předcházející datové části Příklady hello jenom informace o nezávislé na platformě je hello skutečné výstrahy zpráva (Hello!). Šablona je sada pokynů pro hello centra oznámení na tom, jak tooformat nezávislé na platformě zprávy pro registraci hello tohoto konkrétního klienta aplikace. V předchozím příkladu hello, nezávislé zpráva hello platformy je jedinou vlastností: **zpráva = Hello!**.

Hello následující obrázek znázorňuje hello výše proces:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

Hello šablonu pro registraci aplikace klienta iOS hello vypadá takto:

    {"aps": {"alert": "$(message)"}}

Hello odpovídající šablonu pro klientskou aplikaci pro Windows Store hello je:

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

Všimněte si, že hello skutečné zpráva nahradí hello výraz $(zprávy). Tento výraz dá pokyn hello centra oznámení pokaždé, když odešle zprávu toothis konkrétní registrace, toobuild zprávu, která odpovídá jeho a přepínačů v hodnotě běžné hello.

Pokud pracujete s modelem instalace, obsahuje klíč "šablony" hello instalace JSON více šablon. Pokud pracujete s modelem registrace, klientská aplikace hello můžete vytvořit více registrace v pořadí toouse několik šablon; například šablonu pro oznámení a šablonu pro dlaždici aktualizací. Klientské aplikace můžete také kombinovat nativní registrace (registrace s žádnou šablonu) a šablony registrace.

Hello centra oznámení odešle jedno oznámení ke každé šabloně bez ohledu zda patří toohello stejné klientské aplikace. Toto chování může být použité tootranslate nezávislé na platformě oznámení do další oznámení. Například hello stejné toohello nezávislé zpráva platformy, které Centrum oznámení můžete bezproblémově přeložit v informační výstrahy a aktualizace dlaždice, bez nutnosti hello back-end toobe kroky. Všimněte si, že některé platformy (například iOS) může sbalit více toohello oznámení stejného zařízení, pokud jsou odesílány v krátké době.

## <a name="using-templates-for-personalization"></a>Pomocí šablon pro přizpůsobení
Další výhody toousing šablony je hello možnost toouse Notification Hubs tooperform za registraci přizpůsobení oznámení. Představte si třeba počasí aplikaci, která zobrazuje dlaždici hello počasí na určité místo. Uživatel může zvolit c nebo Fahrenheita stupňů a jednoho nebo pětidenního prognózy. Pomocí šablon, každou instalaci klienta aplikace si můžou zaregistrovat hello formátu potřebném (1 den Celsius, 1 den Fahrenheita, 5 dní c, 5 dní Fahrenheita kurzy), a odeslat zprávu, která obsahuje všechny informace o hello požadované toofill ty back-end hello šablony (například pět dní prognózy s c a Fahrenheita stupních).

Hello šablony pro hello jeden den prognózy s c teploty vypadá takto:

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

zpráva byla odeslána toohello Hello centra oznámení obsahuje všechny hello následující vlastnosti:

<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>

Pomocí tohoto vzoru back-end hello pouze odešle do jedné zprávy bez nutnosti toostore přizpůsobení konkrétní možnosti pro uživatele aplikace hello. Hello následující obrázek znázorňuje tento scénář:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-tooregister-templates"></a>Jak tooregister šablony
tooregister s šablony pomocí modelu instalace hello (doporučeno) nebo hello modelu registrace najdete v části [registrace správu](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Výraz jazyka šablony
Šablony jsou omezené tooXML nebo formáty dokumentů JSON. Navíc můžete pouze umístit výrazy konkrétní místech; například atributy uzlu nebo hodnoty pro formát XML řetězec hodnoty vlastností pro formát JSON.

Hello následující tabulka uvádí hello jazyk povoleny v rámci šablon:

| výraz | Popis |
| --- | --- |
| $(prop) |Vlastnost referenčního tooan události se zadaným názvem hello. Názvy vlastností nerozlišují malá a velká písmena. Tento výraz přeloží do hello vlastnost textové hodnoty nebo na prázdný řetězec, pokud vlastnost hello není k dispozici. |
| $(prop, n) |Jak je vyšší, ale hello textu je explicitně oříznut n znaků, například $(název, 20) klipy hello obsah hello název vlastnosti v 20 znaků. |
| . (prop, n) |Jako vyšší, ale hello je text konci se třemi tečkami, jako je oříznut. Celková velikost Hello hello oříznut řetězec a příponu hello nepřesahuje n znaků. . (title, 20) s vstupní vlastností "Toto je název řádku hello" výsledků v **Toto je název hello...** |
| %(Prop) |Podobně jako too$(name) s tím rozdílem, že výstup hello je kódovaný identifikátor URI. |
| #(prop) |Používá se v šablony JSON (například pro iOS a Android šablony).<br><br>Tato funkce funguje přesně hello stejné jako $(prop) dříve zadán, s výjimkou při používány šablony JSON (například Apple šablony). V tomto případě, pokud není tato funkce obklopená "{','}" (například myJsonProperty: '#(název)'), a vyhodnocuje tooa číslo ve formátu Javascript, například regexp: (0 &#124; (&#91; 1-9 &#93; &#91; 0-9 & #93 ;*))(\. &#91; 0-9 &#93; +)? ((e &#124; E) (+ &#124;-)? &#91; 0-9 &#93; +)?, pak hello výstup JSON je číslo.<br><br>Například "oznámení" BADGE ":"#(název), se změní na "oznámení": 40 (a ne 40). |
| 'text' nebo "text" |Literál. Literály obsahovat libovolný text v jednoduchých nebo dvojitých uvozovek. |
| Výraz1 + Výraz2 |operátor řetězení Hello propojení dvou výrazů do jednoho řetězce. |

výrazy Hello může být libovolná z předchozích forms hello.

Pokud používáte zřetězení, musí být uzavřena hello celý výraz s {}. Například {$(prop) + '-' + $(prop2)}. |

Například následující hello není platné šablony XML:

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


Jak je vysvětleno výše při použití zřetězení, výrazy musí být uzavřen do složených závorek. Například:

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

