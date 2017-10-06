---
title: "aaaTask předvolby pro Azure Media Indexer"
description: "Toto téma poskytuje přehled úloh předvolby pro Azure Media Indexer."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: ca0b3e7aa9f6dd9fdecddfc5b3137281ed5cef35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="task-preset-for-azure-media-indexer"></a>Úloha předvolby pro Azure Media Indexer

Azure Media Indexer je procesor médií, že používáte tooperform hello následující úlohy: Zkontrolujte soubory médií a obsah, s možností vyhledávání, generování uzavřené titulků sleduje a klíčová slova, indexu soubory prostředků, které jsou součástí asset.

Toto téma popisuje hello přednastavení úloh, je nutné, aby toopass tooyour indexování úlohy. Úplný příklad najdete v tématu [indexování mediálních souborů pomocí Azure Media Indexer](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer konfigurace XML

Hello následující tabulka popisuje elementy a atributy hello konfigurace XML.

|Name (Název)|Vyžadovat|Popis|
|---|---|---|
|Vstup|Hodnota TRUE|Asset soubory, které chcete tooindex.<br/>Azure Media Indexer podporuje následující formáty souborů médií hello: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Můžete zadat název souboru hello (s) v hello **název** nebo **seznamu** atribut hello **vstupní** – element (jak je znázorněno níže). Pokud nezadáte, které tooindex souboru asset, primární soubor hello vydán. Pokud není nastaven žádný soubor primární asset, je indexovaný hello první soubor v hello vstupní asset.<br/><br/>tooexplicitly zadejte název souboru hello asset, proveďte:<br/>```<input name="TestFile.wmv" />```<br/><br/>Můžete také indexu více souborů asset najednou (až too10 soubory). toodo toto:<br/>-Vytvořte textový soubor (soubor manifestu) a dejte mu .lst rozšíření.<br/>-Přidáte seznam všech názvů souborů asset hello v souboru manifestu toothis vstupní asset.<br/>-Přidáte (nahrávání) thanifest souboru toohello asset.<br/>-Zadejte hello název souboru manifestu hello v atributu seznamu hello vstup.<br/>```<input list="input.lst">```<br/><br/>**Poznámka:** Pokud přidáte víc než 10 souboru manifestu toohello soubory, úlohy indexování hello selže s kódem chyby hello 2006.|
|Metadata|False|Metadata pro hello zadat soubory asset.<br/>```<metadata key="..." value="..." />```<br/><br/>Můžete zadat hodnoty pro předdefinované klíče. <br/><br/>V současné době jsou podporovány hello následující klíče:<br/><br/>**název** a **popis** -použít přesnost rozpoznávání řeči modelu tooupdate hello jazyk tooimprove.<br/>```<metadata key="title" value="[Title of hello media file]" /><metadata key="description" value="[Description of hello media file]" />```<br/><br/>**uživatelské jméno** a **heslo** – používá pro ověřování při stahování souborů Internetu prostřednictvím protokolu http nebo https.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>Hello uživatelské jméno a heslo hodnoty použít tooall média adresy URL v manifestu vstupní hello.|
|Database<br/><br/>Přidaná do verze 1.2. Funkce hello podporována pouze v současné době je rozpoznávání řeči ("ASR").|False|funkce rozpoznávání řeči Hello má hello následující nastavení klíče:<br/><br/>Jazyk:<br/>-hello přirozeného jazyka toobe rozpoznán v hello multimediálních souborů.<br/>-Angličtina, španělština<br/><br/>CaptionFormats:<br/>-středníky oddělený seznam hello požadovaných formátů titulek výstup (pokud existuje)<br/>-ttml; sami; webvtt<br/><br/><br/>GenerateAIB:<br/>-A logický příznak určující, zda je soubor AIB požadované (pro použití se službou SQL Server a hello zákazníka Indexer IFilter). Další informace najdete v tématu pomocí AIB souborů pomocí Azure Media Indexer a SQL Server.<br/>-True; False<br/><br/>GenerateKeywords:<br/>-A logický příznak určující, zda je požadovaný soubor XML – klíčové slovo.<br/>-True; False.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Ukázkový kód XML konfigurace služby Azure Media Indexer

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of hello media file]" />  
    <metadata key="description" value="[Description of hello media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="CaptionFormats" value="ttml;sami;webvtt"/>  
        <add key="GenerateAIB" value ="true" />  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Další kroky

V tématu [indexování mediálních souborů pomocí Azure Media Indexer](media-services-index-content.md).

