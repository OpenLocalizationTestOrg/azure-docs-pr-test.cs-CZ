---
title: "Kurz: Azure Active Directory integrace s pomůže Scout | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pomáhají Scout."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Kurz: Azure Active Directory integrace s pomůže Scout

V tomto kurzu zjistěte, jak integrovat Scout pomoci s Azure Active Directory (Azure AD).

Integrace s Azure AD pomáhají Scout byste získat následující výhody:

- Ve službě Azure AD můžete řídit, kdo má přístup k nápovědě Scout.
- Uživatelům pomůžou Scout můžete automaticky přihlásit pomocí jednotného přihlašování a účtu uživatele Azure AD.
- Můžete spravovat své účty pomocí nich centrální umístění, portálu Azure.

Další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

K nastavení integrace Azure AD s pomůže Scout, potřebujete následující položky:

- Předplatné služby Azure AD
- Pomůže Scout předplatné, se jednotné přihlašování zapnutý 

> [!NOTE]
> Pokud testujete kroky v tomto kurzu, doporučujeme, abyste si je vyzkoušeli v provozním prostředí.

Doporučení pro testování kroky v tomto kurzu:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. 

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidejte pomůže Scout z galerie.
2. Nastavte a otestujte Azure AD jednotné přihlašování.

## <a name="add-help-scout-from-the-gallery"></a>Přidat pomůže Scout z Galerie
K nastavení integrace Scout pomoci s Azure AD v galerii, přidejte pomůže Scout si na seznam spravovaných aplikací SaaS.

Chcete-li přidat pomůže Scout z galerie:

1. V [portál Azure](https://portal.azure.com), v nabídce vlevo vyberte **Azure Active Directory**. 

    ![Tlačítko Azure Active Directory][1]

2. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Stránku podnikových aplikací][2]
    
3. Chcete-li přidat novou aplikaci, vyberte **novou aplikaci**.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **pomoci Scout**. Ve výsledcích hledání vyberte **pomoci Scout**a potom vyberte **přidat**.

    ![Nápověda Scout v seznamu výsledků](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Nastavte a otestujte Azure AD jednotné přihlašování

V této části můžete nastavit a testu Azure AD jednotné přihlašování s pomůže Scout podle testovacího uživatele s názvem *Britta Simon*.

Pro jednotné přihlašování pro práci Azure AD musí znát příslušného uživatele Azure AD v nápovědě Scout. Je nutné vytvořit vztah propojení mezi uživatele Azure AD a související uživatele v nápovědě Scout.

K navázání vztahu odkaz, v nápovědě Scout pro **uživatelské jméno**, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s pomůže Scout, proveďte následující úlohy:

1. [Nastavení Azure AD jednotné přihlašování](#set-up-azure-ad-single-sign-on). Nastaví uživatele tak, aby tuto funkci používat.
2. [Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user). Testy Azure AD jednotné přihlašování s uživatelem Britta Simon.
3. [Vytvoření zkušebního uživatele pomůže Scout](#create-a-help-scout-test-user). Vytvoří protějšek Britta Simon v pomoci Scout, který je propojený s Azure AD reprezentace uživatele.
4. [Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user). Nastaví Britta Simon používat Azure AD jednotného přihlašování.
5. [Test jednotného přihlašování](#test-single-sign-on). Ověřuje, že konfigurace funguje.

### <a name="set-up-azure-ad-single-sign-on"></a>Nastavení Azure AD jednotné přihlašování

V této části můžete nastavit Azure AD jednotného přihlašování na portálu Azure. Potom nastavíte jednotné přihlašování v aplikaci pomůže Scout.

Nastavení Azure AD jednotné přihlašování s Scout pomáhají:

1. Na portálu Azure na **pomoci Scout** stránky integrace aplikací, vyberte **jednotného přihlašování**.
 
    ![Nastavit odkaz přihlášení][4]

2. Na **jednotného přihlašování** stránky, pro **režimu**, vyberte **na základě SAML přihlašování**.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. V části **pomoci Scout domény a adresy URL**, pokud chcete nastavit aplikaci v režimu spouštěná IDP, dokončení následujících kroků:

    1. V **identifikátor** zadejte adresu URL, která má následující vzoru:`urn:auth0:helpscout:<instancename>`

    2. V **adresa URL odpovědi** zadejte adresu URL, která má následující vzoru:`https://helpscout.auth0.com/login/callback?connection=<instancename>`

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Pokud chcete nastavit aplikaci v režimu spouštěná SP, vyberte **zobrazit upřesňující nastavení adresy URL** zaškrtněte políčko a potom proveďte následující:

    * V **přihlásit na adrese URL** zadejte adresu URL, která má následující formát:`https://secure.helpscout.net/members/login/`

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > Hodnoty v těchto adres URL jsou pouze jako ukázka. Aktualizujte hodnoty s skutečného identifikátoru adresy URL a adresa URL odpovědi. Chcete-li získat tyto hodnoty, obraťte se na [tým podpory pomůže Scout](mailto:help@helpscout.com). 

5. V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Vyberte **Uložit**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. Nastavení jednotného přihlašování na straně pomůže Scout, pošlete stažené metadata souboru XML na [tým podpory pomůže Scout](mailto:help@helpscout.com). Tým podpory pomůže Scout platí toto nastavení tak, aby SAML jeden přihlašování připojení je správně nastavena na obou stranách.

> [!TIP]
> Můžete si přečíst stručným verzi tyto pokyny v [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace! Po přidání aplikace tak, že vyberete **služby Active Directory** > **podnikové aplikace, které**, vyberte **jednotné přihlašování** kartě. Dostanete embedded dokumentaci v **konfigurace** části, v dolní části stránky. Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

V této části portálu Azure můžete vytvořit testovacího uživatele s názvem Britta Simon.

![Vytvořit testovací uživatele Azure AD][100]

Vytvoření zkušebního uživatele ve službě Azure AD:

1. Na portálu Azure v levé nabídce vyberte **Azure Active Directory**.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, vyberte **uživatelů a skupin**a potom vyberte **všichni uživatelé**.

    ![Vyberte uživatele a skupiny a pak vyberte možnost Všichni uživatelé](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, v horní části **všichni uživatelé** vyberte **přidat**.

    ![Tlačítko Přidat](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    1. V **název** zadejte **BrittaSimon**.

    2. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    3. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    4. Vyberte **Vytvořit**.

        ![Dialogové okno uživatele](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Vytvoření zkušebního uživatele pomůže Scout

Objekt v této části je vytvoření uživatele s názvem Britta Simon v nápovědě Scout. Nápověda Scout podporuje za běhu (JIT) zřizování, který je ve výchozím nastavení zapnutá.

V této části se nevyžaduje žádné akce ani na dokončení úlohy. Pokud uživatel v nápovědě Scout ještě neexistuje, vytvoří se nový při pokusu o přístup k nápovědě Scout.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolit uživatele Britta Simon používat Azure AD jednotné přihlašování pomocí udělení přístupu uživatelskému účtu pomůže Scout.

![Přiřadit role uživatele][200] 

Přiřazení Britta Simon Scout pomáhají:

1. Na portálu Azure otevřete zobrazení aplikace a pak přejděte do zobrazení adresáře. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **pomoci Scout**.

    ![Na odkaz Nápověda Scout v seznamu aplikací](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. V nabídce vlevo vyberte **uživatelů a skupin**.

    ![Propojení uživatelů a skupin][202]

4. Vyberte **Přidat**. Potom na **přidat přiřazení** vyberte **uživatelů a skupin**.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** v seznamu uživatelů, vyberte na stránce **Britta Simon**.

6. Na **uživatelů a skupin** vyberte **vyberte**.

7. Na **přidat přiřazení** vyberte **přiřadit**.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.

Když vyberete dlaždici pomůže Scout na přístupovém panelu, můžete by měl být automaticky přihlášeni do vaší aplikace pomůže Scout.

Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

