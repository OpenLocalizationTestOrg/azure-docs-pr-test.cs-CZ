---
title: 'Kurz: Azure Active Directory integrace s Velpic SAML | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Velpic SAML."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 613947d8fe95113382a2cdc0f79ce9eda85a0127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Kurz: Azure Active Directory integrace s Velpic SAML

V tomto kurzu zjistíte, jak toointegrate Velpic SAML s Azure Active Directory (Azure AD).

Integrace Velpic SAML s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooVelpic SAML
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooVelpic SAML (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Velpic SAML, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- Velpic SAML jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Velpic SAML z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-velpic-saml-from-hello-gallery"></a>Přidání Velpic SAML z Galerie hello
tooconfigure hello integrace Velpic SAML do Azure AD, je nutné tooadd Velpic SAML hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Velpic SAML z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Velpic SAML**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. Na panelu výsledků hello vyberte **Velpic SAML**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Velpic SAML podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Velpic SAML je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Velpic SAML musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Velpic SAML.

tooconfigure a testu Azure AD jednotné přihlašování s Velpic SAML, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Velpic SAML](#creating-a-velpic-saml-test-user)**  -toohave protějšek Britta Simon v Velpic SAML, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Velpic SAML.

**tooconfigure Azure AD jednotné přihlašování s Velpic SAML, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **Velpic SAML** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. Zadejte podrobnosti hello v hello **domény Velpic SAML a adres URL** části -

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://<sub-domain>.velpicsaml.net`

    b. V hello **identifikátor** textovému poli, vložte hello **'jednotné přihlašování na adresu URL,** hodnota`https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Upozorňujeme, že hello adresa URL přihlašování budou poskytovat hello team Velpic SAML a hodnotu identifikátoru bude k dispozici, když konfigurujete hello jednotného přihlašování k modulu plug-in na straně Velpic SAML. Je nutné toocopy, který ze stránky aplikace Velpic SAML a vložte ji sem.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. Na hello Velpic SAML konfigurační oddíl klikněte na konfigurace SAML Velpic tooopen konfigurovat přihlašování okno. Zkopírujte hello SAML Entity ID z hello Stručná referenční příručka části.

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Velpic SAML.

8. Klikněte na **spravovat** kartě a přejděte příliš**integrace** část, kde je nutné tooclick na **modulů plug-in** tlačítko toocreate nový modul plug-in pro přihlášení.

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. Klikněte na hello **'Přidání modulu plug-in'** tlačítko.
    
    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. Klikněte na hello **SAML** dlaždici stránce Přidat modul plug-in hello.
    
    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. Zadejte název hello hello nové zásuvný modul SAML a klikněte na tlačítko hello **"Přidat"** tlačítko.

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. Zadejte podrobnosti hello následujícím způsobem:

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    a. V hello **název** textovému poli, název typu hello zásuvný modul SAML.

    b. V hello **URL vystavitele** textovému poli, vložte hello **SAML Entity ID** jste zkopírovali ze hello **konfigurovat přihlášení** okno hello portálu Azure.

    c. V hello **zprostředkovatele metadat konfigurace** nahrát hello soubor XML s metadaty souboru, který jste si stáhli z portálu Azure.

    d. Můžete také tooenable SAML jenom při zřizování čas povolením hello **, automaticky vytvoří nové uživatele'** zaškrtávací políčko. Pokud uživatel neexistuje v Velpic a tento příznak není povolen, hello přihlášení z Azure se nezdaří. Pokud příznak hello je povoleno hello uživatele budou automaticky zřídí do Velpic během hello přihlášení. 

    e. Kopírování hello **jednotné přihlašování na adrese URL** z hello textového pole a vložte jej do hello portálu Azure.
    
    f. Klikněte na **Uložit**.

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Vytvoření zkušebního uživatele Velpic SAML

Tento krok se obvykle nevyžaduje jako aplikace hello podporuje jenom při zřizování uživatelů čas. Pokud zřizování hello automatické uživatelů není povolena můžete vytvoření ruční uživatele provádí, jak je popsáno níže.

Přihlaste se k serveru vaší společnosti Velpic SAML jako správce a proveďte následující kroky:
    
1. Klikněte na kartu Správa a přejděte tooUsers části potom klikněte na nové tlačítko tooadd uživatele.

    ![Přidat uživatele](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. Na hello **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky hello.

    ![Uživatel](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    a. V hello **křestní jméno** textovému poli, typ hello křestní jméno Britta Simon.

    b. V hello **příjmení** textovému poli, typ hello příjmení Britta Simon.

    c. V hello **uživatelské jméno** textovému poli, typ hello uživatelské jméno Britta Simon.

    d. V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.

    e. Zbývající informace hello je volitelné, že je v případě potřeby můžete vyplnit.
    
    f. Klikněte na **ULOŽIT**.  

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte svůj přístup tooVelpic SAML Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooVelpic Britta Simon SAML, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Velpic SAML**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

1. Po kliknutí na tlačítko hello Velpic SAML dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku aplikace Velpic SAML. Měli byste vidět hello **"přihlásit se přes Azure AD,** na přihlašovací stránku hello tlačítko.

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. Klikněte na hello **"přihlásit se přes Azure AD,** tlačítko toolog v tooVelpic pomocí účtu Azure AD.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

