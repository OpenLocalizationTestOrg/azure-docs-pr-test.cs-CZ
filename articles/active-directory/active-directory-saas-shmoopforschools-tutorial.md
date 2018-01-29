---
title: "Kurz: Azure Active Directory integrace s Shmoop pro škol | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Shmoop pro škol."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jeedes
ms.openlocfilehash: 48db70834f96adbb7097457caca8489ea1a57da5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-shmoop-for-schools"></a>Kurz: Azure Active Directory integrace s Shmoop pro školy

V tomto kurzu zjistěte, jak integrovat Shmoop pro škol s Azure Active Directory (Azure AD).

Integrace Shmoop pro škol s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, kdo má přístup k Shmoop pro škol.
- Můžete povolit uživatelům automaticky získat přihlášení k Shmoop pro škol s účty služby Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě – portál Azure.

Další informace o integraci aplikací SaaS v Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Shmoop pro školy, potřebujete následující položky:

- Předplatné služby Azure AD
- Shmoop pro škol jednotného přihlašování na povoleno předplatného

> [!NOTE]
> Nedoporučujeme používat produkčním prostředí pro testování kroky v tomto kurzu.

Chcete-li otestovat kroky v tomto kurzu, doporučujeme:

- Pomocí vaše produkční evironment pouze v případě, že je nutné.
- Získávání [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/) Pokud ještě nemáte prostředí zkušební verze Azure AD.

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář, který je popsané v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Shmoop pro škol z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="add-shmoop-for-schools-from-the-gallery"></a>Přidat Shmoop pro škol z Galerie
Při konfiguraci integrace Shmoop pro škol do služby Azure AD, potřebujete přidat Shmoop pro škol z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Shmoop pro škol z galerie, proveďte následující kroky:**

1. V [portál Azure](https://portal.azure.com), v levém podokně, vyberte **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, vyberte **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Shmoop pro škol**. Vyberte **Shmoop pro škol** z výsledků, pak vyberte **přidat** tlačítko Přidat aplikaci.

    ![Shmoop pro škol v seznamu výsledků](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Shmoop pro škol podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, kdo příslušného uživatele v Shmoop pro škol je pro uživatele ve službě Azure AD. Jinými slovy budete muset vytvořit propojení mezi uživatele Azure AD a související uživatelské v Shmoop pro škol.

V Shmoop pro školy, zadejte **uživatelské jméno** hodnotu stejnou hodnotu **uživatelské jméno** ve službě Azure AD. Nyní jste vytvořili odkaz relace.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Shmoop pro školy, proveďte následující stavební bloky:

1. [Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on) umožňující uživatelům používat tuto funkci.
2. [Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user) k testování Azure AD jednotné přihlašování s Britta Simon.
3. [Vytvoření zkušebního uživatele Shmoop pro škol](#create-a-shmoop-for-schools-test-user) tak, aby měl protějšek Britta Simon v Shmoop pro školy, propojené služby Azure AD reprezentace uživatele.
4. [Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user) povolit Britta Simon používat Azure AD jednotné přihlašování.
5. [Test jednotného přihlašování](#test-single-sign-on) k ověření, že konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Shmoop pro škol.

**Ke konfiguraci Azure AD jednotné přihlašování s Shmoop pro školy, proveďte následující kroky:**

1. Na portálu Azure na **Shmoop pro škol** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. V **jednotného přihlašování** dialogové okno, v rozevírací nabídce v části **režimu přihlašování**, vyberte **na základě SAML přihlašování**.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_samlbase.png)

3. V **Shmoop pro škol domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_url.png)

    a. V **přihlašovací adresa URL** pole, zadejte adresu URL pomocí vzoru následující:`https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. V **identifikátor** pole, zadejte adresu URL pomocí vzoru následující:`https://schools.shmoop.com/<uniqueid>`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se [tým podpory Shmoop pro škol klienta](mailto:support@shmoop.com) k získání těchto hodnot. 
 
4. Aplikace Shmoop pro škol očekává SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte následující deklarace identity pro tuto aplikaci. Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace. Následující snímek obrazovky ukazuje, jak nakonfigurovat kontrolních výrazů:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute.png)

    > [!NOTE]
    > Shmoop pro školní podporuje dvě role pro uživatele: **učitele** a **Student**. Nastavte tyto role ve službě Azure AD, aby uživatelé se dají přiřadit na příslušné role. Chcete-li pochopit, jak nakonfigurovat role ve službě Azure AD, přečtěte si téma [řízení přístupu na základě rolí v cloudových aplikacích pomocí služby Azure AD](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/).
    
5. V **uživatelské atributy** tématu **jednotného přihlašování** dialogové okno nakonfigurujte atribut tokenu SAML, jak je vidět na předchozím obrázku.  Proveďte následující kroky:

    | Název atributu | Hodnota atributu |
    | -------------- | --------------- |
    | Role           | User.assignedroles |

    a. Chcete-li otevřít **přidat atribut** dialogové okno, vyberte **přidat atribut**.
    
    ![Konfigurovat jednotné přihlašování ](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_05.png)
    
    b. V **název** zadejte název atributu, který je uvedený na příslušném řádku.
    
    c. Z **hodnotu** vyberte hodnotu atributu, který je uvedený na příslušném řádku.

    d. Ponechte **Namespace** pole prázdné.
    
    e. Vyberte **Ok**.

6. Vyberte tlačítko **Uložit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_400.png)

7. Ke generování **Metadata** adresu URL, proveďte následující kroky:

    a. Vyberte **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_appregistrations.png)
   
    b. Chcete-li otevřít **koncové body** dialogové okno, vyberte **koncové body**.  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_endpointicon.png)

    c. Vyberte tlačítko Kopírovat, pokud chcete zkopírovat **dokument FEDERAČNÍCH METADAT** adresy URL a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_endpoint.png)
     
    d. Přejděte na stránku vlastností **Shmoop pro škol**. Zkopírujte **ID aplikace** pomocí **kopie** tlačítko. Ho vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_appid.png)

    e. Vygenerovat **adresu URL metadat** pomocí následujícího vzorce: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`.   

8. Konfigurace jednotného přihlašování na **Shmoop pro škol** straně, budete muset odeslat **adresu URL metadat** k [tým podpory Shmoop pro škol](mailto:support@shmoop.com).

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com) při k nastavení aplikace. Po přidání této aplikace z **služby Active Directory** > **podnikové aplikace, které** vyberte **jednotné přihlašování** kartě a přístup k vložený dokumentace prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace v [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele volat Britta Simon na portálu Azure.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně, vyberte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**. Potom vyberte **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Vyberte **Vytvořit**.
 
### <a name="create-a-shmoop-for-schools-test-user"></a>Vytvoření zkušebního uživatele Shmoop pro školy

Cílem této části je vytvoření uživatele volal Britta Simon v Shmoop pro škol. Shmoop pro škol podporuje za běhu zřizování, který je ve výchozím nastavení povolené. Neexistuje žádná položka akce pro vás v této části. Pokud nový uživatel ještě neexistuje, vytvoří se při pokusu o přístup k Shmoop pro škol.

>[!NOTE]
>Pokud je potřeba ručně vytvořit uživateli, obraťte se [tým podpory Shmoop pro škol](mailto:support@shmoop.com).

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Shmoop pro škol.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon Shmoop pro školy, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikace. Pak přejděte na **podnikové aplikace, které** v zobrazení adresáře.  Potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Shmoop pro škol**.

    ![V seznamu aplikací na odkaz Shmoop pro školy](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_app.png)  

3. V nabídce na levé straně vyberte **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Vyberte **přidat** tlačítko. Potom v **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.

    ![V podokně Přidat přiřazení][203]

5. V **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelů.

6. V **uživatelů a skupin** dialogové okno, klikněte **vyberte** tlačítko. 

7. V **přidat přiřazení** dialogové okno, vyberte **přiřadit** tlačítko.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.

Když vyberete **Shmoop pro škol** dlaždici na přístupovém panelu budete by měl získat automaticky přihlášení k aplikaci Shmoop pro škol.

Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů pro postup pro integraci aplikací SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_203.png

