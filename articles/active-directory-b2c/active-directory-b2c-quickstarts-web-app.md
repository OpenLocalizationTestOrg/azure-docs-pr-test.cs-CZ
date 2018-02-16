---
title: "Test jednotky webové aplikace Azure AD B2C povoleno | Microsoft Docs"
description: "Test jednotky přihlášení, registrace, upravit profil a resetovat heslo uživatele cesty pomocí prostředí testovací Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: patricka
ms.openlocfilehash: bc56da695145f396a2899fb9dc7add3af9a549e8
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="test-drive-an-azure-ad-b2c-enabled-web-app"></a><span data-ttu-id="a0720-103">Webové aplikace s povoleným test jednotky Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="a0720-103">Test drive an Azure AD B2C enabled web app</span></span>

<span data-ttu-id="a0720-104">Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné.</span><span class="sxs-lookup"><span data-stu-id="a0720-104">Azure Active Directory B2C provides cloud identity management to keep your application, business, and customers protected.</span></span> <span data-ttu-id="a0720-105">Tento rychlý start používá ukázkovou aplikaci seznamu úkolů k předvedení:</span><span class="sxs-lookup"><span data-stu-id="a0720-105">This quickstart uses a sample to-do list app to demonstrate:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0720-106">Přihlaste se pomocí vlastní přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="a0720-106">Sign-in with a custom login page.</span></span>
> * <span data-ttu-id="a0720-107">Přihlášení pomocí zprostředkovatele identity sociálních.</span><span class="sxs-lookup"><span data-stu-id="a0720-107">Sign-in using a social identity provider.</span></span>
> * <span data-ttu-id="a0720-108">Vytváření a správě vašeho profilu účtu a uživatele Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a0720-108">Creating and managing your Azure AD B2C account and user profile.</span></span>
> * <span data-ttu-id="a0720-109">Volání webového rozhraní API, které jsou zabezpečené službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a0720-109">Calling a web API secured by Azure AD B2C.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0720-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a0720-110">Prerequisites</span></span>

* <span data-ttu-id="a0720-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) s **ASP.NET a webové vývoj** zatížení.</span><span class="sxs-lookup"><span data-stu-id="a0720-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload.</span></span> 
* <span data-ttu-id="a0720-112">Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru.</span><span class="sxs-lookup"><span data-stu-id="a0720-112">A social account from either Facebook, Google, Microsoft, or Twitter.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="a0720-113">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="a0720-113">Download the sample</span></span>

<span data-ttu-id="a0720-114">[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="a0720-114">[Download or clone the sample application](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) from GitHub.</span></span>

## <a name="run-the-app-in-visual-studio"></a><span data-ttu-id="a0720-115">Spusťte aplikaci v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0720-115">Run the app in Visual Studio</span></span>

<span data-ttu-id="a0720-116">Ve složce projektu ukázkové aplikace otevřete `B2C-WebAPI-DotNet.sln` řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0720-116">In the sample application project folder, open the `B2C-WebAPI-DotNet.sln` solution in Visual Studio.</span></span> 

<span data-ttu-id="a0720-117">Řešení je ukázková aplikace seznamu úkolů skládající se ze dvou projektů:</span><span class="sxs-lookup"><span data-stu-id="a0720-117">The solution is a sample to-do list application consisting of two projects:</span></span>

* <span data-ttu-id="a0720-118">**TaskWebApp** – ASP.NET MVC webové aplikace, kde uživatel může spravovat jejich položky seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="a0720-118">**TaskWebApp** – An ASP.NET MVC web application where a user can manage their to-do list items.</span></span>  
* <span data-ttu-id="a0720-119">**TaskService** – rozhraní ASP.NET Web API back-end, který spravuje operace provést na položky seznamu úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0720-119">**TaskService** – An ASP.NET Web API backend that manages operations performed on a user's To-do list items.</span></span> <span data-ttu-id="a0720-120">Webová aplikace volání této webové rozhraní API a zobrazí výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0720-120">The web app calls this web API and displays the results.</span></span>

<span data-ttu-id="a0720-121">Pro tento rychlý start, budete muset spustit i `TaskWebApp` a `TaskService` projekty ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="a0720-121">For this quickstart, you need to run both the `TaskWebApp` and `TaskService` projects at the same time.</span></span> 

1. <span data-ttu-id="a0720-122">V nabídce sady Visual Studio, vyberte **projekty > nastavit projekty po spuštění...** .</span><span class="sxs-lookup"><span data-stu-id="a0720-122">In the Visual Studio menu, select **Projects > Set StartUp Projects...**.</span></span> 
2. <span data-ttu-id="a0720-123">Vyberte **více projektů po spuštění** přepínač.</span><span class="sxs-lookup"><span data-stu-id="a0720-123">Select **Multiple startup projects** radio button.</span></span>
3. <span data-ttu-id="a0720-124">Změna **akce** pro oba projekty tak, aby **spustit**.</span><span class="sxs-lookup"><span data-stu-id="a0720-124">Change the **Action** for both projects to **Start**.</span></span> <span data-ttu-id="a0720-125">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0720-125">Click **OK**.</span></span>

![Nastavit spouštěcí stránku v sadě Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

<span data-ttu-id="a0720-127">Vyberte **ladění > Spustit ladění** sestavení a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0720-127">Select **Debug > Start Debugging** to build and run both applications.</span></span> <span data-ttu-id="a0720-128">Každá aplikace se otevře v jeho vlastní kartu prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="a0720-128">Each application opens in its own browser tab:</span></span>

<span data-ttu-id="a0720-129">`https://localhost:44316/` – Tato stránka je webová aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0720-129">`https://localhost:44316/` - This page is the ASP.NET web application.</span></span> <span data-ttu-id="a0720-130">Můžete pracovat přímo pomocí této aplikace v rychlý start.</span><span class="sxs-lookup"><span data-stu-id="a0720-130">You interact directly with this application in the quickstart.</span></span>
<span data-ttu-id="a0720-131">`https://localhost:44332/` – Tato stránka je webové rozhraní API, která volá webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0720-131">`https://localhost:44332/` - This page is the web API that is called by the ASP.NET web application.</span></span>

## <a name="create-an-account"></a><span data-ttu-id="a0720-132">Vytvořit účet</span><span class="sxs-lookup"><span data-stu-id="a0720-132">Create an account</span></span>

<span data-ttu-id="a0720-133">Klikněte na tlačítko **zaregistrovat / přihlásit** odkaz ve webové aplikaci ASP.NET spustit **zaregistrovat nebo přihlásit** pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a0720-133">Click the **Sign up / Sign in** link in the ASP.NET web application to start the **Sign Up or Sign In** workflow.</span></span> <span data-ttu-id="a0720-134">Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="a0720-134">When creating an account, you can use an existing social identity provider account or an email account.</span></span> <span data-ttu-id="a0720-135">Pro tento rychlý start používejte účet sociálních identity zprostředkovatele ze sítě Facebook, Google, Microsoft nebo Twitteru.</span><span class="sxs-lookup"><span data-stu-id="a0720-135">For this quickstart, use a social identity provider account from either Facebook, Google, Microsoft, or Twitter.</span></span>

![Ukázka ASP.NET webové aplikace](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a><span data-ttu-id="a0720-137">Zaregistrujte si pomocí zprostředkovatele identity sociálních</span><span class="sxs-lookup"><span data-stu-id="a0720-137">Sign up using a social identity provider</span></span>

<span data-ttu-id="a0720-138">Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a0720-138">To sign up using a social identity provider, click the button of the identity provider you want to use.</span></span> 

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

<span data-ttu-id="a0720-140">Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit).</span><span class="sxs-lookup"><span data-stu-id="a0720-140">You need to authenticate (sign-in) using your social account credentials and authorize the application to read information from your social account.</span></span> <span data-ttu-id="a0720-141">Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města.</span><span class="sxs-lookup"><span data-stu-id="a0720-141">By granting access, the application can retrieve profile information from the social account such as your name and city.</span></span> 

<span data-ttu-id="a0720-142">Dokončete proces přihlášení pro zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="a0720-142">Finish the sign-in process for the identity provider.</span></span> <span data-ttu-id="a0720-143">Například pokud jste zvolili Twitter, zadejte své přihlašovací údaje služby Twitter a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="a0720-143">For example, if you chose Twitter, enter your Twitter credentials and click **Sign in**.</span></span>

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

<span data-ttu-id="a0720-145">Jsou předem vyplněny informace z vašeho účtu sociálních nové Podrobnosti profilu účtu Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a0720-145">Your new Azure AD B2C account profile details are pre-populated with information from your social account.</span></span>

<span data-ttu-id="a0720-146">Aktualizujte pole Zobrazovaný název, funkce a města a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="a0720-146">Update the Display Name, Job Title, and City fields and click **Continue**.</span></span>  <span data-ttu-id="a0720-147">Hodnoty, které zadáte, se používají pro svůj profil účtu uživatele Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a0720-147">The values you enter are used for your Azure AD B2C user account profile.</span></span>

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

<span data-ttu-id="a0720-149">Máte úspěšně:</span><span class="sxs-lookup"><span data-stu-id="a0720-149">You have successfully:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0720-150">Ověřit pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="a0720-150">Authenticated using an identity provider.</span></span>
> * <span data-ttu-id="a0720-151">Vytvořit uživatelský účet Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a0720-151">Created an Azure AD B2C user account.</span></span> 

## <a name="edit-your-profile"></a><span data-ttu-id="a0720-152">Úpravy profilu</span><span class="sxs-lookup"><span data-stu-id="a0720-152">Edit your profile</span></span>

<span data-ttu-id="a0720-153">Azure Active Directory B2C poskytuje funkce, aby uživatelé mohli aktualizaci jejich profilů.</span><span class="sxs-lookup"><span data-stu-id="a0720-153">Azure Active Directory B2C provides functionality to allow users to update their profiles.</span></span> <span data-ttu-id="a0720-154">V panelu nabídek webové aplikace klikněte na název profilu a vyberte **upravit profil** upravit profil, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a0720-154">In the web application menu bar, click your profile name and select **Edit profile** to edit the profile you created.</span></span>

![Úprava profilu](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

<span data-ttu-id="a0720-156">Změna vaše **zobrazovaný název** a **města**.</span><span class="sxs-lookup"><span data-stu-id="a0720-156">Change your **Display name** and **City**.</span></span>  <span data-ttu-id="a0720-157">Klikněte na tlačítko **pokračovat** pro váš profil aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a0720-157">Click **Continue** to update your profile.</span></span>

![Aktualizovat profil](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

<span data-ttu-id="a0720-159">Všimněte si, že název zobrazovaný v pravé horní části stránky se zobrazí aktualizovaná název.</span><span class="sxs-lookup"><span data-stu-id="a0720-159">Notice your display name in the upper right portion of the page shows the updated name.</span></span> 

## <a name="access-a-secured-web-api-resource"></a><span data-ttu-id="a0720-160">Přístup k zabezpečeným webové rozhraní API prostředků</span><span class="sxs-lookup"><span data-stu-id="a0720-160">Access a secured web API resource</span></span>

<span data-ttu-id="a0720-161">Klikněte na tlačítko **seznam úkolů** zadat a upravit položky seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="a0720-161">Click **To-Do List** to enter and modify your to-do list items.</span></span> <span data-ttu-id="a0720-162">Webové aplikace ASP.NET obsahuje přístupový token v požadavku na webové rozhraní API prostředků požadavku na oprávnění k provádění operací na položky seznamu úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0720-162">The ASP.NET web application includes an access token in the request to the web API resource requesting permission to perform operations on the user's to-do list items.</span></span> 

<span data-ttu-id="a0720-163">Zadejte text v **nová položka** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a0720-163">Enter text in the **New Item** text box.</span></span> <span data-ttu-id="a0720-164">Klikněte na tlačítko **přidat** volat Azure AD B2C zabezpečené webového rozhraní API, která přidá položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="a0720-164">Click **Add** to call the Azure AD B2C secured web API that adds a to-do list item.</span></span>

![Přidat položku seznamu úkolů](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

<span data-ttu-id="a0720-166">Úspěšně jste použili účtu uživatele Azure AD B2C Chcete-li provést autorizovaným volání webového rozhraní API Azure AD B2C zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="a0720-166">You have successfully used your Azure AD B2C user account to make an authorized call an Azure AD B2C secured web API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0720-167">Další postup</span><span class="sxs-lookup"><span data-stu-id="a0720-167">Next steps</span></span>

<span data-ttu-id="a0720-168">Ukázka používá v tento rychlý start slouží k akci scénáře Azure AD B2C, včetně:</span><span class="sxs-lookup"><span data-stu-id="a0720-168">The sample used in this quickstart can be used to try other Azure AD B2C scenarios including:</span></span>

* <span data-ttu-id="a0720-169">Vytvoření nové místní účet pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="a0720-169">Creating a new local account using an email address.</span></span>
* <span data-ttu-id="a0720-170">Resetování hesla místní účet.</span><span class="sxs-lookup"><span data-stu-id="a0720-170">Resetting your local account password.</span></span>

<span data-ttu-id="a0720-171">Pokud jste připraveni pustíte do vytvoření vlastního klienta Azure AD B2C a nakonfigurovat vzorku, který se spustí pomocí vlastního klienta, vyzkoušejte následující kurzu.</span><span class="sxs-lookup"><span data-stu-id="a0720-171">If you're ready to delve into creating your own Azure AD B2C tenant and configure the sample to run using your own tenant, try out the following tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="a0720-172">Vytvoření webové aplikace ASP.NET s Azure Active Directory B2C profil registrace, přihlášení, úpravy a resetování hesla</span><span class="sxs-lookup"><span data-stu-id="a0720-172">[Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span></span>