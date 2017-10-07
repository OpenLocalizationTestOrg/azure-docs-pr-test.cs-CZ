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
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Importu trénovacích dat ze souboru na pevném disku do nástroje Machine Learning Studio
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Zjistěte, jak tooupload dat souboru z vašeho pevného disku toouse jako Cvičná data v Azure Machine Learning Studio. Importováním souboru dat hello máte datovou sadu modul, který je připravený k použití v pracovním prostoru.

## <a name="steps-tooimport-data-from-a-local-file"></a>Kroky tooimport data z místního souboru
tooimport data z místního pevného disku, hello následující:

1. Klikněte na tlačítko **+ nový** v hello dolní části okna Machine Learning Studio hello.
2. Vyberte **datovou sadu** a **z místního souboru**.
3. V hello **nahrát nová datová sada** dialogu procházení toohello soubor má tooupload
4. Zadejte název, identifikovat hello datový typ a volitelně zadat popis. Popis se doporučuje – umožňuje toorecord žádné charakteristiky hello dat má tooremember při použití hello data v budoucnosti hello.
5. Hello políčko **Toto je nová verze hello existující datovou sadu** vám umožní tooupdate existující datovou sadu s nová data. Klikněte na toto políčko a potom zadejte název hello existující datovou sadu.

![Nahrát nová datová sada](media/machine-learning-import-data-from-local-file/upload-dataset.png)

Během nahrávání se zobrazí zpráva, že váš soubor se nahrává. Nahrát doba závisí na velikosti hello rychlosti dat a hello služby toohello připojení. Pokud víte, že soubor hello bude trvat dlouhou dobu, můžete provést jiných věcí uvnitř Machine Learning Studio popředí. Ale zavření prohlížeče hello způsobí, že toofail nahrávání dat hello.

## <a name="dataset-module-is-ready-for-use"></a>Modul datové sady je připravený k použití
Po nahrání dat je uložený v modulu datovou sadu a je k dispozici tooany experimentu v pracovním prostoru.

Při úpravě experiment, můžete najít hello datové sady, které jste vytvořili v hello **Moje datové sady** seznamu v části hello **uložit datové sady** seznamu v palety modulů hello. Můžete přetáhnout a vyřadit hello datovou sadu na plátno experimentu hello, pokud chcete datovou sadu toouse hello k další analýze a strojové učení.
