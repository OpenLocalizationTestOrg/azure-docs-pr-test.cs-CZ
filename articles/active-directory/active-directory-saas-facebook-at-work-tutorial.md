---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: fd19b3f178a2aee7dd2f204d6d3cf6df8fe6b444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.

V tomto kurzu zjistíte, jak toointegrate síti na pracovišti ve službě Facebook se službou Azure Active Directory (Azure AD).

Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooWorkplace ve službě Facebook.
- Můžete povolit uživatelům tooautomatically získat podepsaný na tooWorkplace Facebook (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě: hello portálu Azure.

Další podrobnosti o softwaru, služba (SaaS) aplikace integraci s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, je třeba hello následující položky:

- Předplatné služby Azure AD
- Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidejte síti na pracovišti ve službě Facebook z Galerie hello.
2. Konfigurace a otestování Azure AD jednotné přihlašování.

## <a name="add-workplace-by-facebook-from-hello-gallery"></a>Přidat pracovní ploše ve službě Facebook z Galerie hello
tooconfigure hello integrace síti na pracovišti ve službě Facebook do Azure AD, přidejte síti na pracovišti ve službě Facebook hello Galerie tooyour seznamu spravovaných aplikací SaaS.

1. V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, vyberte **Azure Active Directory**. 

    ![tlačítko Azure Active Directory Hello][1]

2. Procházet příliš**podnikové aplikace, které** > **všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd hello novou aplikaci, vyberte **novou aplikaci** hello nahoře dialogového okna hello.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**a vyberte **síti na pracovišti ve službě Facebook** z výsledků. Potom vyberte **přidat**, tooadd hello aplikace.

    ![Síti na pracovišti ve službě Facebook v seznamu výsledků hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se síti na pracovišti ve službě Facebook, podle testovacího uživatele názvem "Britta Simon."

Azure AD musí pro jednotné přihlašování toowork tooknow hello příslušného uživatele v síti na pracovišti ve službě Facebook se tooa uživatele ve službě Azure AD. Jinými slovy je třeba vytvořit propojené vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti ve službě Facebook.

Vytvořit tento vztah přiřazením hello hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v síti na pracovišti ve službě Facebook.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části Povolení jednotného přihlašování Azure AD v hello portál Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.

1. V portálu Azure, na hello hello **síti na pracovišti ve službě Facebook** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. V hello **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. V hello **síti na pracovišti Facebook domény a adresy URL** část, hello následující:

    a. V hello **přihlašovací adresa URL** textového pole zadejte adresu URL, která používá následující vzor hello:`https://<company subdomain>.facebook.com`

    b. V hello **identifikátor** textového pole zadejte adresu URL, která používá následující vzor hello:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Tyto hodnoty jsou pouze příklad. Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor. Kontaktujte hello [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) tooget tyto hodnoty. 

4. V hello **SAML podpisový certifikát** vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Vyberte **Uložit**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. V hello **síti na pracovišti v konfiguraci sítě Facebook** vyberte **pracoviště nakonfigurovat ve službě Facebook** tooopen hello **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka** části.

    ![Síti na pracovišti v konfiguraci sítě Facebook](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. V okně prohlížeče jiný web přihlaste jako správce tooyour síti na pracovišti lokalitou společnosti Facebook.
  
   > [!NOTE] 
   > Jako součást hello procesu ověřování SAML síti na pracovišti, můžete použít řetězce dotazu až too2.5 kilobajtů velikostí v pořadí toopass parametry tooAzure AD.

8. V hello **řídicí panel společnosti**, přejděte toohello **ověřování** kartě.

9. V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu hello.

10. Zadejte hodnoty hello zkopírovaných z hello **síti na pracovišti v konfiguraci sítě Facebook** části hello portálu Azure do odpovídajícího pole hello:

    *   V **SAML URL** textového pole, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure.
    *   V **URL vystavitele SAML** textového pole, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z hello portálu Azure.
    *   V **SAML odhlášení přesměrovat (volitelné)**, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z hello portálu Azure.
    *   Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure hello. Kopírovat obsah hello ho do schránky a pak ji vložit toothe **certifikátu SAML** textové pole.

11. Může být nutné cílovou skupinu hello tooenter adresu URL, příjemce adresy URL a služby ACS (služba Assertion příjemce) adresy URL, které jsou uvedené v části hello **konfigurace SAML** části.

12. Posuňte zobrazení dolní toohello hello oddílu a vyberte **Test jednotného přihlašování k**. Automaticky otevírané okno se zobrazí s přihlašovací stránku hello Azure AD. tooauthenticate, zadejte své přihlašovací údaje jako normální. Ujistěte se, stejně jako hello pracovní účet, kterým jste přihlášeni s je hello hello e-mailovou adresu, která je vrácena zpět z Azure AD.

13. Pokud hello testu se úspěšně dokončil, posuňte toohello dolní části stránky hello a vyberte **Uložit**.

14. Každý, kdo používá síti na pracovišti se teď zobrazí s Azure AD přihlašovací stránka pro ověřování.

Můžete tooconfigure SAML odhlaste se adresa URL, která lze použít toopoint na odhlášení stránku hello Azure AD. Pokud je toto nastavení povolené a nakonfigurované, hello uživatel je již směrovanou toohello síti na pracovišti odhlašovací stránky. Místo toho uživatel hello je přesměrovaného toohello URL, která byla přidána do nastavení odhlášení přesměrování SAML hello.


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello. Po přidání této aplikace z hello **služby Active Directory** > **podnikové aplikace, které** jednoduše vyberte hello **jednotné přihlašování** kartě a přístup hello vložené dokumentace prostřednictvím hello **konfigurace** části dolnímu hello. Další informace o funkci hello embedded dokumentace v hello [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Nakonfigurovat četnost opětovné ověření

Můžete nakonfigurovat tooprompt síti na pracovišti pro kontrolu SAML každý den, tři dny jeden týden, dva týdny, jeden měsíc, nebo vůbec.

> [!NOTE] 
>Hello minimální hodnota hello SAML kontroly na mobilní aplikace nastavena tooone týden.

Můžete taky přinutit SAML obnovit pro všechny uživatele. toodo se použití hello **vyžadovat ověřování SAML pro všechny uživatele nyní** tlačítko.


### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

1. V hello **portál Azure**, v levém podokně text hello, vyberte **Azure Active Directory**.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a vyberte **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, vyberte **přidat**.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. V hello **uživatele** dialogové okno pole, hello následující:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. V hello **název** textového pole, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textového pole, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla**a poznamenejte si ho.

    d. Vyberte **Vytvořit**.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Vytvoření pracovišti Facebook testovací uživatel

V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook. Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná akce pro vás v této části. Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, je vytvořen nový pokusíte-li tooaccess síti na pracovišti ve službě Facebook.

>[!Note]
>Pokud potřebujete toocreate uživatel ručně, obraťte se na hello [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/).

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooWorkplace ve službě Facebook.

![Přiřadit uživatele][200] 

1. V hello Azure aplikace hello portálu, otevřete zobrazení. Přejděte toohello directory zobrazení, přejděte příliš**podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **síti na pracovišti ve službě Facebook**.

    ![Hello síti na pracovišti podle propojení sítě Facebook v seznamu aplikace hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. V nabídce hello na levé straně hello vyberte **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. Vyberte **Přidat**. Potom v hello **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.

    ![Podokno Přidat přidružení Hello][203]

5. V hello **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelé hello.

6. V hello **uživatelů a skupin** dialogové okno, vyberte **vyberte**.

7. V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.
Další informace najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Další kroky

* V tématu hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md).
* Čtení [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).
* Další informace o příliš[konfiguraci zřizování uživatelů](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

