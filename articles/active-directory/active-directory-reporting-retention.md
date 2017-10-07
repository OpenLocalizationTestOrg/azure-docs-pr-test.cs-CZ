---
title: "Zásady uchování sestav služby Active Directory aaaAzure | Microsoft Docs"
description: "Zásady uchovávání informací na data sestavy ve vašem Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c46a68198cb34e9c92662b2f8461010745392c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Zásady uchování sestav Azure Active Directory


Toto téma vám poskytne odpovědi toohello nejčastější dotazy ve spojení s hello uchovávání dat pro sestavy hello jiné aktivitě v Azure Active Directory. 

**Otázka: jak můžete získat hello shromažďování dat aktivity spustit?**

**ODPOVĚĎ:**

| Edice Azure AD | Počáteční kolekce |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Při registraci pro předplatné |
| Azure AD Free | Při prvním spuštění hello Hello [okno Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) nebo použijte hello [reporting rozhraní API](https://aka.ms/aadreports)  |

---
**Otázka: Pokud je vaše data aktivity k dispozici v hello portál Azure?**

**ODPOVĚĎ:**

- **Okamžitě** – Pokud již pracujete se sestavami v hello portál Azure classic
- **V rámci 2 hodiny** – Pokud jste nezapnuli vytváření sestav v hello portál Azure classic

---
**Otázka: jak můžete získat hello kolekce signálů zabezpečení spustit?**  

**Odpověď:** signálů zabezpečení, se proces shromažďování hello spustí, když jste souhlas toouse hello Identity Protection Center. 


---
**Otázka: jak dlouho hello shromážděná data ukládají?**

**ODPOVĚĎ:**

**Sestavy aktivit**    

| Sestava                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Audit adresáře        | 7 dní        | 30 dní             | 30 dní             |
| Přihlašovací aktivita       | 7 dní        | 30 dní             | 30 dní             |

**Signály zabezpečení**

| Sestava         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Ohrožení uživatelé  | 7 dní        | 30 dní             | 90 dnů             |
| Rizikové přihlášení | 7 dní        | 30 dní             | 90 dnů             |

---