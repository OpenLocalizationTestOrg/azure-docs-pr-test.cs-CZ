---
title: "aaaCreate prostředí Azure časové řady Insights | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak toocreate časové řady prostředí, připojte ho zdroj události tooan a připravené tooanalyze vaše data události v minutách."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 7120fc9a6e4d4a4972f8cb37e4d9945cfb746fd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-time-series-insights-environment-in-hello-azure-portal"></a>Vytvoření nového prostředí časové řady statistiky v hello portálu Azure

Prostředí Time Series Insights je prostředek Azure s kapacitou úložiště a příchozího přenosu dat. Zákazníci zřídit s kapacitou hello požadované prostředí prostřednictvím hello portálu Azure.

## <a name="steps-toocreate-hello-environment"></a>Kroky toocreate hello prostředí

Postupujte podle těchto kroků toocreate prostředí:

1.  Přihlaste se toohello [portál Azure](https://portal.azure.com).
2.  V horní části hello levého horního rohu klikněte na tlačítko hello plus přihlašovací ("+").
3.  Vyhledejte "Časové řady Insights" hello vyhledávacího pole.

  ![Vytvoření prostředí časové řady Statistika hello](media/get-started/getstarted-create-environment1.png)

4.  Vyberte Time Series Insights a klikněte na Vytvořit.

  ![Vytvořte skupinu prostředků časové řady Statistika hello](media/get-started/getstarted-create-environment2.png)

5.  Zadejte název prostředí. Tento název bude reprezentovat hello prostředí v [časové řady explorer](https://insights.timeseries.azure.com).
6.  Vyberte předplatné. Vyberte to, které obsahuje váš zdroj událostí. Statistika časové řady může automaticky rozpoznat Azure IoT Hub a prostředky centra událostí existující v hello stejného předplatného.
7.  Vyberte nebo vytvořte skupinu prostředků. Skupina prostředků je kolekce společně používaných prostředků Azure.
8.  Vyberte umístění pro hostování. tooavoid přesunutí dat mezi datového centra, vyberte umístění, které obsahuje váš zdroj událostí.
9.  Vyberte cenovou úroveň.
10. Vyberte kapacitu. Kapacitu prostředí můžete po vytvoření změnit.
11. Vytvořte prostředí. Můžete taky připnout řídicího panelu toohello prostředí pro snadný přístup při každém přihlášení.

  ![Vytvoření toodashboard pin časové řady Statistika hello](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Další kroky

* [Definovat zásady přístupu k datům](time-series-insights-data-access.md) tooaccess prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com)
* [Vytvoření zdroje událostí](time-series-insights-add-event-source.md)
* [Odesílání událostí](time-series-insights-send-events.md) toohello zdroj události
