---
title: "Šifrování databáze v klidovém stavu: Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak Azure Cosmos DB poskytuje výchozí šifrování všechna data."
services: cosmos-db
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: voellm
ms.openlocfilehash: d52d55fe45fdd18394166ec7ccd6e46e8f8f434b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Azure Cosmos DB databáze šifrování v klidovém stavu

Šifrování v klidovém stavu je slovní spojení, které běžně odkazuje toohello šifrování dat na zařízeních stálé úložiště, jako jsou jednotky SSD (Solid-State Drive) a pevných disků (HDD). Cosmos DB ukládá její primární databáze na jednotkách SSD. Její přílohy média a zálohy jsou uloženy v úložišti objektů Blob v Azure, které se obecně zálohuje pevné disky. S vydáním hello šifrování v klidovém stavu pro Cosmos DB jsou všechny databáze, přílohy média a zálohování šifrované. Vaše data se nyní šifrují během přenosu (přes síť hello) a v klidu (stálé úložiště), která poskytuje šifrování klient server.

Protože služba PaaS, Cosmos DB je velmi snadné toouse. Protože je v klidovém stavu a přenosu šifrovaný všechna uživatelská data, které jsou uloženy v databázi Cosmos, nemáte tootake žádnou akci. Jiný způsob tooput Toto je šifrování rest je "na" ve výchozím nastavení. Neexistují žádné ovládací prvky tooturn ho nebo vypnout. Poskytujeme tuto funkci při abychom mohli pokračovat toomeet naše [dostupnosti a výkonu SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implement-encryption-at-rest"></a>Implementace šifrování v klidovém stavu

Šifrování v klidovém stavu je implementována pomocí několika technologiemi zabezpečení, včetně systémů zabezpečeného úložiště klíčů, šifrované sítí a kryptografické rozhraní API. Systémy, které dešifrovat a zpracování dat mají toocommunicate se systémy, které správu klíčů. Hello diagram znázorňuje, jak je oddělená úložiště šifrovaných dat a hello správy klíčů. 

![Diagram návrhu](./media/database-encryption-at-rest/design-diagram.png)

základní tok požadavku uživatele Hello vypadá takto:
- databázový účet uživatele Hello je připraven a klíče úložiště jsou načteny prostřednictvím požadavku toohello poskytovatele prostředků služby správy.
- Uživatel vytvoří připojení tooCosmos DB prostřednictvím protokolu HTTPS nebo zabezpečení přenosu. (hello sady SDK abstraktní hello podrobnosti.)
- Hello uživatel odešle toobe dokumentu JSON uložené přes hello vytvořili zabezpečené připojení.
- dokument JSON Hello je indexovaný, pokud uživatel hello zakáže indexování.
- Hello dokumentu JSON a data indexu jsou zapsány toosecure úložiště.
- Data se pravidelně čtení ze zabezpečeného úložiště hello a zálohovat toohello úložišti objektů Blob Azure šifrovaný.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>Otázka: jak mnohem víc Azure Storage náklady Pokud je povolené šifrování služby úložiště?
Odpověď: není bez dalších nákladů.

### <a name="q-who-manages-hello-encryption-keys"></a>Otázka: kdo spravuje hello šifrovacích klíčů?
Odpověď: hello klíče spravuje Microsoft.

### <a name="q-how-often-are-encryption-keys-rotated"></a>Otázka: jak často jsou šifrovací klíče otáčet?
Odpověď: Microsoft obsahuje sadu interní pokyny pro šifrovací klíče otočení, který následuje po Cosmos DB. konkrétní pokyny Hello nejsou publikovány. Microsoft publikování hello [životního cyklu SDL (Security Development)](https://www.microsoft.com/sdl/default.aspx), který je považovat za podmnožinu interní pokyny a obsahuje užitečné osvědčené postupy pro vývojáře.

### <a name="q-can-i-use-my-own-encryption-keys"></a>Otázka: je možné použít vlastní šifrovací klíče?
Odpověď: cosmos DB je služba PaaS a jsme fungovala pevný tookeep hello služby snadno toouse. Zaznamenali jsme si, že toto je často dotaz pokládán jako proxy otázka pro splnění požadavků dodržování předpisů jako PCI-DSS. Jako součást sestavení tuto funkci jsme pracovali s tooensure auditory dodržování předpisů, že zákazníci, kteří používají Cosmos DB splňují požadavky na jejich bez hello nutné toomanage klíče sami.
V důsledku toho jsme aktuálně nenabízejí tooburden možnost hello uživatelé sami pomocí správy klíčů.

### <a name="q-what-regions-have-encryption-turned-on"></a>Otázka: co oblasti mají zapnutým šifrováním?
Odpověď: všechny oblasti Azure Cosmos DB mít šifrování pro všechna uživatelská data zapnuté.

### <a name="q-does-encryption-affect-hello-performance-latency-and-throughput-slas"></a>Otázka: šifrování vliv hello výkonu latence a propustnosti SLA?
Odpověď: neexistuje žádný dopad změny toohello výkon nebo SLA teď, když je povolené šifrování v klidovém stavu pro všechny stávající i nové účty. Si můžete přečíst více na hello [SLA pro Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) stránka toosee hello nejnovější záruky.

### <a name="q-does-hello-local-emulator-support-encryption-at-rest"></a>Otázka: hello místní emulátoru podporuje šifrování v klidovém stavu?
Odpověď: hello emulátoru se o samostatný nástroj pro vývoj/testování a nepoužívá hello služby správy klíčů, které hello spravované služby používá Cosmos DB. Naše doporučení je tooenable nástroj BitLocker na jednotkách, kde ukládáte citlivé emulátoru testovacích datech. Hello [emulátoru podporuje Změna výchozí adresář dat hello](local-emulator.md) a také pomocí známých umístění.

## <a name="next-steps"></a>Další kroky

Přehled zabezpečení systému Cosmos DB a hello nejnovější vylepšení, najdete v tématu [zabezpečení databáze Azure Cosmos DB](database-security.md).
Další informace o certifikace společnosti Microsoft najdete v tématu hello [Centrum zabezpečení Azure](https://azure.microsoft.com/en-us/support/trust-center/).
