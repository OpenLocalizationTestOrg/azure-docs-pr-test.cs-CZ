---
title: "Nastavení Windows 10 zařízení s Azure AD z nastavení | Microsoft Docs"
description: "Vysvětluje, jak mohou uživatelé připojit k Azure AD pomocí nabídky nastavení."
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
ms.openlocfilehash: 5a3963f16b471ad1ca8681b22a1a027935400465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Nastavení Windows 10 zařízení s Azure AD z nastavení
Pokud už používáte systém Windows 7 nebo Windows 8 a počítač nebo zařízení byl upgradován na verzi Windows 10, můžete pomocí nabídky nastavení připojení k Azure Active Directory (Azure AD).

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>V nabídce nastavení připojení k Azure AD
1. Z **spustit** nabídky, klikněte na tlačítko **nastavení** tlačítka.
2. Z **nastavení**, vyberte **systému**->**o**->**připojení k Azure AD**.
   
   <center>
   ![Připojení k Azure AD v nabídce nastavení](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>
3. Klikněte na tlačítko **pokračovat** v okně zprávy připojení ke službě Azure AD.
   
   <center>
   ![Okno zprávy připojení k Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Zadejte přihlašovací údaje. Toto přihlášení bude obsahovat všechny kroky, které jsou požadovány pro dokončení ověřování. Pokud jste součástí federovaného klienta, váš správce vám poskytne federační prostředí, které hostuje vaše organizace.
   <center>
   ![Zadejte přihlašovací údaje](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Pokud vaše organizace nakonfiguroval Azure Multi-Factor Authentication pro připojení ke službě Azure AD, zadejte druhý faktor než budete pokračovat.
6. Klikněte na tlačítko **přijmout** na **povolit toto zařízení ke správě** obrazovky.
7. Měli byste vidět zprávu "zařízení je teď připojené k vaší organizaci ve službě Azure AD".

## <a name="additional-information"></a>Další informace
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)

