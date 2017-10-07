---
title: "nižší úrovně zařízení připojená k hybridní aaaTroubleshooting Azure Active Directory | Microsoft Docs"
description: "Řešení potíží s hybridní Azure Active Directory připojené zařízení nižší úrovně."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: edd56b89579fac6b427732902284ad9c568b87b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Nižší úrovně zařízení připojená k řešení potíží s hybridní Azure Active Directory 

Toto téma se vztahuje pouze toohello následující zařízení: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Pro Windows 10 nebo Windows Server 2016, najdete v části [hybridní Poradce při potížích s Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello následující scénáře:

- Podmíněný přístup využívající zařízení

- [Enterprise cestovní nastavení](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello pro firmy](active-directory-azureadjoin-passport-deployment.md) 





Toto téma poskytuje pokyny o tom, jak tooresolve potenciální problémy při řešení potíží.  

**Co byste měli vědět:** 

- maximální počet zařízení na uživatele Hello je zaměřená na zařízení. Například pokud *jdoe* a *jharnett* přihlášení tooa zařízení samostatných registrací (DeviceID) se vytvoří pro každý z nich v hello **uživatele** informace o kartě.  

- Hello počáteční registrace / join zařízení je nakonfigurované tooperform k pokusu o přihlášení nebo zámku a odemknout. Může dojít k 5 minut zpoždění aktivovány úloh služby Plánovač úloh. 

- Přeinstalujte hello operačního systému nebo ruční unregister a zopakujte registraci na Azure AD může vytvořit nové registrace a má za následek více položek kartě údaje uživatele hello v hello portálu Azure. 


## <a name="step-1-retrieve-hello-registration-status"></a>Krok 1: Načíst stav registrace hello 

**Stav registrace tooverify hello:**  

1. Hello otevřete příkazový řádek jako správce 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení k hello.

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-hybrid-azure-ad-join-status"></a>Krok 2: Posouzení stavu připojení k Azure AD hybridní hello 

Pokud hello hybridní Azure AD join nebyla úspěšná, dialogové okno hello poskytuje podrobnosti o hello problém, který došlo k chybě.

**Většina běžných potíží s Hello jsou:**

- Konfigurace služby AD FS nebo služby Azure AD

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Nejsou přihlášeni jako uživatel domény

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- Bylo dosaženo kvóty

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Služba Hello neodpovídá 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Informace o stavu hello můžete také najít v protokolu událostí hello v části **aplikace a služby Log\Microsoft-pracovišti**.
  
**Hello nejběžnější příčiny selhání hybridní spojení Azure AD jsou:** 

- Váš počítač není v interní síti organizace hello nebo síť VPN bez připojení tooan místní řadič domény AD.

- Jste přihlášeni tooyour počítači pomocí účtu místního počítače. 

- Problémy s konfigurací služby: 

  - Hello federační server, musí být nakonfigurované toosupport **WIAORMULTIAUTHN**. 

  - Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do.

  - Uživatel dosáhl limitu hello zařízení. 

## <a name="next-steps"></a>Další kroky

Máte dotazy, viz hello [nejčastější dotazy ke správě zařízení](device-management-faq.md)  
