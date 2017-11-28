---
title: "Instalace na základě Azure AD v2 JS SPA – konfigurace | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: adff529bfdc40006666cc643d49a302d7f1d09ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="register-your-application"></a><span data-ttu-id="207ce-103">Registrace vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="207ce-103">Register your application</span></span>

<span data-ttu-id="207ce-104">Chcete-li vytvořit aplikaci, vyberte jeden z nich několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="207ce-104">There are multiple ways to create an application, please select one of them:</span></span>

### <a name="option-1-register-your-application-express-mode"></a><span data-ttu-id="207ce-105">Možnost 1: Registrace vaší aplikace (expresní režim)</span><span class="sxs-lookup"><span data-stu-id="207ce-105">Option 1: Register your application (Express mode)</span></span>
<span data-ttu-id="207ce-106">Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="207ce-106">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>

1.  <span data-ttu-id="207ce-107">Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span><span class="sxs-lookup"><span data-stu-id="207ce-107">Register your application via the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span></span>
2.  <span data-ttu-id="207ce-108">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="207ce-108">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="207ce-109">Ujistěte se, možnost *instalace na základě* je zaškrtnuta možnost</span><span class="sxs-lookup"><span data-stu-id="207ce-109">Make sure the option for *Guided Setup* is checked</span></span>
4.  <span data-ttu-id="207ce-110">Postupujte podle pokynů a získat ID aplikace a vložte jej do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="207ce-110">Follow the instructions to obtain the application ID and paste it into your code</span></span>

### <a name="option-2-register-your-application-advanced-mode"></a><span data-ttu-id="207ce-111">Možnost 2: Registrace vaší aplikace (pokročilé režim)</span><span class="sxs-lookup"><span data-stu-id="207ce-111">Option 2: Register your application (Advanced mode)</span></span>

1. <span data-ttu-id="207ce-112">Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci</span><span class="sxs-lookup"><span data-stu-id="207ce-112">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) to register an application</span></span>
2. <span data-ttu-id="207ce-113">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="207ce-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="207ce-114">Ujistěte se, možnost *instalace na základě* není zaškrtnuta</span><span class="sxs-lookup"><span data-stu-id="207ce-114">Make sure the option for *Guided Setup* is unchecked</span></span>
4.  <span data-ttu-id="207ce-115">Klikněte na tlačítko `Add Platform`, zvolte položku`Web`</span><span class="sxs-lookup"><span data-stu-id="207ce-115">Click `Add Platform`, then select `Web`</span></span>
5. <span data-ttu-id="207ce-116">Přidat `Redirect URL` , odpovídají adrese URL aplikace založené na vašem webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="207ce-116">Add the `Redirect URL` that correspond to the application's URL based on your web server.</span></span> <span data-ttu-id="207ce-117">Najdete v částech níže pokyny o tom, jak nastavit / získat adresu URL přesměrování v sadě Visual Studio a Python.</span><span class="sxs-lookup"><span data-stu-id="207ce-117">See the sections below for instructions on how to set/ obtain the redirect URL in Visual Studio and Python.</span></span>
6. <span data-ttu-id="207ce-118">Klikněte na tlačítko *uložit*</span><span class="sxs-lookup"><span data-stu-id="207ce-118">Click *Save*</span></span>

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="207ce-119">Visual Studio pokyny pro získání adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="207ce-119">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="207ce-120">Postupujte podle pokynů a získat adresu URL přesměrování:</span><span class="sxs-lookup"><span data-stu-id="207ce-120">Follow the instructions to obtain your redirect URL:</span></span>
> 1.    <span data-ttu-id="207ce-121">V *Průzkumníku řešení*, vyberte projekt a podívejte se na `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)</span><span class="sxs-lookup"><span data-stu-id="207ce-121">In *Solution Explorer*, select the project and look at the `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="207ce-122">Zkopírujte hodnotu z `URL` do schránky:</span><span class="sxs-lookup"><span data-stu-id="207ce-122">Copy the value from `URL` to the clipboard:</span></span><br/> <span data-ttu-id="207ce-123">![Vlastnosti projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="207ce-123">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="207ce-124">Přepnout zpět *portálu pro registraci aplikace* a vložte hodnotu jako `Redirect URL` a klikněte na tlačítko 'uložit.</span><span class="sxs-lookup"><span data-stu-id="207ce-124">Switch back to the *Application Registration Portal* and paste the value as a `Redirect URL` and click 'Save'</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="207ce-125">Nastavení přesměrování URL pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="207ce-125">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="207ce-126">Pro jazyk Python může web nastavit port serveru prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="207ce-126">For Python, you can set the web server port via command line.</span></span> <span data-ttu-id="207ce-127">Tato instalace s průvodcem používá port 8080 odkaz ale chování můžete používat všechny ostatní porty, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="207ce-127">This guided setup uses the port 8080 for reference but feel free to use any other port available.</span></span> <span data-ttu-id="207ce-128">V každém případě postupujte podle pokynů níže nastavit adresu URL přesměrování v informace o registraci aplikace:</span><span class="sxs-lookup"><span data-stu-id="207ce-128">In any case, follow the instructions below to set up a redirect URL in the application registration information:</span></span><br/>
> - <span data-ttu-id="207ce-129">Přepnout zpět *portálu pro registraci aplikace* a nastavte `http://localhost:8080/` jako `Redirect URL`, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* vlastní port TCP je číslo) a klikněte na tlačítko 'uložit.</span><span class="sxs-lookup"><span data-stu-id="207ce-129">Switch back to the *Application Registration Portal* and set `http://localhost:8080/` as a `Redirect URL`, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is the custom TCP port number) and click 'Save'</span></span>


#### <a name="configure-your-javascript-spa"></a><span data-ttu-id="207ce-130">Konfigurace vaší JavaScript SPA</span><span class="sxs-lookup"><span data-stu-id="207ce-130">Configure your JavaScript SPA</span></span>

1.  <span data-ttu-id="207ce-131">Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="207ce-131">Create a file named `msalconfig.js` containing the application registration information.</span></span> <span data-ttu-id="207ce-132">Pokud používáte Visual Studio, vyberte projektu (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="207ce-132">If you are using Visual Studio, select the project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="207ce-133">Název`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="207ce-133">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="207ce-134">Přidejte následující kód do vaší `msalconfig.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="207ce-134">Add the following code to your `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
<span data-ttu-id="207ce-135">Nahraďte <code>Enter_the_Application_Id_here</code> s Id aplikace, který jste právě zaregistrovali</span><span class="sxs-lookup"><span data-stu-id="207ce-135">Replace <code>Enter_the_Application_Id_here</code> with the Application Id you just registered</span></span>
</li>
</ol>
