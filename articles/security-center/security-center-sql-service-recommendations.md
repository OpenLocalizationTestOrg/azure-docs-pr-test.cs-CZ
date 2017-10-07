---
title: "aaaProtecting Azure SQL služby a data v Azure Security Center | Microsoft Docs"
description: "Tento dokument adresy doporučení v Azure Security Center, které vám pomůžou chránit vaše data a služba Azure SQL a zůstat souladu se zásadami zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 75d782d3c2418f9645139e4cd6ecb7765c488f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Ochrana služby Azure SQL a data v Azure Security Center
Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem hello konfigurace hello potřebné ovládací prvky.  Doporučení se týkají tooAzure typy prostředků: virtuální počítače (VM), sítě, SQL a datům a aplikacím.

Tento článek se zaměřuje na doporučení, které se vztahují tooAzure SQL služby a data. Doporučení center kolem povolení auditování pro servery Azure SQL a databáze, povolení šifrování u databází SQL a povolení šifrování vašeho účtu úložiště Azure.  Použití hello tabulce jako referenční toohelp porozumíte hello k dispozici SQL služby a data doporučení a co každé z nich dělá, pokud ji použijete.

## <a name="available-sql-service-and-data-recommendations"></a>K dispozici doporučení služby a dat SQL
| Doporučení | Popis |
| --- | --- |
| [Povolení auditování a detekce hrozeb na SQL serverech](security-center-enable-auditing-on-sql-servers.md) |Doporučuje zapnout auditování a zjišťování hrozeb pro servery Azure SQL (služba Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích). |
| [Povolení auditování a detekce hrozeb v databázích SQL](security-center-enable-auditing-on-sql-databases.md) |Doporučuje zapnout auditování a zjišťování hrozeb pro databáze Azure SQL (služba Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích). |
| [Povolit transparentní šifrování dat v databázích SQL](security-center-enable-transparent-data-encryption.md) |Doporučuje se, že povolíte šifrování pro databáze SQL (pouze služby Azure SQL). |

## <a name="see-also"></a>Viz také
toolearn Další informace o doporučení, které se vztahují tooother typy prostředků Azure, najdete v části hello následující:

* [Ochrana virtuálních počítačů v Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Ochrana aplikací v Azure Security Center](security-center-application-recommendations.md)
* [Ochrana sítě v Azure Security Center](security-center-network-recommendations.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
