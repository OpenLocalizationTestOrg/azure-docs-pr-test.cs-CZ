---
title: aaaConfiguring telemetrie Azure Media Services s .NET | Microsoft Docs
description: "Tento článek ukazuje, jak toouse hello telemetrie Azure Media Services pomocí sady .NET SDK."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f8f55e37-0714-49ea-bf4a-e6c1319bec44
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 4019fa7d080ca3f8a8709bd1e666f7062b883954
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-azure-media-services-telemetry-with-net"></a><span data-ttu-id="130b8-103">Konfigurace služby Azure Media Services telemetrie s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="130b8-103">Configuring Azure Media Services telemetry with .NET</span></span>

<span data-ttu-id="130b8-104">Toto téma popisuje obecné kroky, které může provést při konfiguraci telemetrie hello Azure Media Services (AMS) pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="130b8-104">This topic describes general steps that you might take when configuring hello Azure Media Services (AMS) telemetry using .NET SDK.</span></span> 

>[!NOTE]
><span data-ttu-id="130b8-105">Pro hello podrobné vysvětlení, co je AMS telemetrie a jak tooconsume, najdete v části hello [přehled](media-services-telemetry-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="130b8-105">For hello detailed explanation of what is AMS telemetry and how tooconsume it, see hello [overview](media-services-telemetry-overview.md) topic.</span></span>

<span data-ttu-id="130b8-106">Můžete využívat telemetrická data v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="130b8-106">You can consume telemetry data in one of hello following ways:</span></span>

- <span data-ttu-id="130b8-107">Číst data přímo z úložiště tabulek Azure (např. pomocí hello sada SDK úložiště).</span><span class="sxs-lookup"><span data-stu-id="130b8-107">Read data directly from Azure Table Storage (e.g. using hello Storage SDK).</span></span> <span data-ttu-id="130b8-108">Popis hello telemetrie úložiště tabulek naleznete v tématu hello **využívání telemetrické informace** v [to](https://msdn.microsoft.com/library/mt742089.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="130b8-108">For hello description of telemetry storage tables, see hello **Consuming telemetry information** in [this](https://msdn.microsoft.com/library/mt742089.aspx) topic.</span></span>

<span data-ttu-id="130b8-109">Nebo</span><span class="sxs-lookup"><span data-stu-id="130b8-109">Or</span></span>

- <span data-ttu-id="130b8-110">Podpora použití hello v hello sady Media Services .NET SDK pro čtení dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="130b8-110">Use hello support in hello Media Services .NET SDK for reading storage data.</span></span> <span data-ttu-id="130b8-111">Toto téma ukazuje, jak tooenable telemetrie hello zadán účet AMS a jak hello tooquery hello metriky pomocí Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="130b8-111">This topic shows how tooenable telemetry for hello specified AMS account and how tooquery hello metrics using hello Azure Media Services .NET SDK.</span></span>  

## <a name="configuring-telemetry-for-a-media-services-account"></a><span data-ttu-id="130b8-112">Konfigurace telemetrie pro účet Media Services</span><span class="sxs-lookup"><span data-stu-id="130b8-112">Configuring telemetry for a Media Services account</span></span>

<span data-ttu-id="130b8-113">Hello následující kroky jsou potřebné tooenable telemetrie:</span><span class="sxs-lookup"><span data-stu-id="130b8-113">hello following steps are needed tooenable telemetry:</span></span>

- <span data-ttu-id="130b8-114">Získáte přihlašovací údaje hello hello úložiště připojeného účtu toohello účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="130b8-114">Get hello credentials of hello storage account attached toohello Media Services account.</span></span> 
- <span data-ttu-id="130b8-115">Vytvoření koncového bodu oznámení s **EndPointType** nastavit příliš**AzureTable** a endPointAddress odkazující tabulce toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="130b8-115">Create a Notification Endpoint with **EndPointType** set too**AzureTable** and endPointAddress pointing toohello storage table.</span></span>

        INotificationEndPoint notificationEndPoint = 
                      _context.NotificationEndPoints.Create("monitoring", 
                      NotificationEndPointType.AzureTable,
                      "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/");

- <span data-ttu-id="130b8-116">Vytvořit nastavení konfigurace sledování hello služeb, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="130b8-116">Create a monitoring configuration settings for hello services you want toomonitor.</span></span> <span data-ttu-id="130b8-117">Více než jeden monitorování nastavení konfigurace je povolen.</span><span class="sxs-lookup"><span data-stu-id="130b8-117">No more than one monitoring configuration settings is allowed.</span></span> 
  
        IMonitoringConfiguration monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
            new List<ComponentMonitoringSetting>()
            {
                new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
            });

## <a name="consuming-telemetry-information"></a><span data-ttu-id="130b8-118">Využívání telemetrické informace</span><span class="sxs-lookup"><span data-stu-id="130b8-118">Consuming telemetry information</span></span>

<span data-ttu-id="130b8-119">Informace o využívání telemetrické informace najdete v tématu [to](media-services-telemetry-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="130b8-119">For information about consuming telemetry information, see [this](media-services-telemetry-overview.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="130b8-120">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="130b8-120">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="130b8-121">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="130b8-121">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

2. <span data-ttu-id="130b8-122">Přidejte následující element příliš hello**appSettings** definované v souboru app.config:</span><span class="sxs-lookup"><span data-stu-id="130b8-122">Add hello following element too**appSettings** defined in your app.config file:</span></span>

    <add key="StorageAccountName" value="storage_name" />
 
## <a name="example"></a><span data-ttu-id="130b8-123">Příklad</span><span class="sxs-lookup"><span data-stu-id="130b8-123">Example</span></span>  
    
<span data-ttu-id="130b8-124">Hello následující příklad ukazuje, jak tooenable telemetrie hello zadán účet AMS a jak hello tooquery hello metriky pomocí Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="130b8-124">hello following example shows how tooenable telemetry for hello specified AMS account and how tooquery hello metrics using hello Azure Media Services .NET SDK.</span></span>  

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;

    namespace AMSMetrics
    {
        class Program
        {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly string _mediaServicesStorageAccountName =
            ConfigurationManager.AppSettings["StorageAccountName"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static IStreamingEndpoint _streamingEndpoint = null;
        private static IChannel _channel = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            _streamingEndpoint = _context.StreamingEndpoints.FirstOrDefault();
            _channel = _context.Channels.FirstOrDefault();

            var monitoringConfigurations = _context.MonitoringConfigurations;
            IMonitoringConfiguration monitoringConfiguration = null;

            // No more than one monitoring configuration settings is allowed.
            if (monitoringConfigurations.ToArray().Length != 0)
            {
            monitoringConfiguration = _context.MonitoringConfigurations.FirstOrDefault();
            }
            else
            {
            INotificationEndPoint notificationEndPoint =
                      _context.NotificationEndPoints.Create("monitoring",
                      NotificationEndPointType.AzureTable, GetTableEndPoint());

            monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
                new List<ComponentMonitoringSetting>()
                {
                    new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                    new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)

                });
            }

            //Print metrics for a Streaming Endpoint.
            PrintStreamingEndpointMetrics();

            Console.ReadLine();
        }

        private static string GetTableEndPoint()
        {
            return "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/";
        }

        private static void PrintStreamingEndpointMetrics()
        {
            Console.WriteLine(string.Format("Telemetry for streaming endpoint '{0}'", _streamingEndpoint.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some streaming endpoint metrics.
            var telemetry = _streamingEndpoint.GetTelemetry();

            var res = telemetry.GetStreamingEndpointRequestLogs(timerangeStart, timerangeEnd);

            Console.Title = "Streaming endpoint metrics:";

            foreach (var log in res)
            {
            Console.WriteLine("AccountId: {0}", log.AccountId);
            Console.WriteLine("BytesSent: {0}", log.BytesSent);
            Console.WriteLine("EndToEndLatency: {0}", log.EndToEndLatency);
            Console.WriteLine("HostName: {0}", log.HostName);
            Console.WriteLine("ObservedTime: {0}", log.ObservedTime);
            Console.WriteLine("PartitionKey: {0}", log.PartitionKey);
            Console.WriteLine("RequestCount: {0}", log.RequestCount);
            Console.WriteLine("ResultCode: {0}", log.ResultCode);
            Console.WriteLine("RowKey: {0}", log.RowKey);
            Console.WriteLine("ServerLatency: {0}", log.ServerLatency);
            Console.WriteLine("StatusCode: {0}", log.StatusCode);
            Console.WriteLine("StreamingEndpointId: {0}", log.StreamingEndpointId);
            Console.WriteLine();
            }

            Console.WriteLine();
        }

        private static void PrintChannelMetrics()
        {
            if (_channel == null)
            {
            Console.WriteLine("There are no channels in this AMS account");
            return;
            }

            Console.WriteLine(string.Format("Telemetry for channel '{0}'", _channel.Name));

            DateTime timerangeEnd = DateTime.UtcNow; 
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some channel metrics.
            var telemetry = _channel.GetTelemetry();

            var channelMetrics = telemetry.GetChannelHeartbeats(timerangeStart, timerangeEnd);

            // Print hello channel metrics.
            Console.WriteLine("Channel metrics:");

            foreach (var channelHeartbeat in channelMetrics.OrderBy(x => x.ObservedTime))
            {
            Console.WriteLine(
                "    Observed time: {0}, Last timestamp: {1}, Incoming bitrate: {2}",
                channelHeartbeat.ObservedTime,
                channelHeartbeat.LastTimestamp,
                channelHeartbeat.IncomingBitrate);
            }

            Console.WriteLine();
        }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="130b8-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="130b8-125">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="130b8-126">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="130b8-126">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
