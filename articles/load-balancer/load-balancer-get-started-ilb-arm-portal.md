---
title: "aaaCreate Vyrovnávání zatížení interní – portál Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate interní nástroj pro vyrovnávání ve Správci prostředků pomocí portálu Azure hello zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 80124217a84857b542eb41cb814ec97234176dd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-in-hello-azure-portal"></a>Vytvořit interní nástroj v hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Začínáme vytvářet interní nástroj pro vyrovnávání zatížení pomocí webu Azure Portal

Pomocí následujících kroků toocreate interní nástroj z portálu Azure hello hello.

1. Otevřete prohlížeč, přejít toohello [portál Azure](http://portal.azure.com)a přihlaste se pomocí účtu Azure.
2. V levém horním straně hello úvodní obrazovka, klikněte na **nový** > **sítě** > **nástroj pro vyrovnávání zatížení**.
3. V hello **vytvořit nástroj pro vyrovnávání zatížení** okno, zadejte **název** pro nástroj pro vyrovnávání zatížení.
4. V části **Schéma** klikněte na **Interní**.
5. Klikněte na tlačítko **virtuální síť**, a pak vyberte hello virtuální sítě, kam chcete nástroj pro vyrovnávání zatížení toocreate hello.

   > [!NOTE]
   > Pokud se chcete toouse virtuální sítě hello nezobrazí, zkontrolujte hello **umístění** používají nástroje pro vyrovnávání zatížení hello a změňte odpovídajícím způsobem.

6. Klikněte na tlačítko **podsíť**a potom vyberte místo, kam chcete nástroj pro vyrovnávání zatížení toocreate hello podsíť hello.
7. V části **přiřazení IP adresy**, klikněte na možnost **dynamické** nebo **statické**, v závislosti na tom, jestli chcete hello IP adresu pro hello zatížení vyrovnávání toobe fixed (statické) nebo ne.

   > [!NOTE]
   > Pokud vyberete toouse statickou IP adresu, bude mít tooprovide adresu pro nástroj pro vyrovnávání zatížení hello.

8. V části **skupiny prostředků** zadejte hello název nové skupiny prostředků nástroje pro vyrovnávání zatížení hello nebo klikněte na tlačítko **vyberte existující** a vyberte existující skupinu prostředků.
9. Klikněte na možnost **Vytvořit**.

## <a name="configure-load-balancing-rules"></a>Konfigurace pravidel vyrovnávání zatížení

Po hello načíst vyrovnávání vytvoření, přejděte tooconfigure prostředků nástroje pro vyrovnávání zatížení toohello ho.
Potřebujete tooconfigure nejprve fond back-end adresy a sondu před konfigurací pravidlo Vyrovnávání zatížení.

### <a name="step-1-configure-a-back-end-pool"></a>Krok 1: Konfigurace back-endového fondu

1. V hello portálu Azure, klikněte na **Procházet** > **nástroje pro vyrovnávání zatížení**a potom klikněte na nástroj pro vyrovnávání zatížení hello vytvořili výše.
2. V hello **nastavení** okně klikněte na tlačítko **back-endové fondy**.
3. V hello **back-endové fondy adres** okně klikněte na tlačítko **přidat**.
4. V hello **přidejte fond back-end** okno, zadejte **název** hello back-endového fondu a pak klikněte na tlačítko **OK**.

### <a name="step-2-configure-a-probe"></a>Krok 2: Konfigurace testu

1. V hello portálu Azure, klikněte na **Procházet** > **nástroje pro vyrovnávání zatížení**a potom klikněte na nástroj pro vyrovnávání zatížení hello vytvořili výše.
2. V hello **nastavení** okně klikněte na tlačítko **sondy**.
3. V hello **sondy** okně klikněte na tlačítko **přidat**.
4. V hello **přidat test** okno, zadejte **název** u testu paměti hello.
5. V části **Protokol** vyberte **HTTP** (pro weby) nebo **TCP** (pro ostatní aplikace založené na protokolu TCP).
6. V části **Port**, zadejte hello port toouse při přístupu k testu hello.
7. V části **cesta** (pro HTTP jenom sondy), zadejte jako sondu toouse cesta hello.
8. V části **Interval** zadejte, jak často tooprobe hello aplikace.
9. V části **prahová hodnota špatného stavu**, zadejte počet pokusů o má selhat, než je označena hello back-end virtuálního počítače není v pořádku.
10. Klikněte na tlačítko **OK** toocreate testu.

### <a name="step-3-configure-load-balancing-rules"></a>Krok 3: Konfigurace pravidel vyrovnávání zatížení

1. V hello portálu Azure, klikněte na **Procházet** > **nástroje pro vyrovnávání zatížení**a potom klikněte na nástroj pro vyrovnávání zatížení hello vytvořili výše.
2. V hello **nastavení** okně klikněte na tlačítko **pravidla Vyrovnávání zatížení**.
3. V hello **pravidla Vyrovnávání zatížení** okně klikněte na tlačítko **přidat**.
4. V hello **pravidlo Vyrovnávání zatížení přidat** okno, zadejte **název** pro pravidlo hello.
5. V části **Protokol** vyberte **HTTP** (pro weby) nebo **TCP** (pro ostatní aplikace založené na protokolu TCP).
6. V části **Port**, zadejte hello port klienti připojit nástroj pro vyrovnávání zatížení tooin hello.
7. V části **back-endový port**, zadejte toobe port hello používá ve fondu back-end hello (obvykle portů nástroje pro vyrovnávání zatížení hello a hello back-end jsou hello stejné).
8. V části **fond back-end**, vyberte fond back-end hello, kterou jste vytvořili výše.
9. V části **trvalost relace**vyberte, jak chcete toopersist relací.
10. V části **časový limit nečinnosti (v minutách)**, zadejte časový limit nečinnosti hello.
11. V části **Plovoucí IP (přímá odpověď ze serveru)** klikněte na **Zakázáno** nebo **Povoleno**.
12. Klikněte na tlačítko **OK**.

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

