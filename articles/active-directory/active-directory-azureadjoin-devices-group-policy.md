---
title: "Připojení zařízení připojených k doméně ke službě Azure AD pro Windows 10 vyskytne | Microsoft Docs"
description: "Vysvětluje, jak může správce nakonfigurovat zásady skupiny k zařízením povolit, aby doméně k podnikové síti."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 9c91579d20bb84701f6d0b97d944728c84044adf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe
Připojení k doméně je že tradičním způsobem, jakým organizace mají připojená zařízení pro práci pro posledních 15 let a další. Má povoleno přihlašování ke svým zařízením pomocí práci Windows Server Active Directory (aktivní adresář) nebo školní účty uživatelů a povoleny IT pro plnohodnotnou správu těchto zařízení. Organizace se zpravidla spoléhají na vytváření bitové kopie metody pro zřizování zařízení pro uživatele a obvykle použijte System Center Configuration Manager (SCCM) nebo zásady skupiny k jejich správě.


Připojení k doméně v systému Windows 10 poskytuje následující výhody po připojení zařízení k Azure Active Directory (Azure AD):

* Jednotné přihlašování (SSO) k prostředkům Azure AD odkudkoli.
* Přístup k podnikové síti Windows Store pomocí pracovní nebo školní účty (bez účtu Microsoft vyžaduje)
* Kompatibilní se standardem Enterprise cestovní nastavení uživatele v zařízeních pomocí pracovní nebo školní účty (bez účtu Microsoft požadované)
* Silné ověřování a pohodlný přihlášení pro pracovní nebo školní účty s Windows Hello pro firmy a Windows Hello
* Umožňuje omezit přístup jenom na zařízení, které jsou v souladu s nastaveními zásad skupiny organizační zařízení

## <a name="prerequisites"></a>Požadavky
Připojení k doméně je nadále užitečné. Ale na získat výhody Azure AD jednotné přihlašování, cestovní nastavení s pracovní nebo školní účty a přístup k Windows Store s pracovní nebo školní účty, budete potřebovat následující:

* Předplatné Azure AD
* Azure AD Connect místní adresář rozšířit do Azure AD
* Zásady, které má nastavený na připojení zařízení připojených k doméně ke službě Azure AD
* Sestavení Windows 10 (sestavení 10551 nebo novější) pro zařízení

Pokud chcete povolit Windows Hello pro firmy a Windows Hello, budete také potřebovat následující:

- **Infrastruktury veřejných klíčů (PKI)** pro vystavování certifikátů uživatele.

- **System Center Configuration Manager aktuální větev** – je potřeba nainstalovat verzi 1606 nebo vyšší.  
Další informace naleznete v tématu: 
    - [Dokumentace pro System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx)
    - [Blog týmu System Center Configuration Manager](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [Windows Hello pro firmy nastavení v nástroji System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

Jako alternativu k požadavcích nasazení infrastruktury veřejných KLÍČŮ můžete provést následující:

* Máte několik řadičů domény s Windows Server 2016 Active Directory Domain Services.

Chcete-li povolit podmíněný přístup, můžete vytvořit nastavení zásad skupiny, které umožňují přístup k zařízení připojených k doméně pomocí žádné další nasazení. Ke správě řízení přístupu podle stavu kompatibility zařízení, budete potřebovat následující:

* System Center Configuration Manager aktuální větev (1606 nebo novější) pro Windows Hello pro firmy scénáře

## <a name="deployment-instructions"></a>Pokyny k nasazení

Pokud chcete nasadit, postupujte podle kroků uvedených v [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="next-step"></a>Další krok
* [Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

