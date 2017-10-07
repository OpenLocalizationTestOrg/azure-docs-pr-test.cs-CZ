---
title: "Přehled zásad aaaSSL Azure Application Gateway. | Microsoft Docs"
description: "Další informace o Azure Application Gateway vám umožní tooconfigure zásady protokolu SSL"
services: application gateway
documentationcenter: na
author: amsriva
manager: 
editor: 
tags: azure resource manager
ms.service: application gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure services
ms.date: 08/03/2017
ms.author: amsriva
ms.openlocfilehash: 80dbc437da3dba8a558c518ecdbb5927a11a2ed9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-ssl-policy-overview"></a>Přehled zásad brány SSL aplikace

Aplikace brána vám umožní správu certifikátu SSL toocentralize a snížit režijní náklady na šifrování a dešifrování z back-end serverové farmy. Tento centrální SSL zpracování také vám umožní toospecify centrální SSL zásady vhodné tooyour požadavky na zabezpečení organizace. To pomáhá splnit požadavky na dodržování předpisů jak pokyny pro zabezpečení a doporučené postupy.

Zásady protokolu SSL zahrnuje kontrolu nad verzi protokolu SSL a také šifrovacích sad a pořadí hello šifry se používají během metody handshake pro protokol SSL. Směrem tuto bránu aplikace nabízí zásady protokolu SSL toocontrol zákazníci tooallow dvou mechanismů, i předdefinovanou nebo vlastní zásadu.

## <a name="predefined-ssl-policy"></a>Předdefinované zásady protokolu SSL

Aplikační brána obsahuje tři předdefinované zásady zabezpečení. Můžete nakonfigurovat bránu s žádným z těchto zásad tooget hello odpovídající úroveň zabezpečení. názvy zásad Hello jsou opatřeny poznámkami hello rok a měsíc, ve kterém byly nakonfigurovány. Každé zásady nabízí různé SSL protocol verze a šifrovací sady. Doporučujeme toouse hello nejnovější zásady tooensure hello nejlepší SSL zabezpečení SSL. Aplikační brána ještě dnes umožňuje uživatelům toochoose z jednoho z následujících hello předdefinované.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Vlastnost  |Hodnota  |
|---|---|
|Name (Název)     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Výchozí| Hodnota TRUE, (Pokud je zadán žádné předdefinované zásady) |
|AES     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
  ### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Vlastnost  |Hodnota  |
|   ---      |  ---       |
|Name (Název)     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_0        |
|Výchozí| False |
|AES     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Vlastnost  |Hodnota  |
|---|---|
|Name (Název)     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_2        |
|Výchozí| False |
|AES     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-ssl-policy"></a>Vlastní zásady protokolu SSL

Pokud předdefinované zásady protokolu SSL potřebuje toobe nakonfigurovaný pro vaše požadavky, je třeba musí definovat vlastní vlastní zásady protokolu SSL. V takovém případě budete mít úplnou kontrolu nad hello minimální SSL protocol verze toosupport a také podporované šifrovací sady a jejich pořadí podle priority.
 
Verze protokolu SSL:

* Protokoly SSL 2.0 a 3.0 jsou ve výchozím nastavení zakázané pro všechny brány Application Gateway. Tyto verze protokolu se nedají konfigurovat.
* Vlastní SSL zásady definice poskytuje možnost tooselect kterékoli z následujících tří protokolů jako hello minimální SSL protokolu verze vaší brány TLSv1_0, TLSv1_1, TLSv1_2 hello.
* Pokud není nadefinovaná žádná zásada SSL všechny tři (TLSv1_0, TLSv1_1, TLSv1_2) jsou povoleny.

Šifrovací sada:

Aplikační brána podporuje hello následující šifrovací sady, ze kterých je možné vybrat vlastní zásady. Hello řazení hello šifrovací sada určuje pořadí podle priority hello při vyjednávání SSL.

K dispozici šifrovací sady:

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

## <a name="next-steps"></a>Další kroky

[Konfigurace zásady protokolu SSL na aplikační brány](application-gateway-configure-ssl-policy-powershell.md)
