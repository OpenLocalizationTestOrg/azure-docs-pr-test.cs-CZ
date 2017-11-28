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
# <a name="extend-your-experiment-with-r"></a><span data-ttu-id="a3df5-103">Rozšíření experimentu pomocí R</span><span class="sxs-lookup"><span data-stu-id="a3df5-103">Extend your experiment with R</span></span>
<span data-ttu-id="a3df5-104">Funkce hello nástroje Azure Machine Learning Studio prostřednictvím jazyka hello R můžete rozšířit pomocí hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="a3df5-104">You can extend hello functionality of Azure Machine Learning Studio through hello R language by using hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="a3df5-105">Tento modul přijímá více vstupní datové sady a výsledkem jednu datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="a3df5-105">This module accepts multiple input datasets and yields a single dataset as output.</span></span> <span data-ttu-id="a3df5-106">Skript jazyka R je možné zadat do hello **skript jazyka R** parametr hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="a3df5-106">You can type an R script into hello **R Script** parameter of hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="a3df5-107">Máte přístup k každý vstupní port hello modulu pomocí podobné toohello následující kód:</span><span class="sxs-lookup"><span data-stu-id="a3df5-107">You access each input port of hello module by using code similar toohello following:</span></span>

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a><span data-ttu-id="a3df5-108">Seznam aktuálně nainstalovaných balíčků</span><span class="sxs-lookup"><span data-stu-id="a3df5-108">Listing all currently-installed packages</span></span>
<span data-ttu-id="a3df5-109">Hello seznam balíčků nainstalovaných můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="a3df5-109">hello list of installed packages can change.</span></span> <span data-ttu-id="a3df5-110">Seznam aktuálně nainstalovaných balíčků naleznete v [R balíčky nepodporuje Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3df5-110">A list of currently installed packages can be found in [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span></span>

<span data-ttu-id="a3df5-111">Také můžete získat hello dokončení, aktuální seznam balíčků nainstalovaných zadáním hello následující kód do hello [spustit skript jazyka R] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="a3df5-111">You also can get hello complete, current list of installed packages by entering hello following code into hello [Execute R Script][execute-r-script] module:</span></span>

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

<span data-ttu-id="a3df5-112">Tím se odešle hello seznam balíčků toohello výstupní port modulu hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="a3df5-112">This sends hello list of packages toohello output port of hello [Execute R Script][execute-r-script] module.</span></span>
<span data-ttu-id="a3df5-113">balíček hello tooview seznamu, jako například připojit modul převod [převést tooCSV] [ convert-to-csv] toohello zbývajících výstup hello [spustit skript jazyka R] [ execute-r-script]modul, spustit hello experiment, potom klikněte na výstup hello hello převod modul a vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="a3df5-113">tooview hello package list, connect a conversion module such as [Convert tooCSV][convert-to-csv] toohello left output of hello [Execute R Script][execute-r-script] module, run hello experiment, then click hello output of hello conversion module and select **Download**.</span></span> 

![Stažení výstupu "Převést tooCSV" modulu](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is hello [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a><span data-ttu-id="a3df5-115">Import balíčků</span><span class="sxs-lookup"><span data-stu-id="a3df5-115">Importing packages</span></span>
<span data-ttu-id="a3df5-116">Můžete importovat balíčky, které již nejsou nainstalovány pomocí následujících příkazů v hello hello [spustit skript jazyka R] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="a3df5-116">You can import packages that are not already installed by using hello following commands in hello [Execute R Script][execute-r-script] module:</span></span>

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

<span data-ttu-id="a3df5-117">kde hello `my_favorite_package.zip` soubor obsahuje vašeho balíčku.</span><span class="sxs-lookup"><span data-stu-id="a3df5-117">where hello `my_favorite_package.zip` file contains your package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
