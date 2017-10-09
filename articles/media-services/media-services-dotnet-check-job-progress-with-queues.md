---
title: "aaaUse Azure Queue storage toomonitor Media Services úlohy oznámení s .NET | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Queue storage toomonitor Media Services úlohy oznámení. Ukázka kódu Hello je napsána v jazyce C# a používá hello sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: f535d0b5-f86c-465f-81c6-177f4f490987
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/14/2017
ms.author: juliako
ms.openlocfilehash: e4068621ada00d763133dc0d01cfc666b53f8b1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-queue-storage-toomonitor-media-services-job-notifications-with-net"></a>Azure Queue storage toomonitor Media Services úlohy oznámení pomocí rozhraní .NET
Při spuštění úlohy kódování, často vyžadují průběh úlohy tootrack způsobem. Je možné nakonfigurovat oznámení toodeliver Media Services příliš[Azure Queue storage](../storage/storage-dotnet-how-to-use-queues.md). Průběh úlohy můžete sledovat pomocí získávání oznámení z hello Queue storage. 

Zpráv doručených tooQueue úložiště je přístupná z kdekoli v hello, world. Hello architektura zasílání zpráv fronty úložiště je spolehlivé a vysoce škálovatelná. Dotazování Queue storage pro zprávy, se doporučuje namísto použití jiných metod.

Jednou z běžný scénář pro naslouchání služby oznámení tooMedia je, pokud vyvíjíte systém správy obsahu, které je tooperform dokončení některé další úlohy po kódování úlohy (například tootrigger hello dalším krokem v procesu pracovního postupu, nebo toopublish obsah).

Toto téma ukazuje, jak oznámení tooget zpráv z fronty úložiště.  

## <a name="considerations"></a>Požadavky
Při vývoji aplikací Media Services, které používají fronty úložiště, zvažte následující hello:

* Fronty úložiště neposkytuje zárukou toho first-in-first-out (FIFO) seřazené doručení. Další informace najdete v tématu [fronty Azure a Azure Service Bus fronty porovnání a Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
* Fronty úložiště není nabízecí služby. Máte toopoll hello fronty.
* Může mít libovolný počet front. Další informace najdete v tématu [rozhraní API REST služby fronty](https://docs.microsoft.com/rest/api/storageservices/Queue-Service-REST-API).
* Fronty úložiště má určitá omezení a specifika toobe vědět. Tyto možnosti jsou popsány v [fronty Azure a Azure Service Bus fronty porovnání a Contrasted](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted).

## <a name="net-code-example"></a>Příklad kódu rozhraní .NET

Příklad kódu Hello v této části hello následující:

1. Definuje hello **EncodingJobMessage** třídu, která mapuje toohello formát zprávy oznámení. Kód Hello deserializuje zprávy přijaté z fronty hello do objektů hello **EncodingJobMessage** typu.
2. Načítání hello Media Services a informace o účtu úložiště ze souboru app.config hello. Příklad kódu Hello používá tato informace toocreate hello **CloudMediaContext** a **CloudQueue** objekty.
3. Vytvoří frontu hello, která přijímá zprávy s oznámením o hello kódování úlohy.
4. Vytvoří hello oznámení, že koncový bod, který je namapovaný toohello fronty.
5. Připojí úlohu toohello koncového bodu oznámení hello a předá hello úlohy kódování. Může mít několik oznámení koncové body, které jsou připojené tooa úlohy.
6. Předává **NotificationJobState.FinalStatesOnly** toohello **AddNew** metoda. (V tomto příkladu jsme jsou jenom zájem o poslední stavy hello úlohy zpracování).

        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
7. Pokud předáte **NotificationJobState.All**, získáte všechny hello následující oznámení změny stavu: zařazených do fronty, naplánované, zpracování a dokončení. Ale jak bylo uvedeno dříve, Queue storage nezaručuje doručení. tooorder zprávy, použijte hello **časové razítko** vlastnosti (definovaná v hello **EncodingJobMessage** typu v následujícím příkladu hello). Je možné, duplicitní zprávy. toocheck duplicity, použijte hello **vlastnost ETag** (definované na hello **EncodingJobMessage** typ). Je také možné, že některé oznámení změny stavu získat přeskočit.
8. Počká pro hello úlohy tooget toohello dokončení stavu kontrolou hello fronty každých 10 sekund. Odstraní zprávy po jejich zpracování.
9. Odstraní frontu hello a hello oznámení koncového bodu.

> [!NOTE]
> Hello toomonitor doporučeným způsobem, stav úlohy je pomocí naslouchání toonotification zprávy, jak ukazuje následující příklad hello.
>
> Alternativně můžete zkontrolovat stav úlohy s použitím hello **IJob.State** vlastnost.  Oznámení o dokončení úlohy může doručení před hello stavu do **IJob** je nastaven příliš**dokončeno**. Hello **IJob.State** vlastnost odráží hello přesného stavu se ke krátké prodlevě.
>
>

### <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

1. Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 
2. Vytvořte novou složku (složka může být kdekoli na místní disk) a zkopírujte soubor .mp4 má tooencode a datový proud nebo progresivně stahovat. V tomto příkladu se používá hello "C:\Media" cesta.

### <a name="code"></a>Kód

```
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Collections.Generic;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;
using System.Runtime.Serialization.Json;

namespace JobNotification
{
    public class EncodingJobMessage
    {
        // MessageVersion is used for version control.
        public String MessageVersion { get; set; }

        // Type of hello event. Valid values are
        // JobStateChange and NotificationEndpointRegistration.
        public String EventType { get; set; }

        // ETag is used toohelp hello customer detect if
        // hello message is a duplicate of another message previously sent.
        public String ETag { get; set; }

        // Time of occurrence of hello event.
        public String TimeStamp { get; set; }

        // Collection of values specific toohello event.

        // For hello JobStateChange event hello values are:
        //     JobId - Id of hello Job that triggered hello notification.
        //     NewState- hello new state of hello Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
        //     OldState- hello old state of hello Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished

        // For hello NotificationEndpointRegistration event hello values are:
        //     NotificationEndpointId- Id of hello NotificationEndpoint
        //          that triggered hello notification.
        //     State- hello state of hello Endpoint.
        //          Valid values are: Registered and Unregistered.

        public IDictionary<string, object> Properties { get; set; }
    }

    class Program
    {

        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
        private static readonly string _StorageConnectionString = 
            ConfigurationManager.AppSettings["StorageConnectionString"];

        private static CloudMediaContext _context = null;
        private static CloudQueue _queue = null;
        private static INotificationEndPoint _notificationEndPoint = null;

        private static readonly string _singleInputMp4Path =
            Path.GetFullPath(@"C:\Media\BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            string endPointAddress = Guid.NewGuid().ToString();

            // Create hello context.
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Create hello queue that will be receiving hello notification messages.
            _queue = CreateQueue(_StorageConnectionString, endPointAddress);

            // Create hello notification point that is mapped toohello queue.
            _notificationEndPoint =
                    _context.NotificationEndPoints.Create(
                    Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);


            if (_notificationEndPoint != null)
            {
                IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                WaitForJobToReachedFinishedState(job.Id);
            }

            // Clean up.
            _queue.Delete();
            _notificationEndPoint.Delete();
        }


        static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);

            // Create hello queue client
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

            // Retrieve a reference tooa queue
            CloudQueue queue = queueClient.GetQueueReference(endPointAddress);

            // Create hello queue if it doesn't already exist
            queue.CreateIfNotExists();

            return queue;
        }


        public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("My MP4 tooSmooth Streaming encoding job");

            //Create an encrypted asset and upload hello mp4.
            IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted,
                inputMediaFilePath);

            // Get a media processor reference, and pass tooit hello name of the
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello conversion details, using a configuration file.
            ITask task = job.Tasks.AddNew("My encoding Task",
                processor,
                "Adaptive Streaming",
                Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Add a notification point toohello job. You can add multiple notification points.  
            job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly,
                _notificationEndPoint);

            job.Submit();

            return job;
        }

        public static void WaitForJobToReachedFinishedState(string jobId)
        {
            int expectedState = (int)JobState.Finished;
            int timeOutInSeconds = 600;

            bool jobReachedExpectedState = false;
            DateTime startTime = DateTime.Now;
            int jobState = -1;

            while (!jobReachedExpectedState)
            {
                // Specify how often you want tooget messages from hello queue.
                Thread.Sleep(TimeSpan.FromSeconds(10));

                foreach (var message in _queue.GetMessages(10))
                {
                    using (Stream stream = new MemoryStream(message.AsBytes))
                    {
                        DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                        settings.UseSimpleDictionaryFormat = true;
                        DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                        EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);

                        Console.WriteLine();

                        // Display hello message information.
                        Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                        Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                        Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                        Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                        foreach (var property in encodingJobMsg.Properties)
                        {
                            Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                        }

                        // We are only interested in messages
                        // where EventType is "JobStateChange".
                        if (encodingJobMsg.EventType == "JobStateChange")
                        {
                            string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                            if (JobId == jobId)
                            {
                                string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                string newJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "NewState").FirstOrDefault().Value;

                                JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);

                                if (newJobState == (JobState)expectedState)
                                {
                                    Console.WriteLine("job with Id: {0} reached expected state: {1}",
                                        jobId, newJobState);
                                    jobReachedExpectedState = true;
                                    break;
                                }
                            }
                        }
                    }
                    // Delete hello message after we've read it.
                    _queue.DeleteMessage(message);
                }

                // Wait until timeout
                TimeSpan timeDiff = DateTime.Now - startTime;
                bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                if (timedOut)
                {
                    Console.WriteLine(@"Timeout for checking job notification messages,
                                        latest found state ='{0}', wait time = {1} secs",
                        jobState,
                        timeDiff.TotalSeconds);

                    throw new TimeoutException();
                }
            }
        }

        static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(),
                assetCreationOptions);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);
            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading of {0}", assetFile.Name);

            return asset;
        }

        static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```
předchozí příklad vytváří hello následující výstup Hello. Vaše hodnoty se budou lišit.

    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4

    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35

    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 tooSmooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected
    State: Finished


## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
