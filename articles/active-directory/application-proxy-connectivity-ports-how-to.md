---
title: "aaaHow tooopen hello porty brány firewall potřebné pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Zjistěte, jaké porty tooopen pro hello Azure AD Application Proxy toowork správně"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cdc7badb7c15591689a3bfd6bb26da182b00fb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-hello-firewall-ports-required-for-an-application-proxy-application"></a>Jak tooopen hello porty brány firewall, které jsou potřebné pro aplikaci Proxy aplikace

toosee úplný seznam hello požadované porty a hello funkci každý port, najdete v části požadavky hello hello [Proxy aplikace dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable). Všimněte si, že Proxy aplikace používá jenom Odchozí porty.

Můžete také zkontrolovat, jestli máte všechny otevřené porty hello požadované podle otevírání hello [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/) z vaší místní sítě. Další zelené značky zaškrtnutí znamená větší odolnost proti chybám. 

## <a name="app-proxy-regions"></a>Oblasti Proxy aplikace

Pracujeme na způsob, jak toolet víte, které z těchto oblastí musí toobe zelená za vás. Teď Ujistěte se, že všechny jsou. Také je vyžadována bez ohledu na to, které oblasti jsou ve střed USA.

toomake zda hello nástroj poskytuje hello správné výsledky, nezapomeňte:

-   Nástroj hello v prohlížeči otevřít z hello serveru s nainstalovanou hello konektor.

-   Zajistěte, aby všechny proxy nebo brány firewall použít tooyour konektor jsou také aplikováno toothis stránky. To lze provést v aplikaci Internet Explorer tak, že přejdete příliš**nastavení**  - &gt; **Možnosti Internetu**  - &gt; **připojení**  - &gt; **Nastavení místní sítě**. Na této stránce najdete v části hello pole "Použití Proxy serveru pro vaše místní sítě". Zaškrtněte toto políčko a uveďte adresu proxy serveru hello do pole "Address" hello.

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
