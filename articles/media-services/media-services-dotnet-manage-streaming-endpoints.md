---
title: "aaaManage streamování koncové body pomocí sady .NET SDK. | Dokumentace Microsoftu"
description: "Toto téma ukazuje, jak hello koncových bodů streamování toomanage pomocí portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 0da34a97-f36c-48d0-8ea2-ec12584a2215
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 30c092a8ebf4e2b2902392f4cf98f46d812ccdbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-net-sdk"></a><span data-ttu-id="473ea-104">Správa koncových bodů streamování pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="473ea-104">Manage streaming endpoints with .NET SDK</span></span>

>[!NOTE]
><span data-ttu-id="473ea-105">Ujistěte se, zda text hello tooreview [přehled](media-services-streaming-endpoints-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="473ea-105">Make sure tooreview hello [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> <span data-ttu-id="473ea-106">Projděte si také téma [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="473ea-106">Also, review [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="473ea-107">Hello kód v tomto tématu ukazuje, jak hello toodo hello následující úlohy pomocí Azure Media Services .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="473ea-107">hello code in this topic shows how toodo hello following tasks using hello Azure Media Services .NET SDK:</span></span>

- <span data-ttu-id="473ea-108">Zkontrolujte hello výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="473ea-108">Examine hello default streaming endpoint.</span></span>
- <span data-ttu-id="473ea-109">Vytvořit nebo přidáte nový koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="473ea-109">Create/add new streaming endpoint.</span></span>

    <span data-ttu-id="473ea-110">Můžete chtít toohave více koncových bodů streamování Pokud máte v plánu toohave různých sítím CDN nebo CDN a přímý přístup.</span><span class="sxs-lookup"><span data-stu-id="473ea-110">You might want toohave multiple streaming endpoints if you plan toohave different CDNs or a CDN and direct access.</span></span>

    > [!NOTE]
    > <span data-ttu-id="473ea-111">Fakturuje se jenom, když je v běžícím stavu je koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="473ea-111">You are only billed when your Streaming Endpoint is in running state.</span></span>
    
- <span data-ttu-id="473ea-112">Aktualizujte hello koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="473ea-112">Update hello streaming endpoint.</span></span>
    
    <span data-ttu-id="473ea-113">Ujistěte se, zda text hello toocall Update() funkce.</span><span class="sxs-lookup"><span data-stu-id="473ea-113">Make sure toocall hello Update() function.</span></span>

- <span data-ttu-id="473ea-114">Odstraňte koncový bod streamování hello.</span><span class="sxs-lookup"><span data-stu-id="473ea-114">Delete hello streaming endpoint.</span></span>

    >[!NOTE]
    ><span data-ttu-id="473ea-115">nelze odstranit, Hello výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="473ea-115">hello default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="473ea-116">Informace o tom, jak tooscale hello koncový bod streamování najdete v tématu [to](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="473ea-116">For information about how tooscale hello streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="473ea-117">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="473ea-117">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="473ea-118">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="473ea-118">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="add-code-that-manages-streaming-endpoints"></a><span data-ttu-id="473ea-119">Přidejte kód, který spravuje koncových bodů streamování</span><span class="sxs-lookup"><span data-stu-id="473ea-119">Add code that manages streaming endpoints</span></span>
    
<span data-ttu-id="473ea-120">Nahraďte kód hello v hello Program.cs hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="473ea-120">Replace hello code in hello Program.cs with hello following code:</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;

    namespace AMSStreamingEndpoint
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            var defaultStreamingEndpoint = _context.StreamingEndpoints.Where(s => s.Name.Contains("default")).FirstOrDefault();
            ExamineStreamingEndpoint(defaultStreamingEndpoint);

            IStreamingEndpoint newStreamingEndpoint = AddStreamingEndpoint();
            UpdateStreamingEndpoint(newStreamingEndpoint);
            DeleteStreamingEndpoint(newStreamingEndpoint);
        }

        static public void ExamineStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            Console.WriteLine(streamingEndpoint.Name);
            Console.WriteLine(streamingEndpoint.StreamingEndpointVersion);
            Console.WriteLine(streamingEndpoint.FreeTrialEndTime);
            Console.WriteLine(streamingEndpoint.ScaleUnits);
            Console.WriteLine(streamingEndpoint.CdnProvider);
            Console.WriteLine(streamingEndpoint.CdnProfile);
            Console.WriteLine(streamingEndpoint.CdnEnabled);
        }

        static public IStreamingEndpoint AddStreamingEndpoint()
        {
            var name = "StreamingEndpoint" + DateTime.UtcNow.ToString("hhmmss");
            var option = new StreamingEndpointCreationOptions(name, 1)
            {
            StreamingEndpointVersion = new Version("2.0"),
            CdnEnabled = true,
            CdnProfile = "CdnProfile",
            CdnProvider = CdnProviderType.PremiumVerizon
            };

            var streamingEndpoint = _context.StreamingEndpoints.Create(option);

            return streamingEndpoint;
        }

        static public void UpdateStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            if (streamingEndpoint.StreamingEndpointVersion == "1.0")
            streamingEndpoint.StreamingEndpointVersion = "2.0";

            streamingEndpoint.CdnEnabled = false;
            streamingEndpoint.Update();
        }

        static public void DeleteStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            streamingEndpoint.Delete();
        }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="473ea-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="473ea-121">Next steps</span></span>
<span data-ttu-id="473ea-122">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="473ea-122">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="473ea-123">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="473ea-123">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

