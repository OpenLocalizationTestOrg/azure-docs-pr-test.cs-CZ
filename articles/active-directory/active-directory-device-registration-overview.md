---
title: "aaaAzure služby Active Directory device registration přehled | Microsoft Docs"
description: "Registrace zařízení služby Azure Active Directory je hello základem pro scénáře podmíněného přístupu podle zařízení. Když je zařízení registrováno, hello zřizuje registrace zařízení služby Azure Active Directory zařízení s identitou, která je použité tooauthenticate hello zařízení při přihlášení uživatele hello."
services: active-directory
keywords: "registrace zařízení, povolení registrace zařízení, registrace zařízení a MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 1e92c1a2-01b8-4225-950b-373cd601b035
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: b9846e34fe873d271ebe71cbf8e70c6b4768885b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Začínáme s registrací zařízení ve službě Azure Active Directory
Registrace zařízení služby Azure Active Directory je hello základem pro scénáře podmíněného přístupu podle zařízení. Když je zařízení registrováno, poskytne mu nástroj registrace zařízení služby Azure Active Directory hello zařízení s identitou, která je použité tooauthenticate hello zařízení při přihlášení uživatele hello. Hello ověření zařízení a atributy hello hello zařízení, lze potom použít tooenforce zásady podmíněného přístupu pro aplikace, které jsou hostované v cloudu hello a místní.

V kombinaci s řešením správy mobilních zařízení, jako je například Microsoft Intune, hello atributy zařízení ve službě Azure Active Directory jsou aktualizované o další informace o zařízení hello. Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů. Další informace o registraci zařízení v Microsoft Intune najdete v tématu [Registrace zařízení pro správu v Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Scénáře pro registraci zařízení ve službě Azure Active Directory
Registrace zařízení služby Azure Active Directory zahrnuje podporu pro zařízení se systémy iOS, Android a Windows. Hello jednotlivé scénáře použití nástroje registrace zařízení služby Azure AD mohou mít konkrétněji určené požadavky a podporované platformy. Jedná se o následující scénáře:

* **Tooapplications podmíněného přístupu, které jsou hostované na místě**: registrovaná zařízení můžete použít se zásadami přístupu pro aplikace, které jsou nakonfigurované toouse služby AD FS v systému Windows Server 2012 R2. Další informace o nastavení podmíněného přístupu pro místní úložiště naleznete v tématu [Nastavení místního podmíněného přístupu pomocí registrace zařízení služby Azure Active Directory](active-directory-device-registration-on-premises-setup.md).
* **Podmíněný přístup pro aplikace Office 365 s Microsoft Intune** : správci IT mohou poskytnout podmíněného přístupu zařízení zásady toosecure podnikovým prostředkům, zatímco v hello stejný čas povolení pracovníkům s vhodnými zařízeními tooaccess hello služby. Další informace najdete v tématu [Zásady podmíněného přístupu zařízení pro služby Office 365](active-directory-conditional-access-device-policies.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Nastavení registrace zařízení ve službě Azure Active Directory
Je třeba registrace zařízení služby Azure AD tooenable v hello portálu Azure tak, aby mohla mobilní zařízení zjistit službu hello vyhledáváním známých záznamů DNS. Záznamy DNS vaší společnosti musíte nakonfigurovat tak, aby zařízení s Windows 10, Windows 8.1, Windows 7, Android a iOS můžete zjišťovat a používat služby hello.
Můžete zobrazit a povolit nebo zakázat registrovaná zařízení pomocí portálu správce hello v Azure Active Directory.

> [!NOTE]
> Pro nejnovější pokyny, jak zjistit, tooset až automatické registrace zařízení [jak toosetup automatickou registraci domény systému Windows připojené zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
> 
> 

### <a name="enable-azure-active-directory-device-registration-service"></a>Povolení registrace zařízení ve službě Azure Active Directory
1. Přihlaste se toohello portálu Microsoft Azure jako správce.
2. V levém podokně hello vyberte **služby Active Directory**.
3. Na hello **Directory** vyberte adresáře.
4. Vyberte hello **konfigurace** kartě.
5. Posuňte se toohello části s názvem **zařízení**.
6. V poli **UŽIVATELÉ, KTEŘÍ MOHOU PŘIPOJIT ZAŘÍZENÍ K PRACOVIŠTI** vyberte **VŠICHNI**.
7. Vyberte maximální počet hello zařízení chcete tooauthorize na uživatele.

> [!NOTE]
> Registrace ke službám Microsoft Intune nebo Správa mobilních zařízení (MDM) pro Office 365 vyžaduje připojení k pracovišti. Pokud jste nakonfigurovali některou z těchto služeb, je zvolena možnost Všichni a tlačítko NONE hello je zakázána.
> 
> 

Ve výchozím nastavení není povoleno dvoufaktorové ověřování pro službu hello. Dvoufaktorové ověřování ale doporučujeme při registraci zařízení.

* Před pro tuto službu, které vyžadují dvoufaktorové ověřování, musíte nakonfigurovat poskytovatele dvoufaktorového ověřování ve službě Azure Active Directory a nakonfigurovat uživatelské účty pro službu Multi-Factor Authentication, najdete v části [přidání služby Multi-Factor TooAzure ověřování služby Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Pokud používáte službu AD FS a systém Windows Server 2012 R2, musíte modul dvoufaktorového ověřování nakonfigurovat ve službě AD FS. Více informací naleznete v článku [Použití služby Multi-Factor Authentication ve službě Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Konfigurace zjišťování registrace zařízení ve službě Azure Active Directory
Zařízení s Windows 7 a Windows 8.1 bude zjišťovat služby registrace zařízení hello kombinací hello název uživatelského účtu s názvem dobře známé zařízení registraci serveru.

Musíte vytvořit záznam DNS CNAME, který odkazuje záznam toohello A související s vaší službou registrace zařízení služby Azure Active Directory. Hello záznam CNAME musí použít hello známou předponu enterpriseregistration, za nímž následuje přípona UPN hello používá hello uživatelské účty ve vaší organizaci. Pokud vaše organizace používá více přípon UPN, je nutné v záznamech DNS vytvořit více záznamů CNAME.

Například, pokud používáte dvě přípony UPN ve vaší organizaci s názvem @contoso.com a @region.contoso.com, vytvoříte hello následující záznamy DNS.

| Záznam | Typ | Adresa |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Zobrazení a správa objektů zařízení ve službě Azure Active Directory
1. Z portálu správce Azure hello můžete zobrazit, zablokovat a odblokovat zařízení. Zařízení, který je blokovaný nebude mít přístup tooapplications, které jsou nakonfigurované tooallow pouze registrovaná zařízení.
2. Přihlaste se na toohello portálu Microsoft Azure jako správce.
3. V levém podokně hello vyberte **služby Active Directory**.
4. Vyberte svůj adresář.
5. Vyberte hello **uživatelé** kartě. Potom vyberte uživatele tooview svá zařízení
6. Vyberte hello **zařízení** kartě.
7. Vyberte **registrovaná zařízení** z hello rozevírací nabídce.
8. Zde si můžete zobrazit, zablokovat nebo odblokovat zařízení registrovaná uživateli hello.

## <a name="additional-topics"></a>Další témata
Zařízení se systémy Windows 7 a Windows 8.1 připojená k doméně můžete zaregistrovat pomocí registrace zařízení služby Azure AD. Následující témata Hello poskytuje další informace o požadavcích hello a registrace zařízení vyžaduje tooconfigure hello kroky na zařízeních s Windows 7 a Windows 8.1.

* [Automatická registrace zařízení ve službě Azure Active Directory u zařízení se systémem Windows připojených k doméně](active-directory-conditional-access-automatic-device-registration.md)
* [Automatická registrace zařízení ve službě Azure Active Directory u zařízení se systémem Windows 10 připojených k doméně](active-directory-azureadjoin-devices-group-policy.md)

