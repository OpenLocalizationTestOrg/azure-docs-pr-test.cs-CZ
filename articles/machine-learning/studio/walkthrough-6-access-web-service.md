---
title: "Krok 6: Přístup ke službě Machine Learning Web | Microsoft Docs"
description: "Krok 6 vývoj prediktivního řešení návod: přístup k Azure Machine Learning webové služby aktivní."
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
ms.openlocfilehash: d1a31bc95b249c3fa1caced138bbf3459eb6282c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Krok 6 průvodce: Přístup k webové službě Azure Machine Learning

Toto je poslední krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](walkthrough-develop-predictive-solution.md)

1. [Vytvoření pracovního prostoru Machine Learning](walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů](walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby](walkthrough-5-publish-web-service.md)
6. **Přístup k webové službě**

- - -
V předchozím kroku v tomto návodu jsme nasadili webová služba, která používá našeho úvěrové riziko předpovědi modelu. Uživatelé jsou nyní může odesílat data a přijímat výsledky. 

Webová služba je Azure webové služby, který může přijímat a vrátit data pomocí rozhraní API REST v jednom ze dvou způsobů:  

* **Požadavek a odpověď** – uživatel odesílá jeden nebo více řádků platební dat ve službě s použitím protokolu HTTP a službu odpoví jednu nebo více sad výsledků.
* **Spuštění dávky** – uživatel uloží jednu nebo více řádky platební dat v Azure blob a poté odešle umístění objektu blob ve službě. Tato služba skóre všechny řádky dat v vstupního objektu blob, uloží výsledky v jiném objektu blob a vrátí adresu URL tohoto kontejneru.  

Nejrychlejší a nejsnazší způsob pro přístup k webové službě Classic je prostřednictvím [webové aplikace Azure ML požadavků a odpovědí Service](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) nebo [Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Tyto šablony webové aplikace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data a co vrátí webové služby. Jediné, co musíte udělat je poskytnout přístup k webové služby a data a šablona nemá rest.

Další informace o použití šablony webové aplikace, najdete v části [využívat Azure Machine Learning webové služby pomocí šablony webové aplikace](consume-web-service-with-web-app-template.md).

Také lze vytvářet vlastní aplikaci pro přístup k webové službě pomocí počáteční kód stanovené v R, C# a Python programovací jazyky.

Najdete kompletní informace v [využívání Azure Machine Learning webové služby](consume-web-services.md).

