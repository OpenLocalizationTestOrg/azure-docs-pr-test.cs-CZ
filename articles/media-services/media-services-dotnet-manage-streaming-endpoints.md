---
title: "Správa koncových bodů streamování pomocí .NET SDK. | Dokumentace Microsoftu"
description: "Toto téma ukazuje, jak spravovat koncových bodů streamování pomocí portálu Azure."
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
ms.openlocfilehash: 2f4f464f8604b6f453d6b50b736c6a3a889a3408
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-streaming-endpoints-with-net-sdk"></a><span data-ttu-id="01d1f-104">Správa koncových bodů streamování pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="01d1f-104">Manage streaming endpoints with .NET SDK</span></span>

>[!NOTE]
><span data-ttu-id="01d1f-105">Projděte si [přehled](media-services-streaming-endpoints-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="01d1f-105">Make sure to review the [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> <span data-ttu-id="01d1f-106">Projděte si také téma [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="01d1f-106">Also, review [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="01d1f-107">Kód v tomto tématu ukazuje, jak provést tyto úlohy pomocí Azure Media Services .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="01d1f-107">The code in this topic shows how to do the following tasks using the Azure Media Services .NET SDK:</span></span>

- <span data-ttu-id="01d1f-108">Zkontrolujte výchozí koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="01d1f-108">Examine the default streaming endpoint.</span></span>
- <span data-ttu-id="01d1f-109">Vytvořit nebo přidáte nový koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="01d1f-109">Create/add new streaming endpoint.</span></span>

    <span data-ttu-id="01d1f-110">Můžete chtít mít několik koncových bodů streamování, pokud chcete mít jiný sítím CDN nebo CDN a přímý přístup.</span><span class="sxs-lookup"><span data-stu-id="01d1f-110">You might want to have multiple streaming endpoints if you plan to have different CDNs or a CDN and direct access.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01d1f-111">Fakturuje se jenom, když je v běžícím stavu je koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="01d1f-111">You are only billed when your Streaming Endpoint is in running state.</span></span>
    
- <span data-ttu-id="01d1f-112">Aktualizujte koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="01d1f-112">Update the streaming endpoint.</span></span>
    
    <span data-ttu-id="01d1f-113">Ujistěte se, že volání funkce Update().</span><span class="sxs-lookup"><span data-stu-id="01d1f-113">Make sure to call the Update() function.</span></span>

- <span data-ttu-id="01d1f-114">Odstraňte koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="01d1f-114">Delete the streaming endpoint.</span></span>

    >[!NOTE]
    ><span data-ttu-id="01d1f-115">Nelze odstranit výchozí koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="01d1f-115">The default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="01d1f-116">Informace o tom, jak škálování koncový bod streamování najdete v tématu [to](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="01d1f-116">For information about how to scale the streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="01d1f-117">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="01d1f-117">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="01d1f-118">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="01d1f-118">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="add-code-that-manages-streaming-endpoints"></a><span data-ttu-id="01d1f-119">Přidejte kód, který spravuje koncových bodů streamování</span><span class="sxs-lookup"><span data-stu-id="01d1f-119">Add code that manages streaming endpoints</span></span>
    
<span data-ttu-id="01d1f-120">Nahraďte kód souboru Program.cs následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="01d1f-120">Replace the code in the Program.cs with the following code:</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;

    namespace AMSStreamingEndpoint
    {
        class Program
        {
        // Read values from the App.config file.
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


## <a name="next-steps"></a><span data-ttu-id="01d1f-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01d1f-121">Next steps</span></span>
<span data-ttu-id="01d1f-122">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="01d1f-122">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="01d1f-123">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="01d1f-123">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
