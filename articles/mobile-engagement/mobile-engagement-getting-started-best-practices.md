---
title: "aaaAzure Mobile Engagement – Příručka Začínáme s osvědčenými postupy"
description: "Příručka Začínáme k Azure Mobile Engagement s osvědčenými postupy k zahájení práce"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: dfce1183-6398-466e-aa7e-ed702fb52818
ms.service: mobile-engagement
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: d3f81c34fe74f741a60ca511caa55c312af73b1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure Mobile Engagement – příručka Začínáme s osvědčenými postupy
## <a name="overview"></a>Přehled
**Hello mobilní obrazovka je velmi frekventované místo:** 2013, studie z roku zjistila hello na průměrném mobilním zařízení nainstalováno 27 aplikací. Uživatelé obvykle stráví používáním svých aplikací asi 30 hodin měsíčně. Většina této doby připadá na používání sociálních sítí a hraní (asi 20 hodin). Roce 2014 měl hello Android trhu 1,5 milionu aplikací pro uživatele toochoose z. Hello Apple storu obsahovala asi 1,2 milionu aplikací. Popularita mobilních aplikací stále roste a vývojáři neustále uvádějí na trh nové mobilní aplikace. 

Průměrný mobilní uživatel Hello bude instalaci a odinstalaci aplikací se vysoká frekvence v závislosti na proměnách svých zájmů a vyskytne v aplikaci. V pořadí toodetermine hello úspěšnosti aplikace bude důležitých tooknow více než jen počet uživatelů, kteří si aplikaci nainstalovalo. Je důležité tooknow užitečné je vaše aplikace a pokud je změna tento trend využití. je potřeba znát Hello následující otázky:

* Jsou vaši uživatelé od toofind při nezajímavé nebo zastaralé aplikace? 
* Kolik uživatelů úplně přestalo aplikaci používat? 
* Mají nákupy z aplikace rostoucí nebo klesající tendenci?
* Nedokončují uživatelé pracovní postupy toocomplete kvůli problémům s aplikace hello nebo nedostatek zájmu? 
* Může aplikace zachovávat užitečnost a relevantní vynucením základní čerstvého obsahu tooyour uživatele? 
* Bude tento obsah hello stejné pro všechny uživatele nebo cílených segmenty uživatelů lišit v závislosti na chování v aplikaci? 

Podobně jako toothese tooquestions odpovědi pomáhají prodloužit hello života a výnosy z vaší aplikace. Mohou vám také pomoci zjistit, kdo aplikaci používá, a její uživatele si udržet. 

Souvisejících s médii aplikace mívají toohave některé hello nejvyšší uchování mezi uživateli. Jedním z důvodů je že neustále nabízejí čerstvého obsahu toousers. Časná implementace užitečných nabízených oznámení směrované segmentu uživatelů tooa obvykle toohave vysoký dopad na míru uchování aplikace. 

program Azure Mobile Engagement Hello je navrženou toohelp rozšířit hello životnost a zvyšovat míru uchování aplikace poskytnutím toogather metoda a analyzovat podrobné informace o použití hello vaší aplikace. Pomůže vám klasifikovat vaší uživatelské základny podle toobehavior a vytvářet cílené kampaně doručování nabízených oznámení a zprávy v aplikaci tooidentified segmenty uživatelů. Klíčové ukazatele výkonu měří aktivitu uživatelů vaší aplikace s ohledem na její různé aspekty. Azure Mobile Engagement poskytuje metody hello potřebujete toodetermine těchto klíčových ukazatelů výkonu. Zvyšuje hello vraťte se na vaší investice (ROI) tím, že poskytuje infrastrukturu hello musíte tooincrease zapojení vaší aplikace. 

Pořadí tooget hello naplno Azure Mobile Engagement je nutné toostart s dobře navrženým plánem zapojení uživatelů. Váš plán pomůže identifikovat podrobná data hello je nutné mít toosegment toobe uživatelů základní. Lze toho docílit na základě chování uživatelů a způsobu, jakým aplikaci používají. Aby váš plán toobe úspěšné je nejlepší postup tooclearly definovat hello klíčového ukazatele výkonu, které budou měřit cíle vaší aplikace hello. Definované ukazatele výkonu, můžete snadno vložit potřebné logiky hello ve aplikace toocollect jemné možnosti datech, které budete používat tooanalyze a vyhodnocovat u nich klíčových ukazatelů výkonu. Toto téma je příručkou s osvědčenými postupy definování klíčových ukazatelů výkonu hello, které budete používat ve svých plánech zapojení. 

## <a name="step-1-define-your-kpis-toofit-hello-bet-model"></a>Krok 1: Definování klíčových ukazatelů výkonu toofit hello OZT modelu
Správné definování klíčových ukazatelů výkonu lze toocomplete snadné. Aplikace navržené pro různá odvětví mají svá vlastní specifika a účely. To může zpravidla tooconfuse váš přístup. toohelp předejít, cílů a klíčových ukazatelů výkonu by se měly klasifikovat do tří kategorií: **obchodní**, **Engagement**, a **technické**. Toto je takzvaný hello **modelu OZT**.

Kvalitní plán má obecně vzato cíle s hello klíčových ukazatelů výkonu, které měří úspěch hello v každé z následujících kategorií modelu OZT hello hello.

#### <a name="business-kpis"></a>Obchodní klíčové ukazatele výkonu
Obchodní klíčové ukazatele výkonu by měla být nejjednodušší toobuild část hello. Tyto ukazatele jste již pravděpodobně v nějaké podobě definovali při plánování své mobilní aplikace. Obecně vzato pomáhají měřit výnosy z aplikace a návratnost investic do aplikace. Hello následující seznam uvádí některé ukázkové klíčové ukazatele výkonu, které vám mohou pomoci při definování vaše vlastní ukazatele výkonu:

* Obchodní klíčové ukazatele výkonu pro mediální obsah
  * počet kliknutí na reklamy
  * počet návštěv stránky na uživatele
  * počet souběžných odběrů
* Obchodní klíčové ukazatele výkonu pro herní obsah
  * počet nákupů z aplikace
  * průměrný výnos na uživatele
  * strávená doba na relaci
  * počet dnů hraní a aktuální úroveň ve hře
* Klíčové ukazatele výkonu pro elektronické obchodování
  * počet dnů používání aplikace
  * průměrný výnos na uživatele
  * průměrná cena zboží v košíku na pokladně
  * kategorie nejčastěji zobrazovaných a zakoupených produktů
* Obchodní ukazatele výkonu pro obsah z oblasti bankovnictví a pojišťovnictví
  * počet účtů
  * aktivované funkce
  * počet navštívených stránek s nabídkami
  * počet upozornění, která uživatel aktivuje nebo na ně klikne       

#### <a name="engagement-kpis"></a>Klíčové ukazatele zapojení
Klíčový Ukazatel zapojení je výkonu indikátor toomeasure hello zapojení uživatelů. Trendy v této oblasti pomáhají určit míru uchování aplikace hello. Zde je několik ukázkových klíčových ukazatelů výkonu tohoto typu:

* Aktivní uživatelé v hello posledních 7 dnů
* Počet neaktivních uživatelů hello posledních 7 dnů
* Počet uživatelů, kteří hello aplikaci nepoužili během 30 dnů  

Ukazatele v této oblasti mohou ovlivnit některé zřejmé externí faktory. Například byste mohli zvážit toobe mobilních zařízení s uživatelem za všech okolností. To může, ale také nemusí být pravda. Herní aplikace mívají vyšší využití toohave kolem svátků při hráči na hraní více nejsou ve škole nebo zaměstnání.   

Dobře definované klíčové ukazatele výkonu v této kategorii pomáhají měřit hello vztah mezi vaší aplikace a vašich zákazníků.

#### <a name="technical-kpis"></a>Technické klíčové ukazatele výkonu
Ukazatele výkonu v této kategorii pomáhají určit, zda se aplikace správně chová, zda nepřestává reagovat či zda v ní nedochází k chybám. Tyto ukazatele mohou měřit hello stavu aplikace a určovat problémy, které mohou uživatelům bránit v použití aplikace hello. Informace shromážděné pro tuto kategorii mohou obsahovat také informace o výkonu, které by byly relevantní toomarketing týmy. Hello data mohou být také užitečná pro řešení potíží pomocí IT a toohelp týmy podpory identifikovat uvedený chyby. 

Zde jsou některé příklady technických klíčových ukazatelů výkonu:

* informace o neošetřených a ošetřených výjimkách a jejich počtu 
* časové razítko poslední chyby
* tlačítko, na které uživatel naposledy kliknul, nebo stránka, kterou naposledy navštívil
* Využití paměti aplikace hello
* obnovovací frekvence aplikace
* Verze operačního systému, který hello aplikace běží na
* verze aplikace

Definování těchto klíčových ukazatelů výkonu toohelp měřit výkon aplikace a identifikovat potenciální chyby. Tyto indikátory by měly pomoci zkrátit čas hello potřebujete toodeliver opravu pro vaše zákazníky. Mohou vám také pomoci určit segment uživatelů, kteří se setkali s konkrétními potížemi. Můžete použít, že uživatel segmentace toocreate kampaně toodeliver oznámení týkající se dostupných oprav a potenciální propagační akce toohelp obnovit spokojenost zákazníků. 

#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Cvičení se scénářem 1: Vytvoření řídicího panelu klíčových ukazatelů výkonu
Při definování marketingové strategie by klíčové ukazatele výkonu měli reprezentovat pohled na každý z vašich hlavních cílů. Měly by být jasně definované datové body, které vám umožní vám toocollect podstatné informace toomonitor chování koncových uživatelů hello vaší aplikace a hello.

Klíčový ukazatel výkonu řídicí panel, který obsahuje hello níže uvedené informace sestavení

1. Jaké jsou hello klíčových ukazatelů výkonu pro hello aplikaci?
2. Jaké datové body použiji toorepresent jednotlivých klíčových ukazatelů výkonu?
3. Kde tato data ve své aplikaci najdu (na obrazovce, v nastavení, v systému…)?
4. Mohu pro tento klíčový ukazatel výkonu spustit sekvenci zapojení?

Můžete použít hello **Tvůrce klíčových ukazatelů výkonu** listu v našich [Media Playbook šablony] [ Media Playbook link] příklady a nápovědu.

## <a name="step-2-your-engagement-program"></a>Krok 2: Váš program zapojení
Kvalitní program zapojení mobilních zařízení je klíčovou komponentou vaší aplikace. Absolutně ten by měl obsahovat špičkový uvítací program, který provádí pro uživatele během hello prvních dnů používání vaší aplikace. To obvykle toohave velmi pozitivní vliv na zapojení uživatele a uchování aplikace. Ze studií vyplývá, že hello většina uživatelů přestat používat aplikace hello prvních několika dnů po instalaci. Chcete toostrive toomeet nebo při hello uživatele stále se zaměřuje na aplikaci již v rané fázi překročit podporovat jeho zájem očekávání zákazníka. Zkontrolujte, zda že je k dispozici hello klíčové přínosy a výhody aplikace tooyour zákazníků. 

![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Nabízená oznámení jsou hello nejlepší přístup tooearly oznámeních podporujících zapojení uživatelů uživatelů mobilních zařízení. Při používání nabízených oznámení je však nutné věnovat důkladnou pozornost rozdělení uživatelů do segmentů. Jakmile uživatel získá dojem, že je mu odesílán spam nebo nezajímavá oznámení, může to mít velmi neblahý vliv. Několika kliknutími uživatel může odstranit aplikaci nikdy tooreturn. Hello uživatel by měl příjmu vysoce přizpůsobené hodnoty v aplikaci a ne obecný spam.

Jakmile jsou uživatelé aktivně zapojení, potom program zapojení může pomoci jednotka dalších aspektů aplikace hello.

Například můžete například nastavit kampaň, která požaduje vaše toorate aktivní uživatelé vaší aplikace. Vzhledem k tomu, že uživatelé aplikaci nejaktivněji hello nejaktivnější a má hello největší zkušenosti s aplikaci, kterou byste očekávali toogive hello nejpřesnější hodnocení. Jakmile získáte vysoké hodnocení aplikace, může to pomoci až hello přirozenému růstu počtu stažení aplikace a snížit náklady na pořízení nového zákazníka.

#### <a name="engagement-sequence"></a>Sekvence zapojení
Globální program zapojení zahrnuje různé sekvence zapojení. Cílem každé sekvence je tooreach několika cílů.

###### <a name="life-push-sequence"></a>Sekvence nabízených oznámení životního cyklu
Hello cíle pro sekvence nabízených oznámení životnosti se liší v závislosti na životní cyklus hello hello uživatele zapojení aplikace hello. Uživatel může být nový, neaktivní nebo velmi aktivní. V různých fázích životního cyklu mohou mít uživatelé užitek z čerstvého obsahu ve formě tipů nebo odkazů toodocumentation hello. 

Například může potřebovat nového uživatele pomůže získávání aplikace orientované tooan nebo využívat nové uživatele pobídkové podobné toohello následující hello prvním spuštění aplikace hello...

*"Rádi toohave jste zaváděním! Mějte na paměti, toologin tooget vaše 1 měsíc zdarma! "*

###### <a name="behavioral-push-sequence"></a>Sekvence nabízených oznámení chování
cílem sekvence nabízených oznámení chování Hello je tooincrease využití podle chování uživatele shromážděných pro aplikaci hello.  

Například může mít velmi aktivní uživatel aplikace fantasy fotbalu užitek z s hello následující nabízené oznámení...

*„Honzo, jste pravý fanoušek fotbalu! Přihlaste se v části tooour NFL a win volný přístup toohello SuperBowl!"*

###### <a name="alerting-push-sequence"></a>Sekvence nabízených oznámení upozornění
Uživatelé ocení relevantní novinky týkající se jejich zájmů. Sekvence nabízených oznámení upozornění zvyšuje zapojení odesíláním upozornění na základě zřetelně projevených zájmů uživatele. To může být explicitní, když uživatel vybere zájmy v aplikaci hello. Ho může také odvodit z dat shromážděných během interakce uživatele s aplikací hello.

Hello uživatel aplikace pro elektronické obchodování například může pravidelně nakupovat dané značky kávy, což zaznamenáte pomocí obchodního klíčového ukazatele výkonu. Hello může zvýšit následující upozornění zapojení tohoto uživatele s aplikací hello.

*"Hi Petře, jedna z vašich oblíbených značek kávy se bude nacházet na prodej 25 % slevou hello první týden v září 2015. Děkujeme vám za jako zákazník a chtěli toomake zda jste vědět."*

###### <a name="rentention-push-sequence"></a>Sekvence nabízených oznámení uchování
Toto pořadí uživatelé tooretain cílů toohelp kampaní opakovaných nabízených oznámení pomocí jednotky, která podporují zapojení aplikace hello. To může pomoci zvýšit míru uchování aplikace, pokud uživatel hello požívá hello interakce. 

Hello uživateli aplikace související se sportem například může zobrazit hello následující nabízené oznámení týdně podle hello jeho oblíbených týmech:

*"Prvního toowin 200 bodů, najdete hlas zda hello sparta bude win víkendu porazí Slávii tato týdnu!"*

#### <a name="hello-3w-approach"></a>Hello 3W přístup
Ovládnutí koncepcí hello různých sekvencí nabízených oznámení vám pomůže zapojit koncové uživatele. Však stále potřebujete toouse hello 3W přístup pro přizpůsobení oznámení. Hello 3W přístupu je potřeba vyřešit kdo, co a kdy jednotlivá oznámení. Když získáte jasné odpovědi na tyto tři otázky, vaše oznámení by měla být správně zaměřena na podporu zapojení.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)

###### <a name="who-hello-user-segment-that-will-receive-messages"></a>Kdo: segment uživatelů, který bude přijímat zprávy hello
Nabízené oznámení tooyour uživatelé měli považovat za velmi citlivý komunikační kanál. Zajistěte, aby hello oznámení, že cílem segmentu uživatelů tooa toosend se dobře vymezená toohello zájmy uživatelů v daném segmentu. Nesprávně směrované oznámení je velmi pravděpodobně toohave negativní vliv na uživatele. Mohou ho považovat za spam úvodní tooyour aplikaci odinstalují. 

Při definování segmentu uživatelů, kteří budou dostávat oznámení, použijte kombinaci specifických technických kritérií a kritérií chování. Jednoduchý příklad určení segmentu uživatelů může být podobné toohello následující příkaz:

"Všichni uživatelé, kteří spustili hello mobilních aplikací pro hello poprvé před 3 dny a dvakrát bez skutečně přihlásili navštívili přihlašovací stránku hello".

Tato definice pomáhá identifikovat data hello potřebovali byste toocollect toosupport na konkrétní scénář.

###### <a name="what-hello-message-that-you-will-send"></a>Co: zpráva hello, který bude odesílat
**Styl podání**

V oznámeních podporujících zapojení uživatelů používejte styl podání, který je vhodný pro vás i pro uživatele v segmentech. To se o výborný dobře tooconnect s koncovým uživatelům a povyšte zájmu uživatele ve vaší aplikaci. 

**Přesměrování**

Nabízená oznámení lze použít pro více než otevírání aplikace hello. Pokud hello zpráva oznámení poskytuje kontext, například čerstvé zprávy nebo propagaci produktu, tento přímý odkaz na oznámení může přímo toohello pravým obsahu v rámci aplikace hello. toosupport, musíte vytvořit adresu URL aplikace hello toolet schéma spravovat hello přesměrování. Při práci na sekvencích zapojení je to důležitý krok, na který nesmíte zapomenout.

Přesměrování lze spravovat i pro jiné systémy. Například adresa URL akce je možné tooredirect koncoví uživatelé toomany jiných systémů, včetně hello následující:

* web
* poštovní schránka s již připraveným e-mailem
* schránka na zprávy SMS
* služba pro vytáčení telefonních čísel
* Přímo toohello obchodu s aplikacemi pro hodnocení aplikace hello. 

To poskytuje mnoho příležitostí tooengage koncovým uživatelům a vytvořte pravidla automatického tooimprove přínos.

**Formát a obsah**

Různé typy formátování nabízených oznámení:

1. **Oznámení** : umožňuje toousers zprávy inzerování toosend v různých situacích (mimo aplikaci, v aplikaci nebo kdykoliv).
2. **Hlasování** : povoleno toogather informace od koncových uživatelů ve formě odpovědí na otázky. Tyto odpovědi jsou poté dostupné při vytváření kritérií tootarget koncoví uživatelé.
3. **Datová oznámení** : umožňuje toosend base64 nebo binární soubor tooupdate hello aplikaci datové. Hello informace obsažené v datovém oznámení se odešlou prostředí tooyour aplikace toopersonalize hello uživatelů ve vaší aplikaci. Aplikace musí toobe určené toosupport hello dat v datových oznámeních.
4. **Dlaždice (pouze Windows Phone)** : umožňovala toouse hello Microsoft nabízené služby oznámení (MPNS) toosend nativní nabízená oznámení obsahující Data XML (podporováno od verze sady SDK 0.9.0. Hello velikost datové části pro dlaždice nesmí překročit 32 kilobajtů.). Zobrazí se zpráva Hello přímo na vaší kartě dlaždice.
5. **Webové zobrazení**: Webové zobrazení je automaticky otevírané okno s webovým obsahem. Toto automaticky otevírané okno se zobrazí, když hello uživatel klikne na nabízené oznámení hello. Webové zobrazení umožňuje toohave větší interakce s koncovým uživatelem hello.

> [!NOTE]
> Ujistěte se, že obsah hello, odesílaný jako nabízené oznámení splňuje hello příslušné platformy (iOS, Android, Windows) pokyny pro vývoj aplikací a odesílání nabízených oznámení.
> 
> 

###### <a name="when-hello-timing-of-your-campaign"></a>Kdy: načasování vaší kampaně hello
Pokud je nejlepší čas tooactivate hello kampaň spouštějící nabízená oznámení? Má se aktivovat ručně nebo automaticky? Má se opakovat? Určení hello správnou chvíli a frekvenci je nezbytné tooengage uživatelé s nejlepších výsledků dosáhnete hello. Pro každou sekvenci a scénář zapojení, musíte určit, kdy bude hello nejlepší čas toosend nabízená oznámení. Zde je několik možných příkladů:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Pokud odesíláte nabízená oznámení denně, musíte důkladně zvážit, zda uživatelé vaše zprávy nebudou považovat za spam. 

Azure Mobile Engagement nabízí dva způsoby, jak se vyhnout toohelp vaše zprávy považovali za spam. Nejprve pomocí spočívá v jemné segmentaci tooensure uděláte není cílový hello stejným uživatelům. Kromě toho Azure Mobile Engagement poskytuje tzv. „kvóty“. Pomocí této funkce lze omezit počet oznámení odeslaných v rámci kampaně. Například nastavení výchozí kvótu too5 za týden zajistí, že uživatel zahrnutý součástí segmentu uživatelů kampaně hello přijímá více než 5 oznámení pro tento týden.

#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Cvičení se scénářem 2: Vytvoření programu podpory zapojení
Věnujte nějaký čas shrnutí svých cílů a definování kampaní hello očekáváte, že tooplay pomocí specifických sekvencí. Ujistěte se, že použijete hello 3W přístup toohello oznámení kampaní. 

Použití hello **Program zapojení** listu v hello [Media Playbook šablony] [ Media Playbook link] příklady a nápovědu.

## <a name="step-3-app-integration"></a>Krok 3: Integrace aplikací
#### <a name="create-a-tag-plan"></a>Vytvoření plánu značek
toointegrate Azure Mobile Engagementu do vaší aplikace budete potřebovat toocreate plán značek. plán značek Hello je hello kamenem projektu hello. Definuje hello vztah mezi marketingovými specifikacemi, hello pracovní postup aplikace hello a hello skutečnými daty značek shromážděných v toomeasure aplikace hello klíčových ukazatelů výkonu. Určuje, jaké analytické údaje bude možné toosee hello portálu. Pomáhá také definovat segmenty uživatelů a odesílat cílená nabízená oznámení tooengage koncovým uživatelům. Jakmile máte hello plán značek definovaný, přidání toointegrate hello kódu do vaší aplikace se snadno pomocí hello Azure Mobile Engagement SDK.

Plán značek by neměl značkami označovat úplně vše v aplikaci. Měl by zahrnovat pouze data značek, která jsou součástí vaší strategie pro zapojení mobilních zařízení. V různých aplikacích se bude pravděpodobně lišit. Hello [Media Playbook šablony] [ Media Playbook link] poskytovaný platformou Azure Mobile Engagement pomůže sestavit plán značek pomocí dané metody. Použití hello **plán značek** list jako toobuilding Příručka plánování vaší značky.

Při definování sekce značek na listu hello, buďte velmi konkrétní. To je velmi důležité tooavoid nejasnostem. Podrobně popište jednotlivé scénáře ve kterých se budou odesílat jednotlivé značky. Zahrnout název hello hello aktivity, kde jsou jednotlivé značky vložené. To všechny zahrnuté do hello **Informative** součástí hello listu. List plánu značek Hello by měl být hello hlavní referencí pro ověřování testů. 

V hello **Data toocollect** část, váš vývojový tým musí nalézt hello typy, názvy, hodnoty a páry klíč/hodnota dalších informací všech značek, které budou vloženy aplikace hello.

Doporučujeme, abyste kontrola plán značek hello se všemi týmy podílejícími s projektem hello. Proveďte potřebné úpravy a ujistěte se, že je členům marketingových a vývojových týmů vše jasné.

Hello **příkaz pracovní** sešitu lze použít toohelp průvodce všechny účastníky projektu hello.

#### <a name="data-types"></a>Typy dat
Níže jsou uvedeny běžné typy dat podporovaných Azure Mobile Engagementem.

###### <a name="devices-and-users"></a>Zařízení a uživatelé
Azure Mobile Engagement rozpoznává uživatele tak, že pro každé zařízení generuje jedinečný identifikátor. Tento identifikátor se nazývá identifikátor zařízení hello (neboli deviceid). Generuje se tak, že všechny aplikace spuštěné na sdílející stejný zařízení hello stejný identifikátor zařízení.

###### <a name="sessions-and-activities"></a>Relace a aktivity
Relace je jedna instance aplikace hello spuštěná uživatelem. Hello relace začíná hello spuštění hello uživatele aplikace hello, dokud se zastaví.

Aktivita je logické seskupení sady bodů, které může aplikace hello během relace. Je obvykle na konkrétní obrazovku v aplikaci hello, ale může být, že nic definované hello logiku aplikace hello. Minimem by mělo být označení jednotlivých obrazovek aplikace. To vám umožní vám toounderstand hello uživatelské cesty.

###### <a name="events"></a>Události
Události jsou použité tooreport interakce uživatele s aplikace hello. Může se jednat o okamžité akce, například sdílení obsahu nebo spuštění videa. Označení událostí značkami vám poskytne kolekce dat ukazujích způsob interakce uživatelů s aplikací hello. 

###### <a name="jobs"></a>Úlohy
Úlohy jsou použité tooreport akce, které s dobou trvání. Níže je několik příkladů:

* provádění volání rozhraní API
* doba zobrazení reklam
* doba provádění úloh na pozadí 
* doba trvání procesu nákupu
* přehrávání videa

###### <a name="errors"></a>Chyby
Chyby jsou použité tooreport problémů zjištěných aplikace hello. Může se například jednat o chybné akce uživatelů nebo chyby volání rozhraní API.

###### <a name="application-information"></a>Informace o aplikaci
Informace o aplikaci (App-Info) se používá tootag data související s tooa uživatelské prostředí s aplikací. Je generován interakce uživatele s aplikací hello. 

Pro daný klíč Azure Mobile Engagement pouze uchovává informace o hello nejnovější hodnotu (žádná historie). Informace o aplikaci zjistí stav hello vaše aplikace nebo koncovým uživatelům. Například stav přihlášení hello nebo uživatele oblíbenou skupinu produktů.

###### <a name="crash-data"></a>Data o chybách
Data o chybách automaticky shromážděná sadou Mobile Engagement SDK hello selhání aplikace sestavy nebyla zpracována hello aplikací. Může se jednat například o neošetřené výjimky.

###### <a name="extra-data"></a>Doplňující data
Informace o událostech, chybách, aktivitách a úlohách lze rozšířit o další parametry. Toto je doplňující informace, které vývojář může zadat jako konkrétních dat z aplikace hello. Je to důležité pro jemné rozdělení uživatelů do segmentů. 

Například hodnota značky "článek" hello vám umožní koncovým uživatelům toosegment podle kdo zobrazil konkrétní článek. To však nemusí stačit. Lepší by bylo, kdyby stejná značka „article“ v aktivitě obsahovala také doplňující informace, například „news-category“ (kategorie zpráv). By to užitečné toodetermine dynamicky hello oblíbených kategorií uživatele hello. 

Další informace jsou poskytovány jako páry klíč–hodnota. V příkladu této mediální aplikace hello by hello doplňující informace "News – category" hello hodnotou této kategorie. Například „sports“ (sport), „economy“ (ekonomika) nebo „politics“ (politika).

#### <a name="tag-and-sdk-integration"></a>Značky a integrace sady SDK
Pro podrobné pokyny k integraci hello Azure Mobile Engagement SDK do aplikace, postupujte podle hello [integraci sady Engagement SDK](mobile-engagement-windows-store-integrate-engagement.md) dokumentaci na webu Azure. Z odkazů hello hello horní části této stránce zvolte svou cílovou platformu.

Doporučujeme vytvářet projekty pro dvě aplikace založené na Azure Mobile Engagement. Jeden pro vývoj a testovací pracovní a hello jiné pro fáze produkce. Váš IT tým z testu pracovní tooproduction po úspěšném otestování přijetí uživateli hello pak můžete zvýšit úroveň.

#### <a name="user-acceptance-testing-uat"></a>Testování přijetí uživateli
Účelem testování přijetí uživateli je ujistit se, že vše funguje tak, jak má. Lze dokončit pracovní postupy a na základě vašeho plánu značek získat všechna požadovaná data:

* Označování informace by měly být nasazené podle toodocumented koncepcí AZME.
* Měly by být shromážděny všechny potřebné informace (včetně doplňkových informací a informací o aplikaci).
* Klasifikace odpovídá podle tooyout plánu značek.
* Neměly by se odesílat žádné duplicitní značky.

Důkladně otestujte všechny typy hello chování oznámení vložených do aplikace

* oznámení, hlasování, datová oznámení mimo aplikaci a v aplikaci
* textová a webová zobrazení
* aktualizace oznámení, kategorie

#### <a name="setup"></a>Nastavení
Nastavení Azure Mobile Engagementu je velmi snadné. Veškerá dokumentace hello související toohello uživatelské rozhraní je k dispozici na webu Azure Mobile Engagement hello, [jak toonavigate hello uživatelské rozhraní](mobile-engagement-user-interface-home.md).

Doporučuje se spouštět nastavením správných rolí hello a členství v rolích pro uživatele hello projektu. To vám pomůže spravovat přístup toohello platformu pro všechny uživatele. Můžete použít následující role:

* Správci
* Vývojáři
* Prohlížející 

Dále:

* Zaregistrujte vaše deviceID tootest ve svém vlastním zařízení.
* Přejděte toohello nastavení vašeho účtu a nastavte hello časové pásmo toohave grafy a čas doručení oznámení nastavit pro časové pásmo.
* Přejít toohello nastavení vaší aplikace a zaregistrujte hello "App-info" potřebujete tootarget koncového uživatele v Reachi.

Další informace o tom, jak toorun první push kampaň oznámení, zkontrolujte [jak tooget pomocí spuštěna a správa nabízených oznámení tooreach si koncoví uživatelé tooyour](mobile-engagement-how-tos.md).

## <a name="conclusion"></a>Závěr
Programy zapojení se neustále opakují, a proto byste měli na základě nových poznatků o své aplikaci svůj program stále vylepšovat. 

Při získávání zkušeností s engagement strategie nepokoušejte toobuild úplnou globální strategii zapojení. Proveďte krok za krokem identifikování svých klíčových ukazatelů výkonu a jak tooleverage je. Strategie zapojení bude pro každou aplikaci jedinečná.

Poté, co jste si vytvořili nějaké zkušenosti, můžete zvážit přidání hello následující tooyour zapojení aplikace:

* Sledování: Získáte uživatele a pravděpodobně se vám podaří definovat zdroje sběru dat. Azure mobile Engagement může být propojený toodata kolekci zdrojů. Umožňuje vám toomonitor přínos jednotlivých zdrojů. Tyto informace budou zajímavé toomaximize vaší investice do pořízení. 
* Testování A/B: Jedná se o základní součást programu zapojení. Každá aplikace má svá vlastní specifika. Testování A/B vám umožní program zapojení vylepšit.
* Zeměpisná poloha: Funkce určování zeměpisné polohy mají obrovský potenciál pro značky. Děkujeme toothis funkce dosáhnout na správném místě hello a čas. Doporučujeme ověřit, že jste shromáždili dostatek dat chování koncových uživatelů před zahájením toouse geografického umístění.
* Datová oznámení: Datová oznámení jsou neviditelná oznámení. Datová oznámení umožňují přizpůsobit aplikaci na základě chování koncových uživatelů. Například pokud uživatel segmentu často zajímají špičkové technologické produkty, hello aplikace lze jim odeslat datové oznámení, které budou použity k přizpůsobení přizpůsobí domovskou stránku s obsahem špičkovými.

## <a name="next-steps"></a>Další kroky
* [Vytvoření účtu Azure Mobile Engagementu](mobile-engagement-create.md).
* Navštivte [definování strategie Mobile Engagementu](mobile-engagement-define-your-mobile-engagement-strategy.md) toolearn informace o definování strategie Mobile Engagementu.

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
