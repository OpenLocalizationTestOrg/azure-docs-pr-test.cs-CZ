---
title: "Kurz: Azure Active Directory integrace s SAP HANA Cloudová platforma | Microsoft Docs"
description: "Další informace o použití SAP HANA Cloudová platforma s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: e03bc2410a8d57363c558f723b3bfd0e69b3f4c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Kurz: Integrace Azure Active Directory se službou SAP HANA Cloud Platform
Cílem tohoto kurzu je zobrazit integraci Azure a Cloudová platforma SAP HANA.

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Účet SAP HANA Cloudová platforma

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace pomocí uživatele Azure AD, které jste přiřadili SAP HANA Cloudová platforma [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Potřebujete nasadit vlastní aplikaci nebo přihlášení k aplikaci na vašem účtu SAP HANA Cloudová platforma k testování jednotné přihlašování v odběru. V tomto kurzu je aplikace nasazená v účtu.
> 
> 

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikace pro SAP HANA Cloudová platforma
2. Konfigurace jednotného přihlašování (SSO)
3. Přiřazení role pro uživatele
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "scénář")

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Povolení integrace aplikace pro SAP HANA Cloudová platforma
Cílem této části se popisují postup povolení integrace aplikace pro SAP HANA Cloudová platforma.

**Pokud chcete povolit integraci aplikací pro SAP HANA Cloudová platforma, proveďte následující kroky:**

1. V portálu pro správu Azure, v levém navigačním podokně klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Přidat aplikaci](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **SAP HANA Cloudová platforma**.
   
    ![Galerie aplikací](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "galerii aplikací")
7. V podokně výsledků vyberte **SAP HANA Cloudová platforma**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují postup povolení uživatelů k ověřování a SAP HANA Cloudovou platformu pro svůj účet ve službě Azure AD využívající federaci na základě protokolu SAML.

V rámci tohoto postupu je nutné odeslat kódování base-64 kódovaného certifikátu klienta SAP HANA Cloudová platforma.  

Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit SAP HANA Cloudová platforma** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "nakonfigurovat jednotné přihlašování")
3. V okně prohlížeče jiný web Přihlaste se k SAP HANA cloudové platformy řídicím panelu v https://account. \<hostitele na šířku\>.ondemand.com/cockpit (např: *https://account.hanatrial.ondemand.com/cockpit*).
4. Klikněte **důvěřovat** kartě.
   
    ![Vztah důvěryhodnosti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "vztahu důvěryhodnosti")
5. V části Správa vztahu důvěryhodnosti proveďte následující kroky:
   
    ![Získat Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "získat Metadata")
   
   1. Klikněte **místní poskytovatele služeb** kartě.
   2. Chcete-li stáhnout soubor metadat SAP HANA Cloudová platforma, klikněte na tlačítko **získat Metadata**.
6. Na portálu Azure Active classic na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurovat adresu URL aplikace")
   
   1. V **přihlašovací adresa URL** textové pole, zadejte adresu URL používají vaši uživatelé pro přihlášení do vaší **SAP HANA Cloudová platforma** aplikace. Toto je adresa URL účtu konkrétní chráněného prostředku v aplikaci SAP HANA Cloudová platforma. Adresa URL je založena na následující vzoru: *https://\<applicationName\>\<accountName\>.\< na šířku hostitele\>.ondemand.com/\<cesta\_k\_chráněné\_prostředků\>*  (např: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)
      
     >[!NOTE]
     >Toto je adresa URL v aplikaci SAP HANA Cloudová platforma, která vyžaduje ověření uživatele.
     > 

   2. Otevřete stažený soubor metadat SAP HANA Cloudová platforma a vyhledejte **ns3:AssertionConsumerService** značky.
   3. Zkopírujte hodnotu **umístění** atribut a vložte ji do **SAP HANA cloudové platformy adresa URL odpovědi** textové pole.

7. Na **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** stránku a stáhnout metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "nakonfigurovat jednotné přihlašování")
8. Na SAP HANA cloudové platformy řídicím panelu v **místní poskytovatele služeb** část, proveďte následující kroky:
   
    ![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "důvěřovat správy")
   
  1. Klikněte na **Upravit**.
  2. Jako **typ konfigurace**, vyberte **vlastní**.
  3. Jako **místní název zprostředkovatele**, ponechte výchozí hodnotu.
  4. Generovat **podpisový klíč** a **certifikát pro podpis** dvojice klíčů, klikněte na tlačítko **Generování páru klíč**.
  5. Jako **hlavní šíření**, vyberte **zakázané**.
  6. Jako **vynucení ověřování**, vyberte **zakázané**.
  7. Klikněte na **Uložit**.

9. Klikněte **důvěryhodného zprostředkovatele Identity** a pak klikněte **přidání důvěryhodného zprostředkovatele Identity**.
   
    ![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "důvěřovat správy")
   
    >[!NOTE]
    >Ke správě seznamu zprostředkovatelů identity důvěryhodné, budete muset vybrali vlastní konfigurace zadejte v části místní poskytovatele služeb. Výchozí typ konfigurace je nutné upravovat a implicitní vztah důvěryhodnosti ke službě ID SAP. Pro None nemáte žádné nastavení důvěryhodnosti.
    > 
    > 

10. Klikněte **Obecné** a pak klikněte **Procházet** nahrát soubor stažený metadat.
    
    ![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "důvěřovat správy")
    
    >[!NOTE]
    >Po nahrání souboru metadat, hodnoty **adresy jednotného přihlašování**, **jednu adresu URL odhlašovací** a **certifikát pro podpis** jsou naplněny automaticky.
    > 
    > 

11. Klikněte na kartu **Atributy**.
12. Na **atributy** kartu, proveďte následující krok:
    
    ![Atributy](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "atributy") 
  * Klikněte na tlačítko **Add Assertion-Based atribut**a poté přidejte následující na základě assertion atributy:
       
    | Kontrolní výraz atribut | Objekt zabezpečení atribut |
    | --- | --- |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/givenName |FirstName |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Surname |Příjmení |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/EmailAddress |E-mailu 
   
     >[!NOTE]
     >Konfigurace atributů závisí na tom, jak aplikace na HCP jsou vyvinuté, tj. atributy, které se očekává v odpovědi SAML a pod které názvem (hlavní atribut) přistupují tento atribut v kódu.
     > 
     >  

    1.  **Výchozí atribut** na snímku obrazovky je jen pro účely obrázku. Chcete-li tento scénář fungovat není potřeba.   
    2.  Názvy a hodnoty pro **hlavní atribut** ukazuje snímek obrazovky závisí na tom, jak je aplikace vyvinuté. Je možné, že vaše aplikace vyžaduje jiný mapování.
     
13. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** Dialogový stránky, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "nakonfigurovat jednotné přihlašování")

###<a name="assertion-based-groups"></a>Skupiny založené na výrazu
Volitelný krok můžete nakonfigurovat skupiny založené na assertion pro zprostředkovatele Identity Azure Active Directory.

Použití skupin na SAP HANA Cloudová platforma umožňuje dynamicky přiřadit jeden nebo více uživatelů k jedné nebo více rolí v aplikacích SAP HANA Cloudová platforma, určit podle hodnot atributů v kontrolního výrazu SAML 2.0. 

Například, pokud kontrolní výraz obsahuje atribut "*kontrakt = dočasné*", může být vhodné všechny ovlivnění uživatelé mají být přidány do skupiny"*dočasné*". Skupiny "*dočasné*" může obsahovat jednu nebo více rolí z jedné nebo více aplikací, které jsou nasazené v účtu SAP HANA Cloudová platforma.
 
Skupiny založené na assertion použijte, pokud chcete současně přiřadit jednu nebo více rolí aplikací ve vašem účtu SAP HANA Cloudová platforma mnoha uživateli. Pokud chcete přiřadit jenom na jeden nebo malý počet uživatelů pro konkrétní role, doporučujeme, abyste jim přiřadil přímo v "**autorizací**" kartě řídící panel SAP HANA Cloudová platforma.

## <a name="assign-a-role-to-a-user"></a>Přiřazení role uživatele
Pokud chcete povolit uživatelům Azure AD Přihlaste se k SAP HANA Cloudová platforma, je třeba přiřadit role v SAP HANA Cloudová platforma k nim.

**Pokud chcete přiřadit role pro uživatele, proveďte následující kroky:**

1. Přihlaste se k vaší **SAP HANA Cloudová platforma** řídící panel.
2. Postupujte takto:
   
   ![Povolení](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "autorizací")
   
  1. Klikněte na tlačítko **autorizace**.
  2. Klikněte **uživatelé** kartě.
  3. V **uživatele** textovému poli, zadejte e-mailovou adresu uživatele.
  4. Klikněte na tlačítko **přiřadit** přiřadit uživatele k roli.
  5. Klikněte na **Uložit**.

## <a name="assign-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k SAP HANA Cloudová platforma, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.
2. Na **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ano")

Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

