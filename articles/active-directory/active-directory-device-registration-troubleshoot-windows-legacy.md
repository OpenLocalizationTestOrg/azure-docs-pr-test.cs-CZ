---
title: "aaaTroubleshooting hello automatickou registraci Azure AD domény k počítačům připojeným k pro klienty nižší úrovně Windows | Microsoft Docs"
description: "Řešení potíží s hello automatickou registraci Azure AD domény k počítačům připojeným k pro klienty nižší úrovně systému Windows."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 84fe666576f13de09d1eaa5692517d45a4dbeebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad-for-windows-down-level-clients"></a>Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD pro klienty nižší úrovně systému Windows 

Toto téma je použít jenom toohello následující klienty: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Pro Windows 10 nebo Windows Server 2016, najdete v části [řešení potíží s automatickou registraci domény připojené počítače tooAzure AD – Windows 10 a Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).

Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-device-registration-get-started.md).
 
Toto téma poskytuje pokyny o tom, jak tooresolve potenciální problémy při řešení potíží.  
Pro úspěšné výstupy některé toonote věcí: 

- Registrace těchto klientů na Azure AD je na uživatele a zařízení. Jako příklad: Pokud jdoe a jharnett přihlásit toothis zařízení, samostatné registrace (DeviceID) se vytvoří pro každou z těchto uživatelů hello uživatelské informace o kartě.  

- Registrace těchto klientů předinstalované hello je nakonfigurované tootry při přihlášení a uzamčení a může být zpoždění 5 minut, zda tento stav je aktivován pomocí úlohy plánovače úloh. 

- Znovu nainstalujte hello operačního systému nebo Ruční zrušení registrace a znovu zaregistrovat může vytvořit novou registraci na Azure AD a bude mít za následek více položek kartě údaje uživatele hello v hello portálu Azure. 


## <a name="step-1-retrieve-hello-registration-status"></a>Krok 1: Načíst stav registrace hello 

**Stav registrace tooverify hello:**  

1. Hello otevřete příkazový řádek jako správce 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení k hello.

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-registration-status"></a>Krok 2: Vyhodnoťte stav registrace hello 

Pokud hello spojení nebyla úspěšná, dialogové okno hello poskytuje podrobnosti o hello problém, který došlo k chybě.

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
  
**Hello nejběžnější příčiny selhání registrace jsou:** 

- Váš počítač není v interní síti organizace hello nebo síť VPN bez připojení tooan místní řadič domény AD.

- Jste přihlášeni tooyour počítači pomocí účtu místního počítače. 

- Problémy s konfigurací služby: 

  - Hello federační server, musí být nakonfigurované toosupport **WIAORMULTIAUTHN**. 

  - Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do.

  - Uživatel dosáhl limitu hello zařízení. Podrobnosti viz Začínáme s Azure Active Directory Device Registration.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu hello [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md) 
