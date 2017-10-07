---
title: "aaaUsing Azure DNS s jinými službami Azure | Microsoft Docs"
description: "Pochopení, jak Azure DNS tooresolve toouse název jinými službami Azure"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: gwallace
ms.openlocfilehash: 791f93d6aff2c638c08518e9f1e8ab89ac8de3f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Jak funguje Azure DNS s jinými službami Azure

Azure DNS je hostovanou službu DNS správu a název řešení. To umožňuje vám toocreate veřejné názvy DNS hello dalšími aplikacemi a službami, které jste nasadili v Azure. Vytváření název služby Azure v vaše vlastní doména je jednoduché, přidání záznam hello správný typ služby.

* Pro dynamicky přidělené IP adresy musíte vytvořit záznam DNS CNAME tento název DNS toohello mapy, který vytvořili Azure pro vaši službu. Standardech DNS zabránit vám v použití záznam CNAME pro vrcholu zóny hello.
* Pro staticky přidělené IP adresy, můžete vytvořit záznam DNS A používat libovolný název, včetně *holé domény* názvem na vrcholu zóny hello.

Hello následující tabulka obsahuje přehled hello podporované typy záznamů, které lze použít k různým službám Azure. Jak vidíte z této tabulky, podporuje Azure DNS pouze záznamy DNS pro internetový síťové prostředky. Azure DNS nelze použít pro překlad názvu interní, privátní adresy.

| Služba Azure | Síťové rozhraní | Popis |
| --- | --- | --- |
| Application Gateway |[Veřejná IP adresa front-endu](dns-custom-domain.md#public-ip-address) |Můžete vytvořit záznam CNAME nebo DNS A. |
| Load Balancer |[Veřejná IP adresa front-endu](dns-custom-domain.md#public-ip-address)  |Můžete vytvořit záznam CNAME nebo DNS A. Nástroj pro vyrovnávání zatížení může mít veřejnou IP adresu IPv6 adresu, která se dynamicky přiřadit. Proto musíte vytvořit záznam CNAME pro adresu IPv6. |
| Traffic Manager |Veřejný název |Můžete vytvořit pouze záznam CNAME, který mapuje název trafficmanager.net toohello přiřazené tooyour profil služby Traffic Manager. Další informace najdete v tématu [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#traffic-manager-example). |
| Cloudové služby |[Veřejná IP adresa](dns-custom-domain.md#public-ip-address) |Pro staticky přidělené IP adresy můžete vytvořit záznam DNS A. Pro dynamicky přidělené IP adresy, musíte vytvořit záznam CNAME, který mapuje toohello *cloudapp.net* název. Toto pravidlo aplikuje tooVMs v portálu classic hello vytvořit, protože jsou nasazeny jako cloudová služba. Další informace najdete v tématu [konfigurace vlastního názvu domény v cloudové službě](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App Service | [Externí IP](dns-custom-domain.md#app-service-web-apps) |Pro externí IP adresy můžete vytvořit záznam DNS A. V ostatních případech musíte vytvořit záznam CNAME, který mapuje název toohello azurewebsites.net. Další informace najdete v tématu [mapy tooan název vlastní domény aplikace Azure](../app-service-web/web-sites-custom-domain-name.md) |
| Virtuální počítače správce prostředků |[Veřejná IP adresa](dns-custom-domain.md#public-ip-address) |Virtuální počítače správce prostředků může mít veřejné IP adresy. Virtuální počítač s veřejnou IP adresu. může být také za službou Vyrovnávání zatížení. Můžete vytvořit záznam CNAME nebo DNS A pro hello veřejné adresy. Vlastní název nesmí být použité toobypass hello VIP adresa zákazníka ve Vyrovnávání zatížení hello. |
| Klasické virtuální počítače |[Veřejná IP adresa](dns-custom-domain.md#public-ip-address) |Klasické virtuální počítače vytvořené pomocí prostředí PowerShell nebo rozhraní příkazového řádku se dá nakonfigurovat s dynamická nebo statická (vyhrazená) virtuální adresu. Vytvořením DNS CNAME nebo záznam, v uvedeném pořadí. |
