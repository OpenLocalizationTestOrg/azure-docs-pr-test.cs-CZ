---
title: 'Kurz: Azure Active Directory integrace s Zscaler Beta | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Zscaler Beta."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1471c2b51ca5684a11acd40f4e450521605bb786
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Kurz: Azure Active Directory integrace s Zscaler Beta

V tomto kurzu zjistíte, jak toointegrate Zscaler Beta službou Azure Active Directory (Azure AD).

Integrace Zscaler Beta s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooZscaler Beta
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZscaler Beta (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD pomocí Zscaler Beta, je třeba hello následující položky:

- Předplatné služby Azure AD
- Zscaler Beta jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Zscaler Beta z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-zscaler-beta-from-hello-gallery"></a>Přidání Zscaler Beta z Galerie hello
tooconfigure hello integrace Zscaler Beta do Azure AD, je nutné tooadd Zscaler Beta hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Zscaler Beta z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Zscaler Beta**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. Na panelu výsledků hello vyberte **Zscaler Beta**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování pomocí Zscaler Beta podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello ve verzi Zscaler Beta je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello ve verzi Zscaler Beta musí toobe navázat.

Ve verzi Zscaler Beta, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování pomocí Zscaler Beta, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  -tooconfigure hello nastavení proxy serveru v Internet Exploreru
3. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření zkušebního uživatele Zscaler Beta](#creating-a-zscaler-beta-test-user)**  -toohave protějšek Britta Simon Zscaler používání beta verze, která je propojená toohello Azure AD reprezentace uživatele.
5. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
6. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Zscaler Beta.

**tooconfigure Azure AD jednotné přihlašování pomocí Zscaler Beta, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Zscaler Beta** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. Na hello **Zscaler Beta domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    Do pole Adresa URL hello přihlašování zadejte adresu URL hello používá vaše tooyour toosign na uživatele Zscaler Beta aplikace.

    > [!NOTE] 
    > Máte tooupdate tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Zscaler beta verzi klienta](https://www.zscaler.com/company/contact) tooget tuto hodnotu. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_400.png)

6. Na hello **Zscaler Beta konfigurace** klikněte na tlačítko **konfigurace Zscaler Beta** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se tooyour Zscaler Beta společnosti lokality jako správce.

8. V nabídce hello hello nahoře, klikněte na tlačítko **správy**.
   
    ![Správa](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "správy")

9. V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.   
            
    ![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "správu uživatelů a ověřování")

10. V hello **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky hello:   
                
    ![Ověřování](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "ověřování")
   
    a. Vyberte **ověření pomocí SAML Single Sign-On**.

    b. Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.

11. Na hello **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky hello a pak klikněte na **provést**

    ![Jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "jednotného přihlašování")
    
    a. Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **URL hello SAML portál toowhich uživatelů jsou odesílány pro ověřování** textové pole.
    
    b. V hello **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.
    
    c. tooupload stažený certifikát, klikněte na tlačítko **Zscaler pem**.
    
    d. Vyberte **zapnout automatické zřizování SAML**.

12. Na hello **konfigurace ověřování uživatele** dialogové okno proveďte hello následující kroky:

    ![Správa](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "správy")
    
    a. Klikněte na **Uložit**.

    b. Klikněte na tlačítko **aktivovat nyní**.

## <a name="configuring-proxy-settings"></a>Konfigurace nastavení proxy serveru
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a>tooconfigure hello nastavení proxy serveru v Internet Exploreru

1. Spustit **aplikace Internet Explorer**.

2. Vyberte **Možnosti Internetu** z hello **nástroje** nabídku pro otevřete hello **Možnosti Internetu** dialogové okno.   
    
     ![Možnosti Internetu](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Možnosti Internetu")

3. Klikněte na tlačítko hello **připojení** kartě.   
  
     ![Připojení](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "připojení")

4. Klikněte na tlačítko **nastavení místní sítě** tooopen hello **nastavení místní sítě** dialogové okno.

5. V části Proxy server hello proveďte hello následující kroky:   
   
    ![Proxy server](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "Proxy serveru")

    a. Vyberte **použít proxy server pro síť LAN**.

    b. V textovém poli hello adresu, zadejte **gateway.zscalerbeta.net**.

    c. V textovém poli hello Port, zadejte **80**.

    d. Vyberte **Nepoužívat proxy server pro místní adresy**.

    e. Klikněte na tlačítko **OK** tooclose hello **nastavení místní sítě (LAN)** dialogové okno.

6. Klikněte na tlačítko **OK** tooclose hello **Možnosti Internetu** dialogové okno.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-zscaler-beta-test-user"></a>Vytvoření zkušebního uživatele Zscaler Beta

Uživatelé toolog tooenable Azure AD v tooZscaler Beta, musí být zřízená tooZscaler Beta. V případě hello Zscaler beta zřizování je ruční úloha.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure zřizování uživatelů, proveďte následující kroky hello:

1. Přihlaste se tooyour **Zscaler Beta** klienta.

2. Klikněte na tlačítko **správy**.   
   
    ![Správa](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "správy")

3. Klikněte na tlačítko **Správa uživatelů**.   
        
     ![Přidat](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "přidat")

4. V hello **uživatelé** , klikněte na **přidat**.
      
    ![Přidat](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "přidat")

5. V části přidat uživatele hello proveďte hello následující kroky:
        
    ![Přidat uživatele](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "přidat uživatele")
   
    a. Typ hello **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny**a hello **oddělení** platný Azure AD účet chcete tooprovision.

    b. Klikněte na **Uložit**.

> [!NOTE]
> Můžete použít všechny ostatní Zscaler Beta uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Zscaler Beta tooprovision uživatelské účty Azure AD.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooZscaler Beta Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooZscaler Beta, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Zscaler Beta**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Zscaler Beta dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zscaler Beta aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_203.png

