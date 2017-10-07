---
title: "Kurz: Azure Active Directory integrace s SAP HANA Cloudová platforma | Microsoft Docs"
description: "Zjistěte, jak toouse SAP HANA Cloudová platforma s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: cc6610969b1c7b08f776e6969a5977fc75eb9ab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Kurz: Integrace Azure Active Directory se službou SAP HANA Cloud Platform
cílem Hello tohoto kurzu je tooshow hello integrace Azure a SAP HANA Cloudová platforma.

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Účet SAP HANA Cloudová platforma

Po dokončení tohoto kurzu, hello uživatele Azure AD, které jste přiřadili tooSAP HANA Cloudová platforma bude možné toosingle přihlášení do aplikace hello pomocí hello [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Potřebujete toodeploy vlastní aplikace nebo odebírat aplikace tooan na vaše SAP HANA Cloudová platforma jeden účet tootest přihlásit. V tomto kurzu je aplikace nasazená v účtu hello.
> 
> 

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení integrace aplikace hello pro SAP HANA Cloudová platforma
2. Konfigurace jednotného přihlašování (SSO)
3. Přiřazení role uživatele tooa
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "scénář")

## <a name="enabling-hello-application-integration-for-sap-hana-cloud-platform"></a>Povolení integrace aplikace hello pro SAP HANA Cloudová platforma
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro SAP HANA Cloudová platforma.

**Integrace aplikace hello tooenable pro SAP HANA Cloudová platforma, proveďte hello následující kroky:**

1. V hello Azure Management Portal, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Přidat aplikaci](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **SAP HANA Cloudová platforma**.
   
    ![Galerie aplikací](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **SAP HANA Cloudová platforma**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooSAP HANA Cloudová platforma ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

V rámci tohoto postupu jsou požadované tooupload klienta SAP HANA Cloudová platforma tooyour kódování base-64 kódovaného certifikátu.  

Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooSAP HANA Cloudová platforma** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "nakonfigurovat jednotné přihlašování")
3. V okně prohlížeče jiných webových přihlaste toohello SAP HANA cloudové platformy řídící panel na https://account. \<hostitele na šířku\>.ondemand.com/cockpit (např: *https://account.hanatrial.ondemand.com/cockpit*).
4. Klikněte na tlačítko hello **důvěřovat** kartě.
   
    ![Vztah důvěryhodnosti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "vztahu důvěryhodnosti")
5. V části Správa vztahu důvěryhodnosti proveďte následující kroky hello:
   
    ![Získat Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "získat Metadata")
   
   1. Klikněte na tlačítko hello **místní poskytovatele služeb** kartě.
   2. Klikněte na tlačítko toodownload hello SAP HANA Cloudová platforma soubor metadat, **získat Metadata**.
6. Na portálu Azure Active classic hello na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurovat adresu URL aplikace")
   
   1. V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše toosign uživatelů do vašeho **SAP HANA Cloudová platforma** aplikace. Toto je adresa URL účtu konkrétní hello chráněného prostředku v aplikaci SAP HANA Cloudová platforma. Hello adresa URL je založena na následujících vzor hello: *https://\<applicationName\>\<accountName\>.\< na šířku hostitele\>.ondemand.com/\<cesta\_k\_chráněné\_prostředků\>*  (např: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)
      
     >[!NOTE]
     >Toto je hello URL ve vaší aplikace SAP HANA Cloudová platforma, která vyžaduje tooauthenticate hello uživatele.
     > 

   2. Otevřete soubor metadat SAP HANA Cloudová platforma hello stáhli a vyhledejte hello **ns3:AssertionConsumerService** značky.
   3. Zkopírujte hodnotu hello hello **umístění** atributů a pak ji vložit do hello **SAP HANA cloudové platformy adresa URL odpovědi** textové pole.

7. Na hello **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** stránky, toodownload metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "nakonfigurovat jednotné přihlašování")
8. Na hello SAP HANA cloudové platformy řídicí panel, v hello **místní poskytovatele služeb** část, proveďte následující kroky hello:
   
    ![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "důvěřovat správy")
   
  1. Klikněte na **Upravit**.
  2. Jako **typ konfigurace**, vyberte **vlastní**.
  3. Jako **místní název zprostředkovatele**, ponechte výchozí hodnotu hello.
  4. toogenerate **podpisový klíč** a **certifikát pro podpis** dvojice klíčů, klikněte na tlačítko **Generování páru klíč**.
  5. Jako **hlavní šíření**, vyberte **zakázané**.
  6. Jako **vynucení ověřování**, vyberte **zakázané**.
  7. Klikněte na **Uložit**.

9. Klikněte na tlačítko hello **důvěryhodného zprostředkovatele Identity** a pak klikněte **přidání důvěryhodného zprostředkovatele Identity**.
   
    ![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "důvěřovat správy")
   
    >[!NOTE]
    >toomanage hello seznamu zprostředkovatelů identity důvěryhodný, je nutné toohave vybrali hello vlastní typ konfigurace v hello části místní poskytovatele služeb. Výchozí typ konfigurace je nutné upravovat a implicitní vztah důvěryhodnosti toohello SAP ID služby. Pro None nemáte žádné nastavení důvěryhodnosti.
    > 
    > 

10. Klikněte na tlačítko hello **Obecné** a pak klikněte **Procházet** tooupload hello stáhnout soubor metadat.
    
    ![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "důvěřovat správy")
    
    >[!NOTE]
    >Po nahrání souboru metadat hello, hello hodnoty pro **adresy jednotného přihlašování**, **jednu adresu URL odhlašovací** a **certifikát pro podpis** jsou naplněny automaticky.
    > 
    > 

11. Klikněte na tlačítko hello **atributy** kartě.
12. Na hello **atributy** kartu, proveďte následující krok hello:
    
    ![Atributy](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "atributy") 
  * Klikněte na tlačítko **Add Assertion-Based atribut**a poté přidejte následující atributy založené na assertion hello:
       
    | Kontrolní výraz atribut | Objekt zabezpečení atribut |
    | --- | --- |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/givenName |FirstName |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Surname |Příjmení |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/EmailAddress |E-mailu 
   
     >[!NOTE]
     >Konfigurace Hello hello atributů závisí na tom, jak aplikace hello na HCP jsou vyvinuté, tj. atributy, které budou očekávat při hello odpověď SAML a pod které názvem (hlavní atribut) přistupují tento atribut v kódu hello.
     > 
     >  

    1.  Hello **výchozí atribut** v hello je snímek jen pro účely obrázku. Není nutné toomake hello scénář pracovní.   
    2.  Hello názvy a hodnoty pro **hlavní atribut** ukazuje hello snímek závisí na tom, jak je aplikace hello vyvinuta. Je možné, že vaše aplikace vyžaduje jiný mapování.
     
13. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** Dialogový stránky, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "nakonfigurovat jednotné přihlašování")

###<a name="assertion-based-groups"></a>Skupiny založené na výrazu
Volitelný krok můžete nakonfigurovat skupiny založené na assertion pro zprostředkovatele Identity Azure Active Directory.

Pomocí skupin na SAP HANA Cloudová platforma umožňuje toodynamically přiřadit jeden nebo více uživatelů tooone nebo více rolí v aplikacích SAP HANA Cloudová platforma určit podle hodnot atributů v hello SAML 2.0 kontrolní výraz. 

Například pokud hello kontrolní výraz obsahuje atribut hello "*kontrakt = dočasné*", může být vhodné všechny skupiny přidané toohello toobe ovlivnění uživatelé"*dočasné*". skupiny Hello "*dočasné*" může obsahovat jednu nebo více rolí z jedné nebo více aplikací, které jsou nasazené v účtu SAP HANA Cloudová platforma.
 
Skupiny založené na assertion použijte, pokud chcete přiřadit toosimultaneously mnoho tooone uživatele nebo více rolí aplikací ve vašem účtu SAP HANA Cloudová platforma. Pokud chcete tooassign pouze jednoho či malý počet uživatelů toospecific rolí, doporučujeme, abyste jim přiřadil přímo v hello "**autorizací**" kartě řídící panel SAP HANA Cloudová platforma hello.

## <a name="assign-a-role-tooa-user"></a>Přiřazení role uživatele tooa
V pořadí tooenable Azure AD Uživatelé toolog do SAP HANA cloudové platformy je nutné přiřadit role v toothem SAP HANA Cloudová platforma hello.

**tooassign tooa role uživatele, proveďte následující kroky hello:**

1. Přihlaste se tooyour **SAP HANA Cloudová platforma** řídící panel.
2. Proveďte následující hello:
   
   ![Povolení](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "autorizací")
   
  1. Klikněte na tlačítko **autorizace**.
  2. Klikněte na tlačítko hello **uživatelé** kartě.
  3. V hello **uživatele** textovému poli, typ hello uživatele e-mailovou adresu.
  4. Klikněte na tlačítko **přiřadit** tooassign hello uživatele tooa role.
  5. Klikněte na **Uložit**.

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign uživatelé tooSAP HANA Cloudová platforma, proveďte následující kroky hello:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ano")

Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

