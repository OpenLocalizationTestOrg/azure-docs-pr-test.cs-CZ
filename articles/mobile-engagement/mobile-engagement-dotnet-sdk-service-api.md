---
title: "Pomocí sady .NET SDK pro přístup k rozhraní API služby Azure Mobile Engagement"
description: "Popisuje, jak používat Mobile Engagement .NET SDK pro přístup k rozhraní API služby Azure Mobile Engagement"
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
ms.openlocfilehash: 6a497189268c5a1b7e269cc57904ebc77c1906fd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a><span data-ttu-id="be462-103">Pomocí sady .NET SDK pro přístup k rozhraní API služby Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="be462-103">Using .NET SDK to access Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="be462-104">Azure Mobile Engagement poskytuje sadu rozhraní API pro správu zařízení, kampaní Reach/nabízených atd. Pro interakci s Tato rozhraní API, poskytujeme také můžete [soubor Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , můžete pomocí nástrojů můžete vygenerovat pomocí sady SDK pro preferovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="be462-104">Azure Mobile Engagement exposes a set of APIs for you to manage Devices, Reach/Push campaigns etc. To interact with these APIs, we also provide you a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools to generate SDKs for your preferred language.</span></span> <span data-ttu-id="be462-105">Doporučujeme používat [AutoRest](https://github.com/Azure/AutoRest) nástroj pro generování vaší sady SDK z našich souboru Swagger.</span><span class="sxs-lookup"><span data-stu-id="be462-105">We recommend using the [AutoRest](https://github.com/Azure/AutoRest) tool to generate your SDK from our Swagger file.</span></span>

> [!NOTE]
> <span data-ttu-id="be462-106">Službu Azure Mobile Engagement vyřadíme z provozu v březnu 2018. V současnosti je dostupná jenom pro stávající zákazníky.</span><span class="sxs-lookup"><span data-stu-id="be462-106">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="be462-107">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="be462-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="be462-108">Vytvořili jsme .NET SDK podobným způsobem, který umožňuje pracovat s Tato rozhraní API pomocí obálku C# a nemusíte dělat vyjednávání tokenu ověřování a aktualizujte sami.</span><span class="sxs-lookup"><span data-stu-id="be462-108">We have created a .NET SDK in a similar manner which allows you to interact with these APIs using a C# wrapper and you don't have to do the authentication token negotiation and refresh yourself.</span></span>  

<span data-ttu-id="be462-109">Tato ukázka prochází sadu kroků se používat sadu .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="be462-109">This sample goes through the set of steps to follow to use the .NET SDK:</span></span>

1. <span data-ttu-id="be462-110">Nejdřív všech, je potřeba nastavit ověřování pro vaše rozhraní API pomocí Azure Active Directory, jak je popsáno [zde](mobile-engagement-api-authentication.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="be462-110">First of all, you need to setup the authentication for your APIs using the Azure Active Directory as described [here](mobile-engagement-api-authentication.md#authentication).</span></span> <span data-ttu-id="be462-111">Na konci těchto kroků, by měl mít platnou **SubscriptionId**, **TenantId**, **ApplicationId** a **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="be462-111">At the end of these steps, you should have a valid **SubscriptionId**, **TenantId**, **ApplicationId** and **Secret**.</span></span> 
2. <span data-ttu-id="be462-112">Použijeme jednoduchou aplikaci konzoly Windows k předvedení pracovní pomocí .NET SDK se scénářem vytvoření kampaně oznámení.</span><span class="sxs-lookup"><span data-stu-id="be462-112">We will use a simple Windows Console app to demonstrate working with the .NET SDK with the scenario of creating an Announcement campaign.</span></span> <span data-ttu-id="be462-113">Proto otevřete Visual Studio a vytvořte **konzolové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="be462-113">So open up Visual Studio and create a **Console Application**.</span></span>   
3. <span data-ttu-id="be462-114">Potom budete muset stáhnout sady .NET SDK, která je k dispozici jako **knihovna Microsoft Azure Engagement správy** v galerii Nuget [zde](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span><span class="sxs-lookup"><span data-stu-id="be462-114">Next you need to download the .NET SDK which is available as **Microsoft Azure Engagement Management Library** in the Nuget gallery [here](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span></span>
   <span data-ttu-id="be462-115">Pokud instalujete Nuget ze sady Visual Studio, je potřeba zajistit, abyste měli kontrolu označena **zahrnout předběžné verze** možnost při vyhledávání pro balíček:</span><span class="sxs-lookup"><span data-stu-id="be462-115">If you are installing the Nuget from Visual Studio, you need to ensure that you have check marked the **Include prerelease** option while searching for the package:</span></span>
   
    ![][1]
4. <span data-ttu-id="be462-116">V `Program.cs` soubor, přidejte následující obory názvů:</span><span class="sxs-lookup"><span data-stu-id="be462-116">In the `Program.cs` file, add the following namespaces:</span></span>
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. <span data-ttu-id="be462-117">Dále je třeba definovat následující konstanty, které budeme používat pro ověřování a interakci s aplikace Mobile Engagementu, ve kterém vytváříte kampaň oznámení:</span><span class="sxs-lookup"><span data-stu-id="be462-117">Next you need to define the following constants that we will use for authentication and interacting with the Mobile Engagement App in which you are creating the Announcement campaign:</span></span>
   
        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";
   
        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";
   
        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";
6. <span data-ttu-id="be462-118">Definování `EngagementManagementClient` proměnné, které použijete k volání metody Mobile Engagement SDK:</span><span class="sxs-lookup"><span data-stu-id="be462-118">Define the `EngagementManagementClient` variable which we will use to call the Mobile Engagement SDK methods:</span></span>
   
        static EngagementManagementClient engagementClient; 
7. <span data-ttu-id="be462-119">Přidejte následující vaší `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="be462-119">Add the following to your `Main` method:</span></span>
   
        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();
   
                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }
8. <span data-ttu-id="be462-120">Definujte následující metodu, která má na starosti inicializaci `EngagementManagementClient` nejprve ověřením a přiřazení samotné aplikace Mobile Engagementu, ve kterém chcete vytvořit kampaň oznámení:</span><span class="sxs-lookup"><span data-stu-id="be462-120">Define the following method which takes care of initializing the `EngagementManagementClient` by first authenticating and then associating itself with the Mobile Engagement App in which you plan to create the Announcement campaign:</span></span>
   
        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
   
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;
   
            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }
   
   > [!IMPORTANT]
   > <span data-ttu-id="be462-121">Všimněte si, že budete muset použít **název prostředku aplikace** jak jsou definovány v portálu správy Azure pro parametr AppName.</span><span class="sxs-lookup"><span data-stu-id="be462-121">Note that you need to use the **App Resource Name** as defined in the Azure management portal for the AppName parameter.</span></span> 
   > 
   > 
9. <span data-ttu-id="be462-122">Nakonec zadejte CreateCampaign metodu, která se postará o pomocí dříve inicializovaného EngagementClient vytvořit jednoduchou **kdykoli** & **pouze oznámení** kampaně s název a zpráva:</span><span class="sxs-lookup"><span data-stu-id="be462-122">Lastly, define the CreateCampaign method which will take care of using the previously initialized EngagementClient to create a simple **AnyTime** & **Notification-only** campaign with a title and message:</span></span> 
   
        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );
   
            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }
10. <span data-ttu-id="be462-123">Spusťte konzolovou aplikaci a zobrazí se následující v úspěšném vytvoření kampaně:</span><span class="sxs-lookup"><span data-stu-id="be462-123">Run the console app and you will see the following on successful creation of the campaign:</span></span>
    
    <span data-ttu-id="be462-124">**Id kampaně "159" byl úspěšně vytvořen a je ve stavu "konceptu.**</span><span class="sxs-lookup"><span data-stu-id="be462-124">**Campaign Id '159' was created successfully and it is in 'draft' state**</span></span>

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
