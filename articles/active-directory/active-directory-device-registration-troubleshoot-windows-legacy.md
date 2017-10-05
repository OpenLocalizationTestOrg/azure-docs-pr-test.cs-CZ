---
title: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro klienty nižší úrovně Windows | Microsoft Docs"
description: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro klienty nižší úrovně systému Windows."
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
ms.openlocfilehash: a7c8ef4c59c53c21258f0c61963d8f994a3946ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a>Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD pro klienty nižší úrovně systému Windows 

Toto téma se vztahuje pouze na následující klienty: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Pro Windows 10 nebo Windows Server 2016, najdete v části [řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD – Windows 10 a Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).

Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-device-registration-get-started.md).
 
Toto téma poskytuje pokyny o tom, jak vyřešit potenciální problémy při řešení potíží.  
Pro úspěšné výstupy poznamenat některé věci: 

- Registrace těchto klientů na Azure AD je na uživatele a zařízení. Jako příklad: Pokud jdoe a jharnett přihlásit s tímto zařízením, samostatných registrací (DeviceID) se vytvoří pro každou z těchto uživatelů na kartě uživatelské informace.  

- Registrace těchto klientů ihned nakonfigurovaný tak, aby opakujte přihlášení nebo uzamčení a může být zpoždění 5 minut, zda tento stav je aktivován pomocí úlohy plánovače úloh. 

- Znovu nainstalujte operační systém nebo Ruční zrušení registrace a znovu zaregistrovat může vytvořit novou registraci na Azure AD a bude mít za následek více položek na kartě informace uživatele na portálu Azure. 


## <a name="step-1-retrieve-the-registration-status"></a>Krok 1: Načíst stav registrace 

**Chcete-li ověřit stav registrace:**  

1. Otevřete příkazový řádek jako správce 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení.

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a>Krok 2: Vyhodnoťte stav registrace 

Pokud spojovací nebyla úspěšná, dialogové okno vám poskytuje podrobnosti o problému, který došlo k chybě.

**Nejběžnějším problémům, jsou:**

- Konfigurace služby AD FS nebo služby Azure AD

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Nejsou přihlášeni jako uživatel domény

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- Bylo dosaženo kvóty

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Služba nereaguje. 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Informace o stavu můžete také najít v protokolu událostí v části **aplikace a služby Log\Microsoft-pracovišti**.
  
**Nejběžnější příčiny selhání registrace jsou:** 

- Váš počítač není v interní síti organizace nebo síť VPN bez připojení k místní řadič domény AD.

- Jste přihlášeni k počítači pomocí účtu místního počítače. 

- Problémy s konfigurací služby: 

  - Federační server nakonfigurovaný pro podporu **WIAORMULTIAUTHN**. 

  - Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, pokud je počítač členem.

  - Uživatel byl dosažen maximální počet zařízení. Podrobnosti viz Začínáme s Azure Active Directory Device Registration.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md) 
