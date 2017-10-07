---
title: "aaaImport rozhraní API do Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooimport rozhraní API a jeho operací do Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 20fbbb53243aecc24d72833ec0904ae8fab97863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-hello-definition-of-an-api-with-operations-in-azure-api-management"></a>Jak tooimport hello definice rozhraní API s operacemi v Azure API Management
Ve službě API Management vytvořením nových rozhraní API a operace hello přidat ručně nebo hello rozhraní API může být importován společně s hello operace v jednom kroku.

Rozhraní API a jejich operace lze importovat pomocí hello následující formáty.

* WADL
* Swagger

Tato příručka ukazuje, jak vytvořit nové rozhraní API a importovat jeho operací v jednom kroku. Informace o ruční vytvoření rozhraní API a přidávání operací najdete v tématu [jak toocreate rozhraní API] [ How toocreate APIs] a [jak tooan tooadd operace rozhraní API] [ How tooadd operations tooan API].

## <a name="import-api"></a>Import rozhraní API
Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele hello. Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.

![Portál vydavatele][api-management-management-console]

Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **importovat rozhraní API**.

![Rozhraní API pro import][api-management-import-apis]

Hello **rozhraní API pro Import** okno má tři karty, které odpovídají toohello tři způsoby tooprovide hello API specifikace.

* **Ze schránky** umožňuje specifikaci toopaste hello rozhraní API do hello určené textového pole.
* **Ze souboru** vám umožní toobrowse tooand hello vyberte soubor, který obsahuje specifikaci hello rozhraní API.
* **Z adresy URL** vám umožní toosupply hello URL toohello specifikace hello rozhraní API.

![Formát importovat rozhraní API][api-management-import-api-clipboard]

Po zadání specifikace hello rozhraní API, použijte hello rádiová tlačítka na hello správné tooindicate hello specifikace formátu. jsou podporovány následující formáty Hello.

* WADL
* Swagger

Potom zadejte **přípona adresy URL webového rozhraní API**. Toto je základní adresu URL připojení toohello služby API management. základní adresu URL Hello je společná pro všechna rozhraní API hostované na každou instanci služby API Management. API Management odlišuje rozhraní API podle jejich přípona a proto hello přípona musí být jedinečný pro každé rozhraní API na konkrétní instance služby API management.

Po zadání všech hodnot, klikněte na **Uložit** toocreate hello rozhraní API a hello související operace. 

> [!NOTE]
> Kurz import základní kalkulačky rozhraní API ve formátu Swagger, najdete v části [Správa vašeho prvního rozhraní API v Azure API Management](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Export rozhraní API
V tooimporting přidání nových rozhraní API, můžete exportovat hello Definice vaše rozhraní API z portálu vydavatele hello. toodo proto, klikněte na **Export rozhraní API** z hello **kartu Souhrn** z vaší **rozhraní API**.

![Export rozhraní API][api-management-export-api]

Rozhraní API je možné exportovat pomocí WADL nebo Swagger. Vyberte požadovaný formát hello, klikněte na tlačítko **Uložit**a vyberte umístění hello v souboru které toosave hello.

![Formát exportu rozhraní API][api-management-export-api-format]

## <a name="next-steps"></a>Další kroky
Po vytvoření rozhraní API a operace hello importovat, můžete zkontrolovat a nakonfigurujte jakákoli další nastavení, přidejte hello rozhraní API tooa produktu a publikovat, aby bylo k dispozici pro vývojáře. Další informace najdete v tématu hello následující příručky.

* [Jak nastavení tooconfigure rozhraní API][How tooconfigure API settings]
* [Jak toocreate a publikování produktu][How toocreate and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate APIs]: api-management-howto-create-apis.md
[How tooconfigure API settings]: api-management-howto-create-apis.md#configure-api-settings
