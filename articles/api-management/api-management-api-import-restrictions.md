---
title: "Omezení a známé problémy v Azure API Management API import | Microsoft Docs"
description: "Podrobnosti o známých problémech a omezení při importu do Azure API Management pomocí otevřené rozhraní API, WSDL nebo WADL formáty."
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: 758babce3ed387ed4864f1934650cf701bda788f
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>Omezení import rozhraní API a známé problémy
## <a name="about-this-list"></a>O tomto seznamu
Při importu rozhraní API, můžete setkat při určitá omezení a identifikovat problémy, které musí být opravena, aby bylo možné úspěšně naimportovat. Tento článek dokumenty, tyto uspořádané podle formát import rozhraní API.

## <a name="open-api"></a>Otevřete rozhraní API/Swagger
Pokud obdržíte chyby import dokumentu otevřené rozhraní API, ujistěte se, jste ověřili jeho – buď pomocí návrháře na portálu Azure (návrhu - Front-endu - otevřené rozhraní API specifikace Editor), nebo s třetích stran nástroje, jako <a href="http://www.swagger.io">editoru Swagger</a>.

* **Název hostitele** APIM vyžaduje atribut název hostitele.
* **Základní cesta** APIM vyžaduje atribut základní cesta.
* **Schémata** APIM vyžaduje schéma pole. 

## <a name="wsdl"></a>WSDL
WSDL soubory se používají ke generování SOAP průchozí rozhraní API nebo sloužit jako back-end SOAP REST API.

* **WSDL: import** -APIM v současné době nepodporuje rozhraní API pomocí tohoto atributu. Zákazníci měli sloučit importované elementy do jednoho dokumentu.
* **Zprávy s více částmi** APIM v současné době nepodporuje tyto typy zpráv.
* **WCF wsHttpBinding** SOAP služby vytvořené pomocí Windows Communication Foundation by měl používat basicHttpBinding – wsHttpBinding není podporován.
* **MTOM** služby používající MTOM <em>může</em> fungovat. V tuto chvíli není nabídnuta oficiální podporu.
* **Rekurze** typy, které jsou definované rekurzivně (například odkazovat na pole sami) nejsou podporována APIM.

## <a name="wadl"></a>WADL
V současné době nejsou žádné známé problémy WADL importu.
