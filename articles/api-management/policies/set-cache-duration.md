---
title: "Azure API management zásad ukázka - nastavení doby trvání mezipaměti odpověď | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak nastavit dobu trvání mezipaměti odpovědi v maxAge hodnotu hlavičky Cache-Control poslal back-end..."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 8471b51999159c8f6233abbbce998b122163efd8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="set-response-cache-duration"></a>Nastavení doby trvání mezipaměti odpovědi

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak nastavit dobu trvání mezipaměti odpovědi v maxAge hodnotu hlavičky Cache-Control poslal back-end. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

