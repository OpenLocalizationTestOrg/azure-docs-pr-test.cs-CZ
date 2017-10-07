---
title: "Přehled monitorování pro Azure Application Gateway aaaHealth | Microsoft Docs"
description: "Další informace o hello funkcemi sledováním v Azure Application Gateway"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 5091d80394a354ff849ce7ccee8cc9d2fd0456db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-health-monitoring-overview"></a>Monitorování přehled stavu aplikace brány

Ve výchozím nastavení služba Azure Application Gateway monitoruje stav hello všechny prostředky v jeho fond back-end a automaticky odebere všechny prostředků z fondu hello považoval za poškozený. Aplikační brána pokračuje toomonitor hello není v pořádku instancí a přidá je zpět toohello pořádku fond back-end, jakmile budou k dispozici a reakce toohealth testy. Aplikační brána odešle sondy stavu hello s hello stejný port, který je definován v nastavení HTTP back-end hello. Tato konfigurace zajistí, že tento test hello je testování hello stejný port, že zákazníci by pomocí tooconnect toohello back-end.

![Příklad testu brány aplikace][1]

Přidání toousing výchozí stav testu monitorování, můžete také přizpůsobit toosuit test stavu hello požadavky vaší aplikace. V tomto článku jsou popsané výchozí i vlastní stavu sondy.

> [!NOTE]
> Pokud je skupina NSG na podsítě brány aplikace, musí být otevřen rozsahy portů 65503 65534 v podsíti hello aplikační brány pro příchozí provoz. Tyto porty jsou povinné pro hello back-end stavu rozhraní API toowork.

## <a name="default-health-probe"></a>Výchozí kontroly stavu

Služby application gateway automaticky nakonfiguruje výchozí kontrolu stavu, když nemáte nastavíte žádnou konfiguraci vlastní test paměti. monitorování chování Hello funguje tak, že, ze kterého navázalo HTTP žádost toohello IP adresy nakonfigurované pro fond back-end hello. Pro výchozí sondy Pokud jsou nastavení http back-end hello nakonfigurovány pro protokol HTTPS, test hello používá protokol HTTPS jako dobře tootest stav back-EndY hello.

Příklad: konfigurace vaší aplikace brány toouse back-end serverů A, B a C tooreceive HTTP síťový provoz na portu 80. sledování stavu výchozí Hello testy tři servery hello každých 30 sekund pro pořádku odpověď HTTP. Je v pořádku odpovědi HTTP [stavový kód](https://msdn.microsoft.com/library/aa287675.aspx) 200 až 399.

Pokud kontrola test výchozí hello nezdaří pro server A, hello Aplikační brána se odebere z jeho fond back-end a síťový provoz bude zastaven toothis serveru. Hello výchozí kontroly stále přetrvává toocheck pro server každých 30 sekund. Server A odpovídá úspěšně tooone žádosti z výchozí kontrolu stavu, je znovu přidal jako v pořádku toohello fond back-end a provoz začnou komunikovat toohello server znovu.

### <a name="default-health-probe-settings"></a>Výchozí nastavení kontroly stavu

| Vlastnost testu | Hodnota | Popis |
| --- | --- | --- |
| Adresa URL testu |http://127.0.0.1:\<portu\>/ |Cesta adresy URL |
| Interval |30 |Interval testu paměti v sekundách |
| Časový limit |30 |Časový limit testu v sekundách |
| Prahová hodnota špatného stavu |3 |Počet opakování testu. Hello back-end serveru je označena po počet po sobě jdoucích test selhání hello dosáhne hello prahová hodnota špatného stavu. |

> [!NOTE]
> Hello portu je hello stejný port jako nastavení HTTP back-end hello.

Hello výchozí kontroly zjistí pouze http://127.0.0.1:\<port\> toodetermine stav. Pokud potřebujete tooconfigure hello stav testu toogo tooa vlastní adresu URL nebo změnit další nastavení, musíte použít vlastní testy paměti, jak je popsáno v hello následující kroky:

## <a name="custom-health-probe"></a>Test vlastní stavu

Vlastní testy paměti umožní toohave podrobnější řídit hello sledování stavu. Pokud používáte vlastní testy paměti, můžete nakonfigurovat interval testu hello hello adresy URL a cesty tootest a kolik neúspěšných odpovědí tooaccept před označením hello fond back-end instance jako chybné.

### <a name="custom-health-probe-settings"></a>Nastavení testu vlastní stavu

Hello následující tabulka obsahuje definice pro vlastnosti hello vlastní stav kontroly.

| Vlastnost testu | Popis |
| --- | --- |
| Name (Název) |Název testu hello. Tento název je použité toorefer toohello testu v nastavení HTTP back-end. |
| Protocol (Protokol) |Používá protokol toosend hello testu. Test Hello používá protokol hello definované v nastavení HTTP back-end hello |
| Hostitel |Hostitele název toosend hello testu. Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1". Tato hodnota se liší od název hostitele virtuálního počítače. |
| Cesta |Relativní cesta hello kontroly. platná cesta Hello spustí z '/'. |
| Interval |Interval testu paměti v sekundách. Tato hodnota je hello časový interval mezi dvě po sobě jdoucích sondy. |
| Časový limit |Časový limit testu v sekundách. Pokud není platnou odpověď v rámci tento časový limit, hello probe je označen jako se nezdařilo.  |
| Prahová hodnota špatného stavu |Počet opakování testu. Hello back-end serveru je označena po počet po sobě jdoucích test selhání hello dosáhne hello prahová hodnota špatného stavu. |

> [!IMPORTANT]
> Pokud aplikace brána je nakonfigurovaná pro jednu lokalitu, ve výchozím nastavení hello hostitele název musí být zadán jako "127.0.0.1", pokud nebudou jinak nakonfigurovaná v vlastní test paměti.
> Pro referenci vlastní test paměti se odesílají příliš\<protokol\>://\<hostitele\>:\<port\>\<cestu\>. Hello port používá, bude text hello stejný port, jak jsou definovány v nastavení HTTP back-end hello.

## <a name="next-steps"></a>Další kroky
Po informací o sledování stavu Application Gateway, můžete nakonfigurovat [test vlastní stavu](application-gateway-create-probe-portal.md) v hello portál Azure nebo [test vlastní stavu](application-gateway-create-probe-ps.md) pomocí prostředí PowerShell a hello Azure Resource Manager model nasazení.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
