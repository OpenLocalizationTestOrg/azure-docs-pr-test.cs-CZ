---
title: "Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů v Azure | Microsoft Docs"
description: "Naučte se automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: b061f0ddad70be4a5ca48d48d1a737d6af8afa8d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a>Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Google Apps a službou Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Google Apps.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Pro pracovní nebo Google Apps pro vzdělávací organizace musí mít platný klienta pro Google Apps. Bezplatný zkušební účet můžete použít buď služby.
*   Uživatelský účet v Google Apps s oprávněními správce týmu.

## <a name="assigning-users-to-google-apps"></a>Přiřazení uživatelů ke Google Apps

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Google Apps. Jakmile se rozhodli, můžete přiřadit tyto uživatele do aplikace pro Google Apps podle pokynů tady: [přiřadit uživatele nebo skupinu enterprise aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

> [!IMPORTANT]
>*   Dále je doporučeno jednoho uživatele Azure AD pro Google Apps přidělí otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.
>*   Při přiřazování uživatele Google Apps, je nutné zvolit roli uživatele nebo "Skupiny" v dialogovém okně přiřazení. Roli "Výchozí přístup" nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit zřizování automatizované uživatelů

Tato část příručky vás připojení služby Azure AD k rozhraní API Google Apps uživatel účet zřizování a konfigurací zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v Google Apps na základě uživatele a přiřazení skupiny ve službě Azure AD.

>[!Tip]
>Také můžete povolit na základě SAML jednotné přihlašování pro Google Apps, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu

> [!NOTE]
> Jiné vhodným řešením pro automatizaci zřizování uživatelů ke Google Apps je použití [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) která zřizuje identitami místní služby Active Directory pro Google Apps. Naproti tomu zřídí řešení v tomto kurzu uživatelů Azure Active Directory (cloud) a poštovní skupiny Google Apps. 

1. Přihlaste se k [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce a klikněte na tlačítko **zabezpečení**. Pokud nevidíte odkaz, mohou být skryty pod **více ovládacích prvků** nabídky v dolní části obrazovky.
   
    ![Klikněte na Zabezpečení.][10]

2. Na **zabezpečení** klikněte na tlačítko **referenční dokumentace rozhraní API**.
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][15]

3. Vyberte **povolit rozhraní API pro přístup**.
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][16]

    > [!IMPORTANT]
    > Pro každého uživatele, který máte v úmyslu zřízení Google Apps, svoje uživatelské jméno ve službě Azure Active Directory *musí* být vázáno vlastní doménu. Například uživatelská jména, které vypadají bob@contoso.onmicrosoft.com není přijat Google Apps, zatímco bob@contoso.com byla přijata. Existujícího uživatele domény můžete změnit úpravou jejich vlastnosti ve službě Azure AD. Pokyny, jak nastavit vlastní doménu pro Azure Active Directory a Google Apps jsou součástí následující kroky.
      
4. Pokud zatím jste nepřidali vlastního názvu domény do Azure Active Directory, postupujte podle následujících kroků:
  
    a. V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. V seznamu adresáře vyberte svůj adresář. 

    b. Klikněte na tlačítko **název domény** v levém navigačním podokně a pak klikněte na tlačítko **přidat**.
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Přidání domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Zadejte název domény do **název domény** pole. Tento název domény by měl být stejný jako název domény, kterou chcete použít pro Google Apps. Až budete připravení, klikněte na **přidáním domény** tlačítko.
     
     ![Název domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Klikněte na tlačítko **Další** přejděte na stránku pro ověření. Pokud chcete ověřit, že jste vlastníkem této domény, je nutné upravit záznamy DNS domény podle hodnoty na této stránce. Můžete ověřit pomocí buď **záznamů MX** nebo **záznamů TXT**podle toho, co jste vybrali pro **typ záznamu** možnost. Komplexní pokyny o tom, jak ověřit název domény se službou Azure AD, najdete v části [přidat vlastní název domény do Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Opakujte předchozí kroky u všech domén, které chcete přidat do vašeho adresáře.

5. Teď, když si ověříte, že všechny vaše domény s Azure AD, je nutné nyní ověřit jejich znovu s Google Apps. Pro každou doménu, která již není registrován u služby Google Apps proveďte následující kroky:
   
    a. V [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **domény**.
     
     ![Klikněte na domény][20]

    b. Klikněte na tlačítko **přidejte k doméně nebo alias domény**.
     
     ![Přidání nové domény][21]

    c. Vyberte **přidat jiné domény**a zadejte název domény, který chcete přidat.
     
     ![Zadejte název domény][22]

    d. Klikněte na tlačítko **pokračovat a ověření vlastnictví domény**. Postupujte podle pokynů k ověření, že jste vlastníkem názvu domény. Komplexní pokyny o tom, jak ověřit doménu s Google Apps naleznete v tématu. [Ověřit vlastnictví lokality s Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Opakujte předchozí kroky pro další domény, které chcete přidat do Google Apps.
     
     > [!WARNING]
     > Pokud změníte primární doménu vašeho klienta Google Apps, a pokud již máte nakonfigurovat jednotné přihlašování s Azure AD, pak je nutné zopakovat krok #3 pod [krok dva: Povolit jednotné přihlašování](#step-two-enable-single-sign-on).
       
6. V [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **rolí správce**.
   
     ![Klikněte na Google Apps][26]

7. Určí, které účet správce, který chcete použít ke správě zřizování uživatelů. Pro **role správce** tohoto účtu, upravit **oprávnění** pro tuto roli. Ujistěte se, obsahuje všechny **oprávnění rozhraní API Správce** povolené tak, aby tento účet slouží pro zřizování.
   
     ![Klikněte na Google Apps][27]
   
    > [!NOTE]
    > Pokud konfigurujete provozním prostředí, osvědčeným postupem je vytvoření účtu správce v Google Apps speciálně pro tento krok. Tyto účty musí mít roli správce s ním spojená s potřebnými oprávněními rozhraní API.
     
8. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

9. Pokud jste již nakonfigurovali Google Apps pro jednotné přihlašování, vyhledávání pro instanci služby Google Apps pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Google Apps** v galerii aplikací. Vyberte Google Apps ve výsledcích hledání a přidejte ji do seznamu aplikací.

10. Vyberte instanci služby Google Apps a pak vyberte **zřizování** kartě.

11. Nastavte **režimu zřizování** k **automatické**. 

     ![Zřizování](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. V části **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Otevře se dialogové okno Google Apps autorizace v nové okno prohlížeče.

13. Potvrďte, že byste chtěli poskytnout Azure Active Directory oprávnění k provádění změn pro vašeho klienta Google Apps. Klikněte na tlačítko **přijmout**.
    
     ![Zkontrolujte oprávnění.][28]

14. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Google Apps. Pokud se nepovede připojit, ujistěte se, váš účet Google Apps má oprávnění správce týmu a zkuste to **"Ověřit"** krok opakujte.

15. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.

16. Klikněte na tlačítko **uložit.**

17. V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Google Apps.**

18. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Google Apps. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Google Apps pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

19. Chcete-li povolit zřizování pro Google Apps služby Azure AD, změňte **Stav zřizování** k **na** v sekci nastavení

20. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené ke Google Apps v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Google Apps.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png