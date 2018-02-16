---
title: Test jednotky plochy aplikace Azure AD B2C | Microsoft Docs
description: "Test jednotky přihlášení, registrace, upravit profil a resetovat heslo uživatele cesty pomocí prostředí testovací Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 86293627-26fb-4e96-a76b-f263f9a945bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 51f5643f0bd975beb939c2d5a8853810fb609ec9
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="test-drive-a-desktop-application-configured-with-azure-ad-b2c"></a><span data-ttu-id="24835-103">Vyzkoušejte desktopová aplikace nakonfigurované v Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="24835-103">Test drive a desktop application configured with Azure AD B2C</span></span>

<span data-ttu-id="24835-104">Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné.</span><span class="sxs-lookup"><span data-stu-id="24835-104">Azure Active Directory B2C provides cloud identity management to keep your application, business, and customers protected.</span></span>  <span data-ttu-id="24835-105">Tento rychlý start používá ukázkovou aplikaci plochy Windows Presentation Foundation (WPF) za účelem ukázky:</span><span class="sxs-lookup"><span data-stu-id="24835-105">This quickstart uses a sample Windows Presentation Foundation (WPF) desktop app to demonstrate:</span></span>

* <span data-ttu-id="24835-106">Pomocí **zaregistrovat nebo přihlásit** zásad k vytvoření nebo Přihlaste se pomocí zprostředkovatele identity sociálních nebo místní účet pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="24835-106">Using the **Sign Up or Sign In** policy to create or sign in with a social identity provider or a local account using an email address.</span></span> 
* <span data-ttu-id="24835-107">**Volání rozhraní API** načíst zobrazovaný název ze Azure AD B2C zabezpečené prostředků.</span><span class="sxs-lookup"><span data-stu-id="24835-107">**Calling an API** to retrieve your display name from an Azure AD B2C secured resource.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24835-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="24835-108">Prerequisites</span></span>

* <span data-ttu-id="24835-109">Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:</span><span class="sxs-lookup"><span data-stu-id="24835-109">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with the following workloads:</span></span>
    - <span data-ttu-id="24835-110">**Vývoj aplikací rozhraní .NET**</span><span class="sxs-lookup"><span data-stu-id="24835-110">**.NET desktop development**</span></span>

* <span data-ttu-id="24835-111">Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru.</span><span class="sxs-lookup"><span data-stu-id="24835-111">A social account from either Facebook, Google, Microsoft, or Twitter.</span></span> <span data-ttu-id="24835-112">Pokud nemáte účet sociálních, je třeba zadat platnou e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="24835-112">If you don't have a social account, a valid email address is required.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="24835-113">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="24835-113">Download the sample</span></span>

<span data-ttu-id="24835-114">[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="24835-114">[Download or clone the sample application](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) from GitHub.</span></span>

## <a name="run-the-app-in-visual-studio"></a><span data-ttu-id="24835-115">Spusťte aplikaci v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24835-115">Run the app in Visual Studio</span></span>

<span data-ttu-id="24835-116">Ve složce projektu ukázkové aplikace otevřete `active-directory-b2c-wpf.sln` řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24835-116">In the sample application project folder, open the `active-directory-b2c-wpf.sln` solution in Visual Studio.</span></span> 

<span data-ttu-id="24835-117">Vyberte **ladění > Spustit ladění** sestavení a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="24835-117">Select **Debug > Start Debugging** to build and run the application.</span></span> 

## <a name="create-an-account"></a><span data-ttu-id="24835-118">Vytvořit účet</span><span class="sxs-lookup"><span data-stu-id="24835-118">Create an account</span></span>

<span data-ttu-id="24835-119">Klikněte na tlačítko **přihlášení** spustit **zaregistrovat nebo přihlásit** pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="24835-119">Click **Sign in** to start the **Sign Up or Sign In** workflow.</span></span> <span data-ttu-id="24835-120">Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="24835-120">When creating an account, you can use an existing social identity provider account or an email account.</span></span>

![Ukázková aplikace](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

### <a name="sign-up-using-a-social-identity-provider"></a><span data-ttu-id="24835-122">Zaregistrujte si pomocí zprostředkovatele identity sociálních</span><span class="sxs-lookup"><span data-stu-id="24835-122">Sign up using a social identity provider</span></span>

<span data-ttu-id="24835-123">Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="24835-123">To sign up using a social identity provider, click the button of the identity provider you want to use.</span></span> <span data-ttu-id="24835-124">Pokud byste radši chtěli použít e-mailovou adresu, přejít na [zaregistrovat pomocí e-mailovou adresu](#sign-up-using-an-email-address) části.</span><span class="sxs-lookup"><span data-stu-id="24835-124">If you prefer to use an email address, jump to the [Sign up using an email address](#sign-up-using-an-email-address) section.</span></span>

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

<span data-ttu-id="24835-126">Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit).</span><span class="sxs-lookup"><span data-stu-id="24835-126">You need to authenticate (sign-in) using your social account credentials and authorize the application to read information from your social account.</span></span> <span data-ttu-id="24835-127">Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města.</span><span class="sxs-lookup"><span data-stu-id="24835-127">By granting access, the application can retrieve profile information from the social account such as your name and city.</span></span> 

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

<span data-ttu-id="24835-129">Vaše nové podrobnosti účtu profilu jsou předem vyplněny informace z vašeho sociálních účtu.</span><span class="sxs-lookup"><span data-stu-id="24835-129">Your new account profile details are pre-populated with information from your social account.</span></span> <span data-ttu-id="24835-130">Upravit podrobnosti, pokud chcete a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="24835-130">Modify the details if you wish and click **Continue**.</span></span>

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

<span data-ttu-id="24835-132">Úspěšně jste vytvořili nový uživatelský účet Azure AD B2C, který používá zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="24835-132">You have successfully created a new Azure AD B2C user account that uses an identity provider.</span></span> <span data-ttu-id="24835-133">Po přihlášení se přístupový token se zobrazí v *Token informace* textové pole.</span><span class="sxs-lookup"><span data-stu-id="24835-133">After sign-in, the access token is shown in the *Token info* text box.</span></span> <span data-ttu-id="24835-134">Přístupový token se používá při přístupu k prostředku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="24835-134">The access token is used when accessing the API resource.</span></span>

![Autorizační token](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

<span data-ttu-id="24835-136">Další krok: [přejít na upravit svůj profil](#edit-your-profile) části.</span><span class="sxs-lookup"><span data-stu-id="24835-136">Next step: [Jump to Edit your profile](#edit-your-profile) section.</span></span>

### <a name="sign-up-using-an-email-address"></a><span data-ttu-id="24835-137">Registrace pomocí e-mailovou adresu</span><span class="sxs-lookup"><span data-stu-id="24835-137">Sign up using an email address</span></span>

<span data-ttu-id="24835-138">Pokud si zvolíte účet sociálních nechcete použít k ověření, můžete vytvořit uživatelský účet Azure AD B2C pomocí platné e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="24835-138">If you choose to not use a social account to provide authentication, you can create an Azure AD B2C user account using a valid email address.</span></span> <span data-ttu-id="24835-139">Místní uživatelský účet Azure AD B2C použije Azure Active Directory jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="24835-139">An Azure AD B2C local user account uses Azure Active Directory as the identity provider.</span></span> <span data-ttu-id="24835-140">Chcete-li používat e-mailovou adresu, klikněte na tlačítko **nemáte účet? Zaregistrujte si teď** odkaz.</span><span class="sxs-lookup"><span data-stu-id="24835-140">To use your email address, click the **Don't have an account? Sign up now** link.</span></span>

![Přihlásit nebo zaregistrovat pomocí e-mailu](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-email-wpf.png)

<span data-ttu-id="24835-142">Zadejte platnou e-mailovou adresu a klikněte na tlačítko **Odeslat ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="24835-142">Enter a valid email address and click **Send verification code**.</span></span> <span data-ttu-id="24835-143">Platné e-mailová adresa se vyžaduje pro příjem ověřovací kód z Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="24835-143">A valid email address is required to receive the verification code from Azure AD B2C.</span></span>

<span data-ttu-id="24835-144">Zadejte ověřovací kód odběru e-mailem a klikněte na tlačítko **ověřit kód**.</span><span class="sxs-lookup"><span data-stu-id="24835-144">Enter the verification code you receive in email and click **Verify code**.</span></span>

<span data-ttu-id="24835-145">Přidat informace z vašeho profilu a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="24835-145">Add your profile information and click **Create**.</span></span>

![Zaregistrovat nový účet pomocí e-mailu](media/active-directory-b2c-quickstarts-desktop-app/sign-up-new-account-profile-email-wpf.png)

<span data-ttu-id="24835-147">Úspěšně jste vytvořili nový účet místního uživatele Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="24835-147">You have successfully created a new Azure AD B2C local user account.</span></span> <span data-ttu-id="24835-148">Po přihlášení se přístupový token se zobrazí v *Token informace* textové pole.</span><span class="sxs-lookup"><span data-stu-id="24835-148">After sign-in, the access token is shown in the *Token info* text box.</span></span> <span data-ttu-id="24835-149">Přístupový token se používá při přístupu k prostředku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="24835-149">The access token is used when accessing the API resource.</span></span>

![Autorizační token](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="edit-your-profile"></a><span data-ttu-id="24835-151">Úpravy profilu</span><span class="sxs-lookup"><span data-stu-id="24835-151">Edit your profile</span></span>

<span data-ttu-id="24835-152">Azure Active Directory B2C poskytuje funkce, aby uživatelé mohli aktualizaci jejich profilů.</span><span class="sxs-lookup"><span data-stu-id="24835-152">Azure Active Directory B2C provides functionality to allow users to update their profiles.</span></span> <span data-ttu-id="24835-153">Klikněte na tlačítko **upravit profil** upravit profil, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="24835-153">Click **Edit profile** to edit the profile you created.</span></span>

![Úprava profilu](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

<span data-ttu-id="24835-155">Vyberte zprostředkovatele identity přidružené k účtu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="24835-155">Choose the identity provider associated with the account you created.</span></span> <span data-ttu-id="24835-156">Například pokud jste použili Twitter jako zprostředkovatele identity při vytváření účtu, vyberte služby Twitter. Chcete-li upravit podrobnosti související profilu.</span><span class="sxs-lookup"><span data-stu-id="24835-156">For example, if you used Twitter as the identity provider when you created your account, choose Twitter to modify the associated profile details.</span></span>

![Vyberte zprostředkovatele spojeného s profilem upravit](media/active-directory-b2c-quickstarts-desktop-app/edit-account-choose-provider-wpf.png)

<span data-ttu-id="24835-158">Změna vaše **zobrazovaný název** nebo **města**.</span><span class="sxs-lookup"><span data-stu-id="24835-158">Change your **Display name** or **City**.</span></span> 

![Aktualizovat profil](media/active-directory-b2c-quickstarts-desktop-app/update-profile-wpf.png)

<span data-ttu-id="24835-160">Nový přístupový token se zobrazí v *Token informace* textové pole.</span><span class="sxs-lookup"><span data-stu-id="24835-160">A new access token is displayed in the *Token info* text box.</span></span> <span data-ttu-id="24835-161">Pokud chcete ověřit změny v profilu, zkopírujte a vložte do tokenu decoder https://jwt.ms přístupový token.</span><span class="sxs-lookup"><span data-stu-id="24835-161">If you want to verify the changes to your profile, copy and paste the access token into the token decoder https://jwt.ms.</span></span>

![Autorizační token](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="access-a-resource"></a><span data-ttu-id="24835-163">Přístup k prostředku</span><span class="sxs-lookup"><span data-stu-id="24835-163">Access a resource</span></span>

<span data-ttu-id="24835-164">Klikněte na tlačítko **volání rozhraní API** vytvořte žádost na do Azure AD B2C zabezpečené https://fabrikamb2chello.azurewebsites.net/hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="24835-164">Click **Call API** to make a request to the Azure AD B2C secured resource https://fabrikamb2chello.azurewebsites.net/hello.</span></span> 

![Volání rozhraní API](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

<span data-ttu-id="24835-166">Aplikace obsahuje token přístupu zobrazují v *Token informace* textového pole v požadavku.</span><span class="sxs-lookup"><span data-stu-id="24835-166">The application includes the access token displayed in the *Token info* text box in the request.</span></span> <span data-ttu-id="24835-167">Rozhraní API odesílá zpět zobrazovaný název obsažených v tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="24835-167">The API sends back the display name contained in the access token.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24835-168">Další postup</span><span class="sxs-lookup"><span data-stu-id="24835-168">Next steps</span></span>

<span data-ttu-id="24835-169">Dalším krokem je vytvoření vlastního klienta Azure AD B2C a konfigurace vzorku, který se spustí pomocí vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="24835-169">The next step is to create your own Azure AD B2C tenant and configure the sample to run using your tenant.</span></span> 

> [!div class="nextstepaction"]
> <span data-ttu-id="24835-170">Vytvoření klienta Azure Active Directory B2C na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="24835-170">[Create an Azure Active Directory B2C tenant in the Azure portal](active-directory-b2c-get-started.md)</span></span>
