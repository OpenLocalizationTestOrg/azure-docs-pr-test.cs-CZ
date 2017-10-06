---
title: "Kurz: Azure Active Directory integrace s centrální plochy | Microsoft Docs"
description: "Zjistěte, jak toouse centrální plochy s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 93036ae801c446ce476288c00579931ba10a843b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Kurz: Azure Active Directory integrace s centrální plochy
cílem Hello tohoto kurzu je tooshow hello integrace Azure a centrální plochy. Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Centrální plochy jednotné přihlašování v předplatném povolené / centrální plochy klienta

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

* Povolení integrace aplikace hello pro centrální plochy
* Konfigurace jednotného přihlašování (SSO)
* Konfiguraci zřizování uživatelů
* Přiřazení uživatelů

![Scénář](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "scénář")

## <a name="enable-hello-application-integration-for-central-desktop"></a>Povolit integraci aplikace hello pro centrální plochu
Hello cílem této části je toooutline jak tooenable hello integraci aplikací pro centrální plochu.

**Integrace aplikace hello tooenable pro centrální plochy, proveďte hello následující kroky:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
   ![Aplikace](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
   ![Přidat aplikaci](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **centrální plochy**.
   
   ![Galerie aplikací](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **centrální plochy**a potom klikněte na **Complete** tooadd hello aplikace.
   
   ![Centrální plochy](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrální plochy")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooCentral plochy ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

V rámci tohoto postupu jsou požadované tooupload klienta centrální plochy tooyour kódování base-64 kódovaného certifikátu.  
Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello ** nakonfigurovat jednotné přihlašování ** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooCentral plochy** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "nakonfigurovat jednotné přihlašování")
3. Na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**: 
   
   1. V hello **centrální plochy přihlašovací v adrese URL** textovému poli, typ hello adresu URL vašeho klienta centrální Desktop (např: *http://contoso.centraldesktop.com*).
   2. V textovém poli Adresa URL odpovědi centrální plochy hello, zadejte adresu URL centrální AssertionConsumerService plochy (např: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Hodnota hello můžete získat z centrální plochy metadat hello (např: *http://contoso.centraldesktop.com*).
   >  
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurovat adresu URL aplikace")
4. Na hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, toodownload svůj certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello ve vašem počítači.
   
  ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "nakonfigurovat jednotné přihlašování")
5. Přihlaste se tooyour **centrální plochy** klienta.
6. Přejděte příliš**nastavení**, klikněte na tlačítko **Upřesnit**a potom klikněte na **jednotné přihlašování**.
   
  ![Instalační program – pokročilé](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "instalační program – Rozšířená")
7. Na hello **jeden znak v nastavení** proveďte hello následující kroky:
   
  ![Jednotné přihlašování v nastavení](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "jednotné přihlašování v nastavení")
   
  1. Vyberte **povolit SAML v2 jednotného přihlašování**.
  2. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, kopie hello **URL vystavitele** hodnotu a pak ji vložit do hello **jednotného přihlašování k adrese URL** textové pole.
  3. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, kopie hello **vzdálené adresy URL pro přihlášení** hodnotu a pak ji vložit do hello **adresu URL pro přihlášení SSO**textové pole.
  4. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, kopie hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **adresy URL odhlašovací jednotného přihlašování k** textové pole.
8. V hello **metodu ověřování podpisu zpráva** část, proveďte následující kroky hello:
   
   ![Zpráva metodu ověřování podpisu](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "zprávy metodu ověřování podpisu")
   
  1. Vyberte **certifikát**.
  2. Z hello **certifikát jednotného přihlašování k** seznamu, vyberte **RSH SHA256**.
  3. Vytvořte textový soubor z hello stáhnout certifikát, hello kopírování obsahu hello textového souboru a pak ji vložit do hello **certifikát jednotného přihlašování k** pole.  
     >[!TIP]
     >Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Vyberte **zobrazit stránku přihlášení tooyour SAMLv2 odkaz**.
9. Klikněte na tlačítko **aktualizace**.
10. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "nakonfigurovat jednotné přihlašování")
    
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

Pro AAD uživatelé toobe možné toosign v musí být zřízená toohello centrální plochy aplikace. Tato část popisuje, jak toocreate AAD uživatelských účtů v centrální plochy.

**tooprovision uživatelské účty tooCentral plochy:**
1. Přihlaste se tooyour centrální plochy klienta.
2. Přejděte příliš**osoby \> vnitřní členy**.
3. Klikněte na tlačítko **přidat vnitřní členy**.
   
  ![Lidé](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "osoby")
4. V hello **e-mailovou adresu nové členy** textovému poli, zadejte účet AAD tooprovision a pak klikněte na **Další**.
   
  ![E-mailové adresy nové členy](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-mailové adresy nové členy")
5. Klikněte na tlačítko **přidejte interní členy**.
   
  ![Přidání interní člena](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "přidat vnitřní člen")
   
   >[!NOTE]
   >Hello uživatelů, které jste přidali obdrží e-mail, který obsahuje odkaz potvrzení potřebují tooclick tooactivate hello účtu.
   > 

>[!NOTE]
>Můžete použít jakékoli jiné centrální plochy uživatele účtu vytvoření nástroje nebo rozhraní API poskytované centrální plochy tooprovision AAD uživatelské účty
>  

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign uživatelé tooCentral plochy, proveďte hello následující kroky:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ano")

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

