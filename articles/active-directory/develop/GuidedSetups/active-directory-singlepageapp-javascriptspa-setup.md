---
title: "aaaAzure AD v2 JS SPA na základě nastavení – nastavení | Microsoft Docs"
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
ms.openlocfilehash: 19e15c6c8db8bea2975f30e7505af79ccad17e02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="setting-up-your-web-server-or-project"></a><span data-ttu-id="ed33c-103">Nastavení webového serveru nebo projektu</span><span class="sxs-lookup"><span data-stu-id="ed33c-103">Setting up your web server or project</span></span>

> <span data-ttu-id="ed33c-104">Dáváte přednost toodownload tento ukázkový projekt místo?</span><span class="sxs-lookup"><span data-stu-id="ed33c-104">Prefer toodownload this sample's project instead?</span></span> 
> - [<span data-ttu-id="ed33c-105">Stáhněte si projekt sady Visual Studio hello</span><span class="sxs-lookup"><span data-stu-id="ed33c-105">Download hello Visual Studio project</span></span>](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> <span data-ttu-id="ed33c-106">nebo</span><span class="sxs-lookup"><span data-stu-id="ed33c-106">or</span></span>
> - <span data-ttu-id="ed33c-107">[Stáhnout soubory projektu hello](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) pro místní webový server, jako je například Python</span><span class="sxs-lookup"><span data-stu-id="ed33c-107">[Download hello project files](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) for a local web server, such as Python</span></span>
>
> <span data-ttu-id="ed33c-108">A potom přejděte toohello [krok konfigurace](#create-an-application-express) tooconfigure hello ukázka kódu před jeho provedením.</span><span class="sxs-lookup"><span data-stu-id="ed33c-108">And then  skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed33c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ed33c-109">Prerequisites</span></span>
<span data-ttu-id="ed33c-110">Místní webový server jako [Python http.server](https://www.python.org/downloads/), [http server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), nebo služba IIS Express integrace s [Visual Studio 2017](https://www.visualstudio.com/downloads/) je požadováno toorun tuto s průvodcem instalací.</span><span class="sxs-lookup"><span data-stu-id="ed33c-110">A local web server such as [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), or IIS Express integration with [Visual Studio 2017](https://www.visualstudio.com/downloads/) is required toorun this guided setup.</span></span> 

<span data-ttu-id="ed33c-111">Pokyny v této příručce jsou založené na Python a Visual Studio 2017, ale myslíte, že volné toouse jiné vývojové prostředí nebo webového serveru.</span><span class="sxs-lookup"><span data-stu-id="ed33c-111">Instructions in this guide are based on both Python and Visual Studio 2017, but feel free toouse any other development environment or Web Server.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="ed33c-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="ed33c-112">Create your project</span></span> 

> ### <a name="option-1-visual-studio"></a><span data-ttu-id="ed33c-113">Možnost 1: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed33c-113">Option 1: Visual Studio</span></span> 
> <span data-ttu-id="ed33c-114">Pokud používáte Visual Studio a vytvořit nový projekt, postupujte podle kroků hello toocreate nové řešení sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ed33c-114">If you are using Visual Studio and are creating a new project, follow hello steps below toocreate a new Visual Studio solution:</span></span>
> 1.    <span data-ttu-id="ed33c-115">V sadě Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="ed33c-115">In Visual Studio:  `File` > `New` > `Project`</span></span>
> 2.    <span data-ttu-id="ed33c-116">V části `Visual C#\Web`, vyberte možnost`ASP.NET Web Application (.NET Framework)`</span><span class="sxs-lookup"><span data-stu-id="ed33c-116">Under `Visual C#\Web`, select `ASP.NET Web Application (.NET Framework)`</span></span>
> 3.    <span data-ttu-id="ed33c-117">Název aplikace a klikněte na tlačítko *OK*</span><span class="sxs-lookup"><span data-stu-id="ed33c-117">Name your application and click *OK*</span></span>
> 4.    <span data-ttu-id="ed33c-118">V části `New ASP.NET Web Application`, vyberte možnost`Empty`</span><span class="sxs-lookup"><span data-stu-id="ed33c-118">Under `New ASP.NET Web Application`, select `Empty`</span></span>

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a><span data-ttu-id="ed33c-119">Možnost 2: Python / jiné webové servery</span><span class="sxs-lookup"><span data-stu-id="ed33c-119">Option 2: Python/ other web servers</span></span>
> <span data-ttu-id="ed33c-120">Ujistěte se, že máte nainstalovanou [Python](https://www.python.org/downloads/), pak postupujte podle kroku hello níže:</span><span class="sxs-lookup"><span data-stu-id="ed33c-120">Make sure you have installed [Python](https://www.python.org/downloads/), then follow hello step below:</span></span>
> - <span data-ttu-id="ed33c-121">Vytvořte složku toohost vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed33c-121">Create a folder toohost your application.</span></span>


## <a name="create-your-single-page-applications-ui"></a><span data-ttu-id="ed33c-122">Vytvoření uživatelského rozhraní aplikace jediné stránce</span><span class="sxs-lookup"><span data-stu-id="ed33c-122">Create your single page application’s UI</span></span>
1.  <span data-ttu-id="ed33c-123">Vytvoření *index.html* soubor pro vaše SPA JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ed33c-123">Create an *index.html* file for your JavaScript SPA.</span></span> <span data-ttu-id="ed33c-124">Pokud používáte Visual Studio, vyberte hello projekt (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `HTML page` a pojmenujte ji index.html</span><span class="sxs-lookup"><span data-stu-id="ed33c-124">If you are using Visual Studio, select hello project (project root folder), right click and select: `Add` > `New Item` > `HTML page` and name it index.html</span></span>
2.  <span data-ttu-id="ed33c-125">Přidejte následující kód tooyour stránku hello:</span><span class="sxs-lookup"><span data-stu-id="ed33c-125">Add hello following code tooyour page:</span></span>
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling hello page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn tooreference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.1/js/msal.min.js"></script>

    <!-- hello 'bluebird' and 'fetch' references below are required if you need toorun this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
