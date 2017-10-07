---
title: "Kurz: Azure Active Directory integrace s pomůže Scout | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pomáhají Scout."
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
ms.openlocfilehash: 58edd140eb1eb5980796ca743b5f7acd891729a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Kurz: Azure Active Directory integrace s pomůže Scout

V tomto kurzu zjistíte, jak pomoci toointegrate Scout službou Azure Active Directory (Azure AD).

Můžete získat hello z integrace s Azure AD pomáhají Scout následující výhody:

- Ve službě Azure AD můžete řídit, kdo má přístup tooHelp Scout.
- Vaši uživatelé tooHelp Scout můžete automaticky přihlásit pomocí jednotného přihlašování a účtu uživatele Azure AD.
- Můžete spravovat své účty pomocí nich centrální umístění, hello portálu Azure.

toolearn Další informace o softwaru, služba (SaaS) aplikace integraci s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooset až integrace Azure AD s pomůže Scout, je třeba hello následující položky:

- Předplatné služby Azure AD
- Pomůže Scout předplatné, se jednotné přihlašování zapnutý 

> [!NOTE]
> Pokud testujete hello kroky v tomto kurzu, doporučujeme, abyste si je vyzkoušeli v provozním prostředí.

Doporučení pro testování hello kroky v tomto kurzu:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. 

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidejte pomůže Scout z Galerie hello.
2. Nastavte a otestujte Azure AD jednotné přihlašování.

## <a name="add-help-scout-from-hello-gallery"></a>Přidat pomůže Scout z Galerie hello
tooset až hello integrace Scout pomoci s Azure AD v galerii hello přidat pomůže Scout tooyour seznam spravovaných aplikací SaaS.

tooadd pomoci Scout z Galerie hello:

1. V hello [portál Azure](https://portal.azure.com), v levé nabídce text hello, vyberte **Azure Active Directory**. 

    ![tlačítko Azure Active Directory Hello][1]

2. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![stránka aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, vyberte **novou aplikaci**.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **pomoci Scout**. Ve výsledcích hledání hello, vyberte **pomoci Scout**a potom vyberte **přidat**.

    ![Nápověda Scout v seznamu výsledků hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Nastavte a otestujte Azure AD jednotné přihlašování

V této části můžete nastavit a testu Azure AD jednotné přihlašování s pomůže Scout podle testovacího uživatele s názvem *Britta Simon*.

Pro toowork jeden přihlašování musí Azure AD tooknow hello Azure AD příslušného uživatele v pomoci Scout. Je nutné vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské hello v nápovědě Scout.

tooestablish hello propojení vztahu v rámci pomoci Scout pro **uživatelské jméno**, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD.

tooconfigure a testování Azure AD jednotné přihlašování s pomůže Scout dokončení hello následující úlohy:

1. [Nastavení Azure AD jednotné přihlašování](#set-up-azure-ad-single-sign-on). Nastaví uživatele toouse tuto funkci.
2. [Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user). Testy Azure AD jednotné přihlašování s uživatelem hello Britta Simon.
3. [Vytvoření zkušebního uživatele pomůže Scout](#create-a-help-scout-test-user). Vytvoří protějšek Britta Simon v pomoci Scout, který je propojený toohello reprezentace hello uživatele Azure AD.
4. [Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user). Nastaví Britta Simon toouse Azure AD jednotné přihlašování.
5. [Test jednotného přihlašování](#test-single-sign-on). Ověřuje, že konfigurace hello funguje.

### <a name="set-up-azure-ad-single-sign-on"></a>Nastavení Azure AD jednotné přihlašování

V této části můžete nastavit Azure AD jednotné přihlašování v hello portálu Azure. Potom nastavíte jednotné přihlašování v aplikaci pomůže Scout.

tooset do Azure AD jednotné přihlašování s pomůže Scout:

1. V portálu Azure, na hello hello **pomoci Scout** stránky integrace aplikací, vyberte **jednotného přihlašování**.
 
    ![Nastavit odkaz přihlášení][4]

2. Na hello **jednotného přihlašování** stránky, pro **režimu**, vyberte **na základě SAML přihlašování**.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. V části **pomoci Scout domény a adresy URL**, pokud chcete tooset si aplikace hello v rozšíření IDP spouštěná režimu dokončení hello následující kroky:

    1. V hello **identifikátor** zadejte adresu URL, která má následující vzor hello:`urn:auth0:helpscout:<instancename>`

    2. V hello **adresa URL odpovědi** zadejte adresu URL, která má následující vzor hello:`https://helpscout.auth0.com/login/callback?connection=<instancename>`

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Pokud chcete tooset až hello aplikace v režimu spouštěná SP, vyberte hello **zobrazit upřesňující nastavení adresy URL** zaškrtněte políčko a potom hello následující:

    * V hello **přihlásit na adrese URL** zadejte adresu URL, která má hello následující formát:`https://secure.helpscout.net/members/login/`

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > Hello hodnoty v těchto adres URL jsou pouze jako ukázka. Aktualizujte hodnoty hello s hello skutečného identifikátoru adresy URL a adresa URL odpovědi. Obraťte se na tooget tyto hodnoty [tým podpory pomůže Scout](mailto:help@helpscout.com). 

5. V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Vyberte **Uložit**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. tooset až jedním přihlašování na straně hello pomůže Scout, odesílat hello stáhnout metadata XML soubor toohello [tým podpory pomůže Scout](mailto:help@helpscout.com). tým podpory pomůže Scout Hello platí toto nastavení tak, aby hello SAML jednoho přihlášení připojení je správně nastavena na obou stranách.

> [!TIP]
> Můžete si přečíst stručným verzi tyto pokyny v hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace! Po přidání aplikace hello výběrem **služby Active Directory** > **podnikové aplikace, které**, vyberte hello **jednotné přihlašování** kartě. Můžete získat přístup k dokumentaci hello vložených v hello **konfigurace** oddíl na hello dolní části stránky hello. Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

V této části v hello portál Azure můžete vytvořit testovacího uživatele s názvem Britta Simon.

![Vytvořit testovací uživatele Azure AD][100]

toocreate testovacího uživatele ve službě Azure AD:

1. V hello portál Azure, v levé nabídce hello, vyberte **Azure Active Directory**.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, vyberte **uživatelů a skupin**a potom vyberte **všichni uživatelé**.

    ![Vyberte uživatele a skupiny a pak vyberte možnost Všichni uživatelé](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, hello horní části hello **všichni uživatelé** vyberte **přidat**.

    ![tlačítko Přidat Hello](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno, dokončení hello následující kroky:

    1. V hello **název** zadejte **BrittaSimon**.

    2. V hello **uživatelské jméno** zadejte hello e-mailovou adresu uživatele Britta Simon.

    3. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    4. Vyberte **Vytvořit**.

        ![Dialogové okno uživatelského Hello](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Vytvoření zkušebního uživatele pomůže Scout

objekt Hello této části je toocreate uživatele s názvem Britta Simon v nápovědě Scout. Nápověda Scout podporuje za běhu (JIT) zřizování, který je ve výchozím nastavení zapnutá.

V této části je toocomplete žádná akce nebo úloh. Pokud uživatel v nápovědě Scout ještě neexistuje, nový vytvoří při pokusíte tooaccess pomoci Scout.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části můžete povolit uživateli hello Britta Simon toouse Azure AD jednotné přihlašování, poskytněte hello uživatele účtu přístup tooHelp Scout.

![Přiřadit role uživatele hello][200] 

tooassign tooHelp Britta Simon Scout:

1. V hello portálu Azure otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **pomoci Scout**.

    ![Hello pomůže Scout odkaz v seznamu aplikace hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. V levé nabídce hello, vyberte **uživatelů a skupin**.

    ![Hello uživatelé a skupiny odkaz][202]

4. Vyberte **Přidat**. Potom na hello **přidat přiřazení** vyberte **uživatelů a skupin**.

    ![Podokno Přidat přidružení Hello][203]

5. Na hello **uživatelů a skupin** v hello seznam uživatelů, vyberte na stránce **Britta Simon**.

6. Na hello **uživatelů a skupin** vyberte **vyberte**.

7. Na hello **přidat přiřazení** vyberte **přiřadit**.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.

Když vyberete hello pomůže Scout dlaždice v hello přístupového panelu, by měl být automaticky přihlášeni tooyour pomoci Scout aplikace.

Další informace o na přístupovém panelu najdete v tématu [Úvod toohello přístupový panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
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

