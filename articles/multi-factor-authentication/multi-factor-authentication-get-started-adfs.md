---
title: "Krok aaaTwo ověřování a služby AD FS - Azure MFA | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak tooget pracovat s Azure MFA a AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 7c1c925039d3cb753ba60e286168e5869faeae4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Začínáme s ověřováním Azure Multi-Factor Authentication a službami Active Directory Federation Services
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Pokud má vaše organizace federované místní služby Active Directory s Azure Active Directory využívající službu AD FS, jsou k dispozici následující 2 možnosti používání ověřování Azure Multi-Factor Authentication.

* Zabezpečení cloudových prostředků pomocí ověřování Azure Multi-Factor Authentication nebo služeb Active Directory Federation Services
* Zabezpečení cloudu a místních prostředků pomocí ověřování Azure Multi-Factor Authentication Server

Hello následující tabulka shrnuje zkušeností ověřování hello mezi zabezpečením prostředků pomocí ověřování Azure Multi-Factor Authentication a AD FS

| Zkušenosti s ověřováním – aplikace založené na prohlížeči | Zkušenosti s ověřováním – aplikace nezaložené na prohlížeči |
|:--- |:--- |:--- |
| Zabezpečení prostředků Azure AD pomocí Azure Multi-Factor Authentication |<li>prvním krokem ověření Hello se provádí místně pomocí služby AD FS.</li> <li>druhý krok Hello je metoda založené na telefonu prováděná pomocí cloudového ověřování.</li> |
| Zabezpečení prostředků Azure AD pomocí Active Directory Federation Services |<li>prvním krokem ověření Hello se provádí místně pomocí služby AD FS.</li><li>druhým krokem Hello je prováděn v místě dodržením deklarace identity hello.</li> |

Upozornění s hesly aplikací pro federované uživatele:

* Hesla aplikace se ověřují pomocí cloudového ověřování, takže obcházejí federaci. Federace se aktivně používá jenom při nastavování hesla aplikace.
* Místní nastavení služby Access Control klienta nejsou dodržená hesly aplikace.
* U hesel aplikací ztratíte schopnost ověřování přihlašování v místě.
* Zakázání/odstranění účtu může trvat hodiny toothree pro synchronizaci adresáře, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace v cloudové identitě hello.

## <a name="next-steps"></a>Další kroky
Informace o nastavení ověřování Azure Multi-Factor Authentication nebo hello Azure Multi-Factor Authentication Server se službou AD FS najdete v tématu hello následující články:

* [Zabezpečené cloudové prostředky používající ověřování Azure Multi-Factor Authentication a AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
* [Zabezpečený cloud a místní prostředky používající ověřování Azure Multi-Factor Authentication Server s Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
* [Zabezpečené cloudové a lokální prostředky používající Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
