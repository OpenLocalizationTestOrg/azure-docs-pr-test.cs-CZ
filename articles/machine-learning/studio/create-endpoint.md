---
title: "Vytváření koncových bodů webové služby v Machine Learning | Microsoft Docs"
description: "Vytváření koncových bodů webové služby v Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 6de83042779a1a4edae57499f108dcddc9d68309
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="creating-endpoints"></a>Vytváření koncových bodů
> [!NOTE]
>  Toto téma popisuje techniky pro **Classic** Machine Learning webové služby.
> 
> 

Při vytváření webové služby, které prodeje dál zákazníkům, je třeba zadat trénované modely u každého zákazníka, které jsou stále spojeny s experiment, ze kterého byl vytvořen webovou službu. Kromě toho všechny aktualizace experimentu bude použito selektivní pro koncový bod bez přepsal přizpůsobení.

K tomu, Azure Machine Learning můžete vytvořit několik koncových bodů pro nasazenou webovou službu. Každý koncový bod webové služby je nezávisle řešit, omezení a spravovat. Každý koncový bod je jedinečnou adresu URL a autorizační klíč, které můžete distribuovat zákazníkům.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Přidání koncových bodů webové služby
Existují dva způsoby, jak přidat koncový bod webové služby.

* Prostřednictvím kódu programu
* Prostřednictvím portálu Azure Machine Learning webové služby

Po vytvoření koncového bodu, můžete využívat prostřednictvím synchronní rozhraní API, batch rozhraní API a listy aplikace excel. Kromě přidání koncových bodů prostřednictvím tohoto uživatelského rozhraní, můžete také použít rozhraní API pro správu koncový bod programově přidat koncové body.

> [!NOTE]
> Pokud jste přidali další koncové body k webové službě, nelze odstranit výchozí koncový bod.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Přidání koncového bodu prostřednictvím kódu programu
Koncový bod můžete přidat k webové službě programově pomocí [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Přidání koncového bodu pomocí portálu Azure Machine Learning webové služby
1. Machine Learning Studio v levém navigačním sloupec, klikněte na možnost webové služby.
2. V dolní části řídicího panelu webové služby, klikněte na tlačítko **umožňuje spravovat koncové body**. Webové služby Azure Machine Learning portál otevře stránku koncové body pro webovou službu.
3. Klikněte na možnost **Nové**.
4. Zadejte název a popis pro nový koncový bod. Názvy koncových bodů musí být 24 znaky nebo méně délku a musí být tvořen malá písmena nebo číslice. Vyberte úroveň protokolování a určuje, jestli je povolená ukázková data. Další informace o protokolování naleznete v tématu [povolení protokolování pro Machine Learning webové služby](web-services-logging.md).

## <a name="next-steps"></a>Další kroky
[Jak používat Azure Machine Learning webové služby](consume-web-services.md).

