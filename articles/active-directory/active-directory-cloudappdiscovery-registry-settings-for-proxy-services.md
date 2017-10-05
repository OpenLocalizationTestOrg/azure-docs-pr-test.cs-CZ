---
title: "Cloud App Discovery. nastavení registru pro Proxy služby | Microsoft Docs"
description: "Cílem tohoto tématu je poskytnout kroky, které je třeba provést nastavit požadovaný port v počítačích se systémem agenta Cloud App Discovery."
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
ms.openlocfilehash: ea15dc9a9f20a296e622c8fb1011f7ee99de3e99
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery. nastavení registru pro služby Proxy
Ve výchozím nastavení agenta Cloud App Discovery konfigurován pro použití pouze porty 80 nebo 443. Pokud plánujete instalaci Cloud App Discovery v prostředí s proxy serveru, který používá vlastní port (80 ani 443), budete muset nakonfigurovat agenty na tento port použít. Konfigurace je založena na klíč registru.

Cílem tohoto tématu je poskytnout kroky, které je třeba provést nastavit požadovaný port v počítačích se systémem agenta Cloud App Discovery.

**Pokud chcete upravit portu používá počítač se spuštěným agentem Cloud App Discovery, proveďte následující kroky:**

1. Spusťte editor registru. <br> ![Spuštění](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Přejděte na nebo vytvořte následující klíč registru: <br> **Discovery\Endpoint HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikace** 
3. Vytvořte novou **víceřetězcovou** hodnotu s názvem **porty**. ![Nový](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Chcete-li otevřít **Upravit víceřetězcovou** dialogové okno, dvakrát klikněte na hodnotu porty.
5. Do textového pole hodnoty dat zadejte následující hodnoty a přidejte všechny vlastní porty, které se používají ve vaší organizaci: <br><br>
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
6. Klikněte na tlačítko **OK** zavřete **Upravit víceřetězcovou** dialogové okno.

**Další zdroje**

* [Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace](active-directory-cloudappdiscovery-whatis.md) 

