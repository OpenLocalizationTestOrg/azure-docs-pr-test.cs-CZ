---
title: "vyskytne aaaConnect tooAzure připojená k doméně AD pro Windows 10 | Microsoft Docs"
description: "Vysvětluje, jak správci můžou nakonfigurovat zásady skupiny tooenable zařízení toobe připojený k doméně toohello podnikové sítě."
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
ms.openlocfilehash: 9766aa702352dea2ecad3a9a0bdf8d3286ee6d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-domain-joined-devices-tooazure-ad-for-windows-10-experiences"></a>Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10
Připojení k doméně je hello tradiční způsob organizace mají připojená zařízení pro práci pro hello posledních 15 let a další. Aktivoval toosign uživatele v zařízení tootheir pomocí práci Windows Server Active Directory (aktivní adresář) nebo školní účty a povolených toofully IT spravovat tato zařízení. Organizace se zpravidla spoléhají na imaging metody tooprovision zařízení toousers a obecně používat toomanage System Center Configuration Manager (SCCM) nebo zásad skupiny je.


Připojení k doméně v systému Windows 10 vám poskytne hello po připojení zařízení tooAzure služby Active Directory (Azure AD) následující výhody:

* Jeden přihlašování (SSO) tooAzure AD prostředkům z libovolného místa
* Přístup k podnikovým toohello Windows Store pomocí pracovní nebo školní účty (bez účtu Microsoft požadované)
* Kompatibilní se standardem Enterprise cestovní nastavení uživatele v zařízeních pomocí pracovní nebo školní účty (bez účtu Microsoft požadované)
* Silné ověřování a pohodlný přihlášení pro pracovní nebo školní účty s Windows Hello pro firmy a Windows Hello
* Možnost toorestrict přístup pouze toodevices, že jsou v souladu s nastaveními zásad skupiny organizační zařízení

## <a name="prerequisites"></a>Požadavky
Připojení k doméně pokračuje toobe užitečné. Ale tooget hello Azure AD výhody jednotné přihlašování, cestovní nastavení s pracovní nebo školní účty a přístup k úložišti tooWindows s pracovní nebo školní účty, budete potřebovat hello následující:

* Předplatné Azure AD
* Azure AD Connect tooextend hello místní adresář tooAzure AD
* Zásady, které nastavil tooAzure tooconnect připojená k doméně AD
* Sestavení Windows 10 (sestavení 10551 nebo novější) pro zařízení

tooenable Windows Hello pro firmy a Windows Hello, budete také potřebovat hello následující:

- **Infrastruktury veřejných klíčů (PKI)** pro vystavování certifikátů uživatele.

- **System Center Configuration Manager aktuální větve** -potřebovat tooinstall verze 1606 nebo vyšší.  
Další informace naleznete v tématu: 
    - [Dokumentace pro System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx)
    - [Blog týmu System Center Configuration Manager](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [Windows Hello pro firmy nastavení v nástroji System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

Jako alternativní toohello infrastruktury veřejných KLÍČŮ nasazení požadavek, můžete provést následující hello:

* Máte několik řadičů domény s Windows Server 2016 Active Directory Domain Services.

tooenable podmíněný přístup, můžete vytvořit nastavení zásad skupiny, které umožňují přístup k zařízení připojená k toodomain s žádné další nasazení. řízení přístupu toomanage založené na dodržování předpisů hello zařízení, budete potřebovat následující hello:

* System Center Configuration Manager aktuální větev (1606 nebo novější) pro Windows Hello pro firmy scénáře

## <a name="deployment-instructions"></a>Pokyny k nasazení

toodeploy, postupujte podle kroků hello uvedených v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="next-step"></a>Další krok
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

