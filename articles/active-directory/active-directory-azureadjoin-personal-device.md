---
title: "aaaJoin v organizaci tooyour osobní zařízení | Microsoft Docs"
description: "Vysvětluje, jak uživatelé mohou registrovat své osobní Windows 10 zařízení tootheir podnikové síti a poskytuje kroky nasazení pro scénáři modelu BYOD."
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
ms.openlocfilehash: 979e2461dd9ad0438aa402a0a3223cc84c4c625c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-personal-device-tooyour-organization"></a>Připojení organizaci tooyour osobní zařízení
## <a name="toojoin-a-windows-10-device-tooyour-organization"></a>organizace tooyour zařízení toojoin Windows 10
1. Z hello **spustit** nabídce vyberte možnost **nastavení**.
2. Vyberte **účty**a potom klikněte na **účtu**.
3. Klikněte na tlačítko **přidat pracovní nebo školní účet**a poté zadejte v účtu organizace.
4. Na hello přihlašovací stránku vaší organizace, zadejte uživatelské jméno a heslo a pak klikněte na tlačítko **OK**.
5. Zobrazí se výzva pro výzvu ověřování Multi-Factor authentication. (Tento problém lze konfigurovat správcem IT).
6. Azure Active Directory (Azure AD) kontroluje, zda text hello zařízení vyžaduje registraci správy mobilních zařízení.
7. Windows hello zařízení registruje v adresáři organizace hello ve službě Azure AD a zaregistruje ve správě mobilních zařízení v případě potřeby.
8. Pokud jste spravované uživatele, trvá Windows desktop toohello prostřednictvím hello automatické přihlášení.
9. Pokud se Federovaný uživatel, bude provedena tooa přihlášení Windows obrazovky tooenter přihlašovacích údajů.

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

