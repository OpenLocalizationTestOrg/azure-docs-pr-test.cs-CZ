---
title: "Instalace na základě SPA JS v2 Azure AD: konfigurace (ARP) | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory (ARP)"
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
ms.openlocfilehash: 708f4ff606d79639de979918a9cacd4ed75db311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="e8a2d-103">Přidat informace o registraci aplikace do aplikace</span><span class="sxs-lookup"><span data-stu-id="e8a2d-103">Add the application’s registration information to your App</span></span>

<span data-ttu-id="e8a2d-104">V tomto kroku budete muset nakonfigurovat adresu URL pro přesměrování informace o registraci aplikace a pak přidejte do vaší aplikace JavaScript SPA Id aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-104">In this step, you need to configure the Redirect URL of your application registration information and then add the Application Id to your JavaScript SPA application.</span></span>

### <a name="configure-redirect-url"></a><span data-ttu-id="e8a2d-105">Nakonfigurujte URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="e8a2d-105">Configure redirect URL</span></span>

<span data-ttu-id="e8a2d-106">Konfigurace `Redirect URL` pole výše s adresou URL pro stránku index.html založené na vašem webovém serveru a pak klikněte na *aktualizace*.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-106">Configure the `Redirect URL` field above with the URL for your index.html page based on your web server, then click *Update*.</span></span>


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="e8a2d-107">Visual Studio pokyny pro získání adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="e8a2d-107">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="e8a2d-108">Pokud chcete získat adresu URL přesměrování, postupujte podle následujících pokynů:</span><span class="sxs-lookup"><span data-stu-id="e8a2d-108">To obtain your redirect URL, follow the instructions below:</span></span>
> 1.    <span data-ttu-id="e8a2d-109">V *Průzkumníku řešení*, vyberte projekt a podívejte se na `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)</span><span class="sxs-lookup"><span data-stu-id="e8a2d-109">In *Solution Explorer*, select the project and look at the `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="e8a2d-110">Zkopírujte hodnotu z `URL` do schránky:</span><span class="sxs-lookup"><span data-stu-id="e8a2d-110">Copy the value from `URL` to the clipboard:</span></span><br/> <span data-ttu-id="e8a2d-111">![Vlastnosti projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="e8a2d-111">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="e8a2d-112">Vložit hodnotu jako `Redirect URL` horní tuto stránku, klikněte`Update`</span><span class="sxs-lookup"><span data-stu-id="e8a2d-112">Paste the value as a `Redirect URL` on the top of this page, then click `Update`</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="e8a2d-113">Nastavení přesměrování URL pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="e8a2d-113">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="e8a2d-114">Pro jazyk Python může web nastavit port serveru prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-114">For Python, you can set the web server port via command line.</span></span> <span data-ttu-id="e8a2d-115">Tato instalace s průvodcem používá port 8080 odkaz ale chování můžete používat všechny ostatní porty, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-115">This guided setup uses the port 8080 for reference but feel free to use any other port available.</span></span> <span data-ttu-id="e8a2d-116">V každém případě postupujte podle pokynů níže nastavit adresu URL přesměrování v informace o registraci aplikace:</span><span class="sxs-lookup"><span data-stu-id="e8a2d-116">In any case, follow the instructions below to set up a redirect URL in the application registration information:</span></span><br/>
> <span data-ttu-id="e8a2d-117">Nastavit `http://localhost:8080/` jako `Redirect URL` horní této stránky, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* je vlastní číslo portu TCP) a potom klikněte na 'aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-117">Set `http://localhost:8080/` as a `Redirect URL` on the top of this page, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is the custom TCP port number), and then click 'Update'</span></span>

### <a name="configure-your-javascript-spa-application"></a><span data-ttu-id="e8a2d-118">Konfiguraci aplikace JavaScript SPA</span><span class="sxs-lookup"><span data-stu-id="e8a2d-118">Configure your JavaScript SPA application</span></span>

1.  <span data-ttu-id="e8a2d-119">Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-119">Create a file named `msalconfig.js` containing the application registration information.</span></span> <span data-ttu-id="e8a2d-120">Pokud používáte Visual Studio, vyberte projektu (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="e8a2d-120">If you are using Visual Studio, select the project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="e8a2d-121">Název`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="e8a2d-121">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="e8a2d-122">Přidejte následující kód do vaší `msalconfig.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="e8a2d-122">Add the following code to your `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
