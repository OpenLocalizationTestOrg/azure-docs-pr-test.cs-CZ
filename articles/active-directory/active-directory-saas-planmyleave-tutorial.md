---
title: 'Kurz: Azure Active Directory integrace s PlanMyLeave | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: ba418a641b339a0d94a3c7b2596d37fbd88a30c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a>Kurz: Azure Active Directory integrace s PlanMyLeave

V tomto kurzu zjistěte, jak integrovat PlanMyLeave s Azure Active Directory (Azure AD).

Integrace PlanMyLeave s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k PlanMyLeave
- Můžete povolit uživatelům, aby automaticky získat přihlášení k PlanMyLeave (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s PlanMyLeave, potřebujete následující položky:

- Předplatné služby Azure AD
- PlanMyLeave jednotného přihlašování povolené předplatné


> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání PlanMyLeave z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-planmyleave-from-the-gallery"></a>Přidání PlanMyLeave z Galerie
Při konfiguraci integrace PlanMyLeave do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS PlanMyLeave z galerie.

**Pokud chcete přidat PlanMyLeave z galerie, proveďte následující kroky:**

1. V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **PlanMyLeave**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. Na panelu výsledků vyberte **PlanMyLeave**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PlanMyLeave podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PlanMyLeave je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PlanMyLeave musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v PlanMyLeave.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PlanMyLeave, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele PlanMyLeave](#creating-a-planmyleave-test-user)**  – Pokud chcete mít protějšek Britta Simon v PlanMyLeave propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci PlanMyLeave.

**Ke konfiguraci Azure AD jednotné přihlašování s PlanMyLeave, proveďte následující kroky:**

1. Na portálu Azure Management portal na **PlanMyLeave** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno stránky, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. Na **PlanMyLeave domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.planmyleave.com/Login.aspx`
    
    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.planmyleave.com`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou skutečné hodnoty. Budete muset aktualizovat tyto hodnoty se skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory PlanMyLeave](mailto:support@planmyleave.com) k získání těchto hodnot.

4. Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. Na **vytvořit nový certifikát** dialogové okno, klikněte na ikonu kalendáři a vyberte **datum vypršení platnosti**. Pak klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. Na **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. V místní nabídce **certifikát výměny** okně klikněte na tlačítko **OK**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. Na **PlanMyLeave konfigurace** klikněte na tlačítko **konfigurace PlanMyLeave** otevřete **konfigurovat přihlášení** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. V okně prohlížeče jiný web Přihlaste se ke klientovi PlanMyLeave jako správce.

11. Přejděte na **nastavení systému**. Pak na **zabezpečení správy** klikněte na část **nastavení společnosti SAML** .

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. Na **SAML nastavení** klikněte ikonu editor.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. Na **SAML nastavení aktualizace** část, proveďte následující kroky:

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    a.  V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.

    b.  Otevřete soubor stažený certifikátu v poznámkovém bloku kopírovat pouze obsah mezi---začít certifikát a---konec certifikátu---ji do schránky a vložte jej do **certifikát** textové pole.

    c. Nastavit "**je povolit**"do"**Ano**".

    d. Klikněte na **Uložit**.



### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**. 



### <a name="creating-a-planmyleave-test-user"></a>Vytvoření zkušebního uživatele PlanMyLeave

Cílem této části je vytvoření uživatele v PlanMyLeave nazývá Britta Simon. PlanMyLeave podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Vytvoří se nový uživatel během pokusu o přístup k PlanMyLeave, pokud ještě neexistuje.

> [!NOTE]
> Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory PlanMyLeave](mailto:support@planmyleave.com).



### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup k PlanMyLeave Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon PlanMyLeave, proveďte následující kroky:**

1. V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **PlanMyLeave**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici PlanMyLeave na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci PlanMyLeave.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png