---
title: "Začínáme se službou Azure AD Cordova | Microsoft Docs"
description: "Jak sestavit aplikaci Cordova, který se integruje s Azure AD pro přihlášení a zavolá rozhraní API Azure AD chráněné pomocí OAuth."
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
ms.openlocfilehash: d9f53148787729d29a0a89cce1b8b2b83ba228f8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a><span data-ttu-id="793b9-103">Integrace Azure AD s platformě Apache Cordova app</span><span class="sxs-lookup"><span data-stu-id="793b9-103">Integrate Azure AD with an Apache Cordova app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="793b9-104">K vývoji aplikací HTML5/JavaScript, které můžou běžet na mobilních zařízeních jako plně kvalifikované nativních aplikací můžete použít Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="793b9-104">You can use Apache Cordova to develop HTML5/JavaScript applications that can run on mobile devices as full-fledged native applications.</span></span> <span data-ttu-id="793b9-105">S Azure Active Directory (Azure AD) můžete přidat možnosti ověřování podnikové úrovni pro vaše aplikace Cordova.</span><span class="sxs-lookup"><span data-stu-id="793b9-105">With Azure Active Directory (Azure AD), you can add enterprise-grade authentication capabilities to your Cordova applications.</span></span>

<span data-ttu-id="793b9-106">Modulu plug-in Cordova zabalí Azure AD nativních sad SDK pro iOS, Android, Windows Store a Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="793b9-106">A Cordova plug-in wraps Azure AD native SDKs on iOS, Android, Windows Store, and Windows Phone.</span></span> <span data-ttu-id="793b9-107">Pomocí, modul plug-in, můžete vylepšit vaší aplikace pro podporu přihlašování pomocí účtů služby Active Directory pro Windows Server vaši uživatelé získat přístup k Office 365 a rozhraní API služby Azure a i pomoc při ochraně volání vlastní vlastní webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="793b9-107">By using that plug-in, you can enhance your application to support sign-in with your users' Windows Server Active Directory accounts, gain access to Office 365 and Azure APIs, and even help protect calls to your own custom web API.</span></span>

<span data-ttu-id="793b9-108">V tomto kurzu používáme Apache Cordova, modul plug-in pro Active Directory Authentication Library (ADAL) ke zlepšování jednoduchou aplikaci přidáním následující funkce:</span><span class="sxs-lookup"><span data-stu-id="793b9-108">In this tutorial, we'll use the Apache Cordova plug-in for Active Directory Authentication Library (ADAL) to improve a simple app by adding the following features:</span></span>

* <span data-ttu-id="793b9-109">Pomocí několika řádků kódu ověření uživatele a získat token.</span><span class="sxs-lookup"><span data-stu-id="793b9-109">With just a few lines of code, authenticate a user and obtain a token.</span></span>
* <span data-ttu-id="793b9-110">Tento token používaná k volání rozhraní Graph API pro dotazování adresáře a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="793b9-110">Use that token to invoke the Graph API to query that directory and display the results.</span></span>  
* <span data-ttu-id="793b9-111">Chcete-li minimalizovat ověřování výzvy pro uživatele pomocí ADAL mezipamětí tokenů.</span><span class="sxs-lookup"><span data-stu-id="793b9-111">Use the ADAL token cache to minimize authentication prompts for the user.</span></span>

<span data-ttu-id="793b9-112">Chcete-li tato vylepšení, budete muset:</span><span class="sxs-lookup"><span data-stu-id="793b9-112">To make those improvements, you need to:</span></span>

1. <span data-ttu-id="793b9-113">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="793b9-113">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="793b9-114">Přidávání kódu do vaší aplikace požadovat tokeny.</span><span class="sxs-lookup"><span data-stu-id="793b9-114">Add code to your app to request tokens.</span></span>
3. <span data-ttu-id="793b9-115">Přidejte kód k použití tokenu k dotazování na rozhraní Graph API a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="793b9-115">Add code to use the token for querying the Graph API and display results.</span></span>
4. <span data-ttu-id="793b9-116">Vytvoření projektu nasazení Cordova s všechny platformy, kterou chcete zacílit, přidat Cordova ADAL modulu plug-in a testování řešení v emulátorů.</span><span class="sxs-lookup"><span data-stu-id="793b9-116">Create the Cordova deployment project with all the platforms you want to target, add the Cordova ADAL plug-in, and test the solution in emulators.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="793b9-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="793b9-117">Prerequisites</span></span>
<span data-ttu-id="793b9-118">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="793b9-118">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="793b9-119">Klient služby Azure AD, kde máte účet s právy pro vývoj aplikací.</span><span class="sxs-lookup"><span data-stu-id="793b9-119">An Azure AD tenant where you have an account with app development rights.</span></span>
* <span data-ttu-id="793b9-120">Vývojové prostředí, který je nakonfigurován pro použití Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="793b9-120">A development environment that's configured to use Apache Cordova.</span></span>  

<span data-ttu-id="793b9-121">Jak už máte-li nastavit, přímo přejít na krok 1.</span><span class="sxs-lookup"><span data-stu-id="793b9-121">If you have both already set up, proceed directly to step 1.</span></span>

<span data-ttu-id="793b9-122">Pokud nemáte klient služby Azure AD, použijte [pokyny o tom, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="793b9-122">If you don't have an Azure AD tenant, use the [instructions on how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="793b9-123">Pokud nemáte nastavení na počítači pro Apache Cordova, nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="793b9-123">If you don't have Apache Cordova set up on your machine, install the following:</span></span>

* [<span data-ttu-id="793b9-124">Git</span><span class="sxs-lookup"><span data-stu-id="793b9-124">Git</span></span>](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [<span data-ttu-id="793b9-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="793b9-125">Node.js</span></span>](https://nodejs.org/download/)
* <span data-ttu-id="793b9-126">[Rozhraní příkazového řádku Cordova](https://cordova.apache.org/) (se dá snadno nainstalovat prostřednictvím Správce balíčku NPM: `npm install -g cordova`)</span><span class="sxs-lookup"><span data-stu-id="793b9-126">[Cordova CLI](https://cordova.apache.org/) (can be easily installed via NPM package manager: `npm install -g cordova`)</span></span>

<span data-ttu-id="793b9-127">Předchozí instalace by měly fungovat na počítači PC i na Mac.</span><span class="sxs-lookup"><span data-stu-id="793b9-127">The preceding installations should work both on the PC and on the Mac.</span></span>

<span data-ttu-id="793b9-128">Každý Cílová platforma má jiné předpoklady:</span><span class="sxs-lookup"><span data-stu-id="793b9-128">Each target platform has different prerequisites:</span></span>

* <span data-ttu-id="793b9-129">Sestavení a spuštění aplikace pro Windows Tabletu nebo Windows Phone:</span><span class="sxs-lookup"><span data-stu-id="793b9-129">To build and run an app for Windows Tablet/PC or Windows Phone:</span></span>
  * <span data-ttu-id="793b9-130">Nainstalujte [Visual Studio 2013 pro Windows s aktualizací 2 nebo novější](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express nebo jinou verzi) nebo [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span><span class="sxs-lookup"><span data-stu-id="793b9-130">Install [Visual Studio 2013 for Windows with Update 2 or later](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express or another version) or [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span></span>

* <span data-ttu-id="793b9-131">Sestavení a spuštění aplikace pro iOS:</span><span class="sxs-lookup"><span data-stu-id="793b9-131">To build and run an app for iOS:</span></span>

  * <span data-ttu-id="793b9-132">Instalaci Xcode 6.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="793b9-132">Install Xcode 6.x or later.</span></span> <span data-ttu-id="793b9-133">Stažení z [vývojáře Apple lokality](http://developer.apple.com/downloads) nebo [Mac App Storu](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span><span class="sxs-lookup"><span data-stu-id="793b9-133">Download it from the [Apple Developer site](http://developer.apple.com/downloads) or the [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span></span>
  * <span data-ttu-id="793b9-134">Nainstalujte [ios-sim](https://www.npmjs.org/package/ios-sim).</span><span class="sxs-lookup"><span data-stu-id="793b9-134">Install [ios-sim](https://www.npmjs.org/package/ios-sim).</span></span> <span data-ttu-id="793b9-135">Můžete ji spustit aplikací pro iOS v simulátoru iOS z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="793b9-135">You can use it to start iOS apps in iOS Simulator from the command line.</span></span> <span data-ttu-id="793b9-136">(Můžete ho snadno nainstalovat prostřednictvím terminálu: `npm install -g ios-sim`.)</span><span class="sxs-lookup"><span data-stu-id="793b9-136">(You can easily install it via the terminal: `npm install -g ios-sim`.)</span></span>
* <span data-ttu-id="793b9-137">Sestavení a spuštění aplikace pro Android:</span><span class="sxs-lookup"><span data-stu-id="793b9-137">To build and run an app for Android:</span></span>

  * <span data-ttu-id="793b9-138">Nainstalujte [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="793b9-138">Install [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or later.</span></span> <span data-ttu-id="793b9-139">Zajistěte, aby `JAVA_HOME` (proměnnou prostředí) je správně nastavena podle JDK instalační cestu (například C:\Program Files\Java\jdk1.7.0_75).</span><span class="sxs-lookup"><span data-stu-id="793b9-139">Make sure `JAVA_HOME` (environment variable) is correctly set according to the JDK installation path (for example, C:\Program Files\Java\jdk1.7.0_75).</span></span>
  * <span data-ttu-id="793b9-140">Nainstalujte [sady SDK pro Android](http://developer.android.com/sdk/installing/index.html?pkg=tools) a přidejte `<android-sdk-location>\tools` umístění (například C:\tools\Android\android-sdk\tools) pro vaše `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="793b9-140">Install [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) and add the `<android-sdk-location>\tools` location (for example, C:\tools\Android\android-sdk\tools) to your `PATH` environment variable.</span></span>
  * <span data-ttu-id="793b9-141">Otevřete Android SDK Manageru (například prostřednictvím terminálu: `android`) a nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="793b9-141">Open Android SDK Manager (for example, via the terminal: `android`) and install:</span></span>
    * <span data-ttu-id="793b9-142">*Android – 5.0.1 (rozhraní API 21)* SDK platformy</span><span class="sxs-lookup"><span data-stu-id="793b9-142">*Android 5.0.1 (API 21)* platform SDK</span></span>
    * <span data-ttu-id="793b9-143">*Nástroje sestavení Android SDK* verze 19.1.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="793b9-143">*Android SDK Build Tools* version 19.1.0 or later</span></span>
    * <span data-ttu-id="793b9-144">*Podpora pro Android úložiště* (funkce)</span><span class="sxs-lookup"><span data-stu-id="793b9-144">*Android Support Repository* (Extras)</span></span>

  <span data-ttu-id="793b9-145">Sadu Android SDK neposkytuje žádné výchozí emulátor instance.</span><span class="sxs-lookup"><span data-stu-id="793b9-145">The Android SDK doesn't provide any default emulator instance.</span></span> <span data-ttu-id="793b9-146">Vytvořit spuštěním `android avd` z terminálu a potom vyberete **vytvořit**, pokud chcete spustit aplikaci pro Android na emulátor.</span><span class="sxs-lookup"><span data-stu-id="793b9-146">Create one by running `android avd` from the terminal and then selecting **Create**, if you want to run the Android app on an emulator.</span></span> <span data-ttu-id="793b9-147">Doporučujeme, abyste API úrovně 19 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="793b9-147">We recommend an API level of 19 or higher.</span></span> <span data-ttu-id="793b9-148">Další informace o možnosti Android emulátoru a vytvoření najdete v tématu [správce AVD](http://developer.android.com/tools/help/avd-manager.html) na webu Android.</span><span class="sxs-lookup"><span data-stu-id="793b9-148">For more information about the Android emulator and creation options, see [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) on the Android site.</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="793b9-149">Krok 1: Zaregistrujte aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="793b9-149">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="793b9-150">Tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="793b9-150">This step is optional.</span></span> <span data-ttu-id="793b9-151">Tento kurz obsahuje předem zřízená hodnoty, které můžete použít k viz ukázka v akci bez provádění zřizování v vlastního klienta.</span><span class="sxs-lookup"><span data-stu-id="793b9-151">This tutorial provides pre-provisioned values that you can use to see the sample in action without doing any provisioning in your own tenant.</span></span> <span data-ttu-id="793b9-152">Ale doporučujeme provést tento krok a seznámit se s proces, protože se bude vyžadovat, při vytváření vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="793b9-152">However, we recommend that you do perform this step and become familiar with the process, because it will be required when you create your own applications.</span></span>

<span data-ttu-id="793b9-153">Azure AD vydá tokeny pouze známé aplikací.</span><span class="sxs-lookup"><span data-stu-id="793b9-153">Azure AD issues tokens to only known applications.</span></span> <span data-ttu-id="793b9-154">Než z vaší aplikace můžete použít Azure AD, budete muset vytvořit položku pro něj ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="793b9-154">Before you can use Azure AD from your app, you need to create an entry for it in your tenant.</span></span> <span data-ttu-id="793b9-155">Zaregistrujte novou aplikaci v klientovi:</span><span class="sxs-lookup"><span data-stu-id="793b9-155">To register a new application in your tenant:</span></span>

1. <span data-ttu-id="793b9-156">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="793b9-156">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="793b9-157">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="793b9-157">On the top bar, click your account.</span></span> <span data-ttu-id="793b9-158">V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="793b9-158">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="793b9-159">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="793b9-159">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="793b9-160">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="793b9-160">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="793b9-161">Postupujte podle výzev a vytvořte **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="793b9-161">Follow the prompts and create a **Native Client Application**.</span></span> <span data-ttu-id="793b9-162">(I když jsou aplikace Cordova na základě HTML, vytváříme nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="793b9-162">(Although Cordova apps are HTML based, we're creating a native client application here.</span></span> <span data-ttu-id="793b9-163">**Nativní klientská aplikace** musí být vybraná volba nebo aplikace nebude fungovat.)</span><span class="sxs-lookup"><span data-stu-id="793b9-163">The **Native Client Application** option must be selected, or the application won't work.)</span></span>
  * <span data-ttu-id="793b9-164">**Název** popisuje vaší aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="793b9-164">**Name** describes your application to users.</span></span>
  * <span data-ttu-id="793b9-165">**Identifikátor URI pro přesměrování** je identifikátor URI, který se používá k vrácení tokeny do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="793b9-165">**Redirect URI** is the URI that's used to return tokens to your app.</span></span> <span data-ttu-id="793b9-166">Zadejte **http://MyDirectorySearcherApp**.</span><span class="sxs-lookup"><span data-stu-id="793b9-166">Enter **http://MyDirectorySearcherApp**.</span></span>

<span data-ttu-id="793b9-167">Po dokončení registrace Azure AD jedinečný Identifikátor aplikace přiřadí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="793b9-167">After you finish registration, Azure AD assigns a unique application ID to your app.</span></span> <span data-ttu-id="793b9-168">Budete potřebovat tuto hodnotu v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="793b9-168">You’ll need this value in the next sections.</span></span> <span data-ttu-id="793b9-169">Můžete ji najít na kartě aplikace nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="793b9-169">You can find it on the application tab of the newly created app.</span></span>

<span data-ttu-id="793b9-170">Ke spuštění `DirSearchClient Sample`, udělení oprávnění nově vytvořené aplikace zpracovat dotaz rozhraní Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="793b9-170">To run `DirSearchClient Sample`, grant the newly created app permission to query the Azure AD Graph API:</span></span>

1. <span data-ttu-id="793b9-171">Z **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="793b9-171">From the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>  
2. <span data-ttu-id="793b9-172">Pro aplikaci Azure Active Directory vyberte **Microsoft Graph** jako rozhraní API a přidejte **přístup k adresáři jako přihlášeného uživatele** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="793b9-172">For the Azure Active Directory application, select **Microsoft Graph** as the API and add the **Access the directory as the signed-in user** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="793b9-173">To umožňuje vaše aplikace a dotaz rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="793b9-173">This enables your application to query the Graph API for users.</span></span>

## <a name="step-2-clone-the-sample-app-repository"></a><span data-ttu-id="793b9-174">Krok 2: Klonovat úložiště ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="793b9-174">Step 2: Clone the sample app repository</span></span>
<span data-ttu-id="793b9-175">Z vašeho prostředí nebo na příkazovém řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="793b9-175">From your shell or command line, type the following command:</span></span>

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-the-cordova-app"></a><span data-ttu-id="793b9-176">Krok 3: Vytvoření aplikace Cordova</span><span class="sxs-lookup"><span data-stu-id="793b9-176">Step 3: Create the Cordova app</span></span>
<span data-ttu-id="793b9-177">K vytvoření aplikace Cordova několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="793b9-177">There are multiple ways to create Cordova applications.</span></span> <span data-ttu-id="793b9-178">V tomto kurzu použijeme Cordova rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="793b9-178">In this tutorial, we'll use the Cordova command-line interface (CLI).</span></span>

1. <span data-ttu-id="793b9-179">Z vašeho prostředí nebo na příkazovém řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="793b9-179">From your shell or command line, type the following command:</span></span>

        cordova create DirSearchClient

   <span data-ttu-id="793b9-180">Tento příkaz vytvoří strukturu složek a generování uživatelského rozhraní pro projekt Cordova.</span><span class="sxs-lookup"><span data-stu-id="793b9-180">That command creates the folder structure and scaffolding for the Cordova project.</span></span>

2. <span data-ttu-id="793b9-181">Přesunout do nové složky DirSearchClient:</span><span class="sxs-lookup"><span data-stu-id="793b9-181">Move to the new DirSearchClient folder:</span></span>

        cd .\DirSearchClient

3. <span data-ttu-id="793b9-182">Kopírovat obsah projektu starter v podsložce www pomocí Správce souborového nebo následující příkaz ve vašem prostředí:</span><span class="sxs-lookup"><span data-stu-id="793b9-182">Copy the content of the starter project in the www subfolder by using a file manager or the following command in your shell:</span></span>

  * <span data-ttu-id="793b9-183">Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span><span class="sxs-lookup"><span data-stu-id="793b9-183">Windows: `xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span></span>
  * <span data-ttu-id="793b9-184">Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span><span class="sxs-lookup"><span data-stu-id="793b9-184">Mac: `cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span></span>

4. <span data-ttu-id="793b9-185">Přidáte modul plug-in seznamu povolených IP adres.</span><span class="sxs-lookup"><span data-stu-id="793b9-185">Add the whitelist plug-in.</span></span> <span data-ttu-id="793b9-186">To je nezbytné pro volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="793b9-186">This is necessary for invoking the Graph API.</span></span>

        cordova plugin add cordova-plugin-whitelist

5. <span data-ttu-id="793b9-187">Přidejte všechny platformy, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="793b9-187">Add all the platforms that you want to support.</span></span> <span data-ttu-id="793b9-188">Pokud chcete, aby pracovní vzorek, budete muset provést alespoň jeden z následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="793b9-188">To have a working sample, you need to execute at least one of the following commands.</span></span> <span data-ttu-id="793b9-189">Všimněte si, že nebudete moci emulovat iOS v systému Windows nebo emulovat Windows v počítačích Mac.</span><span class="sxs-lookup"><span data-stu-id="793b9-189">Note that you won't be able to emulate iOS on Windows or emulate Windows on a Mac.</span></span>

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. <span data-ttu-id="793b9-190">Do projektu přidejte knihovny ADAL pro modul plug-in Cordova:</span><span class="sxs-lookup"><span data-stu-id="793b9-190">Add the ADAL for Cordova plug-in to your project:</span></span>

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-to-authenticate-users-and-obtain-tokens-from-azure-ad"></a><span data-ttu-id="793b9-191">Krok 4: Přidejte kód, který ověřuje uživatele a získat tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="793b9-191">Step 4: Add code to authenticate users and obtain tokens from Azure AD</span></span>
<span data-ttu-id="793b9-192">Aplikace, které vyvíjíte v tomto kurzu vám poskytne jednoduchý directory funkce vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="793b9-192">The application that you're developing in this tutorial will provide a simple directory search feature.</span></span> <span data-ttu-id="793b9-193">Uživatele můžete zadejte alias, všechny uživatele v adresáři a vizualizovat některé základní atributy.</span><span class="sxs-lookup"><span data-stu-id="793b9-193">The user can then type the alias of any user in the directory and visualize some basic attributes.</span></span> <span data-ttu-id="793b9-194">Projekt starter obsahuje definici základní uživatelské rozhraní aplikace (ve www/index.html) a generování uživatelského rozhraní, která sváže základní aplikaci událostí cyklů vazby uživatelské rozhraní a výsledky zobrazení logiku (v www/js/index.js).</span><span class="sxs-lookup"><span data-stu-id="793b9-194">The starter project contains the definition of the basic user interface of the app (in www/index.html) and the scaffolding that wires up basic app event cycles, user interface bindings, and results display logic (in www/js/index.js).</span></span> <span data-ttu-id="793b9-195">Úlohu pouze left můžete je přidat logiku, která implementuje identity úlohy.</span><span class="sxs-lookup"><span data-stu-id="793b9-195">The only task left for you is to add the logic that implements identity tasks.</span></span>

<span data-ttu-id="793b9-196">První věcí, kterou je třeba provést v kódu je zavést protokol hodnoty, které používá Azure AD pro identifikaci vaší aplikace a prostředky, že cíl je.</span><span class="sxs-lookup"><span data-stu-id="793b9-196">The first thing you need to do in your code is introduce the protocol values that Azure AD uses for identifying your app and the resources that you target.</span></span> <span data-ttu-id="793b9-197">Tyto hodnoty se použije k vytvoření žádosti o tokeny později.</span><span class="sxs-lookup"><span data-stu-id="793b9-197">Those values will be used to construct the token requests later on.</span></span> <span data-ttu-id="793b9-198">V horní části souboru index.js vložte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="793b9-198">Insert the following snippet at the top of the index.js file:</span></span>

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

<span data-ttu-id="793b9-199">`redirectUri` a `clientId` hodnoty by měla shodují s hodnotami, které popisují aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="793b9-199">The `redirectUri` and `clientId` values should match the values that describe your app in Azure AD.</span></span> <span data-ttu-id="793b9-200">Můžete najít ty z **konfigurace** kartě na portálu Azure, jak je popsáno v kroku 1 v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="793b9-200">You can find those from the **Configure** tab in the Azure portal, as described in step 1 earlier in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="793b9-201">Pokud jste se rozhodli pro novou aplikaci není registraci v vlastního klienta, můžete jednoduše vložit předkonfigurované hodnoty, protože je.</span><span class="sxs-lookup"><span data-stu-id="793b9-201">If you opted for not registering a new app in your own tenant, you can simply paste the preconfigured values as is.</span></span> <span data-ttu-id="793b9-202">Ukázka spuštěn, můžete se podívat, když měli vždycky vytvořit vlastní položku pro aplikace, které jsou určené pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="793b9-202">You can then see the sample running, though you should always create your own entry for your apps that are meant for production.</span></span>

<span data-ttu-id="793b9-203">Dál přidejte kód žádosti o token.</span><span class="sxs-lookup"><span data-stu-id="793b9-203">Next, add the token request code.</span></span> <span data-ttu-id="793b9-204">Vložte následující fragment kódu mezi `search` a `renderData` definice:</span><span class="sxs-lookup"><span data-stu-id="793b9-204">Insert the following snippet between the `search` and `renderData` definitions:</span></span>

```javascript
    // Shows the user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
<span data-ttu-id="793b9-205">Podle jeho rozdělení na dvě hlavní části Podívejme se na této funkce.</span><span class="sxs-lookup"><span data-stu-id="793b9-205">Let's examine that function by breaking it down in its two main parts.</span></span>
<span data-ttu-id="793b9-206">Tato ukázka je navržen pro práci s žádným klientem, a není svázán s některá.</span><span class="sxs-lookup"><span data-stu-id="793b9-206">This sample is designed to work with any tenant, as opposed to being tied to a particular one.</span></span> <span data-ttu-id="793b9-207">Použije "/ běžné" koncový bod, který umožňuje uživateli zadat libovolný účet během ověřování a přesměruje požadavek na klienta, kam patří:</span><span class="sxs-lookup"><span data-stu-id="793b9-207">It uses the "/common" endpoint, which allows the user to enter any account at authentication time and directs the request to the tenant where it belongs.</span></span>

<span data-ttu-id="793b9-208">Tato první část metoda zkontroluje ADAL mezipaměti zobrazíte, pokud je již uložen token.</span><span class="sxs-lookup"><span data-stu-id="793b9-208">This first part of the method inspects the ADAL cache to see if a token is already stored.</span></span> <span data-ttu-id="793b9-209">Pokud ano, používá metodu klienty odkud tokenu pochází pro provést novou inicializaci ADAL.</span><span class="sxs-lookup"><span data-stu-id="793b9-209">If so, the method uses the tenants where the token came from for reinitializing ADAL.</span></span> <span data-ttu-id="793b9-210">To je nezbytné další výzev, protože použití z "/ běžné" vždy výsledkem s dotazem, aby uživatel zadal nový účet.</span><span class="sxs-lookup"><span data-stu-id="793b9-210">This is necessary to avoid extra prompts, because the use of "/common" always results in asking the user to enter a new account.</span></span>

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
<span data-ttu-id="793b9-211">Druhá část metoda provádí správné žádosti o token.</span><span class="sxs-lookup"><span data-stu-id="793b9-211">The second part of the method performs the proper token request.</span></span> <span data-ttu-id="793b9-212">`acquireTokenSilentAsync` ADAL se vraťte zpět token pro zadaného prostředku bez žádné UX zobrazující požádá – metoda</span><span class="sxs-lookup"><span data-stu-id="793b9-212">The `acquireTokenSilentAsync` method asks ADAL to return a token for the specified resource without showing any UX.</span></span> <span data-ttu-id="793b9-213">Může dojít, pokud mezipaměť již obsahuje vhodný přístupový token, uložené, nebo pokud obnovovací token slouží k získání tokenu pro přístup k nové bez zobrazení všech řádku.</span><span class="sxs-lookup"><span data-stu-id="793b9-213">That can happen if the cache already has a suitable access token stored, or if a refresh token can be used to get a new access token without showing any prompt.</span></span> <span data-ttu-id="793b9-214">Pokud se tento pokus selže, jsme vrátit zpět `acquireTokenAsync`– viditelně, který vyzve uživatele k ověření.</span><span class="sxs-lookup"><span data-stu-id="793b9-214">If that attempt fails, we fall back on `acquireTokenAsync`--which will visibly prompt the user to authenticate.</span></span>

```javascript
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
<span data-ttu-id="793b9-215">Teď, když máme token, jsme nakonec volání rozhraní Graph API a provádění vyhledávací dotaz, který má být.</span><span class="sxs-lookup"><span data-stu-id="793b9-215">Now that we have the token, we can finally invoke the Graph API and perform the search query that we want.</span></span> <span data-ttu-id="793b9-216">Vložte následující fragment kódu níže `authenticate` definice:</span><span class="sxs-lookup"><span data-stu-id="793b9-216">Insert the following snippet below the `authenticate` definition:</span></span>

```javascript
// Makes an API call to receive the user list
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
<span data-ttu-id="793b9-217">Soubory od bodu zadat jednoduchý UX pro zadání uživatele alias v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="793b9-217">The starting-point files supplied a simple UX for entering a user's alias in a text box.</span></span> <span data-ttu-id="793b9-218">Tato metoda používá tuto hodnotu sestavte dotaz, ho spojovat se přístupový token, poslat Microsoft Graph a analyzovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="793b9-218">This method uses that value to construct a query, combine it with the access token, send it to Microsoft Graph, and parse the results.</span></span> <span data-ttu-id="793b9-219">`renderData` Metoda, již existuje v souboru od bodu, má na starosti vizualizace výsledků.</span><span class="sxs-lookup"><span data-stu-id="793b9-219">The `renderData` method, already present in the starting-point file, takes care of visualizing the results.</span></span>

## <a name="step-5-run-the-app"></a><span data-ttu-id="793b9-220">Krok 5: Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="793b9-220">Step 5: Run the app</span></span>
<span data-ttu-id="793b9-221">Aplikace je nakonec připraven ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="793b9-221">Your app is finally ready to run.</span></span> <span data-ttu-id="793b9-222">Jeho provoz je jednoduchý: při spuštění aplikace, zadejte alias uživatele, kterou chcete vyhledat a potom klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="793b9-222">Operating it is simple: when the app starts, enter the alias of the user you want to look up, and then click the button.</span></span> <span data-ttu-id="793b9-223">Se zobrazí výzva k ověření.</span><span class="sxs-lookup"><span data-stu-id="793b9-223">You're prompted for authentication.</span></span> <span data-ttu-id="793b9-224">Po úspěšném ověření a hledání úspěšné zobrazí se atributy vyhledávaná uživatele.</span><span class="sxs-lookup"><span data-stu-id="793b9-224">Upon successful authentication and successful search, the attributes of the searched user are displayed.</span></span>

<span data-ttu-id="793b9-225">Při dalším spuštění provede vyhledávání bez zobrazení všech řádku díky přítomnosti dříve získal token v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="793b9-225">Subsequent runs will perform the search without showing any prompt, thanks to the presence of the previously acquired token in cache.</span></span>

<span data-ttu-id="793b9-226">Konkrétní kroky pro spuštění aplikace se liší podle platformy.</span><span class="sxs-lookup"><span data-stu-id="793b9-226">The concrete steps for running the app vary by platform.</span></span>

### <a name="windows-10"></a><span data-ttu-id="793b9-227">Windows 10</span><span class="sxs-lookup"><span data-stu-id="793b9-227">Windows 10</span></span>
   <span data-ttu-id="793b9-228">Tabletu:`cordova run windows --archs=x64 -- --appx=uap`</span><span class="sxs-lookup"><span data-stu-id="793b9-228">Tablet/PC: `cordova run windows --archs=x64 -- --appx=uap`</span></span>

   <span data-ttu-id="793b9-229">Mobilní zařízení (vyžaduje připojení k počítači zařízení Windows 10 Mobile):`cordova run windows --archs=arm -- --appx=uap --phone`</span><span class="sxs-lookup"><span data-stu-id="793b9-229">Mobile (requires a Windows 10 Mobile device connected to a PC): `cordova run windows --archs=arm -- --appx=uap --phone`</span></span>

   > [!NOTE]
   > <span data-ttu-id="793b9-230">Při prvním spuštění může vyzváni k přihlášení pro vývojáře licenci.</span><span class="sxs-lookup"><span data-stu-id="793b9-230">During the first run, you might be asked to sign in for a developer license.</span></span> <span data-ttu-id="793b9-231">Další informace najdete v tématu [vývojáře licence](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="793b9-231">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-81-tabletpc"></a><span data-ttu-id="793b9-232">Windows 8.1 Tabletu</span><span class="sxs-lookup"><span data-stu-id="793b9-232">Windows 8.1 Tablet/PC</span></span>
   `cordova run windows`

   > [!NOTE]
   > <span data-ttu-id="793b9-233">Při prvním spuštění může vyzváni k přihlášení pro vývojáře licenci.</span><span class="sxs-lookup"><span data-stu-id="793b9-233">During the first run, you might be asked to sign in for a developer license.</span></span> <span data-ttu-id="793b9-234">Další informace najdete v tématu [vývojáře licence](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="793b9-234">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-phone-81"></a><span data-ttu-id="793b9-235">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="793b9-235">Windows Phone 8.1</span></span>
   <span data-ttu-id="793b9-236">Ke spuštění na připojeném zařízení:`cordova run windows --device -- --phone`</span><span class="sxs-lookup"><span data-stu-id="793b9-236">To run on a connected device: `cordova run windows --device -- --phone`</span></span>

   <span data-ttu-id="793b9-237">Ke spuštění na výchozí emulátor:`cordova emulate windows -- --phone`</span><span class="sxs-lookup"><span data-stu-id="793b9-237">To run on the default emulator: `cordova emulate windows -- --phone`</span></span>

   <span data-ttu-id="793b9-238">Použití `cordova run windows --list -- --phone` zobrazíte všechny dostupné cíle a `cordova run windows --target=<target_name> -- --phone` ke spuštění aplikace na určité zařízení nebo emulátoru (například `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span><span class="sxs-lookup"><span data-stu-id="793b9-238">Use `cordova run windows --list -- --phone` to see all available targets and `cordova run windows --target=<target_name> -- --phone` to run the application on a specific device or emulator (for example, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span></span>

### <a name="android"></a><span data-ttu-id="793b9-239">Android</span><span class="sxs-lookup"><span data-stu-id="793b9-239">Android</span></span>
   <span data-ttu-id="793b9-240">Ke spuštění na připojeném zařízení:`cordova run android --device`</span><span class="sxs-lookup"><span data-stu-id="793b9-240">To run on a connected device: `cordova run android --device`</span></span>

   <span data-ttu-id="793b9-241">Ke spuštění na výchozí emulátor:`cordova emulate android`</span><span class="sxs-lookup"><span data-stu-id="793b9-241">To run on the default emulator: `cordova emulate android`</span></span>

   <span data-ttu-id="793b9-242">Ujistěte se, že jste vytvořili instanci emulátoru pomocí Správce AVD, jak je popsáno výše v části "Požadavky".</span><span class="sxs-lookup"><span data-stu-id="793b9-242">Make sure you've created an emulator instance by using AVD Manager, as described earlier in the "Prerequisites" section.</span></span>

   <span data-ttu-id="793b9-243">Použití `cordova run android --list` zobrazíte všechny dostupné cíle a `cordova run android --target=<target_name>` ke spuštění aplikace na určité zařízení nebo emulátoru (například `cordova run android --target="Nexus4_emulator"`).</span><span class="sxs-lookup"><span data-stu-id="793b9-243">Use `cordova run android --list` to see all available targets and `cordova run android --target=<target_name>` to run the application on a specific device or emulator (for example, `cordova run android --target="Nexus4_emulator"`).</span></span>

### <a name="ios"></a><span data-ttu-id="793b9-244">iOS</span><span class="sxs-lookup"><span data-stu-id="793b9-244">iOS</span></span>
   <span data-ttu-id="793b9-245">Ke spuštění na připojeném zařízení:`cordova run ios --device`</span><span class="sxs-lookup"><span data-stu-id="793b9-245">To run on a connected device: `cordova run ios --device`</span></span>

   <span data-ttu-id="793b9-246">Ke spuštění na výchozí emulátor:`cordova emulate ios`</span><span class="sxs-lookup"><span data-stu-id="793b9-246">To run on the default emulator: `cordova emulate ios`</span></span>

   > [!NOTE]
   > <span data-ttu-id="793b9-247">Zajistěte, aby byla `ios-sim` nainstalována pro spouštění v emulátoru balíčku.</span><span class="sxs-lookup"><span data-stu-id="793b9-247">Make sure you have the `ios-sim` package installed to run on the emulator.</span></span> <span data-ttu-id="793b9-248">Další informace najdete v části "Požadavky".</span><span class="sxs-lookup"><span data-stu-id="793b9-248">For more information, see the "Prerequisites" section.</span></span>

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run the application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` to see additional build and run options.

## <a name="next-steps"></a><span data-ttu-id="793b9-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="793b9-249">Next steps</span></span>
<span data-ttu-id="793b9-250">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) k dispozici v [Githubu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span><span class="sxs-lookup"><span data-stu-id="793b9-250">For reference, the completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span></span>

<span data-ttu-id="793b9-251">Nyní se můžete přesunout pokročilejší (a zajímavějšího) scénářů.</span><span class="sxs-lookup"><span data-stu-id="793b9-251">You can now move on to more advanced (and more interesting) scenarios.</span></span> <span data-ttu-id="793b9-252">Můžete chtít zkuste: [zabezpečit webové rozhraní Node.js API s Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="793b9-252">You might want to try: [Secure a Node.js Web API with Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
