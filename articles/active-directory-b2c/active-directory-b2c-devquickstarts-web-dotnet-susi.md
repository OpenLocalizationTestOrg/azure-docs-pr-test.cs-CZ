---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Jak profil toobuild webové aplikace, která má registrace-množství nebo přihlášení, úpravy a resetování hesla pomocí Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 187f99a8dd50d212de4f0517f552cdbbe5a8edf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a><span data-ttu-id="76b07-103">Vytvoření webové aplikace ASP.NET s Azure Active Directory B2C profil registrace, přihlášení, úpravy a resetování hesla</span><span class="sxs-lookup"><span data-stu-id="76b07-103">Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset</span></span>

<span data-ttu-id="76b07-104">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="76b07-104">This tutorial shows you how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="76b07-105">Přidání Azure AD B2C identity funkce tooyour webové aplikace</span><span class="sxs-lookup"><span data-stu-id="76b07-105">Add Azure AD B2C identity features tooyour web app</span></span>
> * <span data-ttu-id="76b07-106">Registrace vaší webové aplikace ve vašem adresáři Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="76b07-106">Register your web app in your Azure AD B2C directory</span></span>
> * <span data-ttu-id="76b07-107">Vytvoření uživatele registrace-množství nebo přihlášení, upravit profil a zásady resetování hesel pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="76b07-107">Create a user sign-up/sign-in, profile edit, and password reset policy for your web app</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76b07-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76b07-108">Prerequisites</span></span>

- <span data-ttu-id="76b07-109">Je nutné připojit vašeho klienta B2C tooan účet Azure.</span><span class="sxs-lookup"><span data-stu-id="76b07-109">You must connect your B2C Tenant tooan Azure account.</span></span> <span data-ttu-id="76b07-110">Můžete vytvořit bezplatný účet Azure [zde](https://azure.microsoft.com/en-us/).</span><span class="sxs-lookup"><span data-stu-id="76b07-110">You can create a free Azure account [here](https://azure.microsoft.com/en-us/).</span></span>
- <span data-ttu-id="76b07-111">Je třeba [Microsoft Visual Studio](https://www.visualstudio.com/) nebo podobné programu tooview a upravit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="76b07-111">You need [Microsoft Visual Studio](https://www.visualstudio.com/) or a similar program tooview and modify hello sample code.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="76b07-112">Vytvoření adresáře Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="76b07-112">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="76b07-113">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="76b07-113">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="76b07-114">Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="76b07-114">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="76b07-115">Pokud nemáte již, vytvořte adresář B2C, než budete pokračovat v této příručce.</span><span class="sxs-lookup"><span data-stu-id="76b07-115">If you don't have one already, create a B2C directory before you continue in this guide.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> <span data-ttu-id="76b07-116">Je nutné tooconnect hello klienta B2C tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="76b07-116">You need tooconnect hello B2C Tenant tooyour Azure subscription.</span></span> <span data-ttu-id="76b07-117">Po výběru **vytvořit**, vyberte hello **toomy odkaz existující Azure AD B2C klienta předplatného Azure** možnost a potom v hello **klienta Azure AD B2C** rozevírací nabídku, vyberte Chcete-li tooassociate Hello klienta.</span><span class="sxs-lookup"><span data-stu-id="76b07-117">After selecting **Create**, select hello **Link an existing Azure AD B2C Tenant toomy Azure subscription** option, and then in hello **Azure AD B2C Tenant** drop down, select hello tenant you want tooassociate.</span></span>

## <a name="create-and-register-an-application"></a><span data-ttu-id="76b07-118">Vytvoření a registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="76b07-118">Create and register an application</span></span>

<span data-ttu-id="76b07-119">V dalším kroku třeba toocreate a zaregistrujte hello aplikace ve svém adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="76b07-119">Next, you need toocreate and register hello app in your B2C directory.</span></span> <span data-ttu-id="76b07-120">Obsahuje informace, že Azure AD B2C musí toosecurely komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="76b07-120">This provides information that Azure AD B2C needs toosecurely communicate with your app.</span></span> 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

<span data-ttu-id="76b07-121">Až skončíte, bude mít rozhraní API a nativní aplikace v nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b07-121">When you are done, you will have both an API and a native application in your application settings.</span></span>

## <a name="create-policies-on-your-b2c-tenant"></a><span data-ttu-id="76b07-122">Vytvořit zásady na svého klienta B2C</span><span class="sxs-lookup"><span data-stu-id="76b07-122">Create policies on your B2C tenant</span></span>

<span data-ttu-id="76b07-123">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="76b07-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="76b07-124">Tato ukázka kódu obsahuje tři činnosti identity: **registrace a přihlášení**, **profilu upravit**, a **resetování hesla**.</span><span class="sxs-lookup"><span data-stu-id="76b07-124">This code sample contains three identity experiences: **sign-up & sign-in**, **profile edit**, and **password reset**.</span></span>  <span data-ttu-id="76b07-125">Je třeba jedna zásada toocreate každého typu, jak je popsáno v hello [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="76b07-125">You need toocreate one policy of each type, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="76b07-126">Pro každou zásadu být jisti tooselect hello zobrazovaný název atributu nebo deklarace identity a toocopy dolů hello název vaší zásady pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="76b07-126">For each policy, be sure tooselect hello Display name attribute or claim, and toocopy down hello name of your policy for later use.</span></span>

### <a name="add-your-identity-providers"></a><span data-ttu-id="76b07-127">Přidat vaši zprostředkovatelé identity</span><span class="sxs-lookup"><span data-stu-id="76b07-127">Add your identity providers</span></span>

<span data-ttu-id="76b07-128">Nastavení, vyberte **zprostředkovatelů Identity** a zvolte registrace uživatelského jména nebo e-mailu registrace.</span><span class="sxs-lookup"><span data-stu-id="76b07-128">From your settings, select **Identity Providers** and choose Username signup or Email signup.</span></span>

### <a name="create-a-sign-up-and-sign-in-policy"></a><span data-ttu-id="76b07-129">Vytvoření zásad registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="76b07-129">Create a sign-up and sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a><span data-ttu-id="76b07-130">Vytvoření profilu úpravy zásad</span><span class="sxs-lookup"><span data-stu-id="76b07-130">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a><span data-ttu-id="76b07-131">Vytvoření zásady resetování hesel</span><span class="sxs-lookup"><span data-stu-id="76b07-131">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

<span data-ttu-id="76b07-132">Po vytvoření zásad jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b07-132">After you create your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="76b07-133">Stáhněte si ukázkový kód hello</span><span class="sxs-lookup"><span data-stu-id="76b07-133">Download hello sample code</span></span>

<span data-ttu-id="76b07-134">Hello kódu pro účely tohoto kurzu je udržovaný na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="76b07-134">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="76b07-135">Ukázka hello může klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="76b07-135">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="76b07-136">Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit.</span><span class="sxs-lookup"><span data-stu-id="76b07-136">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="76b07-137">Hello soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="76b07-137">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="76b07-138">`TaskWebApp`je hello webové aplikace MVC, která hello uživatel komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="76b07-138">`TaskWebApp` is hello MVC web application that hello user interacts with.</span></span> <span data-ttu-id="76b07-139">`TaskService`je back endové webové rozhraní API hello aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="76b07-139">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="76b07-140">V tomto článku pouze zabývat hello `TaskWebApp` aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b07-140">This article will only discuss hello `TaskWebApp` application.</span></span> <span data-ttu-id="76b07-141">toolearn jak toobuild `TaskService` pomocí Azure AD B2C, najdete v tématu [naše kurz webové rozhraní api .NET](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="76b07-141">toolearn how toobuild `TaskService` using Azure AD B2C, see [our .NET web api tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="update-code-toouse-your-tenant-and-policies"></a><span data-ttu-id="76b07-142">Aktualizovat toouse kód klienta a zásad</span><span class="sxs-lookup"><span data-stu-id="76b07-142">Update code toouse your tenant and policies</span></span>

<span data-ttu-id="76b07-143">Naše ukázka je ID zásad a klienta hello nakonfigurované toouse naše ukázka klienta.</span><span class="sxs-lookup"><span data-stu-id="76b07-143">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="76b07-144">tooconnect ho tooyour vlastní klienta, je nutné tooopen `web.config` v hello `TaskWebApp` projektu a nahraďte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="76b07-144">tooconnect it tooyour own tenant, you need tooopen `web.config` in hello `TaskWebApp` project and replace hello following values:</span></span>

* <span data-ttu-id="76b07-145">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="76b07-145">`ida:Tenant` with your tenant name</span></span>
* <span data-ttu-id="76b07-146">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="76b07-146">`ida:ClientId` with your web app application ID</span></span>
* <span data-ttu-id="76b07-147">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="76b07-147">`ida:ClientSecret` with your web app secret key</span></span>
* <span data-ttu-id="76b07-148">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="76b07-148">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
* <span data-ttu-id="76b07-149">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="76b07-149">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
* <span data-ttu-id="76b07-150">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="76b07-150">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>

## <a name="launch-hello-app"></a><span data-ttu-id="76b07-151">Spusťte aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="76b07-151">Launch hello app</span></span>
<span data-ttu-id="76b07-152">Z Visual Studia, spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-152">From within Visual Studio, launch hello app.</span></span> <span data-ttu-id="76b07-153">Přejděte toohello kartě seznam úkolů a Poznámka hello je adresa URl: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...</span><span class="sxs-lookup"><span data-stu-id="76b07-153">Navigate toohello To-Do List tab, and note hello URl is: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*&client_id=*YourclientID*.....</span></span>

<span data-ttu-id="76b07-154">Zaregistrujte se pro aplikaci hello pomocí e-mailové adresy nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="76b07-154">Sign up for hello app by using your email address or user name.</span></span> <span data-ttu-id="76b07-155">Odhlásit se, potom znovu přihlásit a upravit profil hello nebo resetování hesla hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-155">Sign out, then sign in again and edit hello profile or reset hello password.</span></span> <span data-ttu-id="76b07-156">Odhlásit se a přihlaste se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="76b07-156">Sign out and sign in as a different user.</span></span> 

## <a name="add-social-idps"></a><span data-ttu-id="76b07-157">Přidat sociálních IDPs</span><span class="sxs-lookup"><span data-stu-id="76b07-157">Add social IDPs</span></span>

<span data-ttu-id="76b07-158">V současné době podporuje pouze uživatel registrace a přihlášení pomocí aplikace hello **místní účty**; uložené v adresáři B2C účty, které používají uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="76b07-158">Currently, hello app supports only user sign-up and sign-in by using **local accounts**; accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="76b07-159">Pomocí Azure AD B2C můžete přidat podporu pro ostatní **zprostředkovatelů identity** (IDPs) beze změny některé z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="76b07-159">By using Azure AD B2C, you can add support for other **identity providers** (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="76b07-160">tooadd sociálních IDPs tooyour aplikace, začněte tím, že následující hello podrobné pokyny v těchto článcích.</span><span class="sxs-lookup"><span data-stu-id="76b07-160">tooadd social IDPs tooyour app, begin by following hello detailed instructions in these articles.</span></span> <span data-ttu-id="76b07-161">Pro každé rozšíření IDP chcete toosupport, potřebujete tooregister aplikace v daném systému a získat ID klienta.</span><span class="sxs-lookup"><span data-stu-id="76b07-161">For each IDP you want toosupport, you need tooregister an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="76b07-162">Nastavení sítě Facebook jako IDP</span><span class="sxs-lookup"><span data-stu-id="76b07-162">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="76b07-163">Nastavit Google jako IDP</span><span class="sxs-lookup"><span data-stu-id="76b07-163">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="76b07-164">Nastavit Amazon jako IDP</span><span class="sxs-lookup"><span data-stu-id="76b07-164">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="76b07-165">Nastavit LinkedIn jako IDP</span><span class="sxs-lookup"><span data-stu-id="76b07-165">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="76b07-166">Po přidání tooyour zprostředkovatelé identity hello adresáři B2C, upravit každý vaší tři zásady tooinclude hello nové IDPs, jak je popsáno v hello [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="76b07-166">After you add hello identity providers tooyour B2C directory, edit each of your three policies tooinclude hello new IDPs, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="76b07-167">Po uložení zásad hello a znovu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76b07-167">After you save your policies, run hello app again.</span></span>  <span data-ttu-id="76b07-168">Měli byste vidět hello přidají nové IDPs jako přihlášení a odhlášení možnosti v každé z vaší činnosti identity.</span><span class="sxs-lookup"><span data-stu-id="76b07-168">You should see hello new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="76b07-169">Můžete experimentovat s vašimi zásadami a sledovat hello vliv na vaši ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76b07-169">You can experiment with your policies and observe hello effect on your sample app.</span></span> <span data-ttu-id="76b07-170">Přidat nebo odebrat IDPs, pracovat s deklarace identity aplikace nebo změnit atributy registrace.</span><span class="sxs-lookup"><span data-stu-id="76b07-170">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="76b07-171">Experiment, dokud se nezobrazí, jak zásady, žádosti o ověření a OWIN tie společně.</span><span class="sxs-lookup"><span data-stu-id="76b07-171">Experiment until you can see how policies, authentication requests, and OWIN tie together.</span></span>

## <a name="sample-code-walkthrough"></a><span data-ttu-id="76b07-172">Ukázka kódu návod</span><span class="sxs-lookup"><span data-stu-id="76b07-172">Sample code walkthrough</span></span>
<span data-ttu-id="76b07-173">Konfigurace kódu aplikace hello ukázka zobrazit Hello následující části.</span><span class="sxs-lookup"><span data-stu-id="76b07-173">hello following sections show you how hello sample application code is configured.</span></span> <span data-ttu-id="76b07-174">Může to použijte jako vodítko při vývoji vaší budoucí aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b07-174">You may use this as a guide in your future app development.</span></span>

### <a name="add-authentication-support"></a><span data-ttu-id="76b07-175">Přidat podporu ověřování</span><span class="sxs-lookup"><span data-stu-id="76b07-175">Add authentication support</span></span>

<span data-ttu-id="76b07-176">Teď můžete konfigurovat toouse vaše aplikace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="76b07-176">You can now configure your app toouse Azure AD B2C.</span></span> <span data-ttu-id="76b07-177">Aplikace komunikuje se službou Azure AD B2C odesláním OpenID Connect žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="76b07-177">Your app communicates with Azure AD B2C by sending OpenID Connect authentication requests.</span></span> <span data-ttu-id="76b07-178">Hello požadavky stanovují, hello uživatelské prostředí aplikace chce tooexecute zadáním hello zásad.</span><span class="sxs-lookup"><span data-stu-id="76b07-178">hello requests dictate hello user experience your app wants tooexecute by specifying hello policy.</span></span> <span data-ttu-id="76b07-179">Můžete použít tyto požadavky společnosti Microsoft OWIN knihovny toosend, provést zásady, spravovat relace uživatele a další.</span><span class="sxs-lookup"><span data-stu-id="76b07-179">You can use Microsoft's OWIN library toosend these requests, execute policies, manage user sessions, and more.</span></span>

#### <a name="install-owin"></a><span data-ttu-id="76b07-180">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="76b07-180">Install OWIN</span></span>

<span data-ttu-id="76b07-181">toobegin, přidejte hello OWIN middleware NuGet balíčky toohello projekt pomocí hello Konzola správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76b07-181">toobegin, add hello OWIN middleware NuGet packages toohello project by using hello Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a><span data-ttu-id="76b07-182">Přidání třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="76b07-182">Add an OWIN startup class</span></span>

<span data-ttu-id="76b07-183">Přidání třídy toohello spuštění OWIN rozhraní API volat `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="76b07-183">Add an OWIN startup class toohello API called `Startup.cs`.</span></span>  <span data-ttu-id="76b07-184">Klikněte pravým tlačítkem na projekt hello, vyberte **přidat** a **nová položka**a poté vyhledejte OWIN.</span><span class="sxs-lookup"><span data-stu-id="76b07-184">Right-click on hello project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="76b07-185">Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b07-185">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="76b07-186">V našem ukázce jsme příliš změnili deklaraci třídy hello`public partial class Startup` a implementovat hello jiných součástí třídy hello v `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="76b07-186">In our sample, we changed hello class declaration too`public partial class Startup` and implemented hello other part of hello class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="76b07-187">Uvnitř hello `Configuration` metoda, jsme přidali volání příliš`ConfigureAuth`, která je definována v `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="76b07-187">Inside hello `Configuration` method, we added a call too`ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="76b07-188">Po hello úpravy `Startup.cs` vypadá jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b07-188">After hello modifications, `Startup.cs` looks like hello following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-hello-authentication-middleware"></a><span data-ttu-id="76b07-189">Nakonfigurujte middleware ověřování hello</span><span class="sxs-lookup"><span data-stu-id="76b07-189">Configure hello authentication middleware</span></span>

<span data-ttu-id="76b07-190">Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="76b07-190">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="76b07-191">Hello parametry, které zadáte v `OpenIdConnectAuthenticationOptions` sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="76b07-191">hello parameters you provide in `OpenIdConnectAuthenticationOptions` serve as coordinates for your app toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="76b07-192">Pokud některé parametry nezadáte, použije hello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="76b07-192">If you do not specify certain parameters, it will use hello default value.</span></span> <span data-ttu-id="76b07-193">Například můžeme nezadávejte hello `ResponseType` v ukázce hello, takže hello výchozí hodnota `code id_token` se použije v každém odchozí požadavek tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="76b07-193">For example, we do not specify hello `ResponseType` in hello sample, so hello default value `code id_token` will be used in each outgoing request tooAzure AD B2C.</span></span>

<span data-ttu-id="76b07-194">Budete také potřebovat tooset až ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="76b07-194">You also need tooset up cookie authentication.</span></span> <span data-ttu-id="76b07-195">middleware OpenID Connect Hello používá soubory cookie toomaintain uživatelské relace, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="76b07-195">hello OpenID Connect middleware uses cookies toomaintain user sessions, among other things.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure hello OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate hello metadata address using hello tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify hello callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify hello claims toovalidate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement hello "Notification" methods...
}
```

<span data-ttu-id="76b07-196">V `OpenIdConnectAuthenticationOptions` vyšší, jsme definovali sadu funkce zpětného volání pro konkrétní oznámení, které jsou přijaty middlewarem OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-196">In `OpenIdConnectAuthenticationOptions` above, we define a set of callback functions for specific notifications that are received by hello OpenID Connect middleware.</span></span> <span data-ttu-id="76b07-197">Tyto chování jsou definovány pomocí `OpenIdConnectAuthenticationNotifications` objektu a uložena do hello `Notifications` proměnné.</span><span class="sxs-lookup"><span data-stu-id="76b07-197">These behaviors are defined using a `OpenIdConnectAuthenticationNotifications` object and stored into hello `Notifications` variable.</span></span> <span data-ttu-id="76b07-198">V našem ukázce jsme definovali tři různé zpětná volání v závislosti na hello událostí.</span><span class="sxs-lookup"><span data-stu-id="76b07-198">In our sample, we define three different callbacks depending on hello event.</span></span>

### <a name="using-different-policies"></a><span data-ttu-id="76b07-199">Pomocí různých zásad</span><span class="sxs-lookup"><span data-stu-id="76b07-199">Using different policies</span></span>

<span data-ttu-id="76b07-200">Hello `RedirectToIdentityProvider` oznámení se aktivuje vždy, když je vytvořena žádost tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="76b07-200">hello `RedirectToIdentityProvider` notification is triggered whenever a request is made tooAzure AD B2C.</span></span> <span data-ttu-id="76b07-201">Ve funkci zpětného volání hello `OnRedirectToIdentityProvider`, jsme se změnami hello odchozí volání, pokud má být toouse jiné zásady.</span><span class="sxs-lookup"><span data-stu-id="76b07-201">In hello callback function `OnRedirectToIdentityProvider`, we check in hello outgoing call if we want toouse a different policy.</span></span> <span data-ttu-id="76b07-202">V pořadí toodo heslo resetovat, nebo upravte profil musíte toouse hello odpovídající zásada, například zásady místo výchozí hello "Registrace nebo přihlášení" zásady resetování hesel hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-202">In order toodo a password reset or edit a profile, you need toouse hello corresponding policy such as hello password reset policy instead of hello default "Sign-up or Sign-in" policy.</span></span>

<span data-ttu-id="76b07-203">Ve výběru když uživatel chce tooreset hello heslo nebo upravte profil hello, přidáme hello zásad jsme dáváte přednost toouse do kontextu OWIN hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-203">In our sample, when a user wants tooreset hello password or edit hello profile, we add hello policy we prefer toouse into hello OWIN context.</span></span> <span data-ttu-id="76b07-204">To můžete udělat pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b07-204">That can be done by doing hello following:</span></span>

```CSharp
    // Let hello middleware know you are trying toouse hello edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

<span data-ttu-id="76b07-205">A můžete implementovat funkce zpětného volání hello `OnRedirectToIdentityProvider` díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b07-205">And you can implement hello callback function `OnRedirectToIdentityProvider` by doing hello following:</span></span>

```CSharp
/*
*  On each call tooAzure AD B2C, check if a policy (e.g. hello profile edit or password reset policy) has been specified in hello OWIN context.
*  If so, use that policy when making hello call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a><span data-ttu-id="76b07-206">Zpracování autorizačních kódů</span><span class="sxs-lookup"><span data-stu-id="76b07-206">Handling authorization codes</span></span>

<span data-ttu-id="76b07-207">Hello `AuthorizationCodeReceived` oznámení se aktivuje při přijetí autorizační kód.</span><span class="sxs-lookup"><span data-stu-id="76b07-207">hello `AuthorizationCodeReceived` notification is triggered when an authorization code is received.</span></span> <span data-ttu-id="76b07-208">middleware OpenID Connect Hello nepodporuje výměnou kódy pro tokeny přístupu.</span><span class="sxs-lookup"><span data-stu-id="76b07-208">hello OpenID Connect middleware does not support exchanging codes for access tokens.</span></span> <span data-ttu-id="76b07-209">Můžete ručně vyměnit hello kód pro token hello ve funkci zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="76b07-209">You can manually exchange hello code for hello token in a callback function.</span></span> <span data-ttu-id="76b07-210">Další informace, podívejte se prosím na hello [dokumentace](active-directory-b2c-devquickstarts-web-api-dotnet.md) to vysvětluje, jak.</span><span class="sxs-lookup"><span data-stu-id="76b07-210">For more information, please look at hello [documentation](active-directory-b2c-devquickstarts-web-api-dotnet.md) that explains how.</span></span>

### <a name="handling-errors"></a><span data-ttu-id="76b07-211">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="76b07-211">Handling errors</span></span>

<span data-ttu-id="76b07-212">Hello `AuthenticationFailed` oznámení se aktivuje, když se ověřování nezdaří.</span><span class="sxs-lookup"><span data-stu-id="76b07-212">hello `AuthenticationFailed` notification is triggered when authentication fails.</span></span> <span data-ttu-id="76b07-213">V jeho metoda zpětného volání může zpracovávat chyby hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="76b07-213">In its callback method, you can handle hello errors as you wish.</span></span> <span data-ttu-id="76b07-214">Měli byste ale přidat kontrolu pro kód chyby hello `AADB2C90118`.</span><span class="sxs-lookup"><span data-stu-id="76b07-214">You should however add a check for hello error code `AADB2C90118`.</span></span> <span data-ttu-id="76b07-215">Během provádění hello hello "Registrace nebo přihlášení" zásady, má uživatel hello hello příležitost tooselect **zapomněli jste heslo?** odkaz.</span><span class="sxs-lookup"><span data-stu-id="76b07-215">During hello execution of hello "Sign-up or Sign-in" policy, hello user has hello opportunity tooselect a **Forgot your password?** link.</span></span> <span data-ttu-id="76b07-216">V takovém případě Azure AD B2C odešle aplikace této chyby kódu oznamující, že vaše aplikace by měl provádět žádost místo toho použít zásady resetování hesel hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-216">In this event, Azure AD B2C sends your app that error code indicating that your app should make a request using hello password reset policy instead.</span></span>

```CSharp
/*
* Catch any failures received by hello authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle hello error code that Azure AD B2C throws when trying tooreset a password from hello login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If hello user clicked hello reset password link, redirect toohello reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-tooazure-ad"></a><span data-ttu-id="76b07-217">Odeslání žádosti o tooAzure ověřování AD</span><span class="sxs-lookup"><span data-stu-id="76b07-217">Send authentication requests tooAzure AD</span></span>

<span data-ttu-id="76b07-218">Aplikace je nyní správně nakonfigurované toocommunicate s Azure AD B2C pomocí ověřovacího protokolu OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-218">Your app is now properly configured toocommunicate with Azure AD B2C by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="76b07-219">OWIN spravuje hello podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD B2C a údržbě uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="76b07-219">OWIN manages hello details of crafting authentication messages, validating tokens from Azure AD B2C, and maintaining user session.</span></span> <span data-ttu-id="76b07-220">Všechno, co zůstane je tooinitiate toku každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="76b07-220">All that remains is tooinitiate each user's flow.</span></span>

<span data-ttu-id="76b07-221">Když uživatel vybere **až přihlášení nebo přihlášení**, **upravit profil**, nebo **resetovat heslo** hello webové aplikace hello přidružené akce je volána v `Controllers\AccountController.cs`:</span><span class="sxs-lookup"><span data-stu-id="76b07-221">When a user selects **Sign up/Sign in**, **Edit profile**, or **Reset password** in hello web app, hello associated action is invoked in `Controllers\AccountController.cs`:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign up or sign in
*/
public void SignUpSignIn()
{
    // Use hello default policy tooprocess hello sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting tooedit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let hello middleware know you are trying toouse hello edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set hello page tooredirect tooafter editing hello profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting tooreset a password
*/
public void ResetPassword()
{
    // Let hello middleware know you are trying toouse hello reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set hello page tooredirect tooafter changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

<span data-ttu-id="76b07-222">Můžete také použít OWIN toosign out hello uživatele z aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="76b07-222">You can also use OWIN toosign out hello user from hello app.</span></span> <span data-ttu-id="76b07-223">V `Controllers\AccountController.cs` máme:</span><span class="sxs-lookup"><span data-stu-id="76b07-223">In `Controllers\AccountController.cs` we have:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign out
*/
public void SignOut()
{
    // toosign out hello user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

<span data-ttu-id="76b07-224">V přidání tooexplicitly vyvolání zásady, můžete použít `[Authorize]` značky v řadičích, které provádí zásadu, pokud hello uživatel není přihlášený.</span><span class="sxs-lookup"><span data-stu-id="76b07-224">In addition tooexplicitly invoking a policy, you can use a `[Authorize]` tag in your controllers that executes a policy if hello user is not signed in.</span></span> <span data-ttu-id="76b07-225">Otevřete `Controllers\HomeController.cs` a přidejte hello `[Authorize]` značky toohello deklarací řadiče.</span><span class="sxs-lookup"><span data-stu-id="76b07-225">Open `Controllers\HomeController.cs` and add hello `[Authorize]` tag toohello claims controller.</span></span>  <span data-ttu-id="76b07-226">OWIN vybere hello poslední zásady nakonfigurované při hello `[Authorize]` je dosáhl značky.</span><span class="sxs-lookup"><span data-stu-id="76b07-226">OWIN selects hello last policy configured when hello `[Authorize]` tag is hit.</span></span>

```CSharp
// Controllers\HomeController.cs

// You can use hello Authorize decorator tooexecute a certain policy if hello user is not already signed into hello app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a><span data-ttu-id="76b07-227">Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="76b07-227">Display user information</span></span>

<span data-ttu-id="76b07-228">Při ověřování uživatele pomocí OpenID Connect, Azure AD B2C vrátí ID tokenu toohello aplikaci, která obsahuje **deklarace identity**.</span><span class="sxs-lookup"><span data-stu-id="76b07-228">When you authenticate users by using OpenID Connect, Azure AD B2C returns an ID token toohello app that contains **claims**.</span></span> <span data-ttu-id="76b07-229">Toto jsou tvrzení o hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="76b07-229">These are assertions about hello user.</span></span> <span data-ttu-id="76b07-230">Můžete použít deklarace identity toopersonalize vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b07-230">You can use claims toopersonalize your app.</span></span>

<span data-ttu-id="76b07-231">Otevřete hello `Controllers\HomeController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="76b07-231">Open hello `Controllers\HomeController.cs` file.</span></span> <span data-ttu-id="76b07-232">Dostanete deklarace identity uživatelů v řadičích prostřednictvím hello `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="76b07-232">You can access user claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

<span data-ttu-id="76b07-233">Dostanete všechny deklarace identity, aby vaše aplikace přijímá v hello stejný způsobem.</span><span class="sxs-lookup"><span data-stu-id="76b07-233">You can access any claim that your application receives in hello same way.</span></span>  <span data-ttu-id="76b07-234">Seznam všech deklarací identity hello hello aplikace obdrží je k dispozici pro vás na hello **deklarace identity** stránky.</span><span class="sxs-lookup"><span data-stu-id="76b07-234">A list of all hello claims hello app receives is available for you on hello **Claims** page.</span></span>
