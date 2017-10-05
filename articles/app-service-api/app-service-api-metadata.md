---
title: "Metadata App Service API Apps pro rozhraní API zjišťování a generování kódu | Microsoft Docs"
description: "Zjistěte, jak aplikace API v Azure App Service pomocí Swagger metadata ke zjednodušení API zjišťování a generování kódu."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: c7f8e33a-61cc-486f-89df-4a97dc3c71d4
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: alkarche
ms.openlocfilehash: 800bb9df9b957bec2c80abb3edefbaf354b549ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Metadata App Service API Apps pro rozhraní API zjišťování a generování kódu
Podpora pro [Swagger 2.0](http://swagger.io/) metadat rozhraní API je integrovaná do App Service API Apps. Nemusíte používat Swagger, ale pokud použijete, můžete využít výhod funkcí aplikace API, které usnadnění zjišťování a využití.   

## <a name="swagger-endpoint"></a>Koncový bod swagger
Zadaný koncový bod, který poskytuje metadata JSON pro Swagger 2.0 pro aplikaci API ve vlastnosti aplikace API. Koncový bod může být relativní vzhledem k základní adresu URL aplikace API nebo absolutní adresu URL. Absolutní adresy URL můžete bodu mimo aplikaci API. 

Mnoho podřízené klientů (například vytváření kódu v sadě Visual Studio a toku PowerApps "Přidat rozhraní API"), adresa URL musí být veřejně přístupná (není chráněn uživatele nebo ověřování služby). To znamená, že pokud používáte ověřování aplikace služby a chcete vystavit definice rozhraní API z v rámci vaší aplikace, budete muset použít možnost ověřování, které umožňuje anonymní přenos k dosažení vaše rozhraní API. Další informace najdete v tématu [ověřování a autorizace pro App Service API Apps](app-service-api-authentication.md).

### <a name="portal-blade"></a>Okno portálu
V [portál Azure](https://portal.azure.com/) koncový bod adresy URL můžete zobrazit a změnit na **definice rozhraní API** okno.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Vlastnost Azure Resource Manager
Adresa URL definice rozhraní API pro aplikace API můžete nakonfigurovat také pomocí [Průzkumníka prostředků](https://resources.azure.com/) nebo [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) v nástrojích příkazového řádku, jako [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs)a [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). 

V **Průzkumníka prostředků**, přejděte na **odběry > {vaše předplatné} > Skupinyprostředků > {vaší skupiny prostředků} > poskytovatelé > Microsoft.Web > lokality > {váš web} > Konfigurace > webové** , a zobrazí se `apiDefinition` vlastnost:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Příklad šablony Azure Resource Manager, která nastaví `apiDefinition` vlastnost, otevřete [souboru azuredeploy.json v ukázkové aplikaci seznamu úkolů](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Najděte část šablony, která vypadá jako ukázka JSON uvedené výše.

### <a name="default-value"></a>Výchozí hodnota
Pokud používáte Visual Studio k vytvoření aplikace API, koncový bod definice rozhraní API se automaticky nastaví na základní adresu URL aplikace API plus `/swagger/docs/v1`. Toto je výchozí adresa URL, [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) používá balíček NuGet k obsluze dynamicky generovaném Swagger metadata pro projekt webového rozhraní API ASP.NET. 

## <a name="code-generation"></a>Vytváření kódu
Jednou z výhod integrace Swagger do aplikace Azure API je automatické generování kódu. Vygenerované třídy klienta usnadňují psaní kódu, který volá aplikaci API.

Kód klienta pro aplikaci API můžete vygenerovat pomocí sady Visual Studio nebo z příkazového řádku. Informace o tom, jak vygenerovat klienta třídy v sadě Visual Studio pro projekt webového rozhraní API ASP.NET najdete v tématu [Začínáme s API Apps a technologií ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informace o tom, jak udělat z příkazového řádku pro všechny podporované jazyky, najdete v souboru readme [Azure/autorest](https://github.com/azure/autorest) na webu GitHub.com.

## <a name="next-steps"></a>Další kroky
Podrobný kurz, který provede vás procesem vytvoření, nasazení a použití aplikace API, najdete v části [Začínáme s API Apps v Azure App Service](app-service-api-dotnet-get-started.md).

Pokud používáte aplikace API Azure API Management, můžete importovat rozhraní API do rozhraní API správy Swagger metadata. Další informace najdete v tématu [jak importovat definici rozhraní API s operacemi v Azure API Management](../api-management/api-management-howto-import-api.md). 

