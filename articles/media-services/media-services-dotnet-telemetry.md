---
title: "Konfigurace služby Azure Media Services telemetrie s .NET | Microsoft Docs"
description: "Tento článek ukazuje, jak používat telemetrie Azure Media Services pomocí sady .NET SDK."
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
ms.openlocfilehash: 1d857f3d062d8d1b15c64fa4b8c3e27ad6c2247e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-azure-media-services-telemetry-with-net"></a><span data-ttu-id="d5a95-103">Konfigurace služby Azure Media Services telemetrie s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="d5a95-103">Configuring Azure Media Services telemetry with .NET</span></span>

<span data-ttu-id="d5a95-104">Toto téma popisuje obecné kroky, které může provést při konfiguraci telemetrie Azure Media Services (AMS) pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="d5a95-104">This topic describes general steps that you might take when configuring the Azure Media Services (AMS) telemetry using .NET SDK.</span></span> 

>[!NOTE]
><span data-ttu-id="d5a95-105">Podrobné vysvětlení, co je AMS telemetrie a jak ho zpracovat, najdete v části [přehled](media-services-telemetry-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="d5a95-105">For the detailed explanation of what is AMS telemetry and how to consume it, see the [overview](media-services-telemetry-overview.md) topic.</span></span>

<span data-ttu-id="d5a95-106">Můžete využívat telemetrická data v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="d5a95-106">You can consume telemetry data in one of the following ways:</span></span>

- <span data-ttu-id="d5a95-107">Číst data přímo z úložiště tabulek Azure (např. pomocí sady SDK úložiště).</span><span class="sxs-lookup"><span data-stu-id="d5a95-107">Read data directly from Azure Table Storage (e.g. using the Storage SDK).</span></span> <span data-ttu-id="d5a95-108">Popis telemetrie úložiště tabulek naleznete v tématu **využívání telemetrické informace** v [to](https://msdn.microsoft.com/library/mt742089.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="d5a95-108">For the description of telemetry storage tables, see the **Consuming telemetry information** in [this](https://msdn.microsoft.com/library/mt742089.aspx) topic.</span></span>

<span data-ttu-id="d5a95-109">Nebo</span><span class="sxs-lookup"><span data-stu-id="d5a95-109">Or</span></span>

- <span data-ttu-id="d5a95-110">Použijte podporu v .NET SDK služby Media Services pro čtení dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5a95-110">Use the support in the Media Services .NET SDK for reading storage data.</span></span> <span data-ttu-id="d5a95-111">Toto téma ukazuje, jak povolit telemetrii pro zadaný účet AMS a jak dotazovat metriky pomocí Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="d5a95-111">This topic shows how to enable telemetry for the specified AMS account and how to query the metrics using the Azure Media Services .NET SDK.</span></span>  

## <a name="configuring-telemetry-for-a-media-services-account"></a><span data-ttu-id="d5a95-112">Konfigurace telemetrie pro účet Media Services</span><span class="sxs-lookup"><span data-stu-id="d5a95-112">Configuring telemetry for a Media Services account</span></span>

<span data-ttu-id="d5a95-113">Chcete-li povolit telemetrii vyžaduje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5a95-113">The following steps are needed to enable telemetry:</span></span>

- <span data-ttu-id="d5a95-114">Získání přihlašovacích údajů účtu úložiště připojené k účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="d5a95-114">Get the credentials of the storage account attached to the Media Services account.</span></span> 
- <span data-ttu-id="d5a95-115">Vytvoření koncového bodu oznámení s **EndPointType** nastavena na **AzureTable** a endPointAddress odkazující na tabulku úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5a95-115">Create a Notification Endpoint with **EndPointType** set to **AzureTable** and endPointAddress pointing to the storage table.</span></span>

        INotificationEndPoint notificationEndPoint = 
                      _context.NotificationEndPoints.Create("monitoring", 
                      NotificationEndPointType.AzureTable,
                      "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/");

- <span data-ttu-id="d5a95-116">Vytvořte monitorování konfiguraci nastavení pro služby, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="d5a95-116">Create a monitoring configuration settings for the services you want to monitor.</span></span> <span data-ttu-id="d5a95-117">Více než jeden monitorování nastavení konfigurace je povolen.</span><span class="sxs-lookup"><span data-stu-id="d5a95-117">No more than one monitoring configuration settings is allowed.</span></span> 
  
        IMonitoringConfiguration monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
            new List<ComponentMonitoringSetting>()
            {
                new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
            });

## <a name="consuming-telemetry-information"></a><span data-ttu-id="d5a95-118">Využívání telemetrické informace</span><span class="sxs-lookup"><span data-stu-id="d5a95-118">Consuming telemetry information</span></span>

<span data-ttu-id="d5a95-119">Informace o využívání telemetrické informace najdete v tématu [to](media-services-telemetry-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="d5a95-119">For information about consuming telemetry information, see [this](media-services-telemetry-overview.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="d5a95-120">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="d5a95-120">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="d5a95-121">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="d5a95-121">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

2. <span data-ttu-id="d5a95-122">Přidejte následující element pro **appSettings** definované v souboru app.config:</span><span class="sxs-lookup"><span data-stu-id="d5a95-122">Add the following element to **appSettings** defined in your app.config file:</span></span>

    <add key="StorageAccountName" value="storage_name" />
 
## <a name="example"></a><span data-ttu-id="d5a95-123">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5a95-123">Example</span></span>  
    
<span data-ttu-id="d5a95-124">Následující příklad ukazuje, jak povolit telemetrii pro zadaný účet AMS a jak dotazovat metriky pomocí Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="d5a95-124">The following example shows how to enable telemetry for the specified AMS account and how to query the metrics using the Azure Media Services .NET SDK.</span></span>  

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

            // Print the channel metrics.
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


## <a name="next-steps"></a><span data-ttu-id="d5a95-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5a95-125">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d5a95-126">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="d5a95-126">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
