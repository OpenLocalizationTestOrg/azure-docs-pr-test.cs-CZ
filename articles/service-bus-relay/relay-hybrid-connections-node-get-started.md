---
title: "aaaGet začít s Azure předávání hybridní připojení v uzlu | Microsoft Docs"
description: "Napište konzolovou aplikaci v Node.js pro Azure Relay Hybrid Connections."
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 235548399570074f7fd160fec28de8d3633625c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Začínáme s hybridními připojeními pro přenos

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Tento kurz obsahuje úvod příliš[Azure předávání hybridní připojení](relay-what-is-it.md#hybrid-connections)a ukazuje, jak toouse Node.js toocreate klientskou aplikaci, která odesílá zprávy tooa odpovídající naslouchací proces aplikace. 

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme

Protože služba Hybrid Connections vyžaduje komponentu klienta i serveru, vytvoříme v tomto kurzu dvě konzolové aplikace. Zde jsou kroky hello:

1. Vytvořte obor názvů předávání pomocí hello portálu Azure.
2. Umožňuje vytvořte hybridní připojení, pomocí hello portálu Azure.
3. Zápis serveru zpráv tooreceive aplikace konzoly.
4. Zapsat klienta zprávy toosend aplikace konzoly.

## <a name="prerequisites"></a>Požadavky

1. [Node.js](https://nodejs.org/en/).
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Vytvoření oboru názvů pomocí hello portálu Azure

Pokud už máte obor názvů předávání vytvořen, přeskočit toohello [vytvořit hybridní připojení pomocí portálu Azure hello](#2-create-a-hybrid-connection-using-the-azure-portal) části.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a>2. Vytvořit hybridní připojení pomocí hello portálu Azure

Pokud již máte vytvořit hybridní připojení, přeskočit toohello [vytvořit aplikaci typu server](#3-create-a-server-application-listener) části.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Vytvoření serverové aplikace (naslouchací proces)

toolisten a příjem zpráv z hello předávání, jsme zapíše konzolovou aplikaci Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Vytvoření klientské aplikace (odesílatel)

toosend zpráv toohello předávání, jsme zapíše konzolovou aplikaci Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-hello-applications"></a>5. Spouštění aplikací hello

1. Spusťte aplikaci server hello: z typu příkazového řádku Node.js `node listener.js`.
2. Spuštění klienta aplikace hello: z typu příkazového řádku Node.js `node sender.js`a zadejte nějaký text.
3. Zkontrolujte, že server hello aplikace konzoly výstupy hello text, který byl zadán v aplikaci klienta hello.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Blahopřejeme, vytvořili jste kompletní aplikaci Hybrid Connections pomocí Node.js!

## <a name="next-steps"></a>Další kroky:

* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)

