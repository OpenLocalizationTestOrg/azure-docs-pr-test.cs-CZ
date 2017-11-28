---
title: "Vícevrstvá aplikace .NET, která používá Azure Service Bus | Dokumentace Microsoftu"
description: "Kurz .NET, který vám pomůže vytvořit vícevrstvou aplikaci v Azure, která používá fronty Service Bus ke komunikaci mezi vrstvami."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: sethm
ms.openlocfilehash: 8b502f5ac5d89801d390a872e7a8b06e094ecbba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a><span data-ttu-id="b793d-103">Vícevrstvá aplikace .NET, která používá fronty Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="b793d-103">.NET multi-tier application using Azure Service Bus queues</span></span>
## <a name="introduction"></a><span data-ttu-id="b793d-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="b793d-104">Introduction</span></span>
<span data-ttu-id="b793d-105">Vývoj pro Microsoft Azure je snadný při použití Visual Studia a bezplatné sady Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="b793d-105">Developing for Microsoft Azure is easy using Visual Studio and the free Azure SDK for .NET.</span></span> <span data-ttu-id="b793d-106">Tento kurz vás provede jednotlivými kroky při vytváření aplikace, která používá několik prostředků Azure běžících ve vašem lokálním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b793d-106">This tutorial walks you through the steps to create an application that uses multiple Azure resources running in your local environment.</span></span>

<span data-ttu-id="b793d-107">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="b793d-107">You will learn the following:</span></span>

* <span data-ttu-id="b793d-108">Jak ve svém počítači vytvořit prostředí pro vývoj pro Azure stažením a instalací jednoho balíčku.</span><span class="sxs-lookup"><span data-stu-id="b793d-108">How to enable your computer for Azure development with a single download and install.</span></span>
* <span data-ttu-id="b793d-109">Jak používat Visual Studio k vývoji pro Azure.</span><span class="sxs-lookup"><span data-stu-id="b793d-109">How to use Visual Studio to develop for Azure.</span></span>
* <span data-ttu-id="b793d-110">Jak vytvořit vícevrstvou aplikaci v Azure pomocí webových rolí a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b793d-110">How to create a multi-tier application in Azure using web and worker roles.</span></span>
* <span data-ttu-id="b793d-111">Jak komunikovat mezi vrstvami pomocí front Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-111">How to communicate between tiers using Service Bus queues.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<span data-ttu-id="b793d-112">V tomto kurzu sestavíte a spustíte vícevrstvou aplikaci v cloudové službě Azure.</span><span class="sxs-lookup"><span data-stu-id="b793d-112">In this tutorial you'll build and run the multi-tier application in an Azure cloud service.</span></span> <span data-ttu-id="b793d-113">Front-endem je webová role ASP.NET MVC a back-endem je role pracovního procesu, která používá frontu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-113">The front end is an ASP.NET MVC web role and the back end is a worker-role that uses a Service Bus queue.</span></span> <span data-ttu-id="b793d-114">Můžete vytvořit stejnou vícevrstvou aplikaci s front-endem jako webový projekt, který není nasazený do cloudové služby, ale na web Azure.</span><span class="sxs-lookup"><span data-stu-id="b793d-114">You can create the same multi-tier application with the front end as a web project, that is deployed to an Azure website instead of a cloud service.</span></span> <span data-ttu-id="b793d-115">Taky můžete vyzkoušet kurz [Hybridní lokální/cloudová aplikace .NET](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md).</span><span class="sxs-lookup"><span data-stu-id="b793d-115">You can also try out the [.NET on-premises/cloud hybrid application](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) tutorial.</span></span>

<span data-ttu-id="b793d-116">Na následujícím snímku obrazovky je vidět hotová aplikace.</span><span class="sxs-lookup"><span data-stu-id="b793d-116">The following screen shot shows the completed application.</span></span>

![][0]

## <a name="scenario-overview-inter-role-communication"></a><span data-ttu-id="b793d-117">Přehled scénáře: komunikace mezi rolemi</span><span class="sxs-lookup"><span data-stu-id="b793d-117">Scenario overview: inter-role communication</span></span>
<span data-ttu-id="b793d-118">Abyste mohli odeslat objednávku ke zpracování, musí komponenta uživatelského prostředí front-endu, která běží ve webové roli, pracovat s logikou střední úrovně běžící v roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b793d-118">To submit an order for processing, the front-end UI component, running in the web role, must interact with the middle tier logic running in the worker role.</span></span> <span data-ttu-id="b793d-119">Tento příklad používá zasílání zpráv Service Bus pro komunikaci mezi vrstvami.</span><span class="sxs-lookup"><span data-stu-id="b793d-119">This example uses Service Bus messaging for the communication between the tiers.</span></span>

<span data-ttu-id="b793d-120">Pomocí zasílání zpráv Service Bus mezi webem a prostředními úrovněmi odděluje obě části.</span><span class="sxs-lookup"><span data-stu-id="b793d-120">Using Service Bus messaging between the web and middle tiers decouples the two components.</span></span> <span data-ttu-id="b793d-121">Na rozdíl od přímého přenosu zpráv (tzn. TCP nebo HTTP) se webová úroveň nemusí k prostřední úrovni připojit přímo, namísto toho odesílá pracovní jednotky jako zprávy do služby Service Bus, která je spolehlivě uchová, dokud nebude prostřední vrstva připravená je spotřebovat a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="b793d-121">In contrast to direct messaging (that is, TCP or HTTP), the web tier does not connect to the middle tier directly; instead it pushes units of work, as messages, into Service Bus, which reliably retains them until the middle tier is ready to consume and process them.</span></span>

<span data-ttu-id="b793d-122">Service Bus nabízí dvě entity, které podporují zprostředkované zasílání zpráv: fronty a témata.</span><span class="sxs-lookup"><span data-stu-id="b793d-122">Service Bus provides two entities to support brokered messaging: queues and topics.</span></span> <span data-ttu-id="b793d-123">V případě front se každá zpráva odeslaná do fronty spotřebuje jedním příjemcem.</span><span class="sxs-lookup"><span data-stu-id="b793d-123">With queues, each message sent to the queue is consumed by a single receiver.</span></span> <span data-ttu-id="b793d-124">Témata podporují chování typu publikovat/odebírat, ve kterém je každá publikovaná zpráva zpřístupněná odběru registrovanému pro dané téma.</span><span class="sxs-lookup"><span data-stu-id="b793d-124">Topics support the publish/subscribe pattern in which each published message is made available to a subscription registered with the topic.</span></span> <span data-ttu-id="b793d-125">Každý odběr logicky uchovává svoji vlastní frontu zpráv.</span><span class="sxs-lookup"><span data-stu-id="b793d-125">Each subscription logically maintains its own queue of messages.</span></span> <span data-ttu-id="b793d-126">Odběry se taky dají konfigurovat pomocí pravidel filtrů, které omezují skupinu zpráv předávaných do fronty odběru na takové, které odpovídají filtru.</span><span class="sxs-lookup"><span data-stu-id="b793d-126">Subscriptions can also be configured with filter rules that restrict the set of messages passed to the subscription queue to those that match the filter.</span></span> <span data-ttu-id="b793d-127">Následující příklad používá fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-127">The following example uses Service Bus queues.</span></span>

![][1]

<span data-ttu-id="b793d-128">Tento komunikační mechanizmus má několik výhod oproti přímému přenosu zpráv.</span><span class="sxs-lookup"><span data-stu-id="b793d-128">This communication mechanism has several advantages over direct messaging:</span></span>

* <span data-ttu-id="b793d-129">**Časové oddělení**.</span><span class="sxs-lookup"><span data-stu-id="b793d-129">**Temporal decoupling.**</span></span> <span data-ttu-id="b793d-130">S asynchronním vzorcem zasílání zpráv nemusí být producenti a spotřebitelé online ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b793d-130">With the asynchronous messaging pattern, producers and consumers need not be online at the same time.</span></span> <span data-ttu-id="b793d-131">Service Bus spolehlivě uchová zprávy, dokud spotřebitel nebude připravený je přijmout.</span><span class="sxs-lookup"><span data-stu-id="b793d-131">Service Bus reliably stores messages until the consuming party is ready to receive them.</span></span> <span data-ttu-id="b793d-132">Díky tomu se součásti distribuované aplikace můžou odpojit, například při údržbě nebo při selhání jedné ze součástí, a přitom to nebude mít vliv na systém jako celek.</span><span class="sxs-lookup"><span data-stu-id="b793d-132">This enables the components of the distributed application to be disconnected, either voluntarily, for example, for maintenance, or due to a component crash, without impacting the system as a whole.</span></span> <span data-ttu-id="b793d-133">Navíc stačí, aby spotřebitelská aplikace byla online i jen v určitou dobu během dne.</span><span class="sxs-lookup"><span data-stu-id="b793d-133">Furthermore, the consuming application might only need to come online during certain times of the day.</span></span>
* <span data-ttu-id="b793d-134">**Vyrovnávání zátěže**.</span><span class="sxs-lookup"><span data-stu-id="b793d-134">**Load leveling.**</span></span> <span data-ttu-id="b793d-135">V mnoha aplikacích se zátěž na systém může postupně měnit, zatímco doba nutná ke zpracování pracovní jednotky je obvykle stálá.</span><span class="sxs-lookup"><span data-stu-id="b793d-135">In many applications, system load varies over time, while the processing time required for each unit of work is typically constant.</span></span> <span data-ttu-id="b793d-136">Propojovací producenti a spotřebitelé zpráv s frontou – to znamená, že spotřebitelskou aplikaci (pracovní proces) stačí zřídit jen na obvyklou zátěž, ne na zátěž ve špičce.</span><span class="sxs-lookup"><span data-stu-id="b793d-136">Intermediating message producers and consumers with a queue means that the consuming application (the worker) only needs to be provisioned to accommodate average load rather than peak load.</span></span> <span data-ttu-id="b793d-137">S měnící se příchozí zátěží se mění hloubka fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-137">The depth of the queue grows and contracts as the incoming load varies.</span></span> <span data-ttu-id="b793d-138">To znamená přímou úsporu nákladů ve smyslu infrastruktury nutné pro zvládání zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b793d-138">This directly saves money in terms of the amount of infrastructure required to service the application load.</span></span>
* <span data-ttu-id="b793d-139">**Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="b793d-139">**Load balancing.**</span></span> <span data-ttu-id="b793d-140">Když se zátěž zvyšuje, můžou se přidat další pracovní procesy, které budou číst zprávy z fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-140">As load increases, more worker processes can be added to read from the queue.</span></span> <span data-ttu-id="b793d-141">Každou zprávu zpracovává jen jeden pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="b793d-141">Each message is processed by only one of the worker processes.</span></span> <span data-ttu-id="b793d-142">Toto vyrovnávání zátěže podle požadavků umožňuje optimální využívání pracovních počítačů i v případě, že se pracovní počítače liší z hlediska výkonu, protože zprávy žádaný a zpracovávají svou vlastním maximální rychlostí.</span><span class="sxs-lookup"><span data-stu-id="b793d-142">Furthermore, this pull-based load balancing enables optimal use of the worker machines even if the worker machines differ in terms of processing power, as they will pull messages at their own maximum rate.</span></span> <span data-ttu-id="b793d-143">Tomuto chování se často říká *konkurence mezi spotřebiteli*.</span><span class="sxs-lookup"><span data-stu-id="b793d-143">This pattern is often termed the *competing consumer* pattern.</span></span>
  
  ![][2]

<span data-ttu-id="b793d-144">V následující části se probírá kód, který tuto architekturu implementuje.</span><span class="sxs-lookup"><span data-stu-id="b793d-144">The following sections discuss the code that implements this architecture.</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="b793d-145">Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="b793d-145">Set up the development environment</span></span>
<span data-ttu-id="b793d-146">Než začnete s vývojem aplikací pro Azure, připravte si nástroje a vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b793d-146">Before you can begin developing Azure applications, get the tools and set up your development environment.</span></span>

1. <span data-ttu-id="b793d-147">Nainstalujte sadu Azure SDK pro .NET ze [stránky pro stažení SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b793d-147">Install the Azure SDK for .NET from the SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="b793d-148">Ve sloupci **.NET** klikněte na verzi sady [Visual Studio](http://www.visualstudio.com), kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="b793d-148">In the **.NET** column, click the version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="b793d-149">Kroky v tomto kurzu používají sadu Visual Studio 2015, ale také pracují se sadou Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b793d-149">The steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="b793d-150">Když se zobrazí dialog pro spuštění nebo uložení instalačního programu, klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="b793d-150">When prompted to run or save the installer, click **Run**.</span></span>
4. <span data-ttu-id="b793d-151">V **Instalačním programu webové platformy** klikněte na **Instalovat** a pokračujte v instalaci.</span><span class="sxs-lookup"><span data-stu-id="b793d-151">In the **Web Platform Installer**, click **Install** and proceed with the installation.</span></span>
5. <span data-ttu-id="b793d-152">Po dokončení instalace budete mít všechno, co je potřeba k vývoji aplikace.</span><span class="sxs-lookup"><span data-stu-id="b793d-152">Once the installation is complete, you will have everything necessary to start to develop the app.</span></span> <span data-ttu-id="b793d-153">Sada SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace pro Azure ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="b793d-153">The SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="b793d-154">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="b793d-154">Create a namespace</span></span>
<span data-ttu-id="b793d-155">Dál je potřeba vytvořit obor názvů služby a získat klíč sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="b793d-155">The next step is to create a service namespace, and obtain a Shared Access Signature (SAS) key.</span></span> <span data-ttu-id="b793d-156">Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-156">A namespace provides an application boundary for each application exposed through Service Bus.</span></span> <span data-ttu-id="b793d-157">Systém vygeneruje klíč SAS při vytvoření oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b793d-157">A SAS key is generated by the system when a namespace is created.</span></span> <span data-ttu-id="b793d-158">Kombinace oboru názvů a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b793d-158">The combination of namespace and SAS key provides the credentials for Service Bus to authenticate access to an application.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a><span data-ttu-id="b793d-159">Vytvoření webové role</span><span class="sxs-lookup"><span data-stu-id="b793d-159">Create a web role</span></span>
<span data-ttu-id="b793d-160">V této části vytvoříte front-end své aplikace.</span><span class="sxs-lookup"><span data-stu-id="b793d-160">In this section, you build the front end of your application.</span></span> <span data-ttu-id="b793d-161">Nejdřív vytvoříte stránky, které vaše aplikace zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b793d-161">First, you create the pages that your application displays.</span></span>
<span data-ttu-id="b793d-162">Potom přidáte kód, který odesílá položky do fronty Service Bus a zobrazí informace o stavu fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-162">After that, add code that submits items to a Service Bus queue and displays status information about the queue.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="b793d-163">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="b793d-163">Create the project</span></span>
1. <span data-ttu-id="b793d-164">Jako správce spusťte Visual Studio: klikněte pravým tlačítkem na ikonu programu **Visual Studio** a vyberete možnost **Spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="b793d-164">Using administrator privileges, start Visual Studio: right-click the **Visual Studio** program icon, and then click **Run as administrator**.</span></span> <span data-ttu-id="b793d-165">Emulátor výpočtů v Azure, který se bude probírat později v tomto článku, potřebuje, aby bylo Visual Studio spuštěné s právy správce.</span><span class="sxs-lookup"><span data-stu-id="b793d-165">The Azure compute emulator, discussed later in this article, requires that Visual Studio be started with administrator privileges.</span></span>
   
   <span data-ttu-id="b793d-166">Ve Visual Studiu v nabídce **Soubor** klikněte na **Nový** a pak na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b793d-166">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="b793d-167">V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Cloud** a pak na **Cloudová služba Azure**.</span><span class="sxs-lookup"><span data-stu-id="b793d-167">From **Installed Templates**, under **Visual C#**, click **Cloud** and then click **Azure Cloud Service**.</span></span> <span data-ttu-id="b793d-168">Jako název projektu zadejte **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="b793d-168">Name the project **MultiTierApp**.</span></span> <span data-ttu-id="b793d-169">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b793d-169">Then click **OK**.</span></span>
   
   ![][9]
3. <span data-ttu-id="b793d-170">Z rolí **.NET Framework 4.5** poklikáním vyberte **Webovou roli ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="b793d-170">From **.NET Framework 4.5** roles, double-click **ASP.NET Web Role**.</span></span>
   
   ![][10]
4. <span data-ttu-id="b793d-171">Najeďte kurzorem na **WebRole1** v části **řešení Cloudové služby Azure**, klikněte na ikonu tužky a přejmenujte webovou roli na **FrontendWebRole**.</span><span class="sxs-lookup"><span data-stu-id="b793d-171">Hover over **WebRole1** under **Azure Cloud Service solution**, click the pencil icon, and rename the web role to **FrontendWebRole**.</span></span> <span data-ttu-id="b793d-172">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b793d-172">Then click **OK**.</span></span> <span data-ttu-id="b793d-173">(Ujistěte se, že „Frontend“ zadáte s malým „e“ tzn. nikoli „FrontEnd“.)</span><span class="sxs-lookup"><span data-stu-id="b793d-173">(Make sure you enter "Frontend" with a lower-case 'e,' not "FrontEnd".)</span></span>
   
   ![][11]
5. <span data-ttu-id="b793d-174">V dialogovém okně **Nový projekt ASP.NET** v seznamu **Vyberte šablonu** klikněte na **MVC**.</span><span class="sxs-lookup"><span data-stu-id="b793d-174">From the **New ASP.NET Project** dialog box, in the **Select a template** list, click **MVC**.</span></span>
   
   ![][12]
6. <span data-ttu-id="b793d-175">Pořád ještě v dialogovém okně **Nový projekt ASP.NET** klikněte na tlačítko **Změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="b793d-175">Still in the **New ASP.NET Project** dialog box, click the **Change Authentication** button.</span></span> <span data-ttu-id="b793d-176">V dialogovém okně **Změna ověřování** klikněte na možnost **Bez ověřování** a poté klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b793d-176">In the **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="b793d-177">V tomto kurzu nasazujete aplikaci, která nepotřebuje přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="b793d-177">For this tutorial, you're deploying an app that doesn't need a user login.</span></span>
   
    ![][16]
7. <span data-ttu-id="b793d-178">Pořád ještě v dialogovém okně **Nový projekt ASP.NET** klikněte na tlačítko **OK** a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="b793d-178">Back in the **New ASP.NET Project** dialog box, click **OK** to create the project.</span></span>
8. <span data-ttu-id="b793d-179">V **Průzkumníku řešení** v projektu **FrontendWebRole** klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b793d-179">In **Solution Explorer**, in the **FrontendWebRole** project, right-click **References**, then click **Manage NuGet Packages**.</span></span>
9. <span data-ttu-id="b793d-180">Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="b793d-180">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="b793d-181">Vyberte balíček **WindowsAzure.ServiceBus**, klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="b793d-181">Select the **WindowsAzure.ServiceBus** package, click **Install**, and accept the terms of use.</span></span>
   
   ![][13]
   
   <span data-ttu-id="b793d-182">Poznámka: Teď jsou vytvořené reference na sestavení klienta a přidané některé nové soubory s kódem.</span><span class="sxs-lookup"><span data-stu-id="b793d-182">Note that the required client assemblies are now referenced and some new code files have been added.</span></span>
10. <span data-ttu-id="b793d-183">V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely**, pak klikněte na **Přidat** a pak na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="b793d-183">In **Solution Explorer**, right-click **Models** and click **Add**, then click **Class**.</span></span> <span data-ttu-id="b793d-184">Do pole **Název** zadejte název **OnlineOrder.cs**.</span><span class="sxs-lookup"><span data-stu-id="b793d-184">In the **Name** box, type the name **OnlineOrder.cs**.</span></span> <span data-ttu-id="b793d-185">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b793d-185">Then click **Add**.</span></span>

### <a name="write-the-code-for-your-web-role"></a><span data-ttu-id="b793d-186">Napsání kódu pro vaši webovou roli</span><span class="sxs-lookup"><span data-stu-id="b793d-186">Write the code for your web role</span></span>
<span data-ttu-id="b793d-187">V této části vytvoříte stránky, které vaše aplikace zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b793d-187">In this section, you create the various pages that your application displays.</span></span>

1. <span data-ttu-id="b793d-188">V souboru OnlineOrder.cs ve Visual Studiu nahraďte existující definici oboru názvů následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b793d-188">In the OnlineOrder.cs file in Visual Studio, replace the existing namespace definition with the following code:</span></span>
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. <span data-ttu-id="b793d-189">V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b793d-189">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span> <span data-ttu-id="b793d-190">Na začátek souboru přidejte následující příkazy **using**, které přidají obory názvů pro model, který jste právě vytvořili, a pro Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-190">Add the following **using** statements at the top of the file to include the namespaces for the model you just created, as well as Service Bus.</span></span>
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. <span data-ttu-id="b793d-191">v souboru HomeController.cs ve Visual Studiu taky místo existující definice oboru názvů zadejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="b793d-191">Also in the HomeController.cs file in Visual Studio, replace the existing namespace definition with the following code.</span></span> <span data-ttu-id="b793d-192">Tento kód obsahuje metody pro zpracování odesílání položek do fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-192">This code contains methods for handling the submission of items to the queue.</span></span>
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. <span data-ttu-id="b793d-193">V nabídce **Sestavení** klikněte na **Sestavit řešení** a vyzkoušejte přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="b793d-193">From the **Build** menu, click **Build Solution** to test the accuracy of your work so far.</span></span>
5. <span data-ttu-id="b793d-194">Teď vytvořte zobrazení pro metodu `Submit()`, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="b793d-194">Now, create the view for the `Submit()` method you created earlier.</span></span> <span data-ttu-id="b793d-195">Klikněte pravým tlačítkem do metody `Submit()` (přetížení `Submit()`, které nepřijímá žádné parametry), a pak vyberte **Přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b793d-195">Right-click within the `Submit()` method (the overload of `Submit()` that takes no parameters), and then choose **Add View**.</span></span>
   
   ![][14]
6. <span data-ttu-id="b793d-196">Objeví se dialogové okno pro vytvoření zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b793d-196">A dialog box appears for creating the view.</span></span> <span data-ttu-id="b793d-197">V seznamu **Šablona** vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b793d-197">In the **Template** list, choose **Create**.</span></span> <span data-ttu-id="b793d-198">V seznamu **Třída modelu** klikněte na třídu **OnlineOrder**.</span><span class="sxs-lookup"><span data-stu-id="b793d-198">In the **Model class** list, click the **OnlineOrder** class.</span></span>
   
   ![][15]
7. <span data-ttu-id="b793d-199">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b793d-199">Click **Add**.</span></span>
8. <span data-ttu-id="b793d-200">Teď změňte zobrazený název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b793d-200">Now, change the displayed name of your application.</span></span> <span data-ttu-id="b793d-201">V **Průzkumníku řešení** poklikejte na soubor **Views\Shared\\_Layout.cshtml** a otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b793d-201">In **Solution Explorer**, double-click the **Views\Shared\\_Layout.cshtml** file to open it in the Visual Studio editor.</span></span>
9. <span data-ttu-id="b793d-202">Všechny výskyty **My ASP.NET Application** změňte na **LITWARE'S Products**.</span><span class="sxs-lookup"><span data-stu-id="b793d-202">Replace all occurrences of **My ASP.NET Application** with **LITWARE'S Products**.</span></span>
10. <span data-ttu-id="b793d-203">Odstraňte odkazy **Home**, **About** a **Contact**.</span><span class="sxs-lookup"><span data-stu-id="b793d-203">Remove the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="b793d-204">Odstraňte zvýrazněný uzel:</span><span class="sxs-lookup"><span data-stu-id="b793d-204">Delete the highlighted code:</span></span>
    
    ![][28]
11. <span data-ttu-id="b793d-205">Nakonec upravte stránku pro odesílání tak, aby obsahovala nějaké informace o frontě.</span><span class="sxs-lookup"><span data-stu-id="b793d-205">Finally, modify the submission page to include some information about the queue.</span></span> <span data-ttu-id="b793d-206">V **Průzkumníku řešení** poklikejte na soubor **Views\Home\Submit.cshtml** a otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b793d-206">In **Solution Explorer**, double-click the **Views\Home\Submit.cshtml** file to open it in the Visual Studio editor.</span></span> <span data-ttu-id="b793d-207">Po `<h2>Submit</h2>` přidejte následující řádek.</span><span class="sxs-lookup"><span data-stu-id="b793d-207">Add the following line after `<h2>Submit</h2>`.</span></span> <span data-ttu-id="b793d-208">Hodnota `ViewBag.MessageCount` je zatím prázdná.</span><span class="sxs-lookup"><span data-stu-id="b793d-208">For now, the `ViewBag.MessageCount` is empty.</span></span> <span data-ttu-id="b793d-209">Zaplníte ji později.</span><span class="sxs-lookup"><span data-stu-id="b793d-209">You will populate it later.</span></span>
    
    ```html
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. <span data-ttu-id="b793d-210">Teď je implementované vaše uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="b793d-210">You now have implemented your UI.</span></span> <span data-ttu-id="b793d-211">Stisknutím klávesy **F5** můžete spustit svoji aplikaci a zkontrolovat, že vypadá tak, jak má.</span><span class="sxs-lookup"><span data-stu-id="b793d-211">You can press **F5** to run your application and confirm that it looks as expected.</span></span>
    
    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a><span data-ttu-id="b793d-212">Napsání nového kódu pro odesílání položek do fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="b793d-212">Write the code for submitting items to a Service Bus queue</span></span>
<span data-ttu-id="b793d-213">Teď přidejte kód pro odesílání položek do fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-213">Now, add code for submitting items to a queue.</span></span> <span data-ttu-id="b793d-214">Nejdřív vytvořte třídu, která obsahuje informace o připojení k vaší frontě Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-214">First, you create a class that contains your Service Bus queue connection information.</span></span> <span data-ttu-id="b793d-215">Potom inicializujte připojení ze souboru Global.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="b793d-215">Then, initialize your connection from Global.aspx.cs.</span></span> <span data-ttu-id="b793d-216">Nakonec aktualizujte kód pro odesílání, který jste vytvořili předtím v souboru HomeController.cs tak, aby položky odesílal do fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-216">Finally, update the submission code you created earlier in HomeController.cs to actually submit items to a Service Bus queue.</span></span>

1. <span data-ttu-id="b793d-217">V **Průzkumníku řešení** klikněte pravým tlačítkem na **FrontendWebRole** (pravým tlačítkem klikněte na projekt, ne na roli).</span><span class="sxs-lookup"><span data-stu-id="b793d-217">In **Solution Explorer**, right-click **FrontendWebRole** (right-click the project, not the role).</span></span> <span data-ttu-id="b793d-218">Klikněte na **Přidat** a potom na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="b793d-218">Click **Add**, and then click **Class**.</span></span>
2. <span data-ttu-id="b793d-219">Zadejte název třídy **QueueConnector.cs**.</span><span class="sxs-lookup"><span data-stu-id="b793d-219">Name the class **QueueConnector.cs**.</span></span> <span data-ttu-id="b793d-220">Klikněte na **Přidat** a třída se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="b793d-220">Click **Add** to create the class.</span></span>
3. <span data-ttu-id="b793d-221">Teď přidejte kód, který bude obsahovat informace o připojení a inicializovat připojení k frontě Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b793d-221">Now, add code that encapsulates the connection information and initializes the connection to a Service Bus queue.</span></span> <span data-ttu-id="b793d-222">Celý obsah souboru QueueConnector.cs nahraďte následujícím kódem a zadejte hodnoty pro `your Service Bus namespace` (název vašeho oboru názvů) a `yourKey` – to je **primární klíč**, který jste předtím získali z webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b793d-222">Replace the entire contents of QueueConnector.cs with the following code, and enter values for `your Service Bus namespace` (your namespace name) and `yourKey`, which is the **primary key** you previously obtained from the Azure portal.</span></span>
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from the portal.
           public const string Namespace = "your Service Bus namespace";
   
           // The name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create the namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http to be friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create the namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create the queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client to the queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. <span data-ttu-id="b793d-223">Teď musíte zajistit, aby se vaše metoda **Initialize** volala.</span><span class="sxs-lookup"><span data-stu-id="b793d-223">Now, ensure that your **Initialize** method gets called.</span></span> <span data-ttu-id="b793d-224">V **Průzkumníku řešení** poklikejte na **Global.asax\Global.asax.cs**.</span><span class="sxs-lookup"><span data-stu-id="b793d-224">In **Solution Explorer**, double-click **Global.asax\Global.asax.cs**.</span></span>
5. <span data-ttu-id="b793d-225">Přidejte následující řádek na konec metody **Application_Start**.</span><span class="sxs-lookup"><span data-stu-id="b793d-225">Add the following line of code at the end of the **Application_Start** method.</span></span>
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. <span data-ttu-id="b793d-226">Nakonec aktualizujte webový kód, který jste vytvořili předtím, aby se položky odesílaly do fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-226">Finally, update the web code you created earlier, to submit items to the queue.</span></span> <span data-ttu-id="b793d-227">V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b793d-227">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span>
7. <span data-ttu-id="b793d-228">Aktualizujte metodu `Submit()` (přetížení, které nepřijímá žádné parametry) následujícím způsobem, aby se získal počet zpráv pro frontu.</span><span class="sxs-lookup"><span data-stu-id="b793d-228">Update the `Submit()` method (the overload that takes no parameters) as follows to get the message count for the queue.</span></span>
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you to perform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get the queue, and obtain the message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. <span data-ttu-id="b793d-229">Aktualizujte metodu `Submit(OnlineOrder order)` (přetížení, které přijímá jeden parametr) následujícím způsobem, aby se do fronty odesílaly informace o objednávce.</span><span class="sxs-lookup"><span data-stu-id="b793d-229">Update the `Submit(OnlineOrder order)` method (the overload that takes one parameter) as follows to submit order information to the queue.</span></span>
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from the order.
           var message = new BrokeredMessage(order);
   
           // Submit the order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. <span data-ttu-id="b793d-230">Teď můžete aplikaci znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="b793d-230">You can now run the application again.</span></span> <span data-ttu-id="b793d-231">Pokaždé, když odešlete odbejdnávku, počet zpráv se zvýší.</span><span class="sxs-lookup"><span data-stu-id="b793d-231">Each time you submit an order, the message count increases.</span></span>
   
   ![][18]

## <a name="create-the-worker-role"></a><span data-ttu-id="b793d-232">Vytvoření role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="b793d-232">Create the worker role</span></span>
<span data-ttu-id="b793d-233">Teď vytvoříte roli pracovního procesu, která zpracuje odesílání objednávek.</span><span class="sxs-lookup"><span data-stu-id="b793d-233">You will now create the worker role that processes the order submissions.</span></span> <span data-ttu-id="b793d-234">Tento příklad používá šablonu Visual Studia **Role pracovního procesu s frontou Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="b793d-234">This example uses the **Worker Role with Service Bus Queue** Visual Studio project template.</span></span> <span data-ttu-id="b793d-235">Potřebné pověření jste už získali z portálu.</span><span class="sxs-lookup"><span data-stu-id="b793d-235">You already obtained the required credentials from the portal.</span></span>

1. <span data-ttu-id="b793d-236">Zkontrolujte, že máte Visual Studio připojené ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b793d-236">Make sure you have connected Visual Studio to your Azure account.</span></span>
2. <span data-ttu-id="b793d-237">Ve Visual Studiu v **Průzkumníku řešení** klikněte pravým tlačítkem na složku **Roles** v projektu **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="b793d-237">In Visual Studio, in **Solution Explorer** right-click the **Roles** folder under the **MultiTierApp** project.</span></span>
3. <span data-ttu-id="b793d-238">Klikněte na **Přidat**, a pak klikněte na **Nový projekt role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="b793d-238">Click **Add**, and then click **New Worker Role Project**.</span></span> <span data-ttu-id="b793d-239">Zobrazí se dialogové okno **Přidat nový projekt role**.</span><span class="sxs-lookup"><span data-stu-id="b793d-239">The **Add New Role Project** dialog box appears.</span></span>
   
   ![][26]
4. <span data-ttu-id="b793d-240">V dialogovém okně **Přidat nový projekt role** klikněte na **Role pracovního procesu s frontou Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="b793d-240">In the **Add New Role Project** dialog box, click **Worker Role with Service Bus Queue**.</span></span>
   
   ![][23]
5. <span data-ttu-id="b793d-241">Do pole **Název** zadejte název projektu **OrderProcessingRole**.</span><span class="sxs-lookup"><span data-stu-id="b793d-241">In the **Name** box, name the project **OrderProcessingRole**.</span></span> <span data-ttu-id="b793d-242">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b793d-242">Then click **Add**.</span></span>
6. <span data-ttu-id="b793d-243">Připojovací řetězec, který jste získali v kroku 9 v části „Vytvoření oboru názvů Service Bus“ zkopírujte do schránky.</span><span class="sxs-lookup"><span data-stu-id="b793d-243">Copy the connection string that you obtained in step 9 of the "Create a Service Bus namespace" section to the clipboard.</span></span>
7. <span data-ttu-id="b793d-244">V **Průzkumníku řešení** klikněte pravým tlačítkem na roli **OrderProcessingRole**, které jste vytvořili v kroku 5 (ujistěte se, že kliknete pravým tlačítkem na **OrderProcessingRole** v seznamu **Role**, ne na třídu).</span><span class="sxs-lookup"><span data-stu-id="b793d-244">In **Solution Explorer**, right-click the **OrderProcessingRole** you created in step 5 (make sure that you right-click **OrderProcessingRole** under **Roles**, and not the class).</span></span> <span data-ttu-id="b793d-245">Potom klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="b793d-245">Then click **Properties**.</span></span>
8. <span data-ttu-id="b793d-246">Na kartě **Nastavení** v dialogovém okně **Vlastnosti** klikněte do pole **Hodnota** pro **Microsoft.ServiceBus.ConnectionString** a vložte hodnotu koncového bodu, kterou jste zkopírovali v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="b793d-246">On the **Settings** tab of the **Properties** dialog box, click inside the **Value** box for **Microsoft.ServiceBus.ConnectionString**, and then paste the endpoint value you copied in step 6.</span></span>
   
   ![][25]
9. <span data-ttu-id="b793d-247">Vytvořte třídu **OnlineOrder**, která bude zastupovat objednávky při jejich zpracování z fronty.</span><span class="sxs-lookup"><span data-stu-id="b793d-247">Create an **OnlineOrder** class to represent the orders as you process them from the queue.</span></span> <span data-ttu-id="b793d-248">Můžete znovu použít třídu, kterou jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b793d-248">You can reuse a class you have already created.</span></span> <span data-ttu-id="b793d-249">V **Průzkumníku řešení** klikněte pravým tlačítkem na třídu **OrderProcessingRole** (pravým tlačítkem klikněte ikonu třídy, ne na roli).</span><span class="sxs-lookup"><span data-stu-id="b793d-249">In **Solution Explorer**, right-click the **OrderProcessingRole** class (right-click the class icon, not the role).</span></span> <span data-ttu-id="b793d-250">Klikněte na **Přidat**, pak klikněte na **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="b793d-250">Click **Add**, then click **Existing Item**.</span></span>
10. <span data-ttu-id="b793d-251">Přejděte do podsložky pro **FrontendWebRole\Models** a poklikejte na **OnlineOrder.cs**, tím ho přidáte do projektu.</span><span class="sxs-lookup"><span data-stu-id="b793d-251">Browse to the subfolder for **FrontendWebRole\Models**, and then double-click **OnlineOrder.cs** to add it to this project.</span></span>
11. <span data-ttu-id="b793d-252">Ve **WorkerRole.cs** změňte hodnotu proměnné **QueueName** z `"ProcessingQueue"` na `"OrdersQueue"`, jak je vidět v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="b793d-252">In **WorkerRole.cs**, change the value of the **QueueName** variable from `"ProcessingQueue"` to `"OrdersQueue"` as shown in the following code.</span></span>
    
    ```csharp
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. <span data-ttu-id="b793d-253">Na začátek souboru WorkerRole.cs přidejte následující příkaz using.</span><span class="sxs-lookup"><span data-stu-id="b793d-253">Add the following using statement at the top of the WorkerRole.cs file.</span></span>
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. <span data-ttu-id="b793d-254">Ve funkci `Run()` ve volání `OnMessage()` místo obsahu klauzule `try` vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="b793d-254">In the `Run()` function, inside the `OnMessage()` call, replace the contents of the `try` clause with the following code.</span></span>
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. <span data-ttu-id="b793d-255">Dokončili jste aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b793d-255">You have completed the application.</span></span> <span data-ttu-id="b793d-256">Celou aplikaci můžete vyzkoušet tak, že v Průzkumníku řešení kliknete pravým tlačítkem na projekt MultiTierApp, vyberete **Nastavit jako spouštěný projekt**, a pak stisknete F5.</span><span class="sxs-lookup"><span data-stu-id="b793d-256">You can test the full application by right-clicking the MultiTierApp project in Solution Explorer, selecting **Set as Startup Project**, and then pressing F5.</span></span> <span data-ttu-id="b793d-257">Všimněte si, že počet zpráv se nezvyšuje, protože role pracovního procesu zpracovává položky z fronty a označuje je jako hotové.</span><span class="sxs-lookup"><span data-stu-id="b793d-257">Note that the message count does not increment, because the worker role processes items from the queue and marks them as complete.</span></span> <span data-ttu-id="b793d-258">Výstup své role pracovního procesu můžete sledovat v uživatelském prostředí Emulátoru výpočtů v Azure.</span><span class="sxs-lookup"><span data-stu-id="b793d-258">You can see the trace output of your worker role by viewing the Azure Compute Emulator UI.</span></span> <span data-ttu-id="b793d-259">To můžete udělat tak, že v oznamovací oblasti hlavního panelu kliknete pravým tlačítkem na ikonu emulátoru a vyberte **Zobrazit uživatelské prostředí emulátoru služby Compute**.</span><span class="sxs-lookup"><span data-stu-id="b793d-259">You can do this by right-clicking the emulator icon in the notification area of your taskbar and selecting **Show Compute Emulator UI**.</span></span>
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a><span data-ttu-id="b793d-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b793d-260">Next steps</span></span>
<span data-ttu-id="b793d-261">Pokud se o službě Service Bus chcete dozvědět víc, pročtěte si následující zdroje:</span><span class="sxs-lookup"><span data-stu-id="b793d-261">To learn more about Service Bus, see the following resources:</span></span>  

* <span data-ttu-id="b793d-262">[Dokumentace ke službě Azure Service Bus][sbdocs]</span><span class="sxs-lookup"><span data-stu-id="b793d-262">[Azure Service Bus documentation][sbdocs]</span></span>  
* <span data-ttu-id="b793d-263">[Stránka služby Service Bus][sbacom]</span><span class="sxs-lookup"><span data-stu-id="b793d-263">[Service Bus service page][sbacom]</span></span>  
* <span data-ttu-id="b793d-264">[Jak používat fronty Service Bus][sbacomqhowto]</span><span class="sxs-lookup"><span data-stu-id="b793d-264">[How to Use Service Bus Queues][sbacomqhowto]</span></span>  

<span data-ttu-id="b793d-265">Další informace o víceúrovňových scénářích najdete v:</span><span class="sxs-lookup"><span data-stu-id="b793d-265">To learn more about multi-tier scenarios, see:</span></span>  

* <span data-ttu-id="b793d-266">[Vícevrstvá aplikace .NET, která používá tabulky, fronty a objekty blob služby Storage][mutitierstorage]</span><span class="sxs-lookup"><span data-stu-id="b793d-266">[.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs][mutitierstorage]</span></span>  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbdocs]: /azure/service-bus-messaging/  
[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
