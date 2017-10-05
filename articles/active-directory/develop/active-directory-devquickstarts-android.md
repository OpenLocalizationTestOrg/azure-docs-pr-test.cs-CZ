---
title: "Začínáme se službou Azure AD Android | Microsoft Docs"
description: "Jak vytvářet aplikace platformy Android, který se integruje s Azure AD pro přihlášení a volání služby Azure AD chráněný rozhraní API pomocí OAuth."
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
ms.openlocfilehash: 746cad19093fd2a1ad23ddd9412394f8d9da331c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a><span data-ttu-id="f0d19-103">Integrace Azure AD do aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="f0d19-103">Integrate Azure AD into an Android app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="f0d19-104">Vyzkoušejte verzi preview našeho nového [portál pro vývojáře](https://identity.microsoft.com/Docs/Android), který vám pomůže zprovoznění s Azure AD za několik minut.</span><span class="sxs-lookup"><span data-stu-id="f0d19-104">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/Android), which will help you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="f0d19-105">Portál pro vývojáře vás provede procesem registrace aplikace a integraci služby Azure AD do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-105">The developer portal will walk you through the process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="f0d19-106">Jakmile budete hotovi, budete mít jednoduchou aplikaci, která může ověřit uživatele v klientovi a back-end, které mohou přijímat tokeny a provést ověření.</span><span class="sxs-lookup"><span data-stu-id="f0d19-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a back end that can accept tokens and perform validation.</span></span>
>
>

<span data-ttu-id="f0d19-107">Pokud vyvíjíte aplikace pracovní plochy, Azure Active Directory (Azure AD) je jednoduchá a přímočará pro vás k ověřování uživatelů pomocí jejich místních účtů služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f0d19-107">If you're developing a desktop application, Azure Active Directory (Azure AD) makes it simple and straightforward for you to authenticate your users by using their on-premises Active Directory accounts.</span></span> <span data-ttu-id="f0d19-108">Taky umožňuje vaší aplikaci bezpečně využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="f0d19-108">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="f0d19-109">Pro Android klientů, kteří potřebují přístup k chráněným prostředkům Azure AD poskytuje službě Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="f0d19-109">For Android clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="f0d19-110">Jediný účel ADAL je snadno pro aplikaci, kterou chcete získat přístupové tokeny.</span><span class="sxs-lookup"><span data-stu-id="f0d19-110">The sole purpose of ADAL is to make it easy for your app to get access tokens.</span></span> <span data-ttu-id="f0d19-111">K předvedení, jak je snadné, jsme budete sestavit Android seznam úkolů aplikaci, která:</span><span class="sxs-lookup"><span data-stu-id="f0d19-111">To demonstrate how easy it is, we’ll build an Android To-Do List application that:</span></span>

* <span data-ttu-id="f0d19-112">Získá přístup k tokeny pro volání rozhraní API seznamu úkolů pomocí [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0d19-112">Gets access tokens for calling a To-Do List API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="f0d19-113">Získá seznam úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-113">Gets a user's to-do list.</span></span>
* <span data-ttu-id="f0d19-114">Provede odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-114">Signs out users.</span></span>

<span data-ttu-id="f0d19-115">Abyste mohli začít, musíte klienta služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0d19-115">To get started, you need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="f0d19-116">Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="f0d19-116">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a><span data-ttu-id="f0d19-117">Krok 1: Stáhněte a spusťte ukázkový server Node.js REST API se seznamem úkolů</span><span class="sxs-lookup"><span data-stu-id="f0d19-117">Step 1: Download and run the Node.js REST API TODO sample server</span></span>
<span data-ttu-id="f0d19-118">Ukázka TODO rozhraní API REST Node.js se zapíše speciálně pro práci s naše existující vzorek pro vytváření jednoho klienta úkolů REST API pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0d19-118">The Node.js REST API TODO sample is written specifically to work against our existing sample for building a single-tenant To-Do REST API for Azure AD.</span></span> <span data-ttu-id="f0d19-119">Toto je předpokladem pro rychlý Start.</span><span class="sxs-lookup"><span data-stu-id="f0d19-119">This is a prerequisite for the Quick Start.</span></span>

<span data-ttu-id="f0d19-120">Informace o tom, jak nastavit tuto možnost najdete v tématu naše stávající ukázky v [Microsoft Azure Active Directory ukázka REST API Service pro Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f0d19-120">For information on how to set this up, see our existing samples in [Microsoft Azure Active Directory Sample REST API Service for Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span></span>


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a><span data-ttu-id="f0d19-121">Krok 2: Registrace webového rozhraní API s klientovi Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0d19-121">Step 2: Register your web API with your Azure AD tenant</span></span>
<span data-ttu-id="f0d19-122">Služba Active Directory podporuje přidání dva typy aplikací:</span><span class="sxs-lookup"><span data-stu-id="f0d19-122">Active Directory supports adding two types of applications:</span></span>

- <span data-ttu-id="f0d19-123">Webové rozhraní API, které nabízejí služby pro uživatele</span><span class="sxs-lookup"><span data-stu-id="f0d19-123">Web APIs that offer services to users</span></span>
- <span data-ttu-id="f0d19-124">Aplikace (běžící na webu nebo v zařízení), které ty přístup k webovým rozhraním API</span><span class="sxs-lookup"><span data-stu-id="f0d19-124">Applications (running either on the web or on a device) that access those web APIs</span></span>

<span data-ttu-id="f0d19-125">V tomto kroku se registrace webové rozhraní API, kterou používáte místně pro testování této ukázce.</span><span class="sxs-lookup"><span data-stu-id="f0d19-125">In this step, you're registering the web API that you're running locally for testing this sample.</span></span> <span data-ttu-id="f0d19-126">Za normálních okolností tomuto webovému rozhraní API je služba REST, která je funkce, které chcete aplikaci pro přístup do nabídky.</span><span class="sxs-lookup"><span data-stu-id="f0d19-126">Normally, this web API is a REST service that's offering functionality that you want an app to access.</span></span> <span data-ttu-id="f0d19-127">Azure AD můžete chránit libovolný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f0d19-127">Azure AD can help protect any endpoint.</span></span>

<span data-ttu-id="f0d19-128">Jsme se za předpokladu, že jste registrace rozhraní REST API TODO odkazuje dříve.</span><span class="sxs-lookup"><span data-stu-id="f0d19-128">We're assuming that you're registering the TODO REST API referenced earlier.</span></span> <span data-ttu-id="f0d19-129">Ale tento postup funguje pro všechny webové rozhraní API, které chcete Azure Active Directory k ochraně.</span><span class="sxs-lookup"><span data-stu-id="f0d19-129">But this works for any web API that you want Azure Active Directory to help protect.</span></span>

1. <span data-ttu-id="f0d19-130">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f0d19-130">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f0d19-131">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="f0d19-131">On the top bar, click your account.</span></span> <span data-ttu-id="f0d19-132">V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-132">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="f0d19-133">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-133">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="f0d19-134">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-134">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="f0d19-135">Zadejte popisný název pro aplikaci (například **TodoListService**), vyberte **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-135">Enter a friendly name for the application (for example, **TodoListService**), select **Web Application and/or Web API**, and click **Next**.</span></span>
6. <span data-ttu-id="f0d19-136">Přihlášení adresy URL zadejte základní adresu URL pro vzorovou.</span><span class="sxs-lookup"><span data-stu-id="f0d19-136">For the sign-on URL, enter the base URL for the sample.</span></span> <span data-ttu-id="f0d19-137">Ve výchozím nastavení je to `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f0d19-137">By default, this is `https://localhost:8080`.</span></span>
7. <span data-ttu-id="f0d19-138">Klikněte na tlačítko **OK** k dokončení registrace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-138">Click **OK** to complete the registration.</span></span>
8. <span data-ttu-id="f0d19-139">Během portál Azure, přejděte na stránku aplikace, najít hodnotu ID aplikace a zkopírujte jej.</span><span class="sxs-lookup"><span data-stu-id="f0d19-139">While still in the Azure portal, go to your application page, find the application ID value, and copy it.</span></span> <span data-ttu-id="f0d19-140">Budete potřebovat to později při konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-140">You'll need this later when configuring your application.</span></span>
9. <span data-ttu-id="f0d19-141">Z **nastavení** -> **vlastnosti** stránky, aktualizovat identifikátor ID URI aplikace – zadejte `https://<your_tenant_name>/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="f0d19-141">From the **Settings** -> **Properties** page, update the app ID URI - enter `https://<your_tenant_name>/TodoListService`.</span></span> <span data-ttu-id="f0d19-142">Nahraďte `<your_tenant_name>` s názvem vašeho klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0d19-142">Replace `<your_tenant_name>` with the name of your Azure AD tenant.</span></span>

## <a name="step-3-register-the-sample-android-native-client-application"></a><span data-ttu-id="f0d19-143">Krok 3: Registrace ukázkovou aplikaci Android Native Client</span><span class="sxs-lookup"><span data-stu-id="f0d19-143">Step 3: Register the sample Android Native Client application</span></span>
<span data-ttu-id="f0d19-144">V této ukázce je nutné zaregistrovat webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-144">You must register your web application in this sample.</span></span> <span data-ttu-id="f0d19-145">To umožňuje aplikacím komunikovat se právě zaregistrován webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0d19-145">This allows your application to communicate with the just-registered web API.</span></span> <span data-ttu-id="f0d19-146">Azure AD bude odmítnout i, aby vaše aplikace a požádejte o přihlášení, pokud je zaregistrována.</span><span class="sxs-lookup"><span data-stu-id="f0d19-146">Azure AD will refuse to even allow your application to ask for sign-in unless it's registered.</span></span> <span data-ttu-id="f0d19-147">Který je součástí modelu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f0d19-147">That's part of the security of the model.</span></span>

<span data-ttu-id="f0d19-148">Jsme se za předpokladu, že registrace ukázkovou aplikaci odkazuje dříve.</span><span class="sxs-lookup"><span data-stu-id="f0d19-148">We're assuming that you're registering the sample application referenced earlier.</span></span> <span data-ttu-id="f0d19-149">Ale tento postup funguje pro každou aplikaci, která jste vývoj.</span><span class="sxs-lookup"><span data-stu-id="f0d19-149">But this procedure works for any app that you're developing.</span></span>

> [!NOTE]
> <span data-ttu-id="f0d19-150">Může vás zajímat, proč jste uvedení aplikace i webové rozhraní API v jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="f0d19-150">You might wonder why you're putting both an application and a web API in one tenant.</span></span> <span data-ttu-id="f0d19-151">Jak vám může mít uhádnout, můžete vytvořit aplikaci, která přistupuje k externí rozhraní API, která je zaregistrovaná ve službě Azure AD z jiného klienta.</span><span class="sxs-lookup"><span data-stu-id="f0d19-151">As you might have guessed, you can build an app that accesses an external API that is registered in Azure AD from another tenant.</span></span> <span data-ttu-id="f0d19-152">Pokud tak učiníte, vaši zákazníci budou vyzváni k souhlas s použitím rozhraní API v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0d19-152">If you do that, your customers will be prompted to consent to the use of the API in the application.</span></span> <span data-ttu-id="f0d19-153">Knihovna ověřování Active Directory pro iOS postará svůj souhlas pro vás.</span><span class="sxs-lookup"><span data-stu-id="f0d19-153">Active Directory Authentication Library for iOS takes care of this consent for you.</span></span> <span data-ttu-id="f0d19-154">Jak jsme prozkoumat nabízí vyspělejší funkce, uvidíte, že se jedná o důležitou součástí práce potřebné pro přístup k sadě Microsoft APIs z Azure a Office, stejně jako ostatní poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="f0d19-154">As we explore more advanced features, you'll see that this is an important part of the work needed to access the suite of Microsoft APIs from Azure and Office, as well as any other service provider.</span></span> <span data-ttu-id="f0d19-155">Teď protože jste zaregistrovali webového rozhraní API a aplikace v rámci stejné klienta, neuvidíte zobrazování výzev k vyjádření souhlasu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-155">For now, because you registered both your web API and your application under the same tenant, you won't see any prompts for consent.</span></span> <span data-ttu-id="f0d19-156">Je tomu tak obvykle Pokud vyvíjíte aplikaci pouze pro vaši vlastní společnost používat.</span><span class="sxs-lookup"><span data-stu-id="f0d19-156">This is usually the case if you're developing an application just for your own company to use.</span></span>

1. <span data-ttu-id="f0d19-157">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f0d19-157">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f0d19-158">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="f0d19-158">On the top bar, click your account.</span></span> <span data-ttu-id="f0d19-159">V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-159">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="f0d19-160">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-160">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="f0d19-161">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-161">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="f0d19-162">Zadejte popisný název pro aplikaci (například **TodoListClient Android**), vyberte **nativní klientská aplikace**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-162">Enter a friendly name for the application (for example, **TodoListClient-Android**), select **Native Client Application**, and click **Next**.</span></span>
6. <span data-ttu-id="f0d19-163">Identifikátor URI přesměrování, zadejte `http://TodoListClient`.</span><span class="sxs-lookup"><span data-stu-id="f0d19-163">For the redirect URI, enter `http://TodoListClient`.</span></span> <span data-ttu-id="f0d19-164">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-164">Click **Finish**.</span></span>
7. <span data-ttu-id="f0d19-165">Na stránce aplikace najít hodnotu ID aplikace a zkopírujte ho.</span><span class="sxs-lookup"><span data-stu-id="f0d19-165">From the application page, find the application ID value and copy it.</span></span> <span data-ttu-id="f0d19-166">Budete potřebovat to později při konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-166">You'll need this later when configuring your application.</span></span>
8. <span data-ttu-id="f0d19-167">Z **nastavení** vyberte **požadovaných oprávnění** a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-167">From the **Settings** page, select **Required Permissions** and select **Add**.</span></span>  <span data-ttu-id="f0d19-168">Vyhledejte a vyberte TodoListService, přidejte **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f0d19-168">Locate and select TodoListService, add the **Access TodoListService** permission under **Delegated Permissions**, and click **Done**.</span></span>

<span data-ttu-id="f0d19-169">Pokud chcete vytvořit s Maven, můžete použít pom.xml na nejvyšší úrovni:</span><span class="sxs-lookup"><span data-stu-id="f0d19-169">To build with Maven, you can use pom.xml at the top level:</span></span>

1. <span data-ttu-id="f0d19-170">Klonování tohoto úložiště do adresáře podle vašeho výběru:</span><span class="sxs-lookup"><span data-stu-id="f0d19-170">Clone this repo into a directory of your choice:</span></span>

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. <span data-ttu-id="f0d19-171">Postupujte podle kroků v [požadavky pro nastavení prostředí Maven pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span><span class="sxs-lookup"><span data-stu-id="f0d19-171">Follow the steps in the [prerequisites to set up your Maven environment for Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span></span>
3. <span data-ttu-id="f0d19-172">Nastavte emulátoru s SDK 19.</span><span class="sxs-lookup"><span data-stu-id="f0d19-172">Set up the emulator with SDK 19.</span></span>
4. <span data-ttu-id="f0d19-173">Přejděte do kořenové složky, které jste naklonovali úložiště.</span><span class="sxs-lookup"><span data-stu-id="f0d19-173">Go to the root folder where you cloned the repo.</span></span>
5. <span data-ttu-id="f0d19-174">Spusťte tento příkaz:`mvn clean install`</span><span class="sxs-lookup"><span data-stu-id="f0d19-174">Run this command: `mvn clean install`</span></span>
6. <span data-ttu-id="f0d19-175">Změňte adresář na ukázkové rychlý Start:`cd samples\hello`</span><span class="sxs-lookup"><span data-stu-id="f0d19-175">Change the directory to the Quick Start sample: `cd samples\hello`</span></span>
7. <span data-ttu-id="f0d19-176">Spusťte tento příkaz:`mvn android:deploy android:run`</span><span class="sxs-lookup"><span data-stu-id="f0d19-176">Run this command: `mvn android:deploy android:run`</span></span>

   <span data-ttu-id="f0d19-177">Měli byste vidět, spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-177">You should see the app starting.</span></span>
8. <span data-ttu-id="f0d19-178">Zadejte přihlašovací údaje testovacího uživatele a zkuste to.</span><span class="sxs-lookup"><span data-stu-id="f0d19-178">Enter test user credentials to try.</span></span>

<span data-ttu-id="f0d19-179">Balíčky JAR bude odeslána vedle balíček AAR.</span><span class="sxs-lookup"><span data-stu-id="f0d19-179">JAR packages will be submitted beside the AAR package.</span></span>

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a><span data-ttu-id="f0d19-180">Krok 4: Stáhnout Android ADAL a přidejte ji do pracovního prostoru Eclipse</span><span class="sxs-lookup"><span data-stu-id="f0d19-180">Step 4: Download the Android ADAL and add it to your Eclipse workspace</span></span>
<span data-ttu-id="f0d19-181">Provedli jsme ho snadno, mají několik možností použití knihovny ADAL v projektu Android:</span><span class="sxs-lookup"><span data-stu-id="f0d19-181">We've made it easy for you to have multiple options to use ADAL in your Android project:</span></span>

* <span data-ttu-id="f0d19-182">Zdrojový kód můžete použít k importu této knihovny do Eclipse a propojení k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0d19-182">You can use the source code to import this library into Eclipse and link to your application.</span></span>
* <span data-ttu-id="f0d19-183">Pokud používáte Android Studio, můžete použít formát balíčku AAR a odkazovat na binární soubory.</span><span class="sxs-lookup"><span data-stu-id="f0d19-183">If you're using Android Studio, you can use the AAR package format and reference the binaries.</span></span>

### <a name="option-1-source-zip"></a><span data-ttu-id="f0d19-184">Možnost 1: Zdroj Zip</span><span class="sxs-lookup"><span data-stu-id="f0d19-184">Option 1: Source Zip</span></span>
<span data-ttu-id="f0d19-185">Chcete-li stáhnout kopii zdrojového kódu, klikněte na tlačítko **stáhnout ZIP** na pravé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="f0d19-185">To download a copy of the source code, click **Download ZIP** on the right side of the page.</span></span> <span data-ttu-id="f0d19-186">Nebo můžete [stáhnout z webu GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span><span class="sxs-lookup"><span data-stu-id="f0d19-186">Or you can [download from GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span></span>

### <a name="option-2-source-via-git"></a><span data-ttu-id="f0d19-187">Možnost 2: Zdroj pomocí Gitu</span><span class="sxs-lookup"><span data-stu-id="f0d19-187">Option 2: Source via Git</span></span>
<span data-ttu-id="f0d19-188">Chcete-li získat zdrojový kód sady SDK prostřednictvím Git, zadejte:</span><span class="sxs-lookup"><span data-stu-id="f0d19-188">To get the source code of the SDK via Git, type:</span></span>

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a><span data-ttu-id="f0d19-189">Možnost 3: Binární soubory přes Gradle</span><span class="sxs-lookup"><span data-stu-id="f0d19-189">Option 3: Binaries via Gradle</span></span>
<span data-ttu-id="f0d19-190">Binární soubory můžete získat z centrální úložiště Maven.</span><span class="sxs-lookup"><span data-stu-id="f0d19-190">You can get the binaries from the Maven central repo.</span></span> <span data-ttu-id="f0d19-191">Balíček AAR můžou být takto součástí projekt v Android Studio:</span><span class="sxs-lookup"><span data-stu-id="f0d19-191">The AAR package can be included as follows in your project in Android Studio:</span></span>

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

### <a name="option-4-aar-via-maven"></a><span data-ttu-id="f0d19-192">Možnost 4: AAR prostřednictvím Maven</span><span class="sxs-lookup"><span data-stu-id="f0d19-192">Option 4: AAR via Maven</span></span>
<span data-ttu-id="f0d19-193">Pokud používáte modul plug-in M2Eclipse, v souboru pom.xml můžete určit závislost:</span><span class="sxs-lookup"><span data-stu-id="f0d19-193">If you're using the M2Eclipse plug-in, you can specify the dependency in your pom.xml file:</span></span>

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a><span data-ttu-id="f0d19-194">Možnost 5: JAR balíčku naleznete ve složce knihovny</span><span class="sxs-lookup"><span data-stu-id="f0d19-194">Option 5: JAR package inside the libs folder</span></span>
<span data-ttu-id="f0d19-195">Můžete získat na soubor JAR z úložiště Maven a umístěte jej do **knihovny** složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-195">You can get the JAR file from the Maven repo and drop it into the **libs** folder in your project.</span></span> <span data-ttu-id="f0d19-196">Budete muset zkopírovat požadované prostředky do projektu je také možné, protože balíčky JAR neobsahují.</span><span class="sxs-lookup"><span data-stu-id="f0d19-196">You need to copy the required resources to your project as well, because the JAR packages don't include them.</span></span>

## <a name="step-5-add-references-to-android-adal-to-your-project"></a><span data-ttu-id="f0d19-197">Krok 5: Přidejte odkazy na Android ADAL do projektu</span><span class="sxs-lookup"><span data-stu-id="f0d19-197">Step 5: Add references to Android ADAL to your project</span></span>
1. <span data-ttu-id="f0d19-198">Přidat odkaz na projekt a zadejte jej jako knihovna pro Android.</span><span class="sxs-lookup"><span data-stu-id="f0d19-198">Add a reference to your project and specify it as an Android library.</span></span> <span data-ttu-id="f0d19-199">Pokud si nejste jisti, jak na to, získáte další informace [lokality Android Studio](http://developer.android.com/tools/projects/projects-eclipse.html).</span><span class="sxs-lookup"><span data-stu-id="f0d19-199">If you're uncertain how to do this, you can get more information on the [Android Studio site](http://developer.android.com/tools/projects/projects-eclipse.html).</span></span>
2. <span data-ttu-id="f0d19-200">Přidáte závislost projektu ladění do nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-200">Add the project dependency for debugging into your project settings.</span></span>
3. <span data-ttu-id="f0d19-201">Aktualizace souboru AndroidManifest.xml vašeho projektu:</span><span class="sxs-lookup"><span data-stu-id="f0d19-201">Update your project's AndroidManifest.xml file to include:</span></span>

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

4. <span data-ttu-id="f0d19-202">Vytvoření instance kontextu AuthenticationContext v hlavní aktivitě.</span><span class="sxs-lookup"><span data-stu-id="f0d19-202">Create an instance of AuthenticationContext at your main activity.</span></span> <span data-ttu-id="f0d19-203">Podrobnosti o toto volání jsou nad rámec tohoto tématu, ale můžete začít funkčním prohlížením [ukázka Android Native Client](https://github.com/AzureADSamples/NativeClient-Android).</span><span class="sxs-lookup"><span data-stu-id="f0d19-203">The details of this call are beyond the scope of this topic, but you can get a good start by looking at the [Android Native Client sample](https://github.com/AzureADSamples/NativeClient-Android).</span></span> <span data-ttu-id="f0d19-204">V následujícím příkladu je SharedPreferences výchozí mezipaměti a autorita ve formě `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span><span class="sxs-lookup"><span data-stu-id="f0d19-204">In the following example, SharedPreferences is the default cache, and Authority is in the form of `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span></span>

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. <span data-ttu-id="f0d19-205">Zkopírujte tento blok kódu pro zpracování konec AuthenticationActivity poté, co uživatel zadá přihlašovací údaje a přijímá autorizační kód:</span><span class="sxs-lookup"><span data-stu-id="f0d19-205">Copy this code block to handle the end of AuthenticationActivity after the user enters credentials and receives an authorization code:</span></span>

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. <span data-ttu-id="f0d19-206">Chcete-li požádat o token, definujte zpětné volání:</span><span class="sxs-lookup"><span data-stu-id="f0d19-206">To ask for a token, define a callback:</span></span>

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

7. <span data-ttu-id="f0d19-207">Nakonec požádejte o token pomocí že zpětné volání:</span><span class="sxs-lookup"><span data-stu-id="f0d19-207">Finally, ask for a token by using that callback:</span></span>

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

<span data-ttu-id="f0d19-208">Následuje vysvětlení parametrů:</span><span class="sxs-lookup"><span data-stu-id="f0d19-208">Here's an explanation of the parameters:</span></span>

* <span data-ttu-id="f0d19-209">*prostředek* je povinná a je prostředků, které se snažíte získat přístup.</span><span class="sxs-lookup"><span data-stu-id="f0d19-209">*resource* is required and is the resource you're trying to access.</span></span>
* <span data-ttu-id="f0d19-210">*ClientID* je povinná a pochází z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0d19-210">*clientid* is required and comes from Azure AD.</span></span>
* <span data-ttu-id="f0d19-211">*RedirectUri* není nutné je třeba zadat acquireToken volání.</span><span class="sxs-lookup"><span data-stu-id="f0d19-211">*RedirectUri* is not required to be provided for the acquireToken call.</span></span> <span data-ttu-id="f0d19-212">Můžete ho nastavit tak jako název balíčku.</span><span class="sxs-lookup"><span data-stu-id="f0d19-212">You can set it up as your package name.</span></span>
* <span data-ttu-id="f0d19-213">*PromptBehavior* umožňuje požádat o přihlašovací údaje tak, aby přeskočil mezipaměti a souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f0d19-213">*PromptBehavior* helps to ask for credentials to skip the cache and cookie.</span></span>
* <span data-ttu-id="f0d19-214">*zpětné volání* je volána po autorizační kód se vyměňují pro token.</span><span class="sxs-lookup"><span data-stu-id="f0d19-214">*callback* is called after the authorization code is exchanged for a token.</span></span> <span data-ttu-id="f0d19-215">Obsahuje objekt AuthenticationResult, který obsahuje přístupový token, datum vypršela platnost a ID informace o tokenu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-215">It has an object of AuthenticationResult, which has access token, date expired, and ID token information.</span></span>
* <span data-ttu-id="f0d19-216">*acquireTokenSilent* je volitelný.</span><span class="sxs-lookup"><span data-stu-id="f0d19-216">*acquireTokenSilent* is optional.</span></span> <span data-ttu-id="f0d19-217">Můžete ji volat popisovač ukládání do mezipaměti a token obnovení.</span><span class="sxs-lookup"><span data-stu-id="f0d19-217">You can call it to handle caching and token refresh.</span></span> <span data-ttu-id="f0d19-218">Nabízí taky verze synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-218">It also provides the sync version.</span></span> <span data-ttu-id="f0d19-219">Přijímá *userId* jako parametr.</span><span class="sxs-lookup"><span data-stu-id="f0d19-219">It accepts *userId* as a parameter.</span></span>

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="f0d19-220">Pomocí tohoto návodu, byste měli mít, co je potřeba úspěšně integraci se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f0d19-220">By using this walkthrough, you should have what you need to successfully integrate with Azure Active Directory.</span></span> <span data-ttu-id="f0d19-221">Další příklady tato práce, najdete v článku AzureADSamples nebo úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-221">For more examples of this working, visit the AzureADSamples/ repository on GitHub.</span></span>

## <a name="important-information"></a><span data-ttu-id="f0d19-222">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="f0d19-222">Important information</span></span>
### <a name="customization"></a><span data-ttu-id="f0d19-223">Přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="f0d19-223">Customization</span></span>
<span data-ttu-id="f0d19-224">Vaše prostředky aplikace můžete přepsat projektu prostředky knihovny.</span><span class="sxs-lookup"><span data-stu-id="f0d19-224">Your application resources can overwrite library project resources.</span></span> <span data-ttu-id="f0d19-225">To se stane, když sestavuje vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-225">This happens when your app is being built.</span></span> <span data-ttu-id="f0d19-226">Z tohoto důvodu můžete přizpůsobit ověřování aktivity rozložení požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f0d19-226">For this reason, you can customize authentication activity layout the way you want.</span></span> <span data-ttu-id="f0d19-227">Ujistěte se, že zachovat ID ovládacích prvků, že používá ADAL (webového zobrazení).</span><span class="sxs-lookup"><span data-stu-id="f0d19-227">Be sure to keep the ID of the controls that ADAL uses (WebView).</span></span>

### <a name="broker"></a><span data-ttu-id="f0d19-228">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="f0d19-228">Broker</span></span>
<span data-ttu-id="f0d19-229">Aplikace portál společnosti Microsoft Intune poskytuje komponentu zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-229">The Microsoft Intune Company Portal app provides the broker component.</span></span> <span data-ttu-id="f0d19-230">Účet je vytvořen v AccountManager.</span><span class="sxs-lookup"><span data-stu-id="f0d19-230">The account is created in AccountManager.</span></span> <span data-ttu-id="f0d19-231">Typ účtu je "com.microsoft.workaccount."</span><span class="sxs-lookup"><span data-stu-id="f0d19-231">The account type is "com.microsoft.workaccount."</span></span> <span data-ttu-id="f0d19-232">AccountManager umožňuje jenom jeden účet jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0d19-232">AccountManager allows only a single SSO account.</span></span> <span data-ttu-id="f0d19-233">Vytvoří soubor cookie jednotného přihlašování pro uživatele po dokončení zařízení výzvu pro jednu z aplikací.</span><span class="sxs-lookup"><span data-stu-id="f0d19-233">It creates an SSO cookie for the user after completing the device challenge for one of the apps.</span></span>

<span data-ttu-id="f0d19-234">ADAL používá zprostředkovatele účtu, pokud jeden uživatelský účet je vytvořen v tato ověřovací data a můžete zvolit nepoužívání přeskočit ho.</span><span class="sxs-lookup"><span data-stu-id="f0d19-234">ADAL uses the broker account if one user account is created at this authenticator and you choose not to skip it.</span></span> <span data-ttu-id="f0d19-235">Můžete přeskočit zprostředkovatele uživatele pomocí:</span><span class="sxs-lookup"><span data-stu-id="f0d19-235">You can skip the broker user with:</span></span>

   `AuthenticationSettings.Instance.setSkipBroker(true);`

<span data-ttu-id="f0d19-236">Je třeba zaregistrovat speciální RedirectUri pro použití zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-236">You need to register a special RedirectUri for broker usage.</span></span> <span data-ttu-id="f0d19-237">RedirectUri je ve formátu `msauth://packagename/Base64UrlencodedSignature`.</span><span class="sxs-lookup"><span data-stu-id="f0d19-237">RedirectUri is in the format of `msauth://packagename/Base64UrlencodedSignature`.</span></span> <span data-ttu-id="f0d19-238">Vaše RedirectUri pro vaši aplikaci můžete získat pomocí skriptu brokerRedirectPrint.ps1 nebo mContext.getBrokerRedirectUri volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0d19-238">You can get your RedirectUri for your app by using the script brokerRedirectPrint.ps1 or the API call mContext.getBrokerRedirectUri.</span></span> <span data-ttu-id="f0d19-239">Podpis se vztahuje k podepisování certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f0d19-239">The signature is related to your signing certificates.</span></span>

<span data-ttu-id="f0d19-240">Aktuální model zprostředkovatele je pro jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-240">The current broker model is for one user.</span></span> <span data-ttu-id="f0d19-241">Kontextu AuthenticationContext poskytuje metodu API získat zprostředkovatele uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-241">AuthenticationContext provides the API method to get the broker user.</span></span>

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

<span data-ttu-id="f0d19-242">Manifest aplikace by měla mít následující oprávnění k používání AccountManager účtů.</span><span class="sxs-lookup"><span data-stu-id="f0d19-242">Your app manifest should have the following permissions to use AccountManager accounts.</span></span> <span data-ttu-id="f0d19-243">Podrobnosti najdete v tématu [AccountManager informace na webu Android](http://developer.android.com/reference/android/accounts/AccountManager.html).</span><span class="sxs-lookup"><span data-stu-id="f0d19-243">For details, see the [AccountManager information on the Android site](http://developer.android.com/reference/android/accounts/AccountManager.html).</span></span>

* <span data-ttu-id="f0d19-244">GET_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="f0d19-244">GET_ACCOUNTS</span></span>
* <span data-ttu-id="f0d19-245">USE_CREDENTIALS</span><span class="sxs-lookup"><span data-stu-id="f0d19-245">USE_CREDENTIALS</span></span>
* <span data-ttu-id="f0d19-246">MANAGE_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="f0d19-246">MANAGE_ACCOUNTS</span></span>

### <a name="authority-url-and-ad-fs"></a><span data-ttu-id="f0d19-247">Adresa URL autority a AD FS</span><span class="sxs-lookup"><span data-stu-id="f0d19-247">Authority URL and AD FS</span></span>
<span data-ttu-id="f0d19-248">Active Directory Federation Services (AD FS) není rozpoznán jako produkční služby tokenů zabezpečení, takže budete muset zapnout zjišťování instance a předá hodnotu false v konstruktoru kontextu AuthenticationContext.</span><span class="sxs-lookup"><span data-stu-id="f0d19-248">Active Directory Federation Services (AD FS) is not recognized as production STS, so you need to turn of instance discovery and pass false at the AuthenticationContext constructor.</span></span>

<span data-ttu-id="f0d19-249">Adresa URL autority potřebuje instance služby tokenů zabezpečení a [název klienta](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="f0d19-249">The authority URL needs an STS instance and a [tenant name](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span></span>

### <a name="querying-cache-items"></a><span data-ttu-id="f0d19-250">Dotazování na položky mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f0d19-250">Querying cache items</span></span>
<span data-ttu-id="f0d19-251">ADAL poskytuje výchozí mezipaměti SharedPreferences s některé jednoduché mezipaměti funkce dotazování.</span><span class="sxs-lookup"><span data-stu-id="f0d19-251">ADAL provides a default cache in SharedPreferences with some simple cache query functions.</span></span> <span data-ttu-id="f0d19-252">Aktuální mezipaměť můžete získat z kontextu AuthenticationContext s použitím:</span><span class="sxs-lookup"><span data-stu-id="f0d19-252">You can get the current cache from AuthenticationContext by using:</span></span>

    ITokenCacheStore cache = mContext.getCache();

<span data-ttu-id="f0d19-253">Pokud chcete přizpůsobit, můžete zadat taky implementaci mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f0d19-253">You can also provide your cache implementation, if you want to customize it.</span></span>

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a><span data-ttu-id="f0d19-254">Chování výzvy</span><span class="sxs-lookup"><span data-stu-id="f0d19-254">Prompt behavior</span></span>
<span data-ttu-id="f0d19-255">ADAL nabízí možnost, chcete-li určit chování výzvy.</span><span class="sxs-lookup"><span data-stu-id="f0d19-255">ADAL provides an option to specify prompt behavior.</span></span> <span data-ttu-id="f0d19-256">PromptBehavior.Auto se zobrazí uživatelské rozhraní, pokud token obnovení je neplatný a jsou vyžadována pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0d19-256">PromptBehavior.Auto will show the UI if the refresh token is invalid and user credentials are required.</span></span> <span data-ttu-id="f0d19-257">PromptBehavior.Always bude přeskočit využití mezipaměti a vždy zobrazí uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f0d19-257">PromptBehavior.Always will skip the cache usage and always show the UI.</span></span>

### <a name="silent-token-request-from-cache-and-refresh"></a><span data-ttu-id="f0d19-258">Tichou žádosti o token z mezipaměti a aktualizace</span><span class="sxs-lookup"><span data-stu-id="f0d19-258">Silent token request from cache and refresh</span></span>
<span data-ttu-id="f0d19-259">Tichou žádosti o token nepoužívá místní uživatelské rozhraní a nevyžaduje aktivitu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-259">A silent token request does not use the UI pop-up and does not require an activity.</span></span> <span data-ttu-id="f0d19-260">Vrátí token z mezipaměti, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f0d19-260">It returns a token from the cache if available.</span></span> <span data-ttu-id="f0d19-261">Pokud je platnost tokenu, pokusí se tato metoda jej aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f0d19-261">If the token is expired, this method tries to refresh it.</span></span> <span data-ttu-id="f0d19-262">Pokud token obnovení vyprší nebo se nezdařilo, vrátí authenticationexception –.</span><span class="sxs-lookup"><span data-stu-id="f0d19-262">If the refresh token is expired or failed, it returns AuthenticationException.</span></span>

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="f0d19-263">Můžete provést také synchronizace volat pomocí této metody.</span><span class="sxs-lookup"><span data-stu-id="f0d19-263">You can also make a sync call by using this method.</span></span> <span data-ttu-id="f0d19-264">Můžete nastavit hodnotu null pro zpětné volání nebo použít acquireTokenSilentSync.</span><span class="sxs-lookup"><span data-stu-id="f0d19-264">You can set null to callback or use acquireTokenSilentSync.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="f0d19-265">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="f0d19-265">Diagnostics</span></span>
<span data-ttu-id="f0d19-266">Toto jsou primární zdroje informací pro diagnostiku problémů:</span><span class="sxs-lookup"><span data-stu-id="f0d19-266">These are the primary sources of information for diagnosing issues:</span></span>

* <span data-ttu-id="f0d19-267">Výjimky</span><span class="sxs-lookup"><span data-stu-id="f0d19-267">Exceptions</span></span>
* <span data-ttu-id="f0d19-268">Logs</span><span class="sxs-lookup"><span data-stu-id="f0d19-268">Logs</span></span>
* <span data-ttu-id="f0d19-269">Trasování sítě</span><span class="sxs-lookup"><span data-stu-id="f0d19-269">Network traces</span></span>

<span data-ttu-id="f0d19-270">Všimněte si, že jsou ID korelace centrální diagnostiku v knihovně.</span><span class="sxs-lookup"><span data-stu-id="f0d19-270">Note that correlation IDs are central to the diagnostics in the library.</span></span> <span data-ttu-id="f0d19-271">Můžete nastavit vaše korelace požadavku ID na základě požadavků, pokud chcete ke korelaci ADAL s dalšími operacemi v kódu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-271">You can set your correlation IDs on a per-request basis if you want to correlate an ADAL request with other operations in your code.</span></span> <span data-ttu-id="f0d19-272">Pokud není nastavený ID korelace, ADAL vygeneruje náhodné.</span><span class="sxs-lookup"><span data-stu-id="f0d19-272">If you don't set a correlation ID, ADAL will generate a random one.</span></span> <span data-ttu-id="f0d19-273">Všechny zprávy protokolu a volání síť pak bude být označený ID korelace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-273">All log messages and network calls will then be stamped with the correlation ID.</span></span> <span data-ttu-id="f0d19-274">Vygenerovaný sám sebou ID změny na každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="f0d19-274">The self-generated ID changes on each request.</span></span>

#### <a name="exceptions"></a><span data-ttu-id="f0d19-275">Výjimky</span><span class="sxs-lookup"><span data-stu-id="f0d19-275">Exceptions</span></span>
<span data-ttu-id="f0d19-276">Výjimky jsou první diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="f0d19-276">Exceptions are the first diagnostic.</span></span> <span data-ttu-id="f0d19-277">Pokusíme se poskytují užitečné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="f0d19-277">We try to provide helpful error messages.</span></span> <span data-ttu-id="f0d19-278">Pokud zjistíte, ten, který není užitečné, oznamte problém a dejte nám vědět.</span><span class="sxs-lookup"><span data-stu-id="f0d19-278">If you find one that is not helpful, please file an issue and let us know.</span></span> <span data-ttu-id="f0d19-279">Zahrnují informace o zařízení, jako je například modelu a číslo SDK.</span><span class="sxs-lookup"><span data-stu-id="f0d19-279">Include device information such as model and SDK number.</span></span>

#### <a name="logs"></a><span data-ttu-id="f0d19-280">Logs</span><span class="sxs-lookup"><span data-stu-id="f0d19-280">Logs</span></span>
<span data-ttu-id="f0d19-281">Můžete nakonfigurovat knihovně k vygenerování zprávy protokolu, které můžete použít pro usnadnění diagnostiky problémů.</span><span class="sxs-lookup"><span data-stu-id="f0d19-281">You can configure the library to generate log messages that you can use to help diagnose issues.</span></span> <span data-ttu-id="f0d19-282">Konfigurace protokolování tím, že provedete následující volání pro zpětné volání, které ADAL použije k ručně vypnout každé zprávě protokolu je generovaná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-282">You configure logging by making the following call to configure a callback that ADAL will use to hand off each log message as it's generated.</span></span>

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this to log file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

<span data-ttu-id="f0d19-283">Jak je znázorněno v následujícím kódu, mohou být zprávy zapisovány do vlastního souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-283">Messages can be written to a custom log file, as shown in the following code.</span></span> <span data-ttu-id="f0d19-284">Bohužel neexistuje žádný standardní způsob získání protokolů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0d19-284">Unfortunately, there is no standard way of getting logs from a device.</span></span> <span data-ttu-id="f0d19-285">Existují některé služby, které vám mohou pomoci s to.</span><span class="sxs-lookup"><span data-stu-id="f0d19-285">There are some services that can help you with this.</span></span> <span data-ttu-id="f0d19-286">Vám může také vytvořte vlastní, například odeslání souboru na server.</span><span class="sxs-lookup"><span data-stu-id="f0d19-286">You can also invent your own, such as sending the file to a server.</span></span>

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

<span data-ttu-id="f0d19-287">Toto jsou úrovní protokolování:</span><span class="sxs-lookup"><span data-stu-id="f0d19-287">These are the logging levels:</span></span>
* <span data-ttu-id="f0d19-288">Chyba (výjimek)</span><span class="sxs-lookup"><span data-stu-id="f0d19-288">Error (exceptions)</span></span>
* <span data-ttu-id="f0d19-289">Warn (upozornění)</span><span class="sxs-lookup"><span data-stu-id="f0d19-289">Warn (warning)</span></span>
* <span data-ttu-id="f0d19-290">Informace o (informační účely)</span><span class="sxs-lookup"><span data-stu-id="f0d19-290">Info (information purposes)</span></span>
* <span data-ttu-id="f0d19-291">Verbose (podrobnosti)</span><span class="sxs-lookup"><span data-stu-id="f0d19-291">Verbose (more details)</span></span>

<span data-ttu-id="f0d19-292">Můžete nastavit úroveň protokolu takto:</span><span class="sxs-lookup"><span data-stu-id="f0d19-292">You set the log level like this:</span></span>

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 <span data-ttu-id="f0d19-293">Všechny zprávy protokolu jsou odesílány logcat, kromě všech zpětná volání vlastního protokolu.</span><span class="sxs-lookup"><span data-stu-id="f0d19-293">All log messages are sent to logcat, in addition to any custom log callbacks.</span></span>
<span data-ttu-id="f0d19-294">Můžete získat protokolu do souboru logcat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f0d19-294">You can get a log to a file from logcat as follows:</span></span>

    adb logcat > "C:\logmsg\logfile.txt"

 <span data-ttu-id="f0d19-295">Podrobnosti o adb příkazů najdete v tématu [logcat informace na webu Android](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span><span class="sxs-lookup"><span data-stu-id="f0d19-295">For details about adb commands, see the [logcat information on the Android site](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span></span>

#### <a name="network-traces"></a><span data-ttu-id="f0d19-296">Trasování sítě</span><span class="sxs-lookup"><span data-stu-id="f0d19-296">Network traces</span></span>
<span data-ttu-id="f0d19-297">Různé nástroje můžete zaznamenávat provoz protokolu HTTP, který generuje ADAL.</span><span class="sxs-lookup"><span data-stu-id="f0d19-297">You can use various tools to capture the HTTP traffic that ADAL generates.</span></span>  <span data-ttu-id="f0d19-298">To je velmi užitečné, pokud jste obeznámeni s protokolem OAuth, nebo pokud potřebujete poskytovat diagnostické informace do společnosti Microsoft nebo jiné kanály podpory.</span><span class="sxs-lookup"><span data-stu-id="f0d19-298">This is most useful if you're familiar with the OAuth protocol or if you need to provide diagnostic information to Microsoft or other support channels.</span></span>

<span data-ttu-id="f0d19-299">Fiddler je nejjednodušší nástroj trasování HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0d19-299">Fiddler is the easiest HTTP tracing tool.</span></span> <span data-ttu-id="f0d19-300">Pomocí následujících odkazů pro ni nastavit až správně záznamů ADAL síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="f0d19-300">Use the following links to set it up to correctly record ADAL network traffic.</span></span> <span data-ttu-id="f0d19-301">Pro trasování nástroje, jako je Fiddler nebo Charlese být užitečné je nutné nakonfigurovat ji k zaznamenání nešifrované přenosy protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="f0d19-301">For a tracing tool like Fiddler or Charles to be useful, you must configure it to record unencrypted SSL traffic.</span></span>  

> [!NOTE]
> <span data-ttu-id="f0d19-302">Trasování generované tímto způsobem může obsahovat vysoce důvěrné informace, například přístupové tokeny, uživatelská jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="f0d19-302">Traces generated in this way might contain highly privileged information such as access tokens, usernames, and passwords.</span></span> <span data-ttu-id="f0d19-303">Pokud používáte produkční účty, nesdílí toto trasování s třetími stranami.</span><span class="sxs-lookup"><span data-stu-id="f0d19-303">If you're using production accounts, do not share these traces with third parties.</span></span> <span data-ttu-id="f0d19-304">Pokud budete muset zadat trasování uživateli, aby bylo možné získat podporu, reprodukujte problém pomocí dočasného účtu s uživatelských jmen a hesel, která vás sdílení.</span><span class="sxs-lookup"><span data-stu-id="f0d19-304">If you need to supply a trace to someone in order to get support, reproduce the issue by using a temporary account with usernames and passwords that you don't mind sharing.</span></span>

* <span data-ttu-id="f0d19-305">Z webu Telerik webu: [nastavení si aplikaci Fiddler pro Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span><span class="sxs-lookup"><span data-stu-id="f0d19-305">From the Telerik website: [Setting Up Fiddler For Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span></span>
* <span data-ttu-id="f0d19-306">Z Githubu: [nakonfigurovat aplikaci Fiddler pravidla pro ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span><span class="sxs-lookup"><span data-stu-id="f0d19-306">From GitHub: [Configure Fiddler Rules For ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span></span>

### <a name="dialog-mode"></a><span data-ttu-id="f0d19-307">Dialogové okno režimu</span><span class="sxs-lookup"><span data-stu-id="f0d19-307">Dialog mode</span></span>
<span data-ttu-id="f0d19-308">Metoda acquireToken bez aktivity podporuje řádku dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0d19-308">The acquireToken method without activity supports a dialog prompt.</span></span>

### <a name="encryption"></a><span data-ttu-id="f0d19-309">Šifrování</span><span class="sxs-lookup"><span data-stu-id="f0d19-309">Encryption</span></span>
<span data-ttu-id="f0d19-310">ADAL šifruje tokeny a úložiště v SharedPreferences ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f0d19-310">ADAL encrypts the tokens and store in SharedPreferences by default.</span></span> <span data-ttu-id="f0d19-311">Můžete si prohlédnout třídy StorageHelper a zobrazit podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f0d19-311">You can look at the StorageHelper class to see the details.</span></span> <span data-ttu-id="f0d19-312">Android zavedl Android úložiště klíčů pro 4.3 (rozhraní API 18) bezpečné uložení privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="f0d19-312">Android introduced Android Keystore for 4.3 (API 18) secure storage of private keys.</span></span> <span data-ttu-id="f0d19-313">ADAL použije tento 18 rozhraní API a vyšší.</span><span class="sxs-lookup"><span data-stu-id="f0d19-313">ADAL uses that for API 18 and higher.</span></span> <span data-ttu-id="f0d19-314">Pokud chcete pomocí knihovny ADAL pro nižší verze sady SDK, budete muset zadat tajný klíč v AuthenticationSettings.INSTANCE.setSecretKey.</span><span class="sxs-lookup"><span data-stu-id="f0d19-314">If you want to use ADAL for lower SDK versions, you need to provide a secret key at AuthenticationSettings.INSTANCE.setSecretKey.</span></span>

### <a name="oauth2-bearer-challenge"></a><span data-ttu-id="f0d19-315">Výzvy nosiče OAuth2</span><span class="sxs-lookup"><span data-stu-id="f0d19-315">OAuth2 bearer challenge</span></span>
<span data-ttu-id="f0d19-316">Třída AuthenticationParameters poskytuje funkci k získání authorization_uri z výzvy nosiče OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f0d19-316">The AuthenticationParameters class provides functionality to get authorization_uri from the OAuth2 bearer challenge.</span></span>

### <a name="session-cookies-in-webview"></a><span data-ttu-id="f0d19-317">Soubory cookie relace ve webovém zobrazení</span><span class="sxs-lookup"><span data-stu-id="f0d19-317">Session cookies in WebView</span></span>
<span data-ttu-id="f0d19-318">Android webové zobrazení nevymaže soubory cookie relace po zavření aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0d19-318">Android WebView does not clear session cookies after the app is closed.</span></span> <span data-ttu-id="f0d19-319">Která dokáže zpracovat pomocí ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="f0d19-319">You can handle that by using this sample code:</span></span>

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

<span data-ttu-id="f0d19-320">Podrobnosti o souborech cookie najdete v tématu [CookieSyncManager informace na webu Android](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span><span class="sxs-lookup"><span data-stu-id="f0d19-320">For details about cookies, see the [CookieSyncManager information on the Android site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span></span>

### <a name="resource-overrides"></a><span data-ttu-id="f0d19-321">Přepsání prostředků</span><span class="sxs-lookup"><span data-stu-id="f0d19-321">Resource overrides</span></span>
<span data-ttu-id="f0d19-322">Knihovna ADAL zahrnuje anglické řetězce pro zprávy ProgressDialog.</span><span class="sxs-lookup"><span data-stu-id="f0d19-322">The ADAL library includes English strings for ProgressDialog messages.</span></span> <span data-ttu-id="f0d19-323">Aplikace je měli přepsat, pokud chcete, aby lokalizovaných řetězců.</span><span class="sxs-lookup"><span data-stu-id="f0d19-323">Your application should overwrite them if you want localized strings.</span></span>

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a><span data-ttu-id="f0d19-324">Dialogové okno protokolu NTLM</span><span class="sxs-lookup"><span data-stu-id="f0d19-324">NTLM dialog box</span></span>
<span data-ttu-id="f0d19-325">ADAL verze 1.1.0 podporuje dialogu protokolu NTLM, který zpracovává se pomocí onReceivedHttpAuthRequest událost z WebViewClient.</span><span class="sxs-lookup"><span data-stu-id="f0d19-325">ADAL version 1.1.0 supports an NTLM dialog box that is processed through the onReceivedHttpAuthRequest event from WebViewClient.</span></span> <span data-ttu-id="f0d19-326">Můžete přizpůsobit rozložení a řetězce pro dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0d19-326">You can customize the layout and strings for the dialog box.</span></span>

### <a name="cross-app-sso"></a><span data-ttu-id="f0d19-327">Jednotné přihlašování napříč aplikacemi</span><span class="sxs-lookup"><span data-stu-id="f0d19-327">Cross-app SSO</span></span>
<span data-ttu-id="f0d19-328">Další informace [postup povolení jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL](active-directory-sso-android.md).</span><span class="sxs-lookup"><span data-stu-id="f0d19-328">Learn [how to enable cross-app SSO on Android by using ADAL](active-directory-sso-android.md).</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
