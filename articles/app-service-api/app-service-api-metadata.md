---
title: "metadata aaaApp Service API Apps pro zjišťování a kód generování rozhraní API | Microsoft Docs"
description: "Zjistěte, jak aplikace API v Azure App Service pomocí generování kódu a zjišťování metadat toofacilitate rozhraní API Swaggeru."
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
ms.openlocfilehash: b27e70b7dd6bd97f5b0b490b496320befe7442c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Metadata App Service API Apps pro rozhraní API zjišťování a generování kódu
Podpora pro [Swagger 2.0](http://swagger.io/) metadat rozhraní API je integrovaná do App Service API Apps. Nemáte toouse Swagger, ale pokud použijete, můžete využít výhod funkcí aplikace API, které usnadnění zjišťování a využití.   

## <a name="swagger-endpoint"></a>Koncový bod swagger
Zadaný koncový bod, který poskytuje metadata JSON pro Swagger 2.0 pro aplikaci API ve vlastnosti aplikace API hello. koncový bod Hello může být relativní toohello základní adresu URL aplikace API hello nebo absolutní adresu URL. Absolutní adresy URL můžete bodu mimo aplikaci API hello. 

Mnoho podřízené klientů (například vytváření kódu v sadě Visual Studio a toku PowerApps "Přidat rozhraní API"), hello adresa URL musí být veřejně přístupná (není chráněn uživatele nebo ověřování služby). To znamená, že pokud používáte ověřování aplikace služby a chcete tooexpose hello definice rozhraní API z v rámci vaší aplikace, je třeba možnost toouse ověřování, která umožňuje anonymní provoz tooreach vaše rozhraní API. Další informace najdete v tématu [ověřování a autorizace pro App Service API Apps](app-service-api-authentication.md).

### <a name="portal-blade"></a>Okno portálu
V hello [portál Azure](https://portal.azure.com/) hello koncový bod adresy URL můžete zobrazit a změnit na hello **definice rozhraní API** okno.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Vlastnost Azure Resource Manager
Adresa URL definice hello rozhraní API pro aplikace API můžete nakonfigurovat také pomocí [Průzkumníka prostředků](https://resources.azure.com/) nebo [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) v nástrojích příkazového řádku, jako [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs)a hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). 

V **Průzkumníka prostředků**, přejděte příliš**odběry > {vaše předplatné} > Skupinyprostředků > {vaší skupiny prostředků} > poskytovatelé > Microsoft.Web > lokality > {váš web} > Konfigurace > webové**, a uvidíte hello `apiDefinition` vlastnost:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Příklad šablony Azure Resource Manager, která nastaví hello `apiDefinition` vlastnost, otevřete hello [souboru azuredeploy.json v hello ukázkovou aplikací seznamu úkolů](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Najít oddíl hello hello šablony, která vypadá jako ukázka JSON hello uvedené výše.

### <a name="default-value"></a>Výchozí hodnota
Pokud používáte Visual Studio toocreate aplikace API, definice endpoint hello rozhraní API se automaticky nastaví toohello základní adresu URL aplikace hello API plus `/swagger/docs/v1`. Toto je výchozí URL hello této hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) balíček NuGet pro projekt webového rozhraní API ASP.NET používá tooserve dynamicky generovaná metadata Swagger. 

## <a name="code-generation"></a>Vytváření kódu
Jednou z výhod hello integraci Swagger do aplikace Azure API je automatické generování kódu. Vygenerované třídy klienta umožňují snadnější toowrite kód, který volá aplikaci API.

Kód klienta pro aplikaci API můžete vygenerovat pomocí sady Visual Studio nebo z příkazového řádku hello. Informace o způsobu toogenerate klienta třídy v sadě Visual Studio pro projekt webového rozhraní API ASP.NET najdete v tématu [Začínáme s API Apps a technologií ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informace o tom, jak toodo z hello příkazového řádku pro všechny podporované jazyky, najdete v souboru readme hello hello [Azure/autorest](https://github.com/azure/autorest) na webu GitHub.com.

## <a name="next-steps"></a>Další kroky
Podrobný kurz, který provede vás procesem vytvoření, nasazení a použití aplikace API, najdete v části [Začínáme s API Apps v Azure App Service](app-service-api-dotnet-get-started.md).

Pokud používáte aplikace API Azure API Management, můžete Swagger metadata tooimport rozhraní API do rozhraní API správy. Další informace najdete v tématu [jak tooimport hello definice rozhraní API s operacemi v Azure API Management](../api-management/api-management-howto-import-api.md). 

