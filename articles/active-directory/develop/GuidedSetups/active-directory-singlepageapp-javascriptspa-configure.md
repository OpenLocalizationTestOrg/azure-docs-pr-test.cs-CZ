---
title: "na základě AD v2 JS SPA aaaAzure instalace – konfigurace | Microsoft Docs"
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
ms.openlocfilehash: 1b93298d4bd4e17dd261dbb75502a122c30aac97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="register-your-application"></a><span data-ttu-id="b7e47-103">Registrace vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="b7e47-103">Register your application</span></span>

<span data-ttu-id="b7e47-104">Existuje několik způsobů toocreate aplikace, vyberte jeden z nich:</span><span class="sxs-lookup"><span data-stu-id="b7e47-104">There are multiple ways toocreate an application, please select one of them:</span></span>

### <a name="option-1-register-your-application-express-mode"></a><span data-ttu-id="b7e47-105">Možnost 1: Registrace vaší aplikace (expresní režim)</span><span class="sxs-lookup"><span data-stu-id="b7e47-105">Option 1: Register your application (Express mode)</span></span>
<span data-ttu-id="b7e47-106">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="b7e47-106">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>

1.  <span data-ttu-id="b7e47-107">Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span><span class="sxs-lookup"><span data-stu-id="b7e47-107">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span></span>
2.  <span data-ttu-id="b7e47-108">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="b7e47-108">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="b7e47-109">Ujistěte se, zda text hello možnost *instalace na základě* je zaškrtnuta možnost</span><span class="sxs-lookup"><span data-stu-id="b7e47-109">Make sure hello option for *Guided Setup* is checked</span></span>
4.  <span data-ttu-id="b7e47-110">Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="b7e47-110">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="option-2-register-your-application-advanced-mode"></a><span data-ttu-id="b7e47-111">Možnost 2: Registrace vaší aplikace (pokročilé režim)</span><span class="sxs-lookup"><span data-stu-id="b7e47-111">Option 2: Register your application (Advanced mode)</span></span>

1. <span data-ttu-id="b7e47-112">Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace</span><span class="sxs-lookup"><span data-stu-id="b7e47-112">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="b7e47-113">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="b7e47-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="b7e47-114">Ujistěte se, zda text hello možnost *instalace na základě* není zaškrtnuta</span><span class="sxs-lookup"><span data-stu-id="b7e47-114">Make sure hello option for *Guided Setup* is unchecked</span></span>
4.  <span data-ttu-id="b7e47-115">Klikněte na tlačítko `Add Platform`, zvolte položku`Web`</span><span class="sxs-lookup"><span data-stu-id="b7e47-115">Click `Add Platform`, then select `Web`</span></span>
5. <span data-ttu-id="b7e47-116">Přidat hello `Redirect URL` , odpovídají adrese URL toohello aplikace založené na vašem webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="b7e47-116">Add hello `Redirect URL` that correspond toohello application's URL based on your web server.</span></span> <span data-ttu-id="b7e47-117">Najdete v části hello níže pokyny o tom, tooset / získat adresu URL přesměrování hello v sadě Visual Studio a Python.</span><span class="sxs-lookup"><span data-stu-id="b7e47-117">See hello sections below for instructions on how tooset/ obtain hello redirect URL in Visual Studio and Python.</span></span>
6. <span data-ttu-id="b7e47-118">Klikněte na tlačítko *uložit*</span><span class="sxs-lookup"><span data-stu-id="b7e47-118">Click *Save*</span></span>

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="b7e47-119">Visual Studio pokyny pro získání adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="b7e47-119">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="b7e47-120">Postupujte podle pokynů tooobtain hello přesměrování URL:</span><span class="sxs-lookup"><span data-stu-id="b7e47-120">Follow hello instructions tooobtain your redirect URL:</span></span>
> 1.    <span data-ttu-id="b7e47-121">V *Průzkumníku řešení*, vyberte hello projekt a podívejte se na hello `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)</span><span class="sxs-lookup"><span data-stu-id="b7e47-121">In *Solution Explorer*, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="b7e47-122">Zkopírujte hodnotu hello z `URL` toohello schránky:</span><span class="sxs-lookup"><span data-stu-id="b7e47-122">Copy hello value from `URL` toohello clipboard:</span></span><br/> <span data-ttu-id="b7e47-123">![Vlastnosti projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="b7e47-123">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="b7e47-124">Přepnout zpět toohello *portálu pro registraci aplikace* a vložte hodnotu hello jako `Redirect URL` a klikněte na tlačítko 'uložit.</span><span class="sxs-lookup"><span data-stu-id="b7e47-124">Switch back toohello *Application Registration Portal* and paste hello value as a `Redirect URL` and click 'Save'</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="b7e47-125">Nastavení přesměrování URL pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="b7e47-125">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="b7e47-126">Pro jazyk Python můžete nastavit port serveru webového hello prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b7e47-126">For Python, you can set hello web server port via command line.</span></span> <span data-ttu-id="b7e47-127">Tuto s průvodcem instalací používá hello port 8080 pro referenci ale myslíte, že volné toouse všechny ostatní porty, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b7e47-127">This guided setup uses hello port 8080 for reference but feel free toouse any other port available.</span></span> <span data-ttu-id="b7e47-128">V každém případě postupujte podle pokynů hello níže tooset si adresu URL přesměrování v informace o registraci aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="b7e47-128">In any case, follow hello instructions below tooset up a redirect URL in hello application registration information:</span></span><br/>
> - <span data-ttu-id="b7e47-129">Přepnout zpět toohello *portálu pro registraci aplikace* a nastavte `http://localhost:8080/` jako `Redirect URL`, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* hello vlastní port TCP je číslo) a klikněte na tlačítko 'uložit.</span><span class="sxs-lookup"><span data-stu-id="b7e47-129">Switch back toohello *Application Registration Portal* and set `http://localhost:8080/` as a `Redirect URL`, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is hello custom TCP port number) and click 'Save'</span></span>


#### <a name="configure-your-javascript-spa"></a><span data-ttu-id="b7e47-130">Konfigurace vaší JavaScript SPA</span><span class="sxs-lookup"><span data-stu-id="b7e47-130">Configure your JavaScript SPA</span></span>

1.  <span data-ttu-id="b7e47-131">Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b7e47-131">Create a file named `msalconfig.js` containing hello application registration information.</span></span> <span data-ttu-id="b7e47-132">Pokud používáte Visual Studio, vyberte hello projekt (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="b7e47-132">If you are using Visual Studio, select hello project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="b7e47-133">Název`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="b7e47-133">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="b7e47-134">Přidejte následující kód tooyour hello `msalconfig.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="b7e47-134">Add hello following code tooyour `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
<span data-ttu-id="b7e47-135">Nahraďte <code>Enter_the_Application_Id_here</code> s hello Id aplikace, které jste právě zaregistrovali</span><span class="sxs-lookup"><span data-stu-id="b7e47-135">Replace <code>Enter_the_Application_Id_here</code> with hello Application Id you just registered</span></span>
</li>
</ol>
