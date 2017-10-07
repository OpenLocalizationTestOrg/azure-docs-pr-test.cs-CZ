---
title: "aaaLogic aplikace B2B edifact dekódovat vyřešit UNH2.5 - Azure Logic Apps | Microsoft Docs"
description: "Azure B2B aplikace logiky edifact dekódovat vyřešit UNH2.5"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 6d85242d0f828fa52cdc9689938f3ba1e51b1183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toohandle-edifact-documents-having-unh25-segment"></a>Jak toohandle EDIFACT dokumentů s UNH2.5 segmentu
Když je k dispozici v dokumentu EDIFACT hello UNH2.5, ho se používá pro vyhledávání schématu. 

Příklad: pole UNH hello je **EAN008** ve zprávě EDIFACT hello  
UNH + SSDD1 + OBJEDNÁVKY: D: 03B: ZRUŠENÍ:**EAN008**.  

Kroky toofollow toohandle uvítací zprávu 
1. Aktualizovat schéma hello
2. Zkontrolujte nastavení smlouvy hello  

## <a name="update-hello-schema"></a>Aktualizovat schéma hello
tooprocess uvítací zprávu, budete potřebovat toodeploy schématu s název hello UNH2.5 kořenového uzlu.  Pro danou příklad, bude název kořenové schématu hello **EFACT_D03B_ORDERS_EAN008**  

Pro každý D03B_ORDERS různých UNH2.5 segmentu byste měli toodeploy jednotlivých schématu.  

## <a name="add-schema-toohello-edifact-agreement"></a>Přidání smlouvy EDIFACT toohello schématu
### <a name="edifact-decode"></a>Dekódovat EDIFACT
tooDecode hello příchozí zprávy, nakonfigurujte hello schématu v hello EDIFACT smlouvy obdrží nastavení
1. Přidat účet integrace toohello schématu hello    
2. Konfigurace hello schématu v hello EDIFACT smlouvy obdrží nastavení. 
3. Vyberte smlouvy EDIFACT a klikněte na **upravit jako JSON**.  Přidejte hodnotu UNH2.5 v hello přijetí smlouvy **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT kódování
tooEncode hello příchozí zprávy, nakonfigurujte hello schématu hello EDIFACT smlouvy odeslání
1. Přidat účet integrace toohello schématu hello    
2. Nakonfigurujte hello schématu hello EDIFACT smlouvy odeslání. 
3. Vyberte smlouvy EDIFACT a klikněte na **upravit jako JSON**.  Přidejte hodnotu UNH2.5 v hello odeslat smlouvy **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o integraci účet smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  