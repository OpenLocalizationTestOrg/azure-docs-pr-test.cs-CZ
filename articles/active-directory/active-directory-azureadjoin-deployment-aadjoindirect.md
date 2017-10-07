---
title: "aaaUsage scénáře a aspekty nasazení pro připojení ke službě Azure AD | Microsoft Docs"
description: "Vysvětluje, jak mohou správci nastavit službu Azure AD Join pro své koncové uživatele (zaměstnanci, studenty, ostatní uživatelé). Popisuje také hello různých reálných scénářů pro použití připojení ke službě Azure AD."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 7e57971481aa312ebf8a69999d194f9dcc3d4708
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Scénáře využití a aspekty nasazení pro Azure AD Join
## <a name="usage-scenarios-for-azure-ad-join"></a>Scénáře použití pro připojení ke službě Azure AD
### <a name="scenario-1-businesses-largely-in-hello-cloud"></a>Scénář 1: Podnikům do značné míry v cloudu hello
Azure Active Directory Join (Azure AD Join) vám může hodit Pokud aktuálně fungovat a Správa identit pro vaši firmu v cloudu hello nebo brzy přesouváte toohello cloudu. Můžete použít účet, který jste vytvořili v Azure AD toosign v tooWindows 10. Prostřednictvím [hello prvním spuštění procesu prostředí (FRX)](active-directory-azureadjoin-user-frx.md), nebo díky připojení ke službě Azure AD z [nabídky nastavení hello](active-directory-azureadjoin-user-upgrade.md), uživatelé se můžete zapojit do jejich počítačů tooAzure AD.  Uživatelé mohou také získejte jednotné přihlašování (SSO) přístup příliš cloudové prostředky jako je Office 365 ve svém prohlížeči zakázanou nebo v aplikacích Office.

### <a name="scenario-2-educational-institutions"></a>Scénář 2: Vzdělávací instituce
Vzdělávací instituce obvykle mají dva typy uživatelů: zaměstnance školy a studenti. Zaměstnance školy členové jsou považovány za dlouhodobější členy hello organizace. Vytváření místních účtů pro ně je žádoucí. Ale studenti, kteří jsou členy shorter-term hello organizace a je možné spravovat své účty ve službě Azure AD. To znamená, že adresář škálování mohou poslat toohello cloudu namísto uloženého místně. Také znamená, že studenty bude možné toosign v tooWindows s účty služby Azure AD a získání přístupu v aplikacích Office 365 prostředky tooOffice.

### <a name="scenario-3-retail-businesses"></a>Scénář 3: Prodejní firmách
Prodejní firmy mají sezónních pracovníků a dlouhodobé zaměstnanci. Obvykle vytvoříte místní účty a připojený k doméně počítače použít pro dlouhodobější zaměstnance na plný úvazek. Ale sezónních pracovníků shorter-term členy hello organizace, a je žádoucí toomanage své účty, kde uživatelské licence můžete snadno přesouvat. Když vytvoříte své uživatelské účty v cloudu hello s licencemi Office 365, tito uživatelé získat výhody hello podepisování tooWindows a v aplikacích Office pomocí účtu Azure AD, při zachování větší flexibilitu s jejich licencí po opustí.

### <a name="scenario-4-additional-scenarios"></a>Scénář 4: Další scénáře
Společně s hello výhody už jsme probírali výše můžete využívat s kvůli zjednodušené spojující prostředí, Správa efektivní zařízení, registrace správu automatické mobilních zařízení a tooAzure přihlášení připojit své zařízení tooAzure AD uživatelům AD a místní prostředky.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Informace o nasazení pro Azure AD Join
### <a name="enable-your-users-toojoin-a-company-owned-device-directly-tooazure-ad"></a>Povolit vaši uživatelé toojoin zařízení ve vlastnictví společnosti přímo tooAzure AD
Podniky můžete zadat jen cloudové účty toopartner společnosti a organizace. Těchto partnerů můžete pak snadný přístup k firemním aplikacím a prostředkům pomocí jednotného přihlašování. Tento scénář je použít toousers, kdo přístup k prostředkům především v cloudu hello, jako je například Office 365 nebo SaaS aplikací, které jsou závislé na službě Azure AD pro ověřování.

### <a name="prerequisites"></a>Požadavky
**Na úrovni podniku hello (správce)**

* Předplatné Azure s Azure Active Directory  

**Na úrovni uživatele hello**

* Windows 10 (edice Professional a Enterprise)

### <a name="administrator-tasks"></a>Úlohy správce
* [Nastavení registrace zařízení](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Úlohy uživatele
* [Nastavit nové zařízení Windows 10 s Azure AD během instalace](active-directory-azureadjoin-user-frx.md)
* [Nastavit v nabídce nastavení hello zařízením s Windows 10 s Azure AD](active-directory-azureadjoin-user-upgrade.md)
* [Připojení osobní organizaci tooyour zařízení Windows 10](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Povolit model BYOD ve vaší organizaci pro Windows 10
Vaše uživatele a zaměstnanci toouse můžete nastavit jejich osobní Windows zařízení (BYOD) tooaccess firemním aplikacím a prostředkům. Vaši uživatelé můžete přidat svých prostředků Azure AD účty (pracovní nebo školní účty) tooa osobní Windows zařízení tooaccess způsobem zabezpečení a dodržování.

### <a name="prerequisites"></a>Požadavky
**Na úrovni podniku hello (správce)**

* Předplatné Azure AD

**Na úrovni uživatele hello**

* Windows 10 (edice Professional a Enterprise)

### <a name="administrator-tasks"></a>Úlohy správce
* [Nastavení registrace zařízení](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Úlohy uživatele
* [Připojení osobní organizaci tooyour zařízení Windows 10](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

