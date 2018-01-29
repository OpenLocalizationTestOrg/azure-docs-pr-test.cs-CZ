---
title: "Další informace o dvoustupňovém ověřování v Azure MFA | Microsoft Docs"
description: "Co je Azure Multi-Factor Authentication, proč použít vícefaktorové ověřování, další informace o vícefaktorového ověřování klienta a různé metody a verze, které jsou k dispozici. "
keywords: "Úvod do MFA, mfa přehled, co je mfa"
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: joflore
ms.openlocfilehash: 63b717fb08861b0efe902400a8765f9515ed5b1a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a>Co je Azure Multi-Factor Authentication?
Dvoustupňové ověření je metoda ověřování, který vyžaduje více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení uživatelská přihlášení a transakce. Funguje tím, že jakékoliv dva nebo více z následujících metod ověřování:

* Něco znáte (obvykle heslo)
* Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)
* Něco že se (biometrika)

<center>![Uživatelské jméno a heslo](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikáty](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Inteligentní Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![čipové karty](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuální čipové karty](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![uživatelské jméno a Heslo](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) je řešení dvoustupňového ověřování od Microsoftu. Azure MFA pomáhá chránit přístup k datům a aplikacím a současně plní požadavky uživatelů na jednoduchý proces přihlašování. Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.

## <a name="why-use-azure-multi-factor-authentication"></a>Proč používat Azure Multi-Factor Authentication?
Dnes, více než někdy uživatelé stále připojeni. Uživatelé s chytré telefony, tablety, přenosné počítače a počítače, mají několik možností pro přístup k jejich účty a aplikace odkudkoli a kdykoli zůstat připojeni.

Azure Multi-Factor Authentication je snadno použitelný, škálovatelného a spolehlivého řešení poskytující druhé metody ověřování chránit vaši uživatelé.

| ![Snadné použití](./media/multi-factor-authentication/simple.png) | ![Škálovatelné](./media/multi-factor-authentication/scalable.png) | ![Vždycky chráněné](./media/multi-factor-authentication/protected.png) | ![Spolehlivost](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Jednoduchost použití** |**Škálovatelné** |**Vždycky chráněné** |**Spolehlivé** |

* **Snadno použitelný** -Azure Multi-Factor Authentication se snadno nastavit a používat. Zvláštní ochranu, která se dodává s Azure Multi-Factor Authentication umožňuje uživatelům spravovat svá zařízení. Nejlepší všech v mnoha případech je lze ji nastavit pomocí několika jednoduchých kliknutí.
* **Škálovatelné** -Azure Multi-Factor Authentication využívá kapacitu cloudu a integruje s místní AD a vlastních aplikací. Tato ochrana je i rozšířit na vaše vysoký počet, klíčové scénáře.
* **Vždycky chráněné** -Azure Multi-Factor Authentication poskytuje silné ověřování s použitím nejvyšší oborových standardů.
* **Spolehlivé** -Microsoft je zárukou 99,9 % dostupnost služby Azure Multi-Factor Authentication. Služba se považuje za není k dispozici, pokud nemůže přijímat nebo zpracovat žádosti o ověření pro dvoustupňové ověření.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Další kroky

- Další informace o [jak funguje Azure Multi-Factor Authentication](multi-factor-authentication-how-it-works.md)

- Přečtěte si informace o různých [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)
