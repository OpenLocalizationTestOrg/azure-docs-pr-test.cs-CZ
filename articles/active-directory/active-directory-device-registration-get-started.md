---
title: "aaaHow tooconfigure Automatická registrace zařízení připojených k doméně systému Windows s Azure Active Directory | Microsoft Docs"
description: "Nastavte vaší doméně Windows zařízení tooregister automaticky a bezobslužně s Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1d736eba734418231f12e23a8fc1a93405f129c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Začínáme s registrací zařízení ve službě Azure Active Directory

Registrace zařízení služby Azure Active Directory je hello základem pro scénáře podmíněného přístupu podle zařízení. Když je zařízení registrováno, poskytne mu nástroj registrace zařízení služby Azure Active Directory hello zařízení s identitou, která je použité tooauthenticate hello zařízení při přihlášení uživatele hello. Hello ověření zařízení a atributy hello hello zařízení, lze potom použít tooenforce zásady podmíněného přístupu pro aplikace, které jsou hostované v cloudu hello a místní.

V kombinaci s řešením správy mobilních zařízení, jako je například Microsoft Intune, hello atributy zařízení ve službě Azure Active Directory jsou aktualizované o další informace o zařízení hello. Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů. Další informace o registraci zařízení v Microsoft Intune najdete v tématu registrace zařízení pro správu v Intune.
Scénáře pro nástroj pomocí Azure Active Directory zařízení registrace Azure Active Directory Device Registration zahrnuje podporu pro iOS, Android a Windows zařízení. Hello jednotlivé scénáře použití nástroje registrace zařízení služby Azure AD mohou mít konkrétněji určené požadavky a podporované platformy. 

Jedná se o následující scénáře:

- **Podmíněný přístup pro aplikace Office 365 s Microsoft Intune:** správci IT mohou poskytnout podmíněného přístupu zařízení zásady toosecure podnikovým prostředkům, zatímco v hello stejný čas povolení pracovníkům s vhodnými zařízeními tooaccess hello služby. Další informace naleznete v tématu Zásady podmíněného přístupu zařízení pro služby Office 365.

- **Tooapplications podmíněného přístupu, které jsou hostované na místě:** registrovaná zařízení můžete použít se zásadami přístupu pro aplikace, které jsou nakonfigurované toouse služby AD FS v systému Windows Server 2012 R2. Další informace o nastavení podmíněného přístupu pro místní úložiště naleznete v tématu [Nastavení místního podmíněného přístupu pomocí registrace zařízení služby Azure Active Directory](active-directory-device-registration-on-premises-setup.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Nastavení nástroje Registrace zařízení ve službě Azure Active Directory

registrace zařízení toosetup, máte několik možností:

- Můžete zaregistrovat zařízení, kdy připojený k tooAzure AD domény. Další informace v tomto tématu můžete další informace o nastavení připojení ke službě Azure AD a hello pro uživatele domény toojoin Azure AD.

- Zařízení můžete zaregistrovaná, když uživatelé přidají pracovní nebo školní účty tooWindows na osobním zařízení nebo při připojení tooa pracovním prostředkům nutnosti registrace mobilních zařízení. tooensure, musíte povolit registraci zařízení služby Azure AD ve hello portálu Azure. 

- Zařízení lze registrovat pomocí automatické registrace zařízení pro tradiční počítače připojené k doméně. tooensure tím, než budete pokračovat s Automatická registrace zařízení se musí první instalaci Azure AD Connect.

Nejnovější pokyny najdete v tématu [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).  
Můžete také zkontrolovat a povolit nebo zakázat registrovaná zařízení pomocí portálu správce hello v Azure Active Directory.

## <a name="enable-hello-azure-active-directory-device-registration-service"></a>Povolení služby registrace zařízení služby Azure Active Directory hello

**tooenable hello služby Azure Active Directory device registration service:**

1.  Přihlaste se toohello portálu Microsoft Azure jako správce.

2.  V levém podokně hello vyberte **služby Active Directory**.

3.  Na kartě hello adresář vyberte svůj adresář.

4.  Klikněte na **Konfigurovat**.

5.  Posuňte se příliš**zařízení**.

6.  Vyberte možnost Všichni uživatelé mohou REGISTROVAT SVÁ zařízení s AZURE AD.

7.  Vyberte maximální počet hello zařízení chcete tooauthorize na uživatele.

Hello registrace s Microsoft Intune nebo Správa mobilních zařízení pro Office 365 vyžaduje registraci zařízení. Pokud jste nakonfigurovali některou z těchto služeb **všechny** je vybraná a **NONE** je zakázána. Zkontrolujte, zda jsou správně nakonfigurované a mít odpovídající licencování hello.

Ve výchozím nastavení není povoleno dvoufaktorové ověřování pro službu hello. Dvoufaktorové ověřování ale doporučujeme při registraci zařízení.

- Před pro tuto službu, které vyžadují dvoufaktorové ověřování, musíte nakonfigurovat poskytovatele dvoufaktorového ověřování ve službě Azure Active Directory a nakonfigurovat uživatelské účty pro službu Multi-Factor Authentication, najdete v části Přidání služby Multi-Factor Authentication tooAzure služby Active Directory

- Pokud používáte službu AD FS v systému Windows Server 2012 R2, musíte modul dvoufaktorového ověřování nakonfigurovat ve službě AD FS, najdete v části Použití vícefaktorového ověřování službou Active Directory Federation Services.

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Zobrazení a správa objektů zařízení ve službě Azure Active Directory

Z portálu správce Azure hello můžete zobrazit, zablokovat a odblokovat zařízení. Zařízení, který je blokovaný nebude mít přístup tooapplications, které jsou nakonfigurované tooallow pouze registrovaná zařízení.

**tooview a Správa objektů zařízení ve službě Azure Active Directory:**
 
1.  Přihlaste se na toohello portálu Microsoft Azure jako správce.

2.  V levém podokně hello vyberte **služby Active Directory**.

3.  Vyberte svůj adresář.

4.  Vyberte **uživatelé**. 

5.  Klikněte na tlačítko hello uživatele, pro které chcete toosee hello zařízení.

6.  Vyberte **zařízení**.

7.  Vyberte **registrované zařízení**.

Nyní můžete zkontrolovat, blokování nebo odblokování hello uživatele registrovaná zařízení.
Zařízení s Windows 10, které jsou místně připojených k doméně a automaticky registrovaný se nezobrazí v kartě Uživatelé hello. Použijte toofind příkaz prostředí PowerShell Get-MsolDevice hello všechna podniková zařízení. 


## <a name="next-steps"></a>Další kroky

toosetup automatické registrace zařízení, najdete v části [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).


