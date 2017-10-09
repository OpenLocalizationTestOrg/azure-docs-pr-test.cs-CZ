---
title: "aaaProtect rozhraní API pomocí Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooprotect vaše rozhraní API pomocí zásad kvót a omezování zásady (omezení rychlosti)."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3113fd277d434da0c051b8b90fd629a102bf4867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Ochrana rozhraní API omezením četnosti pomocí Azure API Management
Tento průvodce vám ukáže, jak je snadné tooadd ochranu pro váš back-end rozhraní API podle konfigurace zásad kvót a omezování míra s Azure API Management.

V tomto kurzu vytvoříte "Bezplatnou zkušební verzi" rozhraní API produktu, který umožňuje vývojářům toomake too10 volání za minutu a až tooa maximálně 200 volání za týden tooyour rozhraní API pomocí hello [omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [ Nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) zásady. Potom publikovat hello rozhraní API a testování omezení četnosti hello.

Pro pokročilejší scénáře omezování pomocí hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady, najdete v části [pokročilé omezování požadavků pomocí Azure API Management](api-management-sample-flexible-throttling.md).

## <a name="create-product"></a>toocreate produktu
V tomto kroku vytvoříte bezplatnou zkušební verzi produktu, který nevyžaduje schválení předplatného.

> [!NOTE]
> Pokud už máte produkt nakonfigurovaný a chcete toouse ho v tomto kurzu, můžete přeskočit příliš[Konfigurace četnosti zásad kvót a omezování] [ Configure call rate limit and quota policies] a postupujte podle pokynů hello kurzu odtamtud se svým produktem místo hello bezplatné zkušební verze produktu.
> 
> 

tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Správa vašeho prvního rozhraní API v Azure API Management] [ Manage your first API in Azure API Management] kurzu.
> 
> 

Klikněte na tlačítko **produkty** v hello **API Management** nabídky na levém toodisplay hello hello **produkty** stránky.

![Přidání produktu][api-management-add-product]

Klikněte na tlačítko **přidat produkt** toodisplay hello **přidání nového produktu** dialogové okno.

![Přidání nového produktu][api-management-new-product-window]

V hello **název** zadejte **bezplatné zkušební verze**.

V hello **popis** pole, typ hello následující text: **Odběratelé, kteří budou mít toorun 10 volání za minutu až tooa maximálně 200 volání za týden. potom bude přístup odepřen.**

Produkty ve službě API Management můžou být chráněné nebo otevřené. Chráněných produktů musí být odebírané toobefore, které mohou být použity. Otevřené produkty můžete používat bez předplatného. Ujistěte se, že **vyžadovat předplatné** je vybrané toocreate chráněný produkt, který vyžaduje předplatné. Toto je výchozí nastavení hello.

Pokud chcete tooreview správce a přijměte nebo odmítněte předplatné pokusí toothis produktu, vyberte **vyžadovat schválení předplatného**. Pokud není zaškrtnuté políčko hello, pokusy o předplatné bude schvalovat automaticky. V tomto příkladu předplatné schvaluje automaticky, takže nezaškrtávejte políčko hello.

tooallow vývojáře účty toosubscribe několikrát toohello nového produktu, vyberte hello **povolit více souběžných předplatných** zaškrtávací políčko. Tento kurz několik souběžných předplatných nevyužívá, takže políčko nechte nezaškrtnuté.

Po zadání všech hodnot, klikněte na tlačítko **Uložit** toocreate hello produktu.

![Produkt přidán][api-management-product-added]

Ve výchozím nastavení jsou nové produkty viditelné toousers v hello **správci** skupiny. Přidáme tooadd hello **vývojáři** skupiny. Klikněte na tlačítko **bezplatné zkušební verze**a potom klikněte na hello **viditelnost** kartě.

> Skupiny ve službě API Management jsou použité toomanage hello viditelnost toodevelopers produkty. Produkty udělují viditelnost toogroups a vývojáři můžou zobrazovat a odebírat toohello produkty, které jsou viditelné toohello skupiny, do které patří. Další informace najdete v tématu [jak toocreate a používání skupin v Azure API Management][How toocreate and use groups in Azure API Management].
> 
> 

![Přidání skupiny vývojářů][api-management-add-developers-group]

Vyberte hello **vývojáři** zaškrtněte políčko a potom klikněte na **Uložit**.

## <a name="add-api"></a>tooadd rozhraní API toohello produktu
V tomto kroku kurzu hello přidáme hello Echo API toohello nové bezplatné zkušební verze produktu.

> Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které je možné použít tooexperiment s a další informace o službě API Management. Další informace najdete v článku [Správa vašeho prvního rozhraní API ve službě Azure API Management][Manage your first API in Azure API Management].
> 
> 

Klikněte na tlačítko **produkty** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **bezplatné zkušební verze** tooconfigure hello produktu.

![Konfigurace produktu][api-management-configure-product]

Klikněte na tlačítko **přidat rozhraní API tooproduct**.

![Přidání rozhraní API tooproduct][api-management-add-api]

Vyberte **Rozhraní API v programu Echo** a potom klikněte na **Uložit**.

![Přidání rozhraní API v programu Echo][api-management-add-echo-api]

## <a name="policies"></a>tooconfigure četnosti zásad kvót a omezování
Omezení četnosti a kvóty se konfigurují v editoru zásad hello. zásady Hello dva přidáme v tomto kurzu jsou hello [omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) zásady. Tyto zásady se musí použít v oboru produktu hello.

Klikněte na tlačítko **zásady** pod hello **API Management** nabídky na levé straně hello. V hello **produktu** seznamu, klikněte na tlačítko **bezplatné zkušební verze**.

![Zásady produktu][api-management-product-policy]

Klikněte na tlačítko **přidat zásadu** tooimport hello šablony zásad a začněte vytvářet hello zásadami kvót a omezování rychlost.

![Přidání zásad][api-management-add-policy]

Míra zásadami kvót a omezování jsou příchozími zásadami, tak kurzor hello pozici v hello příchozího prvku.

![Editor zásad][api-management-policy-editor-inbound]

Posuňte hello seznam zásad a najděte hello **omezení četnosti volání podle předplatného** zásadu.

![Příkazy zásad][api-management-limit-policies]

Po hello kurzor je nastavený v hello **příchozí** zásad klikněte na šipku hello vedle položky **omezení četnosti volání podle předplatného** tooinsert jeho šablonu zásad.

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

Jak je vidět z hello fragment hello zásada umožňuje nastavení omezení pro rozhraní API a operace hello produktu. V tomto kurzu jsme nebude používat tuto funkci, proto můžete odstranit hello **rozhraní api** a **operace** elementy z hello **limit rychlosti** elementu, tak, aby se pouze hello vnější **limit rychlosti** element zůstává, jak ukazuje následující příklad hello.

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

V hello bezplatnou zkušební verzi produktu, hello maximální povolená Četnost volání 10 volání za minutu, proto **10** hello hodnotu hello **volání** atribut a **60** pro hello **doby obnovení** atribut.

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

tooconfigure hello **nastavení kvóty využití podle předplatného** zásady, pozice kurzor bezprostředně pod hello nově přidaná **limit rychlosti** v rámci hello **příchozí** element a poté vyhledejte a klikněte na tlačítko hello šipku toohello nalevo od **nastavení kvóty využití podle předplatného**.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

Podobně toohello **nastavení kvóty využití podle předplatného** zásady, **nastavení kvóty využití podle předplatného** umožňuje zásady CAP k vzdálené ploše pro nastavení na rozhraní API a operace hello produktu. V tomto kurzu jsme nebude používat tuto funkci, proto můžete odstranit hello **rozhraní api** a **operace** elementy z hello **kvóty** elementu, jak ukazuje následující příklad hello.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

Kvóty můžou být založené na hello počet volání za interval, šířky pásma nebo obou. V tomto kurzu Neprovádíme omezování na základě šířky pásma, proto můžete odstranit hello **šířky pásma** atribut.

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

V hello bezplatnou zkušební verzi produktu hello kvóta hodnotu 200 volání za týden. Zadejte **200** hello hodnotu hello **volání** atributů a potom zadejte **604800** hello hodnotu hello **doby obnovení** atribut.

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> Intervaly zásad se zadávají v sekundách. toocalculate hello interval pro týden, můžete násobení hello počet dní (7) hello počtem hodin za den (24) hello počtem minut za hodinu (60) a hello počtem sekund za minutu (60): 7 * 24 * 60 * 60 = 604800.
> 
> 

Po dokončení konfigurace zásad hello shodovat hello následující ukázka.

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

Po hello potřeby, jsou zásady nakonfigurované, klikněte na tlačítko **Uložit**.

![Uložení zásad][api-management-policy-save]

## <a name="publish-product"></a> toopublish hello produktu
Teď, když hello hello přidání rozhraní API, a jsou nakonfigurovány hello zásady, třeba hello produkt publikovat, aby se může použít vývojáři. Klikněte na tlačítko **produkty** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **bezplatné zkušební verze** tooconfigure hello produktu.

![Konfigurace produktu][api-management-configure-product]

Klikněte na tlačítko **publikovat**a potom klikněte na **Ano, publikovat** tooconfirm.

![Publikování produktu][api-management-publish-product]

## <a name="subscribe-account"></a>toosubscribe produktu toohello účtu vývojáře
Teď je publikována hello produktu, je k dispozici toobe odběru tooand používají vývojáři.

> Správci instance API Management jsou automaticky odebírané tooevery produktu. V tomto kroku kurzu jsme odběru jeden hello vývojáře bez oprávnění správce účtů toohello bezplatné zkušební verze produktu. Pokud vývojářský účet součástí role správců hello, potom můžete provést tímto krokem projít i v případě, že jste přihlášeni.
> 
> 

Klikněte na tlačítko **uživatelé** na hello **API Management** nabídky na hello left a pak klikněte na název vývojářského účtu hello. V tomto příkladu používáme hello **Clayton Gragg** vývojářský účet.

![Konfigurace vývojáře][api-management-configure-developer]

Klikněte na **Přidat předplatné**.

![Přidat předplatné][api-management-add-subscription-menu]

Vyberte **Bezplatná zkušební verze** a potom klikněte na **Přihlásit k odběru**.

![Přidat předplatné][api-management-add-subscription]

> [!NOTE]
> V tomto kurzu více souběžných předplatných nejsou povolené pro hello bezplatné zkušební verze produktu. Pokud byly, by byl výzvami tooname hello předplatného, jak ukazuje následující příklad hello.
> 
> 

![Přidat předplatné][api-management-add-subscription-multiple]

Po kliknutí na **přihlásit k odběru**, hello produkt zobrazí v hello **předplatné** seznamu pro uživatele hello.

![Předplatné přidáno][api-management-subscription-added]

## <a name="test-rate-limit"></a>toocall operace a testování omezení četnosti hello
Teď, když hello bezplatnou zkušební verzi produktu nakonfigurovanou a publikovanou, jsme volat operace a testování omezení četnosti hello.
Portál pro vývojáře toohello přepínač kliknutím **portál pro vývojáře** v pravé horní nabídce hello.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** v hello horní nabídce a potom klikněte na **Echo API**.

![Portál pro vývojáře][api-management-developer-portal-api-menu]

Klikněte na **GET Resource** a potom klikněte na **Zkouška**.

![Otevření konzoly][api-management-open-console]

Ponechat hello výchozí hodnoty parametrů a pak vyberte svůj klíč předplatného pro bezplatnou zkušební verzi produktu hello.

![Klíč předplatného][api-management-select-key]

> [!NOTE]
> Pokud máte více předplatných, že klíč hello tooselect pro být **bezplatné zkušební verze**, nebo jiný hello zásady, které byly nakonfigurované v předchozích krocích hello nebudou platit.
> 
> 

Klikněte na tlačítko **odeslat**a potom si prohlédněte hello odpovědi. Poznámka: hello **stav odpovědi** z **200 OK**.

![Výsledky operace][api-management-http-get-results]

Klikněte na tlačítko **odeslat** rychlostí vyšší než hello dovolují zásady omezení 10 volání za minutu. Po překročení zásad omezení četnosti hello stav odezvy **429 – příliš mnoho požadavků** je vrácen.

![Výsledky operace][api-management-http-get-429]

Hello **obsah odpovědi** označuje hello zbývající interval předtím, než bude opakování úspěšné.

Pokud platí zásady omezení četnosti hello 10 volání za minutu, následná volání nebudou úspěšná, dokud neuplyne 60 sekund od hello první hello 10 úspěšných volání toohello produktu před překročením omezení četnosti hello. V tomto příkladu je hello zbývající intervalu 54 sekund.

## <a name="next-steps"></a>Další kroky
* Podívejte se na ukázku nastavení kvót a omezení četnosti v hello následující videa.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How toocreate and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers tooa product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API toohello product]: #add-api
[Publish hello product]: #publish-product
[Subscribe a developer account toohello product]: #subscribe-account
[Call an operation and test hello rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
