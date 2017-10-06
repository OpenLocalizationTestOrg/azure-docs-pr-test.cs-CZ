---
title: "aaaHow tooincrease souběžnosti webové služby Azure Machine Learning | Microsoft Docs"
description: "Zjistěte, jak tooincrease souběžnosti ze Azure Machine Learning webovou službu tak, že přidáte další koncové body."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "Azure machine learningu, webové služby, operationalization, škálování, koncový bod, souběžnosti"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: e2ad16ec766820a64f36c31232f6a33a79196af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Škálování webové služby Azure Machine Learning přidáním další koncové body
> [!NOTE]
> Toto téma popisuje techniky použít tooa **Classic** Machine Learning webové služby. 
> 
> 

Ve výchozím nastavení každý publikované webové služby je nakonfigurované toosupport 20 souběžnými požadavky a může být až 200 souběžných požadavků. Zatímco hello klasický portál Azure poskytuje tooset způsob, jak tuto hodnotu, Azure Machine Learning automaticky optimalizuje hello nastavení tooprovide hello nejlepší výkon pro webové služby a portálu hodnota hello je ignorována. 

Pokud máte v plánu toocall hello rozhraní API pomocí zatížení vyšší než maximální počet souběžných volání hodnota 200 bude podporovat, měli byste vytvořit několik koncových bodů na hello stejné webové služby. Potom můžete náhodně distribuovat zatížení napříč všemi z nich.

Hello škálování webové služby je běžné úlohy. Některé z důvodů tooscale jsou toosupport více než 200 souběžných požadavků, zvýšit dostupnost prostřednictvím několik koncových bodů nebo zadejte samostatné koncové body pro hello webovou službu. Škálování hello můžete zvýšit tak, že přidáte další koncové body pro hello stejné webové služby prostřednictvím [portál Azure classic](https://manage.windowsazure.com/) nebo hello [webovou službu Azure Machine Learning](https://services.azureml.net/) portálu.

Další informace o přidání nové koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md).

Mějte na paměti, který používá souběžnosti vysoký počet může být škodlivé, pokud nejsou volání hello rozhraní API s odpovídajícím způsobem vysokou míru. Můžete se setkat ojediněle vypršení časových limitů nebo špičky hello latence když vložíte relativně nízký zatížení na rozhraní API nakonfigurovaný pro vysokého zatížení.

Hello synchronní rozhraní API se obvykle používá v situacích, kde je žádoucí s nízkou latencí. Latence zde znamená hello doba potřebná pro jeden požadavek toocomplete hello rozhraní API a nemá účet pro všechny zpoždění sítě. Řekněme, že máte rozhraní API s latencí 50 ms. toofully využívat hello s omezení úrovní vysoce dostupné kapacity a maximální počet souběžných volání = 20, je nutné toocall toto rozhraní API 20 * 1000 / 50 = 400 times za sekundu. Tato další rozšíření, maximální počet souběžných volání 200 umožňuje vám toocall hello API 4000 časy za sekundu, za předpokladu, že latence 50 ms.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
