---
title: "aaaWorking se souborem XML zprávy ve svých pracovních postupech - Azure Logic Apps | Microsoft Docs"
description: "Zpracovat, ověřit, transformace a zlepšit komunikaci oddělení XML zprávy aplikace logiky a obchodní tooscenarios pomocí hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: f90ae89fef0a4bd17286adbce398e573940bb790
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a>Ověření a transformace XML, kódování a dekódování plochých souborů a rozšířit funkce zprávy v aplikacích logiky

Použití aplikace logiky, máte hello možnost tooprocess XML zprávy, které odesílat a přijímat. Tato funkce je součástí hello Enterprise integračního balíčku. Pro tyto uživatele s pozadí BizTalk Server hello Enterprise integrační balíček poskytuje podobné tootransform dalo a ověření zprávy, pracovat s plochých souborů a i pomocí jazyka XPath tooenrich nebo extrahuje specifické vlastnosti ze zprávy. 

Pro uživatele, kteří jsou nový prostor toothis rozbalte tyto funkce, jak při zpracování zprávy v rámci pracovního postupu. Například pokud se v případě business-to-business a pracovat s konkrétní schémat XML, můžete použít hello Enterprise integračního balíčku tooenhance jak vaše společnost zpracovává tyto zprávy. 

Hello Enterprise integrační balíček obsahuje: 

* [Ověření XML](logic-apps-enterprise-integration-xml-validation.md "Další informace o ověření XML zprávy") -ověření příchozích nebo odchozích XML zprávy pro konkrétní schéma.
* [Transformace XML](../logic-apps/logic-apps-enterprise-integration-transform.md "Další informace o mapování a transformace zprávy XML") – převést nebo úprava zprávy XML na základě požadavky nebo požadavky hello partnera.
* [Plochý soubor kódování a dekódování plochý soubor](logic-apps-enterprise-integration-flatfile.md "Další informace o plochý soubor kódování a dekódování") – zakódování nebo dekódování plochý soubor. Například SAP přijímá a odesílá IDOC soubory ve formátu plochý soubor. Řada platforem integrace vytvořit XML zprávy, včetně Logic Apps. Ano můžete vytvořit aplikaci logiky, že kodér plochý soubor hello používá příliš "převést" soubory tooflat soubory XML. 
* [Výraz XPath](https://msdn.microsoft.com/library/mt643789.aspx) – zlepšit komunikaci oddělení zprávu a extrahovat specifické vlastnosti z uvítací zprávu. Můžete pak použijte hello extrahovat vlastnosti tooroute hello zpráva tooa cílového nebo zprostředkující koncový bod.

## <a name="try-it-out"></a>Vyzkoušet
[Nasazení aplikace logiky plně funkční ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (ukázka Githubu) pomocí funkcí hello XML v Azure Logic Apps.

## <a name="learn-more"></a>Další informace
[Další informace o hello Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")
