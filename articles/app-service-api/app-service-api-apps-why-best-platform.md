---
title: "aaaAPI aplikace Úvod | Microsoft Docs"
description: "Zjistěte, jak Azure App Service pomáhá nasazovat, hostovat a používat rozhraní RESTful API."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: alkarche
ms.openlocfilehash: f066e96db667d4685480001bc43c2b1bff4ea352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-apps-overview"></a>Přehled API Apps
API apps v Azure App Service nabídka funkcí, které umožňují snadnější toodevelop hostitelů a používání rozhraní API v hello cloudu a místně. Aplikace API poskytují zabezpečení na úrovni potřebné ve velkých firmách, snadné řízení přístupu, hybridní připojení, automatické generování sad SDK a bezproblémovou integraci s [Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

[Azure App Service](../app-service/app-service-value-prop-what-is.md) je plně spravovaná platforma pro webové, mobilní a integrační scénáře. API Apps jsou jedním ze čtyř typů aplikací, které služba [Azure App Service](../app-service/app-service-value-prop-what-is.md) nabízí.

![Typy aplikací v Azure App Service](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Proč používat API Apps?
Toto jsou některé klíčové funkce API Apps:

* **Použití existujícího rozhraní API jako-je** - nemáte toochange žádné hello kód ve vaší stávající rozhraní API tootake výhod API Apps – jen nasadit aplikace kód tooan rozhraní API. Vaše rozhraní API může používat libovolný jazyk nebo rozhraní podporované službou App Service, včetně technologií ASP.NET, C#, Java, PHP, Node.js a Python.
* **Snadné požívání:** Integrovaná podpora [metadat rozhraní API Swaggeru](http://swagger.io/) umožňuje snadné použití vašich rozhraní API pro nejrůznější klienty.  Automaticky generuje kód klienta pro vaše rozhraní API v různých jazycích, včetně C#, Javy a JavaScriptu. Můžete snadno konfigurovat [CORS](app-service-api-cors-consume-javascript.md), aniž byste měnili kód. Další informace najdete v tématech [Metadata App Service API Apps pro zjišťování rozhraní API a generování kódu](app-service-api-metadata.md) a [Využití aplikace API z JavaScriptu pomocí CORS](app-service-api-cors-consume-javascript.md). 
* **Snadné řízení přístupu** -aplikace API jsou chráněny před neověřeným přístupem kód tooyour žádné změny. Integrované služby ověřování zajišťují přístup k rozhraní API pro jiné služby nebo klienty představující uživatele. Mezi podporované zprostředkovatele identity patří Azure Active Directory, Facebook, Twitter, Google a účet Microsoft. Klienti mohou používat Active Directory Authentication Library (ADAL) nebo hello Mobile Apps SDK. Další informace najdete v tématu [Ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md).
* **Integrace aplikace Visual Studio** – vyhrazené nástroje v sadě Visual Studio zjednodušují práci hello při vytváření, nasazování, využívání, ladění a správě aplikací API. Další informace najdete v tématu [Announcing hello Azure SDK 2.8.1 pro .NET](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).
* **Integrace s Logic Apps:** Aplikace API, které vytvoříte, mohou být využívány službou [App Service Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).  Další informace najdete v tématech [Používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md) a [Nová verze schématu 2015-08-01-preview](../logic-apps/logic-apps-schema-2015-08-01.md).

Kromě toho může aplikace API využívat funkce služeb [Web Apps](../app-service-web/app-service-web-overview.md) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md). Hello zpětné platí také: Pokud používáte webovou aplikaci nebo mobilní aplikace toohost rozhraní API, může využívat funkce aplikací API, například metadata Swagger pro klienta generování kódu a CORS pro přístup k prohlížeči mezi doménami. Hello jenom rozdíl mezi hello tři typy aplikací (API, webové, mobilní) je hello název a ikona pro ně používají na hello portálu Azure.

## <a name="whats-hello-difference-between-api-apps-and-azure-api-management"></a>Co je hello rozdíl mezi aplikace API Azure API Management?
API Apps a [Azure API Management](../api-management/api-management-key-concepts.md) jsou doplňkové služby:

* API Management se týká správy rozhraní API. Umístí front-end API Management toomonitor rozhraní API a omezení využití, zpracování vstupu a výstupu, konsolidace několika rozhraní API do jednoho koncového bodu a tak dále. Hello spravovaná rozhraní API je možné hostovat kdekoli.
* Účelem API Apps je hostování rozhraní API. Hello služba obsahuje funkce, které usnadňují vývoj a využívání rozhraní API, ale neprovádí hello druhy monitorování, omezování, manipulace s nebo konsolidaci, že nemá API Management. Pokud nepotřebujete funkce API Management, můžete rozhraní API hostovat v aplikacích API bez použití služby API Management.

Zde je diagram, který znázorňuje použití služby API Management pro rozhraní API hostovaná v aplikacích API nebo jinde.

![Azure API Management a API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Některé funkce mají služba API Management a funkce API Apps podobné.  Obě služby mohou například automatizovat podporu CORS. Pokud používáte dvě služby hello společně, byste použili API Management pro CORS vzhledem k tomu, že funguje jako hello aplikace front-endu tooyour rozhraní API. 

## <a name="getting-started"></a>Začínáme
tooget pracovat s aplikacemi API nasazením tooone ukázkový kód, najdete v části hello kurz pro vámi preferované rozhraní:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

tooask dotazy týkající se aplikace API, otevřete vlákno ve hello [fóru pro API Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 

