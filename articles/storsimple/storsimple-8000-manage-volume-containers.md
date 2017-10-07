---
title: "aaaManage vaše kontejnery svazků zařízení StorSimple na zařízení řady StorSimple 8000 hello | Microsoft Docs"
description: "Vysvětluje, jak je možné používat hello Správce zařízení StorSimple kontejnery svazků služby stránky tooadd, úpravě nebo odstranění kontejner svazků."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 7374d4ab9aecd6280ae1d93a29f17d12d28c9362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-volume-containers"></a>Použití služby StorSimple Manager zařízení hello, toomanage kontejnery svazků zařízení StorSimple

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a spravovat kontejnery svazků zařízení StorSimple.

Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma. Zařízení může mít několik kontejnery svazků pro všechny jeho svazky. 

Kontejner svazků obsahuje hello následující atributy:

* **Svazky** – hello vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků hello. 
* **Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků. Tento klíč se používá pro šifrování dat hello, který se odesílá z vašeho cloudu toohello zařízení StorSimple. Klíč vojenských úrovni AES 256 bitů se používá s klíčem hello zadanou uživatelem. toosecure daty, doporučujeme vždy povolit šifrování úložiště cloudu.
* **Účet úložiště** – hello účtu úložiště Azure, který je použité toostore hello data. Všechny svazky hello umístěných v kontejneru svazků sdílet tento účet úložiště. Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytvoření kontejneru svazků hello a pak zadejte přihlašovací údaje pro přístup k hello k tomuto účtu.
* **Cloud šířky pásma** – hello šířky pásma spotřebovávají hello zařízení při hello data ze zařízení hello je odesílána toohello cloudu. Řízení šířky pásma můžete vynutit tak, že zadáte hodnotu v rozmezí 1 MB/s až 1 000 MB/s, při vytváření tohoto kontejneru. Pokud chcete zařízení tooconsume hello celou dostupnou šířku pásma, nastavte toto pole příliš**neomezený**. Můžete také vytvořit a použít šířky pásma šířky pásma šablony tooallocate podle plánu.

Hello následující postupy popisují, jak toouse hello StorSimple **kontejnery svazků** hello toocomplete okno následující běžné operace:

* Přidat kontejner svazků
* Upravit kontejneru svazků
* Odstranit kontejner svazků

## <a name="add-a-volume-container"></a>Přidat kontejner svazků
Proveďte následující kroky tooadd kontejner svazků hello.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Upravit kontejneru svazků
Proveďte následující kroky toomodify kontejner svazků hello.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Odstranit kontejner svazků
Kontejner svazků obsahuje svazky v něm. Lze odstranit pouze v případě, že všechny hello svazky, které v ní jsou nejprve odstranit. Proveďte následující kroky toodelete kontejner svazků hello.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Další kroky
* Další informace o [správu svazků zařízení StorSimple](storsimple-8000-manage-volumes-u2.md). 
* Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

