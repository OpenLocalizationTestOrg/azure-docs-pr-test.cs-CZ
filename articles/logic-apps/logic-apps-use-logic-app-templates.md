---
title: "pracovní postupy aaaCreate ze šablon - Azure Logic Apps | Microsoft Docs"
description: "Začínáme – rychle vytvořit pracovní postupy pomocí aplikace logiky Azure šablony tooconnect aplikací a integrovat data."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0127523662a12076772b52968f1e2f9cbad72a1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-tooget-started-quickly"></a>Konfigurace pracovního postupu pomocí předdefinovaných šablon nebo vzor tooget rychle začít

## <a name="what-are-logic-app-templates"></a>Co jsou šablony aplikace logiky
Šablonu logiku aplikace je aplikace logiky předem připravené, které můžete použít tooquickly začít vytvářet své vlastní pracovní postup. 

Tyto šablony jsou vhodný způsob toodiscover různých vzorů, které mohou být vytvořeny pomocí aplikací logiky. Můžete použít tyto šablony jako-je nebo je upravit toofit váš scénář.

## <a name="overview-of-available-templates"></a>Přehled dostupných šablon
Existuje mnoho dostupných šablon aktuálně publikované v hello logiku aplikace platformy. Některé kategorie příklad, stejně jako typ hello konektory používány, jsou uvedeny níže.

### <a name="enterprise-cloud-templates"></a>Šablony organizace cloudu
Šablony, které se integrují Dynamics CRM Salesforce, pole, objektů Blob v Azure a ostatní konektory pro vaše potřeby enterprise cloud. Některé příklady z co můžete udělat pomocí těchto šablony zahrnují uspořádání zájemců a zálohování dat podnikových souborů.

### <a name="enterprise-integration-pack-templates"></a>Šablony organizace integration pack
Konfigurace VETER (ověřit, extrakce, transformace, zlepšit komunikaci oddělení, směrovat) kanály, přijetí X12 EDI dokumentu přes AS2 a její transformace tooXML, stejně jako zprávy X12 a AS2 zpracování.

### <a name="protocol-pattern-templates"></a>Protokol vzor šablony
Tyto šablony se skládají logic Apps, které obsahují vzory protokolu, jako je požadavků a odpovědí prostřednictvím protokolu HTTP, jakož i integrace napříč FTP a SFTP. Použijte tyto, které jsou, nebo jako základ pro vytváření složitějších vzory protokolu.  

### <a name="personal-productivity-templates"></a>Osobních kancelářských šablony
Vzory toohelp zvýšení produktivity osobní zahrnují šablony, které nastavit denní připomenutí, zapněte důležité pracovní položky do seznamu úkolů a automatizaci úloh zdlouhavé dolů krok schválení tooa jednoho uživatele.

### <a name="consumer-cloud-templates"></a>Šablony příjemce cloudu
Jednoduché šablony, které integrovat služby sociálních médií jako Twitter, Slack a e-mailu, nakonec schopná posílení sociálních médií marketingové iniciativy. Patří sem také šablony například zakalená kopírování, které může pomoci zvýšit produktivitu uložením čas strávený tradičně opakovaných úloh. 

## <a name="how-toocreate-a-logic-app-using-a-template"></a>Jak toocreate aplikace logiky pomocí šablony
tooget spuštění pomocí šablony aplikace logiky, přejděte do hello logiku aplikace designer. Pokud zadáváte hello Návrhář otevřením existující aplikace logiky, aplikaci logiky hello automaticky načte v návrháře zobrazení. Pokud vytváříte nové aplikace logiky, ale zobrazí úvodní obrazovka níže.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Prostřednictvím této obrazovky můžete buď toostart pomocí prázdné logiku aplikace nebo předem připravených šablon. Pokud vyberete jednu z hello šablon, jsou k dispozici další informace. V tomto příkladu používáme hello *v Dropbox je vytvořen nový soubor, zkopírujte jej tooOneDrive* šablony.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Pokud si zvolíte toouse hello šablony, vyberte možnost hello *tuto šablonu použít* tlačítko. Zobrazí se výzva toosign v účtech tooyour podle využívá šablonu hello konektory. Nebo, pokud jste dříve vytvořili připojení pomocí těchto konektorů, můžete si vybrat pokračovat, jak vidíte níže.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Po vytvoření připojení hello a výběrem *pokračovat*, aplikace logiky hello se otevře v návrháře zobrazení.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

V předchozím příkladu hello stejně jako hello případě mnoho šablony, některá pole povinná vlastnost hello může doplnit v rámci hello konektory; ale některé může stále vyžadují hodnotu bylo možné tooproperly nasazení aplikace logiky hello. Pokud se pokusíte toodeploy bez zadávání některé hello chybějící pole, zobrazí se upozornění s chybovou zprávou.

Pokud chcete tooreturn toohello šablony prohlížeč, vyberte hello *šablony* tlačítko v horním navigačním panelu hello. Přepnutím back toohello šablony prohlížeč ztratit všechny neuložené průběh. Předchozí tooswitching zpět do šablony prohlížeče se zobrazí upozornění zpráva s upozorněním tohoto objektu.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-toodeploy-a-logic-app-created-from-a-template"></a>Jak toodeploy aplikace logiky vytvořené ze šablony
Po načtení šablony a všechny požadované změny, vyberte v levém horním rohu hello hello tlačítko Uložit. Tím se uloží a publikuje aplikace logiky.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Pokud by jako další informace o tom, jak tooadd další kroky do stávající šablony aplikace logiky nebo proveďte úpravy obecně, přečtěte si informace v [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

