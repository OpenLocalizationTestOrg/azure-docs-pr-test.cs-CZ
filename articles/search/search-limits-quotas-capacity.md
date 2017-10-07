---
title: "omezení aaaService ve službě Azure Search | Microsoft Docs"
description: "Omezení služby používá pro plánování kapacity a maximální limit na požadavky a odpovědi pro službu Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/07/2017
ms.author: heidist
ms.openlocfilehash: cb13a0f1c87a654fb5845c9c741f74a91da5b372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-limits-in-azure-search"></a>Omezení služby ve službě Azure Search
Maximální omezuje na úložiště, úlohy a počty indexů, dokumenty a další objekty závisí na tom, zda jste [zřízení Azure Search](search-create-service-portal.md) na **volné**, **základní**, nebo **Standardní** cenová úroveň.

* **Volné** je víceklientské sdílené služby, která se dodává s předplatným Azure. Jedná se o možnost bez dalších nákladů pro existující odběratele, který vám umožní tooexperiment službou hello před registrací vyhrazených prostředcích.
* **Základní** poskytuje vyhrazený výpočetní prostředky pro úlohy v produkčním prostředí v menším měřítku.
* **Standardní** běží na vyhrazené počítače s další kapacitou úložiště a zpracování na všech úrovních. Standard je rozdělena na čtyři úrovně: S1, S2, S3 a S3 (S3 HD) s vysokou hustotou.

> [!NOTE]
> Služba se zřídí v konkrétní úroveň. Pokud potřebujete toojump vrstev tooget víc kapacity, je třeba zřídit (neexistuje žádné místní upgrade) nové služby. Další informace najdete v tématu [zvolte SKU nebo vrstvě](search-sku-tier.md). Další informace o úpravě kapacity v rámci služby toolearn jste jste už zřízené, najdete v části [škálovat prostředek úrovně pro dotaz a indexování úlohy](search-capacity-planning.md).
>

## <a name="per-subscription-limits"></a>Za limity předplatného
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>Omezení jednotlivých služeb
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>Za index omezení
Existuje souvislosti mezi limity pro indexy a omezení na indexery. Zadaný limit 200 indexy, hello maximální limit pro indexery je také 200 pro hello stejnou službu.

| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Index: maximální počet polí na index |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Index: maximální vyhodnocování profily pro jednotlivé indexu |100 |100 |100 |100 |100 |100 |
| Index: maximální funkce jeden profil |8 |8 |8 |8 |8 |8 |
| Indexery: maximální indexování zatížení na vyvolání |10 000 dokumentů |Omezeno pouze maximální dokumenty |Omezeno pouze maximální dokumenty |Omezeno pouze maximální dokumenty |Omezeno pouze maximální dokumenty |NENÍ K DISPOZICI <sup>2</sup> |
| Indexery: maximální dobu běhu | 1-3 minuty, <sup>3</sup> |24 hodin |24 hodin |24 hodin |24 hodin |NENÍ K DISPOZICI <sup>2</sup> |
| Indexer objektů blob: velikost maximální objektu blob, MB |16 |16 |128 |256 |256 |NENÍ K DISPOZICI <sup>2</sup> |
| Indexer objektů blob: maximální počet znaků z objektu blob extrahovat obsahu |32,000 |64,000 |4 miliony |4 miliony |4 miliony |NENÍ K DISPOZICI <sup>2</sup> |

<sup>1</sup> úroveň basic je hello pouze SKU s nižší limit 100 polí na index.

<sup>2</sup> S3 HD aktuálně nepodporuje indexery. Pokud máte naléhavá potřeba pro tuto funkci, kontaktujte podporu Azure.

<sup>3</sup> maximální dobu spuštění indexeru pro úroveň Free hello je 3 minut zdroje blob a pro všechny ostatní zdroje dat 1 minuta.

## <a name="document-size-limits"></a>Omezení velikosti dokumentu
| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Velikosti jednotlivých dokumentů na Index rozhraní API |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |

Při volání rozhraní API Index, odkazuje toohello dokumentu maximální velikost. Velikost dokument je ve skutečnosti limit velikosti hello hello textu žádosti Index rozhraní API. Vzhledem k tomu, že dávky více dokumentů toohello Index rozhraní API můžete předat najednou, omezení velikosti hello ve skutečnosti závisí na tom, kolik dokumenty jsou ve službě hello batch. Hello dokumentu maximální velikost dávky s jedním dokumentem, je 16 MB JSON.

velikost dokumentu tookeep dolů, mějte na paměti tooexclude-dotazovatelné data z požadavku hello. Obrázky a další binární data nejsou přímo dotazovatelný a by neměly být uloženy v indexu hello. toointegrate-dotazovatelné data do výsledky hledání, definujte není prohledávatelné pole, které ukládá toohello prostředku adresy URL odkazu.

## <a name="workload-limits-queries-per-second"></a>Omezení zatížení (dotazy za sekundu)
| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| QPS |Není k dispozici |Přibližně 3 na repliku |Přibližně 15 na repliku |Přibližně 60 na repliku |Více než 60 na repliku |Více než 60 na repliku |

Dotazy na za sekundu (QPS) je přibližný podle heuristiky, pomocí hodnoty tooderive odhadované úlohy simulované a skutečnou zákazníků. Určit přesnou propustnost QPS se liší v závislosti na povaze vaše data a hello hello dotazu.

I když jsou výše uvedeného hrubý odhady, skutečná rychlost je obtížné toodetermine, zejména v hello volné sdílené služby, kde je propustnost podle dostupnou šířku pásma a soutěž o systémové prostředky. V úrovni Free hello výpočetní a úložnou kapacitu jsou sdíleny více odběrateli, tak QPS pro vaše řešení bude vždy lišit v závislosti na tom, kolik dalších zatížení jsou spuštěné v hello stejnou dobu.

Na standardní úrovni hello můžete odhadnout QPS další úzce vzhledem k tomu, že budete mít kontrolu nad více parametrů hello. Části hello osvědčených postupů v [spravovat řešení vyhledávání](search-manage.md) návod toocalculate QPS pro zatížení.

## <a name="api-request-limits"></a>Omezení žádostí o rozhraní API
* Maximálně 16 MB každý požadavek <sup>1</sup>
* Maximální délka adresy URL 8 KB
* Maximální 1000 dokumentů na jednu dávku indexu nahrávání, sloučí nebo odstraní
* Maximální 32 polí v klauzuli $orderby
* Maximální hledání termín velikost je 32 766 bajtů (32 KB minus 2 bajtů) textu ve formátu UTF-8

<sup>1</sup> ve službě Azure Search je hello text požadavku je subjektu tooan horní limit 16 MB, nastavení praktické omezení na hello obsah jednotlivých polí nebo kolekce, které nejsou v opačném případě omezené teoretické omezení (viz [podporované datové typy](https://msdn.microsoft.com/library/azure/dn798938.aspx) Další informace o omezení a pole složení).

## <a name="api-response-limits"></a>Omezení odpovědi rozhraní API
* Maximální 1000 dokumenty vrácené na stránku výsledků hledání
* Maximální 100 návrhy vrátí na žádost navrhnout rozhraní API

## <a name="api-key-limits"></a>Omezení klíč rozhraní API
Klíče API Key se používají pro ověřování služby. Existují dva typy. Správce klíčů se zadaným v hlavičce žádosti hello a udělit přístup Úplné čtení a zápis toohello služby. Klíče dotazu jsou jen pro čtení, zadané v adrese URL hello a tooclient obvykle distribuované aplikace.

* Maximálně 2 klíče správce pro službu
* Maximální počet klíčů 50 dotaz na službu
