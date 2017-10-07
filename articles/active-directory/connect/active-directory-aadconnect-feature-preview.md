---
title: 'Azure AD Connect: Funkce ve verzi preview | Microsoft Docs'
description: "Toto téma popisuje v další podrobnosti o funkcích, které jsou ve verzi preview v Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bcfc710861b19d8f86f094ced0d1c691e0911f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="more-details-about-features-in-preview"></a>Další informace o funkcích ve verzi preview
Toto téma popisuje, jak funkce toouse aktuálně ve verzi preview.

## <a name="group-writeback"></a>Zpětný zápis skupin
Hello možnost pro zpětný zápis skupin v volitelné funkce vám umožňuje toowriteback **skupiny Office 365** tooa doménová struktura s Exchange nainstalované. Toto je skupinu, která řídí se vždy hlavním v cloudu hello. Pokud máte místní Exchange, pak můžete napsat zpět tyto tooon místní skupiny uživatelů s poštovní schránky systému Exchange místně odesílat a přijímat e-maily z těchto skupin.

Další informace o skupiny Office 365 a jak toouse je možné najít [zde](http://aka.ms/O365g).

Skupiny služby Office 365 je reprezentován jako distribuční skupiny v místní službě AD DS. Vaše místní Exchange server musí být na serveru Exchange 2013 kumulativní aktualizaci 8 (vydané v března 2015) nebo toorecognize Exchange 2016 tento nový typ skupiny.

**Poznámky k verzi Preview hello**

* Hello adresu kniha atribut není aktuálně ve verzi preview hello naplněna. Bez tohoto atributu není zobrazená v hello GAL hello skupiny. Hello toopopulate nejjednodušší způsob, jak tento atribut je rutiny Exchange PowerShell hello toouse `update-recipient`.
* Pouze doménové struktury se schématem systému Exchange hello jsou platné cíle pro skupiny. Pokud byla zjištěna žádná Exchange, pak zpětný zápis skupin není možné tooenable.
* Aktuálně jsou podporovány pouze jednou doménovou strukturou nasazení organizaci Exchange. Pokud máte více než jednu organizaci místní Exchange, bude nutné do místní GALSync řešení pro tyto skupiny tooappear v vaší jiných doménových strukturách.
* Funkce zpětného zápisu skupiny Hello nezpracovává skupiny zabezpečení nebo distribuční skupiny.

> [!NOTE]
> Předplatné tooAzure AD Premium je vyžadován pro zpětný zápis skupin.
> 
>

## <a name="user-writeback"></a>Zpětný zápis uživatelů.
> [!IMPORTANT]
> Hello funkce preview zpětný zápis uživatelů byla odebrána v hello srpen 2015 update tooAzure AD Connect. Pokud jste ho povolili, měli byste zakázat tuto funkci.
>
>

## <a name="next-steps"></a>Další kroky
Pokračovat vaše [vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
