---
title: "aaaHow toorequire vícefaktorového ověřování | Microsoft Docs"
description: "Zjistěte, jak toorequire vícefaktorové ověřování (MFA) pro privilegované identity s hello rozšíření Azure Active Directory Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1e3dc4ad-3a6a-4a52-8417-3ca4f84ae05c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c08fad9dc80c09a14a4bd7497da4942fa227c3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorequire-mfa-in-azure-ad-privileged-identity-management"></a>Jak toorequire vícefaktorového ověřování v Azure AD Privileged Identity Management
Doporučujeme vyžadovat vícefaktorové ověřování (MFA) pro všechny správce. Tím se snižuje riziko hello útoku, z důvodu heslo tooa ohrožení zabezpečení.

Může vyžadovat, aby uživatelé dokončit výzvu MFA při přihlášení. Hello příspěvku na blogu [MFA pro Office 365 a Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) porovná předplatných Office a Azure, co je součástí s hello funkce obsažené v nabídce hello Microsoft Azure Multi-Factor Authentication.

Může také vyžadovat, aby uživatelé dokončit výzvu MFA při aktivují role v Azure AD PIM. Tímto způsobem, pokud uživatel hello nebyla dokončena výzvu vícefaktorového ověřování, když se přihlášení, bude výzvami toodo Ano podle PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Vyžadování vícefaktorového ověřování v Azure AD Privileged Identity Management
Když budete spravovat identity v PIM jako správce privilegovaných rolí, může se zobrazit výstrahy, které doporučujeme vícefaktorového ověřování pro privilegované účty. Klikněte na možnost zabezpečení hello výstrahu v řídicím panelu PIM hello a nové okno otevřete seznam hello účtů správce, které by vyžadovalo.  Výběrem víc rolí a pak kliknutím na hello můžete požadovat MFA **opravte** tlačítko, nebo můžete klikněte hello výpustky další tooindividual rolí a pak na hello **opravte** tlačítko.

> [!IMPORTANT]
> Nyní, pravé Azure MFA pracuje pouze s pracovní nebo školní účty, není účty Microsoft (obvykle osobní účet, který byl použit toosign v tooMicrosoft služby, jako je Skype, Xbox, Outlook.com, atd.). Z toho důvodu ostatní uživatele používající účet Microsoft nemůže být oprávněné správce, protože nemohou používat MFA tooactivate jejich rolí. Pokud tito uživatelé potřebovat toocontinue Správa úloh, pomocí účtu Microsoft, zvýšení oprávnění je toopermanent správci nyní.
> 
> 

Kromě toho můžete změnit hello požadavek vícefaktorového ověřování pro určité role kliknutím v hello část role PIM hello řídicího panelu. Potom klikněte na **nastavení** v okně hello role a potom vyberete **povolit** v rámci služby Multi-Factor authentication.

## <a name="how-azure-ad-pim-validates-mfa"></a>Jak Azure AD PIM ověří vícefaktorového ověřování
Existují dvě možnosti pro ověřování MFA, když uživatel aktivuje roli.

Nejjednodušší možnost Hello je toorely na Azure MFA pro uživatele, kteří jsou aktivací privilegované role. toodo se první zkontrolujte, zda tito uživatelé mají licenci, v případě potřeby a zaregistrovali pro Azure MFA. Další informace o tom, jak toodo je to v [Začínáme s Azure Multi-Factor Authentication v cloudu hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Je doporučeno, ale není potřeba, můžete konfigurovat Azure AD tooenforce vícefaktorového ověřování pro tyto uživatele při přihlášení. To je proto hello MFA kontroly provádějí Azure AD PIM sám sebe.

Případně pokud se uživatelé ověřují místně může mít zprostředkovatele identity je zodpovědná za vícefaktorového ověřování. Pokud jste nakonfigurovali ověřování na základě čipových karet federační služby AD toorequire před přístupem k Azure AD, například [zabezpečení cloudových prostředků s Azure Multi-Factor Authentication a AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) obsahuje pokyny pro konfiguraci služby AD FS toosend deklarace identity tooAzure AD. Když se uživatel pokusí tooactivate roli, Azure AD PIM přijme, že MFA již byl ověřen pro uživatele hello po přijetí hello odpovídající deklarace identity.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

