---
title: "Tom, jak hledat adresu pomocí služby Azure na základě polohy (preview) vyhledávání | Microsoft Docs"
description: "Zjistěte, jak hledat adresu pomocí Azure umístění na základě Services (preview) vyhledávací služba"
services: location-based-services
keywords: "Nemáte přidat nebo upravit klíčová slova bez konzultace ohledně specialistou na SEO."
author: philmea
ms.author: philmea
ms.date: 11/29/2017
ms.topic: article
ms.service: location-based-services
ms.openlocfilehash: cacaaab869d3a7752b5a750f01bbfbdaf79814f7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-find-an-address-using-the-azure-location-based-services-preview-search-service"></a>Postup nalezení adresu pomocí služby Azure na základě polohy (preview) vyhledávání
Služba vyhledávání se RESTful sada rozhraní API určená pro vývojáře k vyhledání adresy, míst, bodů zájmu, výpisech obchodních a jiných zeměpisné údaje. Služba vyhledávání zeměpisnou šířku a délku přiřadí konkrétní adresu, křížové ulici, geografické funkci nebo v případě zájmu (bodů zájmu). Zeměpisné šířky a délky hodnot vrácených rozhraním API služby vyhledávání můžete použít jako parametry v jiných služeb na základě umístění Azure jako je například rozhraní API toku provozu a trasy.

## <a name="prerequisites"></a>Požadavky
* Nainstalujte [aplikaci Postman](https://www.getpostman.com/apps).

* Máte účet Azure umístění na základě služby a klíč. Informace o vytvoření účtu a klíč načítání, najdete v části [Správa vašeho účtu služeb na základě umístění Azure a klíče](how-to-manage-account-keys.md). 

## <a name="using-fuzzy-search"></a>Pomocí přibližné vyhledávání

Rozhraní API výchozí pro vyhledávací službu je přibližné vyhledávání, která zpracovává vstupy libovolnou kombinaci adresy nebo bodů zájmu tokeny. Toto hledání rozhraní API je kanonický 'jeden řádek vyhledávání a je užitečné, když si nejste jisti jaké vstupy uživatele jako vyhledávací dotaz. Rozhraní API přibližné vyhledávání je kombinace bodů zájmu vyhledávání a určování zeměpisných souřadnic. Rozhraní API může být také vážené s kontextové pozice (tabulky lat. nebo fyzický pevný disk. spárujte), plně omezené souřadnice a protokolu radius, nebo mohou být provedeny další obecně bez jakékoli geograficky usměrnění údajů bodu ukotvení.

Většina vyhledávací dotazy jako výchozí se maxFuzzyLevel = 1' k získání výkonu a snížení neobvyklou výsledky. Toto výchozí nastavení je možné přepsat podle potřeby každý požadavek předáním v parametr dotazu ' maxFuzzyLevel = 2' nebo '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Hledat adresu pomocí přibližné vyhledávání

1. Otevřete aplikaci Postman, klikněte na nový | Vytvořte nový a vyberte **požadavek GET**. Zadejte název žádosti o **přibližné vyhledávání**, vyberte kolekci nebo ho uložit pro, a klikněte na složku **Uložit**.

    Další informace najdete v dokumentaci k Postman požadavky.

2. Na kartě Tvůrce vyberte **získat** metodu HTTP a zadejte adresu URL požadavku pro koncový bod rozhraní API.

    ![Přibližné vyhledávání ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/fuzzy/json? |
    | Autorizace | Bez ověřování |

    **Json** atribut v cesty URL určuje formát odpovědi. Json v tomto článku používají pro snadné použití a přehlednosti. Můžete najít formáty k dispozici odpověď v **získat přibližné vyhledávání** definice [umístění na základě služby funkční referenční dokumentace rozhraní API] (https://docs.microsoft.com/rest/api/location-based-services/search/getsearchfuzzy).

3. Klikněte na tlačítko **parametry**a zadejte následující klíč / hodnota dvojice k použití jako parametry dotazu nebo cestu v adrese URL požadavku:

    ![Přibližné vyhledávání ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze rozhraní API. | 1.0 |
    | klíč předplatného | \<klíč služeb na základě umístění Azure\> |
    | query | pizza |

4. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 

    Řetězec dotazu nejednoznačný "pizza" vrátí 10 bodů zájmu (bodů zájmu) výsledků s kategorií, které spadají do "pizza" a "restaurace". Každý výsledek vrátí adresu, zeměpisnou šířku / zobrazit hodnoty zeměpisné délky, port a vstupních bodů pro umístění.
    
    Výsledky jsou nastaveny pro tento dotaz není vázaný na libovolné místo konkrétní odkaz. Můžete použít **countrySet** parametr k určení pouze země, pro které aplikace potřebuje pokrytí, jako výchozí chování je na celém světě potenciálně vrácení nepotřebné výsledky hledání.

5. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |------------------|-------------------------|
    | countrySet | USA |
    
    Výsledky jsou nyní ohraničené kód země a dotaz vrátí pizza restaurace ve Spojených státech amerických.
    
    Zajistit výsledky zaměřené na konkrétní na konkrétní umístění můžete dotazovat bodu zájmu a použít vrácený zeměpisnou šířku a zeměpisnou délku hodnoty v volání ke službě přibližné vyhledávání. V takovém případě používá k vrácení umístění ručička místo Praha službu vyhledávání a použít tabulky lat. / fyzický pevný disk. hodnoty pro orientaci hledání.
    
4. Parametry, zadejte následující klíč / hodnota páry a klikněte na tlačítko **odeslat**:

    ![Přibližné vyhledávání ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Klíč | Hodnota |
    |-----|------------|
    | LAT | 47.62039 |
    | fyzický pevný disk | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Hledat vlastnosti adresy a souřadnice 

Můžete předat úplné nebo částečné adresu adres rozhraní API služby Search a obdrží odpověď, který obsahuje vlastnosti podrobné adresy například Okres nebo dělení, jakož i poziční hodnoty zeměpisné šířky a délky.

1. V Postman, klikněte na **novou žádost o** | **požadavek GET** a pojmenujte ji **hledání adresy**.
2. Na kartě Tvůrce vyberte **získat** metodu HTTP, zadejte adresu URL požadavku pro koncový bod rozhraní API a ověřovací protokol, vyberte, pokud existuje.

    ![Hledání adresy ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/address/json? |
    | Autorizace | Bez ověřování |

2. Klikněte na tlačítko **parametry**a zadejte následující klíč / hodnota dvojice k použití jako parametry dotazu nebo cestu v adrese URL požadavku:
    
    ![Hledání adresy ](./media/how-to-search-for-address/address_search_params.png)
    
    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze rozhraní API. | 1.0 |
    | klíč předplatného | \<klíč služeb na základě umístění Azure\> |
    | query | 400 široce St, Seattle, WA 98109 |
    
3. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 
    
    V takovém případě zadaný dotaz celou adresu a přijímat jeden výsledek v textu odpovědi. 
    
4. Ve parametry upravte řetězec dotazu, který má následující hodnotu:
    ```
        400 Broad, Seattle
    ```

5. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | typeahead | true (pravda) |

    **Typeahead** příznak informuje rozhraní API pro vyhledávání adresu částečné vstup považovat za zařízení dotazu a vrátíte se pole prediktivní hodnot.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Vyhledejte adresu pomocí Reverse hledání adresy
1. V Postman, klikněte na **novou žádost o** | **požadavek GET** a pojmenujte ji **Reverse hledání adresy**.

2. Na kartě Tvůrce vyberte **získat** metodu HTTP a zadejte adresu URL požadavku pro koncový bod rozhraní API.
    
    ![Zpětná adresa URL vyhledávání ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/address/reverse/json? |
    | Autorizace | Bez ověřování |
    
2. Klikněte na tlačítko **parametry**a zadejte následující klíč / hodnota dvojice k použití jako parametry dotazu nebo cestu v adrese URL požadavku:
    
    ![Zpětná adresa parametry vyhledávání ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze rozhraní API. | 1.0 |
    | klíč předplatného | \<klíč služeb na základě umístění Azure\> |
    | query | 47.59093,-122.33263 |
    
3. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 
    
    Odpověď obsahuje položku bodů zájmu Safeco pole s bodů zájmu kategorie "stadium". 
    
4. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | číslo | true (pravda) |

    Pokud [číslo](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu je odeslán s požadavkem, odpověď může zahrnovat na straně ulici (doleva nebo doprava) a také posunutí pozice pro toto číslo.
    
5. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | spatialKeys | true (pravda) |

    Když [spatialKeys](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu je nastavena, odpověď obsahuje důvěrné geograficky prostorových klíčové informace pro zadané umístění.

6. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | returnSpeedLimit | true (pravda) |
    
    Když [returnSpeedLimit](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu je nastavena, vrátí odpověď z odeslaných limit rychlosti.

7. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | returnRoadUse | true (pravda) |

    Když [returnRoadUse](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu je nastavena, odpověď vrací pole použití silniční pro reversegeocodes ulice úrovni.

8. Přidat následující klíč / hodnota pár k **parametry** části a klikněte na tlačítko **odeslat**:

    | Klíč | Hodnota |
    |-----|------------|
    | roadUse | true (pravda) |

    Dotaz zpětné geocode můžete omezit na určitý typ silniční použití pomocí [roadUse](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parametr dotazu.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Vyhledejte křížové ulici pomocí Reverse ulice hledání mezi adresy

1. V Postman, klikněte na **novou žádost o** | **požadavek GET** a pojmenujte ji **Reverse adres mezi ulici vyhledávání**.

2. Na kartě Tvůrce vyberte **získat** metodu HTTP a zadejte adresu URL požadavku pro koncový bod rozhraní API.
    
    ![Reverse hledání mezi ulici adresy ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametr | Navrhovaná hodnota |
    |---------------|------------------------------------------------|
    | Metoda HTTP | GET |
    | Adresa URL požadavku | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Autorizace | Bez ověřování |
    
3. Klikněte na tlačítko **parametry**a zadejte následující klíč / hodnota dvojice k použití jako parametry dotazu nebo cestu v adrese URL požadavku:
    
    | Klíč | Hodnota |
    |------------------|-------------------------|
    | verze rozhraní API. | 1.0 |
    | klíč předplatného | \<klíč služeb na základě umístění Azure\> |
    | query | 47.59093,-122.33263 |
    
4. Klikněte na tlačítko **odeslat** a zkontrolujte text odpovědi. 

## <a name="next-steps"></a>Další postup
- Prozkoumejte [služby Azure umístění na základě Serices Search](https://docs.microsoft.com/rest/api/location-based-services/search) dokumentaci k rozhraní API 
