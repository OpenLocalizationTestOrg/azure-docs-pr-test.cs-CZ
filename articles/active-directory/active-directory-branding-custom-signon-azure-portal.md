---
title: "Přizpůsobit přihlašovací stránka ve službě Azure Active Directory | Microsoft Docs"
description: "Informace o postupu přidání firemního brandingu na stránky Azure přihlášení"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 27590c018ea55e9793246c7a4cab10f934ea502b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Přidání firemního brandingu na přihlašovací stránku ve službě Azure Active Directory
Mnoho společností chce předcházet zmatení uživatele a upřednostňuje jednotný vzhled všech webů a služeb, které spravují. Tato funkce poskytuje Azure Active Directory tím, že se můžete přizpůsobit vzhled stránky přihlášení s svoje firemní logo a vlastní barevná schémata. Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity. Budete používat tuto stránku k zadání pověření.

Pokud chcete na této stránce zobrazit značku, barvy a další přizpůsobitelné prvky vaší společnosti, prohlédněte si následující obrázky, abyste pochopili rozdíl mezi oběma prostředími.

Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na stolním počítači **před** přizpůsobením:

![Přihlašovací stránka Office 365 před přizpůsobením](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na stolním počítači **po** přizpůsobení:

![Přihlašovací stránka Office 365 po přizpůsobení](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a>Přizpůsobení přihlašovací stránky
Pokud potřebujete v prohlížeči otevřít cloudové aplikace a služby, které si vaše organizace předplatila, obvykle použijete přihlašovací stránku.

Pokud jste přihlašovací stránku změnili, může se taková změna projevit až za hodinu.

Přihlašovací stránka ve vaší firemní úpravě se zobrazí jenom tehdy, když službu navštívíte pomocí adresy URL konkrétního klienta, například https://outlook.com/**contoso**.com nebo https://mail.**contoso**.com.

Když službu navštívíte pomocí adresy URL, která se neváže ke konkrétnímu klientu (např: https://mail.office365.com), zobrazí se přihlašovací stránka bez firemní úpravy. V tomto případě se branding zobrazí až potom, co zadáte ID uživatele nebo vyberete dlaždici uživatele.

> [!NOTE]
> * Název domény musí zobrazovat jako "Aktivní" v **domén** část portálu Azure, ve kterém jste branding nakonfigurovali. Další informace najdete v tématu [přidat vlastní názvy domén](active-directory-domains-add-azure-portal.md).
> * Branding přihlašovací stránky se nepřenáší na spotřebitelskou přihlašovací stránku Microsoftu. Pokud se přihlásíte pomocí účtu Microsoft, mohou se zobrazit seznam uživatelských dlaždic vykreslí Azure AD partnerské, ale branding vaší organizace nevztahuje na stránku účtu Microsoft přihlásit.
>
>

Na přihlašovací stránce umožňuje zaškrtávací políčko **Zůstat přihlášeni**, aby příslušný uživatel zůstal přihlášen i po zavření a dalším spuštění prohlížeče.

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Na životnost relace to vliv nemá. Příslušné zaškrtávací políčko na přihlašovací stránce služby Azure Active Directory lze skrýt.
Jestli se zobrazí zaškrtávací políčko závisí na nastavení **zůstat přihlášeni zakázáno**.

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

Skrýt políčka, konfigurací tohoto nastavení **Ano**.

> [!NOTE]
> Některé funkce služeb SharePoint Online a Office 2010 závisí na tom, zda uživatelé mohou toto políčko zaškrtnout. Pokud je nastavíte jako skryté, mohou se vašim uživatelům zobrazovat další (neočekávané) výzvy k přihlášení.
>
>

**Postup přidání firemního brandingu na adresáře:**

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. Na **uživatelů a skupin** vyberte **firemní branding**.
4. Na **uživatelé a skupiny - firemní branding** okně, vyberte **upravit** příkaz.

    ![Upravit vlastní branding](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Upravte prvky, které chcete přizpůsobit. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazí.

## <a name="next-steps"></a>Další kroky
[Přidání brandingu firmy konkrétní jazyk](active-directory-branding-localize-azure-portal.md)
