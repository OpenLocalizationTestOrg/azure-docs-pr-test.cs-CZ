---
title: "aaaAdd ukládání do mezipaměti tooimprove výkon v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak načíst tooimprove hello latence, šířku pásma a webové služby pro volání služby API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 056ab7cf788218327e30bd5c028b76e3b1977fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-caching-tooimprove-performance-in-azure-api-management"></a>Přidání ukládání do mezipaměti tooimprove výkon v Azure API Management
Operace ve službě API Management můžete nakonfigurovat tak, aby odpovědi ukládaly do mezipaměti. Ukládání odpovědí do mezipaměti může v případě dat, která se často nemění, výrazně zlepšit latenci rozhraní API, využití šířky pásma a načítání webových služeb.

Tento průvodce vám ukáže, jak tooadd odpovědi ukládání do mezipaměti pro vaše rozhraní API a nakonfigurovat zásady pro operace rozhraní Echo API ukázkový hello. Potom můžete volat operace hello z hello vývojáře portálu tooverify ukládání do mezipaměti v akci.

> [!NOTE]
> Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).
> 
> 

## <a name="prerequisites"></a>Požadavky
Předtím, než následující hello kroky v tomto průvodci, musíte mít instanci služby API Management s rozhraním API a produkt nakonfigurovaný. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.

## <a name="configure-caching"></a>Konfigurace operace pro ukládání do mezipaměti
V tomto kroku zkontrolujete nastavení hello mezipaměti hello **GET Resource (cached)** operaci hello ukázkovém rozhraní Echo API.

> [!NOTE]
> Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které je možné použít tooexperiment s a další informace o službě API Management. Další informace najdete v článku [Začínáme se službou Azure API Management][Get started with Azure API Management].
> 
> 

tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management.

![Portál vydavatele][api-management-management-console]

Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **Echo API**.

![Echo API][api-management-echo-api]

Klikněte na tlačítko hello **operace** a pak klikněte hello **GET Resource (cached)** operaci provést z hello **Operations** seznamu.

![Operace rozhraní API v programu Echo][api-management-echo-api-operations]

Klikněte na tlačítko hello **ukládání do mezipaměti** hello tooview karta ukládání do mezipaměti nastavení pro tuto operaci.

![Karta Ukládání do mezipaměti][api-management-caching-tab]

tooenable ukládání do mezipaměti pro operace, vyberte hello **povolit** zaškrtávací políčko. V tomto příkladu je ukládání do mezipaměti povoleno.

Každá odpověď operace se ukládá do klíčů na základě hodnot hello v hello **podle parametrů řetězce dotazu** a **podle hlaviček** pole. Pokud chcete toocache více odpovědí na základě parametrů řetězce dotazu nebo hlavičky, můžete je nakonfigurovat v těchto dvou polích.

**Doba trvání** Určuje dobu vypršení platnosti hello odpovědí hello do mezipaměti. V tomto příkladu je hello interval **3600** sekund, což je ekvivalentní tooone hodina.

Pomocí hello konfigurace v tomto příkladu ukládání do mezipaměti, první požadavek toohello hello **GET Resource (cached)** operace vrátí odpověď z back-end službu hello. Tato odpověď bude do mezipaměti, s klíči hello zadaný hlavičky a dotaz řetězec parametry. Následující volání operace toohello s odpovídající parametry, budou mít hello do mezipaměti odpovědi vrácené, dokud nevyprší platnost hello mezipaměti Doba trvání intervalu.

## <a name="caching-policies"></a>Hello zkontrolujte zásady ukládání do mezipaměti
V tomto kroku zkontrolujte hello ukládání do mezipaměti nastavení pro hello **GET Resource (cached)** operaci hello ukázkovém rozhraní Echo API.

Když jsou nakonfigurované nastavení ukládání do mezipaměti pro operaci v hello **ukládání do mezipaměti** ukládání do mezipaměti pro hello operaci přidány zásady. Tyto zásady můžete zobrazit a upravit v editoru zásad hello.

Klikněte na tlačítko **zásady** z hello **API Management** nabídce hello vlevo a pak vyberte **Echo API / GET Resource (cached)** z hello **operace**rozevíracího seznamu.

![Operace rozsahu zásad][api-management-operation-dropdown]

V editoru zásad hello zobrazí hello zásady pro tuto operaci.

![Editor zásad služby API Management][api-management-policy-editor]

Hello definice zásad této operace obsahuje zásady hello, které definují hello konfigurace ukládání do mezipaměti, které jste zkontrolovali pomocí hello **ukládání do mezipaměti** v předchozím kroku hello.

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
        </cache-lookup>
        <rewrite-uri template="/resource" />
    </inbound>
    <outbound>
        <base />
        <cache-store caching-mode="cache-on" duration="3600" />
    </outbound>
</policies>
```

> [!NOTE]
> Toohello změny provedené v editoru zásad hello zásady ukládání do mezipaměti se projeví na hello **ukládání do mezipaměti** příslušné operace a naopak.
> 
> 

## <a name="test-operation"></a>Volání operace a testování ukládání do mezipaměti hello
toosee hello ukládání do mezipaměti, v akci, jsme hello operaci volat z portálu pro vývojáře hello. Klikněte na tlačítko **portál pro vývojáře** v pravé horní nabídce hello.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** v hello horní nabídce a potom vyberte **Echo API**.

![Echo API][api-management-apis-echo-api]

> Pokud máte pouze jedno rozhraní API nakonfigurovaný nebo viditelné tooyour účet, klikněte na rozhraní API přejdete přímo toohello operací pro toto rozhraní API.
> 
> 

Vyberte hello **GET Resource (cached)** operace a pak klikněte na tlačítko **otevřít konzolu**.

![Otevření konzoly][api-management-open-console]

Hello konzole vám umožní tooinvoke operací přímo z portálu pro vývojáře hello.

![Konzola][api-management-console]

Ponechte hello výchozí hodnoty pro **param1** a **param2**.

Vyberte hello požadovaný klíč hello **klíč předplatného** rozevíracího seznamu. Pokud má váš účet jenom jedno předplatné, bude už klíč vybrán.

Zadejte **sampleheader: value1** v hello **hlavičky požadavku** textové pole.

Klikněte na tlačítko **HTTP Get** a poznamenejte si hello hlavičky odpovědi.

Zadejte **sampleheader: value2** v hello **hlavičky požadavku** textového pole a pak klikněte na tlačítko **HTTP Get**.

Všimněte si, že hodnota hello **sampleheader** stále **value1** v odpovědi hello. Zkuste několik různých hodnot a Všimněte si, že hello odpovědi v mezipaměti z hello první volání se vrátí.

Zadejte **25** do hello **param2** pole a pak klikněte na **HTTP Get**.

Všimněte si, že hodnota hello **sampleheader** hello odpovědi je teď v **value2**. Protože výsledky operace hello jsou klíčovány pomocí řetězce dotazu, předchozí odpověď uložená v mezipaměti hello nebyla vrácena.

## <a name="next-steps"></a>Další kroky
* Další informace o zásadách ukládání do mezipaměti najdete v tématu [zásady ukládání do mezipaměti] [ Caching policies] v hello [zásady API managementu][API Management policy reference].
* Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review hello caching policies]: #caching-policies
[Call an operation and test hello caching]: #test-operation
[Next steps]: #next-steps
