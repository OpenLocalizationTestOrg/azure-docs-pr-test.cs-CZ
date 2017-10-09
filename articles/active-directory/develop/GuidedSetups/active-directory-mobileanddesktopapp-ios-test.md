---
title: "iOS v2 aaaAzure AD Začínáme - Test | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 98c73eddabf9664feb19ac6878e9d7315b9aa79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-querying-hello-microsoft-graph-api-from-your-ios-application"></a>Test dotazování hello Microsoft Graph API z vaší aplikace iOS

Stiskněte klávesu `Command`  +  `R` toorun hello kód v simulátoru hello.

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Pokud jste připravené tootest, klepněte na *'volání Microsoft Graph API,* a bude výzvami tootype svoje uživatelské jméno a heslo.

### <a name="consent"></a>Vyjádřit souhlas.
Hello při prvním přihlášení tooyour aplikace, zobrazí se souhlasu obrazovky podobné toohello níže, kde je nutné přijmout tooexplicitly:

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět informace o profilu uživatele vrácený hello Microsoft Graph API volání v hello *protokolování* části.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Hello Microsoft Graph API vyžaduje hello `user.read` obor tooread hello uživatelský profil. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory. Například pro Microsoft Graph hello oboru `Calendars.Read` je kalendářích požadované toolist hello uživatele. V pořadí tooaccess hello kalendáře uživatele v kontextu aplikace, musíte tooadd hello `Calendars.Read` delegovaná oprávnění toohello registrace aplikace na informace a poté přidejte hello `Calendars.Read` toohello oboru `acquireTokenSilent` volání. zvýšit číslo hello oborů, může být Hello uživatel vyzván pro další souhlas všech uživatelů.

<!--end-collapse-->



