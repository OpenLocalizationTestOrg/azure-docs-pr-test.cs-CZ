---
title: "aaaIntroduction toodevice správy ve službě Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak Správa zařízení vám může pomoct tooget kontrolu nad hello zařízení, která přistupují k prostředkům ve vašem prostředí."
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
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: e2fc0a3e8d00dc69cf01db9074e34427e396cfcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodevice-management-in-azure-active-directory"></a>Úvod toodevice správy ve službě Azure Active Directory

První mobilní, cloudové první světě Azure Active Directory (Azure AD) umožňuje toodevices přihlašování, aplikacím a službám odkudkoli. S hello jak narůstá počet zařízení – PŘINESTE si vlastní zařízení (), včetně Odborníci v oblasti IT potýkají s dva dosáhnout cíle:

- Posílení produktivity toobe koncoví uživatelé hello kdykoli a kdekoli
- Ochrana podnikových prostředků hello kdykoli

Pomocí zařízení jsou vaši uživatelé získávání přístupu tooyour podnikových prostředků. tooprotect vaše podnikové prostředky, jako správce IT je vhodné toohave řízení těchto zařízení. To vám umožní toomake se, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů. 

Správa zařízení je také hello základ pro [podmíněného přístupu na základě zařízení](active-directory-conditional-access-policy-connected-applications.md). Pomocí podmíněného přístupu podle zařízení můžete zajistit, aby přístupu, kterou tooresources ve vašem prostředí je možné jenom s důvěryhodných zařízení.   

Toto téma vysvětluje, jak funguje správa zařízení ve službě Azure Active Directory.

## <a name="getting-devices-under-hello-control-of-azure-ad"></a>Získávání zařízení pod kontrolou hello Azure AD

tooget zařízení pod kontrolou hello Azure AD, máte dvě možnosti:

- Registrace 
- Připojení

**Registrace** zařízení tooAzure AD umožňuje vám toomanage identity zařízení. Když je zařízení registrováno, poskytne mu nástroj registrace zařízení služby Azure AD hello zařízení s identitou, která je použité tooauthenticate hello zařízení při tooAzure přihlášení uživatele AD. Můžete použít hello identity tooenable nebo zakázat zařízení.

V kombinaci s řešením správy mobilních zařízení, jako je například Microsoft Intune, hello atributy zařízení ve službě Azure AD jsou aktualizované o další informace o zařízení hello. Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů. Další informace o registraci zařízení v Microsoft Intune najdete v tématu registrace zařízení pro správu v Intune.

**Připojení** zařízení je rozšíření tooregistering zařízení. To znamená, poskytne vám registrace zařízení a přidání toothis všechny výhody hello, změní také hello místní stav zařízení. Změna stavu místní hello umožňuje tooa toosign v zařízení uživatele pomocí organizace pracovní nebo školní účet místo osobní účet.

## <a name="azure-ad-registered-devices"></a>Azure AD registrované zařízení   

Hello cílem zařízení zaregistrovat Azure AD je s podporovaných pro hello tooprovide **PŘINESTE si vlastní zařízení ()** scénář. V tomto scénáři může uživatel přístup k prostředkům vaší organizace Azure Active Directory řídí použití osobních zařízení.  

![Azure AD registrované zařízení](./media/device-management-introduction/03.png)

Hello přístup je založen na pracovní nebo školní účet, který byl zadán v zařízení hello.  
Například Windows 10 umožňuje uživatelům tooadd pracovní nebo školní účet tooa osobní počítače, tabletu nebo telefonu.  
Když uživatel přidal pracovního nebo školního účtu, zařízení hello je zaregistrován s Azure AD a volitelně zaregistrované v systému pro správu (MDM) mobilních zařízení hello nakonfigurovaný vaší organizace. Uživatelé vaší organizace můžete přidat pracovní nebo školní účet tooa osobní zařízení pohodlně:

- Při přístupu k aplikaci pracovní pro hello nejprve čas.
- Ručně prostřednictvím hello **nastavení** nabídky v případě hello Windows 10 

Pro Windows 10, iOS, Android a systému macOS můžete konfigurovat Azure AD, které registrované zařízení.

## <a name="azure-ad-joined-devices"></a>Zařízení připojená k Azure AD

cílem Hello zařízení připojených k Azure AD je toosimplify:

- Zařízení ve vlastnictví pracovní nasazení systému Windows 
- Přístup k tooorganizational aplikacím a prostředkům z libovolného zařízení Windows

![Azure AD registrované zařízení](./media/device-management-introduction/02.png)


Jsou těchto cílů dosáhnout tím, že poskytuje uživatelům samoobslužné služby prostředí pro získání zařízení ve vlastnictví pracovní pod kontrolou hello Azure AD.  
**Azure AD Join** je určená pro organizace, které jsou cloudu první / jenom pro cloud. Obvykle jsou to malé a střední firmy, které nemají místní infrastrukturu Windows Server Active Directory. 

Implementace služby Azure AD, které jsou připojené k zařízení vám poskytne hello následující výhody:

- **Jednotného přihlašování (SSO)** tooyour Azure spravované SaaS aplikace a služby. Vaši uživatelé nevidíte další ověřování výzvy při přístupu k pracovním prostředkům. Hello funkce jednotného přihlašování je i když nejsou připojené toohello doménové síti dostupné.

- **Enterprise kompatibilní roaming** uživatelská nastavení napříč připojená. Uživatelé nepotřebují tooconnect nastavení toosee účtu (například Hotmail) Microsoft mezi zařízeními.

- **Přístup k tooWindows Store pro firmy** účtem AD. Uživatele můžete vybrat z inventář aplikací předem vybraná hello organizací.

- **Windows Hello** podpora pro přístup k zabezpečení a pohodlný toowork prostředkům.

- **Omezení přístupu** tooapps z jenom zařízení, které splňují zásady dodržování předpisů.

Při připojení k Azure AD je primárně určený pro organizace, které nemají místní infrastrukturu Windows Server Active Directory, jistě můžete také použít ve scénářích kde:

- Připojení k místní doméně nelze použít například, pokud potřebujete tooget mobilní zařízení, jako jsou tablety a telefony pod kontrolou.

- Uživatelé potřebovat především tooaccess Office 365 nebo jiné aplikace SaaS integrované s Azure AD.

- Chcete toomanage skupinu uživatelů ve službě Azure AD místo ve službě Active Directory. To můžete použít například pracovníci tooseasonal, dodavatelů nebo studenty.

- Chcete tooprovide spojující možnosti tooworkers ve firemní pobočky ve vzdálených pobočkách s omezenou na místní infrastrukturu.

Můžete nakonfigurovat zařízení připojených k Azure AD pro zařízení s Windows 10.


## <a name="hybrid-azure-ad-joined-devices"></a>Zařízení připojená k hybridní Azure AD

Pro více než deset použili řada organizací hello domény spojení tootheir místní služby Active Directory tooenable:

- IT oddělení toomanage pracovní zařízení ve vlastnictví z centrálního umístění.

- Uživatelé toosign v tootheir zařízení s jejich služby Active Directory pracovní nebo školní účty. 

Obvykle organizace s nároky místní spoléhají na zařízení tooprovision metody pro zpracování obrázků a často používají **System Center Configuration Manager (SCCM)** nebo **zásady (zásady skupiny) skupiny** toomanage je.

Pokud vaše prostředí disponuje místní AD nároky a vy chcete také výhody z hello možnosti poskytované službou Azure Active Directory, můžete implementovat hybridní Azure AD, které jsou připojené k zařízení. Jedná se o zařízení, které jsou obě, připojený k tooyour místní služby Active Directory a Azure Active Directory.

![Azure AD registrované zařízení](./media/device-management-introduction/01.png)


Pokud byste měli používat Azure AD hybridní připojené k zařízení:

- Máte Win32 aplikace nasazené toothese zařízení, které používají protokol NTLM nebo Kerberos.

- Vyžadujete, zásady skupiny nebo SCCM nebo DCM toomanage zařízení.

- Chcete toocontinue toouse vytváření bitové kopie řešení tooconfigure zařízení pro vaši zaměstnanci.

Můžete nakonfigurovat hybridní Azure AD pro Windows 10 a zařízení nižší úrovně, jako jsou Windows 8 a Windows 7 připojené zařízení.

## <a name="summary"></a>Souhrn

Správa zařízení ve službě Azure AD můžete: 

- Zjednodušení hello uvedení zařízení pod kontrolou hello Azure AD

- Poskytovat uživatelům snadno toouse přístup tooyour organizace cloudové prostředky

Jako pravidlo Flash měli byste použít:

- Azure AD registrované zařízení pro osobní zařízení

- Azure AD připojené zařízení pro zařízení, které nejsou připojené k tooan místní AD 

- Hybridní Azure AD připojené zařízení pro zařízení, které jsou připojené k tooan místní AD     




## <a name="next-steps"></a>Další kroky

- tooget přehled o tom, jak hello toomanage zařízení v Azure portálu, najdete v tématu [Správa zařízení pomocí portálu Azure hello](device-management-azure-portal.md)

- toolearn Další informace o podmíněného přístupu podle zařízení, najdete v části [konfigurovat zásady podmíněného přístupu na základě zařízení služby Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).

- toosetup hybridní Azure AD, které jsou připojené k zařízení, najdete v části [jak zařízení připojená k hybridní tooconfigure Azure Active Directory](device-management-hybrid-azuread-joined-devices-setup.md).


