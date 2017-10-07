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
## <a name="add-hello-applications-registration-information-tooyour-app"></a>Přidání aplikace hello registrační informace tooyour aplikace

V tomto kroku třeba tooconfigure hello přesměrování URL informace o registraci aplikace a poté přidejte hello Id aplikace tooyour JavaScript SPA aplikace.

### <a name="configure-redirect-url"></a>Nakonfigurujte URL přesměrování

Konfigurace hello `Redirect URL` pole výše s hello adresu URL stránky index.html založené na vašem webovém serveru a pak klikněte na *aktualizace*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio pokyny pro získání adresy URL přesměrování
> tooobtain adresa URL přesměrování, hello postupujte podle pokynů níže:
> 1.    V *Průzkumníku řešení*, vyberte hello projekt a podívejte se na hello `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)
> 2.    Zkopírujte hodnotu hello z `URL` toohello schránky:<br/> ![Vlastnosti projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Vložit hodnotu hello jako `Redirect URL` na hello horní části této stránky, klikněte`Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Nastavení přesměrování URL pro jazyk Python
> Pro jazyk Python můžete nastavit port serveru webového hello prostřednictvím příkazového řádku. Tuto s průvodcem instalací používá hello port 8080 pro referenci ale myslíte, že volné toouse všechny ostatní porty, které jsou k dispozici. V každém případě postupujte podle pokynů hello níže tooset si adresu URL přesměrování v informace o registraci aplikace hello:<br/>
> Nastavit `http://localhost:8080/` jako `Redirect URL` na hello horní části této stránky, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* je hello vlastní číslo portu TCP) a potom klikněte na 'aktualizace.

### <a name="configure-your-javascript-spa-application"></a>Konfiguraci aplikace JavaScript SPA

1.  Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace hello. Pokud používáte Visual Studio, vyberte hello projekt (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`. Název`msalconfig.js`
2.  Přidejte následující kód tooyour hello `msalconfig.js` souboru:

```javascript
var msalconfig = {
    clientID: "[Enter hello application Id here]",
    redirectUri: location.origin
};
``` 
