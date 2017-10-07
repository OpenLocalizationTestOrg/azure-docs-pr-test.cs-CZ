---
title: "latence sestav služby Active Directory aaaAzure | Microsoft Docs"
description: "Další informace o hello dobu potřebnou pro vytváření sestav tooshow událostí až na portálu Azure"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: eee959331262ba59b313dd038cb54699dbef48a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Latence sestav Azure Active Directory.

S [reporting](active-directory-preview-explainer.md) v hello Azure Active Directory, získáte všechny hello informace, které potřebujete toodetermine úspěšnost prostředí. Hello dobu potřebnou pro vytváření sestav, data tooshow nahoru v hello portál Azure je také označován jako latence. 

Toto téma obsahuje informace o hello latenci hello všech sestav kategorií v hello portálu Azure. 


## <a name="activity-reports"></a>Sestavy aktivit

Existují dvě oblasti aktivity generování sestav:

- **Přihlašovací aktivity** – informace o využití hello spravovaných aplikací a aktivit přihlášení uživatelů
- **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů

Hello následující tabulka uvádí informace o protokolování aktivit hello latenci.

| Sestava | Minimální | Průměr | Maximální počet |
| :-- | --- | --- | --- |
| Protokoly auditu             | 30 minut  | 45 minut | 1 hodina     |
| Přihlášení               | 15 minut  | 15 minut | 2 hodiny *   |

>[!NOTE]
> Pro některá data aktivity přihlášení pocházejících z aplikace office starší verze může trvat too8 hodin hello generování sestav dat tooshow nahoru. 


## <a name="security-reports"></a>Sestavy zabezpečení

Existují dvě oblasti generování sestav zabezpečení:

- **Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu. 
- **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. 

Hello následující tabulka uvádí informace o zabezpečení sestavy hello latenci.

| Sestava | Minimální | Průměr | Maximální počet |
| :-- | --- | --- | --- |
| Ohrožení uživatelé          | 5 minut   | 15 minut  | 2 hodiny  |
| Rizikové přihlášení         | 5 minut   | 15 minut  | 2 hodiny  |

## <a name="risk-events"></a>Riziko události

Azure Active Directory používá adaptivní strojového učení algoritmů a heuristiky podezřelé akce toodetect, které jsou související tooyour uživatelské účty. Všechny zjištěné podezřelé akce je uložený v události zavolat riziko záznamu.

Hello následující tabulka uvádí informace o rizikových událostech hello latenci.

| Sestava | Minimální | Průměr | Maximální počet |
| :-- | --- | --- | --- |
| Přihlášení z anonymních IP adres |5 minut |15 minut |2 hodiny |
| Přihlášení z neznámých míst |5 minut |15 minut |2 hodiny |
| Uživatelé s uniklými přihlašovacími údaji |2 hodiny |4 hodiny |8 hodin |
| Neuskutečnitelná cesta tooatypical umístění |5 minut |1 hodina |8 hodin  |
| Přihlášení z nakažených zařízení |2 hodiny |4 hodiny |8 hodin  |
| Přihlášení z IP adres s podezřelou aktivitou |2 hodiny |4 hodiny |8 hodin  |



## <a name="next-steps"></a>Další kroky

Pokud chcete tooknow Další informace o sestavách hello aktivity v hello portálu Azure, najdete v části:

- [Přihlašovací aktivity sestav na portálu Azure Active Directory hello](active-directory-reporting-activity-sign-ins.md)
- [Audit aktivity sestav na portálu Azure Active Directory hello](active-directory-reporting-activity-audit-logs.md)

Pokud chcete tooknow Další informace o sestavách hello zabezpečení v hello portálu Azure, najdete v části:

- [Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory hello](active-directory-reporting-security-user-at-risk.md)
- [Sestava rizikové přihlášení na portálu Azure Active Directory hello](active-directory-reporting-security-risky-sign-ins.md)

Pokud chcete více informací o rizikových událostech tooknow, najdete v části [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).
