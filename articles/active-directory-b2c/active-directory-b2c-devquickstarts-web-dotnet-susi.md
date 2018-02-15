---
title: "Resetování hesla pro zápis, ověřování, ASP.NET Azure Active Directory B2C"
description: "Jak vytvořit webovou aplikaci, která má registrace-množství nebo přihlášení, úprava profilu a resetování hesla pomocí Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: ffc46f4348a2ac3cae51c859a24c609756a710fe
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a><span data-ttu-id="9539b-103">Vytvoření webové aplikace ASP.NET s Azure Active Directory B2C profil registrace, přihlášení, úpravy a resetování hesla</span><span class="sxs-lookup"><span data-stu-id="9539b-103">Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset</span></span>

<span data-ttu-id="9539b-104">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="9539b-104">This tutorial shows you how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9539b-105">Přidání funkce Azure AD B2C identity do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9539b-105">Add Azure AD B2C identity features to your web app</span></span>
> * <span data-ttu-id="9539b-106">Registrace vaší webové aplikace ve vašem adresáři Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9539b-106">Register your web app in your Azure AD B2C directory</span></span>
> * <span data-ttu-id="9539b-107">Vytvoření uživatele registrace-množství nebo přihlášení, upravit profil a zásady resetování hesel pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="9539b-107">Create a user sign-up/sign-in, profile edit, and password reset policy for your web app</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9539b-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9539b-108">Prerequisites</span></span>

- <span data-ttu-id="9539b-109">Svého klienta B2C je nutné připojit k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="9539b-109">You must connect your B2C Tenant to an Azure account.</span></span> <span data-ttu-id="9539b-110">Můžete vytvořit bezplatný účet Azure [zde](https://azure.microsoft.com/en-us/).</span><span class="sxs-lookup"><span data-stu-id="9539b-110">You can create a free Azure account [here](https://azure.microsoft.com/en-us/).</span></span>
- <span data-ttu-id="9539b-111">Je třeba [Microsoft Visual Studio](https://www.visualstudio.com/) nebo podobné program zobrazovat a upravovat ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="9539b-111">You need [Microsoft Visual Studio](https://www.visualstudio.com/) or a similar program to view and modify the sample code.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="9539b-112">Vytvoření adresáře Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9539b-112">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="9539b-113">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="9539b-113">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="9539b-114">Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="9539b-114">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="9539b-115">Pokud nemáte již, vytvořte adresář B2C, než budete pokračovat v této příručce.</span><span class="sxs-lookup"><span data-stu-id="9539b-115">If you don't have one already, create a B2C directory before you continue in this guide.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> <span data-ttu-id="9539b-116">Potřebujete připojit k předplatnému Azure klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="9539b-116">You need to connect the B2C Tenant to your Azure subscription.</span></span> <span data-ttu-id="9539b-117">Po výběru **vytvořit**, vyberte **odkaz existující Azure AD B2C klienta pro Moje předplatné** možnost a potom v **klienta Azure AD B2C** rozevírací nabídku, vyberte klienta, které chcete přidružit.</span><span class="sxs-lookup"><span data-stu-id="9539b-117">After selecting **Create**, select the **Link an existing Azure AD B2C Tenant to my Azure subscription** option, and then in the **Azure AD B2C Tenant** drop down, select the tenant you want to associate.</span></span>

## <a name="create-and-register-an-application"></a><span data-ttu-id="9539b-118">Vytvoření a registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="9539b-118">Create and register an application</span></span>

<span data-ttu-id="9539b-119">Dále musíte vytvořit a registrovat aplikace ve svém adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="9539b-119">Next, you need to create and register the app in your B2C directory.</span></span> <span data-ttu-id="9539b-120">To poskytuje informace, které Azure AD B2C potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="9539b-120">This provides information that Azure AD B2C needs to securely communicate with your app.</span></span> 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

<span data-ttu-id="9539b-121">Až skončíte, bude mít rozhraní API a nativní aplikace v nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-121">When you are done, you will have both an API and a native application in your application settings.</span></span>

## <a name="create-policies-on-your-b2c-tenant"></a><span data-ttu-id="9539b-122">Vytvořit zásady na svého klienta B2C</span><span class="sxs-lookup"><span data-stu-id="9539b-122">Create policies on your B2C tenant</span></span>

<span data-ttu-id="9539b-123">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="9539b-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="9539b-124">Tato ukázka kódu obsahuje tři činnosti identity: **registrace a přihlášení**, **profilu upravit**, a **resetování hesla**.</span><span class="sxs-lookup"><span data-stu-id="9539b-124">This code sample contains three identity experiences: **sign-up & sign-in**, **profile edit**, and **password reset**.</span></span>  <span data-ttu-id="9539b-125">Pro každý typ rozhraní musíte vytvořit zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="9539b-125">You need to create one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="9539b-126">U každé zásady nezapomeňte vybrat atribut název zobrazení nebo deklarace identity a poznamenejte název vaší zásady pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="9539b-126">For each policy, be sure to select the Display name attribute or claim, and to copy down the name of your policy for later use.</span></span>

### <a name="add-your-identity-providers"></a><span data-ttu-id="9539b-127">Přidat vaši zprostředkovatelé identity</span><span class="sxs-lookup"><span data-stu-id="9539b-127">Add your identity providers</span></span>

<span data-ttu-id="9539b-128">Nastavení, vyberte **zprostředkovatelů Identity** a zvolte registrace uživatelského jména nebo e-mailu registrace.</span><span class="sxs-lookup"><span data-stu-id="9539b-128">From your settings, select **Identity Providers** and choose Username signup or Email signup.</span></span>

### <a name="create-a-sign-up-and-sign-in-policy"></a><span data-ttu-id="9539b-129">Vytvoření zásad registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="9539b-129">Create a sign-up and sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a><span data-ttu-id="9539b-130">Vytvoření profilu úpravy zásad</span><span class="sxs-lookup"><span data-stu-id="9539b-130">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a><span data-ttu-id="9539b-131">Vytvoření zásady resetování hesel</span><span class="sxs-lookup"><span data-stu-id="9539b-131">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

<span data-ttu-id="9539b-132">Po vytvoření zásad jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-132">After you create your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="9539b-133">Stažení ukázkového kódu</span><span class="sxs-lookup"><span data-stu-id="9539b-133">Download the sample code</span></span>

<span data-ttu-id="9539b-134">Kód k tomuto kurzu se udržuje na [GitHubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="9539b-134">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="9539b-135">Ukázku můžete klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="9539b-135">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="9539b-136">Po stažení ukázkového kódu otevřete soubor Visual Studio .sln, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="9539b-136">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="9539b-137">Soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="9539b-137">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="9539b-138">`TaskWebApp` je webová aplikace MVC, která uživatel komunikuje.</span><span class="sxs-lookup"><span data-stu-id="9539b-138">`TaskWebApp` is the MVC web application that the user interacts with.</span></span> <span data-ttu-id="9539b-139">`TaskService` je back-endové webové rozhraní API aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="9539b-139">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="9539b-140">Tento článek bude probírat pouze aplikaci `TaskWebApp`.</span><span class="sxs-lookup"><span data-stu-id="9539b-140">This article will only discuss the `TaskWebApp` application.</span></span> <span data-ttu-id="9539b-141">Další informace o sestavení `TaskService` pomocí Azure AD B2C, najdete v tématu [naše kurz webové rozhraní api .NET](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="9539b-141">To learn how to build `TaskService` using Azure AD B2C, see [our .NET web api tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="update-code-to-use-your-tenant-and-policies"></a><span data-ttu-id="9539b-142">Aktualizujte kód používat klienta a zásady</span><span class="sxs-lookup"><span data-stu-id="9539b-142">Update code to use your tenant and policies</span></span>

<span data-ttu-id="9539b-143">Naše ukázka je nakonfigurovaná k použití zásad a ID klienta naše ukázkového tenanta.</span><span class="sxs-lookup"><span data-stu-id="9539b-143">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="9539b-144">Aby se připojila k vlastního klienta, budete muset otevřít `web.config` v `TaskWebApp` projektu a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="9539b-144">To connect it to your own tenant, you need to open `web.config` in the `TaskWebApp` project and replace the following values:</span></span>

* <span data-ttu-id="9539b-145">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="9539b-145">`ida:Tenant` with your tenant name</span></span>
* <span data-ttu-id="9539b-146">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="9539b-146">`ida:ClientId` with your web app application ID</span></span>
* <span data-ttu-id="9539b-147">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9539b-147">`ida:ClientSecret` with your web app secret key</span></span>
* <span data-ttu-id="9539b-148">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="9539b-148">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
* <span data-ttu-id="9539b-149">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="9539b-149">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
* <span data-ttu-id="9539b-150">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="9539b-150">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>

## <a name="launch-the-app"></a><span data-ttu-id="9539b-151">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="9539b-151">Launch the app</span></span>
<span data-ttu-id="9539b-152">Z Visual Studia, spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9539b-152">From within Visual Studio, launch the app.</span></span> <span data-ttu-id="9539b-153">Přejděte na kartu seznam úkolů a poznamenejte si, že je adresa URl: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id =*YourclientID*...</span><span class="sxs-lookup"><span data-stu-id="9539b-153">Navigate to the To-Do List tab, and note the URl is: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*&client_id=*YourclientID*.....</span></span>

<span data-ttu-id="9539b-154">Zaregistrujte se pro aplikace pomocí e-mailové adresy nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9539b-154">Sign up for the app by using your email address or user name.</span></span> <span data-ttu-id="9539b-155">Odhlásit se, potom znovu přihlásit a upravte profil nebo resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="9539b-155">Sign out, then sign in again and edit the profile or reset the password.</span></span> <span data-ttu-id="9539b-156">Odhlásit se a přihlaste se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="9539b-156">Sign out and sign in as a different user.</span></span> 

## <a name="add-social-idps"></a><span data-ttu-id="9539b-157">Přidat sociálních IDPs</span><span class="sxs-lookup"><span data-stu-id="9539b-157">Add social IDPs</span></span>

<span data-ttu-id="9539b-158">Aplikace v současné době podporuje pouze uživatel registrace a přihlášení pomocí **místní účty**; uložené v adresáři B2C účty, které používají uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="9539b-158">Currently, the app supports only user sign-up and sign-in by using **local accounts**; accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="9539b-159">Pomocí Azure AD B2C můžete přidat podporu pro ostatní **zprostředkovatelů identity** (IDPs) beze změny některé z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="9539b-159">By using Azure AD B2C, you can add support for other **identity providers** (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="9539b-160">Sociální IDPs přidat do vaší aplikace, začněte tím, že následující podrobné pokyny v těchto článcích.</span><span class="sxs-lookup"><span data-stu-id="9539b-160">To add social IDPs to your app, begin by following the detailed instructions in these articles.</span></span> <span data-ttu-id="9539b-161">Pro každý deklarací identity, které chcete podporovat je třeba zaregistrovat aplikaci v daném systému a získat ID klienta.</span><span class="sxs-lookup"><span data-stu-id="9539b-161">For each IDP you want to support, you need to register an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="9539b-162">Nastavení sítě Facebook jako IDP</span><span class="sxs-lookup"><span data-stu-id="9539b-162">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="9539b-163">Nastavit Google jako IDP</span><span class="sxs-lookup"><span data-stu-id="9539b-163">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="9539b-164">Nastavit Amazon jako IDP</span><span class="sxs-lookup"><span data-stu-id="9539b-164">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="9539b-165">Nastavit LinkedIn jako IDP</span><span class="sxs-lookup"><span data-stu-id="9539b-165">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="9539b-166">Po přidání zprostředkovatelů identity do vašeho adresáře B2C, upravit, každý z vaší tři zásady, které zahrnují nové IDPs, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="9539b-166">After you add the identity providers to your B2C directory, edit each of your three policies to include the new IDPs, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="9539b-167">Po uložení zásad, znovu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9539b-167">After you save your policies, run the app again.</span></span>  <span data-ttu-id="9539b-168">Měli byste vidět nové IDPs přidat jako přihlášení a registrace možnosti v každé z vaší identity činnost.</span><span class="sxs-lookup"><span data-stu-id="9539b-168">You should see the new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="9539b-169">Můžete experimentovat s vašimi zásadami a sledovat účinek na ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-169">You can experiment with your policies and observe the effect on your sample app.</span></span> <span data-ttu-id="9539b-170">Přidat nebo odebrat IDPs, pracovat s deklarace identity aplikace nebo změnit atributy registrace.</span><span class="sxs-lookup"><span data-stu-id="9539b-170">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="9539b-171">Experiment, dokud se nezobrazí, jak zásady, žádosti o ověření a OWIN tie společně.</span><span class="sxs-lookup"><span data-stu-id="9539b-171">Experiment until you can see how policies, authentication requests, and OWIN tie together.</span></span>

## <a name="sample-code-walkthrough"></a><span data-ttu-id="9539b-172">Ukázka kódu návod</span><span class="sxs-lookup"><span data-stu-id="9539b-172">Sample code walkthrough</span></span>
<span data-ttu-id="9539b-173">Následující části vysvětlují, jak jsou nakonfigurované ukázkového kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-173">The following sections show you how the sample application code is configured.</span></span> <span data-ttu-id="9539b-174">Může to použijte jako vodítko při vývoji vaší budoucí aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-174">You may use this as a guide in your future app development.</span></span>

### <a name="add-authentication-support"></a><span data-ttu-id="9539b-175">Přidat podporu ověřování</span><span class="sxs-lookup"><span data-stu-id="9539b-175">Add authentication support</span></span>

<span data-ttu-id="9539b-176">Teď můžete konfigurovat aplikaci používají Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9539b-176">You can now configure your app to use Azure AD B2C.</span></span> <span data-ttu-id="9539b-177">Aplikace komunikuje se službou Azure AD B2C odesláním OpenID Connect žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="9539b-177">Your app communicates with Azure AD B2C by sending OpenID Connect authentication requests.</span></span> <span data-ttu-id="9539b-178">Požadavky určují uživatelské prostředí, které vaše aplikace chce provést zadáním zásady.</span><span class="sxs-lookup"><span data-stu-id="9539b-178">The requests dictate the user experience your app wants to execute by specifying the policy.</span></span> <span data-ttu-id="9539b-179">Knihovna OWIN společnosti Microsoft můžete odesílat tyto požadavky, provést zásady a spravovat uživatelské relace a další.</span><span class="sxs-lookup"><span data-stu-id="9539b-179">You can use Microsoft's OWIN library to send these requests, execute policies, manage user sessions, and more.</span></span>

#### <a name="install-owin"></a><span data-ttu-id="9539b-180">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="9539b-180">Install OWIN</span></span>

<span data-ttu-id="9539b-181">Pokud chcete začít, přidejte do projektu balíčky NuGet middleware OWIN pomocí konzoly Správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9539b-181">To begin, add the OWIN middleware NuGet packages to the project by using the Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a><span data-ttu-id="9539b-182">Přidání třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="9539b-182">Add an OWIN startup class</span></span>

<span data-ttu-id="9539b-183">Přidejte třídu pro spuštění OWIN s názvem `Startup.cs` do rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9539b-183">Add an OWIN startup class to the API called `Startup.cs`.</span></span>  <span data-ttu-id="9539b-184">Klikněte pravým tlačítkem myši na projekt, vyberte **Přidat** a **Nová položka**, a poté vyhledejte OWIN.</span><span class="sxs-lookup"><span data-stu-id="9539b-184">Right-click on the project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="9539b-185">Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(…)`.</span><span class="sxs-lookup"><span data-stu-id="9539b-185">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="9539b-186">V naší ukázce jsme změnili deklaraci třídy na `public partial class Startup` a implementovali druhou část třídy v `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="9539b-186">In our sample, we changed the class declaration to `public partial class Startup` and implemented the other part of the class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="9539b-187">Uvnitř metody `Configuration` jsme přidali volání metody `ConfigureAuth`, která je definovaná v `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="9539b-187">Inside the `Configuration` method, we added a call to `ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="9539b-188">Po úpravách vypadá `Startup.cs` následovně:</span><span class="sxs-lookup"><span data-stu-id="9539b-188">After the modifications, `Startup.cs` looks like the following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-the-authentication-middleware"></a><span data-ttu-id="9539b-189">Nakonfigurujte middleware ověřování.</span><span class="sxs-lookup"><span data-stu-id="9539b-189">Configure the authentication middleware</span></span>

<span data-ttu-id="9539b-190">Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="9539b-190">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="9539b-191">Parametry, zadejte v `OpenIdConnectAuthenticationOptions` sloužit jako souřadnice pro vaši aplikaci ke komunikaci s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9539b-191">The parameters you provide in `OpenIdConnectAuthenticationOptions` serve as coordinates for your app to communicate with Azure AD B2C.</span></span> <span data-ttu-id="9539b-192">Pokud některé parametry nezadáte, použije se výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="9539b-192">If you do not specify certain parameters, it will use the default value.</span></span> <span data-ttu-id="9539b-193">Například můžeme nezadávejte `ResponseType` v ukázce proto výchozí hodnota `code id_token` se použije v každém odchozí požadavku na Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9539b-193">For example, we do not specify the `ResponseType` in the sample, so the default value `code id_token` will be used in each outgoing request to Azure AD B2C.</span></span>

<span data-ttu-id="9539b-194">Musíte taky nastavit ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="9539b-194">You also need to set up cookie authentication.</span></span> <span data-ttu-id="9539b-195">Middleware OpenID Connect používá soubory cookie k udržování uživatelské relace, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="9539b-195">The OpenID Connect middleware uses cookies to maintain user sessions, among other things.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure the OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate the metadata address using the tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify the callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify the claims to validate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement the "Notification" methods...
}
```

<span data-ttu-id="9539b-196">V `OpenIdConnectAuthenticationOptions` vyšší, jsme definovali sadu funkce zpětného volání pro konkrétní oznámení, které jsou přijaty middlewarem OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="9539b-196">In `OpenIdConnectAuthenticationOptions` above, we define a set of callback functions for specific notifications that are received by the OpenID Connect middleware.</span></span> <span data-ttu-id="9539b-197">Tyto chování jsou definovány pomocí `OpenIdConnectAuthenticationNotifications` objektu a uložena do `Notifications` proměnné.</span><span class="sxs-lookup"><span data-stu-id="9539b-197">These behaviors are defined using a `OpenIdConnectAuthenticationNotifications` object and stored into the `Notifications` variable.</span></span> <span data-ttu-id="9539b-198">V našem ukázce jsme definovali tři různé zpětná volání v závislosti na události.</span><span class="sxs-lookup"><span data-stu-id="9539b-198">In our sample, we define three different callbacks depending on the event.</span></span>

### <a name="using-different-policies"></a><span data-ttu-id="9539b-199">Pomocí různých zásad</span><span class="sxs-lookup"><span data-stu-id="9539b-199">Using different policies</span></span>

<span data-ttu-id="9539b-200">`RedirectToIdentityProvider` Oznámení se aktivuje vždy, když požadavku na službu Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9539b-200">The `RedirectToIdentityProvider` notification is triggered whenever a request is made to Azure AD B2C.</span></span> <span data-ttu-id="9539b-201">Ve funkci zpětného volání `OnRedirectToIdentityProvider`, jsme změnami odchozí volání, pokud chcete použít jinou zásadu.</span><span class="sxs-lookup"><span data-stu-id="9539b-201">In the callback function `OnRedirectToIdentityProvider`, we check in the outgoing call if we want to use a different policy.</span></span> <span data-ttu-id="9539b-202">Aby bylo možné provést resetování hesla nebo upravte profil, budete muset použít odpovídající zásada, jako je heslo resetovat zásady místo výchozí zásady "Registrace nebo přihlášení".</span><span class="sxs-lookup"><span data-stu-id="9539b-202">In order to do a password reset or edit a profile, you need to use the corresponding policy such as the password reset policy instead of the default "Sign-up or Sign-in" policy.</span></span>

<span data-ttu-id="9539b-203">Ve výběru když uživatel chce resetovat heslo nebo upravte profil, přidáme na zásadu, kterou jsme dávají přednost používání do kontextu OWIN.</span><span class="sxs-lookup"><span data-stu-id="9539b-203">In our sample, when a user wants to reset the password or edit the profile, we add the policy we prefer to use into the OWIN context.</span></span> <span data-ttu-id="9539b-204">To můžete udělat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9539b-204">That can be done by doing the following:</span></span>

```CSharp
    // Let the middleware know you are trying to use the edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

<span data-ttu-id="9539b-205">A můžete implementovat funkce zpětného volání `OnRedirectToIdentityProvider` provedením následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="9539b-205">And you can implement the callback function `OnRedirectToIdentityProvider` by doing the following:</span></span>

```CSharp
/*
*  On each call to Azure AD B2C, check if a policy (e.g. the profile edit or password reset policy) has been specified in the OWIN context.
*  If so, use that policy when making the call. Also, don't request a code (since it won't be needed).
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

### <a name="handling-authorization-codes"></a><span data-ttu-id="9539b-206">Zpracování autorizačních kódů</span><span class="sxs-lookup"><span data-stu-id="9539b-206">Handling authorization codes</span></span>

<span data-ttu-id="9539b-207">`AuthorizationCodeReceived` Oznámení se aktivuje při přijetí autorizační kód.</span><span class="sxs-lookup"><span data-stu-id="9539b-207">The `AuthorizationCodeReceived` notification is triggered when an authorization code is received.</span></span> <span data-ttu-id="9539b-208">Middleware OpenID Connect nepodporuje výměnou kódy pro tokeny přístupu.</span><span class="sxs-lookup"><span data-stu-id="9539b-208">The OpenID Connect middleware does not support exchanging codes for access tokens.</span></span> <span data-ttu-id="9539b-209">Kód pro token ve funkci zpětného volání, můžete je ručně vyměnit.</span><span class="sxs-lookup"><span data-stu-id="9539b-209">You can manually exchange the code for the token in a callback function.</span></span> <span data-ttu-id="9539b-210">Další informace, podívejte se prosím na [dokumentace](active-directory-b2c-devquickstarts-web-api-dotnet.md) to vysvětluje, jak.</span><span class="sxs-lookup"><span data-stu-id="9539b-210">For more information, please look at the [documentation](active-directory-b2c-devquickstarts-web-api-dotnet.md) that explains how.</span></span>

### <a name="handling-errors"></a><span data-ttu-id="9539b-211">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="9539b-211">Handling errors</span></span>

<span data-ttu-id="9539b-212">`AuthenticationFailed` Oznámení se aktivuje, když se ověřování nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9539b-212">The `AuthenticationFailed` notification is triggered when authentication fails.</span></span> <span data-ttu-id="9539b-213">V jeho metoda zpětného volání může zpracovávat chyby podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9539b-213">In its callback method, you can handle the errors as you wish.</span></span> <span data-ttu-id="9539b-214">Měli byste ale přidat kontrolu pro kód chyby `AADB2C90118`.</span><span class="sxs-lookup"><span data-stu-id="9539b-214">You should however add a check for the error code `AADB2C90118`.</span></span> <span data-ttu-id="9539b-215">Během provádění zásady "Registrace nebo přihlášení", uživatel má možnost vybrat **zapomněli jste heslo?** odkaz.</span><span class="sxs-lookup"><span data-stu-id="9539b-215">During the execution of the "Sign-up or Sign-in" policy, the user has the opportunity to select a **Forgot your password?** link.</span></span> <span data-ttu-id="9539b-216">V takovém případě Azure AD B2C odešle aplikace této chyby kódu oznamující, že vaše aplikace by měl provádět žádost místo toho použít zásady resetování hesel.</span><span class="sxs-lookup"><span data-stu-id="9539b-216">In this event, Azure AD B2C sends your app that error code indicating that your app should make a request using the password reset policy instead.</span></span>

```CSharp
/*
* Catch any failures received by the authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle the error code that Azure AD B2C throws when trying to reset a password from the login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
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

### <a name="send-authentication-requests-to-azure-ad"></a><span data-ttu-id="9539b-217">Odeslání žádosti o ověření do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9539b-217">Send authentication requests to Azure AD</span></span>

<span data-ttu-id="9539b-218">Aplikace je nyní správně nakonfigurováno pro komunikaci s Azure AD B2C pomocí ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="9539b-218">Your app is now properly configured to communicate with Azure AD B2C by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="9539b-219">OWIN spravuje podrobnosti věnujte zpráv ověřování, ověřování tokenů z Azure AD B2C a údržbě uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="9539b-219">OWIN manages the details of crafting authentication messages, validating tokens from Azure AD B2C, and maintaining user session.</span></span> <span data-ttu-id="9539b-220">Zbývá zahájíte toku každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="9539b-220">All that remains is to initiate each user's flow.</span></span>

<span data-ttu-id="9539b-221">Když uživatel vybere **až přihlášení nebo přihlášení**, **upravit profil**, nebo **resetovat heslo** ve webové aplikaci, přidružené akce je volána v `Controllers\AccountController.cs`:</span><span class="sxs-lookup"><span data-stu-id="9539b-221">When a user selects **Sign up/Sign in**, **Edit profile**, or **Reset password** in the web app, the associated action is invoked in `Controllers\AccountController.cs`:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign up or sign in
*/
public void SignUpSignIn()
{
    // Use the default policy to process the sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting to edit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let the middleware know you are trying to use the edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set the page to redirect to after editing the profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting to reset a password
*/
public void ResetPassword()
{
    // Let the middleware know you are trying to use the reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set the page to redirect to after changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

<span data-ttu-id="9539b-222">OWIN můžete také použít k odhlášení uživatele z aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-222">You can also use OWIN to sign out the user from the app.</span></span> <span data-ttu-id="9539b-223">V `Controllers\AccountController.cs` máme:</span><span class="sxs-lookup"><span data-stu-id="9539b-223">In `Controllers\AccountController.cs` we have:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign out
*/
public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

<span data-ttu-id="9539b-224">Kromě explicitně vyvolání zásady, můžete použít `[Authorize]` značky v řadičích, které provádí zásadu, pokud uživatel není přihlášený.</span><span class="sxs-lookup"><span data-stu-id="9539b-224">In addition to explicitly invoking a policy, you can use a `[Authorize]` tag in your controllers that executes a policy if the user is not signed in.</span></span> <span data-ttu-id="9539b-225">Otevřete `Controllers\HomeController.cs` a přidejte `[Authorize]` značky k řadiči deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="9539b-225">Open `Controllers\HomeController.cs` and add the `[Authorize]` tag to the claims controller.</span></span>  <span data-ttu-id="9539b-226">Vybere OWIN poslední zásady nakonfigurované při zpracování `[Authorize]` je dosáhl značky.</span><span class="sxs-lookup"><span data-stu-id="9539b-226">OWIN selects the last policy configured when the `[Authorize]` tag is hit.</span></span>

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a><span data-ttu-id="9539b-227">Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="9539b-227">Display user information</span></span>

<span data-ttu-id="9539b-228">Při ověřování uživatele pomocí OpenID Connect, Azure AD B2C vrátí ID token na aplikaci, která obsahuje **deklarace identity**.</span><span class="sxs-lookup"><span data-stu-id="9539b-228">When you authenticate users by using OpenID Connect, Azure AD B2C returns an ID token to the app that contains **claims**.</span></span> <span data-ttu-id="9539b-229">Tyto jsou tvrzení o uživateli.</span><span class="sxs-lookup"><span data-stu-id="9539b-229">These are assertions about the user.</span></span> <span data-ttu-id="9539b-230">Deklarace identity můžete použít k přizpůsobení funkcí aplikace.</span><span class="sxs-lookup"><span data-stu-id="9539b-230">You can use claims to personalize your app.</span></span>

<span data-ttu-id="9539b-231">Otevřete soubor `Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="9539b-231">Open the `Controllers\HomeController.cs` file.</span></span> <span data-ttu-id="9539b-232">Dostanete deklarace identity uživatelů v řadičích prostřednictvím `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9539b-232">You can access user claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

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

<span data-ttu-id="9539b-233">Všechny deklarace identity, které aplikace přijímá stejným způsobem můžete přistupovat.</span><span class="sxs-lookup"><span data-stu-id="9539b-233">You can access any claim that your application receives in the same way.</span></span>  <span data-ttu-id="9539b-234">Seznam všech deklarací identity aplikace obdrží je k dispozici pro vás na **deklarace identity** stránky.</span><span class="sxs-lookup"><span data-stu-id="9539b-234">A list of all the claims the app receives is available for you on the **Claims** page.</span></span>
