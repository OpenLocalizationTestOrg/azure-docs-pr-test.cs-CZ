---
title: "aaaRestrict obsahu Azure CDN podle země | Microsoft Docs"
description: "Zjistěte, jak toorestrict přístupu k tooyour Azure CDN obsahu pomocí funkce filtrování geograficky hello."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: ffdd994612b6c9cfbf1a6e29d260709b4afa86e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a>Omezit přístup k obsahu Azure CDN podle země

## <a name="overview"></a>Přehled
Když si uživatel vyžádá obsah, ve výchozím nastavení, hello obsahu obsluhovaného bez ohledu na to, kde uživatel hello provedené této žádosti z. V některých případech může být vhodné toorestrict přístup k obsahu tooyour podle země. Toto téma vysvětluje, jak toouse hello **filtrování geograficky** funkce v tooconfigure pořadí hello služby tooallow nebo blokování přístupu podle země.

> [!IMPORTANT]
> Verizon a Akamai produkty Hello poskytují hello stejné funkce filtrování geograficky, ale mají malý rozdíl v kódy zemí te podporují. Najdete v kroku 3 rozdíly toohello odkaz.


Informace o aspektech, které se vztahují tooconfiguring tento typ omezení, najdete v části hello [aspekty](cdn-restrict-access-by-country.md#considerations) části na konci hello hello tématu.  

![Filtrování podle země](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-hello-directory-path"></a>Krok 1: Definování cesta k adresáři hello
Vyberte váš koncový bod hello portálu a najít hello filtrování geograficky karty v levé navigační toofind hello tuto funkci.

Při konfiguraci země filtru, musíte zadat hello relativní cestu toohello umístění toowhich uživatelé se povoluje nebo odepírá přístup. Filtrování geograficky můžete použít pro všechny soubory s "/" nebo vybrané složky zadáním cesty k adresáři "/ obrázky /". Můžete taky použít geograficky filtrování tooa jedním souborem zadáním hello souboru a vynechala hello koncové lomítko "/ obrazky/Mesto.png".

Příklad filtru cesta adresáře:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-hello-action-block-or-allow"></a>Krok 2: Definování akce hello: blokování nebo povolení
**Blokování:** uživatele z hello zadaný požadovaný z této cesty rekurzivní tooassets přístup bude odepřen zemích. Pokud pro toto umístění nebyly nakonfigurovány žádné jiné země možnosti filtrování, pak všichni ostatní uživatelé budou mít povolený přístup.

**Povolit:** jen uživatelé z hello zadaný zemích budou mít povolený přístup tooassets vyžádaný z této cestě rekurzivní.

## <a name="step-3-define-hello-countries"></a>Krok 3: Definování zemích hello
Vyberte hello zemích, mají být tooblock nebo povolit pro cestu hello. 

Například hello pravidlo blokování /Photos/Štrasburku/bude filtrovat soubory včetně:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Kódy zemí
Hello **filtrování geograficky** funkce používá země kódy toodefine hello zemích ze kterých žádost povolené nebo blokované pro zabezpečené adresář. Zjistíte hello kódy zemí v [kódy zemí CDN Azure](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a id="considerations"></a>Důležité informace
* To může trvat až minut too90 Verizon nebo několik minut s Akamai, změny tooyour země filtrování konfigurace tootake vliv.
* Tato funkce nepodporuje zástupné znaky (například ' *').
* Hello filtrování geograficky konfigurace související s relativní cestou hello bude rekurzivně toothat cesta.
* Pouze jedno pravidlo může být použité toohello stejné relativní cestě (nelze vytvořit více filtrů země tento bod toohello stejné relativní cestě. Do složky, ale může mít více filtrů země. Toto je z důvodu toohello rekurzivní povaha země filtry. Jinými slovy podsložkou dříve nakonfigurované složky lze přiřadit jiné zemi filtru.

