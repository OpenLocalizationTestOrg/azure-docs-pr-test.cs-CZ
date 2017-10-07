---
title: "nastavení portu předávání aaaAzure | Microsoft Docs"
description: "Údaje o předávání přes Azure hodnoty portů."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: e66785f786ee241c974d250f9ec29dfcc1fdc3fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-port-settings"></a>Nastavení portu Azure předávání

Hello následující tabulka popisuje hello požadovanou konfiguraci pro hodnoty portů pro předávání přes Azure.

## <a name="hybrid-connections"></a>Hybridní připojení
Hybridní připojení používá jako podkladový přenosový mechanismus, který používá hello **HTTPS** pouze. 

## <a name="wcf-relays"></a>Přenosy WCF
  
|Vazba|Zabezpečení přenosu|Port|  
|-------------|------------------------|----------|  
|[Třída BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (klient)|Ano|HTTPS| 
| |" |Ne|HTTP|  
|[Třída BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (služba)|Buď|9351/HTTP|  
|[Třída NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (klient)|Ano|9351 NEBO HTTPS|  
||" |Ne|9350/HTTP|  
|[Třída NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (služba)|Buď|9351/HTTP|  
|[Třída NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (klienta nebo služba)|Buď|5671/9352/HTTP (9352/9353 Pokud používáte hybridní)|  
|[Třída NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (klient)|Ano|9351 NEBO HTTPS|  
||" |Ne|9350/HTTP|  
|[Třída NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (služba)|Buď|9351/HTTP|  
|[Třída WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (klient)|Ano|HTTPS|  
||" |Ne|HTTP|  
|[Třída WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (služba)|Buď|9351/HTTP|  
|[Třída WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (klient)|Ano|HTTPS|  
||" |Ne|HTTP|  
|[Třída WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (služba)|Buď|9351/HTTP|

## <a name="next-steps"></a>Další kroky
Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:
* [Co je Azure Relay?](relay-what-is-it.md)
* [Přenos – nejčastější dotazy](relay-faq.md)