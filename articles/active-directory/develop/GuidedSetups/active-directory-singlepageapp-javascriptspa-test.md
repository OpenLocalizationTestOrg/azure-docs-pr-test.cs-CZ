---
title: "aaaAzure AD v2 JS SPA na základě nastavení – Test | Microsoft Docs"
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
ms.openlocfilehash: b2339431a070b5c4ad4058e6c1a9b19b83c84c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

> ### <a name="testing-with-visual-studio"></a>Testování v sadě Visual Studio
> Pokud používáte Visual Studio, stiskněte klávesu `F5` toorun projektu: hello prohlížeč otevře a přesměruje je příliš*http://localhost: {port}* kde uvidíte hello *volání Microsoft Graph API* tlačítko.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Testování v Pythonu nebo jiný webový server
> Pokud nepoužíváte Visual Studio, ujistěte se, váš webový server je spuštěna a je nakonfigurovaný toolisten tooa TCP port podle hello složku, která obsahuje vaše *index.html* souboru. Pro jazyk Python, můžete zahájit naslouchání portu toohello spuštěním hello v příkazu hello výzvy a Terminálový, ze složky aplikace hello:
> 
> ```bash
> python -m http.server 8080
> ```
>  Potom otevřete hello prohlížeč a zadejte *adrese http://localhost: 8080* nebo *http://localhost: {port}* – kde hello *port* odpovídá toohello port, který je váš webový server naslouchání. Měli byste vidět hello obsah vaší stránka index.html s hello *volání Microsoft Graph API* tlačítko.

## <a name="test-your-application"></a>Testování vaší aplikace

Po hello prohlížeč načte vaše *index.html*, klikněte na tlačítko hello *volání Microsoft Graph API* tlačítko. Pokud je to hello poprvé, přesměrování prohlížeče hello je toohello Microsoft Azure Active Directory v2 koncovému bodu, kde se zobrazí výzva toosign v.
 
![Snímek obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Vyjádřit souhlas.
Hello velmi při prvním přihlášení tooyour aplikace se zobrazí souhlasu obrazovky podobné toohello následující, kde je nutné tooaccept:

 ![Souhlas obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět informace o profilu uživatele vrácený hello odpovědi volání Microsoft Graph API.
 
 ![Výsledky](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Můžete také zobrazit základní informace o tokenu hello získali v hello *tokenu přístupu* a *ID tokenu deklarací* polí.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Hello Microsoft Graph API vyžaduje hello `user.read` obor tooread hello uživatelský profil. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory. Například pro Microsoft Graph hello oboru `Calendars.Read` je kalendářích požadované toolist hello uživatele. V pořadí tooaccess hello kalendáře uživatele v kontextu hello aplikace, musíte tooadd hello `Calendars.Read` delegovaná oprávnění toohello registrace aplikace na informace a poté přidejte hello `Calendars.Read` toohello oboru `acquireTokenSilent` volání. zvýšit číslo hello oborů, může být Hello uživatel vyzván pro další souhlas všech uživatelů.

Pokud rozhraní API back-end nevyžaduje obor (nedoporučuje se), můžete použít hello `clientId` jako obor hello v hello `acquireTokenSilent` nebo `acquireTokenRedirect` volání.

<!--end-collapse-->
