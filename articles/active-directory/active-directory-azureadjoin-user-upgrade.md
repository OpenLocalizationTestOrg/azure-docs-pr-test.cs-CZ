---
title: "aaaSet se zařízením s Windows 10 s Azure AD z nastavení | Microsoft Docs"
description: "Vysvětluje, jak uživatelé se můžete zapojit do tooAzure AD pomocí nabídky nastavení hello."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d6485228338db571cc01f913c99fbba49e0bb74a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Nastavení Windows 10 zařízení s Azure AD z nastavení
Pokud jste už pomocí Windows 7 nebo Windows 8 a počítač nebo zařízení byla upgradovaná tooWindows 10, se můžete připojit pomocí nabídky nastavení hello tooAzure služby Active Directory (Azure AD).

## <a name="toojoin-tooazure-ad-from-hello-settings-menu"></a>toojoin tooAzure AD z nabídky nastavení hello
1. Z hello **spustit** nabídky, klikněte na tlačítko hello **nastavení** tlačítka.
2. Z **nastavení**, vyberte **systému**->**o**->**připojení k Azure AD**.
   
   <center>
   ![Připojení k Azure AD z nabídky nastavení hello](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>
3. Klikněte na tlačítko **pokračovat** v okně zprávy připojení ke službě Azure AD hello.
   
   <center>
   ![Okno zprávy připojení k Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Zadejte přihlašovací údaje. Toto přihlášení bude obsahovat všechny hello kroky, které jsou požadované toocomplete ověřování. Pokud jste součástí federovaného klienta, váš správce vám poskytne hello federační prostředí, které hostuje vaše organizace.
   <center>
   ![Zadejte přihlašovací údaje](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Pokud vaše organizace nakonfigurovány pro připojení tooAzure AD Azure Multi-Factor Authentication, zadejte hello druhý faktor než budete pokračovat.
6. Klikněte na tlačítko **přijmout** na hello **povolit toobe toto zařízení spravované** obrazovky.
7. Měli byste vidět hello zpráva "zařízení je teď připojený k tooyour organizace ve službě Azure AD".

## <a name="additional-information"></a>Další informace
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)

