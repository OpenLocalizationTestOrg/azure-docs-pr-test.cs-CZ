---
title: "aaaAzure AD Android Začínáme | Microsoft Docs"
description: "Tom, jak toobuild aplikace platformy Android, který se integruje s Azure AD pro přihlášení a volání služby Azure AD chráněný rozhraní API pomocí OAuth."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1aedc8ff60874b405a182a4ccbfb2c8b4d9d3704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a><span data-ttu-id="89015-103">Integrace Azure AD do aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="89015-103">Integrate Azure AD into an Android app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="89015-104">Vyzkoušení verze preview hello naší nové [portál pro vývojáře](https://identity.microsoft.com/Docs/Android), který vám pomůže zprovoznění s Azure AD za několik minut.</span><span class="sxs-lookup"><span data-stu-id="89015-104">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/Android), which will help you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="89015-105">portál pro vývojáře Hello vás provede procesem hello registrace aplikace a integraci služby Azure AD do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="89015-105">hello developer portal will walk you through hello process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="89015-106">Jakmile budete hotovi, budete mít jednoduchou aplikaci, která může ověřit uživatele v klientovi a back-end, které mohou přijímat tokeny a provést ověření.</span><span class="sxs-lookup"><span data-stu-id="89015-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a back end that can accept tokens and perform validation.</span></span>
>
>

<span data-ttu-id="89015-107">Pokud vyvíjíte aplikace pracovní plochy, Azure Active Directory (Azure AD) je jednoduchá a přímočará pro tooauthenticate můžete uživatelům pomocí jejich místních účtů služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="89015-107">If you're developing a desktop application, Azure Active Directory (Azure AD) makes it simple and straightforward for you tooauthenticate your users by using their on-premises Active Directory accounts.</span></span> <span data-ttu-id="89015-108">Umožňuje také toosecurely vaše aplikace využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="89015-108">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="89015-109">Pro Android klienti, kteří potřebují tooaccess chráněné prostředky Azure AD poskytuje hello Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="89015-109">For Android clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="89015-110">jediným účelem Hello adal je toomake ho snadno pro vaše aplikace tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="89015-110">hello sole purpose of ADAL is toomake it easy for your app tooget access tokens.</span></span> <span data-ttu-id="89015-111">jak snadné je, nemůžeme budete sestavit aplikaci Android seznam úkolů, která toodemonstrate:</span><span class="sxs-lookup"><span data-stu-id="89015-111">toodemonstrate how easy it is, we’ll build an Android To-Do List application that:</span></span>

* <span data-ttu-id="89015-112">Získá přístup k tokeny pro volání rozhraní API seznamu úkolů pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="89015-112">Gets access tokens for calling a To-Do List API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="89015-113">Získá seznam úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="89015-113">Gets a user's to-do list.</span></span>
* <span data-ttu-id="89015-114">Provede odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="89015-114">Signs out users.</span></span>

<span data-ttu-id="89015-115">tooget spustit, musíte klienta služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="89015-115">tooget started, you need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="89015-116">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="89015-116">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-download-and-run-hello-nodejs-rest-api-todo-sample-server"></a><span data-ttu-id="89015-117">Krok 1: Stáhněte a spusťte server ukázka TODO rozhraní API REST Node.js hello</span><span class="sxs-lookup"><span data-stu-id="89015-117">Step 1: Download and run hello Node.js REST API TODO sample server</span></span>
<span data-ttu-id="89015-118">Ukázka TODO rozhraní API REST Node.js Hello je zapsán konkrétně toowork proti naše existující vzorek pro vytváření jednoho klienta úkolů REST API pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="89015-118">hello Node.js REST API TODO sample is written specifically toowork against our existing sample for building a single-tenant To-Do REST API for Azure AD.</span></span> <span data-ttu-id="89015-119">Toto je předpokladem pro hello rychlý Start.</span><span class="sxs-lookup"><span data-stu-id="89015-119">This is a prerequisite for hello Quick Start.</span></span>

<span data-ttu-id="89015-120">Informace o tom, jak tooset to, najdete v části ukázek existující v [Microsoft Azure Active Directory ukázka REST API Service pro Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="89015-120">For information on how tooset this up, see our existing samples in [Microsoft Azure Active Directory Sample REST API Service for Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span></span>


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a><span data-ttu-id="89015-121">Krok 2: Registrace webového rozhraní API s klientovi Azure AD</span><span class="sxs-lookup"><span data-stu-id="89015-121">Step 2: Register your web API with your Azure AD tenant</span></span>
<span data-ttu-id="89015-122">Služba Active Directory podporuje přidání dva typy aplikací:</span><span class="sxs-lookup"><span data-stu-id="89015-122">Active Directory supports adding two types of applications:</span></span>

- <span data-ttu-id="89015-123">Webové rozhraní API, které nabízejí služby toousers</span><span class="sxs-lookup"><span data-stu-id="89015-123">Web APIs that offer services toousers</span></span>
- <span data-ttu-id="89015-124">Aplikace (běžící na webu hello nebo na zařízení), které ty přístup k webovým rozhraním API</span><span class="sxs-lookup"><span data-stu-id="89015-124">Applications (running either on hello web or on a device) that access those web APIs</span></span>

<span data-ttu-id="89015-125">V tomto kroku se registrace webového rozhraní API hello, kterou používáte místně pro testování této ukázce.</span><span class="sxs-lookup"><span data-stu-id="89015-125">In this step, you're registering hello web API that you're running locally for testing this sample.</span></span> <span data-ttu-id="89015-126">Za normálních okolností je tomuto webovému rozhraní API REST službu, která je nabídka funkce, které chcete aplikaci tooaccess.</span><span class="sxs-lookup"><span data-stu-id="89015-126">Normally, this web API is a REST service that's offering functionality that you want an app tooaccess.</span></span> <span data-ttu-id="89015-127">Azure AD můžete chránit libovolný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="89015-127">Azure AD can help protect any endpoint.</span></span>

<span data-ttu-id="89015-128">Jsme se za předpokladu, že jste registrace hello TODO REST API odkazuje dříve.</span><span class="sxs-lookup"><span data-stu-id="89015-128">We're assuming that you're registering hello TODO REST API referenced earlier.</span></span> <span data-ttu-id="89015-129">Ale tento postup funguje pro všechny webové rozhraní API, který chcete Azure Active Directory toohelp chránit.</span><span class="sxs-lookup"><span data-stu-id="89015-129">But this works for any web API that you want Azure Active Directory toohelp protect.</span></span>

1. <span data-ttu-id="89015-130">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="89015-130">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="89015-131">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="89015-131">On hello top bar, click your account.</span></span> <span data-ttu-id="89015-132">V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89015-132">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="89015-133">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="89015-133">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="89015-134">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="89015-134">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="89015-135">Zadejte popisný název aplikace hello (například **TodoListService**), vyberte **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="89015-135">Enter a friendly name for hello application (for example, **TodoListService**), select **Web Application and/or Web API**, and click **Next**.</span></span>
6. <span data-ttu-id="89015-136">Hello přihlašování adresy URL zadejte hello základní adresu URL pro ukázku hello.</span><span class="sxs-lookup"><span data-stu-id="89015-136">For hello sign-on URL, enter hello base URL for hello sample.</span></span> <span data-ttu-id="89015-137">Ve výchozím nastavení je to `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="89015-137">By default, this is `https://localhost:8080`.</span></span>
7. <span data-ttu-id="89015-138">Klikněte na tlačítko **OK** toocomplete hello registrace.</span><span class="sxs-lookup"><span data-stu-id="89015-138">Click **OK** toocomplete hello registration.</span></span>
8. <span data-ttu-id="89015-139">Během hello portálu Azure, přejděte na stránku aplikace tooyour, najít hodnotu ID aplikace hello a zkopírujte jej.</span><span class="sxs-lookup"><span data-stu-id="89015-139">While still in hello Azure portal, go tooyour application page, find hello application ID value, and copy it.</span></span> <span data-ttu-id="89015-140">Budete potřebovat to později při konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89015-140">You'll need this later when configuring your application.</span></span>
9. <span data-ttu-id="89015-141">Z hello **nastavení** -> **vlastnosti** stránky, aktualizovat identifikátor ID URI aplikace hello – zadejte `https://<your_tenant_name>/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="89015-141">From hello **Settings** -> **Properties** page, update hello app ID URI - enter `https://<your_tenant_name>/TodoListService`.</span></span> <span data-ttu-id="89015-142">Nahraďte `<your_tenant_name>` s názvem hello klienta služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="89015-142">Replace `<your_tenant_name>` with hello name of your Azure AD tenant.</span></span>

## <a name="step-3-register-hello-sample-android-native-client-application"></a><span data-ttu-id="89015-143">Krok 3: Registrace aplikace Android Native Client hello ukázka</span><span class="sxs-lookup"><span data-stu-id="89015-143">Step 3: Register hello sample Android Native Client application</span></span>
<span data-ttu-id="89015-144">V této ukázce je nutné zaregistrovat webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="89015-144">You must register your web application in this sample.</span></span> <span data-ttu-id="89015-145">To umožňuje toocommunicate vaší aplikace s hello právě zaregistrován webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="89015-145">This allows your application toocommunicate with hello just-registered web API.</span></span> <span data-ttu-id="89015-146">Azure AD odmítne tooeven povolit tooask vaší aplikace pro přihlášení, pokud je zaregistrována.</span><span class="sxs-lookup"><span data-stu-id="89015-146">Azure AD will refuse tooeven allow your application tooask for sign-in unless it's registered.</span></span> <span data-ttu-id="89015-147">Který je součástí hello zabezpečení hello modelu.</span><span class="sxs-lookup"><span data-stu-id="89015-147">That's part of hello security of hello model.</span></span>

<span data-ttu-id="89015-148">Jsme jste za předpokladu, že jste registrace hello ukázkovou aplikaci odkazuje dříve.</span><span class="sxs-lookup"><span data-stu-id="89015-148">We're assuming that you're registering hello sample application referenced earlier.</span></span> <span data-ttu-id="89015-149">Ale tento postup funguje pro každou aplikaci, která jste vývoj.</span><span class="sxs-lookup"><span data-stu-id="89015-149">But this procedure works for any app that you're developing.</span></span>

> [!NOTE]
> <span data-ttu-id="89015-150">Může vás zajímat, proč jste uvedení aplikace i webové rozhraní API v jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="89015-150">You might wonder why you're putting both an application and a web API in one tenant.</span></span> <span data-ttu-id="89015-151">Jak vám může mít uhádnout, můžete vytvořit aplikaci, která přistupuje k externí rozhraní API, která je zaregistrovaná ve službě Azure AD z jiného klienta.</span><span class="sxs-lookup"><span data-stu-id="89015-151">As you might have guessed, you can build an app that accesses an external API that is registered in Azure AD from another tenant.</span></span> <span data-ttu-id="89015-152">Pokud tak učiníte, budou vaši zákazníci výzva tooconsent toohello použití hello rozhraní API v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="89015-152">If you do that, your customers will be prompted tooconsent toohello use of hello API in hello application.</span></span> <span data-ttu-id="89015-153">Knihovna ověřování Active Directory pro iOS postará svůj souhlas pro vás.</span><span class="sxs-lookup"><span data-stu-id="89015-153">Active Directory Authentication Library for iOS takes care of this consent for you.</span></span> <span data-ttu-id="89015-154">Jak jsme prozkoumat nabízí vyspělejší funkce, uvidíte, že se jedná o důležitou součástí hello práce potřebné tooaccess hello sada Microsoft APIs z Azure a Office, stejně jako ostatní poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="89015-154">As we explore more advanced features, you'll see that this is an important part of hello work needed tooaccess hello suite of Microsoft APIs from Azure and Office, as well as any other service provider.</span></span> <span data-ttu-id="89015-155">Teď, protože jste zaregistrovali webového rozhraní API a aplikace v rámci hello stejné klienta, neuvidíte zobrazování výzev k vyjádření souhlasu.</span><span class="sxs-lookup"><span data-stu-id="89015-155">For now, because you registered both your web API and your application under hello same tenant, you won't see any prompts for consent.</span></span> <span data-ttu-id="89015-156">Toto je obvykle případ hello, pokud vyvíjíte aplikaci jen pro vlastní toouse společnosti.</span><span class="sxs-lookup"><span data-stu-id="89015-156">This is usually hello case if you're developing an application just for your own company toouse.</span></span>

1. <span data-ttu-id="89015-157">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="89015-157">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="89015-158">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="89015-158">On hello top bar, click your account.</span></span> <span data-ttu-id="89015-159">V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89015-159">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="89015-160">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="89015-160">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="89015-161">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="89015-161">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="89015-162">Zadejte popisný název aplikace hello (například **TodoListClient Android**), vyberte **nativní klientská aplikace**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="89015-162">Enter a friendly name for hello application (for example, **TodoListClient-Android**), select **Native Client Application**, and click **Next**.</span></span>
6. <span data-ttu-id="89015-163">Identifikátor URI přesměrování pro hello, zadejte `http://TodoListClient`.</span><span class="sxs-lookup"><span data-stu-id="89015-163">For hello redirect URI, enter `http://TodoListClient`.</span></span> <span data-ttu-id="89015-164">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="89015-164">Click **Finish**.</span></span>
7. <span data-ttu-id="89015-165">Na stránce aplikace hello najít hello hodnota ID aplikace a zkopírujte ho.</span><span class="sxs-lookup"><span data-stu-id="89015-165">From hello application page, find hello application ID value and copy it.</span></span> <span data-ttu-id="89015-166">Budete potřebovat to později při konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89015-166">You'll need this later when configuring your application.</span></span>
8. <span data-ttu-id="89015-167">Z hello **nastavení** vyberte **požadovaných oprávnění** a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="89015-167">From hello **Settings** page, select **Required Permissions** and select **Add**.</span></span>  <span data-ttu-id="89015-168">Vyhledejte a vyberte TodoListService, přidejte hello **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="89015-168">Locate and select TodoListService, add hello **Access TodoListService** permission under **Delegated Permissions**, and click **Done**.</span></span>

<span data-ttu-id="89015-169">toobuild s Maven, můžete použít pom.xml na nejvyšší úrovni hello:</span><span class="sxs-lookup"><span data-stu-id="89015-169">toobuild with Maven, you can use pom.xml at hello top level:</span></span>

1. <span data-ttu-id="89015-170">Klonování tohoto úložiště do adresáře podle vašeho výběru:</span><span class="sxs-lookup"><span data-stu-id="89015-170">Clone this repo into a directory of your choice:</span></span>

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. <span data-ttu-id="89015-171">Postupujte podle kroků hello v hello [tooset požadavky prostředí Maven pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span><span class="sxs-lookup"><span data-stu-id="89015-171">Follow hello steps in hello [prerequisites tooset up your Maven environment for Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span></span>
3. <span data-ttu-id="89015-172">Nastavte hello emulátoru s SDK 19.</span><span class="sxs-lookup"><span data-stu-id="89015-172">Set up hello emulator with SDK 19.</span></span>
4. <span data-ttu-id="89015-173">Přejděte toohello kořenové složky, které jste naklonovali úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="89015-173">Go toohello root folder where you cloned hello repo.</span></span>
5. <span data-ttu-id="89015-174">Spusťte tento příkaz:`mvn clean install`</span><span class="sxs-lookup"><span data-stu-id="89015-174">Run this command: `mvn clean install`</span></span>
6. <span data-ttu-id="89015-175">Změna hello directory toohello rychlý Start ukázka:`cd samples\hello`</span><span class="sxs-lookup"><span data-stu-id="89015-175">Change hello directory toohello Quick Start sample: `cd samples\hello`</span></span>
7. <span data-ttu-id="89015-176">Spusťte tento příkaz:`mvn android:deploy android:run`</span><span class="sxs-lookup"><span data-stu-id="89015-176">Run this command: `mvn android:deploy android:run`</span></span>

   <span data-ttu-id="89015-177">Měli byste vidět, spouštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="89015-177">You should see hello app starting.</span></span>
8. <span data-ttu-id="89015-178">Zadejte tootry přihlašovací údaje testovacího uživatele.</span><span class="sxs-lookup"><span data-stu-id="89015-178">Enter test user credentials tootry.</span></span>

<span data-ttu-id="89015-179">Balíčky JAR bude odeslána vedle hello AAR balíčku.</span><span class="sxs-lookup"><span data-stu-id="89015-179">JAR packages will be submitted beside hello AAR package.</span></span>

## <a name="step-4-download-hello-android-adal-and-add-it-tooyour-eclipse-workspace"></a><span data-ttu-id="89015-180">Krok 4: Stažení hello knihovny ADAL pro Android a přidejte ji tooyour Eclipse prostoru</span><span class="sxs-lookup"><span data-stu-id="89015-180">Step 4: Download hello Android ADAL and add it tooyour Eclipse workspace</span></span>
<span data-ttu-id="89015-181">Provedli jsme ho snadno můžete toohave více možností toouse ADAL v projektu Android:</span><span class="sxs-lookup"><span data-stu-id="89015-181">We've made it easy for you toohave multiple options toouse ADAL in your Android project:</span></span>

* <span data-ttu-id="89015-182">Hello zdrojového kódu tooimport můžete použít tuto knihovnu do aplikace tooyour Eclipse a odkaz.</span><span class="sxs-lookup"><span data-stu-id="89015-182">You can use hello source code tooimport this library into Eclipse and link tooyour application.</span></span>
* <span data-ttu-id="89015-183">Pokud používáte Android Studio, můžete použít hello AAR balíček formátu a referenční dokumentace hello binární soubory.</span><span class="sxs-lookup"><span data-stu-id="89015-183">If you're using Android Studio, you can use hello AAR package format and reference hello binaries.</span></span>

### <a name="option-1-source-zip"></a><span data-ttu-id="89015-184">Možnost 1: Zdroj Zip</span><span class="sxs-lookup"><span data-stu-id="89015-184">Option 1: Source Zip</span></span>
<span data-ttu-id="89015-185">Klikněte na tlačítko toodownload kopii hello zdrojový kód, **stáhnout ZIP** na pravé straně hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="89015-185">toodownload a copy of hello source code, click **Download ZIP** on hello right side of hello page.</span></span> <span data-ttu-id="89015-186">Nebo můžete [stáhnout z webu GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span><span class="sxs-lookup"><span data-stu-id="89015-186">Or you can [download from GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span></span>

### <a name="option-2-source-via-git"></a><span data-ttu-id="89015-187">Možnost 2: Zdroj pomocí Gitu</span><span class="sxs-lookup"><span data-stu-id="89015-187">Option 2: Source via Git</span></span>
<span data-ttu-id="89015-188">zdrojový kód tooget hello hello SDK prostřednictvím Git, zadejte:</span><span class="sxs-lookup"><span data-stu-id="89015-188">tooget hello source code of hello SDK via Git, type:</span></span>

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a><span data-ttu-id="89015-189">Možnost 3: Binární soubory přes Gradle</span><span class="sxs-lookup"><span data-stu-id="89015-189">Option 3: Binaries via Gradle</span></span>
<span data-ttu-id="89015-190">Binární soubory hello můžete získat z hello Maven centrální úložiště.</span><span class="sxs-lookup"><span data-stu-id="89015-190">You can get hello binaries from hello Maven central repo.</span></span> <span data-ttu-id="89015-191">balíček AAR Hello můžou být takto součástí projekt v Android Studio:</span><span class="sxs-lookup"><span data-stu-id="89015-191">hello AAR package can be included as follows in your project in Android Studio:</span></span>

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a><span data-ttu-id="89015-192">Možnost 4: AAR prostřednictvím Maven</span><span class="sxs-lookup"><span data-stu-id="89015-192">Option 4: AAR via Maven</span></span>
<span data-ttu-id="89015-193">Pokud používáte hello M2Eclipse modul plug-in, můžete určit hello závislostí v souboru pom.xml:</span><span class="sxs-lookup"><span data-stu-id="89015-193">If you're using hello M2Eclipse plug-in, you can specify hello dependency in your pom.xml file:</span></span>

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-hello-libs-folder"></a><span data-ttu-id="89015-194">Možnost 5: JAR balíčku uvnitř složky libs hello</span><span class="sxs-lookup"><span data-stu-id="89015-194">Option 5: JAR package inside hello libs folder</span></span>
<span data-ttu-id="89015-195">Můžete získat soubor JAR hello z hello Maven úložišti a umístěte jej do hello **knihovny** složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="89015-195">You can get hello JAR file from hello Maven repo and drop it into hello **libs** folder in your project.</span></span> <span data-ttu-id="89015-196">Je nutné projektu tooyour požadované prostředky toocopy hello je také možné, protože balíčky JAR hello neobsahují.</span><span class="sxs-lookup"><span data-stu-id="89015-196">You need toocopy hello required resources tooyour project as well, because hello JAR packages don't include them.</span></span>

## <a name="step-5-add-references-tooandroid-adal-tooyour-project"></a><span data-ttu-id="89015-197">Krok 5: Přidejte odkazy na tooAndroid ADAL tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="89015-197">Step 5: Add references tooAndroid ADAL tooyour project</span></span>
1. <span data-ttu-id="89015-198">Přidat odkaz na projekt tooyour a zadejte jej jako knihovna pro Android.</span><span class="sxs-lookup"><span data-stu-id="89015-198">Add a reference tooyour project and specify it as an Android library.</span></span> <span data-ttu-id="89015-199">Pokud si nejste jisti, jak toodo, získáte další informace o hello [lokality Android Studio](http://developer.android.com/tools/projects/projects-eclipse.html).</span><span class="sxs-lookup"><span data-stu-id="89015-199">If you're uncertain how toodo this, you can get more information on hello [Android Studio site](http://developer.android.com/tools/projects/projects-eclipse.html).</span></span>
2. <span data-ttu-id="89015-200">Přidáte závislost projektu hello ladění do nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="89015-200">Add hello project dependency for debugging into your project settings.</span></span>
3. <span data-ttu-id="89015-201">Aktualizace tooinclude souboru AndroidManifest.xml vašeho projektu:</span><span class="sxs-lookup"><span data-stu-id="89015-201">Update your project's AndroidManifest.xml file tooinclude:</span></span>

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. <span data-ttu-id="89015-202">Vytvoření instance kontextu AuthenticationContext v hlavní aktivitě.</span><span class="sxs-lookup"><span data-stu-id="89015-202">Create an instance of AuthenticationContext at your main activity.</span></span> <span data-ttu-id="89015-203">Hello podrobnosti tohoto volání je nad rámec hello rozsahu tohoto tématu, ale můžete začít funkčním prohlížením hello [ukázka Android Native Client](https://github.com/AzureADSamples/NativeClient-Android).</span><span class="sxs-lookup"><span data-stu-id="89015-203">hello details of this call are beyond hello scope of this topic, but you can get a good start by looking at hello [Android Native Client sample](https://github.com/AzureADSamples/NativeClient-Android).</span></span> <span data-ttu-id="89015-204">V následujícím příkladu hello, SharedPreferences je hello výchozí mezipaměti a autority hello tvar `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span><span class="sxs-lookup"><span data-stu-id="89015-204">In hello following example, SharedPreferences is hello default cache, and Authority is in hello form of `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span></span>

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. <span data-ttu-id="89015-205">Zkopírujte tento kód bloku toohandle hello konec AuthenticationActivity po hello uživatel zadá přihlašovací údaje a přijímá autorizační kód:</span><span class="sxs-lookup"><span data-stu-id="89015-205">Copy this code block toohandle hello end of AuthenticationActivity after hello user enters credentials and receives an authorization code:</span></span>

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. <span data-ttu-id="89015-206">tooask pro token, definovat zpětné volání:</span><span class="sxs-lookup"><span data-stu-id="89015-206">tooask for a token, define a callback:</span></span>

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. <span data-ttu-id="89015-207">Nakonec požádejte o token pomocí že zpětné volání:</span><span class="sxs-lookup"><span data-stu-id="89015-207">Finally, ask for a token by using that callback:</span></span>

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

<span data-ttu-id="89015-208">Následuje vysvětlení parametrů hello:</span><span class="sxs-lookup"><span data-stu-id="89015-208">Here's an explanation of hello parameters:</span></span>

* <span data-ttu-id="89015-209">*prostředek* je povinná a je hello prostředků, které se snažíte tooaccess.</span><span class="sxs-lookup"><span data-stu-id="89015-209">*resource* is required and is hello resource you're trying tooaccess.</span></span>
* <span data-ttu-id="89015-210">*ClientID* je povinná a pochází z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="89015-210">*clientid* is required and comes from Azure AD.</span></span>
* <span data-ttu-id="89015-211">*RedirectUri* není požadovaná toobe zadaná pro volání acquireToken hello.</span><span class="sxs-lookup"><span data-stu-id="89015-211">*RedirectUri* is not required toobe provided for hello acquireToken call.</span></span> <span data-ttu-id="89015-212">Můžete ho nastavit tak jako název balíčku.</span><span class="sxs-lookup"><span data-stu-id="89015-212">You can set it up as your package name.</span></span>
* <span data-ttu-id="89015-213">*PromptBehavior* pomáhá tooask pro přihlašovací údaje tooskip hello mezipaměti a souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="89015-213">*PromptBehavior* helps tooask for credentials tooskip hello cache and cookie.</span></span>
* <span data-ttu-id="89015-214">*zpětné volání* je volána po hello autorizační kód se vyměňují pro token.</span><span class="sxs-lookup"><span data-stu-id="89015-214">*callback* is called after hello authorization code is exchanged for a token.</span></span> <span data-ttu-id="89015-215">Obsahuje objekt AuthenticationResult, který obsahuje přístupový token, datum vypršela platnost a ID informace o tokenu.</span><span class="sxs-lookup"><span data-stu-id="89015-215">It has an object of AuthenticationResult, which has access token, date expired, and ID token information.</span></span>
* <span data-ttu-id="89015-216">*acquireTokenSilent* je volitelný.</span><span class="sxs-lookup"><span data-stu-id="89015-216">*acquireTokenSilent* is optional.</span></span> <span data-ttu-id="89015-217">Můžete ji volat toohandle ukládání do mezipaměti a token obnovení.</span><span class="sxs-lookup"><span data-stu-id="89015-217">You can call it toohandle caching and token refresh.</span></span> <span data-ttu-id="89015-218">Poskytuje také verzi služby sync hello.</span><span class="sxs-lookup"><span data-stu-id="89015-218">It also provides hello sync version.</span></span> <span data-ttu-id="89015-219">Přijímá *userId* jako parametr.</span><span class="sxs-lookup"><span data-stu-id="89015-219">It accepts *userId* as a parameter.</span></span>

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="89015-220">Pomocí tohoto návodu, byste měli mít, co budete potřebovat toosuccessfully integraci s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="89015-220">By using this walkthrough, you should have what you need toosuccessfully integrate with Azure Active Directory.</span></span> <span data-ttu-id="89015-221">Další příklady tato práce, najdete v článku hello AzureADSamples nebo úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="89015-221">For more examples of this working, visit hello AzureADSamples/ repository on GitHub.</span></span>

## <a name="important-information"></a><span data-ttu-id="89015-222">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="89015-222">Important information</span></span>
### <a name="customization"></a><span data-ttu-id="89015-223">Přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="89015-223">Customization</span></span>
<span data-ttu-id="89015-224">Vaše prostředky aplikace můžete přepsat projektu prostředky knihovny.</span><span class="sxs-lookup"><span data-stu-id="89015-224">Your application resources can overwrite library project resources.</span></span> <span data-ttu-id="89015-225">To se stane, když sestavuje vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89015-225">This happens when your app is being built.</span></span> <span data-ttu-id="89015-226">Z tohoto důvodu můžete přizpůsobit ověřování aktivity rozložení hello požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="89015-226">For this reason, you can customize authentication activity layout hello way you want.</span></span> <span data-ttu-id="89015-227">Být jisti ID hello tookeep prvků hello používá této ADAL (webového zobrazení).</span><span class="sxs-lookup"><span data-stu-id="89015-227">Be sure tookeep hello ID of hello controls that ADAL uses (WebView).</span></span>

### <a name="broker"></a><span data-ttu-id="89015-228">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="89015-228">Broker</span></span>
<span data-ttu-id="89015-229">aplikace portál společnosti Intune Microsoft Hello poskytuje součást zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="89015-229">hello Microsoft Intune Company Portal app provides hello broker component.</span></span> <span data-ttu-id="89015-230">Hello účet je vytvořen v AccountManager.</span><span class="sxs-lookup"><span data-stu-id="89015-230">hello account is created in AccountManager.</span></span> <span data-ttu-id="89015-231">Typ účtu Hello je "com.microsoft.workaccount."</span><span class="sxs-lookup"><span data-stu-id="89015-231">hello account type is "com.microsoft.workaccount."</span></span> <span data-ttu-id="89015-232">AccountManager umožňuje jenom jeden účet jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="89015-232">AccountManager allows only a single SSO account.</span></span> <span data-ttu-id="89015-233">Po dokončení hello zařízení výzvu pro některé z aplikací hello vytvoří soubor cookie jednotného přihlašování pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="89015-233">It creates an SSO cookie for hello user after completing hello device challenge for one of hello apps.</span></span>

<span data-ttu-id="89015-234">ADAL používá účet broker hello, pokud jeden uživatelský účet je vytvořena při tato ověřovací data a vy zvolíte není tooskip ho.</span><span class="sxs-lookup"><span data-stu-id="89015-234">ADAL uses hello broker account if one user account is created at this authenticator and you choose not tooskip it.</span></span> <span data-ttu-id="89015-235">Můžete přeskočit hello zprostředkovatele uživatele pomocí:</span><span class="sxs-lookup"><span data-stu-id="89015-235">You can skip hello broker user with:</span></span>

   `AuthenticationSettings.Instance.setSkipBroker(true);`

<span data-ttu-id="89015-236">Potřebujete tooregister speciální RedirectUri pro použití zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="89015-236">You need tooregister a special RedirectUri for broker usage.</span></span> <span data-ttu-id="89015-237">RedirectUri je ve formátu hello `msauth://packagename/Base64UrlencodedSignature`.</span><span class="sxs-lookup"><span data-stu-id="89015-237">RedirectUri is in hello format of `msauth://packagename/Base64UrlencodedSignature`.</span></span> <span data-ttu-id="89015-238">Vaše RedirectUri pro vaši aplikaci můžete získat pomocí skriptu brokerRedirectPrint.ps1 hello nebo mContext.getBrokerRedirectUri volání hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="89015-238">You can get your RedirectUri for your app by using hello script brokerRedirectPrint.ps1 or hello API call mContext.getBrokerRedirectUri.</span></span> <span data-ttu-id="89015-239">podpis Hello je související tooyour podpisové certifikáty.</span><span class="sxs-lookup"><span data-stu-id="89015-239">hello signature is related tooyour signing certificates.</span></span>

<span data-ttu-id="89015-240">aktuální model zprostředkovatele Hello je pro jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="89015-240">hello current broker model is for one user.</span></span> <span data-ttu-id="89015-241">Kontextu AuthenticationContext poskytuje hello API metoda tooget hello zprostředkovatele uživatele.</span><span class="sxs-lookup"><span data-stu-id="89015-241">AuthenticationContext provides hello API method tooget hello broker user.</span></span>

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

<span data-ttu-id="89015-242">Manifest aplikace by měl mít následující oprávnění toouse AccountManager účty hello.</span><span class="sxs-lookup"><span data-stu-id="89015-242">Your app manifest should have hello following permissions toouse AccountManager accounts.</span></span> <span data-ttu-id="89015-243">Podrobnosti najdete v tématu hello [AccountManager informace na webu Android hello](http://developer.android.com/reference/android/accounts/AccountManager.html).</span><span class="sxs-lookup"><span data-stu-id="89015-243">For details, see hello [AccountManager information on hello Android site](http://developer.android.com/reference/android/accounts/AccountManager.html).</span></span>

* <span data-ttu-id="89015-244">GET_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="89015-244">GET_ACCOUNTS</span></span>
* <span data-ttu-id="89015-245">USE_CREDENTIALS</span><span class="sxs-lookup"><span data-stu-id="89015-245">USE_CREDENTIALS</span></span>
* <span data-ttu-id="89015-246">MANAGE_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="89015-246">MANAGE_ACCOUNTS</span></span>

### <a name="authority-url-and-ad-fs"></a><span data-ttu-id="89015-247">Adresa URL autority a AD FS</span><span class="sxs-lookup"><span data-stu-id="89015-247">Authority URL and AD FS</span></span>
<span data-ttu-id="89015-248">Active Directory Federation Services (AD FS) není rozpoznán jako produkční služby tokenů zabezpečení, takže potřebujete tooturn instance zjišťování a předá hodnotu false v konstruktoru kontextu AuthenticationContext hello.</span><span class="sxs-lookup"><span data-stu-id="89015-248">Active Directory Federation Services (AD FS) is not recognized as production STS, so you need tooturn of instance discovery and pass false at hello AuthenticationContext constructor.</span></span>

<span data-ttu-id="89015-249">Adresa URL autority Hello musí instance služby tokenů zabezpečení a [název klienta](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="89015-249">hello authority URL needs an STS instance and a [tenant name](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span></span>

### <a name="querying-cache-items"></a><span data-ttu-id="89015-250">Dotazování na položky mezipaměti</span><span class="sxs-lookup"><span data-stu-id="89015-250">Querying cache items</span></span>
<span data-ttu-id="89015-251">ADAL poskytuje výchozí mezipaměti SharedPreferences s některé jednoduché mezipaměti funkce dotazování.</span><span class="sxs-lookup"><span data-stu-id="89015-251">ADAL provides a default cache in SharedPreferences with some simple cache query functions.</span></span> <span data-ttu-id="89015-252">Aktuální mezipaměť hello můžete získat z kontextu AuthenticationContext s použitím:</span><span class="sxs-lookup"><span data-stu-id="89015-252">You can get hello current cache from AuthenticationContext by using:</span></span>

    ITokenCacheStore cache = mContext.getCache();

<span data-ttu-id="89015-253">Můžete zadat také implementace mezipaměti, pokud chcete, aby toocustomize ho.</span><span class="sxs-lookup"><span data-stu-id="89015-253">You can also provide your cache implementation, if you want toocustomize it.</span></span>

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a><span data-ttu-id="89015-254">Chování výzvy</span><span class="sxs-lookup"><span data-stu-id="89015-254">Prompt behavior</span></span>
<span data-ttu-id="89015-255">ADAL poskytuje chování výzvy možnost toospecify.</span><span class="sxs-lookup"><span data-stu-id="89015-255">ADAL provides an option toospecify prompt behavior.</span></span> <span data-ttu-id="89015-256">Pokud token obnovení hello je neplatný a jsou vyžadována pověření uživatele, se zobrazí PromptBehavior.Auto hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="89015-256">PromptBehavior.Auto will show hello UI if hello refresh token is invalid and user credentials are required.</span></span> <span data-ttu-id="89015-257">PromptBehavior.Always bude přeskočit využití mezipaměti hello a vždy zobrazovat hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="89015-257">PromptBehavior.Always will skip hello cache usage and always show hello UI.</span></span>

### <a name="silent-token-request-from-cache-and-refresh"></a><span data-ttu-id="89015-258">Tichou žádosti o token z mezipaměti a aktualizace</span><span class="sxs-lookup"><span data-stu-id="89015-258">Silent token request from cache and refresh</span></span>
<span data-ttu-id="89015-259">Tichou žádosti o token nepoužívá hello automaticky otevírané okno uživatelského rozhraní a nevyžaduje aktivitu.</span><span class="sxs-lookup"><span data-stu-id="89015-259">A silent token request does not use hello UI pop-up and does not require an activity.</span></span> <span data-ttu-id="89015-260">Vrátí token z mezipaměti hello, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="89015-260">It returns a token from hello cache if available.</span></span> <span data-ttu-id="89015-261">Pokud hello tokenu vypršela platnost, tato metoda se pokusí toorefresh ho.</span><span class="sxs-lookup"><span data-stu-id="89015-261">If hello token is expired, this method tries toorefresh it.</span></span> <span data-ttu-id="89015-262">Pokud token obnovení hello vyprší nebo se nezdařilo, vrátí authenticationexception –.</span><span class="sxs-lookup"><span data-stu-id="89015-262">If hello refresh token is expired or failed, it returns AuthenticationException.</span></span>

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="89015-263">Můžete provést také synchronizace volat pomocí této metody.</span><span class="sxs-lookup"><span data-stu-id="89015-263">You can also make a sync call by using this method.</span></span> <span data-ttu-id="89015-264">Můžete nastavit hodnotu null toocallback nebo použít acquireTokenSilentSync.</span><span class="sxs-lookup"><span data-stu-id="89015-264">You can set null toocallback or use acquireTokenSilentSync.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="89015-265">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="89015-265">Diagnostics</span></span>
<span data-ttu-id="89015-266">Toto jsou primární zdroje informací pro diagnostiku problémů hello:</span><span class="sxs-lookup"><span data-stu-id="89015-266">These are hello primary sources of information for diagnosing issues:</span></span>

* <span data-ttu-id="89015-267">Výjimky</span><span class="sxs-lookup"><span data-stu-id="89015-267">Exceptions</span></span>
* <span data-ttu-id="89015-268">Logs</span><span class="sxs-lookup"><span data-stu-id="89015-268">Logs</span></span>
* <span data-ttu-id="89015-269">Trasování sítě</span><span class="sxs-lookup"><span data-stu-id="89015-269">Network traces</span></span>

<span data-ttu-id="89015-270">Všimněte si, že ID korelace jsou centrální toohello diagnostiky v hello knihovně.</span><span class="sxs-lookup"><span data-stu-id="89015-270">Note that correlation IDs are central toohello diagnostics in hello library.</span></span> <span data-ttu-id="89015-271">Vaše ID korelace na základě požadavků můžete nastavit, pokud chcete, aby toocorrelate ADAL žádosti s dalšími operacemi v kódu.</span><span class="sxs-lookup"><span data-stu-id="89015-271">You can set your correlation IDs on a per-request basis if you want toocorrelate an ADAL request with other operations in your code.</span></span> <span data-ttu-id="89015-272">Pokud není nastavený ID korelace, ADAL vygeneruje náhodné.</span><span class="sxs-lookup"><span data-stu-id="89015-272">If you don't set a correlation ID, ADAL will generate a random one.</span></span> <span data-ttu-id="89015-273">Všechny zprávy protokolu a volání síť pak bude být označený hello ID korelace.</span><span class="sxs-lookup"><span data-stu-id="89015-273">All log messages and network calls will then be stamped with hello correlation ID.</span></span> <span data-ttu-id="89015-274">Hello generovaný sám sebou ID změny na každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="89015-274">hello self-generated ID changes on each request.</span></span>

#### <a name="exceptions"></a><span data-ttu-id="89015-275">Výjimky</span><span class="sxs-lookup"><span data-stu-id="89015-275">Exceptions</span></span>
<span data-ttu-id="89015-276">Výjimky jsou hello nejprve diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="89015-276">Exceptions are hello first diagnostic.</span></span> <span data-ttu-id="89015-277">Pokusíme tooprovide užitečné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="89015-277">We try tooprovide helpful error messages.</span></span> <span data-ttu-id="89015-278">Pokud zjistíte, ten, který není užitečné, oznamte problém a dejte nám vědět.</span><span class="sxs-lookup"><span data-stu-id="89015-278">If you find one that is not helpful, please file an issue and let us know.</span></span> <span data-ttu-id="89015-279">Zahrnují informace o zařízení, jako je například modelu a číslo SDK.</span><span class="sxs-lookup"><span data-stu-id="89015-279">Include device information such as model and SDK number.</span></span>

#### <a name="logs"></a><span data-ttu-id="89015-280">Logs</span><span class="sxs-lookup"><span data-stu-id="89015-280">Logs</span></span>
<span data-ttu-id="89015-281">Můžete nakonfigurovat hello knihovně toogenerate zprávy protokolu, které můžete použít toohelp diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="89015-281">You can configure hello library toogenerate log messages that you can use toohelp diagnose issues.</span></span> <span data-ttu-id="89015-282">Konfigurace protokolování tak, že hello následující volání tooconfigure zpětné volání, jako je generován budou používat ADAL toohand vypnout každé zprávě protokolu.</span><span class="sxs-lookup"><span data-stu-id="89015-282">You configure logging by making hello following call tooconfigure a callback that ADAL will use toohand off each log message as it's generated.</span></span>

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this toolog file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

<span data-ttu-id="89015-283">Může být zprávy zapisovány tooa vlastního souboru protokolu, jak je znázorněno v následujícím kódu hello.</span><span class="sxs-lookup"><span data-stu-id="89015-283">Messages can be written tooa custom log file, as shown in hello following code.</span></span> <span data-ttu-id="89015-284">Bohužel neexistuje žádný standardní způsob získání protokolů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="89015-284">Unfortunately, there is no standard way of getting logs from a device.</span></span> <span data-ttu-id="89015-285">Existují některé služby, které vám mohou pomoci s to.</span><span class="sxs-lookup"><span data-stu-id="89015-285">There are some services that can help you with this.</span></span> <span data-ttu-id="89015-286">Můžete také vytvořte vlastní, jako třeba odesílání hello souboru tooa server.</span><span class="sxs-lookup"><span data-stu-id="89015-286">You can also invent your own, such as sending hello file tooa server.</span></span>

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

<span data-ttu-id="89015-287">Toto jsou hello úrovní protokolování:</span><span class="sxs-lookup"><span data-stu-id="89015-287">These are hello logging levels:</span></span>
* <span data-ttu-id="89015-288">Chyba (výjimek)</span><span class="sxs-lookup"><span data-stu-id="89015-288">Error (exceptions)</span></span>
* <span data-ttu-id="89015-289">Warn (upozornění)</span><span class="sxs-lookup"><span data-stu-id="89015-289">Warn (warning)</span></span>
* <span data-ttu-id="89015-290">Informace o (informační účely)</span><span class="sxs-lookup"><span data-stu-id="89015-290">Info (information purposes)</span></span>
* <span data-ttu-id="89015-291">Verbose (podrobnosti)</span><span class="sxs-lookup"><span data-stu-id="89015-291">Verbose (more details)</span></span>

<span data-ttu-id="89015-292">Můžete nastavit úroveň protokolu hello takto:</span><span class="sxs-lookup"><span data-stu-id="89015-292">You set hello log level like this:</span></span>

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 <span data-ttu-id="89015-293">Všechny zprávy protokolu jsou zasílány toologcat, v přidání tooany vlastní protokol zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="89015-293">All log messages are sent toologcat, in addition tooany custom log callbacks.</span></span>
<span data-ttu-id="89015-294">Soubor protokolu tooa z logcat můžete získat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="89015-294">You can get a log tooa file from logcat as follows:</span></span>

    adb logcat > "C:\logmsg\logfile.txt"

 <span data-ttu-id="89015-295">Podrobnosti o adb příkazů najdete v tématu hello [logcat informace na webu Android hello](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span><span class="sxs-lookup"><span data-stu-id="89015-295">For details about adb commands, see hello [logcat information on hello Android site](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span></span>

#### <a name="network-traces"></a><span data-ttu-id="89015-296">Trasování sítě</span><span class="sxs-lookup"><span data-stu-id="89015-296">Network traces</span></span>
<span data-ttu-id="89015-297">Můžete použít různé nástroje toocapture hello HTTP provoz, který generuje ADAL.</span><span class="sxs-lookup"><span data-stu-id="89015-297">You can use various tools toocapture hello HTTP traffic that ADAL generates.</span></span>  <span data-ttu-id="89015-298">To je velmi užitečné, pokud jste obeznámeni s hello protokolu OAuth, nebo pokud potřebujete tooprovide diagnostické informace tooMicrosoft nebo jiné kanály podpory.</span><span class="sxs-lookup"><span data-stu-id="89015-298">This is most useful if you're familiar with hello OAuth protocol or if you need tooprovide diagnostic information tooMicrosoft or other support channels.</span></span>

<span data-ttu-id="89015-299">Fiddler je hello nejjednodušší nástroj, trasování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="89015-299">Fiddler is hello easiest HTTP tracing tool.</span></span> <span data-ttu-id="89015-300">Použití hello následující odkazy tooset ho nahoru toocorrectly záznam ADAL síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="89015-300">Use hello following links tooset it up toocorrectly record ADAL network traffic.</span></span> <span data-ttu-id="89015-301">Pro trasování nástroje, jako je Fiddler nebo Charlese toobe užitečné musíte ho nakonfigurovat provoz toorecord bez šifrování SSL.</span><span class="sxs-lookup"><span data-stu-id="89015-301">For a tracing tool like Fiddler or Charles toobe useful, you must configure it toorecord unencrypted SSL traffic.</span></span>  

> [!NOTE]
> <span data-ttu-id="89015-302">Trasování generované tímto způsobem může obsahovat vysoce důvěrné informace, například přístupové tokeny, uživatelská jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="89015-302">Traces generated in this way might contain highly privileged information such as access tokens, usernames, and passwords.</span></span> <span data-ttu-id="89015-303">Pokud používáte produkční účty, nesdílí toto trasování s třetími stranami.</span><span class="sxs-lookup"><span data-stu-id="89015-303">If you're using production accounts, do not share these traces with third parties.</span></span> <span data-ttu-id="89015-304">Pokud potřebujete toosupply toosomeone trasování v pořadí tooget podpory, reprodukujte problém hello pomocí dočasného účtu s uživatelská jména a hesla, aby vás sdílení.</span><span class="sxs-lookup"><span data-stu-id="89015-304">If you need toosupply a trace toosomeone in order tooget support, reproduce hello issue by using a temporary account with usernames and passwords that you don't mind sharing.</span></span>

* <span data-ttu-id="89015-305">Z webu webu Telerik hello: [nastavení si aplikaci Fiddler pro Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span><span class="sxs-lookup"><span data-stu-id="89015-305">From hello Telerik website: [Setting Up Fiddler For Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span></span>
* <span data-ttu-id="89015-306">Z Githubu: [nakonfigurovat aplikaci Fiddler pravidla pro ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span><span class="sxs-lookup"><span data-stu-id="89015-306">From GitHub: [Configure Fiddler Rules For ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span></span>

### <a name="dialog-mode"></a><span data-ttu-id="89015-307">Dialogové okno režimu</span><span class="sxs-lookup"><span data-stu-id="89015-307">Dialog mode</span></span>
<span data-ttu-id="89015-308">Metoda acquireToken Hello bez aktivity podporuje řádku dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="89015-308">hello acquireToken method without activity supports a dialog prompt.</span></span>

### <a name="encryption"></a><span data-ttu-id="89015-309">Šifrování</span><span class="sxs-lookup"><span data-stu-id="89015-309">Encryption</span></span>
<span data-ttu-id="89015-310">ADAL šifruje hello tokeny a úložiště v SharedPreferences ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="89015-310">ADAL encrypts hello tokens and store in SharedPreferences by default.</span></span> <span data-ttu-id="89015-311">Můžete si prohlédnout podrobnosti hello hello StorageHelper třídy toosee.</span><span class="sxs-lookup"><span data-stu-id="89015-311">You can look at hello StorageHelper class toosee hello details.</span></span> <span data-ttu-id="89015-312">Android zavedl Android úložiště klíčů pro 4.3 (rozhraní API 18) bezpečné uložení privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="89015-312">Android introduced Android Keystore for 4.3 (API 18) secure storage of private keys.</span></span> <span data-ttu-id="89015-313">ADAL použije tento 18 rozhraní API a vyšší.</span><span class="sxs-lookup"><span data-stu-id="89015-313">ADAL uses that for API 18 and higher.</span></span> <span data-ttu-id="89015-314">Pokud chcete toouse ADAL pro nižší verze sady SDK, je nutné tooprovide tajný klíč v AuthenticationSettings.INSTANCE.setSecretKey.</span><span class="sxs-lookup"><span data-stu-id="89015-314">If you want toouse ADAL for lower SDK versions, you need tooprovide a secret key at AuthenticationSettings.INSTANCE.setSecretKey.</span></span>

### <a name="oauth2-bearer-challenge"></a><span data-ttu-id="89015-315">Výzvy nosiče OAuth2</span><span class="sxs-lookup"><span data-stu-id="89015-315">OAuth2 bearer challenge</span></span>
<span data-ttu-id="89015-316">Hello AuthenticationParameters třída poskytuje funkce tooget authorization_uri z hello výzvy nosiče OAuth2.</span><span class="sxs-lookup"><span data-stu-id="89015-316">hello AuthenticationParameters class provides functionality tooget authorization_uri from hello OAuth2 bearer challenge.</span></span>

### <a name="session-cookies-in-webview"></a><span data-ttu-id="89015-317">Soubory cookie relace ve webovém zobrazení</span><span class="sxs-lookup"><span data-stu-id="89015-317">Session cookies in WebView</span></span>
<span data-ttu-id="89015-318">Android webové zobrazení nevymaže soubory cookie relace po zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="89015-318">Android WebView does not clear session cookies after hello app is closed.</span></span> <span data-ttu-id="89015-319">Která dokáže zpracovat pomocí ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="89015-319">You can handle that by using this sample code:</span></span>

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

<span data-ttu-id="89015-320">Podrobnosti o souborech cookie najdete v tématu hello [CookieSyncManager informace na webu Android hello](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span><span class="sxs-lookup"><span data-stu-id="89015-320">For details about cookies, see hello [CookieSyncManager information on hello Android site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span></span>

### <a name="resource-overrides"></a><span data-ttu-id="89015-321">Přepsání prostředků</span><span class="sxs-lookup"><span data-stu-id="89015-321">Resource overrides</span></span>
<span data-ttu-id="89015-322">Knihovna ADAL Hello zahrnuje anglické řetězce pro zprávy ProgressDialog.</span><span class="sxs-lookup"><span data-stu-id="89015-322">hello ADAL library includes English strings for ProgressDialog messages.</span></span> <span data-ttu-id="89015-323">Aplikace je měli přepsat, pokud chcete, aby lokalizovaných řetězců.</span><span class="sxs-lookup"><span data-stu-id="89015-323">Your application should overwrite them if you want localized strings.</span></span>

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a><span data-ttu-id="89015-324">Dialogové okno protokolu NTLM</span><span class="sxs-lookup"><span data-stu-id="89015-324">NTLM dialog box</span></span>
<span data-ttu-id="89015-325">ADAL verze 1.1.0 podporuje dialogu protokolu NTLM, který zpracovává se pomocí hello onReceivedHttpAuthRequest událost z WebViewClient.</span><span class="sxs-lookup"><span data-stu-id="89015-325">ADAL version 1.1.0 supports an NTLM dialog box that is processed through hello onReceivedHttpAuthRequest event from WebViewClient.</span></span> <span data-ttu-id="89015-326">Můžete přizpůsobit hello rozložení a řetězce pro dialogové okno hello.</span><span class="sxs-lookup"><span data-stu-id="89015-326">You can customize hello layout and strings for hello dialog box.</span></span>

### <a name="cross-app-sso"></a><span data-ttu-id="89015-327">Jednotné přihlašování napříč aplikacemi</span><span class="sxs-lookup"><span data-stu-id="89015-327">Cross-app SSO</span></span>
<span data-ttu-id="89015-328">Další informace [jak tooenable jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL](active-directory-sso-android.md).</span><span class="sxs-lookup"><span data-stu-id="89015-328">Learn [how tooenable cross-app SSO on Android by using ADAL](active-directory-sso-android.md).</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
