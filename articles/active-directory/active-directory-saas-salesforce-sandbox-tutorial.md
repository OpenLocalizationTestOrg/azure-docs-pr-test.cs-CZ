---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak toouse Salesforce izolovaného prostoru s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: deccd6a07b57c8ba56b7e1c3f3ab311813ca017b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce

cílem Hello tohoto kurzu je tooshow hello integrace Azure a izolovaného prostoru služby Salesforce.  

>[!TIP]
>Pro zpětnou vazbu, najdete v části hello [stránky podpory Azure](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Udělte izolovaných prostorů můžete hello možnost toocreate více kopií vaší organizace v samostatných prostředí pro jiné účely, například vývoj, testování a cvičení bez ohrožení hello datům a aplikacím v provozním Salesforce organizace.  

Další podrobnosti najdete v tématu [izolovaného prostoru Přehled](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Izolovaného prostoru v Salesforce.com

Pokud platný izolovaného prostoru v Salesforce.com zatím nemáte, je třeba toocontact Salesforce.

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení hello integraci aplikací pro izolovaný prostor Salesforce
2. Konfigurace jednotného přihlašování (SSO)
3. Povolení vaší doméně
4. Konfiguraci zřizování uživatelů
5. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "scénář")

## <a name="enable-hello-application-integration-for-salesforce-sandbox"></a>Povolit integraci aplikace hello pro izolovaný prostor Salesforce
Hello cílem této části je toooutline jak tooenable hello integraci aplikací pro izolovaný prostor Salesforce.

**Integrace aplikace hello tooenable pro izolovaný prostor Salesforce, proveďte hello následující kroky:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
   ![Aplikace](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "aplikace")
4. tooopen hello **galerii aplikací**, klikněte na tlačítko **přidat aplikaci**a potom klikněte na **přidat aplikaci pro Moje organizace toouse**.
   
   ![Co chcete toodo? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Co chcete toodo?")
5. V hello **vyhledávacího pole**, typ **izolovaného prostoru Salesforce**.
   
   ![Galerie aplikací](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "galerii aplikací")
6. V podokně výsledků hello, vyberte **izolovaného prostoru Salesforce**a potom klikněte na **Complete** tooadd hello aplikace.
   
   ![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "izolovaného prostoru Salesforce")
   
## <a name="configur-single-sign-on-sso"></a>Configur jednotné přihlašování (SSO)

Hello cílem této části je toooutline jak tooSalesforce tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** Dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooSalesforce izolovaného prostoru** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "izolovaného prostoru Salesforce")
3. Na hello **konfigurace adresy URL aplikace** stránku hello **přihlašovací adresa URL** textovému poli, zadejte URL pomocí hello následující vzor `http://company.my.salesforce.com`a potom klikněte na **Další**.
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "konfigurovat adresu URL aplikace")
4. Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je také potřeba nakonfigurovat hello **identifikátor** toohave hello stejnou hodnotu jako hello **přihlásit na adrese URL**. 
 * Hello **identifikátor** pole naleznete kontrolou hello **zobrazit upřesňující nastavení** zaškrtávací políčko je na hello **konfigurace adresy URL aplikace** stránku hello dialogového okna.
5. Na hello **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello ve vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "nakonfigurovat jednotné přihlašování")
6. V okně prohlížeče jiný web Přihlaste se jako správce do izolovaného prostoru vaší služby Salesforce.
7. V nabídce hello hello nahoře, klikněte na tlačítko **instalační program**.
   
   ![Instalační program](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "instalační program")
8. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.
   
   ![Jednotné přihlašování v nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "jednotné přihlašování v nastavení")
9. V části Nastavení jednotného přihlašování hello proveďte hello následující kroky:
   
   ![Jednotné přihlašování v nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "jednotné přihlašování v nastavení")  
 1.  Vyberte **povoleno SAML**. 
 2.  Klikněte na možnost **Nové**.
10. V hello oddílu SAML jeden přihlašování nastavení proveďte následující kroky hello:
    
    ![SAML jeden nastavení přihlášení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML jeden nastavení přihlášení")  
 1. Do textového pole Název hello, zadejte název hello hello konfigurace (např: *SPSSOWAAD\_Test*). 
 2. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** dialogu stránky, kopie hello **URL vystavitele** hodnotu a pak ji vložit do hello **vystavitele**textové pole.
 3. V hello **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud se jedná o první instance Salesforce izolovaného prostoru hello přidáváte tooyour adresáře. Pokud jste již přidali instance Salesforce karantény, pak pro hello **Entity ID** typu v hello **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`   
 4. Klikněte na tlačítko **Procházet** tooupload hello stáhnout certifikát.  
 5. Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**. 
 6. Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier hello hello subjektu příkaz**.
 7. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** dialogu stránky, kopie hello **vzdálené adresy URL pro přihlášení** hodnotu a pak ji vložit do hello **zprostředkovatele Identity Adresa URL pro přihlášení** textové pole. 
 8. SFDC nepodporuje SAML odhlášení.  Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.
 9. Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**. 
 10. Klikněte na **Uložit**.
11. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "nakonfigurovat jednotné přihlašování")

## <a name="enable-your-domain"></a>Povolit doménu
V této části se předpokládá, že jste již vytvořili domény.  Další podrobnosti najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**tooenable doménu, proveďte následující kroky hello:**

1. V levém navigačním podokně hello, klikněte na **Správa domén**a pak klikněte na tlačítko **Moje domény.**
   
   ![Moje doména](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Moje doména")
   
   >[!NOTE]
   >Přesvědčte se, že doménu je správně nakonfigurováno. 
   > 
2. V hello **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název hello hello SAML jeden přihlašování nastavení z předchozí hello část a nakonec klikněte na tlačítko **Uložit**.
   
   ![Moje doména](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Moje doména")

Jakmile máte doménu nakonfigurován, uživatelé měli používat hello domény adresy URL toologin toohello Salesforce izolovaného prostoru.  

Hodnota hello tooget hello adresy URL, klikněte na tlačítko hello jednotného přihlašování k profilu, který jste vytvořili v předchozí části hello.

## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů
Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele účtů tooSalesforce izolovaného prostoru.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Salesforce portálu hello v horním navigačním panelu hello vyberte vaše tooexpand název nabídky vaše uživatele:
   
   ![Nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "nastavení")
2. Z nabídky uživatele, vyberte **Moje nastavení** tooopen vaše **Moje nastavení** stránky.
3. V levém podokně hello, klikněte na **osobní** tooexpand hello osobní části a pak klikněte na **resetovat Moje zabezpečení tokenu**:
   
   ![Nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "nastavení")
4. Na hello **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** toorequest e-mailu, který obsahuje váš token zabezpečení Salesforce.com.
   
   ![Nový Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "nový Token")
5. Zkontrolujte Doručená pošta e-mailu z Salesforce.com s "**Salesforce.com zabezpečení potvrzení**" jako předmět.
6. Zkontrolujte tato e-mailu a zkopírujte hello zabezpečení hodnota tokenu.
7. V portálu Azure classic, na hello hello **salesforce izolovaného prostoru** stránky integrace aplikací, klikněte na tlačítko **konfiguraci zřizování uživatelů** tooopen hello **konfiguraci zřizování uživatelů**dialogové okno.
   
   ![Konfiguraci zřizování uživatelů](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "konfiguraci zřizování uživatelů")
8. Na hello **zadejte vaše zřizování automatické uživatelů izolovaného prostoru Salesforce pověření tooenable** zadejte hello následující nastavení:
   
   ![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "izolovaného prostoru Salesforce")   
 1. V hello **uživatelské jméno správce izolovaného prostoru Salesforce** textovému poli, zadejte název, který má hello účtu Salesforce izolovaného **správce systému** profil v Salesforce.com přiřazen.
 2. V hello **heslo správce izolovaného prostoru Salesforce** textovému poli, zadejte hello heslo pro tento účet.
 3. V hello **tokenu zabezpečení uživatele** textovému poli, vložte hello hodnota tokenu zabezpečení.
 4. Klikněte na tlačítko **ověřením** tooverify konfiguraci.
 5. Klikněte na tlačítko hello **Další** tlačítko tooopen hello **potvrzení** stránky.
9. Na hello **potvrzení** klikněte na tlačítko **Complete** toosave konfiguraci.
   
## <a name="assigning-users"></a>Přiřazení uživatelů

tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign uživatelé tooSalesforce izolovaného prostoru, proveďte následující kroky hello:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello ** izolovaného prostoru Salesforce ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ano")

Teď by měla Počkejte 10 minut a ověřte, zda text hello účet byl synchronizované tooSalesforce izolovaného prostoru.

Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](https://msdn.microsoft.com/library/dn308586).

