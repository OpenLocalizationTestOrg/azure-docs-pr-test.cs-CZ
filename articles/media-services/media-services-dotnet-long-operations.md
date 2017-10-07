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
# <a name="delivering-live-streaming-with-azure-media-services"></a>Doručování živé streamování pomocí služby Azure Media Services

## <a name="overview"></a>Přehled

Microsoft Azure Media Services nabízí rozhraní API, která posílají požadavky na tooMedia služby toostart operace (například: vytvoření, spuštění, zastavení nebo odstranit kanál). Tyto operace jsou časově náročné.

Hello sady Media Services .NET SDK poskytuje rozhraní API, která odeslání požadavku hello a počkejte hello operaci toocomplete (interně hello, které rozhraní API se dotazuje na průběh operace v některé intervalech). Například při volání kanálu. Start(), vrátí metoda hello po spuštění hello kanálu. Můžete také použít asynchronní verze hello: await kanál. StartAsync() (informace o asynchronní vzor založený na úlohách najdete v tématu [klepněte na](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). Rozhraní API, které odeslat žádost o operaci a poté dotazování na stav hello až do dokončení operace hello se nazývají "dotazování metody". Tyto metody (zejména hello asynchronní verze) se doporučují pro aplikacemi rich client nebo stavové služby.

Existují scénáře, kde aplikace nemůže čekat na dlouho spuštěný požadavek http a chce toopoll pro průběh operace hello ručně. Typickým příkladem by prohlížeč interakci s bezstavové webové služby: Pokud prohlížeč hello požaduje toocreate kanál, hello webová služba spustí dlouhotrvající operace a vrátí hello prohlížeče toohello ID operace. Prohlížeč Hello může pak požádejte hello webové služby tooget hello stav operace na základě ID hello. Hello sady Media Services .NET SDK poskytuje rozhraní API, které jsou užitečné pro tento scénář. Tato rozhraní API se nazývají "-cyklického dotazování metody".
"Hello-cyklického dotazování metody" mají následující vzoru pro pojmenovávání hello: Odeslat*OperationName*operace (například SendCreateOperation). Odeslat*OperationName*operaci metody vrací hello **IOperation** objektu; hello vráceného objektu obsahuje informace, které lze použít tootrack hello operaci. Hello odesílání*OperationName*OperationAsync metody vrací **úloh<IOperation>**.

V současné době hello následující třídy podporu-cyklického dotazování metody: **kanál**, **StreamingEndpoint**, a **programu**.

toopoll pro hello stav operace, použijte hello **GetOperation** metodu hello **OperationBaseCollection** třídy. Použijte následující stav operace hello toocheck intervaly hello: pro **kanál** a **StreamingEndpoint** operace, použijte 30 sekund; pro **Program** operace, použijte 10 sekund.

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).

## <a name="example"></a>Příklad

Hello následující příklad definuje třídu s názvem **ChannelOperations**. Definice této třídy může být výchozím bodem pro – třída definice webové služby. Pro jednoduchost, hello následující příklady používají hello bez asynchronních verzích metody.

Hello příklad také ukazuje, jak může klient hello použít tuto třídu.

### <a name="channeloperations-class-definition"></a>Definice třídy ChannelOperations

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

### <a name="hello-client-code"></a>Kód klienta Hello
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



## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

