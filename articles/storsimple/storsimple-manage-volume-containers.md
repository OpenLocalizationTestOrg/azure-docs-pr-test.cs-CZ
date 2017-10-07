---
title: "aaaManage vaše kontejnery svazků zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak je možné používat hello StorSimple Manager kontejnery svazků služby stránky tooadd, úpravě nebo odstranění kontejner svazků."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 9b29536e0072306e53ac92bacca78a13d932c2b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-volume-containers"></a>Použití služby StorSimple Manager hello, toomanage kontejnery svazků zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak toouse hello toocreate služby StorSimple Manager a spravovat kontejnery svazků zařízení StorSimple.

Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma. Zařízení může mít několik kontejnery svazků pro všechny jeho svazky. 

Kontejner svazků obsahuje hello následující atributy:

* **Svazky** – hello vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků hello. Kontejner svazků může obsahovat až too256 svazky zařízení StorSimple.
* **Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků. Tento klíč se používá pro šifrování dat hello, který se odesílá z vašeho cloudu toohello zařízení StorSimple. Klíč vojenských úrovni AES 256 bitů se používá s klíčem hello zadanou uživatelem. toosecure daty, doporučujeme vždy povolit šifrování úložiště cloudu.
* **Účet úložiště** – hello účet úložiště, který je poskytovatele propojené tooyour cloudové úložiště služby. Všechny svazky hello umístěných v kontejneru svazků sdílet tento účet úložiště. Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytvoření kontejneru svazků hello a pak zadejte přihlašovací údaje pro přístup k hello k tomuto účtu.
* **Cloud šířky pásma** – hello šířky pásma spotřebovávají hello zařízení při hello data ze zařízení hello je odesílána toohello cloudu. Zadejte hodnotu mezi 1 a 1000 Mb/s při definování tento kontejner může vynutit řízení šířky pásma. Pokud chcete zařízení tooconsume hello celou dostupnou šířku pásma, nastavte tuto tooUnlimited pole. Můžete také vytvořit a použít šířky pásma šířky pásma šablony tooallocate podle plánu.

![Stránka kontejnery svazku](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Tato následující postupy popisují, jak toouse hello StorSimple **kontejnery svazků** stránku hello toocomplete následující běžné operace:

* Přidat kontejner svazků 
* Upravit kontejneru svazků 
* Odstranit kontejner svazků 

## <a name="add-a-volume-container"></a>Přidat kontejner svazků
Proveďte následující kroky tooadd kontejner svazků hello.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Upravit kontejneru svazků
Proveďte následující kroky toomodify kontejner svazků hello.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Odstranit kontejner svazků
Kontejner svazků obsahuje svazky v něm. Lze odstranit pouze v případě, že všechny hello svazky, které v ní jsou nejprve odstranit. Proveďte následující kroky toodelete kontejner svazků hello.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Další kroky
* Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md). 
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

