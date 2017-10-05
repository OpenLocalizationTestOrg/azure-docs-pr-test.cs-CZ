---
title: "Připojení osobního zařízení pro vaši organizaci | Microsoft Docs"
description: "Vysvětluje, jak uživatelé mohou registrovat svá osobní zařízení Windows 10 do své podnikové síti a poskytuje kroky nasazení pro scénáři modelu BYOD."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 9f3d38f5-1cfd-43d4-97da-4fed1255a1ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 9418365ea18b065551448742b21c8b17a1749fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-personal-device-to-your-organization"></a>Připojení osobního zařízení pro vaši organizaci
## <a name="to-join-a-windows-10-device-to-your-organization"></a>K připojení k zařízením s Windows 10 pro vaši organizaci
1. Z **spustit** nabídce vyberte možnost **nastavení**.
2. Vyberte **účty**a potom klikněte na **účtu**.
3. Klikněte na tlačítko **přidat pracovní nebo školní účet**a poté zadejte v účtu organizace.
4. Na přihlašovací stránce pro vaši organizaci, zadejte uživatelské jméno a heslo a pak klikněte na tlačítko **OK**.
5. Zobrazí se výzva pro výzvu ověřování Multi-Factor authentication. (Tento problém lze konfigurovat správcem IT).
6. Azure Active Directory (Azure AD) zkontroluje, jestli zařízení vyžaduje registraci správy mobilních zařízení.
7. Systém Windows se zařízení registruje do adresáře organizace v Azure AD a zaregistruje ve správě mobilních zařízení v případě potřeby.
8. Pokud jste spravované uživatel, Windows přejdete na ploše prostřednictvím automatického přihlášení.
9. Pokud se Federovaný uživatel, můžete k zadání pověření přesměrováni na obrazovce přihlášení systému Windows.

## <a name="additional-information"></a>Další informace
* [Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

