---
title: 'Kurz: Azure Active Directory integrace s Cisco Spark | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Cisco Spark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 386c4fd816095e1c61de01dd1dee1bbd00311a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Kurz: Azure Active Directory integrace s Cisco Spark

V tomto kurzu zjistíte, jak toointegrate Cisco Spark v Azure Active Directory (Azure AD).

Integrace Cisco Spark s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooCisco Spark
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCisco Spark (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Cisco Spark, je třeba hello následující položky:

- Předplatné služby Azure AD
- Cisco Spark jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Cisco Spark z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-cisco-spark-from-hello-gallery"></a>Přidání Cisco Spark z Galerie hello
tooconfigure hello integrace Spark Cisco do Azure AD, je nutné tooadd Cisco Spark hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Spark Cisco z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Cisco Spark**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. Na panelu výsledků hello vyberte **Cisco Spark**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cisco Spark podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Cisco Spark je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Cisco Spark musí toobe navázat.

V Cisco Spark přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s Cisco Spark, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Cisco Spark](#creating-a-cisco-spark-test-user)**  -toohave protějšek Britta Simon v Cisco Spark, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Cisco Spark.

**tooconfigure Azure AD jednotné přihlašování s Cisco Spark, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Cisco Spark** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. Na hello **Cisco Spark domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://web.ciscospark.com/#/signin`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečné identifikátor. Obraťte se na [tým podpory Cisco Spark klienta](https://support.ciscospark.com/) tooget tuto hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. Aplikací Spark Cisco očekává hello SAML kontrolní výrazy toocontain konkrétní atributy. Nakonfigurujte hello následující atributy pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:
    
    | Název atributu  | Hodnota atributu |
    | --------------- | -------------------- |    
    |   UID    | User.userPrincipalName |   

    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **OK**.

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. Přihlaste se příliš[správu spolupráce cloudu Cisco](https://admin.ciscospark.com/) pomocí svých přihlašovacích údajů správce s úplnými oprávněními.

9. Vyberte **nastavení** a v části hello **ověřování** klikněte na tlačítko **upravit**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. Vyberte **integrovat zprostředkovatele identity 3. stran. (Rozšířené)**  a přejděte toohello další obrazovce.

11. Na hello **importovat Idp Metadata** stránky, buď přetáhněte soubor metadat hello Azure AD na stránku hello nebo použít hello souboru prohlížeče možnost toolocate a nahrát soubor metadat hello Azure AD. Pak vyberte **vyžadovat certifikát podepsaný certifikační autoritou v metadatech (bezpečnější)** a klikněte na tlačítko **Další**. 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. Vyberte **Test jednotného přihlašování k připojení**a když se otevře novou kartu prohlížeče, ověřování s Azure AD po přihlášení.

13. Vrátí toohello **správu spolupráce cloudu Cisco** kartu prohlížeče. Pokud byl hello test úspěšný, vyberte **tento test proběhl úspěšně. Možnost jednotného přihlašování** a klikněte na tlačítko **Další**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-cisco-spark-test-user"></a>Vytvoření zkušebního uživatele Cisco Spark

V této části vytvoříte uživatele volal Britta Simon v Cisco Spark. V této části vytvoříte uživatele volal Britta Simon v Cisco Spark.

1. Přejděte toohello [správu spolupráce cloudu Cisco](https://admin.ciscospark.com/) pomocí svých přihlašovacích údajů správce s úplnými oprávněními.

2. Klikněte na tlačítko **uživatelé** a potom **Správa uživatelů**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. V hello **spravovat uživatelská** vyberte **ručně přidat nebo změnit uživatele** a klikněte na tlačítko **Další**.

4. Vyberte **názvy a e-mailová adresa**. Potom vyplňte hello textbox následujícím způsobem:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. V hello **křestní jméno** textovému poli, typ **Britta**. 
    
    b. V hello **příjmení** textovému poli, typ **Simon**.
    
    c. V hello **e-mailová adresa** textovému poli, typ  **britta.simon@contoso.com** .

5. Klikněte na tlačítko hello plus tooadd Britta Simon přihlásit. Pak klikněte na **Další**.

6. V hello **přidat služby pro uživatele** okně klikněte na tlačítko **Uložit** a potom **Dokončit**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooCisco Spark Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooCisco Britta Simon Spark, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Cisco Spark**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Když kliknete na dlaždici hello Cisco Spark v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Cisco Spark aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

