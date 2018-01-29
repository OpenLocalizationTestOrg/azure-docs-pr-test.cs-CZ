---
title: "Azure Active Directory B2C: Konfigurace účtu Microsoft | Microsoft Docs"
description: "Zadejte registrace a přihlášení k příjemce s účty Microsoft v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: e746a5e7a2d9a13eb23ac0268104a9394c9b198d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty Microsoft
## <a name="create-a-microsoft-account-application"></a>Vytvoření aplikace účtu Microsoft
Pokud chcete používat účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci účtu Microsoft a zadejte se správné parametry. Potřebujete účet Microsoft k tomu. Pokud nemáte, můžete ho na získat [https://www.live.com/](https://www.live.com/).

1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.
2. Klikněte na tlačítko **přidat aplikaci**.
   
    ![Microsoft účet – přidání nové aplikace](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Zadejte **název** pro aplikaci a klikněte na tlačítko **vytvořit aplikaci**.
   
    ![Účet Microsoft - název aplikace.](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Zkopírujte hodnotu **Id aplikace**. Je nutné ho nakonfigurovat účet Microsoft jako zprostředkovatel identity ve vašem klientovi.
   
    ![Účet Microsoft - Id aplikace](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Klikněte na **přidat platformy** a zvolte **webové**.
   
    ![Microsoft účet – přidejte platformu](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Účet Microsoft – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **identifikátory URI přesměrování** pole. Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).
   
    ![Účet Microsoft - adresy URL pro přesměrování](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Klikněte na **generovat nové heslo** pod **tajné klíče aplikace** části. Zkopírujte nové heslo, které jsou zobrazené na obrazovce. Je nutné ho nakonfigurovat účet Microsoft jako zprostředkovatel identity ve vašem klientovi. Toto heslo je důležitým bezpečnostním pověřením.
   
    ![Microsoft účet - vytvořit nové heslo](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Účet Microsoft - nové heslo](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Zaškrtněte políčko, která uvádí, že **Live SDK podporu** pod **pokročilé možnosti** části. Klikněte na **Uložit**.
   
    ![Účet Microsoft - Live SDK podpory](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Nakonfigurujte účet Microsoft jako zprostředkovatel identity ve vašem klientovi
1. Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.
2. Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na **Přidat** v horní části okna.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity. Zadejte například "Účet spravované služby".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **účtu Microsoft**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte Id aplikace a heslo aplikace účtu Microsoft, který jste vytvořili dříve.
7. Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložit konfiguraci vašeho účtu Microsoft.

