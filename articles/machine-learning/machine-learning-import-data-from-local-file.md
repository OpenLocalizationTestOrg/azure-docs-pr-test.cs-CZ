---
title: aaaImport data ze souboru do Azure Machine Learning Studio | Microsoft Docs
description: "Zjistěte, jak tooupload Cvičná data souborů z vašeho pevného disku tooAzure Machine Learning Studio. Tím se vytvoří datová sada modulu v pracovním prostoru hello."
keywords: "Importujte dat, formát dat, datové typy, zdroje dat, Cvičná data"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 636facd9042145382c953a1c75969149ede6f6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a><span data-ttu-id="66837-105">Importu trénovacích dat ze souboru na pevném disku do nástroje Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="66837-105">Import training data from a file on your hard drive into Machine Learning Studio</span></span>
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="66837-106">Zjistěte, jak tooupload dat souboru z vašeho pevného disku toouse jako Cvičná data v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="66837-106">Learn how tooupload a data file from your hard drive toouse as training data in Azure Machine Learning Studio.</span></span> <span data-ttu-id="66837-107">Importováním souboru dat hello máte datovou sadu modul, který je připravený k použití v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="66837-107">By importing hello data file, you have a dataset module ready for use in your workspace.</span></span>

## <a name="steps-tooimport-data-from-a-local-file"></a><span data-ttu-id="66837-108">Kroky tooimport data z místního souboru</span><span class="sxs-lookup"><span data-stu-id="66837-108">Steps tooimport data from a local file</span></span>
<span data-ttu-id="66837-109">tooimport data z místního pevného disku, hello následující:</span><span class="sxs-lookup"><span data-stu-id="66837-109">tooimport data from a local hard drive, do hello following:</span></span>

1. <span data-ttu-id="66837-110">Klikněte na tlačítko **+ nový** v hello dolní části okna Machine Learning Studio hello.</span><span class="sxs-lookup"><span data-stu-id="66837-110">Click **+NEW** at hello bottom of hello Machine Learning Studio window.</span></span>
2. <span data-ttu-id="66837-111">Vyberte **datovou sadu** a **z místního souboru**.</span><span class="sxs-lookup"><span data-stu-id="66837-111">Select **DATASET** and **FROM LOCAL FILE**.</span></span>
3. <span data-ttu-id="66837-112">V hello **nahrát nová datová sada** dialogu procházení toohello soubor má tooupload</span><span class="sxs-lookup"><span data-stu-id="66837-112">In hello **Upload a new dataset** dialog, browse toohello file you want tooupload</span></span>
4. <span data-ttu-id="66837-113">Zadejte název, identifikovat hello datový typ a volitelně zadat popis.</span><span class="sxs-lookup"><span data-stu-id="66837-113">Enter a name, identify hello data type, and optionally enter a description.</span></span> <span data-ttu-id="66837-114">Popis se doporučuje – umožňuje toorecord žádné charakteristiky hello dat má tooremember při použití hello data v budoucnosti hello.</span><span class="sxs-lookup"><span data-stu-id="66837-114">A description is recommended - it allows you toorecord any characteristics about hello data that you want tooremember when using hello data in hello future.</span></span>
5. <span data-ttu-id="66837-115">Hello políčko **Toto je nová verze hello existující datovou sadu** vám umožní tooupdate existující datovou sadu s nová data.</span><span class="sxs-lookup"><span data-stu-id="66837-115">hello checkbox **This is hello new version of an existing dataset** allows you tooupdate an existing dataset with new data.</span></span> <span data-ttu-id="66837-116">Klikněte na toto políčko a potom zadejte název hello existující datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="66837-116">Click this checkbox and then enter hello name of an existing dataset.</span></span>

![Nahrát nová datová sada](media/machine-learning-import-data-from-local-file/upload-dataset.png)

<span data-ttu-id="66837-118">Během nahrávání se zobrazí zpráva, že váš soubor se nahrává.</span><span class="sxs-lookup"><span data-stu-id="66837-118">During upload, you'll see a message that your file is being uploaded.</span></span> <span data-ttu-id="66837-119">Nahrát doba závisí na velikosti hello rychlosti dat a hello služby toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="66837-119">Upload time depends on hello size of your data and hello speed of your connection toohello service.</span></span> <span data-ttu-id="66837-120">Pokud víte, že soubor hello bude trvat dlouhou dobu, můžete provést jiných věcí uvnitř Machine Learning Studio popředí.</span><span class="sxs-lookup"><span data-stu-id="66837-120">If you know hello file will take a long time, you can do other things inside Machine Learning Studio while you wait.</span></span> <span data-ttu-id="66837-121">Ale zavření prohlížeče hello způsobí, že toofail nahrávání dat hello.</span><span class="sxs-lookup"><span data-stu-id="66837-121">However, closing hello browser causes hello data upload toofail.</span></span>

## <a name="dataset-module-is-ready-for-use"></a><span data-ttu-id="66837-122">Modul datové sady je připravený k použití</span><span class="sxs-lookup"><span data-stu-id="66837-122">Dataset module is ready for use</span></span>
<span data-ttu-id="66837-123">Po nahrání dat je uložený v modulu datovou sadu a je k dispozici tooany experimentu v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="66837-123">Once your data is uploaded, it's stored in a dataset module and is available tooany experiment in your workspace.</span></span>

<span data-ttu-id="66837-124">Při úpravě experiment, můžete najít hello datové sady, které jste vytvořili v hello **Moje datové sady** seznamu v části hello **uložit datové sady** seznamu v palety modulů hello.</span><span class="sxs-lookup"><span data-stu-id="66837-124">When you're editing an experiment, you can find hello datasets you've created in hello **My Datasets** list under hello **Saved Datasets** list in hello module palette.</span></span> <span data-ttu-id="66837-125">Můžete přetáhnout a vyřadit hello datovou sadu na plátno experimentu hello, pokud chcete datovou sadu toouse hello k další analýze a strojové učení.</span><span class="sxs-lookup"><span data-stu-id="66837-125">You can drag and drop hello dataset onto hello experiment canvas when you want toouse hello dataset for further analytics and machine learning.</span></span>
