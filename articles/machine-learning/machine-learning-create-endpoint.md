---
title: "koncových bodů webové služby aaaCreating v Machine Learning | Microsoft Docs"
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
ms.openlocfilehash: 10a2bc586c6fe35e28d8bf0293854c578827c453
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-endpoints"></a>Vytváření koncových bodů
> [!NOTE]
>  Toto téma popisuje techniky použít tooa **Classic** Machine Learning webové služby.
> 
> 

Při vytváření webové služby, které prodeje dopředného tooyour zákazníků, je nutné tooprovide trénované modely tooeach zákazníka, které jsou stále propojené toohello experiment, ze které hello webové služby byl vytvořen. Kromě toho aktualizace toohello experiment by měla být použita selektivně tooan koncový bod bez přepsal hello přizpůsobení.

tooaccomplish, Azure Machine Learning vám umožní toocreate několik koncových bodů pro nasazenou webovou službu. Každý koncový bod v hello webové služby je nezávisle řešit, omezení a spravovat. Každý koncový bod je jedinečný klíč adresy URL a autorizace, které můžete distribuovat tooyour zákazníků.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-tooa-web-service"></a>Přidání koncové body tooa webové služby
Existují tři způsoby tooadd tooa koncový bod webové služby.

* Prostřednictvím kódu programu
* Prostřednictvím portálu webové služby Azure Machine Learning hello
* Přestože hello portál Azure classic

Jakmile je vytvořen koncový bod hello, můžete využívat prostřednictvím synchronní rozhraní API, batch rozhraní API a listy aplikace excel. Kromě toho tooadding koncových bodů prostřednictvím tohoto uživatelského rozhraní, můžete použít také hello koncový bod rozhraní API pro správu tooprogrammatically přidat koncové body.

> [!NOTE]
> Pokud jste přidali další koncové body toohello webové služby, nelze odstranit hello výchozí koncový bod.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Přidání koncového bodu prostřednictvím kódu programu
Můžete přidat koncový bod tooyour webové služby programově pomocí hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.

## <a name="adding-an-endpoint-using-hello-azure-machine-learning-web-services-portal"></a>Přidání koncový bod webové služby Azure Machine Learning portálu hello
1. Machine Learning Studio v levém navigačním sloupec hello, klikněte na možnost webové služby.
2. V dolní části hello řídicího panelu, hello webové služby, klikněte na tlačítko **umožňuje spravovat koncové body**. Hello webové služby Azure Machine Learning portál otevře toohello koncové body stránku hello webové služby.
3. Klikněte na možnost **Nové**.
4. Zadejte název a popis pro nový koncový bod hello. Názvy koncových bodů musí být 24 znaky nebo méně délku a musí být tvořen malá písmena nebo číslice. Vyberte úroveň protokolování hello a určuje, jestli je povolená ukázková data. Další informace o protokolování naleznete v tématu [povolení protokolování pro Machine Learning webové služby](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-hello-azure-classic-portal"></a>Přidání koncového bodu pomocí hello portál Azure classic
1. Přihlaste se toohello [portál Azure classic](http://manage.windowsazure.com), klikněte na tlačítko **Machine Learning** v levém sloupci hello. Klikněte na tlačítko hello pracovní prostor, který obsahuje hello webová služba, která vás zajímá.
   
    ![Přejděte tooworkspace](./media/machine-learning-create-endpoint/figure-1.png)
2. Klikněte na tlačítko **webové služby**.
   
    ![Přejděte tooWeb služby](./media/machine-learning-create-endpoint/figure-2.png)
3. Klikněte na tlačítko hello webové služby, co vás zajímá toosee hello seznam dostupných koncových bodů.
   
    ![Přejděte tooendpoint](./media/machine-learning-create-endpoint/figure-3.png)
4. V dolní části hello hello stránky, klikněte na tlačítko **přidat koncový bod**. Zadejte název a popis, ujistěte se, že neexistují žádné koncové body s hello stejný název v této webové služby. Pokud máte speciální požadavky, ponechte hello omezení úroveň s jeho výchozí hodnotu. toolearn Další informace o omezování, najdete v části [škálování koncových bodů API](machine-learning-scaling-webservice.md).
   
    ![Vytvoření koncového bodu](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Další kroky
[Jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

