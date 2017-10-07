---
title: aaaEnabling end tooend protokolu SSL na Azure Application Gateway | Microsoft Docs
description: "Tato stránka obsahuje přehled hello Application Gateway end tooend podporu protokolu SSL."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: c5cb398a1e7d9a08662a3120baad98edb5575917
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-end-tooend-ssl-with-application-gateway"></a>Přehled end tooend SSL s aplikační brány

Aplikační brána podporuje ukončení protokolu SSL na hello brány, po které obvykle přenosy dat bez šifrování toohello back-end serverů. Tato funkce umožňuje webové servery toobe unburdened z nákladná režii šifrování a dešifrování. Ale někteří zákazníci nešifrovaná komunikace toohello back-end serverech není přijatelné možnost. Tato nešifrovaná komunikace může být z důvodu toosecurity požadavky, požadavky na dodržování předpisů, nebo hello aplikace přijímá pouze zabezpečené připojení. Pro tyto aplikace, aplikační brána podporuje end tooend SSL šifrování.

## <a name="overview"></a>Přehled

End tooend SSL můžete toosecurely přenášet back-end toohello citlivá data šifrují, když poskytuje i nadále využívat výhod hello výhod funkce Vyrovnávání zatížení vrstvy 7 které aplikační brány. Některé z těchto funkcí jsou spřažení na základě souboru cookie relace, na základě adresy URL směrování, podporu pro směrování na základě lokality, nebo možnost tooinject X - předávaných-* hlavičky.

Jestliže nakonfigurované s režimem komunikace SSL tooend end, aplikační brána hello relací SSL na hello brány a dešifruje provozu generovaného uživateli. Pak provede hello nakonfigurovaná pravidla tooselect odpovídající back-end fondu instance tooroute provoz do. Aplikační brána pak inicializuje nový toohello back-end server připojení SSL a znovu je zašifruje data před přenosem hello požadavek toohello back-end pomocí certifikátu veřejného klíče hello back-end serveru. End tooend, který je povolen protokol SSL nastavením nastavení protokolu v BackendHTTPSetting tooHTTPS, který je pak použije tooa back-endový fond. Každý server back-end ve fondu back-end hello s tooend end, který povolen protokol SSL musí být nakonfigurované zabezpečené komunikace tooallow certifikátu.

![end tooend ssl scénář][1]

V tomto příkladu požadavky pomocí TLS1.2 jsou směrované toobackend v Pool1 pomocí end tooend SSL.

## <a name="end-tooend-ssl-and-whitelisting-of-certificates"></a>Ukončení tooend SSL a povolených certifikátů

Aplikační brána komunikuje pouze se službou známé back-end instancí, které mají seznam povolených adres jejich certifikát s hello aplikační brány. tooenable povolených certifikáty, musíte nahrát hello veřejný klíč back-end serveru certifikáty toohello aplikační brány (ne hello kořenový certifikát). Pouze připojení tooknown a seznam povolených adres back-EndY jsou poté povoleny. Hello zbývající back-EndY výsledkem chyba brány. Certifikáty podepsané svým držitelem slouží pouze k testování a nedoporučují se pro úlohy v produkčním prostředí. Tyto certifikáty mít seznam povolených adres toobe s hello aplikační bránu, jak je popsáno v předchozích krocích před použitím hello.

## <a name="next-steps"></a>Další kroky

Po získání informací o end tooend SSL, přejděte příliš[povolit koncové tooend SSL na aplikační brána](application-gateway-end-to-end-ssl-powershell.md) toocreate brány aplikace pomocí ukončení tooend SSL.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
