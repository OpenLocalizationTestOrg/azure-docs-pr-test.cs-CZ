---
title: "aaaHow tooperform kontrola přístupu | Microsoft Docs"
description: "Zjistěte, jak hello tooperform a kontrola s aplikací Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 301a5e9f97b68fedfbf4954e0bd7dadb7f0fc510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-an-access-review-in-azure-ad-privileged-identity-management"></a>Jak tooperform přístupu zkontrolujte v Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management zjednodušuje, jak spravovat podniky tooresources privilegovaného přístupu ve službě Azure AD a dalším službám Microsoft online jako je Office 365 nebo Microsoft Intune.  

Pokud jsou přiřazenou roli správce tooan, privilegovaných rolí správce ve vaší organizaci může požádat tooregularly potvrďte, že je stále nutné této role pro úlohu. Může získat e-mailu, který obsahuje odkaz, nebo můžete přejít rovnou toohello [portál Azure](https://portal.azure.com). Postupujte podle kroků hello v tento článek tooperform samoobslužné zkontrolujte přiřazené role.

Pokud jste správcem privilegovaných rolí zájem o recenze přístup, získat další podrobnosti o [jak toostart přístupu zkontrolujte](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="add-hello-privileged-identity-management-application"></a>Přidání aplikace Privileged Identity Management hello
Můžete použít aplikaci hello Azure AD Privileged Identity Management (PIM) v hello [portál Azure](https://portal.azure.com/) tooperform zkontrolovali.  Pokud nemáte hello aplikace Azure AD Privileged Identity Management na portálu, postupujte podle těchto kroků tooget spuštěna.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Vyberte své uživatelské jméno v hello pravém horním rohu hello portál Azure a vyberte hello adresáře, kde vám budou pracovat.
3. Vyberte **další služby** a používat hello filtru textbox toosearch pro **Azure AD Privileged Identity Management**.
4. Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**. Otevře se Hello aplikace Privileged Identity Management.

## <a name="approve-or-deny-access"></a>Schválí nebo zamítne přístup
Při schválit nebo zamítnout přístup, můžete se právě informuje hello kontrolora zda můžete dál používat tuto roli nebo ne. Zvolte **schválit** toostay hello role, chcete-li nebo **Odepřít** Pokud jste nebudete potřebovat hello přístup už. Stav vaší nedojde ke změně hned, dokud hello kontrolor platí hello výsledky.
Postupujte podle těchto kroků toofind a dokončete kontrola přístupu hello:

1. V hello PIM aplikace, vyberte **zkontrolujte privilegovaný přístup**. Pokud máte všechny recenze čekající přístup, zobrazí se hello Azure AD přístup kontroluje okno.
2. Vyberte kontrolní hello chcete toocomplete.
3. Pokud jste vytvořili hello kontrolní, zobrazí jako hello pouze uživatel probíhající kontrola hello. Vyberte název další tooyour hello zaškrtnutí.
4. Vyberte buď **schválit** nebo **Odepřít**. Může být nutné tooinclude důvod pro vaše rozhodnutí v hello **zadat příslušný důvod** textové pole.  
5. Zavřít hello **rolí zkontrolujte Azure AD** okno.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
