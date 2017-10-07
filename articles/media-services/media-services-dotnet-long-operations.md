---
title: "aaaPolling dlouhotrvající operace | Microsoft Docs"
description: "Toto téma ukazuje, jak dlouho běžící operace toopoll."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: f8315a5ddbe484d794c3e2164e47dd9e70521671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a><span data-ttu-id="8461d-103">Doručování živé streamování pomocí služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="8461d-103">Delivering Live Streaming with Azure Media Services</span></span>

## <a name="overview"></a><span data-ttu-id="8461d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8461d-104">Overview</span></span>

<span data-ttu-id="8461d-105">Microsoft Azure Media Services nabízí rozhraní API, která posílají požadavky na tooMedia služby toostart operace (například: vytvoření, spuštění, zastavení nebo odstranit kanál).</span><span class="sxs-lookup"><span data-stu-id="8461d-105">Microsoft Azure Media Services offers APIs that send requests tooMedia Services toostart operations (for example: create, start, stop, or delete a channel).</span></span> <span data-ttu-id="8461d-106">Tyto operace jsou časově náročné.</span><span class="sxs-lookup"><span data-stu-id="8461d-106">These operations are long-running.</span></span>

<span data-ttu-id="8461d-107">Hello sady Media Services .NET SDK poskytuje rozhraní API, která odeslání požadavku hello a počkejte hello operaci toocomplete (interně hello, které rozhraní API se dotazuje na průběh operace v některé intervalech).</span><span class="sxs-lookup"><span data-stu-id="8461d-107">hello Media Services .NET SDK provides APIs that send hello request and wait for hello operation toocomplete (internally, hello APIs are polling for operation progress at some intervals).</span></span> <span data-ttu-id="8461d-108">Například při volání kanálu. Start(), vrátí metoda hello po spuštění hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="8461d-108">For example, when you call channel.Start(), hello method returns after hello channel is started.</span></span> <span data-ttu-id="8461d-109">Můžete také použít asynchronní verze hello: await kanál. StartAsync() (informace o asynchronní vzor založený na úlohách najdete v tématu [klepněte na](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span><span class="sxs-lookup"><span data-stu-id="8461d-109">You can also use hello asynchronous version: await channel.StartAsync() (for information about Task-based Asynchronous Pattern, see [TAP](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span></span> <span data-ttu-id="8461d-110">Rozhraní API, které odeslat žádost o operaci a poté dotazování na stav hello až do dokončení operace hello se nazývají "dotazování metody".</span><span class="sxs-lookup"><span data-stu-id="8461d-110">APIs that send an operation request and then poll for hello status until hello operation is complete are called “polling methods”.</span></span> <span data-ttu-id="8461d-111">Tyto metody (zejména hello asynchronní verze) se doporučují pro aplikacemi rich client nebo stavové služby.</span><span class="sxs-lookup"><span data-stu-id="8461d-111">These methods (especially hello Async version) are recommended for rich client applications and/or stateful services.</span></span>

<span data-ttu-id="8461d-112">Existují scénáře, kde aplikace nemůže čekat na dlouho spuštěný požadavek http a chce toopoll pro průběh operace hello ručně.</span><span class="sxs-lookup"><span data-stu-id="8461d-112">There are scenarios where an application cannot wait for a long running http request and wants toopoll for hello operation progress manually.</span></span> <span data-ttu-id="8461d-113">Typickým příkladem by prohlížeč interakci s bezstavové webové služby: Pokud prohlížeč hello požaduje toocreate kanál, hello webová služba spustí dlouhotrvající operace a vrátí hello prohlížeče toohello ID operace.</span><span class="sxs-lookup"><span data-stu-id="8461d-113">A typical example would be a browser interacting with a stateless web service: when hello browser requests toocreate a channel, hello web service initiates a long running operation and returns hello operation ID toohello browser.</span></span> <span data-ttu-id="8461d-114">Prohlížeč Hello může pak požádejte hello webové služby tooget hello stav operace na základě ID hello.</span><span class="sxs-lookup"><span data-stu-id="8461d-114">hello browser could then ask hello web service tooget hello operation status based on hello ID.</span></span> <span data-ttu-id="8461d-115">Hello sady Media Services .NET SDK poskytuje rozhraní API, které jsou užitečné pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="8461d-115">hello Media Services .NET SDK provides APIs that are useful for this scenario.</span></span> <span data-ttu-id="8461d-116">Tato rozhraní API se nazývají "-cyklického dotazování metody".</span><span class="sxs-lookup"><span data-stu-id="8461d-116">These APIs are called “non-polling methods”.</span></span>
<span data-ttu-id="8461d-117">"Hello-cyklického dotazování metody" mají následující vzoru pro pojmenovávání hello: Odeslat*OperationName*operace (například SendCreateOperation).</span><span class="sxs-lookup"><span data-stu-id="8461d-117">hello “non-polling methods” have hello following naming pattern: Send*OperationName*Operation (for example, SendCreateOperation).</span></span> <span data-ttu-id="8461d-118">Odeslat*OperationName*operaci metody vrací hello **IOperation** objektu; hello vráceného objektu obsahuje informace, které lze použít tootrack hello operaci.</span><span class="sxs-lookup"><span data-stu-id="8461d-118">Send*OperationName*Operation methods return hello **IOperation** object; hello returned object contains information that can be used tootrack hello operation.</span></span> <span data-ttu-id="8461d-119">Hello odesílání*OperationName*OperationAsync metody vrací **úloh<IOperation>**.</span><span class="sxs-lookup"><span data-stu-id="8461d-119">hello Send*OperationName*OperationAsync methods return **Task<IOperation>**.</span></span>

<span data-ttu-id="8461d-120">V současné době hello následující třídy podporu-cyklického dotazování metody: **kanál**, **StreamingEndpoint**, a **programu**.</span><span class="sxs-lookup"><span data-stu-id="8461d-120">Currently, hello following classes support non-polling methods:  **Channel**, **StreamingEndpoint**, and **Program**.</span></span>

<span data-ttu-id="8461d-121">toopoll pro hello stav operace, použijte hello **GetOperation** metodu hello **OperationBaseCollection** třídy.</span><span class="sxs-lookup"><span data-stu-id="8461d-121">toopoll for hello operation status, use hello **GetOperation** method on hello **OperationBaseCollection** class.</span></span> <span data-ttu-id="8461d-122">Použijte následující stav operace hello toocheck intervaly hello: pro **kanál** a **StreamingEndpoint** operace, použijte 30 sekund; pro **Program** operace, použijte 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="8461d-122">Use hello following intervals toocheck hello operation status: for **Channel** and **StreamingEndpoint** operations, use 30 seconds; for **Program** operations, use 10 seconds.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="8461d-123">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="8461d-123">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="8461d-124">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="8461d-124">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span>

## <a name="example"></a><span data-ttu-id="8461d-125">Příklad</span><span class="sxs-lookup"><span data-stu-id="8461d-125">Example</span></span>

<span data-ttu-id="8461d-126">Hello následující příklad definuje třídu s názvem **ChannelOperations**.</span><span class="sxs-lookup"><span data-stu-id="8461d-126">hello following example defines a class called **ChannelOperations**.</span></span> <span data-ttu-id="8461d-127">Definice této třídy může být výchozím bodem pro – třída definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="8461d-127">This class definition could be a starting point for your web service class definition.</span></span> <span data-ttu-id="8461d-128">Pro jednoduchost, hello následující příklady používají hello bez asynchronních verzích metody.</span><span class="sxs-lookup"><span data-stu-id="8461d-128">For simplicity, hello following examples use hello non-async versions of methods.</span></span>

<span data-ttu-id="8461d-129">Hello příklad také ukazuje, jak může klient hello použít tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="8461d-129">hello example also shows how hello client might use this class.</span></span>

### <a name="channeloperations-class-definition"></a><span data-ttu-id="8461d-130">Definice třídy ChannelOperations</span><span class="sxs-lookup"><span data-stu-id="8461d-130">ChannelOperations class definition</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// hello ChannelOperations class only implements 
    /// hello Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        public ChannelOperations()
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
        }

        /// <summary>  
        /// Initiates hello creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name toobe given toohello new channel</param>  
        /// <returns>  
        /// Operation Id for hello long running operation being executed by Media Services. 
        /// Use this operation Id toopoll for hello channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });

            return operation.Id;
        }

        /// <summary> 
        /// Checks if hello operation has been completed. 
        /// If hello operation succeeded, hello created channel Id is returned in hello out parameter.
        /// </summary> 
        /// <param name="operationId">hello operation Id.</param> 
        /// <param name="channel">
        /// If hello operation succeeded, 
        /// hello created channel Id is returned in hello out parameter.</param>
        /// <returns>Returns false if hello operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle hello failure. 
                    // For example, throw an exception. 
                    // Use hello following information in hello exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }

        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

### <a name="hello-client-code"></a><span data-ttu-id="8461d-131">Kód klienta Hello</span><span class="sxs-lookup"><span data-stu-id="8461d-131">hello client code</span></span>
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have hello newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a><span data-ttu-id="8461d-132">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="8461d-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="8461d-133">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="8461d-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

