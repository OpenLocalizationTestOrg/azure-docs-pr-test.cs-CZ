---
title: "Ochrana osobních údajů v Microsoft Azure | Microsoft Docs"
description: "První článek v řadě článků vám pomohou používat Azure k ochraně osobních údajů"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: dfb046374397c8a19672ce6b67741903fff6e178
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-in-microsoft-azure"></a>Ochrana osobních údajů v Microsoft Azure

Tento článek představuje řadu články, které vám pomohou používat Azure bezpečnostních technologií a služeb na ochranu osobních údajů. Toto je klíčovým požadavkem pro mnoho společností a dodržování předpisů a zásad správného řízení iniciativy odvětví. Scénář, problém prohlášení a společnosti cíle jsou zahrnuté v tomto poli.

## <a name="scenario-and-problem-statement"></a>Scénáře a problému

Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří, Jaderského a baltský moři, jakož i Britské ostrovy. Pro podporu těchto úsilí, získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojeném království

Společnost používá Microsoft Azure k ukládání firemních dat v cloudu. To může zahrnovat zaměstnanců nebo zákazníků informace, jako:

- adresy
- telefonní čísla
- daň identifikační čísla
- lékařské informace
- informace o kreditní kartě

Společnosti musí při zpřístupnění dat pro tyto oddělení, které je třeba jej chránit ochrana osobních údajů zaměstnanců a zákazníků. (například oddělení mzdy a rezervace)

## <a name="company-goals"></a>Cíle společnosti 

- Zdroje dat, které obsahují osobní data se šifrují, pokud umístěný do cloudového úložiště.

- Během přenosu šifrovány osobní data, která se přenáší z jednoho umístění do druhého. Toto je hodnota true, pokud se data mezi podnikovém datovém centru a cloudu Azure cestě přes virtuální síť nebo přes Internet.

- Důvěrnost a integrita osobních údajů je chráněný před neoprávněným přístupem správu silné identit a technologie pro řízení přístupu.

- Osobní data je chráněný před přes porušení zabezpečení dat prostřednictvím monitorování ohrožení zabezpečení a hrozeb.

- Stav zabezpečení služby Azure, které uložení nebo na přenos osobních dat bude posouzeno identifikovat příležitosti k lepší ochraně osobních údajů.

## <a name="data-protection-guidance"></a>Pokyny pro ochranu dat

V následujících článcích obsahovat technické postupy pokyny, které vám pomohou dosáhnout cíle ochrany osobních údajů uvedených výše:

- [Azure šifrovací technologie](protect-personal-data-at-rest.md)

- [Azure šifrovací technologie](protect-personal-data-in-transit-encryption.md)

- [Azure technologie identit a přístupu](protect-personal-data-identity-access-controls.md)

- [Technologie zabezpečení sítě Azure](protect-personal-data-network-security.md)

- [Azure Security Center](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a>Další kroky

- [Server s informacemi o zabezpečení Azure](https://aka.ms/AzureSecInfo)

- [Centrum zabezpečení Microsoft](https://www.microsoft.com/TrustCenter/default.aspx)

- [Azure Security Center](https://azure.microsoft.com/services/security-center/)

- [Blog týmu zabezpečení Azure](https://www.azuresecurityorg)

- [Azure.com Blog - zabezpečení](https://azure.microsoft.com/blog/topics/security/)
