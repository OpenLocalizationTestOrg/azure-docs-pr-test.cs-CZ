---
title: 'Kurz: Azure Active Directory integrace s Qualtrics | Microsoft Docs'
description: "Zjistěte, jak toouse Qualtrics s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 941642e74b90bb13a5ce37ce6665cfa32cd86016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Kurz: Azure Active Directory integrace s Qualtrics
cílem Hello tohoto kurzu je tooshow hello integrace Azure a Qualtrics.  

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Qualtrics jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooQualtrics možné toosingle přihlášení do aplikace hello na váš web společnosti Qualtrics (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [toohello Úvod Přístup k panelu](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení integrace aplikace hello pro Qualtrics
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "scénář")

## <a name="enabling-hello-application-integration-for-qualtrics"></a>Povolení integrace aplikace hello pro Qualtrics
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Qualtrics.

**Integrace aplikace hello tooenable pro Qualtrics, proveďte následující kroky hello:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
   ![Aplikace](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
   ![Přidat aplikaci](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **Qualtrics**.
   
   ![Galerie aplikací](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **Qualtrics**a potom klikněte na **Complete** tooadd hello aplikace.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooQualtrics tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooQualtrics** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "nakonfigurovat jednotné přihlašování")
3. Na hello **konfigurace adresy URL aplikace** stránku hello **Qualtrics přihlašovací adresa URL** textovému poli, zadejte adresu URL (například: "*https://ssotest2ut1.qualtrics.com*") a potom klikněte na **Další**.
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurovat adresu URL aplikace")
4. Na hello **nakonfigurovat jednotné přihlašování v Qualtrics** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat hello ve vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "nakonfigurovat jednotné přihlašování")
5. Odešlete hello metadata souboru toohello Qualtrics tým podpory.
   
   >[!NOTE]
   >Konfigurace jednotného přihlašování k Hello má toobe provádí hello Qualtrics tým podpory. Zobrazí se oznámení a také konfigurace hello se dokončila.
   > 
   > 
6. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooQualtrics. Když se uživatel s přiřazenou pokusí toolog do Qualtrics pomocí hello přístupového panelu, Qualtrics ověří, zda existuje hello uživatele.  

Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Qualtrics.

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign tooQualtrics uživatele, proveďte hello následující kroky:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ano")

Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

