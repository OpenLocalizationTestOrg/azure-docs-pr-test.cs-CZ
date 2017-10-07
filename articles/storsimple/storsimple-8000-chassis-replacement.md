---
title: "Skříň aaaReplace na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak tooremove a nahradit text hello skříň pro StorSimple primární skříň nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 94bbd3d354a9b8866ece036238927e67ec5ce2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-chassis-on-your-storsimple-device"></a>Nahraďte hello skříň zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak tooremove a nahraďte skříň v zařízení řady StorSimple 8000. Hello StorSimple 8100 modelu je zařízení jeden skříň (jednom šasi), zatímco hello 8600 je zařízení duální skříň (dvě chassis). Pro 8600 model jsou potenciálně dvě skříň, které by mohlo selhat v zařízení hello: hello skříň pro primární skříň hello nebo hello skříň pro hello EBOD skříň.

V obou případech skříň hello nahrazení, který je dodáván společností Microsoft je prázdný. Žádné napájení a chlazení moduly (PCMs), moduly řadič diskové jednotky SSD (Solid-State Drive), pevných disků (HDD) nebo EBOD moduly budou zahrnuty.

> [!IMPORTANT]
> Před odebírání a nahrazování hello skříň, zkontrolujte informace o zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-hello-chassis"></a>Odebrat hello skříň
Proveďte následující kroky tooremove hello skříň zařízení StorSimple hello.

#### <a name="tooremove-a-chassis"></a>tooremove skříň
1. Zajistěte, aby toto zařízení StorSimple hello je vypnout a odpojit od všech zdrojů energie hello.
2. Odeberte všechny síťové hello a SAS kabely, pokud je k dispozici.
3. Odeberte hello jednotku ze hello rack.
4. Odeberte všechny jednotky hello a poznamenejte si hello sloty, ze kterých budou odstraněny. Další informace najdete v tématu [odebrat hello disku](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).
5. Na hello EBOD skříň (Pokud se jedná hello skříň, do které se nezdařilo) odeberte hello EBOD řadiče moduly. Další informace najdete v tématu [odebrat řadič EBOD](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).
   
    Na hello primární skříň (Pokud se jedná hello skříň, do které se nezdařilo), odeberte hello řadiče a poznamenejte si hello sloty, ze kterých budou odstraněny. Další informace najdete v tématu [odebrání řadiče](storsimple-8000-controller-replacement.md#remove-a-controller).

## <a name="install-hello-chassis"></a>Nainstalujte hello skříň
Proveďte následující kroky tooinstall hello skříň zařízení StorSimple hello.

#### <a name="tooinstall-a-chassis"></a>tooinstall skříň
1. Připojte hello skříň v racku hello. Další informace najdete v tématu [Rack připojení vaší 8100 StorSimple zařízení](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) nebo [zařízení 8600 vaší StorSimple Rack připojení](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Po hello skříň namontovat do racku hello, nainstalujte v hello stejné umisťuje, zda byly dříve nainstalovány v hello řadiče moduly.
3. Instalace hello jednotek v hello stejné umisťuje a přihrádek, zda byly dříve nainstalovány v.
   
   > [!NOTE]
   > Doporučujeme nejprve nainstalovat hello SSD v hello sloty a pak nainstalujte HDD hello.
  
4. S hello zařízení namontováno v racku hello a hello komponenty nainstalované, připojit vaše zařízení toohello odpovídající power zdroje a zapnout hello zařízení. Podrobnosti najdete v tématu [zapojení kabeláže zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [zapojení kabeláže zařízení StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).

