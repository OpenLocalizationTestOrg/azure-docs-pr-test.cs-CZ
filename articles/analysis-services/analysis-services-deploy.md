---
title: "aaaDeploy tooAzure služby Analysis Services pomocí rozšíření SSDT | Microsoft Docs"
description: "Zjistěte, jak toodeploy tooan tabulkový model Azure Analysis Services serveru pomocí rozšíření SSDT."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: e3f3771fe32a37b9e0173c274080c647152edd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-model-from-ssdt"></a>Nasazení modelu ze sady SSDT
Po vytvoření serveru ve vašem předplatném Azure, jste připravené toodeploy tooit tabulkový model databáze. Můžete použít SQL Server Data Tools (SSDT) toobuild a nasazení projektu tabulkový model, kterou právě pracujete. 

## <a name="prerequisites"></a>Požadavky
tooget spuštění, budete potřebovat:

* **Server služby Analysis Services** v Azure Další, najdete v části toolearn [vytvoření serveru Azure Analysis Services](analysis-services-create-server.md).
* **Tabulkový model projektu** v rozšíření SSDT nebo existující tabulkový model na hello 1200 nebo vyšší úroveň kompatibility. Nikdy jste ho nevytvářeli? Zkuste hello [společnosti Adventure Works Internet prodeje tabulkové modelování kurzu](https://msdn.microsoft.com/library/hh231691.aspx).
* **Místní brána** – Pokud jeden nebo více zdrojů dat jsou místně v síti vaší organizace, měli byste tooinstall [místní brána dat](analysis-services-gateway.md). Hello brány je nutné pro váš server v cloudu hello připojení místní tooyour datového zdroje tooprocess a aktualizace dat v modelu hello.

> [!TIP]
> Před nasazením, ujistěte se, že může zpracovat hello data v tabulkách. V SSDT klikněte na **Model** > **Zpracovat** > **Zpracovat vše**. Pokud se zpracování nepodaří, nebudete moct provést úspěšné nasazení.
> 
> 

## <a name="toodeploy-a-tabular-model-from-ssdt"></a>toodeploy tabulkový model ze rozšíření SSDT

1. Než nasadíte, je třeba název serveru tooget hello. V **portál Azure** > serveru > **přehled** > **název serveru**, název serveru hello kopie.
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. V sadě SSDT > **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello > **vlastnosti**. Potom v **nasazení** > **Server** vložit název serveru hello.   
   
    ![Vložení názvu serveru do vlastnosti nasazení serveru](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. V **Průzkumníku řešení** klikněte pravým tlačítkem na **Vlastnosti** a pak klikněte na **Nasadit**. Může být výzvami toosign v tooAzure.
   
    ![Nasazení tooserver](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Stav nasazení se zobrazí v okně Výstup hello i v nasazení.
   
    ![Stav nasazení](./media/analysis-services-deploy/aas-deploy-status.png)

To je všechno je tooit!


## <a name="troubleshooting"></a>Řešení potíží
Pokud se nasazení nezdaří, při nasazování metadata, je pravděpodobné, protože rozšíření SSDT se nemohli připojit tooyour serveru. Ujistěte se, že se můžete připojit server tooyour pomocí aplikace SSMS. Potom zkontrolujte, zda je správný hello vlastnost nasazení serveru pro projekt hello.

Pokud se nasazení nezdaří v tabulce, je pravděpodobné, protože váš server se nemohl připojit zdroj dat tooa. Pokud zdroj dat je místně v síti vaší organizace, být jisti tooinstall [místní brána dat](analysis-services-gateway.md).

## <a name="next-steps"></a>Další kroky
Nyní když máte nasazené tooyour serveru tabulkový model, jste připravené tooconnect tooit. Můžete [připojit tooit pomocí SSMS](analysis-services-manage.md) toomanage ho. A můžete [připojit pomocí klienta nástroje tooit](analysis-services-connect.md) jako Power BI, Power BI Desktop, nebo aplikaci Excel a zahájení vytváření sestav.

