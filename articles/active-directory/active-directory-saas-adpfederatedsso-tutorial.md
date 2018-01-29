---
title: "Kurz: Azure Active Directory integrace s Federovanému ADP | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ADP federovaného jednotného přihlašování k."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: d2049e3bc8a35792703d06374263466ddc35b5ce
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-adp-federated-sso"></a>Kurz: Azure Active Directory integrace s Federovanému ADP

V tomto kurzu zjistěte, jak integrovat ADP federovaného jednotného přihlašování k službě Azure Active Directory (Azure AD).

ADP federovaného jednotného přihlašování k integraci s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k Federovanému ADP.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k ADP Federovanému (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Federovanému ADP, potřebujete následující položky:

- Předplatné služby Azure AD
- ADP Federovanému povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Federovanému ADP z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adp-federated-sso-from-the-gallery"></a>Přidání Federovanému ADP z Galerie
Při konfiguraci integrace Federovanému ADP do služby Azure AD potřebujete přidat Federovanému ADP z Galerie si na seznam spravovaných aplikací SaaS.

**Chcete-li přidat Federovanému ADP z galerie, proveďte následující kroky:**

1.  Přihlaste se k prostředí zprostředkovatele identity Microsoft Azure jako správce.

2. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

3. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
4. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

5. Do vyhledávacího pole zadejte **Federovanému ADP**, vyberte **Federovanému ADP** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![ADP Federovanému v seznamu výsledků](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ADP federovaného jednotného přihlašování k podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ADP federovaného jednotného přihlašování k je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Federovanému ADP musí navázat.

V ADP Federovanému přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování federovaného ADP, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit uživatele s ADP federovaného jednotného přihlašování k testu](#create-an-adp-federated-sso-test-user)**  – Pokud chcete mít protějšek Britta Simon v ADP federovaného jednotného přihlašování k propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v ADP federovaného jednotného přihlašování k aplikaci.

**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování federovaného ADP, proveďte následující kroky:**

1. Na portálu Azure na **ADP federovaného jednotného přihlašování k** stránky integrace aplikací, klikněte na **karta Vlastnosti** a proveďte následující kroky: 

    ![Vlastnosti přihlášení](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_prop.png)

    a. Nastavte **povolit pro uživatele k přihlášení** pole hodnotu **Ano**.

    b. Kopírování **adresu URL pro přístup uživatelů** a vložte jej do **části Adresa URL přihlašování konfigurovat**, což je vysvětleno později v tomto kurzu.

    c. Nastavte **přiřazení uživatelských požadované** pole hodnotu **Ano**.

    d. Nastavte **viditelný pro uživatele** pole hodnotu **ne**.

2. Klikněte na tlačítko **jednotného přihlašování** na **Federovanému ADP** stránky integrace aplikace.

    ![Konfigurace propojení přihlášení][4]

3. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_samlbase.png)

4. Na **ADP federovaného jednotného přihlašování k doméně a adresy URL** část, proveďte následující kroky:

    ![ADP federovaného jednotného přihlašování k doméně a adresy URL jednotné přihlašování informace](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_url.png)

    V **identifikátor** textovému poli, zadejte adresu URL:`https://fed.adp.com/` 
    
5. Aplikace Federovanému ADP očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML. Následující snímek obrazovky ukazuje příklad pro tento. Název deklarací bude vždy **"PersonImmutableID"** a hodnoty, které jsme mít mapované na **employeeid**. 

    Zde bude provedeno namapování uživatele z Azure AD na Federovanému ADP na **employeeid** , ale to můžete namapovat na jinou hodnotu, na základě svého nastavení aplikace. Proto prosím práci s [tým podpory ADP](https://www.adp.com/contact-us/overview.aspx) nejprve k použijte správný identifikátor uživatele a mapování danou hodnotu s **"PersonImmutableID"** deklarace identity.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_attribute.png)

6. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:
    
    | Název atributu | Hodnota atributu |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.EmployeeID |
    
    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.

    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.
    
    d. Klikněte na tlačítko **OK**.

    > [!NOTE] 
    > Před konfigurací kontrolního výrazu SAML, budete muset kontaktovat vaše [tým podpory ADP](https://www.adp.com/contact-us/overview.aspx) a požadovat hodnotu atributu jedinečný identifikátor pro vašeho klienta. Je nutné tuto hodnotu ke konfiguraci vlastních deklarací identity pro vaši aplikaci. 

7. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_certificate.png) 

8. Konfigurace jednotného přihlašování na **Federovanému ADP** straně, budete muset nahrát stažené **soubor XML s metadaty** na [ADP federovaného jednotného přihlašování k webu](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Tento proces může trvat několik dní. 

### <a name="configure-your-adp-services-for-federated-access"></a>Konfigurace vaší ADP služeb pro federovaný přístup

>[!Important]
> Vaši zaměstnanci, kteří vyžadují federovaný přístup k vašim službám ADP musí být přiřazena k aplikaci služby ADP a následně, uživatelé musí znovu přiřadit ke konkrétní službě ADP.
Po obdržení potvrzení z vaším zástupcem ADP nakonfigurujte pro řízení přístupu uživatelů ke konkrétní službě ADP ADP služeb a přiřadit a spravovat uživatele.

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Federovanému ADP**, vyberte **Federovanému ADP** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![ADP Federovanému v seznamu výsledků](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addservicegallery.png)

5. Na portálu Azure na vaše **Federovanému ADP** stránky integrace aplikací, klikněte na **karta Vlastnosti** a proveďte následující kroky:  

    ![Linkedproperties přihlášení](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedproperties.png)

    a.  Nastavte **povolit pro uživatele k přihlášení** pole hodnotu **Ano**.

    b.  Nastavte **přiřazení uživatelských požadované** pole hodnotu **Ano**.

    c.  Nastavte **viditelný pro uživatele** pole hodnotu **Ano**.

6. Klikněte na tlačítko **jednotného přihlašování** na **Federovanému ADP** stránky integrace aplikace.

    ![Konfigurace propojení přihlášení][4]

7. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **propojené přihlášení**. propojení aplikace **Federovanému ADP**.

    ![Jednotné přihlašování propojené](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linked.png)

8. Přejděte na **konfigurovat přihlašovací adresa URL** část, proveďte následující kroky:

    ![Prop přihlášení](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedsignon.png)
                                                              
    a. Vložení **adresu URL pro přístup uživatelů**, který jste zkopírovali z výše **karta Vlastnosti** (z hlavní aplikace Federovanému ADP).
                                                             
    b. Následují 5 aplikace, které podporují různé **adresy URL stavu předávání**. Budete muset připojit příslušné **předávání stavu URL** ručně na hodnotu pro konkrétní aplikaci **adresu URL pro přístup uživatelů**.
    
    * **Nyní ADP pracovníků**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **Zaměstnanci ADP teď rozšířené čas**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Uložit** změny.

10. Po obdržení potvrzení z vaším zástupcem ADP spusťte test s uživateli jednu nebo dvě.

    a. Několik uživatelů přiřadíte ke službě ADP aplikaci otestovat federovaný přístup.

    b. Test je úspěšný, když uživatelé přístup k aplikaci služby ADP v galerii a můžete k jejich ADP služby.
 
11. Na potvrzení úspěšné testu přiřaďte federované služby ADP na jednotlivé uživatele nebo skupiny uživatelů, které se vysvětluje dále v tomto kurzu a zavedení zaměstnancům. 

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-adp-federated-sso-test-user"></a>Vytvořit uživatele s ADP federovaného jednotného přihlašování k testu

Cílem této části je vytvoření uživatele volal Britta Simon v ADP federovaného jednotného přihlašování k. Práce s [tým podpory ADP](https://www.adp.com/contact-us/overview.aspx) přidat uživatele do ADP federovaného jednotného přihlašování k účtu.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon k používání Azure jednotné přihlašování v ADP federovaného jednotného přihlašování k udělení přístupu.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon Federovanému ADP, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Federovanému ADP**.

    ![Odkaz Federovanému ADP v seznamu aplikací](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Federovanému ADP na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ADP federovaného jednotného přihlašování k.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_203.png

