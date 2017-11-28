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
# <a name="using-net-sdk-tooaccess-azure-mobile-engagement-service-apis"></a><span data-ttu-id="20f82-103">Pomocí sady .NET SDK tooaccess rozhraní API služby Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="20f82-103">Using .NET SDK tooaccess Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="20f82-104">Azure Mobile Engagement poskytuje sadu rozhraní API pro vás toomanage zařízení, kampaně Reach a nabízenou toointeract atd. pomocí těchto rozhraní API, které poskytujeme také [soubor Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , které můžete použít s nástroje toogenerate SDK pro upřednostňovanou jazyk.</span><span class="sxs-lookup"><span data-stu-id="20f82-104">Azure Mobile Engagement exposes a set of APIs for you toomanage Devices, Reach/Push campaigns etc. toointeract with these APIs, we also provide you a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools toogenerate SDKs for your preferred language.</span></span> <span data-ttu-id="20f82-105">Doporučujeme používat hello [AutoRest](https://github.com/Azure/AutoRest) nástroj toogenerate vaší sady SDK z našich souboru Swagger.</span><span class="sxs-lookup"><span data-stu-id="20f82-105">We recommend using hello [AutoRest](https://github.com/Azure/AutoRest) tool toogenerate your SDK from our Swagger file.</span></span>

> [!NOTE]
> <span data-ttu-id="20f82-106">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="20f82-106">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="20f82-107">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="20f82-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="20f82-108">Vytvořili jsme .NET SDK podobným způsobem, což vám umožní toointeract s Tato rozhraní API pomocí obálku C# a nemáte toodo hello ověřování tokenu vyjednávání a aktualizujte sami.</span><span class="sxs-lookup"><span data-stu-id="20f82-108">We have created a .NET SDK in a similar manner which allows you toointeract with these APIs using a C# wrapper and you don't have toodo hello authentication token negotiation and refresh yourself.</span></span>  

<span data-ttu-id="20f82-109">Tato ukázka prochází hello sadu kroků toofollow toouse hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="20f82-109">This sample goes through hello set of steps toofollow toouse hello .NET SDK:</span></span>

1. <span data-ttu-id="20f82-110">Všech, je třeba nejprve toosetup hello ověřování pro vaše rozhraní API pomocí hello Azure Active Directory, jak je popsáno [zde](mobile-engagement-api-authentication.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="20f82-110">First of all, you need toosetup hello authentication for your APIs using hello Azure Active Directory as described [here](mobile-engagement-api-authentication.md#authentication).</span></span> <span data-ttu-id="20f82-111">Na konci hello z těchto kroků, by měl mít platnou **SubscriptionId**, **TenantId**, **ApplicationId** a **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="20f82-111">At hello end of these steps, you should have a valid **SubscriptionId**, **TenantId**, **ApplicationId** and **Secret**.</span></span> 
2. <span data-ttu-id="20f82-112">Budeme používat jednoduché toodemonstrate aplikace konzoly Windows práce hello .NET SDK se scénářem hello vytvoření kampaně oznámení.</span><span class="sxs-lookup"><span data-stu-id="20f82-112">We will use a simple Windows Console app toodemonstrate working with hello .NET SDK with hello scenario of creating an Announcement campaign.</span></span> <span data-ttu-id="20f82-113">Proto otevřete Visual Studio a vytvořte **konzolové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20f82-113">So open up Visual Studio and create a **Console Application**.</span></span>   
3. <span data-ttu-id="20f82-114">Dále je třeba toodownload hello .NET SDK, která je k dispozici jako **knihovna Microsoft Azure Engagement správy** v galerii Nuget hello [zde](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span><span class="sxs-lookup"><span data-stu-id="20f82-114">Next you need toodownload hello .NET SDK which is available as **Microsoft Azure Engagement Management Library** in hello Nuget gallery [here](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span></span>
   <span data-ttu-id="20f82-115">Pokud instalujete hello Nuget ze sady Visual Studio, je nutné, abyste měli kontrolu označena hello tooensure **zahrnout předběžné verze** možnost při vyhledávání pro balíček hello:</span><span class="sxs-lookup"><span data-stu-id="20f82-115">If you are installing hello Nuget from Visual Studio, you need tooensure that you have check marked hello **Include prerelease** option while searching for hello package:</span></span>
   
    ![][1]
4. <span data-ttu-id="20f82-116">V hello `Program.cs` soubor, přidejte následující obory názvů hello:</span><span class="sxs-lookup"><span data-stu-id="20f82-116">In hello `Program.cs` file, add hello following namespaces:</span></span>
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. <span data-ttu-id="20f82-117">Dále je třeba toodefine hello následující konstanty, které budeme používat pro ověřování a interakci s aplikace Mobile Engagementu hello ve kterém vytváříte kampaň hello oznámení:</span><span class="sxs-lookup"><span data-stu-id="20f82-117">Next you need toodefine hello following constants that we will use for authentication and interacting with hello Mobile Engagement App in which you are creating hello Announcement campaign:</span></span>
   
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
6. <span data-ttu-id="20f82-118">Definování hello `EngagementManagementClient` proměnné, které použijete toocall hello Mobile Engagement SDK metody:</span><span class="sxs-lookup"><span data-stu-id="20f82-118">Define hello `EngagementManagementClient` variable which we will use toocall hello Mobile Engagement SDK methods:</span></span>
   
        static EngagementManagementClient engagementClient; 
7. <span data-ttu-id="20f82-119">Přidejte následující tooyour hello `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="20f82-119">Add hello following tooyour `Main` method:</span></span>
   
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
8. <span data-ttu-id="20f82-120">Definování hello následující metodu, která má na starosti inicializaci hello `EngagementManagementClient` nejprve ověřením a přiřazení samotné aplikace Mobile Engagementu hello ve které máte v plánu toocreate hello oznámení kampaně:</span><span class="sxs-lookup"><span data-stu-id="20f82-120">Define hello following method which takes care of initializing hello `EngagementManagementClient` by first authenticating and then associating itself with hello Mobile Engagement App in which you plan toocreate hello Announcement campaign:</span></span>
   
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
   > <span data-ttu-id="20f82-121">Všimněte si, že je potřeba toouse hello **název prostředku aplikace** jak jsou definovány v portálu pro správu Azure hello parametru hello AppName.</span><span class="sxs-lookup"><span data-stu-id="20f82-121">Note that you need toouse hello **App Resource Name** as defined in hello Azure management portal for hello AppName parameter.</span></span> 
   > 
   > 
9. <span data-ttu-id="20f82-122">Nakonec zadejte hello CreateCampaign metodu, která se postará o pomocí toocreate EngagementClient hello dříve inicializovat jednoduchý **kdykoli** & **pouze oznámení** kampaně název a zpráva:</span><span class="sxs-lookup"><span data-stu-id="20f82-122">Lastly, define hello CreateCampaign method which will take care of using hello previously initialized EngagementClient toocreate a simple **AnyTime** & **Notification-only** campaign with a title and message:</span></span> 
   
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
10. <span data-ttu-id="20f82-123">Spuštění hello konzolovou aplikaci a zobrazí v úspěšném vytvoření kampaně hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="20f82-123">Run hello console app and you will see hello following on successful creation of hello campaign:</span></span>
    
    <span data-ttu-id="20f82-124">**Id kampaně "159" byl úspěšně vytvořen a je ve stavu "konceptu.**</span><span class="sxs-lookup"><span data-stu-id="20f82-124">**Campaign Id '159' was created successfully and it is in 'draft' state**</span></span>

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
