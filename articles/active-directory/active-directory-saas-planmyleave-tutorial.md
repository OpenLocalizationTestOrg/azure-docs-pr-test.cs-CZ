---
title: 'Kurz: Azure Active Directory integrace s PlanMyLeave | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: 44a6782e44ef22fc957544960be1742f9eed6e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a>Kurz: Azure Active Directory integrace s PlanMyLeave

V tomto kurzu zjistíte, jak toointegrate PlanMyLeave s Azure Active Directory (Azure AD).

Integrace PlanMyLeave s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooPlanMyLeave
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPlanMyLeave (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s PlanMyLeave tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- PlanMyLeave jednotného přihlašování povolené předplatné


> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání PlanMyLeave z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-planmyleave-from-hello-gallery"></a>Přidání PlanMyLeave z Galerie hello
tooconfigure hello integrace PlanMyLeave do Azure AD, je nutné tooadd PlanMyLeave hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd PlanMyLeave z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **PlanMyLeave**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. Na panelu výsledků hello vyberte **PlanMyLeave**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PlanMyLeave podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PlanMyLeave je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PlanMyLeave musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v PlanMyLeave.

tooconfigure a testu Azure AD jednotné přihlašování s PlanMyLeave, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele PlanMyLeave](#creating-a-planmyleave-test-user)**  -toohave protějšek Britta Simon v PlanMyLeave, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci PlanMyLeave.

**tooconfigure Azure AD jednotné přihlašování s PlanMyLeave, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **PlanMyLeave** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno stránky, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. Na hello **PlanMyLeave domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.planmyleave.com/Login.aspx`
    
    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.planmyleave.com`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty s hello skutečné přihlášení adresy URL a identifikátor. Obraťte se na [tým podpory PlanMyLeave](mailto:support@planmyleave.com) tooget tyto hodnoty.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**. Pak klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. Na hello **PlanMyLeave konfigurace** klikněte na tlačítko **konfigurace PlanMyLeave** tooopen **konfigurovat přihlášení** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. V okně prohlížeče jiný web Přihlaste se ke klientovi PlanMyLeave jako správce.

11. Přejděte příliš**nastavení systému**. Pak na hello **zabezpečení správy** klikněte na část **nastavení společnosti SAML** .

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. Na hello **SAML nastavení** klikněte ikonu editor.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. Na hello **nastavení SAML aktualizace** část, proveďte následující kroky hello:

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    a.  V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello z **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.

    b.  V poznámkovém bloku otevřete soubor stažený certifikátu, kopírovat pouze hello obsah mezi hello---začít certifikát a---End certifikát---ho do schránky a pak ji vložit toohello **certifikát** textové pole.

    c. Nastavení"**je povolit**"příliš"**Ano**".

    d. Klikněte na **Uložit**.



### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 



### <a name="creating-a-planmyleave-test-user"></a>Vytvoření zkušebního uživatele PlanMyLeave

Hello cílem této části je toocreate volal Britta Simon v PlanMyLeave uživatele. PlanMyLeave podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud ještě neexistuje, se během pokusu o tooaccess PlanMyLeave vytvoří nového uživatele.

> [!NOTE]
> Pokud potřebujete toocreate uživatelé ručně, je nutné toocontact [tým podpory PlanMyLeave](mailto:support@planmyleave.com).



### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooPlanMyLeave svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooPlanMyLeave, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **PlanMyLeave**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici PlanMyLeave hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour PlanMyLeave aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png