---
title: "aaaCloud aplikace zjišťování nastavení registru pro Proxy služby | Microsoft Docs"
description: "Hello cílem tohoto tématu je tooprovide vám hello kroky je třeba, aby tooperform tooset hello požadované port na počítačích hello s Cloud App Discovery agent hello."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bb1fe20016459160b4f67cb0125b1781a0260c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery. nastavení registru pro služby Proxy
Ve výchozím nastavení hello Cloud App Discovery agent je nakonfigurované toouse pouze hello porty 80 nebo 443. Pokud plánujete instalaci Cloud App Discovery v prostředí s proxy serveru, který používá vlastní port (80 ani 443), je třeba tooconfigure vaše agenty toouse tento port. Hello konfigurace je založená na klíč registru.

Hello cílem tohoto tématu je tooprovide vám hello kroky je třeba, aby tooperform tooset hello požadované port na počítačích hello s Cloud App Discovery agent hello.

**toomodify hello port používaný systémem hello počítači agenta Cloud App Discovery hello, proveďte následující kroky hello:**

1. Spusťte editor registru hello. <br> ![Spusťte](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Přejděte tooor vytvořit hello následující klíč registru: <br> **Discovery\Endpoint HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikace** 
3. Vytvořte novou **víceřetězcovou** hodnotu s názvem **porty**. ![Nový](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. tooopen hello **Upravit víceřetězcovou** dialogové okno, dvakrát klikněte na hodnotu porty hello.
5. V hello textové pole hodnoty dat zadejte následující hodnoty hello a přidejte všechny vlastní porty, které se používají ve vaší organizaci: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Upravit víceřetězcovou](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Klikněte na tlačítko **OK** tooclose hello **Upravit víceřetězcovou** dialogové okno.

**Další zdroje**

* [Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace](active-directory-cloudappdiscovery-whatis.md) 

