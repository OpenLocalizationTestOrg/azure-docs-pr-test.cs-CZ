---
title: "Rozšíření experimentů pomocí R | Microsoft Docs"
description: "Postup rozšíření funkce Azure Machine Learning Studio prostřednictvím jazyka R pomocí modulu spuštění skriptu R došlo k chybě."
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
ms.openlocfilehash: fe207ef917980be8b554ad9c08176d108b19fb71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="extend-your-experiment-with-r"></a><span data-ttu-id="81a2c-103">Rozšíření experimentu pomocí R</span><span class="sxs-lookup"><span data-stu-id="81a2c-103">Extend your experiment with R</span></span>
<span data-ttu-id="81a2c-104">Funkce Azure Machine Learning Studio prostřednictvím jazyka R můžete rozšířit pomocí [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="81a2c-104">You can extend the functionality of Azure Machine Learning Studio through the R language by using the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="81a2c-105">Tento modul přijímá více vstupní datové sady a výsledkem jednu datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="81a2c-105">This module accepts multiple input datasets and yields a single dataset as output.</span></span> <span data-ttu-id="81a2c-106">Můžete zadat skript jazyka R do **skript jazyka R** parametr [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="81a2c-106">You can type an R script into the **R Script** parameter of the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="81a2c-107">Máte přístup k každý vstupní port modulu pomocí kódu podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="81a2c-107">You access each input port of the module by using code similar to the following:</span></span>

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a><span data-ttu-id="81a2c-108">Seznam aktuálně nainstalovaných balíčků</span><span class="sxs-lookup"><span data-stu-id="81a2c-108">Listing all currently-installed packages</span></span>
<span data-ttu-id="81a2c-109">Seznam balíčků nainstalovaných můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="81a2c-109">The list of installed packages can change.</span></span> <span data-ttu-id="81a2c-110">Seznam aktuálně nainstalovaných balíčků naleznete v [R balíčky nepodporuje Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span><span class="sxs-lookup"><span data-stu-id="81a2c-110">A list of currently installed packages can be found in [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span></span>

<span data-ttu-id="81a2c-111">Můžete také můžete získat úplný, aktuální seznam balíčků nainstalovaných tak, že zadáte následující kód do [spustit skript jazyka R] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="81a2c-111">You also can get the complete, current list of installed packages by entering the following code into the [Execute R Script][execute-r-script] module:</span></span>

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

<span data-ttu-id="81a2c-112">Tím se odešle seznam balíčků do výstupní port modulu [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="81a2c-112">This sends the list of packages to the output port of the [Execute R Script][execute-r-script] module.</span></span>
<span data-ttu-id="81a2c-113">Chcete-li zobrazit seznam balíčků, připojte modul převod [převést na sdílený svazek clusteru] [ convert-to-csv] na levém výstupu [spustit skript jazyka R] [ execute-r-script] modulu, Spusťte experiment, pak klikněte na výstup modulu převod a vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="81a2c-113">To view the package list, connect a conversion module such as [Convert to CSV][convert-to-csv] to the left output of the [Execute R Script][execute-r-script] module, run the experiment, then click the output of the conversion module and select **Download**.</span></span> 

![Stažení výstupu modulu "Převést na CSV"](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a><span data-ttu-id="81a2c-115">Import balíčků</span><span class="sxs-lookup"><span data-stu-id="81a2c-115">Importing packages</span></span>
<span data-ttu-id="81a2c-116">Můžete importovat balíčky, které již nejsou nainstalovány pomocí následujících příkazů v [spustit skript jazyka R] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="81a2c-116">You can import packages that are not already installed by using the following commands in the [Execute R Script][execute-r-script] module:</span></span>

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

<span data-ttu-id="81a2c-117">kde `my_favorite_package.zip` soubor obsahuje vašeho balíčku.</span><span class="sxs-lookup"><span data-stu-id="81a2c-117">where the `my_favorite_package.zip` file contains your package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
