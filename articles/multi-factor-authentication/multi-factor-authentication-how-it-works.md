---
title: "aaaAzure Multi-Factor Authentication – jak to funguje"
description: "Azure Multi-Factor Authentication pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 82f234fb86f145c42e8e56b8bdd2d61720c9ff2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>Jak funguje Azure Multi-Factor Authentication
Hello zabezpečení dvoustupňové ověření spočívá v jeho vrstveného přístupu. Porušení zabezpečení několika faktory ověřování uvede významné výzvu pro útočníky. I v případě, že útočník dokázal toolearn hello uživatelské heslo, je zbytečné bez nutnosti hello důvěryhodné zařízení u sebe. 

![Ověření](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure Multi-Factor Authentication pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení.  Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření.


## <a name="methods-available-for-two-step-verification"></a>Dostupné metody pro dvoustupňové ověření
Když se uživatel přihlásí, je odeslána další ověření toohello uživatele.  Hello následují seznam metod, které lze použít pro tento druhý ověření.

| Metoda ověření | Popis |
| --- | --- |
| Telefonní hovor |Volání je umístěna do registrované telefon tooa uživatele. Hello uživatel zadá kód PIN, v případě potřeby pak stiskne klávesu # hello. |
| Textová zpráva |Mobilní telefon tooa uživatele s šestimístný kód je odeslána textová zpráva. Hello uživatel zadá tento kód na přihlašovací stránku hello. |
| Oznámení mobilní aplikace |Smartphone tooa uživatele je odeslána žádost o ověření. Hello uživatel zadá kód PIN, v případě potřeby pak vybere **ověřte** na mobilní aplikace hello. |
| Kód ověření mobilní aplikace |Hello mobilní aplikaci, která běží na uživatele Smartphone, zobrazí ověřovací kód, který změní každých 30 sekund. uživatel Hello nejnovější kód hello vyhledá a přejde na přihlašovací stránku hello. |
| Tokeny OATH třetích stran | Azure Multi-Factor Authentication Server může být nakonfigurované tooaccept metody ověření dat třetí stranou. |

Azure Multi-Factor Authentication poskytuje metody volitelný ověření pro cloud a serveru. Můžete zvolit, které metody jsou k dispozici pro vaše uživatele: telefonní hovor, text, oznámení aplikaci nebo aplikaci kódy. Další informace najdete v tématu [volitelný ověření metody](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Další kroky

- Přečtěte si informace o různých hello [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

- Vyberte, zda toodeploy Azure MFA [v hello cloudu nebo místně](multi-factor-authentication-get-started.md)

- Přečtěte si odpovědi pro [nejčastější dotazy](multi-factor-authentication-faq.md)