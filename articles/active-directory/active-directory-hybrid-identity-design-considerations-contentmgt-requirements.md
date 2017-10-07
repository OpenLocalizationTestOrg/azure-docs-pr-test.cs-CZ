---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - stanovení požadavků na správu obsahu | Microsoft Docs"
description: "Poskytuje přehled o tom, jak toodetermine hello správy obsahu požadavky vaší firmy. Většinou když má uživatel svůj vlastní zařízení mu může mít také více přihlašovací údaje, které bude možné střídavých podle toohello aplikace, kterou používá. Je důležité toodifferentiate jaké obsah se vytvořil pomocí osobní přihlašovacích údajů oproti hello ty, které jsou vytvořené pomocí podnikové přihlašovací údaje. Řešení identity by měl být schopný toointeract s cloud services tooprovide toohello integrované prostředí koncového uživatele při zajištění jeho ochrany osobních údajů a zvýšit hello ochranu před úniky dat."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 607d366633c37b65ec5cf8ae5c64d73ca1cc96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Stanovení požadavků na správu obsahu pro vaše řešení hybridní identity
Principy hello správy obsahu požadavků pro vaši firmu může směrovat ovlivnit vaše rozhodnutí, na které toouse hybridní identity řešení. S text hello, jak narůstá počet více zařízení a funkce hello uživatelé toobring svá vlastní zařízení ([BYOD](http://aka.ms/byodcg)), hello společnosti musí chránit svá vlastní data, ale je také musí zachovat ochranu osobních údajů uživatelů. Většinou když má uživatel svůj vlastní zařízení mu může mít také více přihlašovací údaje, které bude možné střídavých podle toohello aplikace, kterou používá. Je důležité toodifferentiate jaké obsah se vytvořil pomocí osobní přihlašovacích údajů oproti hello ty, které jsou vytvořené pomocí podnikové přihlašovací údaje. Řešení identity by měl být schopný toointeract s cloud services tooprovide toohello integrované prostředí koncového uživatele při zajištění jeho ochrany osobních údajů a zvýšit hello ochranu před úniky dat. 

Různé technické ovládací prvky správy obsahu tooprovide pořadí bude řešení identity využít, jak je znázorněno na následujícím obrázku hello:

![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Ovládací prvky zabezpečení, které bude možné využívat váš systém správy identit**

Obecně platí požadavky na správu obsahu bude využívat váš systém správy identit v hello následující oblasti:

* Ochrana osobních údajů: identifikace hello uživatele, který vlastní prostředek a použití hello příslušných ovládacích prvků toomaintain integrity.
* Klasifikace dat: Identifikujte hello uživatele nebo skupinu a úroveň přístupu tooan objektu podle tooits klasifikace. 
* Ochrana úniku dat: ovládací prvky zabezpečení odpovědnost za ochranu tooavoid úniku dat, bude nutné toointeract s hello identity systému toovalidate hello identitu uživatele. To je důležité pro auditování záznam účel.

> [!NOTE]
> Čtení [klasifikace dat pro cloud připravenosti](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) Další informace o osvědčených postupech a pokyny pro klasifikaci dat.
> 
> 

Při plánování řešení hybridní identity Ujistěte se, že hello následující otázky jsou zodpovězeny podle požadavků organizace tooyour:

* Má vaše společnost nějaké kontrolních mechanismů pro zabezpečení v místě tooenforce data o ochraně osobních údajů?
  * Pokud ano, kontrolní mechanismy zabezpečení bude možné toointegrate s hello hybridní řešení identit se probíhající tooadopt?
* Používá vaše společnost klasifikace dat?
  * Pokud ano, je možné toointegrate aktuální řešení hello hello hybridní řešení identit se probíhající tooadopt?
* Vaše společnost aktuálně nemá řešení pro úniku dat? 
  * Pokud ano, je možné toointegrate aktuální řešení hello hello hybridní řešení identit se probíhající tooadopt?
* Potřebuje vaše společnost tooresources tooaudit přístup?
  * Pokud ano, jaké typy prostředků?
  * Pokud ano, jakou úroveň informací je potřeba?
  * Pokud ano, kde protokol auditování hello se musí nacházet? Místní nebo v cloudu hello?
* Potřebuje vaše společnost tooencrypt e-mailů, které obsahují citlivá data (čísla sociálního zabezpečení, čísla platebních karet a podobně)?
* Potřebuje vaše společnost tooencrypt všechny dokumenty nebo obsah sdílet s externími obchodními partnery?
* Potřebuje vaše společnost tooenforce podnikové zásady na určité typy e-mailů (dělat žádné Odpovědět všem, nepředávat)?

> [!NOTE]
> Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí. [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po hello dostupných možností a výhod i nevýhod jednotlivých možností.  Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.
> 
> 

## <a name="next-steps"></a>Další kroky
[Určete požadavky řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

