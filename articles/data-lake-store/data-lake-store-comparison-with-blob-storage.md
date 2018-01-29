---
title: "Porovnání Azure Data Lake Store s Azure Storage Blob | Microsoft Docs"
description: "Porovnání Azure Data Lake Store s Azure Storage Blob"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 8881d28e1625664d0a124057821192aabfe1180b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Porovnání Azure Data Lake Store a Azure Blob Storage
Tabulka v tomto článku shrnuje rozdíly mezi Azure Data Lake Store a Azure Blob Storage podél některé klíčové aspekty zpracování velkých objemů dat. Azure Blob Storage je obecné účely, škálovatelné objekt úložiště, které je určená pro širokou škálu scénářů úložiště. Azure Data Lake Store je flexibilně škálovatelné úložiště, která je optimalizovaná pro úlohy analýzy velkých objemů dat.

|  | Azure Data Lake Store | Azure Blob Storage |
| --- | --- | --- |
| Účel |Optimalizované úložiště pro úlohy analýzy velkých objemů dat |Obecné účely objekt uložení pro širokou škálu scénářů úložiště |
| Případy použití |Batch, interaktivní, streamování analýzy a machine learning data, jako soubory, IoT data protokolu, klikněte na tlačítko datové proudy, rozsáhlých datových sad |Žádný druh textu nebo binárních dat, jako je například aplikace back end, zálohování dat, média úložiště pro streamování a obecné účely data |
| Klíčové koncepty |Účet data Lake Store obsahuje složky, který obsahuje data, které jsou uloženy jako soubory |Účet úložiště obsahuje kontejnery, která naopak má data v podobě objektů BLOB |
| Struktura |Hierarchický systém souborů |Objekt úložiště s plochým oborem názvů |
| Rozhraní API |REST API přes protokol HTTPS |REST API přes HTTP nebo HTTPS |
| Rozhraní API na straně serveru |[Rozhraní API REST kompatibilní s WebHDFS](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[REST API úložiště objektů Blob v Azure](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Klient systému Hadoop souboru |Ano |Ano |
| Operace dat – ověřování |Na základě [identit Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md) |Podle sdílené tajné klíče - [přístupových klíčů k účtu](../storage/common/storage-create-storage-account.md#manage-your-storage-account) a [sdíleného přístupu podpisové klíče](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Operace dat – ověřovací protokol |OAuth 2.0. Volání musí obsahovat platný (JSON Web Token) JWT vydaného Azure Active Directory |Hash-based Message Authentication Code (HMAC). Volání musí obsahovat hodnotu hash SHA-256 kódováním Base64 přes součástí požadavku HTTP. |
| Operace dat – autorizace |POSIX seznamy řízení přístupu (ACL).  Seznamy ACL založené na Azure Active Directory identity můžete nastavit úroveň souborů a složek. |Pro účet úroveň ověřování – použít [přístupových klíčů k účtu](../storage/common/storage-create-storage-account.md#manage-your-storage-account)<br>Pro účet, kontejner nebo objekt blob autorizace - použít [sdíleného přístupu podpisové klíče](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Data - auditování operací |K dispozici. V tématu [sem](data-lake-store-diagnostic-logs.md) informace. |Dostupné |
| Šifrování dat v klidovém stavu |<ul><li>Transparentní, na straně serveru</li> <ul><li>Službu spravovat klíče</li><li>Spravované zákazníkem klíče v Azure KeyVault</li></ul></ul> |<ul><li>Transparentní, na straně serveru</li> <ul><li>Službu spravovat klíče</li><li>Spravované zákazníkem klíče v Azure KeyVault (už brzy)</li></ul><li>Šifrování na straně klienta</li></ul> |
| Operace správy (například účet vytvořit) |[Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovaný platformou Azure pro správu účtu |[Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovaný platformou Azure pro správu účtu |
| Vývojáři sady SDK |Rozhraní .NET, Java, Python, Node.js |Rozhraní .net, Java, Python, Node.js, C++, Ruby |
| Analýza výkonu zatížení |Optimalizaci výkonu pro zatížení paralelní analýzy. Vysoké propustnosti a IOPS. |Není optimalizovaný pro úlohy analýzy |
| Omezení velikosti |Žádné omezení velikosti účtů, velikosti souborů nebo počet souborů |Konkrétní omezení popsaná [sem](../azure-subscription-service-limits.md#storage-limits) |
| Geografická redundance |Místně redundantní (víc kopií dat v jedné oblasti Azure) |Místně redundantní (LRS), globálně redundantní (GRS), globálně redundantní přístup pro čtení (RA-GRS). V tématu [sem](../storage/common/storage-redundancy.md) Další informace |
| Stav služby |Obecně k dispozici |Obecně k dispozici |
| Regionální dostupnost |V tématu [sem](https://azure.microsoft.com/regions/#services) |V tématu [sem](https://azure.microsoft.com/regions/#services) |
| Cena |V tématu [ceny](https://azure.microsoft.com/pricing/details/data-lake-store/) |V tématu [ceny](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Další postup
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Začínáme s Data Lake Store](data-lake-store-get-started-portal.md)

