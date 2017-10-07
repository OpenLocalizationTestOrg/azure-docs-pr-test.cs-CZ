---
title: "Krok 6: Přístup k hello Machine Learning webové služby | Microsoft Docs"
description: "Krok 6 hello vývoj prediktivního řešení návod: přístup k Azure Machine Learning webové služby aktivní."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 211de0294092c6a6b5e6eb608d5d3b88107674c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-6-access-hello-azure-machine-learning-web-service"></a>Návod krok 6: Přístup k webové službě Azure Machine Learning hello

Toto je poslední krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby hello](machine-learning-walkthrough-5-publish-web-service.md)
6. **Přístup k webové službě hello**

- - -
V předchozím kroku hello v tomto návodu jsme nasadili webová služba, která používá našeho úvěrové riziko předpovědi modelu. Uživatelé nyní jsou možné toosend data tooit a příjem výsledků. 

Hello webové služby je Azure webové služby, který může přijímat a vrátit data pomocí rozhraní API REST v jednom ze dvou způsobů:  

* **Požadavek a odpověď** – hello uživatel odešle jednu nebo více řádky dat toohello platební služby s použitím protokolu HTTP a hello služby odpoví jednu nebo více sad výsledků.
* **Spuštění dávky** – hello uživatel uloží jednu nebo více řádky platební dat v Azure blob a poté odešle služby toohello umístění objektu blob hello. skóre Hello služby, které všechny hello řádky dat v hello vstupního objektu blob, úložiště hello výsledkem jiného objektu blob a vrátí hello adresu URL tohoto kontejneru.  

Hello tooaccess nejrychlejší a nejjednodušší způsob je webová služba Classic prostřednictvím hello [webové aplikace Azure ML požadavků a odpovědí Service](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) nebo [Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Tyto šablony webové aplikace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data a co vrátí webové služby. Stačí toodo je poskytnout přístup tooyour webové služby a data a šablony hello hello rest.

Další informace o používání hello webové aplikace šablony najdete v tématu [využívat Azure Machine Learning webové služby pomocí šablony webové aplikace](machine-learning-consume-web-service-with-web-app-template.md).

Také lze vytvářet vlastní aplikaci tooaccess hello webové službě pomocí počáteční kód stanovené v R, C# a Python programovacích jazyků.

Najdete kompletní informace v [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

