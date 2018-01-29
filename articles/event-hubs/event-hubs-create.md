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
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: e444c4505d4744c95e08c4ef0d33566356785c81
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Vytvoření oboru názvů Event Hubs a centra událostí pomocí portálu Azure

## <a name="create-an-event-hubs-namespace"></a>Vytvoření oboru názvů Event Hubs
1. Přihlaste se k [portál Azure][Azure portal]a klikněte na tlačítko **vytvořit prostředek** v levém horním rohu obrazovky.
1. Klikněte na tlačítko **Internet věcí**a potom klikněte na **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. V **vytvoření oboru názvů**, zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Po kontrole, že je název oboru názvů k dispozici, zvolte cenovou úroveň (Basic nebo Standard). Zvolte také předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit. 
1. Kliknutím na **Vytvořit** vytvoříte obor názvů. Možná budete muset počkat několik minut, než systém plně zřízení prostředků.
2. V seznamu portálu oborů názvů klikněte na nově vytvořený obor názvů.
2. Klikněte na tlačítko **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. Kliknutím na tlačítko kopírovat zkopírujte připojovací řetězec **RootManageSharedAccessKey** do schránky. Uložte tento připojovací řetězec do dočasného umístění, například Poznámkový blok pro pozdější použití.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Vytvoření centra událostí

1. V seznamu oboru názvů služby Event Hubs klikněte na nově vytvořený obor názvů.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. V okně oboru názvů klikněte na **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. V horní části okna klikněte na **Přidat centrum událostí**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Zadejte název pro vaše Centrum událostí a pak klikněte na **vytvořit**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Centrum událostí je teď vytvořené a máte připojovací řetězce, které potřebujete k odesílání a příjmu událostí.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs naleznete pod těmito odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Přehled rozhraní API služby Event Hubs](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/