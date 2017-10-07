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
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="47966-103">Začínáme s hybridními připojeními pro přenos</span><span class="sxs-lookup"><span data-stu-id="47966-103">Get started with Relay Hybrid Connections</span></span>

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="47966-104">Tento kurz obsahuje úvod příliš[Azure předávání hybridní připojení](relay-what-is-it.md#hybrid-connections)a ukazuje, jak toouse Node.js toocreate klientskou aplikaci, která odesílá zprávy tooa odpovídající naslouchací proces aplikace.</span><span class="sxs-lookup"><span data-stu-id="47966-104">This tutorial provides an introduction too[Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how toouse Node.js toocreate a client application that sends messages tooa corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="47966-105">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="47966-105">What will be accomplished</span></span>

<span data-ttu-id="47966-106">Protože služba Hybrid Connections vyžaduje komponentu klienta i serveru, vytvoříme v tomto kurzu dvě konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="47966-106">Because Hybrid Connections requires both a client and a server component, we will create two console applications in this tutorial.</span></span> <span data-ttu-id="47966-107">Zde jsou kroky hello:</span><span class="sxs-lookup"><span data-stu-id="47966-107">Here are hello steps:</span></span>

1. <span data-ttu-id="47966-108">Vytvořte obor názvů předávání pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47966-108">Create a Relay namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="47966-109">Umožňuje vytvořte hybridní připojení, pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47966-109">Create a hybrid connection, using hello Azure portal.</span></span>
3. <span data-ttu-id="47966-110">Zápis serveru zpráv tooreceive aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="47966-110">Write a server console application tooreceive messages.</span></span>
4. <span data-ttu-id="47966-111">Zapsat klienta zprávy toosend aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="47966-111">Write a client console application toosend messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47966-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="47966-112">Prerequisites</span></span>

1. <span data-ttu-id="47966-113">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="47966-113">[Node.js](https://nodejs.org/en/).</span></span>
2. <span data-ttu-id="47966-114">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="47966-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="47966-115">1. Vytvoření oboru názvů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="47966-115">1. Create a namespace using hello Azure portal</span></span>

<span data-ttu-id="47966-116">Pokud už máte obor názvů předávání vytvořen, přeskočit toohello [vytvořit hybridní připojení pomocí portálu Azure hello](#2-create-a-hybrid-connection-using-the-azure-portal) části.</span><span class="sxs-lookup"><span data-stu-id="47966-116">If you already have a Relay namespace created, jump toohello [Create a hybrid connection using hello Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a><span data-ttu-id="47966-117">2. Vytvořit hybridní připojení pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="47966-117">2. Create a hybrid connection using hello Azure portal</span></span>

<span data-ttu-id="47966-118">Pokud již máte vytvořit hybridní připojení, přeskočit toohello [vytvořit aplikaci typu server](#3-create-a-server-application-listener) části.</span><span class="sxs-lookup"><span data-stu-id="47966-118">If you already have a hybrid connection created, jump toohello [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="47966-119">3. Vytvoření serverové aplikace (naslouchací proces)</span><span class="sxs-lookup"><span data-stu-id="47966-119">3. Create a server application (listener)</span></span>

<span data-ttu-id="47966-120">toolisten a příjem zpráv z hello předávání, jsme zapíše konzolovou aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="47966-120">toolisten and receive messages from hello Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="47966-121">4. Vytvoření klientské aplikace (odesílatel)</span><span class="sxs-lookup"><span data-stu-id="47966-121">4. Create a client application (sender)</span></span>

<span data-ttu-id="47966-122">toosend zpráv toohello předávání, jsme zapíše konzolovou aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="47966-122">toosend messages toohello Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-hello-applications"></a><span data-ttu-id="47966-123">5. Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="47966-123">5. Run hello applications</span></span>

1. <span data-ttu-id="47966-124">Spusťte aplikaci server hello: z typu příkazového řádku Node.js `node listener.js`.</span><span class="sxs-lookup"><span data-stu-id="47966-124">Run hello server application: from a Node.js command prompt type `node listener.js`.</span></span>
2. <span data-ttu-id="47966-125">Spuštění klienta aplikace hello: z typu příkazového řádku Node.js `node sender.js`a zadejte nějaký text.</span><span class="sxs-lookup"><span data-stu-id="47966-125">Run hello client application: from a Node.js command prompt type `node sender.js`, and enter some text.</span></span>
3. <span data-ttu-id="47966-126">Zkontrolujte, že server hello aplikace konzoly výstupy hello text, který byl zadán v aplikaci klienta hello.</span><span class="sxs-lookup"><span data-stu-id="47966-126">Ensure that hello server application console outputs hello text that was entered in hello client application.</span></span>

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

<span data-ttu-id="47966-128">Blahopřejeme, vytvořili jste kompletní aplikaci Hybrid Connections pomocí Node.js!</span><span class="sxs-lookup"><span data-stu-id="47966-128">Congratulations, you have created an end-to-end Hybrid Connections application using Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="47966-129">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="47966-129">Next steps:</span></span>

* [<span data-ttu-id="47966-130">Přenos – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="47966-130">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="47966-131">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="47966-131">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="47966-132">Začínáme s .NET</span><span class="sxs-lookup"><span data-stu-id="47966-132">Get started with .NET</span></span>](relay-hybrid-connections-dotnet-get-started.md)
* [<span data-ttu-id="47966-133">Začínáme s aplikací Node</span><span class="sxs-lookup"><span data-stu-id="47966-133">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

