---
title: "vaše přihlášení stránku hello Azure Active Directory aaaCustomize | Microsoft Docs"
description: "Zjistěte, jak tooadd na stránce firemního brandingu toohello Azure přihlášení"
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
ms.openlocfilehash: 151521e3b9cbc6a438a589735058fbff78443cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a>Přidání firemního brandingu tooyour přihlašovací stránky v hello Azure Active Directory
tooavoid nedorozuměním mnoho společností má tooapply konzistentní vzhled a chování ve všech hello webů a služeb, které spravují. Tato funkce poskytuje Azure Active Directory tak, že umožní toocustomize hello vzhled hello přihlašovací stránka s svoje firemní logo a vlastní barevná schémata. přihlašovací stránku Hello je hello stránka, která se zobrazí při přihlášení tooOffice 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity. Budete používat tuto stránku tooenter přihlašovacích údajů.

Pokud chcete tooshow vaší společnosti značku, barvy a další přizpůsobitelné prvky na této stránce, najdete v části hello následující obrázky toounderstand hello rozdíl mezi oběma prostředími hello.

Následující snímek obrazovky ukazuje příklad stránky přihlašovací hello Office 365 na stolním počítači a Hello **před** přizpůsobení:

![Přihlašovací stránka Office 365 před přizpůsobením](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Následující snímek obrazovky ukazuje příklad stránky přihlašovací hello Office 365 na stolním počítači a Hello **po** přizpůsobení:

![Přihlašovací stránka Office 365 po přizpůsobení](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-hello-sign-in-page"></a>Přizpůsobení přihlašovací stránku hello
Pokud potřebujete přístup založené na prohlížeči tooyour cloudových aplikací a služeb, které vaše organizace předplatila, obvykle použijete přihlašovací stránku hello.

Pokud jste použili změny tooyour přihlašovací stránky, může trvat až hodinu tooan tooappear změny hello.

Přihlašovací stránka ve vaší firemní úpravě se zobrazí jenom tehdy, když službu navštívíte pomocí adresy URL konkrétního klienta, například https://outlook.com/**contoso**.com nebo https://mail.**contoso**.com.

Když službu navštívíte pomocí adresy URL, která se neváže ke konkrétnímu klientu (např: https://mail.office365.com), zobrazí se přihlašovací stránka bez firemní úpravy. V tomto případě se branding zobrazí až potom, co zadáte ID uživatele nebo vyberete dlaždici uživatele.

> [!NOTE]
> * Název domény musí zobrazovat jako "Aktivní" v hello **domén** část hello portálu Azure, ve kterém jste branding nakonfigurovali. Další informace najdete v tématu [přidat vlastní názvy domén](active-directory-domains-add-azure-portal.md).
> * Branding přihlašovací stránky se nepřenáší toohello spotřebitelskou přihlašovací stránku společnosti Microsoft. Pokud se přihlásíte pomocí účtu Microsoft, mohou se zobrazit seznam uživatelských dlaždic vykreslí Azure AD partnerské, ale hello branding vaší organizace se nevztahuje toohello stránky účtu Microsoft přihlásit.
>
>

Na stránku přihlášení hello **zůstat přihlášeni** políčko umožňuje uživatele tooremain, při jejich zavřete a znovu ho otevřete svého prohlížeče přihlášený.

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Na životnost relace to vliv nemá. Můžete skrýt hello zaškrtávací políčko je na hello Azure Active Directory přihlašovací stránky.
Jestli se zobrazí zaškrtávací políčko hello závisí na nastavení hello **zůstat přihlášeni zakázáno**.

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

toohide hello zaškrtávací políčko, nakonfigurujte toto nastavení příliš**Ano**.

> [!NOTE]
> Některé funkce služby SharePoint Online a Office 2010, závisí na uživatele, je možné toocheck toto políčko. Pokud nakonfigurujete toto nastavení toohidden, může se uživatelům zobrazí další a neočekávané výzvy toosign v.
>
>

**tooadd firemního brandingu tooyour adresář:**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. Na hello **uživatelů a skupin** vyberte **firemní branding**.
4. Na hello **uživatelé a skupiny - firemní branding** okně, vyberte hello **upravit** příkaz.

    ![Upravit vlastní branding](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Upravte prvky hello chcete toocustomize. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může to trvat až hodinu tooan pro všechny změny provedené toohello přihlašovací stránka brandingu tooappear.

## <a name="next-steps"></a>Další kroky
[Přidání brandingu firmy konkrétní jazyk](active-directory-branding-localize-azure-portal.md)
