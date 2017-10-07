---
title: "aaaHow toocreate rozhraní API v Azure API Management"
description: "Zjistěte, jak toocreate a konfigurace rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 48ed8d93947253aa1e67ad995927ed6101cac072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-apis-in-azure-api-management"></a>Jak toocreate rozhraní API v Azure API Management
Rozhraní API ve službě API Management představuje sadu operací, které můžete vyvolat klientské aplikace. Vytvoří se nová rozhraní API portálu vydavatele hello a pak hello potřeby, že operace jsou přidány. Jakmile hello operace jsou přidány, hello rozhraní API je přidána tooa produktu a může být publikována. Po publikování rozhraní API může být odebírané tooand používají vývojáři.

Tato příručka obsahuje hello prvním krokem v procesu hello: jak toocreate a nakonfigurujte nové rozhraní API ve službě API Management. Další informace o přidávání operací a publikování produktu najdete v tématu [jak tooan tooadd operace rozhraní API] [ How tooadd operations tooan API] a [jak toocreate a publikování produktu] [ How toocreate and publish a product].

## <a name="create-new-api"></a>Vytvořit nové rozhraní API
Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele hello. Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidání rozhraní API**.

![Vytvoření rozhraní API][api-management-create-api]

Použití hello **přidání nového rozhraní API** okno tooconfigure hello nové rozhraní API.

![Přidání nového rozhraní API][api-management-add-new-api]

Hello následující pole jsou použité tooconfigure hello nové rozhraní API.

* **Název webové rozhraní API** poskytuje jedinečný a popisný název pro rozhraní API hello. Zobrazí se na vývojáře a vydavatele portálech hello.
* **Adresu URL webové služby** odkazy hello služba HTTP implementace rozhraní API hello. Rozhraní API správy předává požadavky toothis adresu.
* **Přípona adresy URL rozhraní API webové** je připojením toohello základní adresu URL pro službu správy hello rozhraní API. základní adresu URL Hello je společná pro všechna rozhraní API hostované instanci služby API Management. API Management odlišuje rozhraní API podle jejich přípona a proto hello přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele.
* **Schéma adresy URL rozhraní API webové** určuje protokoly, které lze použít tooaccess hello API. Ve výchozím nastavení je zadán HTTPs.
* toooptionally přidat tento nový tooa produkt rozhraní API, klikněte na tlačítko hello **produkty (volitelné)** rozevíracího seznamu a vyberte produkt. Tento krok může být opakovaný vícekrát tooadd hello rozhraní API toomultiple produkty.

Jakmile hello potřeby hodnoty jsou nakonfigurované, klikněte na možnost **Uložit**. Po vytvoření nového rozhraní API hello hello souhrnná stránka rozhraní API hello se zobrazí na portálu vydavatele hello.

![Souhrn rozhraní API][api-management-api-summary]

## <a name="configure-api-settings"></a>Nastavení konfigurace rozhraní API
Můžete použít hello **nastavení** kartě tooverify a upravit hello konfigurace rozhraní API. **Název webové rozhraní API**, **adresu URL webové služby**, a **přípona adresy URL webového rozhraní API** původně nastaveny při hello rozhraní API je vytvořen a je možné upravit zde. **Popis** poskytuje volitelný popis a **schéma adresy URL webového rozhraní API** určuje protokoly, které lze použít tooaccess hello API.

![Nastavení rozhraní API][api-management-api-settings]

ověřování brány tooconfigure hello back-end služby implementující rozhraní API hello, vyberte hello **zabezpečení** kartě hello **s přihlašovacími údaji** rozevíracího seznamu lze použít tooconfigure **HTTP základní** nebo **klientské certifikáty** ověřování. Základní ověřování toouse HTTP, jednoduše zadejte přihlašovací údaje hello potřeby. Informace o používání ověřování certifikátu klienta najdete v tématu [jak toosecure back endové služby pomocí klientského certifikátu ověřování ve službě Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].

Hello **zabezpečení** karta může být také použít tooconfigure **autorizace uživatelů** pomocí OAuth 2.0. Další informace najdete v tématu [jak účtů tooauthorize vývojáře pomocí OAuth 2.0 ve službě Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].

![Nastavení základního ověřování][api-management-api-settings-credentials]

Klikněte na tlačítko **Uložit** toosave provedené změny toohello nastavení rozhraní API.

## <a name="next-steps"></a>Další kroky
Po vytvoření rozhraní API a hello nastavení, hello další kroky jsou tooadd hello operations toohello rozhraní API, přidejte hello rozhraní API tooa produktu a publikovat, aby bylo k dispozici pro vývojáře. Další informace najdete v tématu hello následující články.

* [Jak tooan tooadd operace rozhraní API][How tooadd operations tooan API]
* [Jak toocreate a publikování produktu][How toocreate and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How toosecure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How tooauthorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
