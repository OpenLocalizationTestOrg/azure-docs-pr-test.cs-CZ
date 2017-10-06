---
title: "aaaLearn o dvoustupňovém ověřování v Azure MFA | Microsoft Docs"
description: "Co je Azure Multi-Factor Authentication, proč použít vícefaktorové ověřování, další informace o hello vícefaktorového ověřování klienta a různé metody hello a verze, které jsou k dispozici. "
keywords: "tooMFA Úvod přehled vícefaktorového ověřování, co je mfa"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: kgremban
ms.openlocfilehash: a91b8d6941d2b6ce72a789a97c57e10e594e7ada
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a>Co je Azure Multi-Factor Authentication?
Dvoustupňové ověření je metoda ověřování, který vyžaduje více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení toouser přihlášení a transakce. Funguje tím, že jakékoliv dva nebo více hello následující metody ověření:

* Něco znáte (obvykle heslo)
* Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)
* Něco že se (biometrika)

<center>![Uživatelské jméno a heslo](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikáty](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Inteligentní Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![čipové karty](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuální čipové karty](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![uživatelské jméno a Heslo](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) je řešení dvoustupňového ověřování od Microsoftu. Azure MFA pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a>Proč používat Azure Multi-Factor Authentication?
Dnes, více než někdy uživatelé stále připojeni. Chytré telefony, tablety, přenosné počítače a počítače uživatelé mají několik možností na tom, jak se budou tooconnect zůstat připojeni a kdykoli. Lidé mají přístup k jejich účty a aplikace odkudkoli, to znamená, že můžete zvládnout větší objem práce a poskytovat svým zákazníkům lépe.

Azure Multi-Factor Authentication je snadno toouse, škálovatelnou, a spolehlivé řešení, který poskytuje, aby druhé metody ověřování, aby vaši uživatelé jsou vždycky chráněné.

| ![Snadno tooUse](./media/multi-factor-authentication/simple.png) | ![Škálovatelné](./media/multi-factor-authentication/scalable.png) | ![Vždycky chráněné](./media/multi-factor-authentication/protected.png) | ![Spolehlivost](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Snadno toouse** |**Škálovatelné** |**Vždycky chráněné** |**Spolehlivé** |

* **Snadno tooUse** -Azure Multi-Factor Authentication je jednoduchý tooset nahoru a použití. Hello zvláštní ochranu, která se dodává s Azure Multi-Factor Authentication umožňuje uživatelům toomanage svá vlastní zařízení. Nejlepší všech v mnoha případech je lze ji nastavit pomocí několika jednoduchých kliknutí.
* **Škálovatelné** -Azure Multi-Factor Authentication použije hello výkonu hello cloudových a integruje s místní AD a vlastních aplikací. Tato ochrana je i rozšířené tooyour vysoký počet, klíčové scénáře.
* **Vždycky chráněné** -Azure Multi-Factor Authentication poskytuje silné ověřování s použitím hello nejvyšší oborových standardů.
* **Spolehlivé** -Zaručujeme 99,9 % dostupnost služby Azure Multi-Factor Authentication. Hello služby se považuje za není k dispozici po nelze tooreceive nebo proces žádosti o ověření pro hello dvoustupňové ověření.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Další kroky

- Další informace o [jak funguje Azure Multi-Factor Authentication](multi-factor-authentication-how-it-works.md)

- Přečtěte si informace o různých hello [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)
