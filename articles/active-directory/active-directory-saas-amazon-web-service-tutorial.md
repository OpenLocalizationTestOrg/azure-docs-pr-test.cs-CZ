---
title: 'Kurz: Azure Active Directory integrace s Amazon Web Services (AWS) | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1b79572ace63f6174ce4fa014c49bf44bd728228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Kurz: Azure Active Directory integrace s Amazon Web Services (AWS)

V tomto kurzu zjistíte, jak toointegrate Amazon Web Services (AWS) s Azure Active Directory (Azure AD).

Integrace Amazon Web Services (AWS) s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAmazon Web Services (AWS)
- Vaši uživatelé tooautomatically get přihlášeného tooAmazon Web Services (AWS) (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Amazon Web Services (AWS), it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD pomocí Amazon Web Services (AWS), je třeba hello následující položky:

- Předplatné služby Azure AD
- Amazon Web Services (AWS) jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Amazon Web Services (AWS) z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-amazon-web-services-aws-from-hello-gallery"></a>Přidání Amazon Web Services (AWS) z Galerie hello
tooconfigure hello integrace Amazon Web Services (AWS) do Azure AD, je nutné tooadd Amazon Web Services (AWS) hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Amazon Web Services (AWS) z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Amazon Web Services (AWS)**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. Na panelu výsledků hello vyberte **Amazon Web Services (AWS)**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS) podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Amazon Web Services (AWS) je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Amazon Web Services (AWS) musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** Amazon Web Services (AWS).

tooconfigure a testování Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Amazon Web Services](#creating-an-amazon-web-services-test-user)**  -toohave protějšek Britta Simon v Amazon Web Services (AWS), která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Amazon Web Services (AWS).

**tooconfigure Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), proveďte následující kroky hello:**

1. V portálu Azure, hello na hello **Amazon Web Services (AWS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. Na hello **Amazon Web Services (AWS) domény a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. Hello Amazon Web Services (AWS) softwarová aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tento.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:
    
    | Název atributu  | Hodnota atributu | obor názvů |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://AWS.Amazon.com/SAML/Attributes |
    | Role            | User.assignedroles |  https://AWS.Amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Je nutné zřizování v Azure AD toofetch všechny role hello z konzoly AWS tooconfigure hello uživatelů. Podrobnosti najdete níže uvedené kroky pro zřízení hello.

    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek. Přidejte hodnotu Namespace hello jako ve výše uvedených.
    
    d. Klikněte na tlačítko **OK**.

7. Klikněte na tlačítko **Uložit** tlačítko toosave hello nastavení v Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. V okně jiný prohlížeč, lokality společnosti Amazon Web Services (AWS) tooyour přihlášení jako správce.

9. Klikněte na tlačítko **konzoly domovské**.
   
    ![Konfigurovat jednotné přihlašování][11]

10. Klikněte na tlačítko **IAM** z **zabezpečení, Identity a dodržování předpisů** služby.
   
    ![Konfigurovat jednotné přihlašování][12]

11. Klikněte na tlačítko **zprostředkovatelů Identity**a potom klikněte na **vytvoření zprostředkovatele**.
   
    ![Konfigurovat jednotné přihlašování][13]

12. Na hello **konfigurace zprostředkovatele** dialogové okno proveďte hello následující kroky:
   
    ![Konfigurovat jednotné přihlašování][14]
 
    a. Jako **typ zprostředkovatele**, vyberte **SAML**.

    b. V hello **název zprostředkovatele** textovému poli, zadejte název zprostředkovatele (např: *službou WAAD*).

    c. tooupload váš soubor stažený metadata, klikněte na tlačítko **zvolit soubor**.

    d. Klikněte na tlačítko **dalším krokem**.

13. Na hello **ověřte informace o poskytovateli** dialogové okno stránky, klikněte na tlačítko **vytvořit**. 
    
    ![Konfigurovat jednotné přihlašování][15]

14. Klikněte na tlačítko **role**a potom klikněte na **vytvořit novou roli**. 
    
    ![Konfigurovat jednotné přihlašování][16]

15. Na hello **nastavit název Role** dialogové okno, proveďte následující kroky hello: 
    
    ![Konfigurovat jednotné přihlašování][17] 

    a. V hello **název Role** textovému poli, zadejte název role (např: *TestUser*). 

    b. Klikněte na tlačítko **dalším krokem**.

16. Na hello **vyberte typ Role** dialogové okno, proveďte následující kroky hello: 
    
    ![Konfigurovat jednotné přihlašování][18] 

    a. Vyberte **Role pro přístup poskytovatele Identity**. 

    b. V hello **Grant webové jednotné přihlašování (WebSSO) přístup tooSAML zprostředkovatelé** klikněte na tlačítko **vyberte**.

17. Na hello **navázání vztahu důvěryhodnosti** dialogové okno, proveďte následující kroky hello:  
    
    ![Konfigurovat jednotné přihlašování][19] 

    a. Jako poskytovatel SAML, vyberte poskytovatele SAML hello jste dříve vytvořili (např: *službou WAAD*)
  
    b. Klikněte na tlačítko **dalším krokem**.

18. Na hello **ověřte důvěřovat Role** dialogové okno, klikněte na tlačítko **další krok**.
    
    ![Konfigurovat jednotné přihlašování][32]

19. Na hello **připojit zásady** dialogové okno, klikněte na tlačítko **další krok**.
    
    ![Konfigurovat jednotné přihlašování][33]

20. Na hello **zkontrolujte** dialogové okno, proveďte následující kroky hello:
    
    ![Konfigurovat jednotné přihlašování][34]
 
    a. Klikněte na tlačítko **vytvořit roli**.

    b. Vytvořit tolik role podle potřeby a jejich namapování toohello zprostředkovatele Identity.

21. Všechny role hello z AWS teď nakonfigurujte zřizování toofetch hello uživatelů

    a. V přihlášení prostřednictvím konzoly AWS hello pomocí kořenového účtu

    b. Hello pravém horním rohu klikněte na své jméno a pak klikněte na hello **Moje zabezpečovací pověření** možnost. Tím se otevře na obrazovce jako upozornění. Klikněte na tlačítko hello **zabezpečovací pověření** tlačítko toopass hello obrazovky.
        
       ![Konfigurovat jednotné přihlašování][36]

       ![Konfigurovat jednotné přihlašování][37]

    c. V hello přístupové klíče oddílu, klikněte na tlačítko hello **vytvořit nový přístupový klíč** tlačítko. Tím se vytvoří hello ID přístupu klíč a hodnotu tokenu.
    
       ![Konfigurovat jednotné přihlašování][38]

    d. Zkopírujte oba tyto hodnoty a také, tak, aby neztratili ho stáhnout.

    e. V hello Azure Portal, na stránce integrace aplikace hello Amazon Web Services (AWS), klikněte na **zřizování**.
        
       ![Konfigurovat jednotné přihlašování][35]

    f. Nastavit režim zřizování hello příliš**automatické**
        
       ![Konfigurovat jednotné přihlašování][39]

    g. Nyní ve hello **clientsecret** a **tajný klíč tokenu** vložit hello odpovídající hodnoty, které jste zkopírovali z konzoly AWS.
    
    h. Můžete kliknout na hello **Test připojení** tlačítko tootest hello připojení. Po úspěšné, můžete začít hello zřizování konektor.
       
       ![Konfigurovat jednotné přihlašování][40]

    i. Teď povolit hello Stav zřizování příliš**na**. Tím se spustí načítání hello role z aplikace hello.

       ![Konfigurovat jednotné přihlašování][41]

    > [!NOTE]
    > Spuštění služby Azure AD zřizování každých po některé čas toosync hello role z AWS. Měli byste vidět všechny hello zprostředkovatele Identity připojena AWS rolí do služby Azure AD a můžete je použít při přiřazování toousers aplikace hello nebo skupiny.

<!--### Next steps

tooensure users can sign-in tooAmazon Web Services (AWS) after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooAmazon Web Services (AWS) in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Vytváření testovacího uživatele Amazon Web Services

V pořadí tooenable Azure AD Uživatelé toolog v tooAmazon Web Services (AWS) se musí zřízená do Amazon Web Services (AWS). V případě hello Amazon Web Services (AWS) zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Amazon Web Services (AWS)** společnosti lokality jako správce.

2. Klikněte na tlačítko hello **konzoly domovské** ikonu. 
   
    ![Konfigurovat jednotné přihlašování][11]

3. Klikněte na tlačítko identita a správa přístupu. 
   
    ![Konfigurovat jednotné přihlašování][28]

4. V hello řídicí panel, klikněte na **uživatelé**a potom klikněte na **vytvořte nové uživatele**. 
   
    ![Konfigurovat jednotné přihlašování][29]

5. V dialogovém okně Vytvořit uživatele hello proveďte následující kroky hello: 
   
    ![Konfigurovat jednotné přihlašování][30]   
    
    a. V hello **zadejte uživatelská jména** textová pole, zadejte uživatelské jméno Brita Simon (userprincipalname) ve službě Azure AD.

    b. Klikněte na tlačítko **vytvořit.**
        
### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooAmazon Web Services (AWS).

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooAmazon Web Services (AWS), proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Amazon Web Services (AWS)**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Na **vybrat roli** karty, vyberte hello vhodnou roli pro uživatele hello. Tyto role jsou zobrazeny s názvem role hello a název zprostředkovatele identity. Tímto způsobem můžete snadno identifikovat hello role z AWS.

8. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Amazon Web Services (AWS) dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného Amazon Web Services (AWS) aplikace. 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
