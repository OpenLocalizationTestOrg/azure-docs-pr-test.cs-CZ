---
title: "aaaConfigure oznámení a e-mailových šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooconfigure oznámení a e-mailových šablon ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: dc23289c25a1641992b73cb955099b3f207b6968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-notifications-and-email-templates-in-azure-api-management"></a>Jak tooconfigure oznámení a e-mailových šablon ve službě Azure API Management
Služba API Management nabízí možnost hello tooconfigure oznámení pro konkrétní události a tooconfigure hello e-mailové šablony, které jsou používané toocommunicate s hello správci a vývojáři instance služby API Management. Toto téma ukazuje, jak tooconfigure oznámení pro hello k dispozici události a poskytuje přehled o konfiguraci e-mailových šablon hello používá pro tyto události.

## <a name="publisher-notifications"></a>Konfigurace vydavatele oznámení
tooconfigure oznámení, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management.

![Portál vydavatele][api-management-management-console]

> [!NOTE] 
> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.

Klikněte na tlačítko **oznámení** z hello **API Management** nabídce hello zbývajících tooview hello k dispozici oznámení.

![Vydavatele oznámení][api-management-publisher-notifications]

Následující seznam událostí Hello lze nakonfigurovat pro oznámení.

* **Žádostí o odběr (které vyžadují schválení)** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení o odběru požadavky pro rozhraní API produkty, které vyžadují schválení.
* **Nová předplatná** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení o nových předplatných rozhraní API produktu.
* **Žádosti o aplikace Galerie** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení, když nové aplikace jsou odeslaná toohello galerii aplikací.
* **SKRYTÁ** – hello příjemců zadané e-mailu a uživatelé budou obdrží e-mailu skrytá kopii všech e-maily odesílané toodevelopers.
* **Nový problém nebo komentář** – hello příjemců zadané e-mailu a uživatelé obdrží oznámení e-mailu, když nový problém nebo komentář je odesíláno na portál pro vývojáře hello.
* **Zprávu zavřete účet** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení po uzavření účtu.
* **Blížící limit kvóty předplatného** – hello následující příjemců e-mailu a uživatelé obdrží e-mailová oznámení, když využití předplatné získá zavřít toousage kvóty.

U každé události lze zadat příjemců e-mailu pomocí hello e-mailovou adresu textového pole nebo můžete vybrat uživatele ze seznamu.

toospecify hello e-mailové adresy toobe oznámení, zadejte je do hello e-mailová adresa. Pokud máte více e-mailové adresy, oddělte je čárkami.

![Příjemců oznámení][api-management-email-addresses]

toospecify hello uživatelé toobe oznámení, klikněte na tlačítko **přidat příjemce**, zaškrtněte políčko hello vedle hello uživatelé toobe oznámení a klikněte na **OK**.

> [!NOTE] 
> V seznamu hello jsou zobrazeny pouze správci.


Po dokončení konfigurace hello příjemců oznámení, klikněte na tlačítko **Uložit** tooapply hello aktualizovat příjemců oznámení.

> [!NOTE] 
> Pokud přejdete pryč z hello **vydavatele oznámení** portál vydavatele hello karta upozorní vás, pokud existují neuložené změny.


## <a name="email-templates"></a>Konfigurovat e-mailových šablon
API Management poskytuje e-mailových šablon pro hello e-mailové zprávy, které se odesílají v kurzu hello správě a používání služby hello. Následující e-mailových šablon Hello jsou k dispozici.

* Odeslání aplikace Galerie schválení
* Písmeno farewell vývojáře
* Limit kvóty vývojáře blíží oznámení
* Pozvat uživatele
* Nový komentář přidat tooan problém
* Nový problém přijat
* Nové předplatné aktivované
* Potvrzení odběru obnovit
* Odmítne požadavek na odběr
* Přišel požadavek na předplatné

Tyto šablony se dá změnit podle potřeby.

tooview a nakonfigurovat hello e-mailových šablon pro vaše instance služby API Management, klikněte na tlačítko **oznámení** z hello **API Management** nabídce vlevo hello a vyberte hello **e-mailových šablon**  kartě.

![Šablony e-mailů][api-management-email-templates]

tooview nebo konkrétní šablonu upravit, vyberte ji ze hello **šablony** rozevíracího seznamu.

![Seznam šablon e-mailů][api-management-email-templates-list]

Každý e-mailové šablony má předmět ve formátu prostého textu a definice text ve formátu HTML. Každá položka se dají přizpůsobit podle potřeby.

![Editor šablony e-mailu][api-management-email-template]

Hello **parametry** seznam obsahuje seznam parametrů, které v případě vloženy do hello předmět ani text, bude nahrazené hello určené hodnotu, při odeslání e-mailu hello. tooinsert parametr, umístěte kurzor hello, kde chcete toogo hello parametr a klikněte na tlačítko hello šipku toohello nalevo od názvu parametru hello.

Klikněte na tlačítko **Preview** nebo **odeslat testovací** toosee jak hello e-mailu bude vypadat nebo odeslat testovací e-mail.

> [!NOTE] 
> Hello parametry nejsou nahrazené skutečnými hodnotami při zobrazení náhledu nebo odeslání testu.

toosave hello změny toohello e-mailové šablony, klikněte na tlačítko **Uložit**, nebo klikněte na tlačítko toocancel hello změny **zrušit**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
