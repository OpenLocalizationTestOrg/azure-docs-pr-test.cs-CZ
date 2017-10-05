---
title: "Začínáme s hybridními připojeními Azure Relay v Node | Dokumentace Microsoftu"
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
ms.openlocfilehash: c3bfc45969f250059988129f532edd12dfe3dcfe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="f0cf1-103">Začínáme s hybridními připojeními pro přenos</span><span class="sxs-lookup"><span data-stu-id="f0cf1-103">Get started with Relay Hybrid Connections</span></span>

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="f0cf1-104">Tento kurz obsahuje úvod do služby [Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections) a ukazuje, jak pomocí Node.js vytvořit klientskou aplikaci, která odesílá zprávy do příslušné aplikace naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-104">This tutorial provides an introduction to [Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how to use Node.js to create a client application that sends messages to a corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="f0cf1-105">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="f0cf1-105">What will be accomplished</span></span>

<span data-ttu-id="f0cf1-106">Protože služba Hybrid Connections vyžaduje komponentu klienta i serveru, vytvoříme v tomto kurzu dvě konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-106">Because Hybrid Connections requires both a client and a server component, we will create two console applications in this tutorial.</span></span> <span data-ttu-id="f0cf1-107">Postup je následující:</span><span class="sxs-lookup"><span data-stu-id="f0cf1-107">Here are the steps:</span></span>

1. <span data-ttu-id="f0cf1-108">Pomocí webu Azure Portal vytvoříme obor názvů přenosu.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-108">Create a Relay namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="f0cf1-109">Pomocí webu Azure Portal vytvoříme hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-109">Create a hybrid connection, using the Azure portal.</span></span>
3. <span data-ttu-id="f0cf1-110">Napíšeme serverovou aplikaci pro příjem zpráv.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-110">Write a server console application to receive messages.</span></span>
4. <span data-ttu-id="f0cf1-111">Napíšeme aplikaci klientské konzoly pro příjem zpráv.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-111">Write a client console application to send messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0cf1-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f0cf1-112">Prerequisites</span></span>

1. <span data-ttu-id="f0cf1-113">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="f0cf1-113">[Node.js](https://nodejs.org/en/).</span></span>
2. <span data-ttu-id="f0cf1-114">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="f0cf1-115">1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f0cf1-115">1. Create a namespace using the Azure portal</span></span>

<span data-ttu-id="f0cf1-116">Pokud už máte vytvořený obor názvů služby Relay, přejděte do části [Vytvoření hybridního připojení pomocí webu Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="f0cf1-116">If you already have a Relay namespace created, jump to the [Create a hybrid connection using the Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a><span data-ttu-id="f0cf1-117">2. Vytvoření hybridního připojení pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f0cf1-117">2. Create a hybrid connection using the Azure portal</span></span>

<span data-ttu-id="f0cf1-118">Pokud už máte vytvořené hybridní připojení, přejděte do části [Vytvoření serverové aplikace](#3-create-a-server-application-listener).</span><span class="sxs-lookup"><span data-stu-id="f0cf1-118">If you already have a hybrid connection created, jump to the [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="f0cf1-119">3. Vytvoření serverové aplikace (naslouchací proces)</span><span class="sxs-lookup"><span data-stu-id="f0cf1-119">3. Create a server application (listener)</span></span>

<span data-ttu-id="f0cf1-120">Aby bylo možné prostřednictvím přenosu poslouchat a přijímat zprávy, napíšeme konzolovou aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-120">To listen and receive messages from the Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="f0cf1-121">4. Vytvoření klientské aplikace (odesílatel)</span><span class="sxs-lookup"><span data-stu-id="f0cf1-121">4. Create a client application (sender)</span></span>

<span data-ttu-id="f0cf1-122">Aby bylo možné prostřednictvím přenosu odesílat zprávy, napíšeme konzolovou aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-122">To send messages to the Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a><span data-ttu-id="f0cf1-123">5. Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="f0cf1-123">5. Run the applications</span></span>

1. <span data-ttu-id="f0cf1-124">Spuštění serverové aplikace: v příkazovém řádku Node.js zadejte `node listener.js`.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-124">Run the server application: from a Node.js command prompt type `node listener.js`.</span></span>
2. <span data-ttu-id="f0cf1-125">Spuštění klientské aplikace: v příkazovém řádku Node.js zadejte `node sender.js` a nějaký text.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-125">Run the client application: from a Node.js command prompt type `node sender.js`, and enter some text.</span></span>
3. <span data-ttu-id="f0cf1-126">Ujistěte se, že výstupem konzoly serverové aplikace je text, který jste zadali v klientské aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0cf1-126">Ensure that the server application console outputs the text that was entered in the client application.</span></span>

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

<span data-ttu-id="f0cf1-128">Blahopřejeme, vytvořili jste kompletní aplikaci Hybrid Connections pomocí Node.js!</span><span class="sxs-lookup"><span data-stu-id="f0cf1-128">Congratulations, you have created an end-to-end Hybrid Connections application using Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0cf1-129">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="f0cf1-129">Next steps:</span></span>

* [<span data-ttu-id="f0cf1-130">Přenos – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="f0cf1-130">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="f0cf1-131">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="f0cf1-131">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="f0cf1-132">Začínáme s .NET</span><span class="sxs-lookup"><span data-stu-id="f0cf1-132">Get started with .NET</span></span>](relay-hybrid-connections-dotnet-get-started.md)
* [<span data-ttu-id="f0cf1-133">Začínáme s aplikací Node</span><span class="sxs-lookup"><span data-stu-id="f0cf1-133">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

