---
title: aaaAzure latence Reporting Active Directory | Microsoft Docs
description: "Dobu potřebnou pro vytváření sestav události tooshow nahoru v Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 346b14f8-d16d-4b07-8211-e6c5eec07062
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 14367d21dfb28359f991037cc924d416420be456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-latencies"></a>Latence sestav Azure Active Directory
*Tato dokumentace je součástí hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).*

| Sestava | Minimální | Průměr | Maximální počet |
| --- | --- | --- | --- |
| **O zabezpečení** | | | |
| Nestandardní přihlašovací aktivita |2 hodiny |4 hodiny |8 hodin |
| Přihlášení z neznámých zdrojů |2 hodiny |4 hodiny |8 hodin |
| Přihlášení po několika neúspěších |2 hodiny |4 hodiny |8 hodin |
| Přihlášení z více geografických poloh |2 hodiny |4 hodiny |8 hodin |
| Přihlášení z IP adres s podezřelou aktivitou |2 hodiny |4 hodiny |8 hodin |
| Přihlášení z možných nakažených zařízení |2 hodiny |4 hodiny |8 hodin |
| Uživatelé s neobvyklou přihlašovací aktivitou |2 hodiny |4 hodiny |8 hodin |
| Uživatelé s uniklými přihlašovacími údaji |2 hodiny |4 hodiny |8 hodin |
| Všechny uživatelské přihlašovací aktivita |2 hodiny |4 hodiny |8 hodin |
| **Sestavy aplikací** | | | |
| Účet zřizování aktivity |2 hodiny |4 hodiny |8 hodin |
| Chyby zřizování účtů |2 hodiny |4 hodiny |8 hodin |
| Využití aplikací |2 hodiny |4 hodiny |8 hodin |
| Stav Změna hesla |2 hodiny |4 hodiny |8 hodin |
| **Audit a protokolování aktivit** | | | |
| Sestava auditu |1 minuta |15 minut |30 minut |
| Aktivita (Azure AD) resetování hesla |2 hodiny |4 hodiny |8 hodin |
| Aktivita (Identity Manager) resetování hesla |2 hodiny |4 hodiny |8 hodin |
| Činnost registrace (Azure AD) pro resetování hesla |2 hodiny |4 hodiny |8 hodin |
| Resetování hesla činnost registrace (Identity Manager) |2 hodiny |4 hodiny |8 hodin |
| Samoobslužné aktivita skupin (Azure AD) |2 hodiny |4 hodiny |8 hodin |
| Samoobslužné aktivita skupin (Identity Manager) |2 hodiny |4 hodiny |8 hodin |
| **Sestavy služby RMS** | | | |
| Většina aktivní uživatele RMS |2 hodiny |4 hodiny |8 hodin |
| Použití služby RMS |2 hodiny |4 hodiny |8 hodin |
| Použití zařízení RMS |2 hodiny |4 hodiny |8 hodin |
| Využívání aplikací RMS povoleno |2 hodiny |4 hodiny |8 hodin |

