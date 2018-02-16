---
title: "Test jednotky jednostránkové aplikace Azure AD B2C | Microsoft Docs"
description: "Test jednotky přihlášení, registrace, upravit profil a resetovat heslo uživatele cesty pomocí prostředí testovací Azure AD B2C"
services: active-directory-b2c
documentationcenter: 
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 5a8a46af-28bb-4b70-a7f0-01a5240d0255
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: ba8ee4657309ab2a541f4c7b3fd4879542eee63c
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="test-drive-a-single-page-application-configured-with-azure-ad-b2c"></a><span data-ttu-id="6a90d-103">Vyzkoušejte jednostránkové aplikace konfigurovaná na Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="6a90d-103">Test drive a single-page application configured with Azure AD B2C</span></span>

## <a name="about-this-sample"></a><span data-ttu-id="6a90d-104">O této ukázce</span><span class="sxs-lookup"><span data-stu-id="6a90d-104">About this sample</span></span>

<span data-ttu-id="6a90d-105">Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné.</span><span class="sxs-lookup"><span data-stu-id="6a90d-105">Azure Active Directory B2C provides cloud identity management to keep your application, business, and customers protected.</span></span>  <span data-ttu-id="6a90d-106">Tento rychlý start využívá k předvedení ukázkové jednostránkové aplikace:</span><span class="sxs-lookup"><span data-stu-id="6a90d-106">This quickstart uses a sample single page application to demonstrate:</span></span>

* <span data-ttu-id="6a90d-107">Pomocí **zaregistrovat nebo přihlásit** zásad k vytvoření nebo Přihlaste se pomocí zprostředkovatele identity sociálních nebo místní účet pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="6a90d-107">Using the **Sign Up or Sign In** policy to create or sign in with a social identity provider or a local account using an email address.</span></span> 
* <span data-ttu-id="6a90d-108">**Volání rozhraní API** načíst zobrazovaný název ze Azure AD B2C zabezpečené prostředků.</span><span class="sxs-lookup"><span data-stu-id="6a90d-108">**Calling an API** to retrieve your display name from an Azure AD B2C secured resource.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a90d-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a90d-109">Prerequisites</span></span>

* <span data-ttu-id="6a90d-110">Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:</span><span class="sxs-lookup"><span data-stu-id="6a90d-110">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with the following workloads:</span></span>
    - <span data-ttu-id="6a90d-111">**Vývoj pro ASP.NET a web**</span><span class="sxs-lookup"><span data-stu-id="6a90d-111">**ASP.NET and web development**</span></span>

* <span data-ttu-id="6a90d-112">Instalovat [Node.js](https://nodejs.org/en/download/)</span><span class="sxs-lookup"><span data-stu-id="6a90d-112">Install [Node.js](https://nodejs.org/en/download/)</span></span>

* <span data-ttu-id="6a90d-113">Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru.</span><span class="sxs-lookup"><span data-stu-id="6a90d-113">A social account from either Facebook, Google, Microsoft, or Twitter.</span></span> <span data-ttu-id="6a90d-114">Pokud nemáte účet sociálních, je třeba zadat platnou e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="6a90d-114">If you don't have a social account, a valid email address is required.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="6a90d-115">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="6a90d-115">Download the sample</span></span>

<span data-ttu-id="6a90d-116">[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="6a90d-116">[Download or clone the sample application](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) from GitHub.</span></span>

## <a name="run-the-sample-application"></a><span data-ttu-id="6a90d-117">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="6a90d-117">Run the sample application</span></span>

<span data-ttu-id="6a90d-118">Chcete tuto ukázku spustit z příkazového řádku Node.js:</span><span class="sxs-lookup"><span data-stu-id="6a90d-118">To run this sample from the Node.js command prompt:</span></span> 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

<span data-ttu-id="6a90d-119">V okně konzoly zobrazuje číslo portu pro webovou aplikaci spuštěné v počítači.</span><span class="sxs-lookup"><span data-stu-id="6a90d-119">The console window shows the port number for the web application running on your computer.</span></span>

```
Listening on port 6420...
```

<span data-ttu-id="6a90d-120">Otevřete `http://localhost:6420` ve webovém prohlížeči pro přístup k webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a90d-120">Open `http://localhost:6420` in a web browser to access the web application.</span></span>


![Ukázkovou aplikaci v prohlížeči](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a><span data-ttu-id="6a90d-122">Vytvořit účet</span><span class="sxs-lookup"><span data-stu-id="6a90d-122">Create an account</span></span>

<span data-ttu-id="6a90d-123">Klikněte **přihlášení** tlačítko Spustit Azure AD B2C **zaregistrovat nebo přihlásit** pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6a90d-123">Click the **Login** button to start the Azure AD B2C **Sign Up or Sign In** workflow.</span></span> <span data-ttu-id="6a90d-124">Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="6a90d-124">When creating an account, you can use an existing social identity provider account or an email account.</span></span>

### <a name="sign-up-using-a-social-identity-provider"></a><span data-ttu-id="6a90d-125">Zaregistrujte si pomocí zprostředkovatele identity sociálních</span><span class="sxs-lookup"><span data-stu-id="6a90d-125">Sign up using a social identity provider</span></span>

<span data-ttu-id="6a90d-126">Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="6a90d-126">To sign up using a social identity provider, click the button of the identity provider you want to use.</span></span> <span data-ttu-id="6a90d-127">Pokud byste radši chtěli použít e-mailovou adresu, přejít na [zaregistrovat pomocí e-mailovou adresu](#sign-up-using-an-email-address) části.</span><span class="sxs-lookup"><span data-stu-id="6a90d-127">If you prefer to use an email address, jump to the [Sign up using an email address](#sign-up-using-an-email-address) section.</span></span>

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

<span data-ttu-id="6a90d-129">Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit).</span><span class="sxs-lookup"><span data-stu-id="6a90d-129">You need to authenticate (sign-in) using your social account credentials and authorize the application to read information from your social account.</span></span> <span data-ttu-id="6a90d-130">Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města.</span><span class="sxs-lookup"><span data-stu-id="6a90d-130">By granting access, the application can retrieve profile information from the social account such as your name and city.</span></span> 

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

<span data-ttu-id="6a90d-132">Vaše nové podrobnosti účtu profilu jsou předem vyplněny informace z vašeho sociálních účtu.</span><span class="sxs-lookup"><span data-stu-id="6a90d-132">Your new account profile details are pre-populated with information from your social account.</span></span> 

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-spa/new-account-sign-up-profile-details-spa.png)

<span data-ttu-id="6a90d-134">Aktualizujte pole Zobrazovaný název, funkce a města a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="6a90d-134">Update the Display Name, Job Title, and City fields and click **Continue**.</span></span>  <span data-ttu-id="6a90d-135">Hodnoty, které zadáte, se používají pro svůj profil účtu uživatele Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6a90d-135">The values you enter are used for your Azure AD B2C user account profile.</span></span>

<span data-ttu-id="6a90d-136">Úspěšně jste vytvořili nový uživatelský účet Azure AD B2C, který používá zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="6a90d-136">You have successfully created a new Azure AD B2C user account that uses an identity provider.</span></span> 

<span data-ttu-id="6a90d-137">Další krok: [volání prostředek](#call-a-resource) části.</span><span class="sxs-lookup"><span data-stu-id="6a90d-137">Next step: [Call a resource](#call-a-resource) section.</span></span>

### <a name="sign-up-using-an-email-address"></a><span data-ttu-id="6a90d-138">Registrace pomocí e-mailovou adresu</span><span class="sxs-lookup"><span data-stu-id="6a90d-138">Sign up using an email address</span></span>

<span data-ttu-id="6a90d-139">Pokud si zvolíte účet sociálních nechcete použít k ověření, můžete vytvořit uživatelský účet Azure AD B2C pomocí platné e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="6a90d-139">If you choose to not use a social account to provide authentication, you can create an Azure AD B2C user account using a valid email address.</span></span> <span data-ttu-id="6a90d-140">Místní uživatelský účet Azure AD B2C použije Azure Active Directory jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="6a90d-140">An Azure AD B2C local user account uses Azure Active Directory as the identity provider.</span></span> <span data-ttu-id="6a90d-141">Chcete-li používat e-mailovou adresu, klikněte na tlačítko **nemáte účet? Zaregistrujte si teď** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6a90d-141">To use your email address, click the **Don't have an account? Sign up now** link.</span></span>

![Přihlásit nebo zaregistrovat pomocí e-mailu](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-email-spa.png)

<span data-ttu-id="6a90d-143">Zadejte platnou e-mailovou adresu a klikněte na tlačítko **Odeslat ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="6a90d-143">Enter a valid email address and click **Send verification code**.</span></span> <span data-ttu-id="6a90d-144">Platné e-mailová adresa se vyžaduje pro příjem ověřovací kód z Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6a90d-144">A valid email address is required to receive the verification code from Azure AD B2C.</span></span> 

<span data-ttu-id="6a90d-145">Zadejte ověřovací kód odběru e-mailem a klikněte na tlačítko **ověřit kód**.</span><span class="sxs-lookup"><span data-stu-id="6a90d-145">Enter the verification code you receive in email and click **Verify code**.</span></span>

<span data-ttu-id="6a90d-146">Přidat informace z vašeho profilu a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6a90d-146">Add your profile information and click **Create**.</span></span>

![Zaregistrovat nový účet pomocí e-mailu](media/active-directory-b2c-quickstarts-spa/sign-up-new-account-profile-email-web.png)

<span data-ttu-id="6a90d-148">Úspěšně jste vytvořili nový účet místního uživatele Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6a90d-148">You have successfully created a new Azure AD B2C local user account.</span></span>

## <a name="call-a-resource"></a><span data-ttu-id="6a90d-149">Volání prostředku</span><span class="sxs-lookup"><span data-stu-id="6a90d-149">Call a resource</span></span>

<span data-ttu-id="6a90d-150">Jakmile se přihlásíte, můžete kliknout na **volání webového rozhraní API** tlačítko tak, aby měl název zobrazení vrácená z volání webového rozhraní API jako objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="6a90d-150">Once signed in, you can click the **Call Web API** button to have your display name returned from the Web API call as a JSON object.</span></span> 

![Odpověď webové rozhraní API](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

## <a name="next-steps"></a><span data-ttu-id="6a90d-152">Další postup</span><span class="sxs-lookup"><span data-stu-id="6a90d-152">Next steps</span></span>

<span data-ttu-id="6a90d-153">Dalším krokem je vytvoření vlastního klienta Azure AD B2C a konfigurace vzorku, který se spustí pomocí vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="6a90d-153">The next step is to create your own Azure AD B2C tenant and configure the sample to run using your tenant.</span></span> 

> [!div class="nextstepaction"]
> <span data-ttu-id="6a90d-154">Vytvoření klienta Azure Active Directory B2C na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6a90d-154">[Create an Azure Active Directory B2C tenant in the Azure portal](active-directory-b2c-get-started.md)</span></span>