---
title: "aaaExtend experimentů pomocí R | Microsoft Docs"
description: "Jak tooextend hello funkce Azure Machine Learning Studio prostřednictvím jazyka hello R pomocí modulu spustit skript jazyka R hello."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 396489f26f367a744922af65e04f056c7afa1399
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extend-your-experiment-with-r"></a>Rozšíření experimentu pomocí R
Funkce hello nástroje Azure Machine Learning Studio prostřednictvím jazyka hello R můžete rozšířit pomocí hello [spustit skript jazyka R] [ execute-r-script] modulu.

Tento modul přijímá více vstupní datové sady a výsledkem jednu datovou sadu jako výstup. Skript jazyka R je možné zadat do hello **skript jazyka R** parametr hello [spustit skript jazyka R] [ execute-r-script] modulu.

Máte přístup k každý vstupní port hello modulu pomocí podobné toohello následující kód:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Seznam aktuálně nainstalovaných balíčků
Hello seznam balíčků nainstalovaných můžete změnit. Seznam aktuálně nainstalovaných balíčků naleznete v [R balíčky nepodporuje Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Také můžete získat hello dokončení, aktuální seznam balíčků nainstalovaných zadáním hello následující kód do hello [spustit skript jazyka R] [ execute-r-script] modul:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Tím se odešle hello seznam balíčků toohello výstupní port modulu hello [spustit skript jazyka R] [ execute-r-script] modulu.
balíček hello tooview seznamu, jako například připojit modul převod [převést tooCSV] [ convert-to-csv] toohello zbývajících výstup hello [spustit skript jazyka R] [ execute-r-script]modul, spustit hello experiment, potom klikněte na výstup hello hello převod modul a vyberte **Stáhnout**. 

![Stažení výstupu "Převést tooCSV" modulu](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is hello [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Import balíčků
Můžete importovat balíčky, které již nejsou nainstalovány pomocí následujících příkazů v hello hello [spustit skript jazyka R] [ execute-r-script] modul:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

kde hello `my_favorite_package.zip` soubor obsahuje vašeho balíčku.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
