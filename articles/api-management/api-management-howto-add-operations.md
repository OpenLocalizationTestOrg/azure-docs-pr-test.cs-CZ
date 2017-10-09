---
title: "aaaHow tooadd operations tooan rozhraní API v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooadd operations tooan rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d57fa59a2b0ceb392cde23150a0cbb326e52d27d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-operations-tooan-api-in-azure-api-management"></a>Jak tooadd operations tooan rozhraní API v Azure API Management
Před použitím rozhraní API ve službě API Management, je nutné přidat operace. Tato příručka ukazuje jak tooadd a nakonfigurovat různé typy tooan operace rozhraní API ve službě API Management.

## <a name="add-operation"></a>Přidání operace
Operace se přidají a nakonfigurovat tooan rozhraní API portálu vydavatele hello. Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Vyberte hello požadovaného rozhraní API v portálu vydavatele hello a pak vyberte hello **Operations** kartě. 

![Operace][api-management-operations]

Klikněte na tlačítko **přidat operace** tooadd novou operaci. Hello **operaci nového** se zobrazí a hello **podpis** ve výchozím nastavení bude vybraná karta.

![Operace přidání][api-management-add-operation]

Zadejte hello **příkaz HTTP** tak, že zvolíte hello rozevíracího seznamu.

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

Definování šablon URL hello zadáním fragment adresy URL, který se skládá z jedné nebo více segmenty cesty adresy URL a nula nebo více parametrů řetězce dotazu. Hello URL šablony, připojením toohello základní adresu URL hello rozhraní API, identifikuje jednu operaci HTTP. Může obsahovat jeden nebo více s názvem proměnné částí, které jsou určeny složené závorky. Tyto proměnné části se nazývají parametry šablony a dynamicky přiřazené hodnoty extrahovat z adresy URL hello požadavek při zpracování žádosti hello pomocí platformy API Management hello.

> Šablona Hello adresa URL může obsahovat zástupné znaky. Například zadání `/*` bude předávat všechny žádosti týkající se této HTTP Metoda toohello zpět ukončení služby.

![Adresa URL šablony][api-management-url-template]

<a name="rewrite-url-template"></a>

V případě potřeby zadejte hello **přepisu adresy URL šablony**. To vám umožní toouse hello standardní šablona adresy URL pro zpracování příchozích požadavků na hello front-endu, při volání hello back-end prostřednictvím adresy URL převedený podle toohello přepisování šablony. Parametry šablony ze šablony hello adresa URL by měla použít v šabloně přepisování hello. Hello následující příklad ukazuje, jak obsahu typu kódovaný jako segment cesty v hello webové služby z předchozího příkladu hello lze zadat jako parametr dotazu v hello publikován rozhraní API prostřednictvím hello platformy API Management pomocí šablon hello adresy URL.

![Přepisování adres URL šablony][api-management-url-template-rewrite]

Volající toohello operace budou používat formát hello `/customers?customerid=ALFKI` a to bude příliš namapovat`/Customers('ALFKI')` při volání hello back endové služby.

**Zobrazení** název a **popis** zadejte popis hello operace a jsou použité tooprovide dokumentace toohello vývojáře, kteří používají toto rozhraní API v portálu pro vývojáře hello.

![Popis][api-management-description]

Popis operace Hello lze zadat jako prostý text nebo HTML v hello **popis** textové pole.

## <a name="operation-caching"></a>Operace ukládání do mezipaměti
Ukládání odpovědí do mezipaměti snižuje latence zaznamenatelného spotřebiteli hello rozhraní API, snižuje využití šířky pásma a snížení zatížení hello hello HTTP webové služby implementace hello rozhraní API. 

tooeasily a rychle povolit ukládání do mezipaměti pro hello operaci, vyberte hello **ukládání do mezipaměti** kartě a zkontrolujte hello **povolit** zaškrtávací políčko.

![Ukládání do mezipaměti][api-management-caching-tab]

**Doba trvání** Určuje časové období, během které hello odpověď operace zůstává v mezipaměti hello hello. Hello výchozí hodnota je 3 600 sekund nebo 1 hodina.

Klíče mezipaměti jsou použité toodifferentiate mezi odpovědí tak, aby odpovědi hello odpovídající klíč tooeach různé mezipaměti získáte vlastní samostatné hodnotu uloženou v mezipaměti. Volitelně můžete zadat parametrů řetězce dotazu konkrétní nebo toobe hlavičky protokolu HTTP používá v oblasti výpočetních hodnoty klíče mezipaměti v hello **podle parametrů řetězce dotazu** a **podle hlaviček** textová pole v uvedeném pořadí. Pokud žádný není zadaný, úplná požadavku na adresu URL a hello následující hodnoty hlavičky protokolu HTTP se používají v generování klíče mezipaměti: **přijmout** a **Accept-Charset**.

> Další informace o ukládání do mezipaměti a zásady ukládání do mezipaměti najdete v tématu [jak Azure API Management výsledkem operace toocache][How toocache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Parametry žádosti
Parametry operace se spravují na kartě Parametry hello. Parametry zadané v hello **adresa URL šablony** na hello **podpis** se přidávají automaticky a lze změnit pouze úpravou šablony hello adresy URL. Další parametry lze zadat ručně.

Klikněte na tlačítko tooadd nový parametr dotazu, **přidat parametr dotazu** a zadejte hello následující informace:

* **Název** -název parametru.
* **Popis** – stručný popis parametru hello (volitelné).
* **Typ** – typ parametru, vybraný v hello rozevíracího seznamu.
* **Hodnoty** -hodnoty, které lze přiřadit parametr toothis. Jedna z hodnot hello může být označen jako výchozí (volitelné).
* **Požadované** -nastavit povinný parametr hello zaškrtnutím políčka hello. 

![Parametry žádosti][api-management-request-parameters]

## <a name="request-body"></a>Text žádosti
Pokud operace hello umožňuje (např. PUT, POST) a vyžaduje text může uveďte příklad ji ve všech hello podporované formáty reprezentace (například json, XML). 

> Hello textu požadavku se používá pro dokumentaci pouze pro účely a není ověřen.
> 
> 

tooenter obsah žádosti, přepínač toohello **textu** kartě.

Klikněte na tlačítko **přidat reprezentace**, začněte psát název požadovaného typu obsahu (například application/json), vyberte ho v hello rozevíracího seznamu a vložení hello potřeby příklad text žádosti ve vybraném formátu hello do textového pole pro hello. 

![Text žádosti][api-management-request-body]

V další toorepresentations, můžete také zadat volitelné textový popis v hello **popis** textové pole.

## <a name="responses"></a>Odpovědí
Je dobrým zvykem tooprovide, příkladem odpovědi na všechny stavové kódy, které může vytvořit hello operaci. Každý kód stavu může mít více než jeden příklad text odpovědi, jeden pro každou hello podporované typy obsahu. 

Klikněte na tlačítko tooadd odpověď, **přidat** a začněte psát hello potřeby stavový kód. V tomto příkladu hello stavu kód je **200 OK**. Jakmile hello kódu se zobrazí v hello rozevíracího seznamu, vyberte ho a kód odpovědi hello je vytvořený a přidané tooyour operace.

![Kód odpovědi][api-management-response-code]

Klikněte na tlačítko **přidat reprezentace**, začněte psát název hello požadovaný typ obsahu (například application/json) a pak vyberte ho v hello rozevírací nabídku.

![Typ obsahu textu][api-management-response-body-content-type]

Vložte příklad textu odpovědi hello ve vybraném formátu hello do textového pole pro hello. 

![Text odpovědi][api-management-response-body]

V případě potřeby přidejte volitelný popis do hello **popis** textové pole.

Po nakonfigurování hello operace klikněte na tlačítko **Uložit**.

## <a name="next-steps"></a>Další kroky
Po hello operace jsou přidány tooan rozhraní API, hello dalším krokem je tooassociate hello rozhraní API s produktem a publikujete ji tak, aby vývojáři můžou volat jeho operace.

* [Jak toocreate a publikování produktu][How toocreate and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
