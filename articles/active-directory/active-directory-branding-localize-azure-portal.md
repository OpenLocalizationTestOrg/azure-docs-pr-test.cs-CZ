---
title: "Přidání konkrétní jazyk firemního brandingu na přihlašovací stránku ve službě Azure Active Directory | Microsoft Docs"
description: "Informace o přidání určité společnosti jazyk branding obrázky a text na stránku Azure přihlášení"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Přidání konkrétní jazyk firemního brandingu na přihlašovací stránku ve službě Azure Active Directory
Mnoho společností chce předcházet zmatení uživatele a upřednostňuje jednotný vzhled všech webů a služeb, které spravují. Tato funkce poskytuje Azure Active Directory tím, že se můžete přizpůsobit vzhled stránky přihlášení s svoje firemní logo a vlastní barevná schémata. Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity. Budete používat tuto stránku k zadání pověření.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Přizpůsobení přihlašovací stránky pro jiný jazyk
Elementy jazyka můžete přidat vlastní přihlašovací stránku pouze v případě, že jste již vytvořili vlastní stránku přihlášení, jak je popsáno v [přidání firemního brandingu na přihlašovací stránce](active-directory-branding-custom-signon-azure-portal.md). Jednu stránku přihlášení na adresář můžete nakonfigurovat výchozí sadu přizpůsobitelných prvků. Po dokončení konfigurace výchozí sadu elementů stránky, můžete nakonfigurovat další verze pro různá národní prostředí. Různé prvky mezi sebou můžete kombinovat. Například je může:

* Vytvoří výchozí **přihlašovací stránky image** , platí pro všechny kultury, potom vytvořte specifické verze pro angličtinu a francouzštinu. Když svoje prohlížeče nastavíte na jednu z těchto dvou jazyků, bitovou kopii pro specifický jazyk se zobrazí, když u všech ostatních jazyků se zobrazí výchozí obrázek.
* Nakonfigurujte různá loga vaší organizace (třeba verze pro japonštinu nebo hebrejštinu).

Doporučujeme, aby byl počet variant jazyk nízké, údržby a lepšího výkonu.

**Postup přidání firemního brandingu na adresáře:**

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. Na **uživatelů a skupin** vyberte **firemní branding**.
4. Na **uživatelé a skupiny - firemní branding** okně, vyberte **přidat jazyk** příkaz.

    ![Přidání brandingu elementy jazyka](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Upravte prvky, které chcete přizpůsobit. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazí.

## <a name="next-steps"></a>Další kroky
[Přidání firemního brandingu na přihlašovací stránce](active-directory-branding-custom-signon-azure-portal.md)
