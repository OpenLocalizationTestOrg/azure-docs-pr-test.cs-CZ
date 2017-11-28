---
title: "aaaGet začít s Azure Service Bus témat a odběrů | Microsoft Docs"
description: "Napíšeme aplikaci konzoly v jazyce C# využívající témata a odběry zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.openlocfilehash: 619d602599d97ecff2ded0681a383b19f1a8b7ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-topics"></a><span data-ttu-id="df3a3-103">Začínáme s tématy služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="df3a3-103">Get started with Service Bus topics</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="df3a3-104">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="df3a3-104">What will be accomplished</span></span>

<span data-ttu-id="df3a3-105">Tento kurz se zabývá hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="df3a3-105">This tutorial covers hello following steps:</span></span>

1. <span data-ttu-id="df3a3-106">Vytvořte obor názvů Service Bus pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="df3a3-106">Create a Service Bus namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="df3a3-107">Vytvoření tématu Service Bus, pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="df3a3-107">Create a Service Bus topic, using hello Azure portal.</span></span>
3. <span data-ttu-id="df3a3-108">Vytvořte téma toothat předplatné služby Service Bus pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="df3a3-108">Create a Service Bus subscription toothat topic, using hello Azure portal.</span></span>
4. <span data-ttu-id="df3a3-109">Zápis na konzole aplikace toosend téma toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="df3a3-109">Write a console application toosend a message toohello topic.</span></span>
5. <span data-ttu-id="df3a3-110">Zápis na konzole aplikace tooreceive zprávy z předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="df3a3-110">Write a console application tooreceive that message from hello subscription.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df3a3-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="df3a3-111">Prerequisites</span></span>

1. <span data-ttu-id="df3a3-112">[Visual Studio 2015 nebo vyšší](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="df3a3-112">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="df3a3-113">Hello příklady v tomto kurzu použít Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="df3a3-113">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="df3a3-114">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="df3a3-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="df3a3-115">1. Vytvoření oboru názvů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="df3a3-115">1. Create a namespace using hello Azure portal</span></span>

<span data-ttu-id="df3a3-116">Pokud jste již vytvořili obor názvů zasílání zpráv Service Bus, přeskočit toohello [vytvořit téma pomocí portálu Azure hello](#2-create-a-topic-using-the-azure-portal) části.</span><span class="sxs-lookup"><span data-stu-id="df3a3-116">If you have already created a Service Bus Messaging namespace, jump toohello [Create a topic using hello Azure portal](#2-create-a-topic-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-hello-azure-portal"></a><span data-ttu-id="df3a3-117">2. Vytvoří téma pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="df3a3-117">2. Create a topic using hello Azure portal</span></span>

1. <span data-ttu-id="df3a3-118">Přihlaste se toohello [portál Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="df3a3-118">Log on toohello [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="df3a3-119">V levém navigačním podokně hello hello portálu, klikněte na **Service Bus** (Pokud nevidíte **Service Bus**, klikněte na tlačítko **další služby**).</span><span class="sxs-lookup"><span data-stu-id="df3a3-119">In hello left navigation pane of hello portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="df3a3-120">Klikněte na tlačítko hello obor názvů, ve kterém chcete toocreate hello tématu.</span><span class="sxs-lookup"><span data-stu-id="df3a3-120">Click hello namespace in which you would like toocreate hello topic.</span></span> <span data-ttu-id="df3a3-121">Zobrazí se okno Přehled Hello obor názvů:</span><span class="sxs-lookup"><span data-stu-id="df3a3-121">hello namespace overview blade appears:</span></span>
   
    ![Vytvoření tématu][createtopic1]
4. <span data-ttu-id="df3a3-123">V hello **oboru názvů Service Bus** okně klikněte na tlačítko **témata**, pak klikněte na tlačítko **přidat tématu**.</span><span class="sxs-lookup"><span data-stu-id="df3a3-123">In hello **Service Bus namespace** blade, click **Topics**, then click **Add topic**.</span></span>
   
    ![Výběr témat][createtopic2]
5. <span data-ttu-id="df3a3-125">Zadejte název pro téma hello a zrušte zaškrtnutí políčka hello **povolit vytváření oddílů** možnost.</span><span class="sxs-lookup"><span data-stu-id="df3a3-125">Enter a name for hello topic, and uncheck hello **Enable partitioning** option.</span></span> <span data-ttu-id="df3a3-126">Nechte hello jiné možnosti s jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="df3a3-126">Leave hello other options with their default values.</span></span>
   
    ![Vyberte Nový][createtopic3]
6. <span data-ttu-id="df3a3-128">Hello dolní části okna hello, klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="df3a3-128">At hello bottom of hello blade, click **Create**.</span></span>

## <a name="3-create-a-subscription-toohello-topic"></a><span data-ttu-id="df3a3-129">3. Vytvoří téma toohello předplatného</span><span class="sxs-lookup"><span data-stu-id="df3a3-129">3. Create a subscription toohello topic</span></span>

1. <span data-ttu-id="df3a3-130">V podokně portálu prostředky hello klikněte hello obor názvů, který jste vytvořili v kroku 1 a pak klikněte na název hello téma, které jste vytvořili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="df3a3-130">In hello portal resources pane, click hello namespace you created in step 1, then click name of hello topic you created in step 2.</span></span>
2. <span data-ttu-id="df3a3-131">Top hello hello přehled podokna klikněte na tlačítko hello plus přihlásit další příliš**předplatné** tooadd téma toothis předplatné.</span><span class="sxs-lookup"><span data-stu-id="df3a3-131">A hello top of hello overview pane, click hello plus sign next too**Subscription** tooadd a subscription toothis topic.</span></span>

    ![Vytvoření odběru][createtopic4]

3. <span data-ttu-id="df3a3-133">Zadejte název pro předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="df3a3-133">Enter a name for hello subscription.</span></span> <span data-ttu-id="df3a3-134">Nechte hello jiné možnosti s jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="df3a3-134">Leave hello other options with their default values.</span></span>

## <a name="4-send-messages-toohello-topic"></a><span data-ttu-id="df3a3-135">4. Odeslání zprávy toohello tématu</span><span class="sxs-lookup"><span data-stu-id="df3a3-135">4. Send messages toohello topic</span></span>

<span data-ttu-id="df3a3-136">toosend zprávy toohello tématu jsme zápisu konzolovou aplikaci C# pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df3a3-136">toosend messages toohello topic, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="df3a3-137">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="df3a3-137">Create a console application</span></span>

<span data-ttu-id="df3a3-138">Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="df3a3-138">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-hello-service-bus-nuget-package"></a><span data-ttu-id="df3a3-139">Přidání balíčku Service Bus NuGet hello</span><span class="sxs-lookup"><span data-stu-id="df3a3-139">Add hello Service Bus NuGet package</span></span>

1. <span data-ttu-id="df3a3-140">Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="df3a3-140">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="df3a3-141">Klikněte na tlačítko hello **Procházet** kartě, vyhledejte **Microsoft Azure Service Bus**a potom vyberte hello **WindowsAzure.ServiceBus** položky.</span><span class="sxs-lookup"><span data-stu-id="df3a3-141">Click hello **Browse** tab, search for **Microsoft Azure Service Bus**, and then select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="df3a3-142">Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="df3a3-142">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-topic"></a><span data-ttu-id="df3a3-144">Zápis některé kód toosend téma toohello zpráv</span><span class="sxs-lookup"><span data-stu-id="df3a3-144">Write some code toosend a message toohello topic</span></span>

1. <span data-ttu-id="df3a3-145">Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="df3a3-145">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="df3a3-146">Přidejte následující kód toohello hello `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="df3a3-146">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="df3a3-147">Sada hello `connectionString` proměnné toohello připojovací řetězec, který jste získali při vytváření názvů hello a nastavte `topicName` toohello název, který jste použili při vytváření tématu hello.</span><span class="sxs-lookup"><span data-stu-id="df3a3-147">Set hello `connectionString` variable toohello connection string that you obtained when creating hello namespace, and set `topicName` toohello name that you used when creating hello topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="df3a3-148">Soubor Program.cs by měl vypadat takhle.</span><span class="sxs-lookup"><span data-stu-id="df3a3-148">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="df3a3-149">Spuštění programu hello a zkontrolujte hello portálu Azure: klikněte na název hello témat v oboru názvů hello **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="df3a3-149">Run hello program, and check hello Azure portal: click hello name of your topic in hello namespace **Overview** blade.</span></span> <span data-ttu-id="df3a3-150">téma Hello **Essentials** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="df3a3-150">hello topic **Essentials** blade is displayed.</span></span> <span data-ttu-id="df3a3-151">V uvedených téměř hello dolní části okna hello hello odběry, Všimněte si, že hello **počet zpráv** hodnota pro každé předplatné by teď měly být 1.</span><span class="sxs-lookup"><span data-stu-id="df3a3-151">In hello subscription(s) listed near hello bottom of hello blade, notice that hello **Message Count** value for each subscription should now be 1.</span></span> <span data-ttu-id="df3a3-152">Pokaždé, když provedete spuštění aplikace sender hello bez načítání zpráv hello (jak je popsáno v další části hello), tato hodnota se zvýší o 1.</span><span class="sxs-lookup"><span data-stu-id="df3a3-152">Each time you run hello sender application without retrieving hello messages (as described in hello next section), this value increases by 1.</span></span> <span data-ttu-id="df3a3-153">Všimněte si, že hello aktuální velikost hello tématu přírůstcích hello **aktuální** hodnota na hello **Essentials** okno pokaždé, když aplikace hello přidá zprávu toohello téma/odběr.</span><span class="sxs-lookup"><span data-stu-id="df3a3-153">Also note that hello current size of hello topic increments hello **Current** value on hello **Essentials** blade each time hello app adds a message toohello topic/subscription.</span></span>
   
      ![Velikost zpráv][topic-message]

## <a name="5-receive-messages-from-hello-subscription"></a><span data-ttu-id="df3a3-155">5. Příjem zpráv z odběru hello</span><span class="sxs-lookup"><span data-stu-id="df3a3-155">5. Receive messages from hello subscription</span></span>

1. <span data-ttu-id="df3a3-156">tooreceive uvítací zprávu nebo zpráv, které jste právě zaslali, vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček Service Bus NuGet toohello, podobně jako toohello předchozí odesílatele aplikaci.</span><span class="sxs-lookup"><span data-stu-id="df3a3-156">tooreceive hello message or messages you just sent, create a new console application and add a reference toohello Service Bus NuGet package, similar toohello previous sender application.</span></span>
2. <span data-ttu-id="df3a3-157">Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="df3a3-157">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="df3a3-158">Přidejte následující kód toohello hello `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="df3a3-158">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="df3a3-159">Sada hello `connectionString` proměnné toohello připojovací řetězec jste získali při vytváření názvů hello a nastavte `topicName` toohello název, který jste použili při vytváření tématu hello.</span><span class="sxs-lookup"><span data-stu-id="df3a3-159">Set hello `connectionString` variable toohello connection string you obtained when creating hello namespace, and set `topicName` toohello name that you used when creating hello topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="df3a3-160">Soubor Program.cs by měl vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="df3a3-160">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. <span data-ttu-id="df3a3-161">Spuštění programu hello a znovu zkontrolujte hello portálu.</span><span class="sxs-lookup"><span data-stu-id="df3a3-161">Run hello program, and check hello portal again.</span></span> <span data-ttu-id="df3a3-162">Všimněte si, že hello **počet zpráv** a **aktuální** jsou hodnoty 0.</span><span class="sxs-lookup"><span data-stu-id="df3a3-162">Notice that hello **Message Count** and **Current** values are now 0.</span></span>
   
    ![Délka tématu][topic-message-receive]

<span data-ttu-id="df3a3-164">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="df3a3-164">Congratulations!</span></span> <span data-ttu-id="df3a3-165">Právě jste vytvořili téma a odběr, odeslali zprávu a přijali ji.</span><span class="sxs-lookup"><span data-stu-id="df3a3-165">You have now created a topic and subscription, sent a message, and received that message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df3a3-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df3a3-166">Next steps</span></span>

<span data-ttu-id="df3a3-167">Podívejte se na naše [úložiště GitHub se ukázky](https://github.com/Azure/azure-service-bus/tree/master/samples) který předvedli hello pokročilejší funkce zasílání zpráv Service Bus.</span><span class="sxs-lookup"><span data-stu-id="df3a3-167">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of hello more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
