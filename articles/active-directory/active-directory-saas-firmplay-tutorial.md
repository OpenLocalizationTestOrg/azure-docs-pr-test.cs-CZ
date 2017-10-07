---
title: "Kurz: Azure Active Directory integrace s FirmPlay - hájící zájmy zaměstnanec pro nábor | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FirmPlay - hájící zájmy zaměstnanec pro nábor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f143e0bb8f2a42de880d77e5f033694ce3f09cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Kurz: Azure Active Directory integrace s FirmPlay - hájící zájmy zaměstnanec pro nábor

V tomto kurzu zjistíte, jak toointegrate FirmPlay - hájící zájmy zaměstnanec pro nábor službou Azure Active Directory (Azure AD).

Integrace FirmPlay - hájící zájmy zaměstnanec pro nábor s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooFirmPlay - hájící zájmy zaměstnanec pro nábor
- Vaši uživatelé tooautomatically get přihlášeného tooFirmPlay - hájící zájmy zaměstnanec pro nábor (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s FirmPlay - hájící zájmy zaměstnanec pro nábor, je třeba hello následující položky:

- Předplatné služby Azure AD
- FirmPlay - hájící zájmy zaměstnanec pro jednotné přihlašování v předplatném povolené o přijetí


> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-hello-gallery"></a>Přidání FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie hello
integrace hello tooconfigure FirmPlay - hájící zájmy zaměstnanec pro nábor do služby Azure AD, je nutné tooadd FirmPlay - hájící zájmy zaměstnanec pro nábor hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **FirmPlay - hájící zájmy zaměstnanec pro nábor**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. Na panelu výsledků hello vyberte **FirmPlay - hájící zájmy zaměstnanec pro nábor**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FirmPlay - hájící zájmy zaměstnanec pro nábor je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FirmPlay - hájící zájmy zaměstnanec pro nábor musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v FirmPlay - hájící zájmy zaměstnanec pro nábor.

tooconfigure a testování Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření FirmPlay - hájící zájmy zaměstnanec pro nábor testovacího uživatele](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  -toohave protějšek Britta Simon v FirmPlay: hájící zájmy zaměstnanců, pro který je o přijetí propojené toohello Azure AD reprezentace jí.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v vaší FirmPlay - hájící zájmy zaměstnanec nábor aplikace.

**tooconfigure Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **FirmPlay - hájící zájmy zaměstnanec pro nábor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. Na hello **FirmPlay - hájící zájmy zaměstnanec domény o přijetí a adresy URL** oddílem se v hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<your-subdomain>.firmplay.com/`

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Upozorňujeme, že se nejedná hello skutečné hodnoty. Máte tooupdate tuto hodnotu s hello skutečné přihlásit na adrese URL. Obraťte se na [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) tooget tuto hodnotu. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**. Pak klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu hello ve vašem počítači. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. Na hello **FirmPlay - hájící zájmy zaměstnanec pro přijetí konfigurace** klikněte na tlačítko **konfigurace FirmPlay - hájící zájmy zaměstnanec pro nábor** tooopen **konfigurovat přihlašování**dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) a poskytnout hello následující: 

    • hello Stáhnout **soubor certifikátu**

    • hello **SAML jeden přihlašování adresa URL služby**

    • hello **SAML Entity ID**

    • hello **Sign-Out adresy URL**
  

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>Vytváření FirmPlay - hájící zájmy zaměstnanec pro nábor testovacího uživatele

V této části vytvoříte volal Britta Simon v FirmPlay - hájící zájmy zaměstnanec pro nábor uživatele. Spojte se s [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) tooadd hello uživatele v hello FirmPlay - hájící zájmy zaměstnanec pro nábor platformu.


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte svůj přístup tooFirmPlay - hájící zájmy zaměstnanec pro nábor Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooFirmPlay - hájící zájmy zaměstnanec pro nábor, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **FirmPlay - hájící zájmy zaměstnanec pro nábor**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello FirmPlay - hájící zájmy zaměstnanec pro dlaždici nábor v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FirmPlay - hájící zájmy zaměstnanec nábor aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png