---
title: "Přidávání do úložiště objektů Blob Azure Search | Microsoft Docs"
description: "Vytvořte index v kódu pomocí rozhraní HTTP REST API Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: 0cd0cbb05c465d32a9ef02f9350ebdf9ccea36c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Prohledávání služby Blob Storage pomocí Azure Search

Hledání mezi různé typy obsahu, které jsou uložené v úložišti objektů Blob v Azure může být obtížné problém vyřešit. Můžete však indexu a vyhledávat obsah objektů BLOB v pouhých několika kliknutích pomocí Azure Search. Hledání v úložišti objektů Blob vyžaduje zřízení služby Azure Search. Různé limity služby a cenových úrovní Azure Search najdete na [stránce s cenami](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Co je Azure Search?
[Služba Azure Search](https://aka.ms/whatisazsearch) je řešení vyhledávání, které usnadňuje vývojářům přidat vyskytne robustní fulltextové vyhledávání pro webové a mobilní aplikace. Jako služba Azure Search eliminuje nutnost ke správě infrastruktury žádné vyhledávání při nabídky [99,9 % dostupnost SLA](https://aka.ms/azuresearchsla).

Azure Search s Rozšířená podpora pro 56 jazyky, můžete analyzovat obsah a inteligentně zpracování konstrukce pro specifický jazyk. Služba Azure Search také poskytuje nástroje k vytváření hledání bohaté uživatelské prostředí. Je snadno přidat funkce, jako je například Fasetové navigace, typeahead návrhy vyhledávání a zvýrazňování uživatelská rozhraní používání služby Azure Search. Další informace o funkcích Azure Search, můžete si přečíst služby [dokumentaci](https://aka.ms/azsearchdocs).

## <a name="crack-open-and-search-through-the-content-of-enterprise-document-formats"></a>Otevřete lom a hledání prostřednictvím obsah dokumentu formáty enterprise
S podporou [dokumentu extrakce](https://aka.ms/azsblobindexer) v úložišti objektů Blob v Azure, můžete indexu Azure Search obsahu škálu typy souborů ukládají do objektů blob:
- PDF
- Aplikace Microsoft Office: DOCX/DOC, XLSX nebo XLS, PPTX/PPT, MSG (Outlook e-mailů)
- HTML
- Soubory ve formátu prostého textu

Extrahováním metadat z těchto typů souborů a text je snadné více formátů souborů s jeden dotaz, který najde nejdůležitější dokumenty bez ohledu na typ prohledávání. Indexování metadat dokumentů Microsoft Office, souborů PDF a e-mailů a obsah, se dá k vytvoření řešení správy obsahu robustní enterprise pomocí úložiště objektů Blob a Azure Search.

## <a name="search-through-your-blob-metadata"></a>Hledání v metadata objektu blob
Běžný scénář, který usnadňuje řazení prostřednictvím objektů BLOB s libovolným typem obsahu je index metadata objektu blob vlastní, uživatelsky definované, jakož i vlastnosti systému pro každou z objektů BLOB. Tímto způsobem je indexovaný informace pro každý objekt blob bez ohledu na typ dokumentu v objektu blob, lze seřadit a omezující vlastnost napříč všemi obsahu úložiště objektů Blob.

[Podívejte se, že informace o indexování blob metadat.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Obrázek hledání
Fulltextové vyhledávání, Fasetové navigace a řazení možnosti Azure Search je teď použít pro metadata ukládají do objektů BLOB bitové kopie.

Pokud tyto Image jsou předem zpracované pomocí [počítače vize API](https://www.microsoft.com/cognitive-services/computer-vision-api) z kognitivní služby společnosti Microsoft, je možné indexování visual najít v každé bitové kopie, včetně rozpoznávání rozpoznávání znaků a ručně psaného obsahu. Pracujeme na přidání rozpoznávání znaků a další možnosti zpracování bitovou kopii přímo do služby Azure Search, pokud vás zajímá tyto možnosti odešlete prosím žádost na našem [UserVoice](https://aka.ms/azsuv) nebo [e-mailu nám](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Rejstřík a vyhledávání prostřednictvím objekty BLOB JSON
Vyhledávání systému Azure se dá nakonfigurovat k extrakci strukturovaných obsah nalezen do objektů BLOB, které obsahují JSON. Služba Azure Search může číst objekty BLOB JSON a analyzovat strukturovaných obsah do příslušných polí dokument Azure Search. Vyhledávání systému Azure můžete taky využít objekty BLOB, které obsahují pole objektů JSON a mapování každý prvek na samostatný dokument Azure Search.

Všimněte si, že analýza JSON není aktuálně lze změnit v nastavení portálu. [Další informace o analýze JSON ve službě Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Rychlý start
Služba Azure Search lze přidat na objekty BLOB přímo z okna portálu úložiště objektů Blob.

![](./media/search-blob-storage-integration/blob-blade.png)

Kliknutím na možnost "Přidat Azure Search" spustí toku, kde můžete vybrat existující službu Azure Search nebo vytvořit novou službu. Pokud vytvoříte novou službu, přejde z prostředí portálu účtu úložiště. Bude muset znovu přejděte do okna portálu úložiště a znovu vyberte možnost "Přidat Azure Search", kde můžete vybrat existující službu.

### <a name="next-steps"></a>Další kroky
Další informace o indexeru pro hledání objektů Blob Azure v úplném [dokumentaci](https://aka.ms/azsblobindexer).
