---
title: "aaaOptimizing vaše Azure kódu v sadě Visual Studio | Microsoft Docs"
description: "Další informace o tom, jak Azure kódu optimalizace nástroje v sadě Visual Studio pomáhat kódu robustnější a lépe provádění."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 7df932def9dc16c93de29fc6a77c8fc121fda338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-your-azure-code"></a>Optimalizace Azure kódu
Pokud jste programování aplikací, které používají Microsoft Azure, nejsou některé postupy kódování, postupujte podle toohelp vyhnout problémům s aplikace škálovatelnost, chování a výkonu v cloudovém prostředí. Společnost Microsoft poskytuje nástroj pro analýzu kódu Azure, rozpozná a identifikuje některé z těchto problémů obvykle došlo a můžete je vyřešit. Můžete si stáhnout nástroj hello v sadě Visual Studio prostřednictvím balíčku NuGet.

## <a name="azure-code-analysis-rules"></a>Azure pravidel analýzy kódu
Nástroj pro analýzu kódu Azure Hello používá následující pravidla tooautomatically příznak Azure kódu, pokud najde známé problémy výkonových hello. Zjistil problémy se zobrazují jako upozornění nebo chyby kompilátoru. Kód opravy nebo návrhy tooresolve hello upozornění nebo chyby jsou často zajišťováno prostřednictvím ikonou žárovky.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Nepoužívejte výchozí režim stavu relace (v procesu)
### <a name="id"></a>ID
AP0000

### <a name="description"></a>Popis
Pokud používáte hello výchozí režim stavu relace (v procesu) pro cloudové aplikace, může dojít ke ztrátě stavu relace.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Ve výchozím nastavení je režim stavu relace hello zadaný v souboru web.config hello v procesu. Navíc pokud žádný záznam, zadaný v konfiguračním souboru hello, výchozí režim stavu relace hello tooin procesu. režim v procesu Hello ukládá stav relace v paměti na webovém serveru hello. Při restartování instance nebo novou instanci se používá pro vyrovnávání zatížení nebo podporu převzetí služeb při selhání, stav relace hello uložené v paměti na webovém serveru hello se neuloží. Tato situace zabraňuje aplikace hello z důvodu škálovatelné cloudové hello.

Stavu relace ASP.NET podporuje několik různých možností ukládání dat stavu relace: InProc, StateServer, SQL Server, vlastní a vypnutí. Doporučuje se použít vlastní režimu toohost data na externí úložiště stavu relace, například [poskytovatele stavu relace Azure Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Řešení
Jeden doporučené řešení je stav relace toostore mezipaměti spravované služby. Zjistěte, jak toouse [poskytovatele stavu relace Azure Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore vašemu stavu relace. Vám může také úložiště relace stavu v jiných místech tooensure, že je v cloudu hello škálovatelné aplikace. Další informace o alternativní řešení přečtěte si prosím toolearn [režim stavu relace](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Metoda spouštění by neměl být asynchronní
### <a name="id"></a>ID
AP1000

### <a name="description"></a>Popis
Vytváření asynchronních metod (například [await](https://msdn.microsoft.com/library/hh156528.aspx)) mimo hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda a pak volání hello asynchronní metody z [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklarování hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) jako asynchronní metodu způsobí, že pracovní hello role tooenter smyčku restartování.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Volání asynchronních metod uvnitř hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda způsobí, že hello cloudové služby modulu runtime toorecycle hello role pracovního procesu. Když se spustí roli pracovního procesu, všechny spuštění programu probíhá uvnitř hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda. Existujícímu hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda způsobí, že pracovní hello role toorestart. Pokud se modul runtime role pracovního procesu hello dotkne hello asynchronní metody, odešle zprávu všechny operace po hello asynchronní metody a vrátí. To způsobí, že pracovní hello role tooexit z hello [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda a restartování. V hello další iterace provádění role pracovního procesu hello přístupy hello asynchronní metody znovu a restartuje, způsobuje hello pracovní role toorecycle znovu také.

### <a name="solution"></a>Řešení
Umístit všechny asynchronní operace mimo hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda. Potom zavolejte hello rozdělili asynchronní metody z uvnitř hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metody, jako je .wait RunAsync (). Nástroj pro analýzu kódu Azure Hello můžete tento problém vyřešit.

Hello následující fragment kódu ukazuje hello kód opravu tohoto problému:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Ověřování služby sběrnice sdíleného přístupového podpisu
### <a name="id"></a>ID
AP2000

### <a name="description"></a>Popis
Pomocí sdíleného přístupového podpisu (SAS) pro ověřování. Služby Řízení přístupu (ACS) je zastaralá pro ověřování sběrnice služby.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Pro zvýšení zabezpečení Azure Active Directory nahrazuje ověřování služby ACS se ověřování SAS. V tématu [Azure Active Directory je hello budoucí ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) informace o plánu přechodu hello.

### <a name="solution"></a>Řešení
Používejte ověřování SAS ve svých aplikacích. Hello následující příklad ukazuje, jak sběrnici toouse existující SAS token tooaccess služba obor názvů nebo entity.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

V tématu hello následující témata pro další informace.

* Přehled najdete v tématu [ověřování sdíleného přístupového podpisu službou Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)
* [Jak toouse ověřování podpisu sdíleného přístupu se Service Bus](https://msdn.microsoft.com/library/dn205161.aspx)
* Ukázkový projekt, najdete v části [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-tooavoid-receive-loop"></a>Zvažte použití metody tooavoid OnMessage "přijímat smyčka"
### <a name="id"></a>ID
AP2002

### <a name="description"></a>Popis
tooavoid přejdete do "přijímat smyčky," volání hello **OnMessage** metoda je lepší řešení pro příjem zprávy než volajícím hello **Receive** metoda. Ale pokud je nutné použít hello **Receive** metodu a zadejte čas čekání serveru jiné než výchozí, zkontrolujte, že doba čekání serveru hello je více než jedna minuta.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Při volání metody **OnMessage**, hello klient spustí vnitřní zpráva některé čerpadlo, který neustále dotazuje hello fronty nebo předplatné. Tato zpráva čerpadlo obsahuje nekonečnou smyčku, která vydává volání tooreceive zprávy. Pokud vyprší časový limit volání hello, vydá nové volání. interval vypršení časového limitu Hello je dáno hello hodnotu hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) vlastnost hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)který se používá.

Výhodou použití Hello **OnMessage** porovnání příliš**Receive** je, že uživatelé nemají toomanually dotazování pro zprávy, zpracování výjimek, zpracování více zpráv paralelně a dokončení hello zprávy.

Když zavoláte **Receive** bez použití jeho výchozí hodnotu, se že hello *ServerWaitTime* hodnota je více než jedna minuta. Nastavení *ServerWaitTime* vyprší dřív, než je plně přijat uvítací zprávu hello server zabrání toomore než jedna minuta.

### <a name="solution"></a>Řešení
Podrobnosti viz následující příklady kódu pro použití, doporučené hello. Další podrobnosti najdete v tématu [QueueClient.OnMessage – metoda (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)a [QueueClient.Receive – metoda (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

výkon hello tooimprove hello infrastrukturu zasílání zpráv Azure, najdete v části vzoru návrhu hello [asynchronní zasílání zpráv Úvod do](https://msdn.microsoft.com/library/dn589781.aspx).

Hello následuje příklad použití **OnMessage** tooreceive zprávy.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if hello message-pump should call complete on messages after hello callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates hello maximum number of concurrent calls toohello callback hello pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you tooget notified of any errors encountered by hello message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates hello message pump and callback is invoked for each message that is recieved, calling close on hello client will stop hello pump.
    {
        // Process hello message.
    }, options);
    Console.WriteLine("Press any key tooexit.");
    Console.ReadKey();
```

Hello následuje příklad použití **Receive** doba čekání hello výchozí server.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Hello následuje příklad použití **Receive** serveru jiné než výchozí doba čekání.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Zvažte použití asynchronních metod Service Bus
### <a name="id"></a>ID
AP2003

### <a name="description"></a>Popis
Použijte asynchronní výkonu tooimprove metody Service Bus s zprostředkované zasílání zpráv.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Použití asynchronních metod umožňuje souběžnosti program aplikace, protože provádění jednotlivých volání neblokuje hello hlavního vlákna. Při použití služby Service Bus metody pro zasílání zpráv, provádění operace (odesílání, příjem, odstranit, apod) trvá určitou dobu. Tentokrát zahrnuje hello zpracování operace hello podle hello služby Service Bus v přidání toohello latenci dotazů a odpovědí hello hello. tooincrease hello počet operací za čas, operace musí být spuštěn současně. Další informace naleznete v příliš[osvědčené postupy pro výkon vylepšení pomocí Service Bus zprostředkované zasílání zpráv](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Řešení
V tématu [QueueClient – třída (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) informace o způsobu toouse hello doporučená asynchronní metody.

výkon hello tooimprove hello infrastrukturu zasílání zpráv Azure, najdete v části vzoru návrhu hello [asynchronní zasílání zpráv Úvod do](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Vezměte v úvahu rozdělení fronty Service Bus a témat
### <a name="id"></a>ID
AP2004

### <a name="description"></a>Popis
Oddíl fronty Service Bus a témat pro lepší výkon s zasílání zpráv Service Bus.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Vytváření oddílů fronty sběrnice a témata zvýšíte dostupnost výkonu propustnost a služby, protože hello celkovou propustnost oddílů fronta nebo téma je již omezena hello výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv. Kromě toho dočasnému výpadku zasílání zpráv úložiště není znepřístupnit oddílů fronta nebo téma. Další informace najdete v tématu [dělení entity zasílání zpráv](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Řešení
Následující kód fragment kódu ukazuje, jak Hello toopartition entity pro zasílání zpráv.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Další informace najdete v tématu [rozdělena na oddíly fronty služby Service Bus a témat | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) a podívejte se na hello [Microsoft Azure Service Bus rozdělena na oddíly fronty](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) ukázka.

## <a name="do-not-set-sharedaccessstarttime"></a>Nenastavujte SharedAccessStartTime
### <a name="id"></a>ID
AP3001

### <a name="description"></a>Popis
Byste neměli používat SharedAccessStartTimeset toohello aktuální čas tooimmediately spustit hello zásady sdíleného přístupu. Potřebujete jenom tooset tuto vlastnost Pokud chcete zásady sdíleného přístupu toostart hello později.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Synchronizace hodin způsobí, že mírné časový rozdíl mezi datovými centry. Například by logicky domníváte, že čas zahájení hello nastavení úložiště SAS zásady jako hello aktuální čas pomocí DateTime.Now nebo podobné metody způsobí, že hello SAS zásad tootake vliv okamžitě. Hello mírné časové rozdíly mezi datovými centry však může způsobit problémy s tím, vzhledem k tomu, že některé datacenter časy může být mírně pozdější než čas zahájení hello, zatímco jiní jej před jeho. V důsledku toho můžete rychle (nebo i okamžitě) vyprší platnost hello SAS zásad nastaveného hello zásady životnosti je příliš krátké.

Další informace o používání sdíleného přístupového podpisu na úložiště Azure najdete v části [představení tabulky SAS (sdíleného přístupového podpisu), fronta SAS a aktualizace tooBlob SAS – Blog týmu pro úložiště Azure Microsoft - lokality Domů - Blogy MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Řešení
Odeberte hello příkaz, který nastaví čas zahájení hello hello sdílených zásad přístupu. Nástroj pro analýzu kódu Azure Hello poskytuje opravu tohoto problému. Další informace o správu zabezpečení, najdete v tématu vzoru návrhu hello [vzor klíč osobní služby](https://msdn.microsoft.com/library/dn568102.aspx).

Hello následující fragment kódu ukazuje hello kód opravu tohoto problému.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Sdílené zásady přístupu čas vypršení platnosti musí být delší než 5 minut
### <a name="id"></a>ID
AP3002

### <a name="description"></a>Popis
Může být co nejvíce pět minut rozdíl v hodiny mezi datovými centry v různých umístěních kvůli tooa Stav známý jako "posun hodin." token tooprevent hello SAS zásady vypršení platnosti dříve, než plánované, nastavte toobe čas vypršení platnosti hello více než pět minut.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Datových center v různých umístěních kolem hello, world synchronizovat signál hodiny. Protože trvá dobu hodiny signál tootravel toodifferent umístění, může být čas odchylka mezi datovými centry v různých geografických umístěních i když všechno, co je zřejmě synchronizován. Tento časový rozdíl může ovlivnit hello sdíleného přístupu počáteční čas a vypršení platnosti interval zásad. Proto tooensure zásady sdíleného přístupu se okamžitě projeví, nezadávejte hello počáteční čas. Kromě toho zajistěte, aby byl čas vypršení platnosti hello více než 5 minut tooprevent časná vypršení časového limitu.

Další informace o používání sdíleného přístupového podpisu na úložiště Azure najdete v tématu [představení tabulky SAS (sdíleného přístupového podpisu), fronta SAS a aktualizace tooBlob SAS – Blog týmu pro úložiště Azure Microsoft - lokality Domů - Blogy MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Řešení
Další informace o správu zabezpečení, najdete v části vzoru návrhu hello [vzor klíč osobní služby](https://msdn.microsoft.com/library/dn568102.aspx).

Hello tady je příklad není zadávání čas spuštění zásady sdíleného přístupu.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Hello následuje příklad zadání čas spuštění zásady sdíleného přístupu s doba vypršení platnosti zásada, která je větší než pět minut.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Další informace najdete v tématu [vytváření a používání sdíleného přístupového podpisu](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Použití CloudConfigurationManager
### <a name="id"></a>ID
AP4000

### <a name="description"></a>Popis
Pomocí hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) třídy pro projekty, jako je Azure web a mobilní služby Azure nebude znamenat problémy modulu runtime. Jako osvědčený postup je však vhodné toouse cloudu[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) jako jednotné způsob správy konfigurace pro všechny aplikace cloudu Azure.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
CloudConfigurationManager přečte hello konfigurační soubor odpovídající toohello aplikace prostředí.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Řešení
Váš kód toouse hello Refaktorovat [třída CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Nástroj pro analýzu kódu Azure hello zajišťuje opravy kódu pro tento problém.

Hello následující fragment kódu ukazuje hello kód opravu tohoto problému. Nahradit

`var settings = ConfigurationManager.AppSettings["mySettings"];`

S

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Tady je příklad, jak toostore hello nastavení konfigurace v souboru App.config nebo Web.config. Přidejte hello nastavení toohello oddílu appSettings hello konfiguračního souboru. Hello následuje hello souboru Web.config pro předchozí příklad kódu hello.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Nepoužívejte pevně připojovací řetězce
### <a name="id"></a>ID
AP4001

### <a name="description"></a>Popis
Pokud používáte pevně připojovací řetězce a potřebujete tooupdate je později, budete mít toomake změny tooyour zdrojového kódu a překompilování aplikace hello. Ale pokud uchováváte připojovací řetězce v konfiguračním souboru, můžete je později jednoduše aktualizací hello konfigurační soubor.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Pevně kódováno připojovací řetězce je chybný postup, protože by to zavedlo problémy, když potřebují toobe rychle změnit připojovací řetězce. Kromě toho pokud hello projektu potřebuje toobe zaškrtnutí v ovládacím prvku toosource, zavést pevně připojovací řetězce ohrožení zabezpečení, vzhledem k tomu, že řetězce hello lze zobrazit v hello zdrojového kódu.

### <a name="solution"></a>Řešení
Ukládání připojovacích řetězců hello konfigurační soubory nebo prostředí Azure.

* Pro samostatné aplikace použijte nastavení app.config toostore připojovacího řetězce.
* Pro hostované službou IIS webové aplikace použijte řetězce připojení s toostore souboru web.config.
* Pro aplikace ASP.NET vNext použijte configuration.json toostore připojovací řetězce.

Informace o používání soubory konfigurace, jako je soubor web.config nebo app.config najdete v tématu [pokyny pro ASP.NET Web konfigurace](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Informace o tom, jak Azure pracovní proměnné prostředí, najdete v části [weby Azure: fungování řetězců aplikace a připojovací řetězce fungovat](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Informace týkající se ukládání připojovací řetězec do správy zdrojového kódu najdete v tématu [neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložiště zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Použijte diagnostiku konfigurační soubor
### <a name="id"></a>ID
AP5000

### <a name="description"></a>Popis
Namísto konfigurace nastavení diagnostiky ve vašem kódu, například pomocí hello Microsoft.WindowsAzure.Diagnostics programovací rozhraní API, byste měli nakonfigurovat nastavení diagnostiky v souboru diagnostics.wadcfg hello. (Nebo, pokud používáte Azure SDK 2.5 diagnostics.wadcfgx). Díky tomu můžete změnit nastavení diagnostiky bez nutnosti toorecompile vašeho kódu.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Před 2.5 Azure SDK, (která používá Azure diagnostics 1.3), Azure Diagnostics (WAD) se daly konfigurovat pomocí několika různých metod: přidáním toohello konfigurace objektů blob v úložišti pomocí imperativní kódu, deklarativní konfigurace nebo výchozí hello konfigurace. Však hello upřednostňovaný způsob, jak tooconfigure diagnostiky je toouse konfiguračního souboru XML (diagnostics.wadcfg nebo diagnositcs.wadcfgx pro sadu SDK, 2.5 a novější) v projektu aplikace hello. V tomto přístupu hello diagnostics.wadcfg souboru úplně definuje konfiguraci hello a lze aktualizovat a znovu nasazena na bude. Kombinování hello použití hello diagnostics.wadcfg konfigurační soubor s hello programové metody nastavení konfigurace pomocí hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)nebo [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx) třídy může způsobit tooconfusion. V tématu [inicializovat nebo změna konfigurace diagnostiky Azure](https://msdn.microsoft.com/library/azure/hh411537.aspx) Další informace.

Počínaje WAD 1.3 (zahrnutá v Azure SDK 2.5), je již nebude možné toouse diagnostiky tooconfigure kódu. V důsledku toho můžete zadat jenom hello konfigurace při použití nebo aktualizaci rozšíření diagnostiky hello.

### <a name="solution"></a>Řešení
Použijte hello diagnostiky konfigurace návrháře toomove nastavení pro diagnostiku toohello diagnostiky konfigurační soubor (diagnositcs.wadcfg nebo diagnositcs.wadcfgx pro sadu SDK, 2.5 a novější). Doporučujeme také nainstalovat [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) a použít nejnovější funkce diagnostiky hello.

1. V místní nabídce hello hello role, které chcete tooconfigure vyberte vlastnosti a potom vyberte kartu Konfigurace hello.
2. V hello **diagnostiky** část, ujistěte se, že hello **povolení diagnostiky** je zaškrtnuté políčko.
3. Zvolte hello **konfigurace** tlačítko.

   ![Přístup k hello možnost povolení diagnostiky](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   V tématu [konfigurace diagnostiky pro Azure Cloud Services a virtuální počítače](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) Další informace.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vyhněte se deklarace objektů DbContext jako statické
### <a name="id"></a>ID
AP6000

### <a name="description"></a>Popis
paměť toosave, vyhněte se deklarace objektů DBContext jako statické.

Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod
Objekty DBContext podržte hello výsledků dotazu z každé volání. Statické DBContext objekty nebyly použity, dokud doménu aplikace hello je odpojen. Proto statický objekt DBContext spotřebovat velké objemy paměti.

### <a name="solution"></a>Řešení
Jako místní proměnné nebo pole instance nestatické deklarovat DBContext, použijte pro úlohy a nechat ji bude zrušen z po použití.

Hello následující třídy kontroleru MVC příklad ukazuje, jak toouse hello objekt DBContext.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass tooDbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Další kroky
toolearn informace o optimalizaci a řešení potíží s aplikací Azure, najdete v části [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
