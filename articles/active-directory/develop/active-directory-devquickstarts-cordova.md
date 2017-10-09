---
title: "aaaAzure AD Cordova Začínáme | Microsoft Docs"
description: "Jak toobuild aplikace Cordova, se integruje se službou Azure AD pro přihlášení a zavolá rozhraní API Azure AD chráněné pomocí OAuth."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: 573ed638c2180c5231648bcb8c49ceb6f53296f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a><span data-ttu-id="ac3a6-103">Integrace Azure AD s platformě Apache Cordova app</span><span class="sxs-lookup"><span data-stu-id="ac3a6-103">Integrate Azure AD with an Apache Cordova app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="ac3a6-104">Můžete použít Apache Cordova toodevelop HTML5/JavaScript aplikace, které můžou běžet na mobilních zařízeních jako plně kvalifikované nativních aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-104">You can use Apache Cordova toodevelop HTML5/JavaScript applications that can run on mobile devices as full-fledged native applications.</span></span> <span data-ttu-id="ac3a6-105">S Azure Active Directory (Azure AD) můžete přidat podnikové úrovni ověřování možnosti tooyour Cordova aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-105">With Azure Active Directory (Azure AD), you can add enterprise-grade authentication capabilities tooyour Cordova applications.</span></span>

<span data-ttu-id="ac3a6-106">Modulu plug-in Cordova zabalí Azure AD nativních sad SDK pro iOS, Android, Windows Store a Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-106">A Cordova plug-in wraps Azure AD native SDKs on iOS, Android, Windows Store, and Windows Phone.</span></span> <span data-ttu-id="ac3a6-107">Pomocí, že modul plug-in, můžete vylepšit aplikace toosupport přihlašování pomocí účtů služby Windows Server Active Directory, získat přístup k tooOffice 365 a rozhraní API Správce Azure vašich uživatelů a to i v ochraně volání tooyour vlastní vlastní rozhraní web API.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-107">By using that plug-in, you can enhance your application toosupport sign-in with your users' Windows Server Active Directory accounts, gain access tooOffice 365 and Azure APIs, and even help protect calls tooyour own custom web API.</span></span>

<span data-ttu-id="ac3a6-108">V tomto kurzu použijeme hello Apache Cordova modulu plug-in pro Active Directory Authentication Library (ADAL) tooimprove jednoduchou aplikaci přidáním hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-108">In this tutorial, we'll use hello Apache Cordova plug-in for Active Directory Authentication Library (ADAL) tooimprove a simple app by adding hello following features:</span></span>

* <span data-ttu-id="ac3a6-109">Pomocí několika řádků kódu ověření uživatele a získat token.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-109">With just a few lines of code, authenticate a user and obtain a token.</span></span>
* <span data-ttu-id="ac3a6-110">Pomocí tohoto tokenu tooinvoke hello rozhraní Graph API tooquery adresáře a zobrazit výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-110">Use that token tooinvoke hello Graph API tooquery that directory and display hello results.</span></span>  
* <span data-ttu-id="ac3a6-111">Použít hello ADAL mezipamětí tokenů toominimize ověřování vyzve k zadání uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-111">Use hello ADAL token cache toominimize authentication prompts for hello user.</span></span>

<span data-ttu-id="ac3a6-112">toomake těchto vylepšení, budete muset:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-112">toomake those improvements, you need to:</span></span>

1. <span data-ttu-id="ac3a6-113">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-113">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="ac3a6-114">Přidejte kód tooyour aplikace toorequest tokeny.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-114">Add code tooyour app toorequest tokens.</span></span>
3. <span data-ttu-id="ac3a6-115">Přidání kódu toouse hello tokenu pro dotazování hello rozhraní Graph API a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-115">Add code toouse hello token for querying hello Graph API and display results.</span></span>
4. <span data-ttu-id="ac3a6-116">Vytvoření projektu nasazení hello Cordova s všechny platformy hello má tootarget, přidejte hello Cordova ADAL modulu plug-in a testování hello řešení v emulátorů.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-116">Create hello Cordova deployment project with all hello platforms you want tootarget, add hello Cordova ADAL plug-in, and test hello solution in emulators.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac3a6-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ac3a6-117">Prerequisites</span></span>
<span data-ttu-id="ac3a6-118">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-118">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="ac3a6-119">Klient služby Azure AD, kde máte účet s právy pro vývoj aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-119">An Azure AD tenant where you have an account with app development rights.</span></span>
* <span data-ttu-id="ac3a6-120">Vývojové prostředí, který byl nakonfigurován toouse Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-120">A development environment that's configured toouse Apache Cordova.</span></span>  

<span data-ttu-id="ac3a6-121">Jak už máte-li nastavit, pokračovat přímo toostep 1.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-121">If you have both already set up, proceed directly toostep 1.</span></span>

<span data-ttu-id="ac3a6-122">Pokud nemáte klient služby Azure AD, použijte hello [pokyny, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-122">If you don't have an Azure AD tenant, use hello [instructions on how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="ac3a6-123">Pokud nemáte nastavení na počítači pro Apache Cordova, nainstalujte hello následující:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-123">If you don't have Apache Cordova set up on your machine, install hello following:</span></span>

* [<span data-ttu-id="ac3a6-124">Git</span><span class="sxs-lookup"><span data-stu-id="ac3a6-124">Git</span></span>](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [<span data-ttu-id="ac3a6-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="ac3a6-125">Node.js</span></span>](https://nodejs.org/download/)
* <span data-ttu-id="ac3a6-126">[Rozhraní příkazového řádku Cordova](https://cordova.apache.org/) (se dá snadno nainstalovat prostřednictvím Správce balíčku NPM: `npm install -g cordova`)</span><span class="sxs-lookup"><span data-stu-id="ac3a6-126">[Cordova CLI](https://cordova.apache.org/) (can be easily installed via NPM package manager: `npm install -g cordova`)</span></span>

<span data-ttu-id="ac3a6-127">Před instalací Hello by měly fungovat na hello PC i na hello Mac.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-127">hello preceding installations should work both on hello PC and on hello Mac.</span></span>

<span data-ttu-id="ac3a6-128">Každý Cílová platforma má jiné předpoklady:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-128">Each target platform has different prerequisites:</span></span>

* <span data-ttu-id="ac3a6-129">toobuild a spusťte aplikaci pro Windows Tabletu nebo Windows Phone:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-129">toobuild and run an app for Windows Tablet/PC or Windows Phone:</span></span>
  * <span data-ttu-id="ac3a6-130">Nainstalujte [Visual Studio 2013 pro Windows s aktualizací 2 nebo novější](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express nebo jinou verzi) nebo [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-130">Install [Visual Studio 2013 for Windows with Update 2 or later](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express or another version) or [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span></span>

* <span data-ttu-id="ac3a6-131">toobuild a spusťte aplikaci pro iOS:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-131">toobuild and run an app for iOS:</span></span>

  * <span data-ttu-id="ac3a6-132">Instalaci Xcode 6.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-132">Install Xcode 6.x or later.</span></span> <span data-ttu-id="ac3a6-133">Stáhnout z hello [vývojáře Apple lokality](http://developer.apple.com/downloads) nebo hello [Mac App Storu](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-133">Download it from hello [Apple Developer site](http://developer.apple.com/downloads) or hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span></span>
  * <span data-ttu-id="ac3a6-134">Nainstalujte [ios-sim](https://www.npmjs.org/package/ios-sim).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-134">Install [ios-sim](https://www.npmjs.org/package/ios-sim).</span></span> <span data-ttu-id="ac3a6-135">Můžete ho toostart aplikací pro iOS v simulátoru iOS z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-135">You can use it toostart iOS apps in iOS Simulator from hello command line.</span></span> <span data-ttu-id="ac3a6-136">(Můžete ho snadno nainstalovat prostřednictvím hello terminálu: `npm install -g ios-sim`.)</span><span class="sxs-lookup"><span data-stu-id="ac3a6-136">(You can easily install it via hello terminal: `npm install -g ios-sim`.)</span></span>
* <span data-ttu-id="ac3a6-137">toobuild a spusťte aplikaci pro Android:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-137">toobuild and run an app for Android:</span></span>

  * <span data-ttu-id="ac3a6-138">Nainstalujte [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-138">Install [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or later.</span></span> <span data-ttu-id="ac3a6-139">Zajistěte, aby `JAVA_HOME` (proměnnou prostředí) je správně nastavena podle toohello JDK instalační cestu (například C:\Program Files\Java\jdk1.7.0_75).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-139">Make sure `JAVA_HOME` (environment variable) is correctly set according toohello JDK installation path (for example, C:\Program Files\Java\jdk1.7.0_75).</span></span>
  * <span data-ttu-id="ac3a6-140">Nainstalujte [sady SDK pro Android](http://developer.android.com/sdk/installing/index.html?pkg=tools) a přidejte hello `<android-sdk-location>\tools` umístění (například C:\tools\Android\android-sdk\tools) tooyour `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-140">Install [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) and add hello `<android-sdk-location>\tools` location (for example, C:\tools\Android\android-sdk\tools) tooyour `PATH` environment variable.</span></span>
  * <span data-ttu-id="ac3a6-141">Otevřete Android SDK Manageru (například prostřednictvím hello terminálu: `android`) a nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-141">Open Android SDK Manager (for example, via hello terminal: `android`) and install:</span></span>
    * <span data-ttu-id="ac3a6-142">*Android – 5.0.1 (rozhraní API 21)* SDK platformy</span><span class="sxs-lookup"><span data-stu-id="ac3a6-142">*Android 5.0.1 (API 21)* platform SDK</span></span>
    * <span data-ttu-id="ac3a6-143">*Nástroje sestavení Android SDK* verze 19.1.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="ac3a6-143">*Android SDK Build Tools* version 19.1.0 or later</span></span>
    * <span data-ttu-id="ac3a6-144">*Podpora pro Android úložiště* (funkce)</span><span class="sxs-lookup"><span data-stu-id="ac3a6-144">*Android Support Repository* (Extras)</span></span>

  <span data-ttu-id="ac3a6-145">Hello Android SDK neposkytuje žádné výchozí emulátor instance.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-145">hello Android SDK doesn't provide any default emulator instance.</span></span> <span data-ttu-id="ac3a6-146">Vytvořit spuštěním `android avd` z hello terminálu a potom vyberete **vytvořit**, pokud chcete, aby toorun hello aplikace pro Android na emulátor.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-146">Create one by running `android avd` from hello terminal and then selecting **Create**, if you want toorun hello Android app on an emulator.</span></span> <span data-ttu-id="ac3a6-147">Doporučujeme, abyste API úrovně 19 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-147">We recommend an API level of 19 or higher.</span></span> <span data-ttu-id="ac3a6-148">Další informace o možnostech Android emulátoru a vytvoření hello najdete v tématu [správce AVD](http://developer.android.com/tools/help/avd-manager.html) na lokality Android hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-148">For more information about hello Android emulator and creation options, see [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) on hello Android site.</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="ac3a6-149">Krok 1: Zaregistrujte aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac3a6-149">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="ac3a6-150">Tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-150">This step is optional.</span></span> <span data-ttu-id="ac3a6-151">Tento kurz obsahuje předem zřízená hodnoty, které můžete použít toosee hello ukázka v akci bez provádění zřizování v vlastního klienta.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-151">This tutorial provides pre-provisioned values that you can use toosee hello sample in action without doing any provisioning in your own tenant.</span></span> <span data-ttu-id="ac3a6-152">Ale doporučujeme provést tento krok a seznámit se s hello procesu, protože se bude vyžadovat, při vytváření vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-152">However, we recommend that you do perform this step and become familiar with hello process, because it will be required when you create your own applications.</span></span>

<span data-ttu-id="ac3a6-153">Azure AD vydá tokeny tooonly známé aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-153">Azure AD issues tokens tooonly known applications.</span></span> <span data-ttu-id="ac3a6-154">Než budete moct použít Azure AD z vaší aplikace, musíte toocreate položku pro něj ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-154">Before you can use Azure AD from your app, you need toocreate an entry for it in your tenant.</span></span> <span data-ttu-id="ac3a6-155">tooregister novou aplikaci v klientovi služby:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-155">tooregister a new application in your tenant:</span></span>

1. <span data-ttu-id="ac3a6-156">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-156">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ac3a6-157">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-157">On hello top bar, click your account.</span></span> <span data-ttu-id="ac3a6-158">V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-158">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="ac3a6-159">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-159">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ac3a6-160">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-160">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="ac3a6-161">Postupujte podle pokynů hello a vytvořit **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-161">Follow hello prompts and create a **Native Client Application**.</span></span> <span data-ttu-id="ac3a6-162">(I když jsou aplikace Cordova na základě HTML, vytváříme nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-162">(Although Cordova apps are HTML based, we're creating a native client application here.</span></span> <span data-ttu-id="ac3a6-163">Hello **nativní klientská aplikace** musí být vybraná volba nebo hello aplikace nebude fungovat.)</span><span class="sxs-lookup"><span data-stu-id="ac3a6-163">hello **Native Client Application** option must be selected, or hello application won't work.)</span></span>
  * <span data-ttu-id="ac3a6-164">**Název** popisuje toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-164">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="ac3a6-165">**Identifikátor URI pro přesměrování** je hello identifikátor URI, který byl použit tooreturn tokeny tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-165">**Redirect URI** is hello URI that's used tooreturn tokens tooyour app.</span></span> <span data-ttu-id="ac3a6-166">Zadejte **http://MyDirectorySearcherApp**.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-166">Enter **http://MyDirectorySearcherApp**.</span></span>

<span data-ttu-id="ac3a6-167">Po dokončení registrace přiřadí Azure AD aplikace jedinečné ID tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-167">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span> <span data-ttu-id="ac3a6-168">Budete potřebovat tuto hodnotu v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-168">You’ll need this value in hello next sections.</span></span> <span data-ttu-id="ac3a6-169">Najdete ho na kartě aplikace hello hello nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-169">You can find it on hello application tab of hello newly created app.</span></span>

<span data-ttu-id="ac3a6-170">toorun `DirSearchClient Sample`, udělte hello nově vytvořený aplikaci oprávnění tooquery hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-170">toorun `DirSearchClient Sample`, grant hello newly created app permission tooquery hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="ac3a6-171">Z hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-171">From hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>  
2. <span data-ttu-id="ac3a6-172">Hello aplikaci Azure Active Directory, vyberte **Microsoft Graph** jako hello rozhraní API a přidejte hello **přístup k adresáři hello jako hello přihlášeného uživatele** oprávnění v rámci **delegovaní Oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-172">For hello Azure Active Directory application, select **Microsoft Graph** as hello API and add hello **Access hello directory as hello signed-in user** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="ac3a6-173">To umožňuje vaší aplikace tooquery hello rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-173">This enables your application tooquery hello Graph API for users.</span></span>

## <a name="step-2-clone-hello-sample-app-repository"></a><span data-ttu-id="ac3a6-174">Krok 2: Klonování hello ukázkové aplikace úložiště</span><span class="sxs-lookup"><span data-stu-id="ac3a6-174">Step 2: Clone hello sample app repository</span></span>
<span data-ttu-id="ac3a6-175">Vaše prostředí nebo na příkazovém řádku zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-175">From your shell or command line, type hello following command:</span></span>

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-hello-cordova-app"></a><span data-ttu-id="ac3a6-176">Krok 3: Vytvoření aplikace Cordova hello</span><span class="sxs-lookup"><span data-stu-id="ac3a6-176">Step 3: Create hello Cordova app</span></span>
<span data-ttu-id="ac3a6-177">Existuje několik způsobů toocreate Cordova aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-177">There are multiple ways toocreate Cordova applications.</span></span> <span data-ttu-id="ac3a6-178">V tomto kurzu použijeme hello Cordova rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-178">In this tutorial, we'll use hello Cordova command-line interface (CLI).</span></span>

1. <span data-ttu-id="ac3a6-179">Vaše prostředí nebo na příkazovém řádku zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-179">From your shell or command line, type hello following command:</span></span>

        cordova create DirSearchClient

   <span data-ttu-id="ac3a6-180">Tento příkaz vytvoří strukturu složek hello a generování uživatelského rozhraní pro projekt Cordova hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-180">That command creates hello folder structure and scaffolding for hello Cordova project.</span></span>

2. <span data-ttu-id="ac3a6-181">Přesunutí nové složky DirSearchClient toohello:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-181">Move toohello new DirSearchClient folder:</span></span>

        cd .\DirSearchClient

3. <span data-ttu-id="ac3a6-182">Kopírovat obsah hello hello starter projektu v podsložce www hello pomocí Správce souborů nebo hello následující příkaz do vašeho prostředí:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-182">Copy hello content of hello starter project in hello www subfolder by using a file manager or hello following command in your shell:</span></span>

  * <span data-ttu-id="ac3a6-183">Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-183">Windows: `xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span></span>
  * <span data-ttu-id="ac3a6-184">Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-184">Mac: `cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span></span>

4. <span data-ttu-id="ac3a6-185">Přidejte hello povolených modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-185">Add hello whitelist plug-in.</span></span> <span data-ttu-id="ac3a6-186">To je nezbytné pro vyvolání hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-186">This is necessary for invoking hello Graph API.</span></span>

        cordova plugin add cordova-plugin-whitelist

5. <span data-ttu-id="ac3a6-187">Přidejte všechny hello platformy, které chcete toosupport.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-187">Add all hello platforms that you want toosupport.</span></span> <span data-ttu-id="ac3a6-188">toohave pracovní vzorek, musíte tooexecute alespoň jeden Dobrý den, následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-188">toohave a working sample, you need tooexecute at least one of hello following commands.</span></span> <span data-ttu-id="ac3a6-189">Poznámka: nebude se moct tooemulate iOS v systému Windows nebo emulovat Windows v počítačích Mac.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-189">Note that you won't be able tooemulate iOS on Windows or emulate Windows on a Mac.</span></span>

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. <span data-ttu-id="ac3a6-190">Přidejte hello ADAL pro modul plug-in tooyour projektu Cordova:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-190">Add hello ADAL for Cordova plug-in tooyour project:</span></span>

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-tooauthenticate-users-and-obtain-tokens-from-azure-ad"></a><span data-ttu-id="ac3a6-191">Krok 4: Přidejte kód tooauthenticate uživatele a získat tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac3a6-191">Step 4: Add code tooauthenticate users and obtain tokens from Azure AD</span></span>
<span data-ttu-id="ac3a6-192">Hello aplikace, které vyvíjíte v tomto kurzu vám poskytne jednoduchý directory funkce vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-192">hello application that you're developing in this tutorial will provide a simple directory search feature.</span></span> <span data-ttu-id="ac3a6-193">Hello uživatele můžete pak zadejte alias hello všechny uživatele v adresáři hello a vizualizovat některé základní atributy.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-193">hello user can then type hello alias of any user in hello directory and visualize some basic attributes.</span></span> <span data-ttu-id="ac3a6-194">Hello starter projekt obsahuje definici hello hello základní uživatelské rozhraní aplikace hello (ve www/index.html) a hello generování uživatelského rozhraní, která sváže základní aplikaci událostí cyklů vazby uživatelské rozhraní a výsledky zobrazení logiku (v www/js/index.js).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-194">hello starter project contains hello definition of hello basic user interface of hello app (in www/index.html) and hello scaffolding that wires up basic app event cycles, user interface bindings, and results display logic (in www/js/index.js).</span></span> <span data-ttu-id="ac3a6-195">Hello pouze úloha ponecháno pro vás je tooadd hello logiky, která implementuje identity úlohy.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-195">hello only task left for you is tooadd hello logic that implements identity tasks.</span></span>

<span data-ttu-id="ac3a6-196">Hello všeho nejdřív musíte toodo ve vašem kódu je zavést hello protokol hodnoty, které používá Azure AD pro identifikaci vaší aplikace a prostředky text hello, že cíl je.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-196">hello first thing you need toodo in your code is introduce hello protocol values that Azure AD uses for identifying your app and hello resources that you target.</span></span> <span data-ttu-id="ac3a6-197">Tyto hodnoty budou použité tooconstruct žádosti o tokeny hello později.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-197">Those values will be used tooconstruct hello token requests later on.</span></span> <span data-ttu-id="ac3a6-198">Vložte následující fragment kódu hello horní části souboru index.js hello hello:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-198">Insert hello following snippet at hello top of hello index.js file:</span></span>

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

<span data-ttu-id="ac3a6-199">Hello `redirectUri` a `clientId` hodnoty by měla odpovídat hello hodnoty, které popisují aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-199">hello `redirectUri` and `clientId` values should match hello values that describe your app in Azure AD.</span></span> <span data-ttu-id="ac3a6-200">Můžete najít ty z hello **konfigurace** kartě v hello portál Azure, jak je popsáno v kroku 1 v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-200">You can find those from hello **Configure** tab in hello Azure portal, as described in step 1 earlier in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="ac3a6-201">Pokud jste se rozhodli pro novou aplikaci není registraci v vlastního klienta, můžete jednoduše vložit hello předkonfigurované hodnoty, jako je.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-201">If you opted for not registering a new app in your own tenant, you can simply paste hello preconfigured values as is.</span></span> <span data-ttu-id="ac3a6-202">Můžete se podívat, hello ukázka spuštěn, když měli vždycky vytvořit vlastní položku pro aplikace, které jsou určené pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-202">You can then see hello sample running, though you should always create your own entry for your apps that are meant for production.</span></span>

<span data-ttu-id="ac3a6-203">Dál přidejte kód žádosti o token hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-203">Next, add hello token request code.</span></span> <span data-ttu-id="ac3a6-204">Vložte následující fragment kódu mezi hello hello `search` a `renderData` definice:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-204">Insert hello following snippet between hello `search` and `renderData` definitions:</span></span>

```javascript
    // Shows hello user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
        });

    },
```
<span data-ttu-id="ac3a6-205">Podle jeho rozdělení na dvě hlavní části Podívejme se na této funkce.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-205">Let's examine that function by breaking it down in its two main parts.</span></span>
<span data-ttu-id="ac3a6-206">Tato ukázka je navrženou toowork s žádným klientem jako názvem na rozdíl od toobeing svázané tooa některá.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-206">This sample is designed toowork with any tenant, as opposed toobeing tied tooa particular one.</span></span> <span data-ttu-id="ac3a6-207">Používá hello "/ běžné" koncový bod, který umožňuje hello uživatele tooenter žádnému účtu v době ověřování a přesměruje klienta toohello hello požadavku, kam patří.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-207">It uses hello "/common" endpoint, which allows hello user tooenter any account at authentication time and directs hello request toohello tenant where it belongs.</span></span>

<span data-ttu-id="ac3a6-208">Tato první část hello metoda zkontroluje toosee ADAL mezipaměti hello, pokud token je již uložen.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-208">This first part of hello method inspects hello ADAL cache toosee if a token is already stored.</span></span> <span data-ttu-id="ac3a6-209">Pokud ano, metoda hello používá hello klientům a odkud hello tokenu pochází pro provést novou inicializaci ADAL.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-209">If so, hello method uses hello tenants where hello token came from for reinitializing ADAL.</span></span> <span data-ttu-id="ac3a6-210">To je nezbytné tooavoid další výzvy, protože použití hello z "/ běžné" vždy výsledkem žádostí hello uživatele tooenter nový účet.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-210">This is necessary tooavoid extra prompts, because hello use of "/common" always results in asking hello user tooenter a new account.</span></span>

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
<span data-ttu-id="ac3a6-211">Druhá část Hello hello metody provede hello žádosti o token správné.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-211">hello second part of hello method performs hello proper token request.</span></span> <span data-ttu-id="ac3a6-212">Hello `acquireTokenSilentAsync` metoda ADAL tooreturn token vyzve k zadání hello zadaný prostředek bez zobrazení všech UX</span><span class="sxs-lookup"><span data-stu-id="ac3a6-212">hello `acquireTokenSilentAsync` method asks ADAL tooreturn a token for hello specified resource without showing any UX.</span></span> <span data-ttu-id="ac3a6-213">Může dojít, pokud mezipaměti hello již vhodný přístupový token, uložené, nebo pokud token obnovení lze použít tooget nový přístupový token bez zobrazuje všechny řádku.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-213">That can happen if hello cache already has a suitable access token stored, or if a refresh token can be used tooget a new access token without showing any prompt.</span></span> <span data-ttu-id="ac3a6-214">Pokud se tento pokus selže, jsme vrátit zpět `acquireTokenAsync`– viditelně, který vyzve uživatele tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-214">If that attempt fails, we fall back on `acquireTokenAsync`--which will visibly prompt hello user tooauthenticate.</span></span>

```javascript
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
```
<span data-ttu-id="ac3a6-215">Teď, když máme hello token, jsme nakonec vyvolání hello rozhraní Graph API a provádění hello vyhledávací dotaz, který má být.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-215">Now that we have hello token, we can finally invoke hello Graph API and perform hello search query that we want.</span></span> <span data-ttu-id="ac3a6-216">Vložte následující fragment kódu níže hello hello `authenticate` definice:</span><span class="sxs-lookup"><span data-stu-id="ac3a6-216">Insert hello following snippet below hello `authenticate` definition:</span></span>

```javascript
// Makes an API call tooreceive hello user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
<span data-ttu-id="ac3a6-217">soubory od bodu Hello zadat jednoduchý UX pro zadání uživatele alias v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-217">hello starting-point files supplied a simple UX for entering a user's alias in a text box.</span></span> <span data-ttu-id="ac3a6-218">Tato metoda používá tuto hodnotu tooconstruct dotazu, kombinovat s hello přístupový token, odešle tooMicrosoft graf a analyzovat hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-218">This method uses that value tooconstruct a query, combine it with hello access token, send it tooMicrosoft Graph, and parse hello results.</span></span> <span data-ttu-id="ac3a6-219">Hello `renderData` metoda, již existuje v souboru od bodu hello postará vizualizace výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-219">hello `renderData` method, already present in hello starting-point file, takes care of visualizing hello results.</span></span>

## <a name="step-5-run-hello-app"></a><span data-ttu-id="ac3a6-220">Krok 5: Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="ac3a6-220">Step 5: Run hello app</span></span>
<span data-ttu-id="ac3a6-221">Nakonec připravené toorun je vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-221">Your app is finally ready toorun.</span></span> <span data-ttu-id="ac3a6-222">Jeho provoz je jednoduchý: při spuštění aplikace hello zadejte hello alias uživatele hello chcete toolook nahoru a pak klikněte na tlačítko hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-222">Operating it is simple: when hello app starts, enter hello alias of hello user you want toolook up, and then click hello button.</span></span> <span data-ttu-id="ac3a6-223">Se zobrazí výzva k ověření.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-223">You're prompted for authentication.</span></span> <span data-ttu-id="ac3a6-224">Po úspěšném ověření a hledání úspěšné zobrazí se hello atributy hello hledat uživatele.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-224">Upon successful authentication and successful search, hello attributes of hello searched user are displayed.</span></span>

<span data-ttu-id="ac3a6-225">Při dalším spuštění provede vyhledávání hello bez zobrazení všech řádku, thanks toohello přítomnost hello dříve získat token v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-225">Subsequent runs will perform hello search without showing any prompt, thanks toohello presence of hello previously acquired token in cache.</span></span>

<span data-ttu-id="ac3a6-226">Hello konkrétní kroky pro spuštění aplikace hello se liší podle platformy.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-226">hello concrete steps for running hello app vary by platform.</span></span>

### <a name="windows-10"></a><span data-ttu-id="ac3a6-227">Windows 10</span><span class="sxs-lookup"><span data-stu-id="ac3a6-227">Windows 10</span></span>
   <span data-ttu-id="ac3a6-228">Tabletu:`cordova run windows --archs=x64 -- --appx=uap`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-228">Tablet/PC: `cordova run windows --archs=x64 -- --appx=uap`</span></span>

   <span data-ttu-id="ac3a6-229">Mobilní zařízení (vyžaduje tooa zařízení připojené Windows 10 Mobile PC):`cordova run windows --archs=arm -- --appx=uap --phone`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-229">Mobile (requires a Windows 10 Mobile device connected tooa PC): `cordova run windows --archs=arm -- --appx=uap --phone`</span></span>

   > [!NOTE]
   > <span data-ttu-id="ac3a6-230">Při prvním spuštění hello můžete být požádáni toosign v pro vývojáře licenci.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-230">During hello first run, you might be asked toosign in for a developer license.</span></span> <span data-ttu-id="ac3a6-231">Další informace najdete v tématu [vývojáře licence](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-231">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-81-tabletpc"></a><span data-ttu-id="ac3a6-232">Windows 8.1 Tabletu</span><span class="sxs-lookup"><span data-stu-id="ac3a6-232">Windows 8.1 Tablet/PC</span></span>
   `cordova run windows`

   > [!NOTE]
   > <span data-ttu-id="ac3a6-233">Při prvním spuštění hello můžete být požádáni toosign v pro vývojáře licenci.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-233">During hello first run, you might be asked toosign in for a developer license.</span></span> <span data-ttu-id="ac3a6-234">Další informace najdete v tématu [vývojáře licence](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-234">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-phone-81"></a><span data-ttu-id="ac3a6-235">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="ac3a6-235">Windows Phone 8.1</span></span>
   <span data-ttu-id="ac3a6-236">toorun na připojeném zařízení:`cordova run windows --device -- --phone`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-236">toorun on a connected device: `cordova run windows --device -- --phone`</span></span>

   <span data-ttu-id="ac3a6-237">toorun na výchozí emulátor hello:`cordova emulate windows -- --phone`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-237">toorun on hello default emulator: `cordova emulate windows -- --phone`</span></span>

   <span data-ttu-id="ac3a6-238">Použití `cordova run windows --list -- --phone` toosee všechny dostupné cíle a `cordova run windows --target=<target_name> -- --phone` aplikace hello toorun na určité zařízení nebo emulátoru (například `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-238">Use `cordova run windows --list -- --phone` toosee all available targets and `cordova run windows --target=<target_name> -- --phone` toorun hello application on a specific device or emulator (for example, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span></span>

### <a name="android"></a><span data-ttu-id="ac3a6-239">Android</span><span class="sxs-lookup"><span data-stu-id="ac3a6-239">Android</span></span>
   <span data-ttu-id="ac3a6-240">toorun na připojeném zařízení:`cordova run android --device`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-240">toorun on a connected device: `cordova run android --device`</span></span>

   <span data-ttu-id="ac3a6-241">toorun na výchozí emulátor hello:`cordova emulate android`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-241">toorun on hello default emulator: `cordova emulate android`</span></span>

   <span data-ttu-id="ac3a6-242">Ujistěte se, že jste vytvořili instanci emulátoru pomocí Správce AVD, jak je popsáno výše v části "Požadavky" hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-242">Make sure you've created an emulator instance by using AVD Manager, as described earlier in hello "Prerequisites" section.</span></span>

   <span data-ttu-id="ac3a6-243">Použití `cordova run android --list` toosee všechny dostupné cíle a `cordova run android --target=<target_name>` aplikace hello toorun na určité zařízení nebo emulátoru (například `cordova run android --target="Nexus4_emulator"`).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-243">Use `cordova run android --list` toosee all available targets and `cordova run android --target=<target_name>` toorun hello application on a specific device or emulator (for example, `cordova run android --target="Nexus4_emulator"`).</span></span>

### <a name="ios"></a><span data-ttu-id="ac3a6-244">iOS</span><span class="sxs-lookup"><span data-stu-id="ac3a6-244">iOS</span></span>
   <span data-ttu-id="ac3a6-245">toorun na připojeném zařízení:`cordova run ios --device`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-245">toorun on a connected device: `cordova run ios --device`</span></span>

   <span data-ttu-id="ac3a6-246">toorun na výchozí emulátor hello:`cordova emulate ios`</span><span class="sxs-lookup"><span data-stu-id="ac3a6-246">toorun on hello default emulator: `cordova emulate ios`</span></span>

   > [!NOTE]
   > <span data-ttu-id="ac3a6-247">Zajistěte, aby byla hello `ios-sim` toorun balíček nainstalován v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-247">Make sure you have hello `ios-sim` package installed toorun on hello emulator.</span></span> <span data-ttu-id="ac3a6-248">Další informace najdete v tématu požadavky"hello" oddílu.</span><span class="sxs-lookup"><span data-stu-id="ac3a6-248">For more information, see hello "Prerequisites" section.</span></span>

    Use `cordova run ios --list` toosee all available targets and `cordova run ios --target=<target_name>` toorun hello application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` toosee additional build and run options.

## <a name="next-steps"></a><span data-ttu-id="ac3a6-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac3a6-249">Next steps</span></span>
<span data-ttu-id="ac3a6-250">Pro srovnání je k dispozici v hello dokončit ukázka (bez vašich hodnot nastavení) [Githubu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-250">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span></span>

<span data-ttu-id="ac3a6-251">Můžete teď scénáře přesunout na toomore rozšířené (a další zajímavé).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-251">You can now move on toomore advanced (and more interesting) scenarios.</span></span> <span data-ttu-id="ac3a6-252">Můžete chtít tootry: [zabezpečit webové rozhraní Node.js API s Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ac3a6-252">You might want tootry: [Secure a Node.js Web API with Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
