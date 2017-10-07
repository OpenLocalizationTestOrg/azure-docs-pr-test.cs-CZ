---
title: "aaaCreate privátní registru Docker - portálu Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello portálu Azure"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40f3ce44fea26e5fbeca865c9da6df55c2df9511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-portal"></a>Vytvořit privátní registru kontejner Docker pomocí hello portálu Azure
Použijte hello Azure portálu toocreate kontejneru registru a jeho nastavení. Můžete také vytvořit a spravovat pomocí hello registrech kontejneru [příkazy Azure CLI 2.0](container-registry-get-started-azure-cli.md), [prostředí Azure PowerShell](container-registry-get-started-powershell.md) nebo prostřednictvím kódu programu hello kontejneru registru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).

Pozadí a koncepty najdete v tématu [hello přehled](container-registry-intro.md).

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
1. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **+ nový**.
2. Hledání hello marketplace pro **kontejner Azure registru**.
3. Vyberte **Azure Container Registry** s vydavatelem **Microsoft**.
    ![Služba Container Registry v Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)
4. Klikněte na možnost **Vytvořit**. Hello **registru kontejner Azure** otevře se okno.

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/container-registry-settings.png)
5. V hello **registru kontejner Azure** okno, zadejte následující informace hello. Až budete hotovi, klikněte na **Vytvořit**.

    a. **Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru. V tomto příkladu je název registru hello *myRegistry01*, ale nahraďte vlastní jedinečný název. Hello název může obsahovat jenom písmena a číslice.

    b. **Skupina prostředků**: Vyberte existující [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové.

    c. **Umístění**: Vyberte datové centrum Azure umístění, kde je služba hello [k dispozici](https://azure.microsoft.com/regions/services/), jako například **jihu USA**.

    d. **Uživatel s oprávněními správce**: Pokud chcete, povolení registru hello tooaccess uživatele správce. Toto nastavení po vytvoření hello registru můžete změnit.

      > [!IMPORTANT]
      > Kromě toho tooproviding přístup prostřednictvím uživatelský účet správce, kontejner registrech podporu ověřování založenou na objekty služby Azure Active Directory. Další informace a aspekty najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md).
      >

    e. **Účet úložiště**: použijte hello výchozí nastavení toocreate [účet úložiště](../storage/common/storage-introduction.md), nebo vybrat existující účet úložiště v hello stejné umístění. Storage úrovně Premium se v tuto chvíli nepodporuje.

## <a name="manage-registry-settings"></a>Správa nastavení registru
Po vytvoření hello registru, najde hello nastavení registru spuštěním v hello **kontejneru registrech** okna portálu hello. Například může být nutné hello toolog nastavení v registru tooyour, nebo může být vhodné tooenable nebo zakázat hello uživatel s oprávněními správce.

1. Na hello **kontejneru registrech** okně klikněte na název hello registru systému Windows.

    ![Okno registru kontejneru](./media/container-registry-get-started-portal/container-registry-blade.png)
2. Klikněte na tlačítko Nastavení přístupu k toomanage, **přístupový klíč**.

    ![Přístup k registru kontejneru](./media/container-registry-get-started-portal/container-registry-access.png)
3. Všimněte si hello následující nastavení:

   * **Přihlášení na server** -hello použijete toolog v registru toohello plně kvalifikovaný název. V tomto příkladu je to `myregistry01.azurecr.io`.
   * **Uživatel s oprávněními správce** – přepnutí tooenable nebo zakázat hello registru Správce uživatelský účet.
   * **Uživatelské jméno** a **heslo** -hello přihlašovací údaje účtu uživatele správce hello (Pokud je povoleno) můžete použít toolog v toohello registru. Volitelně můžete obnovit hesla hello. Dvě hesla jsou vytvořeny tak, aby můžete udržovat připojení toohello registru pomocí jedno heslo, zatímco si znovu vygenerujete hello jiné heslo. Místo toho najdete v části tooauthenticate pomocí objektu služby [ověřit s privátní registru kontejner Docker](container-registry-authentication.md).

## <a name="next-steps"></a>Další kroky
* [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md)
