---
title: "aaaAdding Azure Search tooBlob úložiště | Microsoft Docs"
description: "Vytvořte index v kódu pomocí hello HTTP REST API služby Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: a3801790067bf169693b500e83799286b7387a77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Prohledávání služby Blob Storage pomocí Azure Search

Hledání mezi hello různé typy obsahu, které jsou uložené v úložišti objektů Blob v Azure může být obtížné problém toosolve. Však mohou indexu a hledání hello obsah objektů BLOB v pouhých několika kliknutích pomocí Azure Search. Hledání v úložišti objektů Blob vyžaduje zřízení služby Azure Search. Hello různé omezení služby a cenových úrovní Azure Search najdete na hello [stránce s cenami](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Co je Azure Search?
[Služba Azure Search](https://aka.ms/whatisazsearch) je řešení vyhledávání, které lze snadno pro vývojáře tooadd robustní fulltextové vyhledávání vyskytne tooweb a mobilní aplikace. Jako služba Azure Search odebere hello nutné toomanage jakékoliv infrastruktury vyhledávání při nabídky [99,9 % dostupnost SLA](https://aka.ms/azuresearchsla).

Azure Search s Rozšířená podpora pro 56 jazyky, můžete analyzovat obsah a inteligentně zpracování konstrukce pro specifický jazyk. Služba Azure Search také nabízí nástroje toobuild hello bohaté vyhledávání uživatelské prostředí. Je jednoduchý tooadd funkce, jako je Fasetové navigace, návrhy vyhledávání typeahead a přístupů zvýraznění toouser rozhraní používání služby Azure Search. toolearn o funkcích Azure Search, můžete si přečíst hello služby [dokumentaci](https://aka.ms/azsearchdocs).

## <a name="crack-open-and-search-through-hello-content-of-enterprise-document-formats"></a>Otevřete lom a hledání prostřednictvím hello obsah dokumentu formáty enterprise
S podporou [dokumentu extrakce](https://aka.ms/azsblobindexer) v Azure Blob storage, mohou indexu Azure Search hello obsahu škálu typy souborů ukládají do objektů blob:
- PDF
- Aplikace Microsoft Office: DOCX/DOC, XLSX nebo XLS, PPTX/PPT, MSG (Outlook e-mailů)
- HTML
- Soubory ve formátu prostého textu

Extrahováním metadat z těchto typů souborů a text je snadno toosearch napříč více formátů souborů s jeden dotaz toofind hello dokumenty nejdůležitější bez ohledu na typ. Pomocí indexování metadat obsahu a hello hello dokumentů Microsoft Office, PDF a e-mailů, možné toobuild obsahu robustní podnikové řešení pro správu pomocí úložiště objektů Blob a Azure Search.

## <a name="search-through-your-blob-metadata"></a>Hledání v metadata objektu blob
Běžný scénář, který umožňuje snadno toosort prostřednictvím objektů BLOB s libovolným typem obsahu je metadata objektu blob vlastní, uživatelsky definované hello tooindex, jakož i hello vlastnosti systému pro každou z objektů BLOB. Tímto způsobem je indexovaný informace pro každý objekt blob bez ohledu na typ dokumentu v blob hello, takže lze seřadit a omezující vlastnost hello napříč všemi obsahu úložiště objektů Blob.

[Podívejte se, že informace o indexování blob metadat.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Obrázek hledání
Služba Azure Search fulltextové vyhledávání, Fasetové navigace a možnosti řazení teď může být použité toohello metadata ukládají do objektů BLOB bitové kopie.

Pokud tyto Image jsou předem zpracované pomocí hello [počítače vize API](https://www.microsoft.com/cognitive-services/computer-vision-api) z kognitivní služby společnosti Microsoft, pak je možné tooindex hello visual obsah v každé bitové kopie, včetně rozpoznávání rozpoznávání znaků a ručně psaného nalezen. Pracujeme na přidání rozpoznávání znaků a dalších možností zpracování obrázku přímo tooAzure hledání, pokud vás zajímá tyto možnosti odešlete prosím žádost na našem [UserVoice](https://aka.ms/azsuv) nebo [e-mailu nám](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Rejstřík a vyhledávání prostřednictvím objekty BLOB JSON
Vyhledávání systému Azure může být nakonfigurované tooextract strukturovaná obsah nalezen do objektů BLOB, které obsahují JSON. Služba Azure Search může číst objekty BLOB JSON a analyzovat hello strukturovaná obsah do příslušných polí hello dokumentu Azure Search. Vyhledávání systému Azure můžete taky využít objekty BLOB, které obsahují pole objektů JSON a mapování každý element tooa samostatné Azure Search dokument.

Všimněte si, že analýza JSON není aktuálně lze změnit v nastavení portálu hello. [Další informace o analýze JSON ve službě Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Rychlý start
Vyhledávání systému Azure, mohou být přidány tooblobs přímo z okno portálu úložiště objektů Blob hello.

![](./media/search-blob-storage-integration/blob-blade.png)

Kliknutím na možnost "Přidat Azure Search" hello spouští toku, kde můžete vybrat existující službu Azure Search nebo vytvořit novou službu. Pokud vytvoříte novou službu, přejde z prostředí portálu účtu úložiště. Budete potřebovat toore-přejděte okno portálu toohello úložiště a znovu vyberte možnost "Přidat Azure Search" hello, kde můžete vybrat existující službu hello.

### <a name="next-steps"></a>Další kroky
Další informace o hello Indexer objektu Blob Azure Search v hello úplné [dokumentaci](https://aka.ms/azsblobindexer).
