---
title: "Umožňuje importovat data z jiného experimentu do nástroje Machine Learning Studio | Microsoft Docs"
description: "Jak uložit Cvičná data v Azure Machine Learning Studio a použít ho v jiného experimentu."
keywords: "Importujte dat, data, zdroje dat, Cvičná data"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 7da9dcec-5693-4bb6-8166-15904e7f75c3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: ecfa2110d0d51511ceb5bd07b730732ecfa2e9e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="import-your-data-into-azure-machine-learning-studio-from-another-experiment"></a><span data-ttu-id="5f352-104">Import dat do nástroje Azure Machine Learning Studio z jiného experimentu</span><span class="sxs-lookup"><span data-stu-id="5f352-104">Import your data into Azure Machine Learning Studio from another experiment</span></span>
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="5f352-105">Bude nastat situace, kdy budete chtít využít výsledek zprostředkující z jednoho experiment a používat ji jako součást jiného experimentu.</span><span class="sxs-lookup"><span data-stu-id="5f352-105">There will be times when you'll want to take an intermediate result from one experiment and use it as part of another experiment.</span></span> <span data-ttu-id="5f352-106">K tomuto účelu uložit modul jako datové sady:</span><span class="sxs-lookup"><span data-stu-id="5f352-106">To do this, you save the module as a dataset:</span></span>

1. <span data-ttu-id="5f352-107">Klikněte na výstup modulu, který chcete uložit jako datové sady.</span><span class="sxs-lookup"><span data-stu-id="5f352-107">Click the output of the module that you want to save as a dataset.</span></span>
2. <span data-ttu-id="5f352-108">Klikněte na tlačítko **uložit jako datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="5f352-108">Click **Save as Dataset**.</span></span>
3. <span data-ttu-id="5f352-109">Po zobrazení výzvy zadejte název a popis, který by bylo možné snadno identifikovat datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5f352-109">When prompted, enter a name and a description that would allow you to identify the dataset easily.</span></span>
4. <span data-ttu-id="5f352-110">Klikněte **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="5f352-110">Click the **OK** checkmark.</span></span>

<span data-ttu-id="5f352-111">Po dokončení uložení datová sada bude k dispozici pro použití v rámci všech experimentu v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="5f352-111">When the save finishes, the dataset will be available for use within any experiment in your workspace.</span></span> <span data-ttu-id="5f352-112">Najdete ho v **uložit datové sady** seznamu paletě modulů.</span><span class="sxs-lookup"><span data-stu-id="5f352-112">You can find it in the **Saved Datasets** list in the module palette.</span></span>

