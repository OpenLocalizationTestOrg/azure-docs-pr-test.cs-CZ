---
title: "aaaGet začít s Azure předávání hybridní připojení v rozhraní .NET | Microsoft Docs"
description: "Napište konzolovou aplikaci v jazyce C# pro Azure Relay Hybrid Connections."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 1e4af28e7cd4393c8ca965a149a0b83ebcc44f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Začínáme s hybridními připojeními pro přenos
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Tento kurz obsahuje úvod příliš[Azure předávání hybridní připojení](relay-what-is-it.md#hybrid-connections)a ukazuje, jak toouse .NET toocreate klientskou aplikaci, která odesílá zprávy tooa odpovídající naslouchací proces aplikace. 

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme
Protože hybridní připojení vyžaduje klienta a součásti serveru, hello kurzu vytvoří dvě aplikace konzoly. Zde jsou kroky hello:

1. Vytvořte obor názvů předávání pomocí hello portálu Azure.
2. V tomto oboru názvů pomocí hello portálu Azure vytvořte hybridní připojení.
3. Zápis konzoly serveru (naslouchání) aplikace tooreceive zpráv.
4. Zápis konzoly klienta (odesílatel) aplikace toosend zpráv.

## <a name="prerequisites"></a>Požadavky

toocomplete tohoto kurzu budete potřebovat hello následující požadavky:

1. [Visual Studio 2015 nebo vyšší](http://www.visualstudio.com). Hello příklady v tomto kurzu použít Visual Studio 2017.
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Vytvoření oboru názvů pomocí hello portálu Azure
Pokud jste již vytvořili předávání názvů, přeskočit toohello [vytvořit hybridní připojení pomocí portálu Azure hello](#2-create-a-hybrid-connection-using-the-azure-portal) části.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a>2. Vytvořit hybridní připojení pomocí hello portálu Azure
Pokud jste již vytvořili hybridní připojení, přeskočit toohello [vytvořit aplikaci typu server](#3-create-a-server-application-listener) části.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Vytvoření serverové aplikace (naslouchací proces)
toolisten a přijímat zprávy z hello předávání, jsme zapíše konzolovou aplikaci C# pomocí sady Visual Studio.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Vytvoření klientské aplikace (odesílatel)
Předávání, toosend zprávy toohello jsme zapíše konzolovou aplikaci C# pomocí sady Visual Studio.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-hello-applications"></a>5. Spouštění aplikací hello
1. Spusťte aplikaci server hello.
2. Spuštění klienta aplikace hello a zadejte nějaký text.
3. Zkontrolujte, že server hello aplikace konzoly výstupy hello text, který byl zadán v aplikaci klienta hello.

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Blahopřejeme, vytvořili jste kompletní aplikaci Hybridní připojení.

## <a name="next-steps"></a>Další kroky:
* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)

