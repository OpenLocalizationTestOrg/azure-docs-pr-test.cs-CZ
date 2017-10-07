---
title: "aaaAzure AD v2 Windows Desktop Začínáme - Test | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 0ae9612e1585c54a3fe35ba9d18f92554099b2c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

V pořadí tootest vaší aplikace, stiskněte klávesu `F5` toorun projektu v sadě Visual Studio. Hlavní okno by měl zobrazit:

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Když jste připravené tootest, klikněte na tlačítko *volání Microsoft Graph API* a používat službu Microsoft Azure Active Directory (účet organizace) nebo účet toosign Account Microsoft (live.com, outlook.com) v. Ho je hello poprvé, zobrazí se okno s dotazem, toosign uživatele v:

![Přihlášení](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Vyjádřit souhlas.
Hello při prvním přihlášení tooyour aplikace, zobrazí se souhlasu obrazovky podobné toohello níže, kde je nutné přijmout tooexplicitly:

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět informace o profilu uživatele vrácený hello Microsoft Graph API volání na úvodní obrazovka výsledky volání rozhraní API.

Měli byste taky vidět základní informace o tokenu hello získaných prostřednictvím `AcquireTokenAsync` nebo `AcquireTokenSilentAsync` hello informace o tokenu pole:

|Vlastnost  |Formát  |Popis |
|---------|---------|---------|
|Name (Název) | {Úplné uživatelské jméno} |Hello uživatelské jméno a příjmení|
|Uživatelské jméno |<span>user@domain.com</span> |uživatelské jméno Hello používá tooidentify hello uživatele|
|Platnost tokenu vyprší |{{Data a času}         |Hello čas, na které hello vyprší platnost tokenu. MSAL bude prodloužit datum vypršení platnosti hello vám obnovením hello token, pokud je to nezbytné|
|Přístupový token |{Řetězec}         |řetězec tokenu Hello odeslány, odešle tooHTTP požadavky, které vyžadují hlavičku autorizace|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění
Rozhraní Graph API vyžaduje hello `user.read` obor tooread uživatelský profil. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé jiné rozhraní Graph API a také vlastní rozhraní API pro back-end serveru vyžadovat další obory. Například pro graf `Calendars.Read` je kalendářích požadované toolist uživatele. V pořadí tooaccess hello kalendáře uživatele v kontextu aplikace, musíte tooadd `Calendars.Read` delegovat informace o registraci aplikace a pak přidejte `Calendars.Read` toohello `AcquireTokenAsync` volání. Zvýšit číslo hello oborů, může být uživatel vyzván pro další souhlas všech uživatelů.

<!--end-collapse-->



