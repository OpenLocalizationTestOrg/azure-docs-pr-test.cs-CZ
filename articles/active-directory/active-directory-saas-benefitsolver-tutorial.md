---
title: 'Kurz: Azure Active Directory integrace s Benefitsolver | Microsoft Docs'
description: "Zjistěte, jak toouse Benefitsolver s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 5bb8511ef9be1e386956188a93e899d6ebe56ed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Kurz: Azure Active Directory integrace s Benefitsolver
cílem Hello tohoto kurzu je tooshow hello integrace Azure a Benefitsolver.  

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Benefitsolver jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooBenefitsolver možné toosingle přihlášení do aplikace hello pomocí hello [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení integrace aplikace hello pro Benefitsolver
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "scénář")

## <a name="enabling-hello-application-integration-for-benefitsolver"></a>Povolení integrace aplikace hello pro Benefitsolver
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Benefitsolver.

### <a name="tooenable-hello-application-integration-for-benefitsolver-perform-hello-following-steps"></a>Integrace aplikace hello tooenable pro Benefitsolver, proveďte následující kroky hello:
1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
   ![Aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
   ![Přidat aplikaci](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **Benefitsolver**.
   
   ![Galerie aplikací](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **Benefitsolver**a potom klikněte na **Complete** tooadd hello aplikace.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooBenefitsolver tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.  

Aplikace Benefitsolver očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu saml** konfigurace. 

Hello následující snímek obrazovky ukazuje příklad pro tento.

![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **Benefitsolver** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** Dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooBenefitsolver** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "nakonfigurovat jednotné přihlašování")
3. Na hello **nakonfigurovat nastavení aplikace** proveďte hello následující kroky:
   
   ![Konfigurovat nastavení aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurovat nastavení aplikace")
   
   1. V hello **přihlašovací adresa URL** textovému poli, typ **http://azure.benefitsolver.com**.
   2. V hello **adresa URL odpovědi** textovému poli, typ **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  
   3. Klikněte na **Další**.
4. Na hello **nakonfigurovat jednotné přihlašování v Benefitsolver** stránky, toodownload metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat hello místně na vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "nakonfigurovat jednotné přihlašování")
5. Odešlete hello stáhnout metadata souboru tooyour Benefitsolver tým podpory.
   
   >[!NOTE]
   >Váš tým podpory Benefitsolver má toodo hello skutečné jednotné přihlašování konfiguraci. Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.
   >

6. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "nakonfigurovat jednotné přihlašování")
7. V nabídce hello hello nahoře, klikněte na tlačítko **atributy** tooopen hello **atributy tokenu SAML** dialogové okno.
   
   ![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "atributy")
8. mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:
   
   ![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")
   
   | Název atributu | Hodnota atributu |
   | --- | --- |
   | ClientID |Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu. |
   | ClientKey |Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu. |
   | LogoutURL |Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu. |
   | Číslo zaměstnance |Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu. |
   
   1. Pro každý řádek dat v tabulce hello výše, klikněte na tlačítko **přidat atribut uživatele**.
   2. V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
   3. V hello **hodnota atributu** textovému poli, hodnota atributu vyberte hello zobrazený pro tento řádek.
   4. Klikněte na **Dokončit**.
9. Klikněte na tlačítko **změny**.

## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů
V pořadí tooenable Azure AD Uživatelé toolog do Benefitsolver musí být zřízená do Benefitsolver.  

V případě hello Benefitsolver zaměstnanec data jsou v aplikaci naplněno prostřednictvím soubor úplné zjišťování z vašeho systému HRIS (obvykle je každou noc).  

>[!NOTE]
>Můžete použít všechny ostatní Benefitsolver uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Benefitsolver tooprovision AAD uživatelské účty. 
> 

## <a name="assigning-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

### <a name="tooassign-users-toobenefitsolver-perform-hello-following-steps"></a>tooassign tooBenefitsolver uživatele, proveďte následující kroky hello:
1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello ** Benefitsolver ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ano")

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

