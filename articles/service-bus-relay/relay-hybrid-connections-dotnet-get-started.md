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
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="afa8f-103">Začínáme s hybridními připojeními pro přenos</span><span class="sxs-lookup"><span data-stu-id="afa8f-103">Get started with Relay Hybrid Connections</span></span>
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="afa8f-104">Tento kurz obsahuje úvod příliš[Azure předávání hybridní připojení](relay-what-is-it.md#hybrid-connections)a ukazuje, jak toouse .NET toocreate klientskou aplikaci, která odesílá zprávy tooa odpovídající naslouchací proces aplikace.</span><span class="sxs-lookup"><span data-stu-id="afa8f-104">This tutorial provides an introduction too[Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how toouse .NET toocreate a client application that sends messages tooa corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="afa8f-105">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="afa8f-105">What will be accomplished</span></span>
<span data-ttu-id="afa8f-106">Protože hybridní připojení vyžaduje klienta a součásti serveru, hello kurzu vytvoří dvě aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="afa8f-106">Because Hybrid Connections requires both a client and a server component, hello tutorial creates two console applications.</span></span> <span data-ttu-id="afa8f-107">Zde jsou kroky hello:</span><span class="sxs-lookup"><span data-stu-id="afa8f-107">Here are hello steps:</span></span>

1. <span data-ttu-id="afa8f-108">Vytvořte obor názvů předávání pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="afa8f-108">Create a Relay namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="afa8f-109">V tomto oboru názvů pomocí hello portálu Azure vytvořte hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="afa8f-109">Create a hybrid connection in that namespace, using hello Azure portal.</span></span>
3. <span data-ttu-id="afa8f-110">Zápis konzoly serveru (naslouchání) aplikace tooreceive zpráv.</span><span class="sxs-lookup"><span data-stu-id="afa8f-110">Write a server (listener) console application tooreceive messages.</span></span>
4. <span data-ttu-id="afa8f-111">Zápis konzoly klienta (odesílatel) aplikace toosend zpráv.</span><span class="sxs-lookup"><span data-stu-id="afa8f-111">Write a client (sender) console application toosend messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="afa8f-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="afa8f-112">Prerequisites</span></span>

<span data-ttu-id="afa8f-113">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="afa8f-113">toocomplete this tutorial, you'll need hello following prerequisites:</span></span>

1. <span data-ttu-id="afa8f-114">[Visual Studio 2015 nebo vyšší](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="afa8f-114">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="afa8f-115">Hello příklady v tomto kurzu použít Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="afa8f-115">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="afa8f-116">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="afa8f-116">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="afa8f-117">1. Vytvoření oboru názvů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="afa8f-117">1. Create a namespace using hello Azure portal</span></span>
<span data-ttu-id="afa8f-118">Pokud jste již vytvořili předávání názvů, přeskočit toohello [vytvořit hybridní připojení pomocí portálu Azure hello](#2-create-a-hybrid-connection-using-the-azure-portal) části.</span><span class="sxs-lookup"><span data-stu-id="afa8f-118">If you have already created a Relay namespace, jump toohello [Create a hybrid connection using hello Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a><span data-ttu-id="afa8f-119">2. Vytvořit hybridní připojení pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="afa8f-119">2. Create a hybrid connection using hello Azure portal</span></span>
<span data-ttu-id="afa8f-120">Pokud jste již vytvořili hybridní připojení, přeskočit toohello [vytvořit aplikaci typu server](#3-create-a-server-application-listener) části.</span><span class="sxs-lookup"><span data-stu-id="afa8f-120">If you have already created a hybrid connection, jump toohello [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="afa8f-121">3. Vytvoření serverové aplikace (naslouchací proces)</span><span class="sxs-lookup"><span data-stu-id="afa8f-121">3. Create a server application (listener)</span></span>
<span data-ttu-id="afa8f-122">toolisten a přijímat zprávy z hello předávání, jsme zapíše konzolovou aplikaci C# pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afa8f-122">toolisten and receive messages from hello Relay, we will write a C# console application using Visual Studio.</span></span>

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="afa8f-123">4. Vytvoření klientské aplikace (odesílatel)</span><span class="sxs-lookup"><span data-stu-id="afa8f-123">4. Create a client application (sender)</span></span>
<span data-ttu-id="afa8f-124">Předávání, toosend zprávy toohello jsme zapíše konzolovou aplikaci C# pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afa8f-124">toosend messages toohello Relay, we will write a C# console application using Visual Studio.</span></span>

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-hello-applications"></a><span data-ttu-id="afa8f-125">5. Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="afa8f-125">5. Run hello applications</span></span>
1. <span data-ttu-id="afa8f-126">Spusťte aplikaci server hello.</span><span class="sxs-lookup"><span data-stu-id="afa8f-126">Run hello server application.</span></span>
2. <span data-ttu-id="afa8f-127">Spuštění klienta aplikace hello a zadejte nějaký text.</span><span class="sxs-lookup"><span data-stu-id="afa8f-127">Run hello client application and enter some text.</span></span>
3. <span data-ttu-id="afa8f-128">Zkontrolujte, že server hello aplikace konzoly výstupy hello text, který byl zadán v aplikaci klienta hello.</span><span class="sxs-lookup"><span data-stu-id="afa8f-128">Ensure that hello server application console outputs hello text that was entered in hello client application.</span></span>

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

<span data-ttu-id="afa8f-130">Blahopřejeme, vytvořili jste kompletní aplikaci Hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="afa8f-130">Congratulations, you have created an end-to-end Hybrid Connections application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="afa8f-131">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="afa8f-131">Next steps:</span></span>
* [<span data-ttu-id="afa8f-132">Přenos – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="afa8f-132">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="afa8f-133">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="afa8f-133">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="afa8f-134">Začínáme s aplikací Node</span><span class="sxs-lookup"><span data-stu-id="afa8f-134">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

