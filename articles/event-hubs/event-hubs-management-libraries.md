---
title: "knihovny správy aaaAzure Event Hubs | Microsoft Docs"
description: "Správa oborů názvů Event Hubs a entity z rozhraní .NET"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b7db0077f6f31397ae46e926c3c28630a157824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-management-libraries"></a>Knihovny správy centra událostí

knihovny správy Hello Event Hubs můžete dynamicky zajišťují Event Hubs obory názvů a entity. To umožňuje komplexní nasazení a scénáře zasílání zpráv, tak, aby prostřednictvím kódu programu můžete určit, jaké tooprovision entity. Tyto knihovny jsou aktuálně dostupné pro rozhraní .NET.

## <a name="supported-functionality"></a>Podporované funkce

* Namespace vytváření, aktualizaci, odstranění
* Vytvoření centra událostí, aktualizaci, odstranění
* Vytvoření skupiny uživatelů, aktualizaci, odstranění

## <a name="prerequisites"></a>Požadavky

tooget pomocí knihovny správy Event Hubs hello spuštěna, je třeba ověřit s Azure Active Directory (AAD). AAD vyžaduje ověřování jako hlavní název služby, který poskytuje přístup tooyour prostředků Azure. Informace o vytvoření objektu služby najdete v jednom z těchto článků:  

* [Pomocí aplikace hello portálu toocreate Azure Active Directory a objektu služby, které mají přístup k prostředkům](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Tyto kurzy vám poskytují `AppId` (ID klienta), `TenantId`, a `ClientSecret` (ověřovací klíč), které se používají pro ověřování pomocí knihovny správy hello. Musíte mít **vlastníka** oprávnění pro skupinu prostředků hello, na kterém chcete toorun.

## <a name="programming-pattern"></a>Vzor programování

Hello toomanipulate vzor odpovídá jakémukoli prostředku, Event Hubs společný protokol:

1. Získání tokenu z AAD pomocí hello `Microsoft.IdentityModel.Clients.ActiveDirectory` knihovny.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Vytvoření hello `EventHubManagementClient` objektu.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Sada hello `CreateOrUpdate` tooyour parametry zadané hodnoty.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Spusťte volání hello.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Další kroky
* [Ukázka správy rozhraní .NET](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Odkaz na Microsoft.Azure.Management.EventHub](/dotnet/api/Microsoft.Azure.Management.EventHub) 
