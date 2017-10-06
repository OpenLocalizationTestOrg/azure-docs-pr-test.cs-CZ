---
title: "aaaCreate serveru služby Analysis Services v Azure | Microsoft Docs"
description: "Zjistěte, jak instance toocreate serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3668f659039f79f3dd71498d1066e8682bf33228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a>Vytvoření serveru Azure Analysis Services na portálu Azure
Tento článek vás provede procesem vytvoření prostředek serveru služby Analysis Services ve vašem předplatném Azure.

## <a name="before-you-begin"></a>Než začnete
toocomplete tento rychlý start, budete potřebovat:

* **Předplatné Azure**: navštivte [bezplatná zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate účet.
* **Azure Active Directory**: vaše předplatné musí být přidružen klienta služby Azure Active Directory. A je třeba toobe přihlášení tooAzure s účtem v této službě Azure Active Directory. Účty Microsoft nejsou podporovány. Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).
* **Skupina prostředků**: už máte skupinu prostředků nebo [vytvořte novou](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba. Další, najdete v části toolearn [ceny služby Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).
> 
> 

## <a name="toocreate-a-server-in-azure-portal"></a>toocreate serveru na portálu Azure
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).  
2. Klikněte na tlačítko **+ nový** > **Data + analýzy** > **služby Analysis Services**.
3. V hello **služby Analysis Services** okně vyplňte hello požadované pole a stiskněte klávesu **vytvořit**.
   
    ![Vytvoření serveru](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Název serveru**: Zadejte jedinečný název používá tooreference hello serveru.
   * **Předplatné**: Vyberte předplatné hello bills tento server k.
   * **Skupina prostředků**: Tyto kontejnery jsou navrženou toohelp správu kolekcí prostředků Azure. Další, najdete v části toolearn [skupiny prostředků](../azure-resource-manager/resource-group-overview.md).
   * **Umístění**: Tento Azure datacenter umístění hostitele hello serveru. Vyberte umístění pro nejbližší největší uživatelskou základnu.
   * **Cenová úroveň**: Vyberte cenovou úroveň. Jsou podporovány tabulkové modely až too400 GB. Další, najdete v části toolearn [ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).
4. Klikněte na možnost **Vytvořit**.

Vytvoření trvá obvykle pod minutu; často jenom pár sekund. Pokud jste vybrali **přidat tooPortal**, přejděte portálu toosee tooyour nový server. Nebo přejděte příliš**další služby** > **služby Analysis Services** toosee Pokud váš server je připraven.

 ![Řídicí panel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Další kroky
Po vytvoření serveru, můžete [nasadit model](analysis-services-deploy.md) tooit pomocí rozšíření SSDT nebo pomocí SSMS.

Pokud model nasazení serveru tooyour připojí tooon místní zdroje dat, je třeba tooinstall [místní brána dat](analysis-services-gateway.md) na počítači v síti.

