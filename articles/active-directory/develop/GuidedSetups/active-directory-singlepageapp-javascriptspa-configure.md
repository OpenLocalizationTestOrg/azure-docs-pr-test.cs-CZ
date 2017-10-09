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
## <a name="register-your-application"></a>Registrace vaší aplikace

Existuje několik způsobů toocreate aplikace, vyberte jeden z nich:

### <a name="option-1-register-your-application-express-mode"></a>Možnost 1: Registrace vaší aplikace (expresní režim)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:

1.  Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, zda text hello možnost *instalace na základě* je zaškrtnuta možnost
4.  Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu

### <a name="option-2-register-your-application-advanced-mode"></a>Možnost 2: Registrace vaší aplikace (pokročilé režim)

1. Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace
2. Zadejte název vaší aplikace a e-mailu 
3. Ujistěte se, zda text hello možnost *instalace na základě* není zaškrtnuta
4.  Klikněte na tlačítko `Add Platform`, zvolte položku`Web`
5. Přidat hello `Redirect URL` , odpovídají adrese URL toohello aplikace založené na vašem webovém serveru. Najdete v části hello níže pokyny o tom, tooset / získat adresu URL přesměrování hello v sadě Visual Studio a Python.
6. Klikněte na tlačítko *uložit*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio pokyny pro získání adresy URL přesměrování
> Postupujte podle pokynů tooobtain hello přesměrování URL:
> 1.    V *Průzkumníku řešení*, vyberte hello projekt a podívejte se na hello `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)
> 2.    Zkopírujte hodnotu hello z `URL` toohello schránky:<br/> ![Vlastnosti projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Přepnout zpět toohello *portálu pro registraci aplikace* a vložte hodnotu hello jako `Redirect URL` a klikněte na tlačítko 'uložit.

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Nastavení přesměrování URL pro jazyk Python
> Pro jazyk Python můžete nastavit port serveru webového hello prostřednictvím příkazového řádku. Tuto s průvodcem instalací používá hello port 8080 pro referenci ale myslíte, že volné toouse všechny ostatní porty, které jsou k dispozici. V každém případě postupujte podle pokynů hello níže tooset si adresu URL přesměrování v informace o registraci aplikace hello:<br/>
> - Přepnout zpět toohello *portálu pro registraci aplikace* a nastavte `http://localhost:8080/` jako `Redirect URL`, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* hello vlastní port TCP je číslo) a klikněte na tlačítko 'uložit.


#### <a name="configure-your-javascript-spa"></a>Konfigurace vaší JavaScript SPA

1.  Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace hello. Pokud používáte Visual Studio, vyberte hello projekt (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`. Název`msalconfig.js`
2.  Přidejte následující kód tooyour hello `msalconfig.js` souboru:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Nahraďte <code>Enter_the_Application_Id_here</code> s hello Id aplikace, které jste právě zaregistrovali
</li>
</ol>
