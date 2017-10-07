---
title: "aaaUsing tooaccess .NET SDK rozhraní API služby Azure Mobile Engagement"
description: "Popisuje, jak toouse hello Mobile Engagement .NET SDK tooaccess rozhraní API služby Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: c07728aa-43f2-4238-8b4a-c9eddf9d838b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 812be6f507b29f7b2de6454e06face8082c2d161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-net-sdk-tooaccess-azure-mobile-engagement-service-apis"></a>Pomocí sady .NET SDK tooaccess rozhraní API služby Azure Mobile Engagement
Azure Mobile Engagement poskytuje sadu rozhraní API pro vás toomanage zařízení, kampaně Reach a nabízenou toointeract atd. pomocí těchto rozhraní API, které poskytujeme také [soubor Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , které můžete použít s nástroje toogenerate SDK pro upřednostňovanou jazyk. Doporučujeme používat hello [AutoRest](https://github.com/Azure/AutoRest) nástroj toogenerate vaší sady SDK z našich souboru Swagger.

> [!NOTE]
> Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků. Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Vytvořili jsme .NET SDK podobným způsobem, což vám umožní toointeract s Tato rozhraní API pomocí obálku C# a nemáte toodo hello ověřování tokenu vyjednávání a aktualizujte sami.  

Tato ukázka prochází hello sadu kroků toofollow toouse hello .NET SDK:

1. Všech, je třeba nejprve toosetup hello ověřování pro vaše rozhraní API pomocí hello Azure Active Directory, jak je popsáno [zde](mobile-engagement-api-authentication.md#authentication). Na konci hello z těchto kroků, by měl mít platnou **SubscriptionId**, **TenantId**, **ApplicationId** a **tajný klíč**. 
2. Budeme používat jednoduché toodemonstrate aplikace konzoly Windows práce hello .NET SDK se scénářem hello vytvoření kampaně oznámení. Proto otevřete Visual Studio a vytvořte **konzolové aplikace**.   
3. Dále je třeba toodownload hello .NET SDK, která je k dispozici jako **knihovna Microsoft Azure Engagement správy** v galerii Nuget hello [zde](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
   Pokud instalujete hello Nuget ze sady Visual Studio, je nutné, abyste měli kontrolu označena hello tooensure **zahrnout předběžné verze** možnost při vyhledávání pro balíček hello:
   
    ![][1]
4. V hello `Program.cs` soubor, přidejte následující obory názvů hello:
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. Dále je třeba toodefine hello následující konstanty, které budeme používat pro ověřování a interakci s aplikace Mobile Engagementu hello ve kterém vytváříte kampaň hello oznámení:
   
        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";
   
        // This is hello Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";
   
        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using hello one as specified in hello Azure portal (NOT hello App Name)
        const string APP_RESOURCE_NAME = "";
6. Definování hello `EngagementManagementClient` proměnné, které použijete toocall hello Mobile Engagement SDK metody:
   
        static EngagementManagementClient engagementClient; 
7. Přidejte následující tooyour hello `Main` metoda:
   
        try
            {
                // Initialize hello Engagement SDK toocall out APIs. 
                InitEngagementClient().Wait();
   
                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }
8. Definování hello následující metodu, která má na starosti inicializaci hello `EngagementManagementClient` nejprve ověřením a přiřazení samotné aplikace Mobile Engagementu hello ve které máte v plánu toocreate hello oznámení kampaně:
   
        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
   
            // This is hello Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;
   
            // This is your Mobile Engagement App Collection & App Resource Name in which you create hello campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }
   
   > [!IMPORTANT]
   > Všimněte si, že je potřeba toouse hello **název prostředku aplikace** jak jsou definovány v portálu pro správu Azure hello parametru hello AppName. 
   > 
   > 
9. Nakonec zadejte hello CreateCampaign metodu, která se postará o pomocí toocreate EngagementClient hello dříve inicializovat jednoduchý **kdykoli** & **pouze oznámení** kampaně název a zpráva: 
   
        private async static Task CreateCampaign()
        {
            //  Refer toohello Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all hello non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing hello app!",
                type:"only_notif",
                deliveryTime:"any"
                );
   
            // Refer toohello Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }
10. Spuštění hello konzolovou aplikaci a zobrazí v úspěšném vytvoření kampaně hello hello následující:
    
    **Id kampaně "159" byl úspěšně vytvořen a je ve stavu "konceptu.**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
