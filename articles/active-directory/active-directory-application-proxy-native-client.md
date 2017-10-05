---
title: "Publikování aplikací pro nativní klient – Azure AD | Microsoft Docs"
description: "Popisuje postup povolení aplikace nativního klienta ke komunikaci s konektor Proxy aplikace Azure AD poskytnout zabezpečený vzdálený přístup k místní aplikace."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: bdaa5af6ff5331bc310499586615b48a864c3c5e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Postup povolení nativního klienta aplikace pro interakci s proxy aplikace

Kromě webovým aplikacím Proxy aplikace služby Active Directory Azure také slouží k publikování aplikací pro nativní klient systému. Nativní klientské aplikace se liší od webové aplikace, protože se instalují na zařízení, když webové aplikace jsou dostupné prostřednictvím prohlížeče. 

Proxy aplikací podporuje nativní klient aplikace přijímá Azure AD, které jsou vystavené tokeny, které se odesílají v hlavičky standardních autorizovat HTTP.

![Vztah mezi koncovým uživatelům, Azure Active Directory a publikování aplikace](./media/active-directory-application-proxy-native-client/richclientflow.png)

Knihovnu pro ověřování Azure AD, který se stará o ověřování a podporuje mnoho klienta prostředí, použijte k publikování nativních aplikací. Proxy aplikací zapadá do [nativní aplikace za účelem scénáře webového rozhraní API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). Tento článek vás provede čtyři kroky k publikování nativní aplikace s Proxy aplikace a knihovna ověřování Azure AD. 

## <a name="step-1-publish-your-application"></a>Krok 1: Publikování aplikace
Publikování aplikace proxy, stejně jako všechny ostatní aplikace a přiřadit uživatelům přístup k aplikaci. Další informace najdete v tématu [publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Krok 2: Konfigurace aplikace
Nativní aplikace nakonfigurujte následujícím způsobem:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Přejděte na **Azure Active Directory** > **registrace aplikace**.
3. Vyberte **nové registrace aplikace**.
4. Zadejte název pro vaši aplikaci, vyberte **nativní** jako typ aplikace a zadejte identifikátor URI přesměrování pro aplikaci. 

   ![Vytvořit novou registraci aplikace](./media/active-directory-application-proxy-native-client/create.png)
5. Vyberte **Vytvořit**.

Podrobné informace o vytváření nové registrace aplikace, najdete v části [integrace aplikací s Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-to-other-applications"></a>Krok 3: Udělení přístupu k ostatním aplikacím
Povolte nativní aplikace, které mají být exponovány k ostatním aplikacím ve vašem adresáři:

1. Pořád ještě v **registrace aplikace**, vyberte novou nativní aplikaci, kterou jste právě vytvořili.
2. Vyberte **požadovaná oprávnění**.
3. Vyberte **Přidat**.
4. Prvním krokem, otevřete **vybrat rozhraní API**.
5. Pomocí panelu vyhledávání můžete najít aplikaci Proxy aplikace, kterou jste publikovali v první části. Vyberte aplikaci a pak klikněte na **vyberte**. 

   ![Vyhledejte aplikaci, proxy](./media/active-directory-application-proxy-native-client/select_api.png)
6. Otevřete druhý krok **vyberte oprávnění**.
7. Zaškrtnutím políčka pomocí zajišťují vašim nativní aplikace přístup k aplikaci proxy a pak klikněte na **vyberte**.

   ![Udělit přístup k proxy aplikace](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Vyberte **provádí**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Krok 4: Úprava knihovna ověřování Active Directory
Upravte kód nativní aplikace v rámci ověřování služby Active Directory Authentication Library (ADAL) zahrnout následující text:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Proměnné v ukázkovém kódu by měla být nahrazená následujícím způsobem:

* **ID klienta** naleznete na webu Azure portal. Přejděte na **Azure Active Directory** > **vlastnosti** a zkopírujte ID adresáře. 
* **Externí adresu URL** je front-end adresa URL jste zadali v aplikaci Proxy. Chcete-li tuto hodnotu najít, přejděte na **proxy aplikace** části proxy aplikace.
* **ID aplikace** nativní aplikace naleznete na **vlastnosti** stránka nativní aplikace.
* **Identifikátor URI přesměrování nativní aplikace** naleznete na **identifikátory URI přesměrování** stránka nativní aplikace.


## <a name="see-also"></a>Viz také

Další informace o toku nativní aplikace najdete v tématu [nativní aplikace za účelem webového rozhraní API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)
