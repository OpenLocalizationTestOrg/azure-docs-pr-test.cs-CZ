---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f71a59527394730757d501a973251dc293fd3683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.

V tomto kurzu zjistíte, jak toointegrate síti na pracovišti ve službě Facebook se službou Azure Active Directory (Azure AD).

Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooWorkplace ve službě Facebook.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkplace ve službě Facebook (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, je třeba hello následující položky:

- Předplatné služby Azure AD
- Firemní síti pomocí sítě Facebook jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání síti na pracovišti ve službě Facebook z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-workplace-by-facebook-from-hello-gallery"></a>Přidání síti na pracovišti ve službě Facebook z Galerie hello
tooconfigure hello integrace síti na pracovišti ve službě Facebook do Azure AD, je nutné tooadd síti na pracovišti ve službě Facebook hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd síti na pracovišti ve službě Facebook z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. Na panelu výsledků hello vyberte **síti na pracovišti ve službě Facebook**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se na pracovišti ve službě Facebook podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v síti na pracovišti ve službě Facebook se tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti ve službě Facebook musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v síti na pracovišti ve službě Facebook.

tooconfigure a testování Azure AD jednotné přihlašování se na pracovišti ve službě Facebook, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Konfiguraci frekvence opětovné ověření](#configuring-reauthentication-frequency)**  -tooconfigure tooprompt síti na pracovišti pro kontrolu SAML.
3. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření pracovišti uživatelem testovací Facebook](#creating-a-workplace-by-facebook-test-user)**  -toohave protějšek Britta Simon v síti na pracovišti ve službě Facebook, která je propojená toohello Azure AD reprezentace uživatele.
5. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
6. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.

**tooconfigure Azure AD jednotné přihlašování se na pracovišti ve službě Facebook, provést hello následující kroky:**

1. V portálu Azure, na hello hello **síti na pracovišti ve službě Facebook** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. Na hello **síti na pracovišti Facebook domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.facebook.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.facebook.com/company/<instancename>`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. Na hello **síti na pracovišti v konfiguraci sítě Facebook** klikněte na tlačítko **pracoviště nakonfigurovat ve službě Facebook** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. V okně prohlížeče jiný web, přihlášení tooyour síti na pracovišti lokalitou Facebook společnosti jako správce.
  
   > [!NOTE] 
   > Jako součást hello procesu ověřování SAML může využívat síti na pracovišti řetězce dotazu až too2.5 kilobajtů velikostí v pořadí toopass parametry tooAzure AD.

8. V hello **řídicí panel společnosti**, přejděte toohello **ověřování** kartě.

9. V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu hello.

10. Vstupní hello hodnoty zkopírovaných z **síti na pracovišti v konfiguraci sítě Facebook** části hello portálu Azure do odpovídajícího pole hello:

    *   V **SAML URL** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.
    *   V **URL vystavitele SAML textbox**, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.
    *   V **přesměrování odhlašovací SAML** (volitelné), vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.
    *   Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure, kopírovat obsah hello ho do schránky a pak ji vložit toothe **certifikátu SAML** textové pole.

11. Může být nutné tooenter hello cílovou skupinu adresu URL, adresa URL příjemce, a adresa URL služby ACS (služba Assertion příjemce) uvedené v části hello **konfigurace SAML** části.

12. Posuňte se toohello dolní části hello a klikněte na tlačítko hello **Test jednotného přihlašování k** tlačítko. Zobrazí se tato výsledky v automaticky otevíraném okně, zobrazí se přihlašovací stránku služby Azure AD. Zadejte přihlašovací údaje v jako normální tooauthenticate. 

    **Řešení potíží:** zajistěte, aby hello e-mailová adresa se vrací zpět z Azure AD je hello stejná hodnota jako hello pracovní účet, které jste se přihlásili.

13. Po úspěšném dokončení testů hello, posuňte toohello dolní části stránky hello a klikněte na tlačítko hello **Uložit** tlačítko.

14. Všichni uživatelé používali síti na pracovišti nyní zobrazí se přihlašovací stránku služby Azure AD pro ověřování.

15. **SAML odhlášení přesměrovat (volitelné)** - 

    Můžete zvolit toooptionally postup konfigurace adresy Url odhlašovací SAML, který lze použít toopoint v Azure AD odhlašovací stránce. Pokud je toto nastavení povolené a nakonfigurované, hello uživatele už nebude směrovanou toohello síti na pracovišti odhlašovací stránce. Místo toho bude uživatel hello přesměrovaného toohello url, která byla přidána do nastavení přesměrování odhlašovací SAML hello.


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="configuring-reauthentication-frequency"></a>Konfiguraci frekvence opětovné ověření

Můžete nakonfigurovat tooprompt síti na pracovišti pro kontrolu SAML každý den, tři dny týdne dvou týdnů, měsíců nebo vůbec.

> [!NOTE] 
>Hello minimální hodnota hello SAML kontroly na mobilní aplikace nastavena tooone týden.

Můžete taky přinutit SAML obnovit pro všechny uživatele pomocí tlačítka hello: vyžadovat ověřování SAML pro všechny uživatele nyní.


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Vytváření pracovišti Facebook testovací uživatel

V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook. Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná akce pro vás v této části. Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, je vytvořen nový pokusíte-li tooaccess síti na pracovišti ve službě Facebook.

>[!Note]
>Pokud potřebujete toocreate uživatel ručně, obraťte se na [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/)

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooWorkplace ve službě Facebook.

![Přiřadit uživatele][200] 

**tooassign tooWorkplace Britta Simon ve službě Facebook, provést hello následující kroky:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **síti na pracovišti ve službě Facebook**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

