---
title: "Kurz: Azure Active Directory integrace s životního cyklu SCC | Microsoft Docs"
description: "Zjistěte, jak toouse životního cyklu SCC s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: c10c313c5fc157ed70d2ccecfb930a8a765f8444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Kurz: Azure Active Directory integrace s SCC životního cyklu
cílem Hello tohoto kurzu je tooshow hello integrace Azure a SCC životního cyklu.  

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Životního cyklu SCC jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, hello uživatele Azure AD, které jste přiřadili tooSCC životního cyklu bude možné toosingle přihlášení do aplikace hello ve vaší lokalitě společnosti životního cyklu SCC (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [Úvod toohello přístupový Panel](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení integrace hello aplikace pro životní cyklus SCC
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "scénář")

## <a name="enable-hello-application-integration-for-scc-lifecycle"></a>Povolit integraci hello aplikace pro životní cyklus SCC
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro SCC životního cyklu.

**Integrace aplikace hello tooenable pro SCC životního cyklu, proveďte následující kroky hello:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Přidat aplikaci](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **životního cyklu SCC**.
   
    ![Galerie aplikací](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **životního cyklu SCC**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Životní cyklus SCC](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC životního cyklu")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooSCC životního cyklu ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **životního cyklu SCC** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello ** nakonfigurovat jednotné přihlašování ** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooSCC životního cyklu** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "nakonfigurovat jednotné přihlašování")
3. Na hello **konfigurace adresy URL aplikace** stránku hello **přihlašovací adresa URL** textovému poli, zadat adresu URL hello používá vaši uživatelé toosign na tooyour SCC životního cyklu aplikace pomocí hello následující vzor "*https:// BS1.SCC.com/lc7/Welcome/Customer/PICTtest.aspx*"a potom klikněte na **Další**.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "konfigurovat adresu URL aplikace")
4. Na hello **nakonfigurovat jednotné přihlašování v cyklu SCC** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "nakonfigurovat jednotné přihlašování")
5. Předat dál této Metadata souboru tooSCC tým životní cyklus podpory.
   
   >[!NOTE]
   >Jednotné přihlašování má toobe ve hello tým podpory životního cyklu SCC povolené.
   > 
   > 

6. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

V pořadí tooenable Azure AD Uživatelé toolog do SCC životního cyklu musí být zřízená do SCC životního cyklu. Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooSCC životního cyklu.

Při pokusech přiřazený uživatel toolog do SCC životního cyklu SCC životní cyklus účtu se automaticky vytvoří v případě potřeby.

>[!NOTE]
>Můžete použít jakékoli jiné SCC životní cyklus uživatelského účtu vytvoření nástroje nebo rozhraní API poskytované životního cyklu SCC tooprovision AAD uživatelské účty.
> 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign uživatelé tooSCC životního cyklu, proveďte následující kroky hello:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello ** životního cyklu SCC ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
    ![Přiřazení uživatelů](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
    ![Ano](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ano")

Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

