---
title: 'Azure Active Directory B2C: Konfigurace Amazon | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte Amazon účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 60d7c4b76d9d3e86ed535765329abed07f1e5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-amazon-accounts"></a>Azure Active Directory B2C: Poskytnout účty Amazon tooconsumers registrace a přihlášení
## <a name="create-an-amazon-application"></a>Vytvoření aplikace Amazon
toouse Amazon jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate Amazon aplikaci a zadat s hello správné parametry. Tuto funkci potřebujete toodo účtu Amazon. Pokud nemáte, můžete ho na získat [http://www.amazon.com/](http://www.amazon.com/).

1. Přejděte toohello [středisku pro vývojáře Amazon](https://login.amazon.com/) a přihlaste se pomocí přihlašovacích údajů účtu Amazon.
2. Pokud jste tak již neučinili, klikněte na tlačítko **zaregistrovat**, postupujte podle kroků registrace vývojáře hello a přijměte zásady hello.
3. Klikněte na tlačítko **zaregistrujte novou aplikaci**.
   
    ![Registrace nové aplikace na webu Amazon hello](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Zadejte informace o aplikaci (**název**, **popis**, a **URL oznámení o ochraně osobních údajů**) a klikněte na tlačítko **Uložit**.
   
    ![Poskytuje informace o aplikaci pro registraci novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. V hello **nastavení webové** část hodnoty hello kopie **ID klienta** a **tajný klíč klienta**. (Potřebujete tooclick hello **zobrazit tajný klíč** tlačítko to toosee.) Budete potřebovat oba dva tooconfigure Amazon jako zprostředkovatel identity ve vašem klientovi. Klikněte na tlačítko **upravit** dolnímu hello hello oddílu. **Tajný klíč klienta** je důležitým bezpečnostním pověřením.
   
    ![Poskytnutí ID klienta a tajný klíč klienta pro novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Zadejte `https://login.microsoftonline.com` v hello **povolené zdroje JavaScript** pole a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **povoleno vrátit adresy URL** pole. Nahraďte **{klient}** s názvem vašeho klienta (například contoso.onmicrosoft.com). Klikněte na **Uložit**. Hello **{klient}** hodnota je malá a velká písmena.
   
    ![Poskytnutí zdroje JavaScript a vrátit adresy URL pro novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfigurace Amazon jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "Amzn".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Amazon**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello ID a klienta tajný klíč klienta z hello Amazon aplikace, kterou jste vytvořili dříve.
7. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave konfiguraci Amazon.

