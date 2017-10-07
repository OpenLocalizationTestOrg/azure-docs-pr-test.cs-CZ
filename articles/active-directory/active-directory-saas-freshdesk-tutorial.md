---
title: 'Kurz: Azure Active Directory integrace s FreshDesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FreshDesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 577a5eb6d9b1bc03030a2b47f63d375869c903bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Kurz: Azure Active Directory integrace s FreshDesk

V tomto kurzu zjistíte, jak toointegrate FreshDesk s Azure Active Directory (Azure AD).

Integrace FreshDesk s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooFreshDesk
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFreshDesk (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s FreshDesk tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- FreshDesk jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání FreshDesk z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-freshdesk-from-hello-gallery"></a>Přidání FreshDesk z Galerie hello
tooconfigure hello integrace FreshDesk do Azure AD, je nutné tooadd FreshDesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd FreshDesk z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **FreshDesk**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. Na panelu výsledků hello vyberte **FreshDesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshDesk podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FreshDesk je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FreshDesk musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v FreshDesk.

tooconfigure a testu Azure AD jednotné přihlašování s FreshDesk, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele FreshDesk](#creating-a-freshdesk-test-user)**  -toohave protějšek Britta Simon v FreshDesk, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci FreshDesk.

**tooconfigure Azure AD jednotné přihlašování s FreshDesk, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **FreshDesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. Na hello **FreshDesk domény a adresy URL** prosím zadejte hello **přihlašovací adresa URL** jako: `https://<tenant-name>.freshdesk.com` nebo jakoukoli jinou hodnotu Freshdesk navrhl.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Upozorňujeme, že se nejedná skutečné hodnoty. Budete muset aktualizovat hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory FreshDesk klienta](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) získat tuto hodnotu.  

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. Na hello **FreshDesk konfigurace** klikněte na tlačítko **konfigurace FreshDesk** tooopen přihlašování konfigurace okna. Zkopírujte hello SAML jednu přihlašování služby adresu URL a adresy URL Sign-Out z hello **Stručná referenční příručka** části.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Freshdesk.

8. V nabídce hello hello nahoře, klikněte na tlačítko **správce**.
   
   ![Správce](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "správce")

9. V hello **obecné nastavení** , klikněte na **zabezpečení**.
   
   ![Zabezpečení](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "zabezpečení")

10. V hello **zabezpečení** část, proveďte následující kroky hello:
   
    ![Jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "jednotné přihlašování")
   
    a. Pro **jednotné přihlašování na (SSO)**, vyberte **na**.

    b. Vyberte **jednotné přihlašování SAML**.

    c. Typ hello **SAML jeden přihlašování adresa URL služby** jste zkopírovali z portálu Azure do hello **SAML přihlašovací adresa URL** textové pole.

    d. Typ hello **Sign-Out URL** jste zkopírovali z portálu Azure do hello **adresy URL odhlašovací** textové pole.

    e. Kopírování hello **kryptografický otisk** z certifikátu hello stáhli z portálu Azure a vložte ji do hello **otisků certifikátu zabezpečení** textové pole.  
 
    >[!TIP]
    >Další podrobnosti najdete v tématu [jak tooretrieve hodnota kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI). 
    
    f. Klikněte na **Uložit**.


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-freshdesk-test-user"></a>Vytvoření zkušebního uživatele FreshDesk

V pořadí tooenable Azure AD Uživatelé toolog do FreshDesk musí být zřízená do FreshDesk.  
V případě hello FreshDesk zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour **Freshdesk** klienta.
2. V nabídce hello hello nahoře, klikněte na tlačítko **správce**.
   
   ![Správce](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "správce")

3. V hello **obecné nastavení** , klikněte na **agenti**.
   
   ![Agenti](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "agentů")

4. Klikněte na tlačítko **nového agenta**.
   
    ![Nový Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "nového agenta.")

5. V dialogovém okně informace o agentovi hello proveďte následující kroky hello:
   
   ![Informace o agentovi](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "informace o agentovi")
   
   a. V hello **úplný název** textovému poli, název typu hello účtu hello Azure AD má tooprovision.

   b. V hello **e-mailu** textovému poli, typ hello Azure AD e-mailová adresa účtu hello Azure AD má tooprovision.

   c. V hello **název** textovému poli, název typu hello účtu hello Azure AD má tooprovision.

   d. Vyberte **agenti role**a potom klikněte na **přiřadit**.
       
   e. Klikněte na **Uložit**.     
   
    >[!NOTE]
    >Držitel účtu Azure AD Hello dostane e-mailu, který zahrnuje účet odkaz tooconfirm hello předtím, než se aktivuje. 
    > 
    
    >[!NOTE]
    >Můžete použít všechny ostatní Freshdesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Freshdesk tooprovision AAD uživatelské účty. tooFreshDesk.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooBox svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooFreshDesk, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **FreshDesk**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici FreshDesk hello v hello přístupového panelu, měli byste obdržet přihlašovací stránky tooget přihlášeného tooyour FreshDesk aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

