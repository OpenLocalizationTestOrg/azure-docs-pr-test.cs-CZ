---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží - Push nebo Reach"
description: "Řešení potíží s uživatelské interakce a oznámení v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3f1886b7-1fdd-47f4-b6b0-d79f158d5ef3
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4ee0b34b9b753a98ccf24863acb28a5dc76bfb95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Průvodce řešením potíží se službami Push a Reach
Hello následují možných problémů se můžete setkat s jak Azure Mobile Engagement odesílá informace tooyour uživatele.

## <a name="push-failures"></a>Push selhání
### <a name="issue"></a>Problém
* Oznámení nepodporují (v aplikaci z aplikace nebo obojí).

### <a name="causes"></a>Způsobí, že
* Kolikrát selhání nabízené je jako ukazatel toho, že není správně integraci Azure Mobile Engagement Reach a další funkce Azure Mobile engagementu nebo že je upgrade vyžaduje v hello SDK toofix známý problém s nová platforma operačního systému nebo zařízení.
* Pokud je problém v aplikaci nebo aplikaci z testovací právě nabízení v aplikaci a právě toodetermine nabízené z aplikace.
* Testování z hello uživatelského rozhraní a hello rozhraní API v řešení potíží toosee krok jaké další chybové informace jsou k dispozici obou místech.
* Mimo aplikaci nabízených oznámení nebude fungovat, pokud Azure Mobile Engagement a Reach jsou integrované v hello SDK.
* Oznámení nebude fungovat, pokud certifikáty jsou neplatné nebo používají PRODUKČNÍMU vs. DEV správně (jenom iOS). (**Poznámka:** "mimo aplikaci" nabízená oznámení nemusí být doručují tooiOS, pokud máte oba hello vývoj (vývoj) a provozní (PRODUKČNÍMU) verze vaší aplikace nainstalovány na hello stejné zařízení od související token zabezpečení hello certifikátem může být zrušena společností Apple. tooresolve tento problém, odinstalujte hello Vývojářů a PRODUKČNÍMU verzí aplikace a znovu nainstalovat jenom hello jednu verzi na zařízení.)
* Mimo aplikaci nabízená počty proto liší na různých platformách (iOS ukazuje menší informace než Android pokud nativní oznámení jsou zakázány na zařízení, hello rozhraní API můžete zadat další informace než hello uživatelského rozhraní na nabízené statistiky).
* Mimo aplikaci můžete blokovat nabízených oznámení zákazníci na úrovni operačního systému (iOS a Android).
* Mimo aplikaci nabízených oznámení se zobrazí jako zakázané v hello Azure Mobile Engagement uživatelského rozhraní, pokud nejsou správně integrované, ale může selhat bezobslužně z hello rozhraní API.
* V aplikaci nabízených oznámení nebude fungovat, pokud Azure Mobile Engagement a Reach jsou integrované v hello SDK.
* Nabízených oznámení GCM a ADM nebude fungovat, pokud Azure Mobile Engagement a konkrétní server hello jsou integrované v hello SDK (jen Android).
* V aplikaci a mimo aplikaci nabízených oznámení by měla být testována samostatně toodetermine Pokud je problém nabízené nebo Reach.
* V nabízených oznámení aplikace vyžadují aplikaci hello být otevřené toobe přijata.
* V aplikaci nabízených oznámení jsou často toobe instalace filtrovaná podle značky informace o aplikaci přihlášení nebo odhlášení.
* Pokud používáte vlastní kategorii v oznámení v aplikaci toodisplay Reach, budete potřebovat toofollow hello správné životní cyklus hello oznámení, jinak hello oznámení nemusí být vymazán při hello uživatele zprávu zavřete.
* Pokud kampaň začínat žádné datum ukončení a zařízení obdrží hello v oznámení aplikace, ale nezobrazuje se ještě, hello uživatel stále obdrží oznámení hello hello při příštím přihlášení do aplikace hello i v případě, že ručně ukončení kampaň hello.
* Pro problémy se službou hello Push rozhraní API potvrďte Opravdu chcete toouse hello Push API místo hello Reach API (protože hello Reach API se často používá) a že nejste matoucí hello "datová část" a "oznamovatelem" parametry.
* Testování kampaň nabízených pomocí obou zařízení, připojení přes Wi-Fi a 3G tooeliminate hello síťové připojení jako zdroj možných problémů.

## <a name="push-testing"></a>Push testování
### <a name="issue"></a>Problém
* Oznámení mohou být odeslány konkrétní zařízení tooa podle ID zařízení.

### <a name="causes"></a>Způsobí, že
* Testovací zařízení jsou instalace pro každou platformu, ale způsobuje událost v aplikaci na testovací zařízení a hledáte ID zařízení hello portálu by měly fungovat toofind ID zařízení pro všechny platformy.
* Testovací zařízení zavedením IDFA vs. IDFV (jenom iOS).

## <a name="push-customization"></a>Push přizpůsobení
### <a name="issue"></a>Problém
* Advanced obsah, nebude fungovat položky (oznámení, prstence, zavibrovat, obrázek, atd.).
* Odkazy z nabízených oznámení nepodporují (mimo aplikaci, v aplikaci, tooa webu, tooa umístění v aplikaci).
* Zobrazit nabízené statistiky, které push nebyly odeslány tooas (příliš mnoho nebo není dostatek) Spousta lidí dle očekávání.
* Push duplicitní a přijaté dvakrát.
* Nelze zaregistrovat testovací zařízení Azure Mobile Engagement sami (s vlastními produkčnímu nebo vývoj aplikací).

### <a name="causes"></a>Způsobí, že
* toolink tooa konkrétního umístění v aplikaci vyžaduje "kategorie" (jen Android).
* Přímé propojení schémata tooredirect uživatelé tooan alternativního umístění po kliknutí na nabízené oznámení potřebovat toobe vytvořené v a spravuje vaše aplikace a hello operačního systému zařízení není pomocí Mobile Engagement přímo. (**Poznámka:** mimo aplikaci oznámení se nemůže propojit přímo tooin umístění aplikaci s iOS jako je tomu u Android.)
* Externí image servery potřebovat toobe možné toouse HTTP "GET" a "HEAD" pro velký obrázek nabízených oznámení toowork (jen Android).
* V kódu, můžete zakázat agenta Azure Mobile Engagement hello po otevření hello klávesnice a svůj kód znovu aktivovat hello Azure Mobile Engagement agenta po zavření klávesnice hello tak, aby hello klávesnice nebude mít vliv na vzhled hello vaší oznámení (jenom iOS).
* Některé položky nejsou funkční v testovací simulace, ale pouze skutečné kampaně (oznámení, prstence, zavibrovat, obrázek, atd.).
* Žádné na straně serveru, který data se protokolují, když použijete tlačítko hello příliš "test" nabízených oznámení. Data se protokolují pouze pro skutečné nabízené kampaně.
* toohelp izolovat problém, Poradce při potížích s: test, simulaci a skutečné kampaň vzhledem k tomu, že každý fungují trochu jinak.
* Hello dobu, kterou vaše "v aplikaci" a "kdykoliv" kampaně jsou naplánované toorun můžete ovlivňuje doručení čísla od kampaň budou pouze doručena toousers, kdo jsou "v aplikaci" průběhu kampaň hello (a uživatelé, kteří mají jejich nastavení zařízení nastavit tooreceive oznámení "mimo aplikaci").
* Hello rozdíly mezi jak Android a iOS popisovač mimo aplikaci oznámení je obtížné toodirectly Porovnat statistiku nabízené mezi hello Android a iOS verze vaší aplikace. Android poskytuje další úroveň oznámení informace operačního systému než iOS. Android sestavy při přijatých, kliknutí na nebo odstranit v centru oznámení hello nativní oznámení, ale iOS nehlásí tyto informace, pokud se po kliknutí na hello oznámení. 
* Hlavním důvodem Hello, které jsou jiné než než kampaně reach "doručit" čísla pro různé je, že "v aplikace" a "mimo aplikaci" oznámení, se počítají jinak "stisknutí" čísla. "V aplikaci" oznámení zpracovává Mobile Engagement, ale "mimo aplikaci" oznámení zpracovává hello Centrum oznámení na hello operačního systému zařízení.

## <a name="push-targeting"></a>Push cílení
### <a name="issue"></a>Problém
* Součástí cílení nebude fungovat podle očekávání.
* Cílení na značky informace o aplikaci nebude fungovat podle očekávání.
* Cílení na geografického umístění, nebude fungovat podle očekávání.
* Jazykového nefungují podle očekávání.

### <a name="causes"></a>Způsobí, že
* Ujistěte se, že jste nahráli značky informace o aplikaci prostřednictvím hello Azure Mobile Engagement uživatelského rozhraní nebo rozhraní API.
* Omezení hello nabízené rychlost nebo nabízené kvóta na úrovni aplikace hello nebo limitující hello cílové skupiny na úrovni hello kampaň můžete zakázat osoby přijímání konkrétní push i v případě, že splňují vaše kritéria cílení. 
* Nastavení "Jazyk" je jiné než cílení založený na zemi nebo národní prostředí, které se také liší od cílení podle geografického umístění na telefonu umístění nebo umístění GPS.
* odeslání zprávy Hello v hello "výchozí jazyk" tooany zákazníkovi, který nemá jejich zařízení, nastavit tooone hello alternativní jazyky, které zadáte.

## <a name="push-scheduling"></a>Push plánování
### <a name="issue"></a>Problém
* Plánování nabízené nebude fungovat dle očekávání, (odeslat příliš časnému nebo zpožděné).

### <a name="causes"></a>Způsobí, že
* Časová pásma může problémy s plánování, zvláště při používání hello koncoví uživatelé časové pásmo.
* Funkce Rozšířené nabízené zpozdit nabízených oznámení.
* Cílení na základě na telefon, který se může zpozdit nastavení (namísto značky informace o aplikaci) nabízených oznámení, protože Azure Mobile Engagement může mít toorequest data z hello phone reálném čase před odesláním oznámení.
* Kampaně vytvořen bez koncové datum hello nabízené místně uložených na zařízení hello a zobrazit ji hello při příštím otevření aplikace hello i v případě, že ručně hello kampaň skončí.
* Spouštění více než jeden kampaně v hello stejnou dobu může trvat delší dobu tooscan vaší uživatelské základny (zkuste tooonly spuštění jedné kampaně najednou s maximálně čtyři, také cíle jen aktivní uživatele tooyour tak, aby starý uživatelé nemají toobe zkontrolovat).
* Pokud použijete možnost "Ignorovat cílovou skupinu, nabízení se odešle toousers prostřednictvím rozhraní API hello" hello v části "Kampaně" hello kampaně Reach hello kampaň bude neodesílal automaticky, budete potřebovat toosend ručně pomocí hello Reach API.
* Pokud používáte vlastní kategorii v oznámení v aplikaci toodisplay Reach, budete potřebovat toofollow hello správné životní cyklus oznámení, jinak hello oznámení nemusí být vymazán při hello uživatele zprávu zavřete.

