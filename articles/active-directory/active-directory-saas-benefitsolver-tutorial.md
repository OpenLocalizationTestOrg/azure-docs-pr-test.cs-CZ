---
title: 'Kurz: Azure Active Directory integrace s Benefitsolver | Microsoft Docs'
description: "Další informace o použití Benefitsolver s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: 8a13dd5ebd872f86247158379b28bc291a9c9d83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Kurz: Azure Active Directory integrace s Benefitsolver
Cílem tohoto kurzu je zobrazit integraci Azure a Benefitsolver.  

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Benefitsolver jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace pomocí uživatele Azure AD, které jste přiřadili Benefitsolver [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikace pro Benefitsolver
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "scénář")

## <a name="enabling-the-application-integration-for-benefitsolver"></a>Povolení integrace aplikace pro Benefitsolver
Cílem této části se popisují postup povolení integrace aplikace pro Benefitsolver.

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikací pro Benefitsolver, proveďte následující kroky:
1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
   ![Aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
   ![Přidat aplikaci](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **Benefitsolver**.
   
   ![Galerie aplikací](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "galerii aplikací")
7. V podokně výsledků vyberte **Benefitsolver**a potom klikněte na **Complete** tuto aplikaci přidat.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují, jak uživatelům povolit ověřování na Benefitsolver ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.  

Aplikace Benefitsolver očekává SAML kontrolní výrazy ve specifickém formátu, který můžete přidat mapování vlastní atribut vyžaduje vaše **atributy tokenu saml** konfigurace. 

Následující snímek obrazovky ukazuje příklad pro tento.

![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **Benefitsolver** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit Benefitsolver** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "nakonfigurovat jednotné přihlašování")
3. Na **nakonfigurovat nastavení aplikace** proveďte následující kroky:
   
   ![Konfigurovat nastavení aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurovat nastavení aplikace")
   
   1. V **přihlašovací adresa URL** textovému poli, typ **http://azure.benefitsolver.com**.
   2. V **adresa URL odpovědi** textovému poli, typ **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  
   3. Klikněte na **Další**.
4. Na **nakonfigurovat jednotné přihlašování v Benefitsolver** stránku a stáhnout metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "nakonfigurovat jednotné přihlašování")
5. Odešlete soubor stažený metadat pro váš tým podpory Benefitsolver.
   
   >[!NOTE]
   >Váš tým podpory Benefitsolver musí provést konfiguraci skutečné jednotné přihlašování. Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.
   >

6. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "nakonfigurovat jednotné přihlašování")
7. V nabídce v horní části, klikněte na tlačítko **atributy** otevřete **atributy tokenu SAML** dialogové okno.
   
   ![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "atributy")
8. Pokud chcete přidat mapování požadovaný atribut, proveďte následující kroky:
   
   ![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")
   
   | Název atributu | Hodnota atributu |
   | --- | --- |
   | ClientID |Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver. |
   | ClientKey |Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver. |
   | LogoutURL |Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver. |
   | Číslo zaměstnance |Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver. |
   
   1. Pro každý řádek dat v předchozí tabulce, klikněte na tlačítko **přidat atribut uživatele**.
   2. V **název atributu** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.
   3. V **hodnota atributu** textovému poli, vyberte hodnotu atributu zobrazený pro tento řádek.
   4. Klikněte na **Dokončit**.
9. Klikněte na tlačítko **změny**.

## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů
Pokud chcete povolit uživatelům Azure AD přihlášení do Benefitsolver, musí být zřízená do Benefitsolver.  

V případě Benefitsolver zaměstnanec data jsou v aplikaci naplněno prostřednictvím soubor úplné zjišťování z vašeho systému HRIS (obvykle je každou noc).  

>[!NOTE]
>Můžete použít všechny ostatní Benefitsolver uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Benefitsolver zřídit AAD uživatelské účty. 
> 

## <a name="assigning-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Přiřazení uživatelů k Benefitsolver, proveďte následující kroky:
1. Na portálu Azure classic vytvořte zkušební účet.
2. Na ** Benefitsolver ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

