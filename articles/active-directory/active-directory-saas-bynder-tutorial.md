---
title: 'Kurz: Azure Active Directory integrace s Bynder | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 6786d7eb6a11405278ef7267f25279f9e39b3bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Kurz: Azure Active Directory integrace s Bynder
Cílem tohoto kurzu je ukazují, jak integrovat Bynder s Azure Active Directory (Azure AD).

Integrace Bynder s Azure AD poskytuje následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup k Bynder
* Můžete povolit uživatelům, aby automaticky získat přihlášení k Bynder jednotné přihlašování (SSO) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Konfigurace integrace Azure AD s Bynder, potřebujete následující položky:

* Předplatné služby Azure AD
* Předplatné povolené Bynder jednotného přihlašování (SSO)

>[!NOTE]
>K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí. 
> 

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
Cílem tohoto kurzu je vám umožní Microsoft Azure AD jednotného přihlašování k testování v testovacím prostředí.

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Bynder z Galerie
2. Konfigurace a testování jednotného přihlašování pro aplikaci Microsoft Azure AD

## <a name="add-bynder-from-the-gallery"></a>Přidat Bynder z Galerie
Při konfiguraci integrace Bynder do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Bynder z galerie.

**Pokud chcete přidat Bynder z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 
   
    ![Active Directory][1]
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace][2]
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3]
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Aplikace][4]
6. Do vyhledávacího pole zadejte **Bynder**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. Na panelu výsledků vyberte **Bynder**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Výběr aplikace v galerii](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>Konfigurace a otestování jednotného přihlašování pro aplikaci Microsoft Azure AD
Cílem této části je ukazují, jak nakonfigurovat a otestovat Microsoft Azure AD přihlášení SSO se Bynder podle testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v Bynder pro uživatele ve službě Azure AD je Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Bynder musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Bynder.

Nakonfigurovat a otestovat Microsoft Azure AD přihlášení SSO se Bynder, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Microsoft Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Microsoft Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Bynder](#creating-a-bynder-test-user)**  – Pokud chcete mít protějšek Britta Simon v Bynder propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Microsoft Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-microsoft-azure-ad-sso"></a>Konfigurace jednotného přihlašování Microsoft Azure AD
V této části můžete povolit jednotné přihlašování pro aplikaci Microsoft Azure AD na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Bynder.

**Pokud chcete konfigurovat jednotné přihlašování pro aplikaci Microsoft Azure AD s Bynder, proveďte následující kroky:**

1. Na portálu classic na **Bynder** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 
2. Na **jak chcete uživatelům se přihlásit Bynder** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. Na **nakonfigurovat nastavení aplikace** dialogové okno stránky, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**, proveďte následující kroky a klikněte na **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.getbynder.com/sso/SAML/authenticate/`
  2. Klikněte na **Další**.
4. Pokud chcete nakonfigurovat aplikace **SP iniciované režimu** na **nakonfigurovat nastavení aplikace** stránku dialogové okno a potom kliknutím na tlačítko **"Zobrazit upřesňující nastavení (volitelné)"** a pak zadejte **přihlašovací adresa URL** a klikněte na tlačítko **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.getbynder.com/login/`
  2. Klikněte na **Další**.
  
   >[!NOTE]
   >Hodnota pro přihlášení na URL v tomto kurzu je právě placeholfer. Chcete-li získat skutečný vlaue pro vaše prostředí, obraťte se na Bynder.
   >

5. Na **nakonfigurovat jednotné přihlašování v Bynder** , proveďte následující kroky a klikněte na tlačítko **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači.
  2. Klikněte na **Další**.
6. Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Bynder. Připojte soubor stažený metadat a sdílet je s týmem Bynder na jejich straně nastavit jednotné přihlašování.
7. Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][10]
8. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
   
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu classic názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. V uživatelské jméno **textbox**, typ **BrittaSimon**.
  3. Klikněte na **Další**.
6. Na **profil uživatele** dialogové okno proveďte následující kroky:
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. V **křestní jméno** textovému poli, typ **Britta**.  
  2. V **příjmení** textovému poli, typ, **Simon**. 
  3. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
  4. V **Role** seznamu, vyberte **uživatele**.
  5. Klikněte na **Další**.
7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. Poznamenejte si hodnotu **nové heslo**.
   2. Klikněte na **Dokončit**.   

### <a name="create-a-bynder-test-user"></a>Vytvoření zkušebního uživatele Bynder
Cílem této části je vytvoření uživatele v Bynder nazývá Britta Simon. Bynder podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Vytvoří se nový uživatel během pokusu o přístup k Bynder, pokud ještě neexistuje.

>[!NOTE]
>Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktujte tým podpory Bynder. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD
Cílem této části je povolení Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k Bynder.

   ![Přiřadit uživatele][200]

**Pokud chcete přiřadit Britta Simon Bynder, proveďte následující kroky:**

1. Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Přiřadit uživatele][201]
2. V seznamu aplikací vyberte **Bynder**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203]
4. V seznamu uživatelů vyberte **Britta Simon**.
5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Cílem této části je testování konfigurace jednotného přihlašování pro aplikaci Microsoft Azure AD pomocí přístupového panelu.

Když kliknete na dlaždici Bynder na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Bynder.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
