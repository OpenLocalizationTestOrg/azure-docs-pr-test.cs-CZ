---
title: "aaaGet začít s fronty Azure Service Bus | Microsoft Docs"
description: "Napíšeme konzolovou aplikaci C# využívající fronty zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.openlocfilehash: eaa362ab0eabd2427977398c1deab5dc00105ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-queues"></a><span data-ttu-id="b76db-103">Začínáme s frontami služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="b76db-103">Get started with Service Bus queues</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="b76db-104">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="b76db-104">What will be accomplished</span></span>
<span data-ttu-id="b76db-105">Tento kurz se zabývá hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76db-105">This tutorial covers hello following steps:</span></span>

1. <span data-ttu-id="b76db-106">Vytvořte obor názvů Service Bus pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b76db-106">Create a Service Bus namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="b76db-107">Vytvoření fronty Service Bus, pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b76db-107">Create a Service Bus queue, using hello Azure portal.</span></span>
3. <span data-ttu-id="b76db-108">Zápis konzoly aplikace toosend zprávy.</span><span class="sxs-lookup"><span data-stu-id="b76db-108">Write a console application toosend a message.</span></span>
4. <span data-ttu-id="b76db-109">Zápis konzoly aplikace tooreceive hello zprávy odeslané v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="b76db-109">Write a console application tooreceive hello messages sent in hello previous step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b76db-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b76db-110">Prerequisites</span></span>
1. <span data-ttu-id="b76db-111">[Visual Studio 2015 nebo vyšší](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="b76db-111">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="b76db-112">Hello příklady v tomto kurzu použít Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b76db-112">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="b76db-113">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b76db-113">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="b76db-114">1. Vytvoření oboru názvů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b76db-114">1. Create a namespace using hello Azure portal</span></span>
<span data-ttu-id="b76db-115">Pokud jste již vytvořili obor názvů zasílání zpráv Service Bus, přeskočit toohello [vytvoření fronty pomocí portálu Azure hello](#2-create-a-queue-using-the-azure-portal) části.</span><span class="sxs-lookup"><span data-stu-id="b76db-115">If you've already created a Service Bus Messaging namespace, jump toohello [Create a queue using hello Azure portal](#2-create-a-queue-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-hello-azure-portal"></a><span data-ttu-id="b76db-116">2. Vytvoření fronty pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b76db-116">2. Create a queue using hello Azure portal</span></span>
<span data-ttu-id="b76db-117">Pokud jste již vytvořili fronty Service Bus, přeskočit toohello [toohello fronty pro odesílání zpráv](#3-send-messages-to-the-queue) části.</span><span class="sxs-lookup"><span data-stu-id="b76db-117">If you have already created a Service Bus queue, jump toohello [Send messages toohello queue](#3-send-messages-to-the-queue) section.</span></span>

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-toohello-queue"></a><span data-ttu-id="b76db-118">3. Odesílat zprávy fronty toohello</span><span class="sxs-lookup"><span data-stu-id="b76db-118">3. Send messages toohello queue</span></span>
<span data-ttu-id="b76db-119">Fronta zpráv toohello toosend, jsme zápisu konzolovou aplikaci C# pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b76db-119">toosend messages toohello queue, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="b76db-120">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="b76db-120">Create a console application</span></span>

<span data-ttu-id="b76db-121">Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b76db-121">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-hello-service-bus-nuget-package"></a><span data-ttu-id="b76db-122">Přidání balíčku Service Bus NuGet hello</span><span class="sxs-lookup"><span data-stu-id="b76db-122">Add hello Service Bus NuGet package</span></span>
1. <span data-ttu-id="b76db-123">Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b76db-123">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="b76db-124">Klikněte na tlačítko hello **Procházet** kartě, vyhledejte **Microsoft Azure Service Bus**a potom vyberte hello **WindowsAzure.ServiceBus** položky.</span><span class="sxs-lookup"><span data-stu-id="b76db-124">Click hello **Browse** tab, search for **Microsoft Azure Service Bus**, and then select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="b76db-125">Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b76db-125">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-queue"></a><span data-ttu-id="b76db-127">Zápis některých kódu toosend toohello fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b76db-127">Write some code toosend a message toohello queue</span></span>
1. <span data-ttu-id="b76db-128">Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="b76db-128">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="b76db-129">Přidejte následující kód toohello hello `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="b76db-129">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="b76db-130">Sada hello `connectionString` proměnné toohello připojovací řetězec, který jste získali při vytváření názvů hello a nastavte `queueName` toohello název fronty, který jste použili při vytvoření fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b76db-130">Set hello `connectionString` variable toohello connection string that you obtained when creating hello namespace, and set `queueName` toohello queue name that you used when creating hello queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="b76db-131">Soubor Program.cs by měl vypadat takhle.</span><span class="sxs-lookup"><span data-stu-id="b76db-131">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="b76db-132">Spuštění programu hello a zkontrolujte hello portálu Azure: klikněte na název hello fronty v oboru názvů hello **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="b76db-132">Run hello program, and check hello Azure portal: click hello name of your queue in hello namespace **Overview** blade.</span></span> <span data-ttu-id="b76db-133">fronty Hello **Essentials** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="b76db-133">hello queue **Essentials** blade is displayed.</span></span> <span data-ttu-id="b76db-134">Všimněte si, že hello **počet zpráv aktivní** hodnotu by teď měly být 1.</span><span class="sxs-lookup"><span data-stu-id="b76db-134">Notice that hello **Active Message Count** value should now be 1.</span></span> <span data-ttu-id="b76db-135">Pokaždé, když spustíte aplikace sender hello bez načítání zpráv hello, tato hodnota se zvýší o 1.</span><span class="sxs-lookup"><span data-stu-id="b76db-135">Each time you run hello sender application without retrieving hello messages, this value increases by 1.</span></span> <span data-ttu-id="b76db-136">Všimněte si, že hello aktuální velikost fronty hello zvýší každé aplikace hello čas také přidá toohello front zpráv.</span><span class="sxs-lookup"><span data-stu-id="b76db-136">Also note that hello current size of hello queue increments each time hello app adds a message toohello queue.</span></span>
   
      ![Velikost zpráv][queue-message]

## <a name="4-receive-messages-from-hello-queue"></a><span data-ttu-id="b76db-138">4. Příjem zpráv z fronty hello</span><span class="sxs-lookup"><span data-stu-id="b76db-138">4. Receive messages from hello queue</span></span>

1. <span data-ttu-id="b76db-139">tooreceive hello zprávy, které jste právě zaslali, vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček Service Bus NuGet toohello, podobně jako toohello předchozí odesílatele aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b76db-139">tooreceive hello messages you just sent, create a new console application and add a reference toohello Service Bus NuGet package, similar toohello previous sender application.</span></span>
2. <span data-ttu-id="b76db-140">Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="b76db-140">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="b76db-141">Přidejte následující kód toohello hello `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="b76db-141">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="b76db-142">Sada hello `connectionString` proměnné toohello připojovací řetězec, který byl získán při vytvoření oboru názvů hello a nastavte `queueName` toohello název fronty, který jste použili při vytvoření fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b76db-142">Set hello `connectionString` variable toohello connection string that was obtained when creating hello namespace, and set `queueName` toohello queue name that you used when creating hello queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="b76db-143">Soubor Program.cs by měl vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="b76db-143">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER tooexit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="b76db-144">Spuštění programu hello a znovu zkontrolujte hello portálu.</span><span class="sxs-lookup"><span data-stu-id="b76db-144">Run hello program, and check hello portal again.</span></span> <span data-ttu-id="b76db-145">Všimněte si, že hello **počet zpráv aktivní** a **aktuální** jsou hodnoty 0.</span><span class="sxs-lookup"><span data-stu-id="b76db-145">Notice that hello **Active Message Count** and **Current** values are now 0.</span></span>
   
    ![Délka fronty][queue-message-receive]

<span data-ttu-id="b76db-147">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="b76db-147">Congratulations!</span></span> <span data-ttu-id="b76db-148">Vytvořili jste frontu, zaslali jste zprávu a přijali jste zprávu.</span><span class="sxs-lookup"><span data-stu-id="b76db-148">You have now created a queue, sent a message, and received a message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b76db-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b76db-149">Next steps</span></span>

<span data-ttu-id="b76db-150">Podívejte se na naše [úložiště GitHub se ukázky](https://github.com/Azure/azure-service-bus/tree/master/samples) který předvedli hello pokročilejší funkce zasílání zpráv Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b76db-150">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of hello more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
