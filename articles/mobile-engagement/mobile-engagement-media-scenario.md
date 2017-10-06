---
title: "implementace aaaAzure Mobile Engagementu pro aplikaci média"
description: "Média aplikace scénář tooimplement Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48201cc8-4e04-485c-a8dc-d6406d23f3ed
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6a495eb790993a30d5c03802aa9e6404fea983d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-media-app"></a>Implementace s mediální aplikace Mobile Engagement
## <a name="overview"></a>Přehled
Jan je správcem mobilní projektu pro společnost big média. Zadá nedávno spuštění nové aplikace, který má stažení velmi vysoký počet. Zadá narazil na jeho cíle pro stahování, ale stále jeho vrátit na Investment(ROI) na uživatele nesplňuje požadavky na jeho. 

Jan již nalezla proto jeho návratnost investic je příliš nízká. Uživatelé často zastavit pomocí jeho aplikace jenom 2 týdny a většina z nich nikdy vraťte. Chce tooincrease hello uchování jeho aplikace.

Po některé počáteční testování mu se naučili, pokud mu zapojí svých uživatelů s nabízená oznámení, že mívají toocontinue pomocí jeho aplikace. Uživatelé, které byly neaktivní také často vrátí toohello aplikace v závislosti na oznámení, která mu odešle je. Jan rozhodne tooinvest v nějaký druh Program zapojení pro svou aplikaci, která využívá rozšířené cílení pomocí nabízených oznámení.

Jan má nedávno čtení hello [Azure Mobile Engagement – Příručka Začínáme s osvědčenými postupy](mobile-engagement-getting-started-best-practices.md) a se rozhodla tooimplement hello doporučení z příručky hello.

## <a name="objectives-and-kpis"></a>Cíle a klíčové ukazatele výkonu.
Klíčových zúčastněných stran Jan pro aplikace splňovat. Obchodní je generováno ze služby Active Directory uživatelům využívat jeho média. Zvýšením obsahu využívat na uživatele, se zvyšuje Jan jeho příjmů. Všechny odsouhlasení jedním z hlavních cílů: prodej tooincrease ze služby Active Directory o 25 %. Uživatel vytvořit toomeasure obchodní klíčové ukazatele výkonu (KPI) a disku tohoto cíle

* Počet kliknutí na jednotlivé uživatele služby Active Directory
* Počet stránek článku navštívené (na uživatele / relace / za týden / měsíčně...)
* Jaké jsou oblíbených kategorií

Na základě Jan pro schůzky s klíčových zúčastněných stran mu nemá definovánu jeho klíčové ukazatele výkonu. Zadá následuje část 1 hello [Azure Mobile Engagement – Příručka Začínáme s osvědčenými postupy](mobile-engagement-getting-started-best-practices.md). 

V dalším kroku vytvoří hello následující klíčové ukazatele zapojení tooensure, jsou dosaženo cíle:

* Sledovat uchovávání dat v rámci hello následující intervaly: denně, týdně, jednou za dva týdny a měsíční.
* Počet aktivních uživatelů
* ukládá Hello hodnocení aplikace v aplikaci hello

Na základě doporučení z hello IT tým, hello následující technické klíčové ukazatele výkonu byly přidány hello tooanswer následující otázky:

* Co je cesta k uživatele (je navštívené stránky, která, kolik času uživatelé vynaložit na jeho)
* Počet havárií nebo chyby došlo k relace?
* Jaké verze operačního systému jsou mé uživatelé používají?
* Jaká je průměrná velikost hello obrazovky pro svoje uživatele?
* Jaký druh připojení k Internetu Moji uživatelé mají?

Pro každý ukazatel KPI mu klasifikuje data hello požadovaná a mohl záznamů v hello správné umístění jeho scénářem.

## <a name="engagement-program-and-integration"></a>Integrace produktů a program zapojení
Teď tento Jan dokončí, definování jeho klíčových ukazatelů výkonu, začne jeho fáze strategie zapojení definováním 4 programy zapojení a jejich cíle:![][1]

Jan přejde poté hlubší podle s podrobnostmi o nabízená oznámení pro každý program. Nabízená oznámení jsou definovány pět prvky:

1. Cíl: co je cíl hello hello oznámení
2. Jak bude dosaženo hello cíl
3. Cíl: kdo obdrží oznámení hello?
4. Obsah: Co je hello slov a hello formát hello oznámení (v App/Out z aplikace)
5. V případě: co je hello nejlepší chvíli toosend tato nabízená oznámení
   
    ![][2]

Další informace najdete v části toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Podle toohello část 2 / hello dokumentu white paper Jan použije cílový oddíl toodefine jaká data má toocollect a jeho značka plánování společně s IT tým tooimplement hello řešení zápisy. Po 1 týden implementace a testování přijetí uživateli Jan nakonec spusťte jeho programy.

## <a name="program-results"></a>Výsledky programu
4 měsíců později, Jan zkontroluje přínos programů. Hello uvítací Program a hello týdenní programu jsou splňuje jeho cíle. snižuje počet Hello uživatele s pouze jednu relaci, se používají další funkce aplikace hello a hello počet připojení za týden má dvojnásobný.

Hello **neaktivní Program** pomáhá pochopit tendence uživatele Jan. Zdá se, že 15 % neaktivní uživatele hello vraťte toohello aplikace. Ale většina z nich není zůstávají aktivní, více než 1 měsíc. Jan předpokládá potenciální optimalizace toto pořadí s další oznámení a rozšíření možnosti jeho obsahu.

Hello **zjistit Program** nefunguje správně. Jeho hodnota se zvyšuje křížové prodávané, ale není dostatek tooreach své cíle. Jan identifikuje, že nebude mít dostatek dat toomake relevantní cílení a navrhnout příslušný obsah. Mu tento program zastaví a se zaměřuje na odesílání "redakční nabízených oznámení" pomocí Azure Mobile Engagement. Jeho novináři už máte CMS řešení toosend nabízená oznámení a jejich nechcete, aby toochange.

Jan rozhodne toouse hello Reach API, což je rozhraní REST API HTTP, která umožňuje správu kampaně Reach bez nutnosti toouse AZME webové rozhraní. S tímto přístupem Jan může shromažďovat data hello mu potřebuje a povolit jeho zapisovače tookeep pomocí řešení CMS hello.

tooensure, který funkce funguje správně, Jan požádá IT tým toobe opatrná na hello následující body:

1. **Operační systémy** : mají všechna vlastní pravidla tooadministrate nabízená oznámení, tak Jan rozhodne toolist všechny případy a kontroly Pokud hello rozhraní API ji zpracovat.
   Např: Systému Android nabízené umožňuje velký obrázek, který není hello případ s iOS.
2. **Časový rámec**: John chce rozhraní API, které nastavený hello časový rámec a toocampaigns end. Chce toopreserve uživatele z jakékoli bombing rušivý oznámení.
3. **Kategorie**: Marketing team připraví šablony pro každý typ výstrahy. Jan požádá IT tým tooset kategorie uvnitř hello rozhraní API.

Některé testy Jan po splněna. Děkujeme toothis rozhraní API, novináři může i dál posílat nabízená oznámení s jejich CMS a Azure Mobile Engagement shromažďuje všechna data chování pro ně

Za těchto 4 nejprve měsíců, výsledky odrážel dobrý celkový výkon a poskytuje jistotu Jan a jeho Tabule, návratnost investic za uživatele zvyšuje za 15 % a mobilní prodej představují 17.5 % celkový prodej, 7.5 % zvýšení pouze čtyř měsíců.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
