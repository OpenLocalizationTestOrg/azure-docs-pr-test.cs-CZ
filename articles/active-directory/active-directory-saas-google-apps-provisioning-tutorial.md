---
title: "Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů v Azure | Microsoft Docs"
description: "Zjistěte, jak tooautomatically zřídit a deaktivace zřízení uživatelských účtů ze služby Azure AD tooGoogle aplikace."
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
ms.openlocfilehash: d1fa8449bd6013d1627b3552aaa19db1c0f4f46f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a>Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Google Apps a službou Azure AD tooautomatically zřídit a zrušte zřízení uživatelských účtů z Azure AD tooGoogle aplikace.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Pro pracovní nebo Google Apps pro vzdělávací organizace musí mít platný klienta pro Google Apps. Bezplatný zkušební účet můžete použít buď služby.
*   Uživatelský účet v Google Apps s oprávněními správce týmu.

## <a name="assigning-users-toogoogle-apps"></a>Přiřazení uživatelů tooGoogle aplikace

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k aplikaci Google Apps tooyour. Jakmile se rozhodli, můžete přiřadit aplikaci Google Apps tooyour těchto uživatelů podle pokynů hello zde: [přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

> [!IMPORTANT]
>*   Dále je doporučeno jednoho uživatele Azure AD přiřadit tooGoogle aplikace tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.
>*   Při přiřazování tooGoogle uživatele aplikace, je nutné vybrat hello uživatele nebo roli "Skupina" v dialogovém okně přiřazení hello. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit zřizování automatizované uživatelů

Tato část vás provede připojením aplikace Azure AD tooGoogle uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Google Apps podle přiřazení uživatelů a skupin ve službě Azure AD .

>[!Tip]
>Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Google Apps, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu

> [!NOTE]
> Jiné vhodným řešením pro automatizaci zřizování aplikace tooGoogle uživatelů je toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) která zřizuje vaší místní služby Active Directory identity tooGoogle aplikace. Naproti tomu hello řešení v tomto kurzu zřídí uživatelů Azure Active Directory (cloud) a tooGoogle poštovní skupiny aplikací. 

1. Přihlaste se k hello [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce a klikněte na tlačítko **zabezpečení**. Pokud nevidíte odkaz hello, mohou být skryty pod hello **více ovládacích prvků** nabídky v hello dolní části obrazovky hello.
   
    ![Klikněte na Zabezpečení.][10]

2. Na hello **zabezpečení** klikněte na tlačítko **referenční dokumentace rozhraní API**.
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][15]

3. Vyberte **povolit rozhraní API pro přístup**.
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][16]

    > [!IMPORTANT]
    > Pro každého uživatele, že máte v úmyslu tooprovision tooGoogle aplikace, svoje uživatelské jméno ve službě Azure Active Directory *musí* být vázanou tooa vlastní doménu. Například uživatelská jména, které vypadají bob@contoso.onmicrosoft.com není přijat Google Apps, zatímco bob@contoso.com byla přijata. Existujícího uživatele domény můžete změnit úpravou jejich vlastnosti ve službě Azure AD. Pokyny pro jak tooset vlastní doménu pro Azure Active Directory a Google Apps jsou zahrnuty v následující kroky.
      
4. Pokud zatím jste nepřidali tooyour název vlastní domény Azure Active Directory, postupujte podle následujících kroků hello:
  
    a. V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**. V seznamu adresářů hello vyberte svůj adresář. 

    b. Klikněte na tlačítko **název domény** hello levém navigačním podokně a pak klikněte na tlačítko **přidat**.
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Přidání domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Zadejte název domény do hello **název domény** pole. Tento název domény by měl být hello stejným názvem domény, že máte v úmyslu toouse pro Google Apps. Až budete připravení, klikněte na tlačítko hello **přidáním domény** tlačítko.
     
     ![Název domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Klikněte na tlačítko **Další** toogo toohello ověření stránky. tooverify, že jste vlastníkem této domény, je nutné upravit záznamy DNS domény hello podle hodnoty toohello zadané na této stránce. Můžete se rozhodnout tooverify buď pomocí **záznamů MX** nebo **záznamů TXT**podle toho, co jste vybrali pro hello **typ záznamu** možnost. Komplexní pokyny, jak název domény tooverify s Azure AD, najdete v části [přidat vlastní tooAzure název domény AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Opakováním předchozích kroků pro všechny domény hello, že máte v úmyslu tooadd tooyour directory hello.

5. Teď, když si ověříte, že všechny vaše domény s Azure AD, je nutné nyní ověřit jejich znovu s Google Apps. Pro každou doménu, která již není registrován u služby Google Apps proveďte následující kroky hello:
   
    a. V hello [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **domény**.
     
     ![Klikněte na domény][20]

    b. Klikněte na tlačítko **přidejte k doméně nebo alias domény**.
     
     ![Přidání nové domény][21]

    c. Vyberte **přidat jiné domény**a pak zadejte název hello hello domény, které chcete tooadd.
     
     ![Zadejte název domény][22]

    d. Klikněte na tlačítko **pokračovat a ověření vlastnictví domény**. Potom postupujte podle tooverify hello kroky, které vlastníte hello název domény. Pro komplexní pokyny, jak tooverify doménu s Google Apps, najdete v článku. [Ověřit vlastnictví lokality s Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Opakováním předchozích kroků pro další domény, že máte v úmyslu tooadd tooGoogle aplikace hello.
     
     > [!WARNING]
     > Pokud změníte hello primární doménu vašeho klienta Google Apps, a pokud již máte nakonfigurovat jednotné přihlašování s Azure AD, pak máte toorepeat krok #3 pod [krok dva: Povolit jednotné přihlašování](#step-two-enable-single-sign-on).
       
6. V hello [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **rolí správce**.
   
     ![Klikněte na Google Apps][26]

7. Určit, které správce účtu chcete zřizování toouse toomanage uživatelů. Pro hello **role správce** tohoto účtu, upravit hello **oprávnění** pro tuto roli. Ujistěte se, obsahuje všechny hello **oprávnění rozhraní API Správce** povolené tak, aby tento účet slouží pro zřizování.
   
     ![Klikněte na Google Apps][27]
   
    > [!NOTE]
    > Pokud konfigurujete provozním prostředí, je osvědčeným postupem hello toocreate účet správce v Google Apps speciálně pro tento krok. Tyto účty musí mít roli správce s ním spojená s hello potřebná oprávnění na rozhraní API.
     
8. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

9. Pokud jste již nakonfigurovali Google Apps pro jednotné přihlašování, vyhledávání pro instanci služby Google Apps pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Google Apps** v galerii aplikací hello. Vyberte Google Apps z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

10. Vyberte instanci služby Google Apps a pak vyberte hello **zřizování** kartě.

11. Sada hello **režimu zřizování** příliš**automatické**. 

     ![Zřizování](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Otevře se dialogové okno Google Apps autorizace v nové okno prohlížeče.

13. Potvrďte, že byste chtěli toogive Azure Active Directory oprávnění toomake změn, které tooyour Google Apps klienta. Klikněte na tlačítko **přijmout**.
    
     ![Zkontrolujte oprávnění.][28]

14. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour aplikaci Google Apps. Pokud hello připojení nezdaří, zkontrolujte oprávnění správce Team má váš účet Google Apps a zkuste to hello **"Ověřit"** krok opakujte.

15. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

16. Klikněte na tlačítko **uložit.**

17. V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooGoogle aplikace.**

18. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooGoogle aplikace. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Google Apps pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

19. tooenable hello zřizování služby Azure AD pro Google Apps, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

20. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooGoogle aplikace v části Uživatelé a skupiny hello. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Google Apps.

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