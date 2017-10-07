---
title: 'Azure Active Directory B2C: Google + konfigurace | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte Google + účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ef66eb17777acd95b5f4745ed6097c77e37663b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-google-accounts"></a>Azure Active Directory B2C: Poskytnout Google + účty tooconsumers registrace a přihlášení
## <a name="create-a-google-application"></a>Vytvoření aplikace Google +
toouse Google + jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate aplikace Google + a přiřaďte mu hello správné parametry. Tuto funkci potřebujete účet toodo Google +. Pokud nemáte, můžete ho na získat [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Přejděte toohello [konzole pro vývojáře Google](https://console.developers.google.com/) a přihlaste se pomocí přihlašovací údaje účtu Google +.
2. Klikněte na tlačítko **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.
   
    ![Google + – Začínáme](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + – nový projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření** v levé navigační hello.
4. Klikněte na tlačítko hello **obrazovky souhlas OAuth** karty v horní části hello.
   
    ![Google + - přihlašovací údaje](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Vyberte nebo zadejte platný **e-mailová adresa**, poskytovat **název produktu**a klikněte na tlačítko **Uložit**.
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Klikněte na tlačítko **nové přihlašovací údaje** a potom zvolte **ID klienta OAuth**.
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. V části **typ aplikace**, vyberte **webové aplikace**.
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Zadejte **název** pro vaši aplikaci, zadejte `https://login.microsoftonline.com` v hello **zdroje oprávnění JavaScript** pole, a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **autorizováno přesměrování identifikátory URI**pole. Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com). Hello **{klient}** hodnota je malá a velká písmena. Klikněte na možnost **Vytvořit**.
   
    ![Google + - vytvořit ID klienta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Zkopírujte hodnoty hello **ID klienta** a **tajný klíč klienta**. Budete potřebovat oba dva tooconfigure Google + jako zprostředkovatel identity ve vašem klientovi. **Tajný klíč klienta** je důležitým bezpečnostním pověřením.
   
    ![Google + - tajný klíč klienta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigurace Google + jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "G +".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Google**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello ID a klienta tajný klíč klienta z hello aplikace Google +, který jste vytvořili dříve.
7. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave konfiguraci Google +.

