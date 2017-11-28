---
title: "aaaAzure AD v2 JS SPA na základě nastavení – konfigurace (ARP) | Microsoft Docs"
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
ms.openlocfilehash: 157f4e342cd684294e24da6ee1fad8a7c2fc266a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="5ef03-103">Přidání aplikace hello registrační informace tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="5ef03-103">Add hello application’s registration information tooyour App</span></span>

<span data-ttu-id="5ef03-104">V tomto kroku třeba tooconfigure hello přesměrování URL informace o registraci aplikace a poté přidejte hello Id aplikace tooyour JavaScript SPA aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ef03-104">In this step, you need tooconfigure hello Redirect URL of your application registration information and then add hello Application Id tooyour JavaScript SPA application.</span></span>

### <a name="configure-redirect-url"></a><span data-ttu-id="5ef03-105">Nakonfigurujte URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="5ef03-105">Configure redirect URL</span></span>

<span data-ttu-id="5ef03-106">Konfigurace hello `Redirect URL` pole výše s hello adresu URL stránky index.html založené na vašem webovém serveru a pak klikněte na *aktualizace*.</span><span class="sxs-lookup"><span data-stu-id="5ef03-106">Configure hello `Redirect URL` field above with hello URL for your index.html page based on your web server, then click *Update*.</span></span>


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="5ef03-107">Visual Studio pokyny pro získání adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="5ef03-107">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="5ef03-108">tooobtain adresa URL přesměrování, hello postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="5ef03-108">tooobtain your redirect URL, follow hello instructions below:</span></span>
> 1.    <span data-ttu-id="5ef03-109">V *Průzkumníku řešení*, vyberte hello projekt a podívejte se na hello `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)</span><span class="sxs-lookup"><span data-stu-id="5ef03-109">In *Solution Explorer*, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="5ef03-110">Zkopírujte hodnotu hello z `URL` toohello schránky:</span><span class="sxs-lookup"><span data-stu-id="5ef03-110">Copy hello value from `URL` toohello clipboard:</span></span><br/> <span data-ttu-id="5ef03-111">![Vlastnosti projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="5ef03-111">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="5ef03-112">Vložit hodnotu hello jako `Redirect URL` na hello horní části této stránky, klikněte`Update`</span><span class="sxs-lookup"><span data-stu-id="5ef03-112">Paste hello value as a `Redirect URL` on hello top of this page, then click `Update`</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="5ef03-113">Nastavení přesměrování URL pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="5ef03-113">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="5ef03-114">Pro jazyk Python můžete nastavit port serveru webového hello prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5ef03-114">For Python, you can set hello web server port via command line.</span></span> <span data-ttu-id="5ef03-115">Tuto s průvodcem instalací používá hello port 8080 pro referenci ale myslíte, že volné toouse všechny ostatní porty, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5ef03-115">This guided setup uses hello port 8080 for reference but feel free toouse any other port available.</span></span> <span data-ttu-id="5ef03-116">V každém případě postupujte podle pokynů hello níže tooset si adresu URL přesměrování v informace o registraci aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="5ef03-116">In any case, follow hello instructions below tooset up a redirect URL in hello application registration information:</span></span><br/>
> <span data-ttu-id="5ef03-117">Nastavit `http://localhost:8080/` jako `Redirect URL` na hello horní části této stránky, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* je hello vlastní číslo portu TCP) a potom klikněte na 'aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5ef03-117">Set `http://localhost:8080/` as a `Redirect URL` on hello top of this page, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is hello custom TCP port number), and then click 'Update'</span></span>

### <a name="configure-your-javascript-spa-application"></a><span data-ttu-id="5ef03-118">Konfiguraci aplikace JavaScript SPA</span><span class="sxs-lookup"><span data-stu-id="5ef03-118">Configure your JavaScript SPA application</span></span>

1.  <span data-ttu-id="5ef03-119">Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5ef03-119">Create a file named `msalconfig.js` containing hello application registration information.</span></span> <span data-ttu-id="5ef03-120">Pokud používáte Visual Studio, vyberte hello projekt (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="5ef03-120">If you are using Visual Studio, select hello project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="5ef03-121">Název`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="5ef03-121">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="5ef03-122">Přidejte následující kód tooyour hello `msalconfig.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="5ef03-122">Add hello following code tooyour `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "[Enter hello application Id here]",
    redirectUri: location.origin
};
``` 
