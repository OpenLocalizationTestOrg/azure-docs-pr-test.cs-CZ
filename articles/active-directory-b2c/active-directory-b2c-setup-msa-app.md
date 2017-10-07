---
title: "Azure Active Directory B2C: Konfigurace účtu Microsoft | Microsoft Docs"
description: "Registrace a přihlášení tooconsumers poskytněte účty Microsoft v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: bec4777f003c459030f68c35b24f0e4bcddf84ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Zadejte tooconsumers registrace a přihlášení s účty Microsoft
## <a name="create-a-microsoft-account-application"></a>Vytvoření aplikace účtu Microsoft
toouse účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba toocreate k aplikaci účtu Microsoft a přiřaďte mu hello správné parametry. Tuto funkci potřebujete toodo účet Microsoft. Pokud nemáte, můžete ho na získat [https://www.live.com/](https://www.live.com/).

1. Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.
2. Klikněte na tlačítko **přidat aplikaci**.
   
    ![Microsoft účet – přidání nové aplikace](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Zadejte **název** pro aplikaci a klikněte na tlačítko **vytvořit aplikaci**.
   
    ![Účet Microsoft - název aplikace.](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Zkopírujte hodnotu hello **Id aplikace**. Je nutné ji tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi.
   
    ![Účet Microsoft - Id aplikace](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Klikněte na **přidat platformy** a zvolte **webové**.
   
    ![Microsoft účet – přidejte platformu](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Účet Microsoft – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **identifikátory URI přesměrování** pole. Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).
   
    ![Účet Microsoft - adresy URL pro přesměrování](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Klikněte na **generovat nové heslo** pod hello **tajné klíče aplikace** části. Zkopírujte hello nové heslo zobrazené na obrazovce. Je nutné ji tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi. Toto heslo je důležitým bezpečnostním pověřením.
   
    ![Microsoft účet - vytvořit nové heslo](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Účet Microsoft - nové heslo](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Zaškrtávací políčko hello, která uvádí, že **Live SDK podporu** pod hello **pokročilé možnosti** části. Klikněte na **Uložit**.
   
    ![Účet Microsoft - Live SDK podpory](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Nakonfigurujte účet Microsoft jako zprostředkovatel identity ve vašem klientovi
1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity hello. Zadejte například "Účet spravované služby".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **účtu Microsoft**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello Id aplikace a heslo hello aplikace účtu Microsoft, který jste vytvořili dříve.
7. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave účet Microsoft konfigurace.

