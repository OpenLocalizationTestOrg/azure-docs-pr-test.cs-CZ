---
title: "knihovny správy aaaAzure Service Bus | Microsoft Docs"
description: "Správa oborů názvů Service Bus a entity z rozhraní .NET pro zasílání zpráv."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: 9e4ad91f22815ca0838e6e4647a3606109b2b441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-management-libraries"></a>Správa knihovny Service Bus

knihovny Azure Service Bus správy Hello můžete dynamicky zajišťují obory názvů Service Bus a entity. To umožňuje komplexní nasazení a scénáře zasílání zpráv a umožňuje tooprogrammatically určit, jaké tooprovision entity. Tyto knihovny jsou aktuálně dostupné pro rozhraní .NET.

## <a name="supported-functionality"></a>Podporované funkce

* Namespace vytváření, aktualizaci, odstranění
* Vytvoření fronty, aktualizaci, odstranění
* Téma vytváření, aktualizaci, odstranění
* Vytvoření odběru, aktualizaci, odstranění

## <a name="prerequisites"></a>Požadavky

tooget pomocí knihovny management Service Bus hello spuštěna, je třeba ověřit pomocí hello služby Azure Active Directory (AAD). AAD vyžaduje ověřování jako hlavní název služby, který poskytuje přístup tooyour prostředků Azure. Informace o vytvoření objektu služby najdete v jednom z těchto článků:  

* [Pomocí aplikace hello portálu toocreate Azure Active Directory a objektu služby, které mají přístup k prostředkům](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Tyto kurzy vám poskytují `AppId` (ID klienta), `TenantId`, a `ClientSecret` (ověřovací klíč), které se používají pro ověřování pomocí knihovny správy hello. Musíte mít **vlastníka** oprávnění pro skupinu prostředků hello, na kterém chcete toorun.

## <a name="programming-pattern"></a>Vzor programování

Hello toomanipulate vzor odpovídá jakémukoli prostředku, Service Bus společný protokol:

1. Získání tokenu z Azure Active Directory pomocí hello **Microsoft.IdentityModel.Clients.ActiveDirectory** knihovny.
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. Vytvoření hello `ServiceBusManagementClient` objektu.

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. Sada hello `CreateOrUpdate` tooyour parametry zadané hodnoty.

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. Spusťte volání hello.

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Další kroky
* [Ukázka správy rozhraní .NET](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Odkaz na Microsoft.Azure.Management.ServiceBus rozhraní API](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
