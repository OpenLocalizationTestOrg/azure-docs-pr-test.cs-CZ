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
ms.openlocfilehash: 193dbede0655376714c6f8f12c826ba3f9bc8933
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Azure Cosmos DB databáze šifrování v klidovém stavu

Šifrování v klidovém stavu je slovní spojení, které běžně odkazuje na šifrování dat na zařízeních stálé úložiště, jako jsou jednotky SSD (Solid-State Drive) a pevných disků (HDD). Cosmos DB ukládá její primární databáze na jednotkách SSD. Její přílohy média a zálohy jsou uloženy v úložišti objektů Blob v Azure, které se obecně zálohuje pevné disky. S vydáním šifrování v klidovém stavu pro Cosmos DB budou zašifrovány všechny databáze, přílohy média a zálohování. Vaše data se nyní šifrují během přenosu (přes síť) a v klidu (stálé úložiště), která poskytuje šifrování klient server.

Protože služba PaaS, Cosmos DB je velmi snadno použitelný. Protože je v klidovém stavu a přenosu šifrovaný všechna uživatelská data, které jsou uloženy v databázi Cosmos, nemusíte provádět žádnou akci. Jiný způsob, jak to uvedli je, že je šifrování v klidovém stavu "na" ve výchozím nastavení. Neexistují žádná opatření, můžete zapnout nebo vypnout. Poskytujeme tuto funkci při jsme i dál splňují naše [dostupnosti a výkonu SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implementation-of-encryption-at-rest-for-azure-cosmos-db"></a>Implementace šifrování v klidovém stavu pro Azure Cosmos DB

Šifrování v klidovém stavu je implementována pomocí několika technologiemi zabezpečení, včetně systémů zabezpečeného úložiště klíčů, šifrované sítí a kryptografické rozhraní API. Systémy, které dešifrovat a zpracování dat mají komunikovat se systémy, které správu klíčů. Diagram znázorňuje, jak je oddělená úložiště šifrovaných dat a správu klíčů. 

![Diagram návrhu](./media/database-encryption-at-rest/design-diagram.png)

Základní tok požadavku uživatele je následující:
- Databázový účet uživatele je připraven a klíče k úložišti jsou načteny prostřednictvím žádost, aby zprostředkovatel prostředků služby správy.
- Uživatel vytvoří připojení k databázi Cosmos prostřednictvím protokolu HTTPS nebo zabezpečení přenosu. (Sady SDK abstraktní podrobnosti.)
- Uživatel odešle dokument JSON ukládaly přes dříve vytvořenou zabezpečené připojení.
- Dokument JSON má index, pokud je uživatel vypnul indexování.
- JSON dokumentů a index data se zapisují do zabezpečené úložiště.
- Data se pravidelně čtení ze zabezpečeného úložiště a zálohovat do úložiště objektů Blob Azure šifrovaný.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>Otázka: jak mnohem víc Azure Storage náklady Pokud je povolené šifrování služby úložiště?
Odpověď: není bez dalších nákladů.

### <a name="q-who-manages-the-encryption-keys"></a>Otázka: kdo spravuje šifrovacích klíčů?
Odpověď: klíče jsou spravované microsoftem.

### <a name="q-how-often-are-encryption-keys-rotated"></a>Otázka: jak často jsou šifrovací klíče otáčet?
Odpověď: Microsoft obsahuje sadu interní pokyny pro šifrovací klíče otočení, který následuje po Cosmos DB. Konkrétní pokyny nejsou publikovány. Microsoft publikovat [životního cyklu SDL (Security Development)](https://www.microsoft.com/sdl/default.aspx), který je považovat za podmnožinu interní pokyny a obsahuje užitečné osvědčené postupy pro vývojáře.

### <a name="q-can-i-use-my-own-encryption-keys"></a>Otázka: je možné použít vlastní šifrovací klíče?
Odpověď: cosmos DB je služba PaaS a jsme fungovala pevného zachovat nejjednodušší službu. Zaznamenali jsme si, že toto je často dotaz pokládán jako proxy otázka pro splnění požadavků dodržování předpisů jako PCI-DSS. Jako součást sestavení tuto funkci jsme pracovali s auditory dodržování předpisů zajistit, že zákazníci, kteří používají Cosmos DB splňují požadavky na jejich bez nutnosti Správa klíčů sami.
V důsledku toho jsme aktuálně nenabízejí uživatelům možnost nebyli sami pomocí správy klíčů.

### <a name="q-what-regions-have-encryption-turned-on"></a>Otázka: co oblasti mají zapnutým šifrováním?
Odpověď: všechny oblasti Azure Cosmos DB mít šifrování pro všechna uživatelská data zapnuté.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>Otázka: šifrování ovlivnit výkon latence a propustnosti SLA?
Odpověď: není žádný dopad nebo změny na výkon SLA teď, když je povolené šifrování v klidovém stavu pro všechny stávající i nové účty. Další informace na [SLA pro Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) stránky zobrazíte nejnovější záruky.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>Otázka: místní emulátoru podporuje šifrování v klidovém stavu?
Odpověď: emulátoru se o samostatný nástroj pro vývoj/testování a nepoužívá služby správy klíčů, které používá spravované služby Cosmos DB. Naše doporučení je povolit nástroj BitLocker na jednotkách, kde ukládáte citlivé emulátoru testovacích datech. [Emulátoru podporuje Změna výchozí adresář dat](local-emulator.md) a také pomocí známých umístění.

## <a name="next-steps"></a>Další postup

Přehled zabezpečení systému Cosmos DB a nejnovější vylepšení najdete v tématu [zabezpečení databáze Azure Cosmos DB](database-security.md).
Další informace o certifikace společnosti Microsoft najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/en-us/support/trust-center/).
