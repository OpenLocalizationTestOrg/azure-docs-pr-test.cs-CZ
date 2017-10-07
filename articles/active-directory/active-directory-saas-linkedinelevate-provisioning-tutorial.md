---
title: "Kurz: Konfigurace LinkedIn zvýšení oprávnění pro uživatele automatické zřizování s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooLinkedIn zvýšení."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 08201c078ece0054e75ec0c004840e5186e0e704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-elevate-for-automatic-user-provisioning"></a>Kurz: Konfigurace LinkedIn zvýšení oprávnění pro uživatele automatické zřizování


cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v LinkedIn zvýšení oprávnění a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooLinkedIn zvýšení. 

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klient služby Azure Active Directory
*   Klienta LinkedIn zvýšení oprávnění 
*   Účet správce v zvýšení oprávnění LinkedIn s toohello přístup k centru účtů LinkedIn

> [!NOTE]
> Azure Active Directory se integruje s LinkedIn zvýšení oprávnění pomocí hello [SCIM](http://www.simplecloud.info/) protokolu.

## <a name="assigning-users-toolinkedin-elevate"></a>Přiřazení uživatelů tooLinkedIn zvýšení

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se budou synchronizovat pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD. 

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooLinkedIn zvýšení. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooLinkedIn zvýšení podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-elevate"></a>Důležité tipy pro přiřazení uživatelů tooLinkedIn zvýšení

*   Dále je doporučeno jednoho uživatele Azure AD přiřadit tooLinkedIn zvýšení tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování uživatelů tooLinkedIn zvýšení, je nutné vybrat hello **uživatele** role v dialogovém okně přiřazení hello. role "Výchozí přístup" Hello nefunguje pro zřizování.


## <a name="configuring-user-provisioning-toolinkedin-elevate"></a>Konfigurace tooLinkedIn zvýšení zřizování uživatelů

Tato část vás provede připojením zvýšení vaší služby Azure AD tooLinkedIn SCIM uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v LinkedIn zvýšení oprávnění na základě uživatele a skupiny přiřazení ve službě Azure AD.

**Tip:** můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro zvýšení oprávnění LinkedIn, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-elevate-in-azure-ad"></a>tooconfigure automatické uživatelský účet zřizování tooLinkedIn zvýšení ve službě Azure AD:


prvním krokem Hello je tooretrieve LinkedIn přístupový token. Pokud jste správce podnikové sítě, můžete zřídit samoobslužné přístupový token. V centru váš účet, přejděte příliš**nastavení &gt; globální nastavení** a otevřete hello **SCIM instalace** panelu.

> [!NOTE]
> Pokud se připojujete centra účtů hello přímo a nikoli v rámci odkaz, můžete dosáhnout pomocí následujících kroků hello.

1)  Přihlaste se tooAccount Center.

2)  Vyberte **správce &gt; nastavení správce** .

3)  Klikněte na tlačítko **Advanced integrace** na levém bočním panelu hello. Jste směrovanou toohello centra účtů.

4)  Klikněte na tlačítko **+ přidat novou konfiguraci SCIM** a použijte postup hello vyplněním každé pole.

> Když autoassign licencí není povolen, znamená to, že je synchronizovat pouze data uživatele.

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate1.PNG)

> Pokud je povoleno autolicense přiřazení, budete potřebovat toonote instanci aplikace a typ licence. Licence jsou přiřazeny na první přijde, nejprve sloužit základ, dokud jsou provedeny všechny licence hello.

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  Klikněte na tlačítko **vygenerovat token**. Měli byste vidět zobrazení tokenu přístupu v části hello **přístupový token** pole.

6)  Uložte přístup tokenu tooyour schránky nebo počítač před opuštěním stránky hello.

7) V dalším kroku přihlásit toohello [portál Azure](https://portal.azure.com)a vyhledejte toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

8) Pokud jste již nakonfigurovali LinkedIn zvýšení oprávnění pro jednotné přihlašování, vyhledejte instanci LinkedIn zvýšení oprávnění pomocí pole hledání hello. Jinak vyberte možnost **přidat** a vyhledejte **zvýšení oprávnění LinkedIn** v galerii aplikací hello. Vyberte výsledky hledání hello LinkedIn zvýšení oprávnění a přidejte ji tooyour seznam aplikací.

9)  Vyberte instanci LinkedIn zvýšení oprávnění a potom vyberte hello **zřizování** kartě.

10) Sada hello **režimu zřizování** příliš**automatické**.

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  Vyplňte následující pole v části hello **přihlašovací údaje správce** :

* V hello **URL klienta** zadejte https://api.linkedin.com.

* V hello **tajný klíč tokenu** pole, zadejte hello přístupový token jste vygenerovali v kroku 1 a klikněte na **Test připojení** .

* Měli byste vidět úspěšné oznámení na straně ÚETUChcete hello portálu.

12) Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.

13) Klikněte na **Uložit**. 

14) V hello **mapování atributů** , projděte si atributy hello uživatelů a skupin, které se mají synchronizovat ze služby Azure AD tooLinkedIn zvýšení. Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello uživatelské účty a skupiny v LinkedIn zvýšení oprávnění pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) tooenable hello zřizování služby Azure AD pro LinkedIn zvýšení oprávnění, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

16) Klikněte na **Uložit**. 

Tato akce spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooLinkedIn zvýšení v části Uživatelé a skupiny hello. Všimněte si, že počáteční synchronizace hello bude trvat déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci LinkedIn zvýšení oprávnění.


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
