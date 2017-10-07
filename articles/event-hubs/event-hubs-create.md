---
title: "aaaCreate centra událostí Azure | Microsoft Docs"
description: "Vytvoření Azure Event Hubs obor názvů a centra událostí pomocí hello portálu Azure"
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
ms.openlocfilehash: 9a8b7711e2ca7d112e24be19353d43c365ff6935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-hello-azure-portal"></a><span data-ttu-id="5015c-103">Vytvoření oboru názvů Event Hubs a centra událostí pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5015c-103">Create an Event Hubs namespace and an event hub using hello Azure portal</span></span>

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="5015c-104">Vytvoření oboru názvů Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5015c-104">Create an Event Hubs namespace</span></span>
1. <span data-ttu-id="5015c-105">Přihlaste se toohello [portál Azure][Azure portal]a klikněte na tlačítko **nový** v hello levém horním rohu úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="5015c-105">Log on toohello [Azure portal][Azure portal], and click **New** at hello top left of hello screen.</span></span>
1. <span data-ttu-id="5015c-106">Klikněte na tlačítko **Internet věcí**a potom klikněte na **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="5015c-106">Click **Internet of Things**, and then click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. <span data-ttu-id="5015c-107">V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5015c-107">In hello **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="5015c-108">systém Hello toosee ověří okamžitě, pokud je k dispozici název hello.</span><span class="sxs-lookup"><span data-stu-id="5015c-108">hello system immediately checks toosee if hello name is available.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. <span data-ttu-id="5015c-109">Po provedení že název oboru názvů hello je k dispozici, zvolte hello cenová úroveň (Basic nebo Standard).</span><span class="sxs-lookup"><span data-stu-id="5015c-109">After making sure hello namespace name is available, choose hello pricing tier (Basic or Standard).</span></span> <span data-ttu-id="5015c-110">Také v který toocreate hello prostředek vyberte příslušné předplatné Azure, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="5015c-110">Also, choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> 
1. <span data-ttu-id="5015c-111">Klikněte na tlačítko **vytvořit** toocreate hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5015c-111">Click **Create** toocreate hello namespace.</span></span> <span data-ttu-id="5015c-112">Toowait může mít několik minut, než hello systémové toofully zřídit hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="5015c-112">You may have toowait a few minutes for hello system toofully provision hello resources.</span></span>
2. <span data-ttu-id="5015c-113">V seznamu portálu hello oborů názvů klikněte na tlačítko hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5015c-113">In hello portal list of namespaces, click hello newly created namespace.</span></span>
2. <span data-ttu-id="5015c-114">Klikněte na tlačítko **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="5015c-114">Click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. <span data-ttu-id="5015c-115">Klikněte na tlačítko hello kopie tlačítko toocopy hello **RootManageSharedAccessKey** schránky toohello řetězec připojení.</span><span class="sxs-lookup"><span data-stu-id="5015c-115">Click hello copy button toocopy hello **RootManageSharedAccessKey** connection string toohello clipboard.</span></span> <span data-ttu-id="5015c-116">Uložte tento připojovací řetězec do dočasného umístění, jako je například Poznámkový blok, toouse později.</span><span class="sxs-lookup"><span data-stu-id="5015c-116">Save this connection string in a temporary location, such as Notepad, toouse later.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a><span data-ttu-id="5015c-117">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="5015c-117">Create an event hub</span></span>

1. <span data-ttu-id="5015c-118">V seznamu názvů hello Event Hubs klikněte na tlačítko hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5015c-118">In hello Event Hubs namespace list, click hello newly created namespace.</span></span>      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. <span data-ttu-id="5015c-119">V okně hello obor názvů, klikněte na tlačítko **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="5015c-119">In hello namespace blade, click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. <span data-ttu-id="5015c-120">V horní části hello hello okna, klikněte na tlačítko **přidat centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="5015c-120">At hello top of hello blade, click **Add Event Hub**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. <span data-ttu-id="5015c-121">Zadejte název pro vaše Centrum událostí a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5015c-121">Type a name for your event hub, then click **Create**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub5.png)

<span data-ttu-id="5015c-122">Centrum událostí je teď vytvořené a máte připojovací řetězce hello potřebujete toosend a přijímat události.</span><span class="sxs-lookup"><span data-stu-id="5015c-122">Your event hub is now created, and you have hello connection strings you need toosend and receive events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5015c-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5015c-123">Next steps</span></span>
<span data-ttu-id="5015c-124">toolearn víc o službě Event Hubs naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="5015c-124">toolearn more about Event Hubs, visit these links:</span></span>

* [<span data-ttu-id="5015c-125">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5015c-125">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="5015c-126">Přehled rozhraní API služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5015c-126">Event Hubs API overview</span></span>](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/