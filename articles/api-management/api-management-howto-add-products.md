---
title: "aaaHow toocreate a publikování produktu v Azure API Management"
description: "Zjistěte, jak toocreate a publikovat produkty ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f0a37f08b4e29ca68be9caec4c7604e3b4b6aaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-publish-a-product-in-azure-api-management"></a>Jak toocreate a publikování produktu v Azure API Management
Ve službě Azure API Management produkt obsahuje jeden nebo více rozhraní API a také využití kvóty a hello podmínky použití. Po publikování produktu vývojáři můžou přihlášení k odběru produktu toohello a začít toouse hello rozhraní API. Hello téma obsahuje Průvodce toocreating produkt, přidání rozhraní API a publikování pro vývojáře.

## <a name="create-product"></a>Vytvoření produktu
Operace se přidají a nakonfigurovat tooan rozhraní API portálu vydavatele hello. Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Klikněte na **produkty** v hello nabídky na levém toodisplay hello hello **produkty** a klikněte na tlačítko **přidat produkt**.

![Produkty][api-management-products]

![Nového produktu][api-management-add-new-product]

Zadejte popisný název produktu hello hello **název** pole a popis hello produktu v hello **popis** pole.

Produkty v API Management můžou být **otevřete** nebo **chráněné**. Chráněných produktů musí být odebírané toobefore použitím, otevřené produkty můžete používat bez předplatného. Zkontrolujte **vyžadovat předplatné** toocreate chráněný produkt, který vyžaduje předplatné. Toto je výchozí nastavení hello.

Zkontrolujte **vyžadovat schválení předplatného** Pokud chcete tooreview správce a přijměte nebo odmítněte předplatné pokusí toothis produktu. Pokud není zaškrtnuto políčko hello, pokusy o předplatné bude schvalovat automaticky. Další informace o předplatných najdete v tématu [zobrazení odběratele tooa produktu][View subscribers tooa product].

tooallow vývojáře účty toosubscribe produkt toohello vícekrát, zkontrolujte hello **povolit více předplatných** zaškrtávací políčko. Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit pouze jednou toohello produktu.

![Neomezená více předplatných][api-management-unlimited-multiple-subscriptions]

toolimit hello počet více souběžných předplatných, zkontrolujte hello **omezit počet souběžných odběrů** zaškrtněte políčko a zadejte limitu předplatného pro hello. V následujícím příkladu hello souběžných předplatných jsou omezené toofour za vývojářský účet.

![Čtyři více předplatných][api-management-four-multiple-subscriptions]

Po nakonfigurování všech možností nového produktu, klikněte na tlačítko **Uložit** toocreate hello nového produktu.

![Produkty][api-management-products-page]

> Ve výchozím nastavení jsou nové produkty jsou publikování a jsou viditelné pouze toohello **správci** skupiny.
> 
> 

tooconfigure produkt, klikněte na název produktu hello v hello **produkty** kartě.

## <a name="add-apis"></a>Přidat rozhraní API tooa produktu
Hello **produkty** stránka obsahuje čtyři odkazy pro konfiguraci: **Souhrn**, **nastavení**, **viditelnost**, a  **Odběratelé, kteří**. Hello **Souhrn** karta je, kde můžete přidat rozhraní API a provést nebo zrušit produktu.

![Souhrn][api-management-new-product-summary]

Před publikováním svůj produkt musíte tooadd jeden nebo více rozhraní API. toodo tento, klikněte na tlačítko **přidat rozhraní API tooproduct**.

![Přidání rozhraní API][api-management-add-apis-to-product]

Vyberte hello potřeby rozhraní API a klikněte na tlačítko **Uložit**.

## <a name="add-description"></a>Přidání popisné informace tooa produktu
Hello **nastavení** karta vám umožní tooprovide podrobné informace o produktu hello například svůj účel, hello poskytuje přístup k rozhraní API a další užitečné informace. obsah Hello je cílena na vývojáře hello, které bude volání metody hello rozhraní API a může být napsán v jako prostý text ani značka jazyka HTML.

![Nastavení produktu][api-management-product-settings]

Zkontrolujte **vyžadovat předplatné** toocreate hello chráněný produkt, který vyžaduje předplatné toobe používá, nebo zrušte zaškrtnutí políčka toocreate otevřete produktu, kterou lze volat bez předplatného.

Vyberte **vyžadovat schválení předplatného** Chcete-li toomanually schválení všech žádostí o odběr produktu. Ve výchozím nastavení jsou automaticky udělí všechny odběry produktu.

tooallow vývojáře účty toosubscribe produkt toohello vícekrát, zkontrolujte hello **povolit více předplatných** zaškrtněte políčko a volitelně určete omezení. Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit pouze jednou toohello produktu.

Volitelně můžete vyplnit hello **podmínky použití** pole s popisem hello podmínky použití hello produktu, kterou odběratele musí přijmout v pořadí toouse hello produktu.

## <a name="publish-product"></a>Publikování produktu
Předtím, než je možné volat hello rozhraní API v produktu, musí být publikován hello produktu. Na hello **Souhrn** hello produktu, klikněte na **publikovat**a potom klikněte na **Ano, publikovat** tooconfirm. Klikněte na tlačítko toomake privátního publikované dříve produktu, **zrušit publikování**.

![Publikování produktu][api-management-publish-product]

## <a name="make-visible"></a>Zkontrolujte viditelné toodevelopers produktu
Hello **viditelnost** karta vám umožní toochoose rolí, které jsou možné toosee hello produktu na portál pro vývojáře hello a přihlášení k odběru produktu toohello.

![Přehled produktu][api-management-product-visiblity]

tooenable nebo vypnutí viditelnosti produktů pro vývojáře hello ve skupině, nebo zrušte zaškrtnutí políčka hello zaškrtněte políčko vedle skupiny hello a pak klikněte na **Uložit**.

> Další informace najdete v tématu [jak toocreate a používání skupin toomanage vývojáře účty v Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Zobrazení odběratele tooa produktu
Hello **Odběratelé, kteří** karta Vypíše seznam hello vývojáři, kteří odebírají toohello produktu. Hello podrobnosti a nastavení pro každý vývojář lze zobrazit kliknutím na název pro vývojáře hello. V tomto příkladu jste žádné vývojáři ještě předplatné toohello produktu.

![Vývojáři][api-management-developer-list]

## <a name="next-steps"></a>Další kroky
Jednou hello požadované přidání rozhraní API, a publikovali hello produktu, vývojáři můžou přihlášení k odběru produktu toohello a začít toocall hello rozhraní API. Kurz, který ukazuje, tyto položky, jakož i konfigurace pokročilé produktu najdete v části [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management][How create and configure advanced product settings in Azure API Management].

Další informace o práci s produkty najdete v následující video hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs tooa product]: #add-apis
[Add descriptive information tooa product]: #add-description
[Publish a product]: #publish-product
[Make a product visible toodevelopers]: #make-visible
[View subscribers tooa product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How toocreate and use groups toomanage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
