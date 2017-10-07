---
title: "Azure Active Directory B2C: Konfigurace služby Twitter | Microsoft Docs"
description: "Registrace a přihlášení tooconsumers poskytněte Twitter účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 275c5c73fd5e8e5075e77fee942cbc1b5e1586cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-twitter-accounts"></a>Azure Active Directory B2C: Zadejte tooconsumers registrace a přihlášení s účty služby Twitter.

> [!NOTE]
> Tato funkce je ve verzi preview.
> 

## <a name="create-a-twitter-application"></a>Vytvořit aplikaci služby Twitter.
toouse Twitter jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba toocreate aplikace služby Twitter a přiřaďte mu hello správné parametry. Tuto funkci potřebujete toodo účtu vývojáře Twitter. Pokud nemáte, můžete ho na získat [https://dev.twitter.com/](https://dev.twitter.com/).

1. Přejděte toohello [Twitter webu pro vývojáře](https://dev.twitter.com/) a přihlaste se pomocí přihlašovacích údajů.
2. Klikněte na tlačítko **Moje aplikace** pod **nástroje & podporu** a pak klikněte na **vytvořit novou aplikaci**. 
3. Ve formuláři hello, zadejte hodnotu pro hello **název**, **popis**, a **webu**.
4. Pro hello **adresu URL zpětné volání**, zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Ujistěte se, že tooreplace **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).
5. Zkontrolujte hello pole tooagree toohello **vývojáře smlouvy** a klikněte na tlačítko **vytvořit aplikaci služby Twitter**.
6. Po vytvoření aplikace hello klikněte na tlačítko **klíče a přístupové tokeny**.
7. Zkopírujte hodnotu hello **uživatelský klíč** a **uživatelský tajný klíč**. Budete potřebovat oba dva tooconfigure Twitter jako zprostředkovatel identity ve vašem klientovi.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurace služby Twitter jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "Twitter".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Twitter**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello Twitter **uživatelský klíč** pro hello **id klienta** a hello Twitter **uživatelský tajný klíč**pro hello **tajný klíč klienta**.
7. Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** toosave konfiguraci služby Twitter.

