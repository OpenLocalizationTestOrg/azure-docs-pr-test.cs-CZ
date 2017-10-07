---
title: "Machine Learning doporučení: Integrace jazyka JavaScript | Microsoft Docs"
description: "Dokumentace k Azure Machine Learning doporučení - JavaScript integrace-"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: 4c5f0eee4aa04ce823321d52985374c52850f0d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Doporučení Azure Machine Learning – integrace v jazyce JavaScript
> [!NOTE]
> Měli byste začít používat hello kognitivní služby API doporučení místo tuto verzi. Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate)
> 
> 

Tento dokument zobrazit v ní jak toointegrate váš web pomocí jazyka JavaScript. Hello JavaScript vám umožní toosend získávání dat událostí a doporučení tooconsume po sestavení modelu doporučení. Všechny operace, které se provádí prostřednictvím JS lze také provést z na straně serveru.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Obecné – přehled
Integrace serveru s Azure ML doporučení se skládají na fáze 2:

1. Odesílání událostí do Azure ML doporučení. Tato akce povolí toobuild model doporučení.
2. Využívat hello doporučení. Jakmile je vytvořen hello modelu můžete využívat hello doporučení. (Tento dokument nevysvětlují, jak toobuild modelu, přečtěte si hello úvodní příručka tooget Další informace o tom,).

<ins>Fáze I</ins>

V hello první fázi vložit do stránek html malé knihovna JavaScript, která umožňuje hello stránky toosend událostí, kdy k nim dojde na stránce html hello do Azure ML doporučení servery (prostřednictvím trhu dat):

![Výkres1][1]

<ins>Fáze II</ins>

V hello druhé fáze, když chcete tooshow hello doporučení na stránce hello vyberete jednu z hello následující možnosti:

1 serveru (ve fázi hello vykreslování stránky) volá doporučení tooget Azure ML doporučení serveru (prostřednictvím trhu Data). Hello výsledky budou zahrnovat seznam id položky. Váš server potřebuje tooenrich hello výsledky s položkami hello metadata (např. obrázky, popis) a odesílat hello vytvořit stránku toohello prohlížeče.

![Drawing2][2]

2. hello Další možností je toouse hello malý JavaScript soubor z fáze jeden tooget jednoduché doporučené položek seznamu. je zde přijatá data Hello zeštíhlen než hello, jeden v první možnosti hello.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Požadavky
1. Vytvořte nový model pomocí hello rozhraní API. O tom najdete úvodní příručka hello toodo ho.
2. Zakódovat váš &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; pomocí base64. (Bude se používat pro hello základní ověřování tooenable hello JS kód toocall hello rozhraní API).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. Odesílání událostí získávání dat pomocí jazyka JavaScript
Následující kroky Hello usnadňují odesílání událostí:

1. Zahrnují knihovny JQuery ve vašem kódu. Můžete ji stáhnout z nuget v hello následující adresu URL.
   
     http://www.nuget.org/Packages/jQuery/1.8.2
2. Zahrnout hello skriptu Java doporučení knihovny z hello následující adresu URL: http://aka.ms/RecoJSLib1
3. Inicializace knihovny Azure ML doporučení s příslušnými parametry hello.
   
     <script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Odesílání hello příslušné události. Najdete v podrobné části níže na všechny typu události (příklad click – událost) <script> Pokud (typeof AzureMLRecommendationsEvent == "undefined") {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script>

### <a name="31----limitations-and-browser-support"></a>3.1.    Omezení a podpora prohlížečů.
Toto je odkaz na implementaci a je zadána, jako je. Má podporovat všechny hlavní prohlížeče.

### <a name="32----type-of-events"></a>3.2.    Typ události
Existují 5 typy událostí, které hello knihovně podporuje: klikněte na, klikněte na doporučení, přidejte tooShop košíku, odeberte z košíku obchod a nákup. Není k dispozici další událost, která je volána přihlášení použité tooset hello uživatelský kontext.

#### <a name="321-click-event"></a>3.2.1. Click – událost
Tato událost je třeba použít kdykoli uživatel klikli na položku. Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky hello; na této stránce musí být tato událost aktivuje.

Parametry:

* události (řetězec, povinná) "Kliknutím"
* Položka jedinečný identifikátor položky hello (řetězec, povinné)
* Název položky (řetězec, volitelný) hello název položky hello
* Popis zboží (řetězec, volitelný) hello popis položky hello
* itemCategory (řetězec, volitelný) hello kategorie hello položky
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Nebo s volitelnými daty:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a>3.2.2. Doporučení Click – událost
Tato událost je třeba použít kdykoli uživatel klikli na položku, který jste získali od Azure ML doporučení jako doporučenou položka. Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky hello; na této stránce musí být tato událost aktivuje.

Parametry:

* události (řetězec, povinná) "recommendationclick"
* Položka jedinečný identifikátor položky hello (řetězec, povinné)
* Název položky (řetězec, volitelný) hello název položky hello
* Popis zboží (řetězec, volitelný) hello popis položky hello
* itemCategory (řetězec, volitelný) hello kategorie hello položky
* semen (pole řetězců, volitelný) - hello semen, které generuje hello doporučení dotazu.
* recoList (pole řetězců, volitelný) - hello výsledek požadavku hello doporučení, která vygenerovala hello položku, která označeného.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Nebo s volitelnými daty:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a>3.2.3. Přidat událost nákupní košík
Tato událost má použít při přidání uživatele hello toohello položky nákupní košík.
Parametry:

* události (řetězec, povinná) "addshopcart"
* Položka jedinečný identifikátor položky hello (řetězec, povinné)
* Název položky (řetězec, volitelný) hello název položky hello
* Popis zboží (řetězec, volitelný) hello popis položky hello
* itemCategory (řetězec, volitelný) hello kategorie hello položky
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Odebrat nákupního košíku událostí
Tato událost se mají použít při hello uživatel odebere k položce toohello nákupní košík.

Parametry:

* události (řetězec, povinná) "removeshopcart"
* Položka jedinečný identifikátor položky hello (řetězec, povinné)
* Název položky (řetězec, volitelný) hello název položky hello
* Popis zboží (řetězec, volitelný) hello popis položky hello
* itemCategory (řetězec, volitelný) hello kategorie hello položky
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. Nákup událostí
Tato událost je třeba použít při zakoupení hello uživatele jeho nákupní košík.

Parametry:

* události (řetězec) "zakoupit"
* položky (zakoupili []) – pole, která uchovává záznam pro každou položku zakoupili.<br><br>
  Zakoupené formát:
  * Položka (řetězec) Jedinečný identifikátor položky hello.
  * Count (int nebo string) - počet položek, které byly zakoupeny.
  * cena (float nebo řetězec) - volitelné pole - hello cena hello položky.

Následující příklad Hello ukazuje zakoupení 3 položky (33, 34, 35), dva s všechna pole naplněno (položky, počet, cena) a jeden (položka 34) bez cenu.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Události přihlášení uživatele
Azure ML doporučení události vytvoří knihovny a používání souborů cookie v pořadí tooidentify události, které pocházejí z hello stejný prohlížeč. V pořadí tooimprove hello modelu umožňuje výsledky Azure ML doporučení tooset jedinečnou identifikaci uživatele, který přepíše soubor cookie využití hello.

Tato událost je třeba použít po hello uživatele přihlášení tooyour lokality.

Parametry:

* události (řetězec) "userlogin"
* uživatel (řetězec) jedinečnou identifikaci uživatele hello.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Využívat doporučení prostřednictvím jazyka JavaScript
Hello kód, který využívá hello doporučení je aktivován některé událostí jazyka JavaScript webovou stránku hello klienta. Hello doporučení odpověď obsahuje hello doporučené ID položky, jejich názvy a svoje hodnocení. Je nejlepší toouse, které tato možnost jenom pro zobrazení seznamu hello doporučená položky - složitější zpracování (jako je například přidávání metadata položky hello) by mělo být provedeno na hello integrace straně serveru.

### <a name="41-consume-recommendations"></a>4.1 využívat doporučení
tooconsume doporučení, které že budete potřebovat tooinclude hello požadované knihovny jazyka JavaScript v stránky a toocall AzureMLRecommendationsStart. Najdete v části 2.

tooconsume doporučení pro jednu nebo více položek, je třeba volat metodu toocall: AzureMLRecommendationsGetI2IRecommendation.

Parametry:

* položky (pole řetězce) – jeden nebo více položek tooget doporučení pro. Pokud Fbt sestavení využívat, pak můžete nastavit zde jen jednu položku.
* numberOfResults (int) - počet požadovaných výsledků.
* includeMetadata (logická hodnota, volitelný) – Pokud nastavit too'true "označuje, že hello metadata pole musí být naplněn ve výsledku hello.
* Funkce – funkce, která bude zpracovávat hello doporučení zpracování vrátila. Hello data jsou vrácena jako pole:
  * Položka – položka jedinečné id
  * název – název položky (pokud existují v katalogu)
  * hodnocení - doporučení hodnocení
  * metadata – řetězec, který představuje hello metadata položky hello

Příklad: hello následující kód požadavky 8 doporučení pro položku "64f6eb0d-947a-4c18-a16c-888da9e228ba" (a ne zadáním includeMetadata - ho implicitně upozorněním, že se nevyžaduje žádná metadata), pak řetězení hello výsledky do vyrovnávací paměti.

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
