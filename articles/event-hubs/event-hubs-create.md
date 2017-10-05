---
title: "Vytvoření centra událostí Azure | Microsoft Docs"
description: "Vytvoření Azure Event Hubs obor názvů a centra událostí pomocí portálu Azure"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: sethm
ms.openlocfilehash: 816bf1426704d3391550e80c0700f1b011683a94
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a><span data-ttu-id="1c894-103">Vytvoření oboru názvů Event Hubs a centra událostí pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1c894-103">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="1c894-104">Vytvoření oboru názvů Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1c894-104">Create an Event Hubs namespace</span></span>
1. <span data-ttu-id="1c894-105">Přihlaste se na web [Azure Portal][Azure portal] a v levém horním rohu obrazovky klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="1c894-105">Log on to the [Azure portal][Azure portal], and click **New** at the top left of the screen.</span></span>
1. <span data-ttu-id="1c894-106">Klikněte na tlačítko **Internet věcí**a potom klikněte na **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="1c894-106">Click **Internet of Things**, and then click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. <span data-ttu-id="1c894-107">V okně **Vytvořit obor názvů** zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="1c894-107">In the **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="1c894-108">Systém okamžitě kontroluje, jestli je název dostupný.</span><span class="sxs-lookup"><span data-stu-id="1c894-108">The system immediately checks to see if the name is available.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. <span data-ttu-id="1c894-109">Po kontrole, že je název oboru názvů k dispozici, zvolte cenovou úroveň (Basic nebo Standard).</span><span class="sxs-lookup"><span data-stu-id="1c894-109">After making sure the namespace name is available, choose the pricing tier (Basic or Standard).</span></span> <span data-ttu-id="1c894-110">Zvolte také předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit.</span><span class="sxs-lookup"><span data-stu-id="1c894-110">Also, choose an Azure subscription, resource group, and location in which to create the resource.</span></span> 
1. <span data-ttu-id="1c894-111">Kliknutím na **Vytvořit** vytvoříte obor názvů.</span><span class="sxs-lookup"><span data-stu-id="1c894-111">Click **Create** to create the namespace.</span></span> <span data-ttu-id="1c894-112">Možná budete muset počkat několik minut, než systém plně zřízení prostředků.</span><span class="sxs-lookup"><span data-stu-id="1c894-112">You may have to wait a few minutes for the system to fully provision the resources.</span></span>
2. <span data-ttu-id="1c894-113">V seznamu portálu oborů názvů klikněte na nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="1c894-113">In the portal list of namespaces, click the newly created namespace.</span></span>
2. <span data-ttu-id="1c894-114">Klikněte na tlačítko **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="1c894-114">Click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. <span data-ttu-id="1c894-115">Kliknutím na tlačítko kopírovat zkopírujte připojovací řetězec **RootManageSharedAccessKey** do schránky.</span><span class="sxs-lookup"><span data-stu-id="1c894-115">Click the copy button to copy the **RootManageSharedAccessKey** connection string to the clipboard.</span></span> <span data-ttu-id="1c894-116">Uložte tento připojovací řetězec do dočasného umístění, například Poznámkový blok pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="1c894-116">Save this connection string in a temporary location, such as Notepad, to use later.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a><span data-ttu-id="1c894-117">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="1c894-117">Create an event hub</span></span>

1. <span data-ttu-id="1c894-118">V seznamu oboru názvů služby Event Hubs klikněte na nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="1c894-118">In the Event Hubs namespace list, click the newly created namespace.</span></span>      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. <span data-ttu-id="1c894-119">V okně oboru názvů klikněte na **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="1c894-119">In the namespace blade, click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. <span data-ttu-id="1c894-120">V horní části okna klikněte na **Přidat centrum událostí**.</span><span class="sxs-lookup"><span data-stu-id="1c894-120">At the top of the blade, click **Add Event Hub**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. <span data-ttu-id="1c894-121">Zadejte název pro vaše Centrum událostí a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1c894-121">Type a name for your event hub, then click **Create**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub5.png)

<span data-ttu-id="1c894-122">Centrum událostí je teď vytvořené a máte připojovací řetězce, které potřebujete k odesílání a příjmu událostí.</span><span class="sxs-lookup"><span data-stu-id="1c894-122">Your event hub is now created, and you have the connection strings you need to send and receive events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c894-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c894-123">Next steps</span></span>
<span data-ttu-id="1c894-124">Další informace o službě Event Hubs naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="1c894-124">To learn more about Event Hubs, visit these links:</span></span>

* [<span data-ttu-id="1c894-125">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1c894-125">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="1c894-126">Přehled rozhraní API služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1c894-126">Event Hubs API overview</span></span>](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/