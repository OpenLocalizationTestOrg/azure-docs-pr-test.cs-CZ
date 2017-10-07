---
title: "Kurz: Azure Active Directory integrace s Proofpoint na vyžádání | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Proofpoint na vyžádání."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 0f9472ddc01f2c18ffc9e8d2b59a17b3b595515e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Kurz: Azure Active Directory integrace s Proofpoint na vyžádání

V tomto kurzu zjistíte, jak toointegrate Proofpoint na vyžádání pomocí Azure Active Directory (Azure AD).

Integrace Proofpoint na vyžádání s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooProofpoint na vyžádání
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooProofpoint na vyžádání (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

integrace tooconfigure Azure AD s Proofpoint na vyžádání, je nutné hello následující položky:

- Předplatné služby Azure AD
- Proofpoint na vyžádání jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Proofpoint na vyžádání z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-proofpoint-on-demand-from-hello-gallery"></a>Přidání Proofpoint na vyžádání z Galerie hello
tooconfigure hello integrace Proofpoint na vyžádání do Azure AD, je nutné tooadd Proofpoint na vyžádání z hello Galerie tooyour seznam spravovaných aplikací SaaS.

**tooadd Proofpoint na vyžádání z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Proofpoint na vyžádání**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. Na panelu výsledků hello vyberte **Proofpoint na vyžádání**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Proofpoint na vyžádání v testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Proofpoint na vyžádání je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Proofpoint na vyžádání musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Proofpoint na vyžádání.

tooconfigure a testování Azure AD jednotné přihlašování s Proofpoint na vyžádání, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Proofpoint na vyžádání testovacího uživatele](#creating-a-proofpoint-on-demand-test-user)**  -toohave protějšek Britta Simon v Proofpoint na vyžádání, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší Proofpoint na vyžádání aplikace.

**tooconfigure Azure AD jednotné přihlašování s Proofpoint na vyžádání, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Proofpoint na vyžádání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. Na hello **Proofpoint na vyžádání domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    a.In hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<hostname>.pphosted.com/ppssamlsp_hostname`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<hostname>.pphosted.com/ppssamlsp`

    c.  V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. Na hello **Proofpoint konfiguraci na vyžádání** klikněte na tlačítko **Proofpoint konfiguraci na vyžádání** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. tooconfigure jednotného přihlašování na **Proofpoint na vyžádání** straně, je nutné stáhnout hello toosend **Certificate(Base64)**,**SAML Entity ID**, a **SAML Jednotné přihlašování adresa URL služby** příliš[Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services).

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. Tyto hodnoty nejsou skutečné hello. Tyto hodnoty aktualizovat pomocí skutečného hello
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **Britta Simon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a>Vytváření Proofpoint na vyžádání testovacího uživatele

V této části vytvoříte uživatele volal Britta Simon v Proofpoint na vyžádání. Práce s [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services) tooadd uživatele v hello Proofpoint na vyžádání platformě.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooProofpoint přístup na vyžádání.

![Přiřadit uživatele][200] 

**tooassign tooProofpoint Britta Simon na vyžádání, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Proofpoint na vyžádání**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na tlačítko hello **Proofpoint na vyžádání** na hello přístupového panelu dlaždici, můžete by měla být na tooyour Proofpoint na vyžádání aplikaci automaticky přihlášeni.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).  

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

