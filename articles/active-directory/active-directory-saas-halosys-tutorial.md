---
title: 'Kurz: Azure Active Directory integrace s Halosys | Microsoft Docs'
description: "Zjistěte, jak toouse Halosys s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18043ed1b6f7ab45c59cfd36252bef1621618e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Kurz: Azure Active Directory integrace s Halosys

V tomto kurzu zjistíte, jak toointegrate Halosys s Azure Active Directory (Azure AD).

Integrace Halosys s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooHalosys
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHalosys (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Halosys tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Halosys jednotného přihlašování povolené předplatné


> [!NOTE] 
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Halosys z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-halosys-from-hello-gallery"></a>Přidání Halosys z Galerie hello
tooconfigure hello integrace Halosys do Azure AD, je nutné tooadd Halosys hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Halosys z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.

    ![Active Directory][1]
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.

    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.

    ![Aplikace][3]

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.

    ![Aplikace][4]

6. Hello vyhledávacího pole zadejte **Halosys**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. V podokně výsledků hello, vyberte **Halosys**a potom klikněte na **Complete** tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halosys podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Halosys je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Halosys musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Halosys.

tooconfigure a testu Azure AD jednotné přihlašování s Halosys, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Halosys](#creating-a-halosys-test-user)**  -toohave protějšek Britta Simon v Halosys, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Halosys.


**tooconfigure Azure AD jednotné přihlašování s Halosys, proveďte následující kroky hello:**

1. Na portálu classic hello na hello **Halosys** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
     
    ![Konfigurovat jednotné přihlašování][6] 

2. Na hello **jak jste by například uživatelé toosign na tooHalosys** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    a. V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše aplikace Halosys tooyour toosign na uživatele pomocí hello následující vzor: `https://<company-name>.Halosys.com/client-api/api`.

    b.In hello **identifikátoru adresy URL** textovému poli, zadejte adresu URL hello v hello následující vzor: `https://<company-name>.Halosys.com`. 
         
4. Na hello **nakonfigurovat jednotné přihlašování v Halosys** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. tooget jednotné přihlašování, které jsou nakonfigurované pro aplikace, obraťte se na tým podpory Halosys a poskytnout hello následující:

    • hello Stáhnout **soubor metadat**
    
    • hello **URL jednotné přihlašování SAML**
    

6. Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
V této části vytvoříte na portálu classic hello názvem Britta Simon testovacího uživatele.


![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky hello: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png) 

    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.

    c. Klikněte na **Další**.

6.  Na hello **profil uživatele** dialogové okno proveďte následující kroky hello: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png) 

    a. V hello **křestní jméno** textovému poli, typ **Britta**.  

    b. V hello **příjmení** textovému poli, typ, **Simon**.

    c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.

    d. V hello **Role** seznamu, vyberte **uživatele**.

    e. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    a. Poznamenejte si hodnotu hello hello **nové heslo**.

    b. Klikněte na **Dokončit**.   



### <a name="creating-a-halosys-test-user"></a>Vytvoření zkušebního uživatele Halosys

V této části vytvoříte volal Britta Simon v Halosys uživatele. Spojte se s Halosys podporu team tooadd hello uživatelé v platformě Halosys hello.


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooHalosys svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooHalosys, proveďte následující kroky hello:**

1. Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Halosys**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.

    ![Přiřadit uživatele][203]

4. V seznamu uživatelé hello vyberte **Britta Simon**.

5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.

    ![Přiřadit uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Halosys dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Halosys aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
