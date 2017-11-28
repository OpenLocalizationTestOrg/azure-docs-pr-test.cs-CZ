---
title: "aaaGet začít s Mobile Apps pomocí Xamarin.Forms"
description: "Postupujte podle tohoto kurzu toostart používat Mobile Apps pro vývoj s Xamarin.Forms."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: af6b1c1ce4cf91c397552aa3d8ee40728129238c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinforms-app"></a><span data-ttu-id="1ac1b-103">Vytvoření aplikace na platformě Xamarin.Forms</span><span class="sxs-lookup"><span data-stu-id="1ac1b-103">Create a Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="1ac1b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1ac1b-104">Overview</span></span>
<span data-ttu-id="1ac1b-105">Tento kurz ukazuje, jak hello tooadd Cloudová služba back-end tooa Xamarin.Forms mobilní aplikace pomocí funkce Mobile Apps služby Azure App Service jako hello back-end.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-105">This tutorial shows you how tooadd a cloud-based back-end service tooa Xamarin.Forms mobile app by using hello Mobile Apps feature of Azure App Service as hello back end.</span></span> <span data-ttu-id="1ac1b-106">Vytvoříte jak nový back-end Mobile Apps, tak jednoduchou aplikaci Xamarin.Forms, která bude představovat seznam úkolů a ukládat data do Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-106">You create both a new Mobile Apps back end and a simple to-do list Xamarin.Forms app that stores app data in Azure.</span></span>

<span data-ttu-id="1ac1b-107">Ve všech dalších kurzech k Mobile Apps týkajících se Xamarin.Forms se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-107">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Forms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ac1b-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1ac1b-108">Prerequisites</span></span>
<span data-ttu-id="1ac1b-109">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="1ac1b-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="1ac1b-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-110">An active Azure account.</span></span> <span data-ttu-id="1ac1b-111">Pokud nemáte účet, můžete si zaregistrovat zkušební verzi Azure a získat až too10 bezplatné mobilní aplikace, které můžete používat i po skončení zkušebního období.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-111">If you don't have an account, you can sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="1ac1b-112">Další informace najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-112">For more information, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="1ac1b-113">Visual Studio s Xamarinem.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="1ac1b-114">Informace najdete v tématu hello [nastavit až a nainstalujte Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-114">For information, see hello [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) page.</span></span>

* <span data-ttu-id="1ac1b-115">Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="1ac1b-116">Informace najdete v článcích věnovaných [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx) a [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-116">For information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Set up, install, and verify for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-a-new-mobile-apps-back-end"></a><span data-ttu-id="1ac1b-117">Vytvoření nového back-endu Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="1ac1b-117">Create a new Mobile Apps back end</span></span>

<span data-ttu-id="1ac1b-118">toocreate ukončit nové mobilní aplikace zpět hello následující:</span><span class="sxs-lookup"><span data-stu-id="1ac1b-118">toocreate a new Mobile Apps back end, do hello following:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="1ac1b-119">Právě jste nastavili back-end Mobile Apps, který můžou používat vaše mobilní klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-119">You have now set up a Mobile Apps back end that your mobile client applications can use.</span></span> <span data-ttu-id="1ac1b-120">V dalším kroku stáhnout serverový projekt pro jednoduchý úkolů seznamu back-end a pak ho publikujete tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-120">Next, you download a server project for a simple to-do list back end and then publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="1ac1b-121">Konfigurace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="1ac1b-121">Configure hello server project</span></span>

<span data-ttu-id="1ac1b-122">tooconfigure hello serveru projektu toouse hello Node.js, nebo .NET back-end, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1ac1b-122">tooconfigure hello server project toouse either hello Node.js or .NET back end, do hello following:</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinforms-solution"></a><span data-ttu-id="1ac1b-123">Stažení a spuštění řešení Xamarin.Forms hello</span><span class="sxs-lookup"><span data-stu-id="1ac1b-123">Download and run hello Xamarin.Forms solution</span></span>

<span data-ttu-id="1ac1b-124">Můžete si stáhnout hello řešení v některém ze dvou způsobů.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-124">You can download hello solution in either of two ways.</span></span> <span data-ttu-id="1ac1b-125">Stažení tooa Mac a otevřít v aplikaci Xamarin Studio nebo ho stáhnout na počítač se systémem Windows tooa a otevřete v sadě Visual Studio pomocí síťově připojeného počítače Mac pro vytvoření aplikace pro iOS hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-125">Download it tooa Mac and open it in Xamarin Studio, or download it tooa Windows computer and open it in Visual Studio by using a networked Mac for building hello iOS app.</span></span> <span data-ttu-id="1ac1b-126">Další informace najdete v článku věnovaném [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-126">For more information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>

<span data-ttu-id="1ac1b-127">V počítači Mac nebo Windows hello následující:</span><span class="sxs-lookup"><span data-stu-id="1ac1b-127">On a Mac or Windows computer, do hello following:</span></span>

1. <span data-ttu-id="1ac1b-128">Přejděte toohello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="1ac1b-128">Go toohello [Azure portal].</span></span>

2. <span data-ttu-id="1ac1b-129">Na hello **nastavení** své mobilní aplikace, v části **Mobile**, vyberte **Začínáme** > **Xamarin.Forms**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-129">On hello **Settings** blade for your mobile app, under **Mobile**, select **Get Started** > **Xamarin.Forms**.</span></span> <span data-ttu-id="1ac1b-130">V **kroku 3** vyberte **Vytvořit novou aplikaci** a pak vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-130">Under **step 3**, select  **Create a new app**, and then select **Download**.</span></span>

   <span data-ttu-id="1ac1b-131">Tato akce stáhne projekt, který obsahuje klientskou aplikaci, která je připojená tooyour mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-131">This action downloads a project that contains a client application that's connected tooyour mobile app.</span></span> <span data-ttu-id="1ac1b-132">Uložte hello projektu komprimovaný soubor tooyour místního počítače a poznamenejte si kam jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-132">Save hello compressed project file tooyour local computer, and make a note of where you save it.</span></span>

3. <span data-ttu-id="1ac1b-133">Extrahujte hello projekt, který jste stáhli a potom ho otevřete v nástroji Xamarin Studio (Mac) nebo Visual Studio (Windows).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-133">Extract hello project that you downloaded, and then open it in Xamarin Studio (Mac) or Visual Studio (Windows).</span></span>

   ![Extrahovaný projekt v nástroji Xamarin Studio][9]

   ![Extrahovaný projekt v sadě Visual Studio][8]

## <a name="optional-run-hello-ios-project"></a><span data-ttu-id="1ac1b-136">(Volitelné) Spuštění projektu iOS hello</span><span class="sxs-lookup"><span data-stu-id="1ac1b-136">(Optional) Run hello iOS project</span></span>
<span data-ttu-id="1ac1b-137">V této části spustíte hello projektu Xamarin iOS pro zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-137">In this section, you run hello Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="1ac1b-138">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-138">You can skip this section if you are not working with iOS devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="1ac1b-139">V nástroji Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="1ac1b-139">In Xamarin Studio</span></span>
1. <span data-ttu-id="1ac1b-140">Klikněte pravým tlačítkem na projekt pro iOS hello a potom vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-140">Right-click hello iOS project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="1ac1b-141">Na hello **spustit** nabídce vyberte možnost **spustit ladění** toobuild hello projektu a spusťte hello aplikaci v emulátoru Iphonu hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-141">On hello **Run** menu, select **Start Debugging** toobuild hello project and start hello app in hello iPhone emulator.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="1ac1b-142">V nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ac1b-142">In Visual Studio</span></span>
1. <span data-ttu-id="1ac1b-143">Klikněte pravým tlačítkem na projekt pro iOS hello a potom vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-143">Right-click hello iOS project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="1ac1b-144">Na hello **sestavení** nabídce vyberte možnost **nástroje Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-144">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="1ac1b-145">V hello **nástroje Configuration Manager** dialogové okno, vyberte hello **sestavení** a **nasadit** projektu iOS toohello další zaškrtávací políčka.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-145">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello iOS project.</span></span>

4. <span data-ttu-id="1ac1b-146">toobuild hello projektu a spusťte hello aplikaci v emulátoru Iphonu hello, vyberte hello **F5** klíč.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-146">toobuild hello project and start hello app in hello iPhone emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1ac1b-147">Pokud máte potíže s sestavení projektu hello, spusťte hello NuGet balíček správce a aktualizace toohello nejnovější verzi podpůrných balíčků Xamarin hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-147">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="1ac1b-148">Projekty rychlý start může být pomalé tooupdate toohello nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-148">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="1ac1b-149">Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*, a pak vyberte hello znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-149">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    ![][10]

    <span data-ttu-id="1ac1b-150">Tato akce odešle toohello požadavek post nové Mobile Apps back-end, který je hostován v Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-150">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="1ac1b-151">Data z požadavku hello je vloženy do tabulky TodoItem hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-151">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="1ac1b-152">Položky, které jsou uložené v tabulce hello se vrátí pomocí hello Mobile Apps zpět ukončení a text hello, že data se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-152">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ac1b-153">Najdete tu hello kód, který přistupuje k Mobile Apps back-end vašeho v hello souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd hello vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-153">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-hello-android-project"></a><span data-ttu-id="1ac1b-154">(Volitelné) Spusťte projekt Android hello</span><span class="sxs-lookup"><span data-stu-id="1ac1b-154">(Optional) Run hello Android project</span></span>
<span data-ttu-id="1ac1b-155">V této části spuštění projektu Xamarin Android hello pro Android.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-155">In this section, you run hello Xamarin droid project for Android.</span></span> <span data-ttu-id="1ac1b-156">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-156">You can skip this section if you are not working with Android devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="1ac1b-157">V nástroji Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="1ac1b-157">In Xamarin Studio</span></span>

1. <span data-ttu-id="1ac1b-158">Klikněte pravým tlačítkem na projekt pro Android hello a potom vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-158">Right-click hello Android project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="1ac1b-159">toobuild hello projektu a spusťte aplikaci hello v Android emulátoru, na hello **spustit** nabídce vyberte možnost **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-159">toobuild hello project and start hello app in an Android emulator, on hello **Run** menu, select **Start Debugging**.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="1ac1b-160">V nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ac1b-160">In Visual Studio</span></span>

1. <span data-ttu-id="1ac1b-161">Klikněte pravým tlačítkem na projekt hello Android (Droid) a potom vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-161">Right-click hello Android (Droid) project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="1ac1b-162">Na hello **sestavení** nabídce vyberte možnost **nástroje Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-162">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="1ac1b-163">V hello **nástroje Configuration Manager** dialogové okno, vyberte hello **sestavení** a **nasadit** zaškrtávací políčka další toohello projekt pro Android.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-163">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello Android project.</span></span>

4. <span data-ttu-id="1ac1b-164">toobuild hello projektu a spusťte aplikaci hello v Android emulátoru, vyberte hello **F5** klíč.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-164">toobuild hello project and start hello app in an Android emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1ac1b-165">Pokud máte potíže s sestavení projektu hello, spusťte hello NuGet balíček správce a aktualizace toohello nejnovější verzi podpůrných balíčků Xamarin hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-165">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="1ac1b-166">Projekty rychlý start může být pomalé tooupdate toohello nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-166">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="1ac1b-167">Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*, a pak vyberte hello znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-167">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    ![][11]
    
    <span data-ttu-id="1ac1b-168">Tato akce odešle toohello požadavek post nové Mobile Apps back-end, který je hostován v Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-168">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="1ac1b-169">Data z požadavku hello je vloženy do tabulky TodoItem hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-169">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="1ac1b-170">Položky, které jsou uložené v tabulce hello se vrátí pomocí hello Mobile Apps zpět ukončení a text hello, že data se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-170">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="1ac1b-171">Najdete tu hello kód, který přistupuje k Mobile Apps back-end vašeho v hello souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd hello vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-171">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-hello-windows-project"></a><span data-ttu-id="1ac1b-172">(Volitelné) Spusťte projekt Windows hello</span><span class="sxs-lookup"><span data-stu-id="1ac1b-172">(Optional) Run hello Windows project</span></span>

<span data-ttu-id="1ac1b-173">V této části spuštění projektu Xamarin WinApp pro zařízení se systémem Windows hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-173">In this section, you run hello Xamarin WinApp project for Windows devices.</span></span> <span data-ttu-id="1ac1b-174">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="1ac1b-175">V nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ac1b-175">In Visual Studio</span></span>

1. <span data-ttu-id="1ac1b-176">Klikněte pravým tlačítkem na projekty pro Windows hello a potom vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-176">Right-click any of hello Windows projects, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="1ac1b-177">Na hello **sestavení** nabídce vyberte možnost **nástroje Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-177">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="1ac1b-178">V hello **nástroje Configuration Manager** dialogové okno, vyberte hello **sestavení** a **nasadit** zaškrtávací políčka další toohello Windows projekt, který jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-178">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello Windows project that you chose.</span></span>

4. <span data-ttu-id="1ac1b-179">toobuild hello projektu a spusťte hello aplikaci v emulátoru systému Windows, vyberte hello **F5** klíč.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-179">toobuild hello project and start hello app in a Windows emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1ac1b-180">Pokud máte potíže s sestavení projektu hello, spusťte hello NuGet balíček správce a aktualizace toohello nejnovější verzi podpůrných balíčků Xamarin hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-180">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="1ac1b-181">Projekty rychlý start může být pomalé tooupdate toohello nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-181">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="1ac1b-182">Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*, a pak vyberte hello znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="1ac1b-182">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    <span data-ttu-id="1ac1b-183">Tato akce odešle toohello požadavek post nové Mobile Apps back-end, který je hostován v Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-183">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="1ac1b-184">Data z požadavku hello je vloženy do tabulky TodoItem hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-184">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="1ac1b-185">Položky, které jsou uložené v tabulce hello se vrátí pomocí hello Mobile Apps zpět ukončení a text hello, že data se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-185">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>
    
    ![][12]
    
    > [!NOTE]
    > <span data-ttu-id="1ac1b-186">Najdete tu hello kód, který přistupuje k Mobile Apps back-end vašeho v hello souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd hello vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-186">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="next-steps"></a><span data-ttu-id="1ac1b-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ac1b-187">Next steps</span></span>

* [<span data-ttu-id="1ac1b-188">Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="1ac1b-188">Add authentication tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="1ac1b-189">Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-189">Learn how tooauthenticate users of your app with an identity provider.</span></span>

* [<span data-ttu-id="1ac1b-190">Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="1ac1b-190">Add push notifications tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)  
  <span data-ttu-id="1ac1b-191">Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend hello nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-191">Learn how tooadd push notifications support tooyour app and configure your Mobile Apps back end toouse Azure Notification Hubs toosend hello push notifications.</span></span>

* [<span data-ttu-id="1ac1b-192">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="1ac1b-192">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="1ac1b-193">Zjistěte, jak tooadd offline podporu pro vaši aplikaci pomocí Mobile Apps back-end.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-193">Learn how tooadd offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="1ac1b-194">Offline synchronizace umožňuje zobrazovat, přidávat nebo upravovat data mobilní aplikace i když nemá připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-194">With offline sync, you can view, add, or modify your mobile app's data, even when there is no network connection.</span></span>

* [<span data-ttu-id="1ac1b-195">Použití hello spravovaného klienta pro mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="1ac1b-195">Use hello managed client for Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)  
  <span data-ttu-id="1ac1b-196">Zjistěte, jak spravovat toowork s hello klienta SDK v aplikaci Xamarin.</span><span class="sxs-lookup"><span data-stu-id="1ac1b-196">Learn how toowork with hello managed client SDK in your Xamarin app.</span></span>

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[portál Azure]: https://portal.azure.com/
