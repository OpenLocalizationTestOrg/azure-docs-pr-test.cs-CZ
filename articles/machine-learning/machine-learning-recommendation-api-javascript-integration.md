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
redirect_document_id: TRUE
ms.openlocfilehash: 8f27962d097bffc2a03de80244ae41d6573a4bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Doporučení Azure Machine Learning – integrace v jazyce JavaScript
> [!NOTE]
> Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi. Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)
> 
> 

Tento dokument zobrazit v ní jak integrovat svůj web pomocí jazyka JavaScript. Jazyk JavaScript umožňuje odesílat události získávání dat a využívat doporučení po sestavení modelu doporučení. Všechny operace, které se provádí prostřednictvím JS lze také provést z na straně serveru.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Obecné – přehled
Integrace serveru s Azure ML doporučení se skládají na fáze 2:

1. Odesílání událostí do Azure ML doporučení. To vám umožní vytvořit model doporučení.
2. Využívat doporučení. Jakmile je vytvořen modelu můžete využívat doporučení. (Tento dokument nevysvětluje, jak vytvořit model, přečíst úvodní příručka získat další informace o tom,).

<ins>Fáze I</ins>

V první fázi můžete vložit do stránek html malé knihovna JavaScript, která umožňuje stránce odesílat události, které se používají na stránce html do Azure ML doporučení servery (prostřednictvím trhu dat):

![Výkres1][1]

<ins>Fáze II</ins>

V druhé fázi, když chcete zobrazit doporučení na stránce vyberete jednu z následujících možností:

1 serveru (ve fázi vykreslování stránky) volá Azure ML doporučení serveru (prostřednictvím Data trhu) získat doporučení. Výsledky budou zahrnovat seznam id položky. Server je potřeba rozšířit výsledky s položkami metadata (např. obrázky, popis) a odešle do prohlížeče, vytvořené stránky.

![Drawing2][2]

2. Další možností je použití malý soubor JavaScript z první fáze jednoduchý seznam Doporučené položky. Data přijatá tady je zeštíhlen než ten, který v první možnosti.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Požadavky
1. Vytvořte nový model pomocí rozhraní API. O tom, jak to provést naleznete v Průvodci rychlý start.
2. Zakódovat váš &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; pomocí base64. (Bude se používat pro základní ověřování pro povolení JS kódu pro volání rozhraní API).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. Odesílání událostí získávání dat pomocí jazyka JavaScript
Následující kroky usnadňují odesílání událostí:

1. Zahrnují knihovny JQuery ve vašem kódu. Můžete ji stáhnout z nuget v následující adresu URL.
   
     http://www.nuget.org/Packages/jQuery/1.8.2
2. Zahrnout knihovně doporučení Java skriptu z následující adresy URL: http://aka.ms/RecoJSLib1
3. Inicializace knihovny Azure ML doporučení s příslušnými parametry.
   
     <script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Odeslat příslušné události. Najdete v podrobné části níže na všechny typu události (příklad click – událost) <script> Pokud (typeof AzureMLRecommendationsEvent == "undefined") {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script>

### <a name="31----limitations-and-browser-support"></a>3.1.    Omezení a podpora prohlížečů.
Toto je odkaz na implementaci a je zadána, jako je. Má podporovat všechny hlavní prohlížeče.

### <a name="32----type-of-events"></a>3.2.    Typ události
Existují 5 typy událostí, který podporuje knihovny: klikněte na tlačítko, klikněte na doporučení, přidat do košíku obchod, odeberte z košíku obchod a nákup. Není k dispozici další událost, která se používá k nastavení uživatelský kontext názvem přihlášení.

#### <a name="321-click-event"></a>3.2.1. Click – událost
Tato událost je třeba použít kdykoli uživatel klikli na položku. Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky; na této stránce musí být tato událost aktivuje.

Parametry:

* události (řetězec, povinná) "Kliknutím"
* Položka jedinečný identifikátor položky (řetězec, povinné)
* Název položky (řetězec, volitelné) název položky
* Popis zboží (řetězec, volitelný) – popis položky
* itemCategory (řetězec, volitelný) kategorie položky
  
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
Tato událost je třeba použít kdykoli uživatel klikli na položku, který jste získali od Azure ML doporučení jako doporučenou položka. Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky; na této stránce musí být tato událost aktivuje.

Parametry:

* události (řetězec, povinná) "recommendationclick"
* Položka jedinečný identifikátor položky (řetězec, povinné)
* Název položky (řetězec, volitelné) název položky
* Popis zboží (řetězec, volitelný) – popis položky
* itemCategory (řetězec, volitelný) kategorie položky
* doplňuje (pole řetězců, volitelný) - semen, které generuje dotaz doporučení.
* recoList (pole řetězců, volitelný) - výsledek požadavku doporučení, generovaný označeného položku.
  
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
Tato událost má použít při přidání položky do nákupního košíku uživatele.
Parametry:

* události (řetězec, povinná) "addshopcart"
* Položka jedinečný identifikátor položky (řetězec, povinné)
* Název položky (řetězec, volitelné) název položky
* Popis zboží (řetězec, volitelný) – popis položky
* itemCategory (řetězec, volitelný) kategorie položky
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Odebrat nákupního košíku událostí
Tato událost se mají použít při uživatel odebere položku, kterou chcete nákupní košík.

Parametry:

* události (řetězec, povinná) "removeshopcart"
* Položka jedinečný identifikátor položky (řetězec, povinné)
* Název položky (řetězec, volitelné) název položky
* Popis zboží (řetězec, volitelný) – popis položky
* itemCategory (řetězec, volitelný) kategorie položky
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. Nákup událostí
Tato událost je třeba použít při zakoupení uživatele jeho nákupní košík.

Parametry:

* události (řetězec) "zakoupit"
* položky (zakoupili []) – pole, která uchovává záznam pro každou položku zakoupili.<br><br>
  Zakoupené formát:
  * Položka (řetězec) Jedinečný identifikátor položky.
  * Count (int nebo string) - počet položek, které byly zakoupeny.
  * cena (float nebo řetězec) - volitelné pole - cenu položky.

Následující příklad ukazuje zakoupení 3 položky (33, 34, 35), dva s všechna pole naplněno (položky, počet, cena) a jeden (položka 34) bez cenu.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Události přihlášení uživatele
Knihovna Azure ML doporučení událostí vytvoří a používat soubor cookie za účelem zjištění události, které pocházejí ze stejného prohlížeče. Za účelem zlepšení výsledky modelu Azure ML doporučení umožňuje nastavit jedinečnou identifikaci uživatele, který přepíše soubor cookie využití.

Tato událost je třeba použít po přihlášení uživatele na váš web.

Parametry:

* události (řetězec) "userlogin"
* uživatel (řetězec) jedinečnou identifikaci uživatele.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Využívat doporučení prostřednictvím jazyka JavaScript
Kód, který využívá doporučení je aktivován některé událostí jazyka JavaScript klienta webové stránky. Odpověď doporučení obsahuje ID položky doporučené, jejich názvy a svoje hodnocení. Je vhodné tuto možnost použít pouze pro zobrazení seznamu položek doporučené – složitější zpracování (jako je například přidávání položky metadat) by mělo být provedeno na straně integrace serveru.

### <a name="41-consume-recommendations"></a>4.1 využívat doporučení
Využívat doporučení, které je třeba zahrnout požadované knihovny jazyka JavaScript v stránku a volat AzureMLRecommendationsStart. Najdete v části 2.

Využívat doporučení pro jednu nebo více položek, které je třeba volat metodu s názvem: AzureMLRecommendationsGetI2IRecommendation.

Parametry:

* položky (pole řetězce) - získat doporučení pro jednu nebo více položek. Pokud Fbt sestavení využívat, pak můžete nastavit zde jen jednu položku.
* numberOfResults (int) - počet požadovaných výsledků.
* includeMetadata (logická hodnota, volitelný) – když je nastaven na hodnotu true' značí, že metadata pole musí být naplněn ve výsledku.
* Funkce – funkce, která bude zpracovávat doporučení zpracování vrátila. Data jsou vrácena jako pole:
  * Položka – položka jedinečné id
  * název – název položky (pokud existují v katalogu)
  * hodnocení - doporučení hodnocení
  * metadata – řetězec, který představuje metadata položky

Příklad: Následující kód požadavky 8 doporučení pro položku "64f6eb0d-947a-4c18-a16c-888da9e228ba" (a ne zadáním includeMetadata - ho implicitně upozorněním, že se nevyžaduje žádná metadata), pak řetězení výsledky do vyrovnávací paměti.

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
