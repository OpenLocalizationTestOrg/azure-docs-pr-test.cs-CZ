---
title: "aaa.NET vícevrstvé aplikace pomocí Azure Service Bus | Microsoft Docs"
description: "Kurz .NET, který vám pomůže vytvořit vícevrstvou aplikaci v Azure, která používá toocommunicate fronty Service Bus mezi vrstvami."
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
ms.openlocfilehash: 485910ff1d3b8b0a709ee14ede32e57cf873829a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a><span data-ttu-id="edde2-103">Vícevrstvá aplikace .NET, která používá fronty Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="edde2-103">.NET multi-tier application using Azure Service Bus queues</span></span>
## <a name="introduction"></a><span data-ttu-id="edde2-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="edde2-104">Introduction</span></span>
<span data-ttu-id="edde2-105">Vývoj pro Microsoft Azure je snadný při použití sady Visual Studio a hello bezplatné sady Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="edde2-105">Developing for Microsoft Azure is easy using Visual Studio and hello free Azure SDK for .NET.</span></span> <span data-ttu-id="edde2-106">Tento kurz vás provede kroky toocreate hello aplikaci, která používá několik prostředků Azure běžících ve vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="edde2-106">This tutorial walks you through hello steps toocreate an application that uses multiple Azure resources running in your local environment.</span></span>

<span data-ttu-id="edde2-107">Co se dozvíte hello následující:</span><span class="sxs-lookup"><span data-stu-id="edde2-107">You will learn hello following:</span></span>

* <span data-ttu-id="edde2-108">Jak tooenable počítače pro vývoj pro Azure s jedním stáhněte a nainstalujte.</span><span class="sxs-lookup"><span data-stu-id="edde2-108">How tooenable your computer for Azure development with a single download and install.</span></span>
* <span data-ttu-id="edde2-109">Jak toouse toodevelop Visual Studio pro Azure.</span><span class="sxs-lookup"><span data-stu-id="edde2-109">How toouse Visual Studio toodevelop for Azure.</span></span>
* <span data-ttu-id="edde2-110">Jak toocreate vícevrstvé aplikace v Azure pomocí webových a pracovních rolí.</span><span class="sxs-lookup"><span data-stu-id="edde2-110">How toocreate a multi-tier application in Azure using web and worker roles.</span></span>
* <span data-ttu-id="edde2-111">Jak toocommunicate mezi úrovně pomocí front Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-111">How toocommunicate between tiers using Service Bus queues.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<span data-ttu-id="edde2-112">V tomto kurzu budete sestavte a spusťte hello vícevrstvé aplikace v cloudové službě Azure.</span><span class="sxs-lookup"><span data-stu-id="edde2-112">In this tutorial you'll build and run hello multi-tier application in an Azure cloud service.</span></span> <span data-ttu-id="edde2-113">Webová role ASP.NET MVC je Hello front-endu a back-end hello role pracovního procesu, který používá fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-113">hello front end is an ASP.NET MVC web role and hello back end is a worker-role that uses a Service Bus queue.</span></span> <span data-ttu-id="edde2-114">Můžete vytvořit stejnou vícevrstvou aplikaci s front-endu hello jako webový projekt, který je nasazený tooan webové stránky Azure namísto cloudové služby hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-114">You can create hello same multi-tier application with hello front end as a web project, that is deployed tooan Azure website instead of a cloud service.</span></span> <span data-ttu-id="edde2-115">Můžete také zkusit hello [hybridní lokální/Cloudová aplikace .NET](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="edde2-115">You can also try out hello [.NET on-premises/cloud hybrid application](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) tutorial.</span></span>

<span data-ttu-id="edde2-116">Hello následující snímek obrazovky ukazuje aplikace hello byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="edde2-116">hello following screen shot shows hello completed application.</span></span>

![][0]

## <a name="scenario-overview-inter-role-communication"></a><span data-ttu-id="edde2-117">Přehled scénáře: komunikace mezi rolemi</span><span class="sxs-lookup"><span data-stu-id="edde2-117">Scenario overview: inter-role communication</span></span>
<span data-ttu-id="edde2-118">toosubmit objednávku ke zpracování, hello front-end součást uživatelského rozhraní, spuštěné v hello webovou roli, musí spolupracovat s hello logikou střední úrovně běžící v roli pracovního procesu hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-118">toosubmit an order for processing, hello front-end UI component, running in hello web role, must interact with hello middle tier logic running in hello worker role.</span></span> <span data-ttu-id="edde2-119">Tento příklad používá pro hello komunikaci mezi vrstvami hello zasílání zpráv Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-119">This example uses Service Bus messaging for hello communication between hello tiers.</span></span>

<span data-ttu-id="edde2-120">Pomocí služby Service Bus zasílání zpráv mezi hello webem a prostředními úrovněmi odděluje obě části.</span><span class="sxs-lookup"><span data-stu-id="edde2-120">Using Service Bus messaging between hello web and middle tiers decouples the two components.</span></span> <span data-ttu-id="edde2-121">Na rozdíl od toodirect zasílání zpráv (to znamená, TCP nebo HTTP), hello webová vrstva nepřipojuje střední vrstvy toohello přímo. Namísto toho odesílá pracovní jednotky jako zprávy do služby Service Bus, která spolehlivě uchová, dokud nebude prostřední vrstva hello je připraven tooconsume a jejich zpracování.</span><span class="sxs-lookup"><span data-stu-id="edde2-121">In contrast toodirect messaging (that is, TCP or HTTP), hello web tier does not connect toohello middle tier directly; instead it pushes units of work, as messages, into Service Bus, which reliably retains them until hello middle tier is ready tooconsume and process them.</span></span>

<span data-ttu-id="edde2-122">Service Bus poskytuje dvě entity toosupport zprostředkované zasílání zpráv na úrovni: fronty a témata.</span><span class="sxs-lookup"><span data-stu-id="edde2-122">Service Bus provides two entities toosupport brokered messaging: queues and topics.</span></span> <span data-ttu-id="edde2-123">V případě front se každá zpráva odeslaná toohello fronty spotřebuje jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="edde2-123">With queues, each message sent toohello queue is consumed by a single receiver.</span></span> <span data-ttu-id="edde2-124">Témata podporují hello publikování a přihlášení k odběru vzor ve kterém je každá publikovaná zpráva provedené dostupné tooa odběru registrovanému pro téma hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-124">Topics support hello publish/subscribe pattern in which each published message is made available tooa subscription registered with hello topic.</span></span> <span data-ttu-id="edde2-125">Každý odběr logicky uchovává svoji vlastní frontu zpráv.</span><span class="sxs-lookup"><span data-stu-id="edde2-125">Each subscription logically maintains its own queue of messages.</span></span> <span data-ttu-id="edde2-126">Odběry můžete nakonfigurovat také pomocí pravidel filtrů, které omezují hello skupinu zpráv předávaných do toothose fronty hello předplatné, které odpovídají filtru hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-126">Subscriptions can also be configured with filter rules that restrict hello set of messages passed to hello subscription queue toothose that match hello filter.</span></span> <span data-ttu-id="edde2-127">Hello následující příklad používá fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-127">hello following example uses Service Bus queues.</span></span>

![][1]

<span data-ttu-id="edde2-128">Tento komunikační mechanizmus má několik výhod oproti přímému přenosu zpráv.</span><span class="sxs-lookup"><span data-stu-id="edde2-128">This communication mechanism has several advantages over direct messaging:</span></span>

* <span data-ttu-id="edde2-129">**Časové oddělení**.</span><span class="sxs-lookup"><span data-stu-id="edde2-129">**Temporal decoupling.**</span></span> <span data-ttu-id="edde2-130">S asynchronním vzorcem zasílání zpráv hello nemusí být producenti a spotřebitelé online na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="edde2-130">With hello asynchronous messaging pattern, producers and consumers need not be online at hello same time.</span></span> <span data-ttu-id="edde2-131">Service Bus spolehlivě uchová zprávy, dokud hello spotřebitel nebude připravený je přijmout.</span><span class="sxs-lookup"><span data-stu-id="edde2-131">Service Bus reliably stores messages until hello consuming party is ready to receive them.</span></span> <span data-ttu-id="edde2-132">To umožňuje hello součástí hello distribuované aplikace toobe odpojit, například za účelem údržby nebo z důvodu tooa součást havárií, bez dopadu na systém jako celek.</span><span class="sxs-lookup"><span data-stu-id="edde2-132">This enables hello components of hello distributed application toobe disconnected, either voluntarily, for example, for maintenance, or due tooa component crash, without impacting the system as a whole.</span></span> <span data-ttu-id="edde2-133">Kromě toho hello využívání aplikací potřebovat pouze toocome online v určitou dobu hello den.</span><span class="sxs-lookup"><span data-stu-id="edde2-133">Furthermore, hello consuming application might only need toocome online during certain times of hello day.</span></span>
* <span data-ttu-id="edde2-134">**Vyrovnávání zátěže**.</span><span class="sxs-lookup"><span data-stu-id="edde2-134">**Load leveling.**</span></span> <span data-ttu-id="edde2-135">V mnoha aplikacích zatížení systému se liší v čase, při zpracování hello čas potřebný pro jednotlivé jednotky práce je obvykle stálá.</span><span class="sxs-lookup"><span data-stu-id="edde2-135">In many applications, system load varies over time, while hello processing time required for each unit of work is typically constant.</span></span> <span data-ttu-id="edde2-136">Propojovací producenti a spotřebitelé zpráv s frontou znamená, že tento hello využívání aplikací (pracovník hello) pouze potřebám toobe zřízený průměrnou zátěž tooaccommodate spíše než zátěž ve špičce.</span><span class="sxs-lookup"><span data-stu-id="edde2-136">Intermediating message producers and consumers with a queue means that hello consuming application (hello worker) only needs toobe provisioned tooaccommodate average load rather than peak load.</span></span> <span data-ttu-id="edde2-137">Hello hloubka fronty hello s měnící hello příchozí zátěží se mění.</span><span class="sxs-lookup"><span data-stu-id="edde2-137">hello depth of hello queue grows and contracts as hello incoming load varies.</span></span> <span data-ttu-id="edde2-138">To znamená přímou úsporu nákladů z hlediska hello množství infrastruktury požadované tooservice hello aplikace zatížení.</span><span class="sxs-lookup"><span data-stu-id="edde2-138">This directly saves money in terms of hello amount of infrastructure required tooservice hello application load.</span></span>
* <span data-ttu-id="edde2-139">**Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="edde2-139">**Load balancing.**</span></span> <span data-ttu-id="edde2-140">Když se zátěž zvyšuje, další pracovní procesy přidáním tooread z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-140">As load increases, more worker processes can be added tooread from hello queue.</span></span> <span data-ttu-id="edde2-141">Každou zprávu zpracovává jenom jeden hello pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="edde2-141">Each message is processed by only one of hello worker processes.</span></span> <span data-ttu-id="edde2-142">Kromě toho tato Vyrovnávání zatížení založené na operaci pull umožňuje optimální využívání hello pracovních počítačů i v případě, že se pracovní počítače liší z hlediska výpočetní výkon, jak se bude načítat zprávy na svou vlastním maximální rychlostí.</span><span class="sxs-lookup"><span data-stu-id="edde2-142">Furthermore, this pull-based load balancing enables optimal use of hello worker machines even if the worker machines differ in terms of processing power, as they will pull messages at their own maximum rate.</span></span> <span data-ttu-id="edde2-143">Toto chování se často říká hello *konkurence mezi spotřebiteli* vzor.</span><span class="sxs-lookup"><span data-stu-id="edde2-143">This pattern is often termed hello *competing consumer* pattern.</span></span>
  
  ![][2]

<span data-ttu-id="edde2-144">Hello následující oddíly popisují hello kód, který tuto architekturu implementuje.</span><span class="sxs-lookup"><span data-stu-id="edde2-144">hello following sections discuss hello code that implements this architecture.</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="edde2-145">Nastavit hello vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="edde2-145">Set up hello development environment</span></span>
<span data-ttu-id="edde2-146">Před zahájením vývojem aplikací pro Azure, získání hello nástroje a nastavení vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="edde2-146">Before you can begin developing Azure applications, get hello tools and set up your development environment.</span></span>

1. <span data-ttu-id="edde2-147">Nainstalujte hello Azure SDK pro .NET ze hello SDK [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="edde2-147">Install hello Azure SDK for .NET from hello SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="edde2-148">V hello **.NET** sloupce, klikněte na tlačítko hello verzi [Visual Studio](http://www.visualstudio.com) používáte.</span><span class="sxs-lookup"><span data-stu-id="edde2-148">In hello **.NET** column, click hello version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="edde2-149">Hello kroky v tomto kurzu Visual Studio 2015, ale také pracovat s Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="edde2-149">hello steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="edde2-150">Po zobrazení výzvy toorun nebo uložení instalačního programu hello, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="edde2-150">When prompted toorun or save hello installer, click **Run**.</span></span>
4. <span data-ttu-id="edde2-151">V hello **instalačního programu webové platformy**, klikněte na tlačítko **nainstalovat** a pokračovat v instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-151">In hello **Web Platform Installer**, click **Install** and proceed with hello installation.</span></span>
5. <span data-ttu-id="edde2-152">Po dokončení instalace hello budete mít všechno potřebné toostart toodevelop hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="edde2-152">Once hello installation is complete, you will have everything necessary toostart toodevelop hello app.</span></span> <span data-ttu-id="edde2-153">Hello SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace Azure v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edde2-153">hello SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="edde2-154">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="edde2-154">Create a namespace</span></span>
<span data-ttu-id="edde2-155">dalším krokem Hello je toocreate oboru názvů služby a získat klíč sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="edde2-155">hello next step is toocreate a service namespace, and obtain a Shared Access Signature (SAS) key.</span></span> <span data-ttu-id="edde2-156">Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-156">A namespace provides an application boundary for each application exposed through Service Bus.</span></span> <span data-ttu-id="edde2-157">Hello systém vygeneruje SAS klíč při vytvoření oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="edde2-157">A SAS key is generated by hello system when a namespace is created.</span></span> <span data-ttu-id="edde2-158">Hello kombinace oboru názvů a klíče SAS poskytuje pověření hello pro Service Bus tooauthenticate přístup tooan aplikaci.</span><span class="sxs-lookup"><span data-stu-id="edde2-158">hello combination of namespace and SAS key provides hello credentials for Service Bus tooauthenticate access tooan application.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a><span data-ttu-id="edde2-159">Vytvoření webové role</span><span class="sxs-lookup"><span data-stu-id="edde2-159">Create a web role</span></span>
<span data-ttu-id="edde2-160">V této části vytvoříte front-end hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="edde2-160">In this section, you build hello front end of your application.</span></span> <span data-ttu-id="edde2-161">Nejprve je třeba vytvořit hello stránky, které vaše aplikace zobrazí.</span><span class="sxs-lookup"><span data-stu-id="edde2-161">First, you create hello pages that your application displays.</span></span>
<span data-ttu-id="edde2-162">Potom přidáte kód, který odesílá položky fronty Service Bus tooa a zobrazí informace o frontě hello stavu.</span><span class="sxs-lookup"><span data-stu-id="edde2-162">After that, add code that submits items tooa Service Bus queue and displays status information about hello queue.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="edde2-163">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="edde2-163">Create hello project</span></span>
1. <span data-ttu-id="edde2-164">Pomocí oprávnění správce, spuštění sady Visual Studio: klikněte pravým tlačítkem na hello **Visual Studio** ikonu programu a potom klikněte na **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="edde2-164">Using administrator privileges, start Visual Studio: right-click hello **Visual Studio** program icon, and then click **Run as administrator**.</span></span> <span data-ttu-id="edde2-165">Hello emulátoru služby výpočty Azure, probírat později v tomto článku, vyžaduje, aby Visual Studio spuštěné s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="edde2-165">hello Azure compute emulator, discussed later in this article, requires that Visual Studio be started with administrator privileges.</span></span>
   
   <span data-ttu-id="edde2-166">V sadě Visual Studio na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="edde2-166">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="edde2-167">V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Cloud** a pak na **Cloudová služba Azure**.</span><span class="sxs-lookup"><span data-stu-id="edde2-167">From **Installed Templates**, under **Visual C#**, click **Cloud** and then click **Azure Cloud Service**.</span></span> <span data-ttu-id="edde2-168">Název projektu hello **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="edde2-168">Name hello project **MultiTierApp**.</span></span> <span data-ttu-id="edde2-169">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="edde2-169">Then click **OK**.</span></span>
   
   ![][9]
3. <span data-ttu-id="edde2-170">Z rolí **.NET Framework 4.5** poklikáním vyberte **Webovou roli ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="edde2-170">From **.NET Framework 4.5** roles, double-click **ASP.NET Web Role**.</span></span>
   
   ![][10]
4. <span data-ttu-id="edde2-171">Pozastavte ukazatel myši nad **WebRole1** pod **řešení cloudové služby Azure**, klikněte na ikonu tužky hello a přejmenujte webovou roli hello příliš**FrontendWebRole**.</span><span class="sxs-lookup"><span data-stu-id="edde2-171">Hover over **WebRole1** under **Azure Cloud Service solution**, click hello pencil icon, and rename hello web role too**FrontendWebRole**.</span></span> <span data-ttu-id="edde2-172">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="edde2-172">Then click **OK**.</span></span> <span data-ttu-id="edde2-173">(Ujistěte se, že „Frontend“ zadáte s malým „e“ tzn. nikoli „FrontEnd“.)</span><span class="sxs-lookup"><span data-stu-id="edde2-173">(Make sure you enter "Frontend" with a lower-case 'e,' not "FrontEnd".)</span></span>
   
   ![][11]
5. <span data-ttu-id="edde2-174">Z hello **nový projekt ASP.NET** dialogové okno, v hello **vyberte šablonu** seznamu, klikněte na tlačítko **MVC**.</span><span class="sxs-lookup"><span data-stu-id="edde2-174">From hello **New ASP.NET Project** dialog box, in hello **Select a template** list, click **MVC**.</span></span>
   
   ![][12]
6. <span data-ttu-id="edde2-175">Stále v hello **nový projekt ASP.NET** dialogovém okně klikněte na hello **změna ověřování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="edde2-175">Still in hello **New ASP.NET Project** dialog box, click hello **Change Authentication** button.</span></span> <span data-ttu-id="edde2-176">V hello **změna ověřování** dialogové okno, klikněte na tlačítko **bez ověřování**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="edde2-176">In hello **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="edde2-177">V tomto kurzu nasazujete aplikaci, která nepotřebuje přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="edde2-177">For this tutorial, you're deploying an app that doesn't need a user login.</span></span>
   
    ![][16]
7. <span data-ttu-id="edde2-178">Zpět v hello **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="edde2-178">Back in hello **New ASP.NET Project** dialog box, click **OK** toocreate hello project.</span></span>
8. <span data-ttu-id="edde2-179">V **Průzkumníku řešení**, v hello **FrontendWebRole** projektu, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="edde2-179">In **Solution Explorer**, in hello **FrontendWebRole** project, right-click **References**, then click **Manage NuGet Packages**.</span></span>
9. <span data-ttu-id="edde2-180">Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="edde2-180">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="edde2-181">Vyberte hello **WindowsAzure.ServiceBus** balíček, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-181">Select hello **WindowsAzure.ServiceBus** package, click **Install**, and accept hello terms of use.</span></span>
   
   ![][13]
   
   <span data-ttu-id="edde2-182">Všimněte si, že hello vyžaduje teď jsou reference na sestavení klienta a přidané některé nové soubory s kódem.</span><span class="sxs-lookup"><span data-stu-id="edde2-182">Note that hello required client assemblies are now referenced and some new code files have been added.</span></span>
10. <span data-ttu-id="edde2-183">V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely**, pak klikněte na **Přidat** a pak na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="edde2-183">In **Solution Explorer**, right-click **Models** and click **Add**, then click **Class**.</span></span> <span data-ttu-id="edde2-184">V hello **název** pole, název typu hello **OnlineOrder.cs**.</span><span class="sxs-lookup"><span data-stu-id="edde2-184">In hello **Name** box, type hello name **OnlineOrder.cs**.</span></span> <span data-ttu-id="edde2-185">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="edde2-185">Then click **Add**.</span></span>

### <a name="write-hello-code-for-your-web-role"></a><span data-ttu-id="edde2-186">Psaní kódu hello pro vaši webovou roli</span><span class="sxs-lookup"><span data-stu-id="edde2-186">Write hello code for your web role</span></span>
<span data-ttu-id="edde2-187">V této části vytvoříte hello stránky, které vaše aplikace zobrazí.</span><span class="sxs-lookup"><span data-stu-id="edde2-187">In this section, you create hello various pages that your application displays.</span></span>

1. <span data-ttu-id="edde2-188">V souboru OnlineOrder.cs hello v sadě Visual Studio nahraďte existující definici oboru názvů hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="edde2-188">In hello OnlineOrder.cs file in Visual Studio, replace the existing namespace definition with hello following code:</span></span>
   
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
2. <span data-ttu-id="edde2-189">V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="edde2-189">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span> <span data-ttu-id="edde2-190">Přidejte následující hello **pomocí** příkazy hello horní části hello souboru tooinclude hello obory názvů pro model, který jste právě vytvořili, zvolte a pro Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-190">Add hello following **using** statements at hello top of hello file tooinclude hello namespaces for the model you just created, as well as Service Bus.</span></span>
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. <span data-ttu-id="edde2-191">Také v souboru HomeController.cs hello v sadě Visual Studio, nahraďte existující definici oboru názvů hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="edde2-191">Also in hello HomeController.cs file in Visual Studio, replace the existing namespace definition with hello following code.</span></span> <span data-ttu-id="edde2-192">Tento kód obsahuje metody pro zpracování odesílání hello položky toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="edde2-192">This code contains methods for handling hello submission of items toohello queue.</span></span>
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect tooSubmit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for hello submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from hello submission
           // form.
           [HttpPost]
           // Attribute toohelp prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting tooqueue here.
   
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
4. <span data-ttu-id="edde2-193">Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** tootest hello přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="edde2-193">From hello **Build** menu, click **Build Solution** tootest hello accuracy of your work so far.</span></span>
5. <span data-ttu-id="edde2-194">Teď vytvořte zobrazení hello hello `Submit()` metoda jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="edde2-194">Now, create hello view for hello `Submit()` method you created earlier.</span></span> <span data-ttu-id="edde2-195">Klikněte pravým tlačítkem do hello `Submit()` – metoda (hello přetížení `Submit()` které nepřijímá žádné parametry) a potom zvolte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="edde2-195">Right-click within hello `Submit()` method (hello overload of `Submit()` that takes no parameters), and then choose **Add View**.</span></span>
   
   ![][14]
6. <span data-ttu-id="edde2-196">Zobrazí se dialogové okno pro vytvoření zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-196">A dialog box appears for creating hello view.</span></span> <span data-ttu-id="edde2-197">V hello **šablony** vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="edde2-197">In hello **Template** list, choose **Create**.</span></span> <span data-ttu-id="edde2-198">V hello **třída modelu** seznamu, klikněte na tlačítko hello **OnlineOrder** třídy.</span><span class="sxs-lookup"><span data-stu-id="edde2-198">In hello **Model class** list, click hello **OnlineOrder** class.</span></span>
   
   ![][15]
7. <span data-ttu-id="edde2-199">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="edde2-199">Click **Add**.</span></span>
8. <span data-ttu-id="edde2-200">Teď změňte hello zobrazí název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="edde2-200">Now, change hello displayed name of your application.</span></span> <span data-ttu-id="edde2-201">V **Průzkumníku řešení**, dvakrát klikněte **Views\Shared\\_Layout.cshtml** souboru tooopen ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-201">In **Solution Explorer**, double-click the **Views\Shared\\_Layout.cshtml** file tooopen it in hello Visual Studio editor.</span></span>
9. <span data-ttu-id="edde2-202">Všechny výskyty **My ASP.NET Application** změňte na **LITWARE'S Products**.</span><span class="sxs-lookup"><span data-stu-id="edde2-202">Replace all occurrences of **My ASP.NET Application** with **LITWARE'S Products**.</span></span>
10. <span data-ttu-id="edde2-203">Odebrat hello **Domů**, **o**, a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="edde2-203">Remove hello **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="edde2-204">Odstraňte hello zvýrazněná kód:</span><span class="sxs-lookup"><span data-stu-id="edde2-204">Delete hello highlighted code:</span></span>
    
    ![][28]
11. <span data-ttu-id="edde2-205">Nakonec upravte hello odeslání stránky tooinclude některé informace o frontě hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-205">Finally, modify hello submission page tooinclude some information about hello queue.</span></span> <span data-ttu-id="edde2-206">V **Průzkumníku řešení**, dvakrát klikněte **Views\Home\Submit.cshtml** souboru tooopen ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-206">In **Solution Explorer**, double-click the **Views\Home\Submit.cshtml** file tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="edde2-207">Přidejte následující řádek po hello `<h2>Submit</h2>`.</span><span class="sxs-lookup"><span data-stu-id="edde2-207">Add hello following line after `<h2>Submit</h2>`.</span></span> <span data-ttu-id="edde2-208">Prozatím se hello `ViewBag.MessageCount` je prázdný.</span><span class="sxs-lookup"><span data-stu-id="edde2-208">For now, hello `ViewBag.MessageCount` is empty.</span></span> <span data-ttu-id="edde2-209">Zaplníte ji později.</span><span class="sxs-lookup"><span data-stu-id="edde2-209">You will populate it later.</span></span>
    
    ```html
    <p>Current number of orders in queue waiting toobe processed: @ViewBag.MessageCount</p>
    ```
12. <span data-ttu-id="edde2-210">Teď je implementované vaše uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="edde2-210">You now have implemented your UI.</span></span> <span data-ttu-id="edde2-211">Stisknutím klávesy **F5** toorun aplikaci a potvrďte, že vypadá tak podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="edde2-211">You can press **F5** toorun your application and confirm that it looks as expected.</span></span>
    
    ![][17]

### <a name="write-hello-code-for-submitting-items-tooa-service-bus-queue"></a><span data-ttu-id="edde2-212">Zápis kódu hello pro odesílání položek fronty Service Bus tooa</span><span class="sxs-lookup"><span data-stu-id="edde2-212">Write hello code for submitting items tooa Service Bus queue</span></span>
<span data-ttu-id="edde2-213">Teď přidáte kód pro odesílání položek tooa fronty.</span><span class="sxs-lookup"><span data-stu-id="edde2-213">Now, add code for submitting items tooa queue.</span></span> <span data-ttu-id="edde2-214">Nejdřív vytvořte třídu, která obsahuje informace o připojení k vaší frontě Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-214">First, you create a class that contains your Service Bus queue connection information.</span></span> <span data-ttu-id="edde2-215">Potom inicializujte připojení ze souboru Global.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="edde2-215">Then, initialize your connection from Global.aspx.cs.</span></span> <span data-ttu-id="edde2-216">Nakonec aktualizujte kód odeslání hello, kterou jste vytvořili dříve v HomeController.cs tooactually odeslání položky tooa fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edde2-216">Finally, update hello submission code you created earlier in HomeController.cs tooactually submit items tooa Service Bus queue.</span></span>

1. <span data-ttu-id="edde2-217">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **FrontendWebRole** (klikněte pravým tlačítkem na projekt hello, ne hello roli).</span><span class="sxs-lookup"><span data-stu-id="edde2-217">In **Solution Explorer**, right-click **FrontendWebRole** (right-click hello project, not hello role).</span></span> <span data-ttu-id="edde2-218">Klikněte na **Přidat** a potom na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="edde2-218">Click **Add**, and then click **Class**.</span></span>
2. <span data-ttu-id="edde2-219">Název třídy hello **QueueConnector.cs**.</span><span class="sxs-lookup"><span data-stu-id="edde2-219">Name hello class **QueueConnector.cs**.</span></span> <span data-ttu-id="edde2-220">Klikněte na tlačítko **přidat** toocreate hello třídy.</span><span class="sxs-lookup"><span data-stu-id="edde2-220">Click **Add** toocreate hello class.</span></span>
3. <span data-ttu-id="edde2-221">Nyní přidáte kód, který zapouzdřuje informace o připojení hello a inicializuje fronty Service Bus tooa hello připojení.</span><span class="sxs-lookup"><span data-stu-id="edde2-221">Now, add code that encapsulates hello connection information and initializes hello connection tooa Service Bus queue.</span></span> <span data-ttu-id="edde2-222">Hello celý obsah souboru QueueConnector.cs nahraďte hello následující kód a zadejte hodnoty pro `your Service Bus namespace` (název vašeho oboru názvů) a `yourKey`, což je hello **primární klíč** jste předtím získali z hello Azure portál.</span><span class="sxs-lookup"><span data-stu-id="edde2-222">Replace hello entire contents of QueueConnector.cs with hello following code, and enter values for `your Service Bus namespace` (your namespace name) and `yourKey`, which is hello **primary key** you previously obtained from hello Azure portal.</span></span>
   
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
   
           // Obtain these values from hello portal.
           public const string Namespace = "your Service Bus namespace";
   
           // hello name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create hello namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http toobe friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create hello namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create hello queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client toohello queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. <span data-ttu-id="edde2-223">Teď musíte zajistit, aby se vaše metoda **Initialize** volala.</span><span class="sxs-lookup"><span data-stu-id="edde2-223">Now, ensure that your **Initialize** method gets called.</span></span> <span data-ttu-id="edde2-224">V **Průzkumníku řešení** poklikejte na **Global.asax\Global.asax.cs**.</span><span class="sxs-lookup"><span data-stu-id="edde2-224">In **Solution Explorer**, double-click **Global.asax\Global.asax.cs**.</span></span>
5. <span data-ttu-id="edde2-225">Přidejte následující řádek kódu na konci hello hello hello **Application_Start** metoda.</span><span class="sxs-lookup"><span data-stu-id="edde2-225">Add hello following line of code at hello end of hello **Application_Start** method.</span></span>
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. <span data-ttu-id="edde2-226">Nakonec aktualizujte kód webového hello jste vytvořili dříve, k odeslání položky toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="edde2-226">Finally, update hello web code you created earlier, to submit items toohello queue.</span></span> <span data-ttu-id="edde2-227">V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="edde2-227">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span>
7. <span data-ttu-id="edde2-228">Aktualizace hello `Submit()` – metoda (hello přetížení, které nepřijímá žádné parametry) následujícím způsobem tooget uvítací zprávu počet pro frontu hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-228">Update hello `Submit()` method (hello overload that takes no parameters) as follows tooget hello message count for hello queue.</span></span>
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you tooperform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get hello queue, and obtain hello message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. <span data-ttu-id="edde2-229">Aktualizace hello `Submit(OnlineOrder order)` – metoda (hello přetížení, které přijímá jeden parametr) následujícím způsobem toosubmit pořadí informace toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="edde2-229">Update hello `Submit(OnlineOrder order)` method (hello overload that takes one parameter) as follows toosubmit order information toohello queue.</span></span>
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from hello order.
           var message = new BrokeredMessage(order);
   
           // Submit hello order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. <span data-ttu-id="edde2-230">Teď můžete spustit hello aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="edde2-230">You can now run hello application again.</span></span> <span data-ttu-id="edde2-231">Pokaždé, když odešlete odbejdnávku, počet zpráv hello se zvýší.</span><span class="sxs-lookup"><span data-stu-id="edde2-231">Each time you submit an order, hello message count increases.</span></span>
   
   ![][18]

## <a name="create-hello-worker-role"></a><span data-ttu-id="edde2-232">Vytvoření role pracovního procesu hello</span><span class="sxs-lookup"><span data-stu-id="edde2-232">Create hello worker role</span></span>
<span data-ttu-id="edde2-233">Teď vytvoříte roli pracovního procesu hello, který zpracovává hello objednávek.</span><span class="sxs-lookup"><span data-stu-id="edde2-233">You will now create hello worker role that processes hello order submissions.</span></span> <span data-ttu-id="edde2-234">Tento příklad používá hello **Role pracovního procesu s frontou Service Bus** šablonu Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="edde2-234">This example uses hello **Worker Role with Service Bus Queue** Visual Studio project template.</span></span> <span data-ttu-id="edde2-235">Jste už získali z portálu hello hello vyžaduje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="edde2-235">You already obtained hello required credentials from hello portal.</span></span>

1. <span data-ttu-id="edde2-236">Ujistěte se, že jste se připojili Visual Studio tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="edde2-236">Make sure you have connected Visual Studio tooyour Azure account.</span></span>
2. <span data-ttu-id="edde2-237">V sadě Visual Studio v **Průzkumníku řešení** klikněte pravým tlačítkem myši **role** ve složce hello **MultiTierApp** projektu.</span><span class="sxs-lookup"><span data-stu-id="edde2-237">In Visual Studio, in **Solution Explorer** right-click the **Roles** folder under hello **MultiTierApp** project.</span></span>
3. <span data-ttu-id="edde2-238">Klikněte na **Přidat**, a pak klikněte na **Nový projekt role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="edde2-238">Click **Add**, and then click **New Worker Role Project**.</span></span> <span data-ttu-id="edde2-239">Hello **přidat nový projekt Role** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edde2-239">hello **Add New Role Project** dialog box appears.</span></span>
   
   ![][26]
4. <span data-ttu-id="edde2-240">V hello **přidat nový projekt Role** dialogové okno, klikněte na tlačítko **Role pracovního procesu s frontou Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="edde2-240">In hello **Add New Role Project** dialog box, click **Worker Role with Service Bus Queue**.</span></span>
   
   ![][23]
5. <span data-ttu-id="edde2-241">V hello **název** pole, název projektu hello **OrderProcessingRole**.</span><span class="sxs-lookup"><span data-stu-id="edde2-241">In hello **Name** box, name hello project **OrderProcessingRole**.</span></span> <span data-ttu-id="edde2-242">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="edde2-242">Then click **Add**.</span></span>
6. <span data-ttu-id="edde2-243">Zkopírujte hello připojovací řetězec, který jste získali v kroku 9 schránky toohello části "Vytvoření oboru názvů Service Bus" hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-243">Copy hello connection string that you obtained in step 9 of hello "Create a Service Bus namespace" section toohello clipboard.</span></span>
7. <span data-ttu-id="edde2-244">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **OrderProcessingRole** jste vytvořili v kroku 5 (ujistěte se, že kliknete pravým tlačítkem na **OrderProcessingRole** pod **Role**, a není hello třídy).</span><span class="sxs-lookup"><span data-stu-id="edde2-244">In **Solution Explorer**, right-click hello **OrderProcessingRole** you created in step 5 (make sure that you right-click **OrderProcessingRole** under **Roles**, and not hello class).</span></span> <span data-ttu-id="edde2-245">Potom klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="edde2-245">Then click **Properties**.</span></span>
8. <span data-ttu-id="edde2-246">Na hello **nastavení** kartě hello **vlastnosti** dialogové okno, klikněte do hello **hodnotu** pole pro **Microsoft.ServiceBus.ConnectionString**a potom vložte hodnotu koncového bodu hello jste zkopírovali v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="edde2-246">On hello **Settings** tab of hello **Properties** dialog box, click inside hello **Value** box for **Microsoft.ServiceBus.ConnectionString**, and then paste hello endpoint value you copied in step 6.</span></span>
   
   ![][25]
9. <span data-ttu-id="edde2-247">Vytvoření **OnlineOrder** třídy toorepresent hello objednávky jako při jejich zpracování z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-247">Create an **OnlineOrder** class toorepresent hello orders as you process them from hello queue.</span></span> <span data-ttu-id="edde2-248">Můžete znovu použít třídu, kterou jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="edde2-248">You can reuse a class you have already created.</span></span> <span data-ttu-id="edde2-249">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **OrderProcessingRole** – třída (klikněte pravým tlačítkem na ikonu třídy hello, ne hello roli).</span><span class="sxs-lookup"><span data-stu-id="edde2-249">In **Solution Explorer**, right-click hello **OrderProcessingRole** class (right-click hello class icon, not hello role).</span></span> <span data-ttu-id="edde2-250">Klikněte na **Přidat**, pak klikněte na **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="edde2-250">Click **Add**, then click **Existing Item**.</span></span>
10. <span data-ttu-id="edde2-251">Procházet toohello podsložky pro **FrontendWebRole\Models**a potom dvakrát klikněte na **OnlineOrder.cs** tooadd ho toothis projektu.</span><span class="sxs-lookup"><span data-stu-id="edde2-251">Browse toohello subfolder for **FrontendWebRole\Models**, and then double-click **OnlineOrder.cs** tooadd it toothis project.</span></span>
11. <span data-ttu-id="edde2-252">V **WorkerRole.cs**, změnit hodnotu hello hello **QueueName** proměnnou z `"ProcessingQueue"` příliš`"OrdersQueue"` jak je znázorněno v následujícím kódu hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-252">In **WorkerRole.cs**, change hello value of hello **QueueName** variable from `"ProcessingQueue"` too`"OrdersQueue"` as shown in hello following code.</span></span>
    
    ```csharp
    // hello name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. <span data-ttu-id="edde2-253">Přidejte následující hello pomocí příkazu hello horní části souboru WorkerRole.cs hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-253">Add hello following using statement at hello top of hello WorkerRole.cs file.</span></span>
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. <span data-ttu-id="edde2-254">V hello `Run()` funkce uvnitř hello `OnMessage()` volání, nahraďte obsah hello hello `try` klauzuli with hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="edde2-254">In hello `Run()` function, inside hello `OnMessage()` call, replace hello contents of hello `try` clause with hello following code.</span></span>
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View hello message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. <span data-ttu-id="edde2-255">Dokončili jste aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="edde2-255">You have completed hello application.</span></span> <span data-ttu-id="edde2-256">Hello celou aplikaci můžete otestovat kliknutím pravým tlačítkem na projekt MultiTierApp hello v Průzkumníku řešení, výběr **nastavit jako spouštěný projekt**a pak stisknete F5.</span><span class="sxs-lookup"><span data-stu-id="edde2-256">You can test hello full application by right-clicking hello MultiTierApp project in Solution Explorer, selecting **Set as Startup Project**, and then pressing F5.</span></span> <span data-ttu-id="edde2-257">Všimněte si, že počet zpráv se nezvyšuje, protože hello role pracovního procesu zpracovává položky z fronty hello a označí je jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="edde2-257">Note that the message count does not increment, because hello worker role processes items from hello queue and marks them as complete.</span></span> <span data-ttu-id="edde2-258">Zobrazí se výstup trasování hello své role pracovního procesu zobrazením hello uživatelské prostředí emulátoru výpočtů Azure.</span><span class="sxs-lookup"><span data-stu-id="edde2-258">You can see hello trace output of your worker role by viewing hello Azure Compute Emulator UI.</span></span> <span data-ttu-id="edde2-259">To provedete kliknutím pravým tlačítkem myši na ikonu emulátoru hello v hello oznamovací oblasti hlavního panelu a výběrem **zobrazit uživatelské prostředí emulátoru výpočtů**.</span><span class="sxs-lookup"><span data-stu-id="edde2-259">You can do this by right-clicking hello emulator icon in hello notification area of your taskbar and selecting **Show Compute Emulator UI**.</span></span>
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a><span data-ttu-id="edde2-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edde2-260">Next steps</span></span>
<span data-ttu-id="edde2-261">toolearn víc o službě Service Bus, najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="edde2-261">toolearn more about Service Bus, see hello following resources:</span></span>  

* <span data-ttu-id="edde2-262">[Dokumentace ke službě Azure Service Bus][sbdocs]</span><span class="sxs-lookup"><span data-stu-id="edde2-262">[Azure Service Bus documentation][sbdocs]</span></span>  
* <span data-ttu-id="edde2-263">[Stránka služby Service Bus][sbacom]</span><span class="sxs-lookup"><span data-stu-id="edde2-263">[Service Bus service page][sbacom]</span></span>  
* <span data-ttu-id="edde2-264">[Jak tooUse fronty služby Service Bus][sbacomqhowto]</span><span class="sxs-lookup"><span data-stu-id="edde2-264">[How tooUse Service Bus Queues][sbacomqhowto]</span></span>  

<span data-ttu-id="edde2-265">toolearn Další informace o víceúrovňových scénářích najdete v části:</span><span class="sxs-lookup"><span data-stu-id="edde2-265">toolearn more about multi-tier scenarios, see:</span></span>  

* <span data-ttu-id="edde2-266">[Vícevrstvá aplikace .NET, která používá tabulky, fronty a objekty blob služby Storage][mutitierstorage]</span><span class="sxs-lookup"><span data-stu-id="edde2-266">[.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs][mutitierstorage]</span></span>  

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
