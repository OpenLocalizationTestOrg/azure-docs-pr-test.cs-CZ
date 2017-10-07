---
title: "aaaCreate univerzální platformu Windows (UWP) používající v Mobile Apps | Microsoft Docs"
description: "Využijte tento kurz tooget začít s pomocí back-EndY mobilní aplikace Azure pro vývoj aplikací pro univerzální platformu Windows (UWP) v C#, Visual Basic nebo JavaScript."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: d0f57bca5a8195b8b0461b8f7a0d8516371d56a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-app"></a><span data-ttu-id="83e80-103">Vytvoření aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="83e80-103">Create a Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="83e80-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="83e80-104">Overview</span></span>
<span data-ttu-id="83e80-105">Tento kurz ukazuje, jak tooadd back-end cloudové služby tooa univerzální platformu Windows (UWP) aplikace.</span><span class="sxs-lookup"><span data-stu-id="83e80-105">This tutorial shows you how tooadd a cloud-based backend service tooa Universal Windows Platform (UWP) app.</span></span> <span data-ttu-id="83e80-106">Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="83e80-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span> <span data-ttu-id="83e80-107">Následující Hello jsou z aplikace hello dokončit zachycení obrazovky:</span><span class="sxs-lookup"><span data-stu-id="83e80-107">hello following are screen captures from hello completed app:</span></span>

![Hotová desktopová aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
<span data-ttu-id="83e80-109">Spuštěná na ploše</span><span class="sxs-lookup"><span data-stu-id="83e80-109">Running on a desktop.</span></span>

![Hotová telefonní aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
<span data-ttu-id="83e80-111">Spuštěná na telefonu</span><span class="sxs-lookup"><span data-stu-id="83e80-111">Running on a phone</span></span>

<span data-ttu-id="83e80-112">Ve všech dalších kurzech Mobile App pro aplikace UPW se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="83e80-112">Completing this tutorial is a prerequisite for all other Mobile App tutorials for UWP apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83e80-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83e80-113">Prerequisites</span></span>
<span data-ttu-id="83e80-114">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="83e80-114">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="83e80-115">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="83e80-115">An active Azure account.</span></span> <span data-ttu-id="83e80-116">Pokud nemáte účet, můžete si zaregistrovat zkušební verzi Azure a získat až too10 bezplatné mobilní aplikace, které můžete používat i po skončení zkušebního období.</span><span class="sxs-lookup"><span data-stu-id="83e80-116">If you don't have an account, you can sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="83e80-117">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="83e80-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="83e80-118">[Visual Studio Community 2015] nebo novější verzi</span><span class="sxs-lookup"><span data-stu-id="83e80-118">[Visual Studio Community 2015] or a later version.</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="83e80-119">Vytvoření nového back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="83e80-119">Create a new Azure Mobile App backend</span></span>
<span data-ttu-id="83e80-120">Postupujte podle těchto kroků toocreate nový back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="83e80-120">Follow these steps toocreate a new Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="83e80-121">Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="83e80-121">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="83e80-122">Dále si stáhnete serverový projekt pro jednoduchý "seznam úkolů" back-end a publikujete ho v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="83e80-122">Next, you will download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="83e80-123">Konfigurace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="83e80-123">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-client-project"></a><span data-ttu-id="83e80-124">Stažení a spuštění projektu klienta hello</span><span class="sxs-lookup"><span data-stu-id="83e80-124">Download and run hello client project</span></span>
<span data-ttu-id="83e80-125">Jakmile jste nakonfigurovali váš back-end mobilní aplikace, můžete vytvořit novou klientskou aplikaci nebo upravovat existující tooconnect tooAzure aplikace.</span><span class="sxs-lookup"><span data-stu-id="83e80-125">Once you have configured your Mobile App backend, you can either create a new client app or modify an existing app tooconnect tooAzure.</span></span> <span data-ttu-id="83e80-126">V této části stáhnete projekt šablony aplikace UPW, který je přizpůsobené tooconnect tooyour mobilní aplikace back-end.</span><span class="sxs-lookup"><span data-stu-id="83e80-126">In this section, you download a UWP app template project that is customized tooconnect tooyour Mobile App backend.</span></span>

1. <span data-ttu-id="83e80-127">Zpět v hello **úvodní** back-endu mobilní aplikace, klikněte na **vytvořit novou aplikaci** > **Stáhnout**, extrahujte hello komprimované soubory projektu tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="83e80-127">Back in hello **Quick start** blade for your Mobile App backend, click **Create a new app** > **Download**, then extract hello compressed project files tooyour local computer.</span></span>

    ![Stáhnutí projektu typu rychlý start pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. <span data-ttu-id="83e80-129">(Volitelné) Přidat toohello projekt aplikace UPW hello stejného řešení jako hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="83e80-129">(Optional) Add hello UWP app project toohello same solution as hello server project.</span></span> <span data-ttu-id="83e80-130">To usnadňuje toodebug a testovací obě hello aplikace a hello back-end v hello stejné řešení sady Visual Studio, pokud si zvolíte toodo tak.</span><span class="sxs-lookup"><span data-stu-id="83e80-130">This makes it easier toodebug and test both hello app and hello backend in hello same Visual Studio solution, if you choose toodo so.</span></span> <span data-ttu-id="83e80-131">tooadd řešení toohello projekt aplikace UPW, používejte Visual Studio 2015 nebo novější verze.</span><span class="sxs-lookup"><span data-stu-id="83e80-131">tooadd a UWP app project toohello solution, you must be using Visual Studio 2015 or a later version.</span></span>
3. <span data-ttu-id="83e80-132">Hello aplikace pro UPW jako spouštěný projekt hello stiskněte klíče toodeploy hello F5 a spusťte hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="83e80-132">With hello UWP app as hello startup project, press hello F5 key toodeploy and run hello app.</span></span>
4. <span data-ttu-id="83e80-133">Hello aplikace zadejte smysluplný text, například *hello dokončení kurzu*, v hello **vložení úkolu** textového pole a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="83e80-133">In hello app, type meaningful text, such as *Complete hello tutorial*, in hello **Insert a TodoItem** text box, and then click **Save**.</span></span>

    ![Dokončení desktopového projektu s rychlým startem pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    <span data-ttu-id="83e80-135">Tím se odešle POST požadavek toohello nový mobilní aplikace back-end hostovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="83e80-135">This sends a POST request toohello new mobile app backend that's hosted in Azure.</span></span>
5. <span data-ttu-id="83e80-136">(Volitelné) Aplikace hello zastavte a restartujte ji na jiném zařízení nebo emulátoru mobilního.</span><span class="sxs-lookup"><span data-stu-id="83e80-136">(Optional) Stop hello app and restart it on a different device or mobile emulator.</span></span>

    ![Dokončení telefonního projektu s rychlým startem pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    <span data-ttu-id="83e80-138">Všimněte si, že data uložená hello v předchozím kroku načetla z Azure po spuštění aplikace pro UPW hello.</span><span class="sxs-lookup"><span data-stu-id="83e80-138">Notice that data saved from hello previous step is loaded from Azure after hello UWP app starts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83e80-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83e80-139">Next steps</span></span>
* [<span data-ttu-id="83e80-140">Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="83e80-140">Add authentication tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="83e80-141">Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="83e80-141">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="83e80-142">Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="83e80-142">Add push notifications tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="83e80-143">Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="83e80-143">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="83e80-144">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="83e80-144">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="83e80-145">Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="83e80-145">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="83e80-146">Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="83e80-146">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
