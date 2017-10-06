---
title: "aaaAzure AD v2 Android Začínáme - Test | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API aplikace pro Android"
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
ms.openlocfilehash: 499f32b46fd44cca0e52179bced49b311135d8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

1. Nasaďte vaše zařízení nebo emulátoru tooyour kódu.
2. Až budete připravené tootest, použijte Microsoft Azure Active Directory (účet organizace) nebo účet toosign Account Microsoft (live.com, outlook.com) v. 

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Přihlášení](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Vyjádřit souhlas.
Hello poprvé, co se uživatel přihlásí tooyour aplikace, zobrazí se souhlasu obrazovky podobné toohello níže, kde potřebují přijmout tooexplicitly: 

![Vyjádřit souhlas.](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět hello výsledky tooMicrosoft volání rozhraní Graph API, mi používá koncový bod profilu uživatele hello tootooobtain - https://graph.microsoft.com/v1.0/me. Seznam běžných Microsoft Graph koncové body, najdete v tématu to [článku](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Hello Microsoft Graph API vyžaduje hello `user.read` obor tooread hello uživatelský profil. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory. Například pro Microsoft Graph hello oboru `Calendars.Read` je kalendářích požadované toolist hello uživatele. V pořadí tooaccess hello kalendáře uživatele v kontextu aplikace, musíte tooadd hello `Calendars.Read` delegovaná oprávnění toohello registrace aplikace na informace a poté přidejte hello `Calendars.Read` toohello oboru `acquireTokenSilentAsync` volání. zvýšit číslo hello oborů, může být Hello uživatel vyzván pro další souhlas všech uživatelů.

<!--end-collapse-->
