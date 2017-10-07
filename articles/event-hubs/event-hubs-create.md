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
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-hello-azure-portal"></a>Vytvoření oboru názvů Event Hubs a centra událostí pomocí hello portálu Azure

## <a name="create-an-event-hubs-namespace"></a>Vytvoření oboru názvů Event Hubs
1. Přihlaste se toohello [portál Azure][Azure portal]a klikněte na tlačítko **nový** v hello levém horním rohu úvodní obrazovka.
1. Klikněte na tlačítko **Internet věcí**a potom klikněte na **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů. systém Hello toosee ověří okamžitě, pokud je k dispozici název hello.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Po provedení že název oboru názvů hello je k dispozici, zvolte hello cenová úroveň (Basic nebo Standard). Také v který toocreate hello prostředek vyberte příslušné předplatné Azure, skupinu prostředků a umístění. 
1. Klikněte na tlačítko **vytvořit** toocreate hello oboru názvů. Toowait může mít několik minut, než hello systémové toofully zřídit hello prostředky.
2. V seznamu portálu hello oborů názvů klikněte na tlačítko hello nově vytvořený obor názvů.
2. Klikněte na tlačítko **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. Klikněte na tlačítko hello kopie tlačítko toocopy hello **RootManageSharedAccessKey** schránky toohello řetězec připojení. Uložte tento připojovací řetězec do dočasného umístění, jako je například Poznámkový blok, toouse později.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Vytvoření centra událostí

1. V seznamu názvů hello Event Hubs klikněte na tlačítko hello nově vytvořený obor názvů.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. V okně hello obor názvů, klikněte na tlačítko **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. V horní části hello hello okna, klikněte na tlačítko **přidat centra událostí**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Zadejte název pro vaše Centrum událostí a pak klikněte na **vytvořit**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Centrum událostí je teď vytvořené a máte připojovací řetězce hello potřebujete toosend a přijímat události.

## <a name="next-steps"></a>Další kroky
toolearn víc o službě Event Hubs naleznete pod těmito odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Přehled rozhraní API služby Event Hubs](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/