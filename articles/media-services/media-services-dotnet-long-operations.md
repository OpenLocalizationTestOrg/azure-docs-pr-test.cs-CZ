---
title: "Dotazování dlouhotrvající operace | Microsoft Docs"
description: "Toto téma ukazuje, jak dlouho běžící operace dotazování."
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
ms.openlocfilehash: 7123a2d44d3b7c332afe30fb0fcea88ca29e313a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a><span data-ttu-id="2f6df-103">Doručování živé streamování pomocí služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="2f6df-103">Delivering Live Streaming with Azure Media Services</span></span>

## <a name="overview"></a><span data-ttu-id="2f6df-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2f6df-104">Overview</span></span>

<span data-ttu-id="2f6df-105">Microsoft Azure Media Services nabízí rozhraní API, která odesílat žádosti do Media Services spuštění operace (například: vytvoření, spuštění, zastavení nebo odstranit kanál).</span><span class="sxs-lookup"><span data-stu-id="2f6df-105">Microsoft Azure Media Services offers APIs that send requests to Media Services to start operations (for example: create, start, stop, or delete a channel).</span></span> <span data-ttu-id="2f6df-106">Tyto operace jsou časově náročné.</span><span class="sxs-lookup"><span data-stu-id="2f6df-106">These operations are long-running.</span></span>

<span data-ttu-id="2f6df-107">.NET SDK služby Media Services poskytuje rozhraní API, která odeslat požadavek a počkejte na dokončení operace (interně rozhraní API se dotazuje na průběh operace v některé intervalech).</span><span class="sxs-lookup"><span data-stu-id="2f6df-107">The Media Services .NET SDK provides APIs that send the request and wait for the operation to complete (internally, the APIs are polling for operation progress at some intervals).</span></span> <span data-ttu-id="2f6df-108">Například při volání kanálu. Start(), metoda vrátí po spuštění kanálu.</span><span class="sxs-lookup"><span data-stu-id="2f6df-108">For example, when you call channel.Start(), the method returns after the channel is started.</span></span> <span data-ttu-id="2f6df-109">Můžete také použít asynchronní verze: await kanál. StartAsync() (informace o asynchronní vzor založený na úlohách najdete v tématu [klepněte na](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span><span class="sxs-lookup"><span data-stu-id="2f6df-109">You can also use the asynchronous version: await channel.StartAsync() (for information about Task-based Asynchronous Pattern, see [TAP](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span></span> <span data-ttu-id="2f6df-110">Rozhraní API, které odeslat žádost o operaci a poté dotazování na stav, dokud se nedokončí operaci se nazývají "dotazování metody".</span><span class="sxs-lookup"><span data-stu-id="2f6df-110">APIs that send an operation request and then poll for the status until the operation is complete are called “polling methods”.</span></span> <span data-ttu-id="2f6df-111">Tyto metody (obzvláště verzi asynchronní) – se doporučují pro aplikacemi rich client nebo stavové služby.</span><span class="sxs-lookup"><span data-stu-id="2f6df-111">These methods (especially the Async version) are recommended for rich client applications and/or stateful services.</span></span>

<span data-ttu-id="2f6df-112">Existují scénáře, kde aplikace nemůže čekat na dlouho spuštěný požadavek http a chce pro dotazování na průběh operace ručně.</span><span class="sxs-lookup"><span data-stu-id="2f6df-112">There are scenarios where an application cannot wait for a long running http request and wants to poll for the operation progress manually.</span></span> <span data-ttu-id="2f6df-113">Typickým příkladem by prohlížeč interakci s bezstavové webové služby: Pokud v prohlížeči požádá o vytvoření kanálu, webovou službu inicializuje dlouhotrvající operace a vrátí ID operace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2f6df-113">A typical example would be a browser interacting with a stateless web service: when the browser requests to create a channel, the web service initiates a long running operation and returns the operation ID to the browser.</span></span> <span data-ttu-id="2f6df-114">Prohlížeč může pak požádejte webové služby a získat stav operace podle ID.</span><span class="sxs-lookup"><span data-stu-id="2f6df-114">The browser could then ask the web service to get the operation status based on the ID.</span></span> <span data-ttu-id="2f6df-115">.NET SDK služby Media Services poskytuje rozhraní API, které jsou užitečné pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="2f6df-115">The Media Services .NET SDK provides APIs that are useful for this scenario.</span></span> <span data-ttu-id="2f6df-116">Tato rozhraní API se nazývají "-cyklického dotazování metody".</span><span class="sxs-lookup"><span data-stu-id="2f6df-116">These APIs are called “non-polling methods”.</span></span>
<span data-ttu-id="2f6df-117">"-Cyklického dotazování metody" mají následující vzoru pro pojmenovávání: Odeslat*OperationName*operace (například SendCreateOperation).</span><span class="sxs-lookup"><span data-stu-id="2f6df-117">The “non-polling methods” have the following naming pattern: Send*OperationName*Operation (for example, SendCreateOperation).</span></span> <span data-ttu-id="2f6df-118">Odeslat*OperationName*vrátit operaci metody **IOperation** objekt; vrácený objekt obsahuje informace, které můžete použít ke sledování operaci.</span><span class="sxs-lookup"><span data-stu-id="2f6df-118">Send*OperationName*Operation methods return the **IOperation** object; the returned object contains information that can be used to track the operation.</span></span> <span data-ttu-id="2f6df-119">Odesílání*OperationName*OperationAsync metody vrací **úloh<IOperation>**.</span><span class="sxs-lookup"><span data-stu-id="2f6df-119">The Send*OperationName*OperationAsync methods return **Task<IOperation>**.</span></span>

<span data-ttu-id="2f6df-120">Následující třídy v současné době podporují metody cyklického dotazování: **kanál**, **StreamingEndpoint**, a **programu**.</span><span class="sxs-lookup"><span data-stu-id="2f6df-120">Currently, the following classes support non-polling methods:  **Channel**, **StreamingEndpoint**, and **Program**.</span></span>

<span data-ttu-id="2f6df-121">Dotazování na stav operace, použijte **GetOperation** metodu **OperationBaseCollection** třídy.</span><span class="sxs-lookup"><span data-stu-id="2f6df-121">To poll for the operation status, use the **GetOperation** method on the **OperationBaseCollection** class.</span></span> <span data-ttu-id="2f6df-122">Zkontrolujte stav operace pomocí následující intervaly: pro **kanál** a **StreamingEndpoint** operace, použijte 30 sekund; pro **Program** operace, použijte 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="2f6df-122">Use the following intervals to check the operation status: for **Channel** and **StreamingEndpoint** operations, use 30 seconds; for **Program** operations, use 10 seconds.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="2f6df-123">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="2f6df-123">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="2f6df-124">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="2f6df-124">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span>

## <a name="example"></a><span data-ttu-id="2f6df-125">Příklad</span><span class="sxs-lookup"><span data-stu-id="2f6df-125">Example</span></span>

<span data-ttu-id="2f6df-126">Následující příklad definuje třídu s názvem **ChannelOperations**.</span><span class="sxs-lookup"><span data-stu-id="2f6df-126">The following example defines a class called **ChannelOperations**.</span></span> <span data-ttu-id="2f6df-127">Definice této třídy může být výchozím bodem pro – třída definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="2f6df-127">This class definition could be a starting point for your web service class definition.</span></span> <span data-ttu-id="2f6df-128">Pro jednoduchost použijte následující příklady verze bez asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="2f6df-128">For simplicity, the following examples use the non-async versions of methods.</span></span>

<span data-ttu-id="2f6df-129">Tento příklad také ukazuje, jak může klient použít tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="2f6df-129">The example also shows how the client might use this class.</span></span>

### <a name="channeloperations-class-definition"></a><span data-ttu-id="2f6df-130">Definice třídy ChannelOperations</span><span class="sxs-lookup"><span data-stu-id="2f6df-130">ChannelOperations class definition</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
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
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
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
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
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

### <a name="the-client-code"></a><span data-ttu-id="2f6df-131">Kód klienta</span><span class="sxs-lookup"><span data-stu-id="2f6df-131">The client code</span></span>
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a><span data-ttu-id="2f6df-132">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="2f6df-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2f6df-133">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="2f6df-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

