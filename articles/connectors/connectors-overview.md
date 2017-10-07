---
title: "aaaOverview konektorů aplikace logiky | Microsoft Docs"
description: "Přehled konektory, které lze použít v aplikaci logiky"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: ca8dab2e-9b69-4b1e-865d-1facd9f0cdac
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan
ms.openlocfilehash: dc4cac4c0dfaa2f9fd218ffc04414fa8e52a91d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-in-a-logic-app"></a>Použití konektorů v aplikaci logiky
Konektorů poskytuje rychlý přístup tooevents, datům a akcím napříč služeb, protokoly a platformy.  Hello úplný seznam konektorů, které podporuje aplikace logiky můžete [je zde uveden](apis-list.md).  Konektory lze použít jako aktivační události nebo akce v aplikaci logiky a může vyžadovat nakonfigurované *připojení* toouse (například: autorizace Twitter účet tooaccess nebo post vaším jménem).

## <a name="basics"></a>Základy
Konektory jsou hostované služby v rámci logiku aplikace toointegrate s jinými službami jako Dynamics Azure, služby Salesforce, dostanete [a další](apis-list.md).  Jsou nasazen a spravován společností Microsoft, takže můžete vytvářet pracovní postupy integrace s škálování, propustnosti a zabezpečení postaráno.  Můžete přidat aplikace logiky tooa konektor vyhledávání a výběrem akce konektoru nebo aktivovat pod **zobrazit Microsoft spravované rozhraní API**.

![Nabídka Akce pro výběr aktivační události][1]

Jeho sadu vlastností tooconfigure bude mít každý konektor akce nebo aktivační událost.  Klikněte na toolearn tlačítka hello informace o další informace o akci, nebo reference jeho dokumentace [toolearn Další](apis-list.md).

Pokud chcete toointegrate s služby nebo rozhraní API, které není ještě konektor, můžete také rozšířit aplikace logiky prostřednictvím [vlastní konektor](../logic-apps/logic-apps-create-api-app.md) nebo stačí zavolat přímo toohello služby přes protokol například HTTP.

## <a name="triggers"></a>Triggery
Některé konektory obsahovat aktivační událost, to znamená, bude fire aplikace logiky a předejte všechna data v rámci hello aktivační událost z tohoto konektoru.  Aktivační události je vždycky hello prvním krokem při aplikace logiky.  Oblíbených aktivační procedury patří operace, jako je:

* Opakování - spouštět každou hodinu
* Při přijetí požadavku HTTP
* Když je položka přidána tooa fronty
* Při doručení e-mailu

Některé aktivačních událostí bude platit hello rychlých událost probíhá prostřednictvím aplikace logiky toohello oznámení, a ostatní potřebovat intervalu opakování nakonfigurované na tom, jak často hello aplikace logiky zkontroluje hello služby pro událost (až tooevery 15 sekund).  

Po přijetí události je spuštění aplikace logiky hello se aktivují a spuštění hello akce v pracovním postupu hello.  Také budete moct tooaccess žádná data z hello aktivovat v rámci pracovního postupu hello (pro příklad hello 'na nový tweet, aktivační události předá hello tweet do hello spuštění).

## <a name="actions"></a>Akce
Většina konektory mít jednu nebo mnoho akcí, které mohou být provedeny jako součást pracovního postupu hello.  Akce jsou všechny kroky, které dojít po spuštění hello byla aktivována z aktivační události.  tooadd akce klikněte na tlačítko hello **nový krok** tlačítko a vyhledejte konektor chcete toouse hello.  Po začlenění (a po dokončení konfigurace žádné [připojení](#connections) , může být nutný) se zobrazí karta Akce hello můžete nakonfigurovat.  Můžete vybrat data z předchozích kroků kliknutím na jakékoli hello tokenů pro výstupy nebo zadejte jakoukoli jinou konfiguraci podle potřeby.

![Konfigurace konektoru akce][2]

## <a name="connections"></a>Připojení
Většina konektory vyžadovat tooconfigure *připojení* předtím, než můžete použít konektor hello.  A *připojení* je žádné přihlášení nebo konfigurace připojení potřeba tooaccess hello konektor.  Konektory, které používají OAuth, vytvořit připojení znamená přihlášení k hello službě (např. Office 365, Salesforce nebo Githubu), kde můžete šifrovat a bezpečně uložené v úložišti Azure tajný přístupový token.  Ostatní konektory (např. FTP a SQL) vyžadují připojení, který obsahuje konfiguraci jako adresu serveru, uživatelské jméno a heslo.  Tyto podrobnosti konfigurace připojení jsou také šifrovaný a bezpečně uložit.  Připojení bude hello služby pro možnost tooaccess tak dlouho, dokud služba hello umožňuje.  Pro Azure Active Directory OAuth připojení (např. Office 365 a Dynamics), abychom mohli pokračovat po neomezenou dobu toorefresh hello přístupový token.  Další služby mohou uvedeny omezení na jak dlouho token můžete použít bez se právě aktualizuje.  Obecně určité akce, jako je změna hesla zrušíte platnost všech přístupových tokenů.  

Připojení lze zobrazit a spravovat v Azure kliknutím **Procházet** a výběrem **rozhraní API připojení**.  Z hello API připojení prostředků můžete zobrazit, upravit, aktualizovat nebo znovu autorizovat, všechna připojení, které jste vytvořili.

## <a name="next-steps"></a>Další kroky
* [Vytvoření první aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)
* [Další běžné používá a příklady logic Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Začínáme s enterprise integrace triggery a akce](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png
