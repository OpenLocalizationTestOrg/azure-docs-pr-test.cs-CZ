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
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-tooget-started-quickly"></a><span data-ttu-id="d2647-103">Konfigurace pracovního postupu pomocí předdefinovaných šablon nebo vzor tooget rychle začít</span><span class="sxs-lookup"><span data-stu-id="d2647-103">Configure a workflow using a pre-built template or pattern tooget started quickly</span></span>

## <a name="what-are-logic-app-templates"></a><span data-ttu-id="d2647-104">Co jsou šablony aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="d2647-104">What are logic app templates</span></span>
<span data-ttu-id="d2647-105">Šablonu logiku aplikace je aplikace logiky předem připravené, které můžete použít tooquickly začít vytvářet své vlastní pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="d2647-105">A logic app template is a pre-built logic app that you can use tooquickly get started creating your own workflow.</span></span> 

<span data-ttu-id="d2647-106">Tyto šablony jsou vhodný způsob toodiscover různých vzorů, které mohou být vytvořeny pomocí aplikací logiky.</span><span class="sxs-lookup"><span data-stu-id="d2647-106">These templates are a good way toodiscover various patterns that can be built using logic apps.</span></span> <span data-ttu-id="d2647-107">Můžete použít tyto šablony jako-je nebo je upravit toofit váš scénář.</span><span class="sxs-lookup"><span data-stu-id="d2647-107">You can either use these templates as-is or modify them toofit your scenario.</span></span>

## <a name="overview-of-available-templates"></a><span data-ttu-id="d2647-108">Přehled dostupných šablon</span><span class="sxs-lookup"><span data-stu-id="d2647-108">Overview of available templates</span></span>
<span data-ttu-id="d2647-109">Existuje mnoho dostupných šablon aktuálně publikované v hello logiku aplikace platformy.</span><span class="sxs-lookup"><span data-stu-id="d2647-109">There are many available templates currently published in hello logic app platform.</span></span> <span data-ttu-id="d2647-110">Některé kategorie příklad, stejně jako typ hello konektory používány, jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="d2647-110">Some example categories, as well as hello type of connectors used in them, are listed below.</span></span>

### <a name="enterprise-cloud-templates"></a><span data-ttu-id="d2647-111">Šablony organizace cloudu</span><span class="sxs-lookup"><span data-stu-id="d2647-111">Enterprise cloud templates</span></span>
<span data-ttu-id="d2647-112">Šablony, které se integrují Dynamics CRM Salesforce, pole, objektů Blob v Azure a ostatní konektory pro vaše potřeby enterprise cloud.</span><span class="sxs-lookup"><span data-stu-id="d2647-112">Templates that integrate Dynamics CRM, Salesforce, Box, Azure Blob, and other connectors for your enterprise cloud needs.</span></span> <span data-ttu-id="d2647-113">Některé příklady z co můžete udělat pomocí těchto šablony zahrnují uspořádání zájemců a zálohování dat podnikových souborů.</span><span class="sxs-lookup"><span data-stu-id="d2647-113">Some examples of what can be done with these templates include organizing your leads and backing up your corporate file data.</span></span>

### <a name="enterprise-integration-pack-templates"></a><span data-ttu-id="d2647-114">Šablony organizace integration pack</span><span class="sxs-lookup"><span data-stu-id="d2647-114">Enterprise integration pack templates</span></span>
<span data-ttu-id="d2647-115">Konfigurace VETER (ověřit, extrakce, transformace, zlepšit komunikaci oddělení, směrovat) kanály, přijetí X12 EDI dokumentu přes AS2 a její transformace tooXML, stejně jako zprávy X12 a AS2 zpracování.</span><span class="sxs-lookup"><span data-stu-id="d2647-115">Configurations of VETER (validate, extract, transform, enrich, route) pipelines, receiving an X12 EDI document over AS2 and transforming it tooXML, as well as X12 and AS2 message handling.</span></span>

### <a name="protocol-pattern-templates"></a><span data-ttu-id="d2647-116">Protokol vzor šablony</span><span class="sxs-lookup"><span data-stu-id="d2647-116">Protocol pattern templates</span></span>
<span data-ttu-id="d2647-117">Tyto šablony se skládají logic Apps, které obsahují vzory protokolu, jako je požadavků a odpovědí prostřednictvím protokolu HTTP, jakož i integrace napříč FTP a SFTP.</span><span class="sxs-lookup"><span data-stu-id="d2647-117">These templates consist of logic apps that contain protocol patterns such as request-response over HTTP as well as integrations across FTP and SFTP.</span></span> <span data-ttu-id="d2647-118">Použijte tyto, které jsou, nebo jako základ pro vytváření složitějších vzory protokolu.</span><span class="sxs-lookup"><span data-stu-id="d2647-118">Use these as they exist, or as a basis for creating more complex protocol patterns.</span></span>  

### <a name="personal-productivity-templates"></a><span data-ttu-id="d2647-119">Osobních kancelářských šablony</span><span class="sxs-lookup"><span data-stu-id="d2647-119">Personal productivity templates</span></span>
<span data-ttu-id="d2647-120">Vzory toohelp zvýšení produktivity osobní zahrnují šablony, které nastavit denní připomenutí, zapněte důležité pracovní položky do seznamu úkolů a automatizaci úloh zdlouhavé dolů krok schválení tooa jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="d2647-120">Patterns toohelp improve personal productivity include templates that set daily reminders, turn important work items into to-do lists, and automate lengthy tasks down tooa single user approval step.</span></span>

### <a name="consumer-cloud-templates"></a><span data-ttu-id="d2647-121">Šablony příjemce cloudu</span><span class="sxs-lookup"><span data-stu-id="d2647-121">Consumer cloud templates</span></span>
<span data-ttu-id="d2647-122">Jednoduché šablony, které integrovat služby sociálních médií jako Twitter, Slack a e-mailu, nakonec schopná posílení sociálních médií marketingové iniciativy.</span><span class="sxs-lookup"><span data-stu-id="d2647-122">Simple templates that integrate with social media services such as Twitter, Slack, and email, ultimately capable of strengthening social media marketing initiatives.</span></span> <span data-ttu-id="d2647-123">Patří sem také šablony například zakalená kopírování, které může pomoci zvýšit produktivitu uložením čas strávený tradičně opakovaných úloh.</span><span class="sxs-lookup"><span data-stu-id="d2647-123">These also include templates such as cloudy copying, which can help increase productivity by saving time spent on traditionally repetitive tasks.</span></span> 

## <a name="how-toocreate-a-logic-app-using-a-template"></a><span data-ttu-id="d2647-124">Jak toocreate aplikace logiky pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="d2647-124">How toocreate a logic app using a template</span></span>
<span data-ttu-id="d2647-125">tooget spuštění pomocí šablony aplikace logiky, přejděte do hello logiku aplikace designer.</span><span class="sxs-lookup"><span data-stu-id="d2647-125">tooget started using a logic app template, go into hello logic app designer.</span></span> <span data-ttu-id="d2647-126">Pokud zadáváte hello Návrhář otevřením existující aplikace logiky, aplikaci logiky hello automaticky načte v návrháře zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d2647-126">If you're entering hello designer by opening an existing logic app, hello logic app automatically loads in your designer view.</span></span> <span data-ttu-id="d2647-127">Pokud vytváříte nové aplikace logiky, ale zobrazí úvodní obrazovka níže.</span><span class="sxs-lookup"><span data-stu-id="d2647-127">However, if you're creating a new logic app, you see hello screen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

<span data-ttu-id="d2647-128">Prostřednictvím této obrazovky můžete buď toostart pomocí prázdné logiku aplikace nebo předem připravených šablon.</span><span class="sxs-lookup"><span data-stu-id="d2647-128">From this screen, you can either choose toostart with a blank logic app or a pre-built template.</span></span> <span data-ttu-id="d2647-129">Pokud vyberete jednu z hello šablon, jsou k dispozici další informace.</span><span class="sxs-lookup"><span data-stu-id="d2647-129">If you select one of hello templates, you are provided with additional information.</span></span> <span data-ttu-id="d2647-130">V tomto příkladu používáme hello *v Dropbox je vytvořen nový soubor, zkopírujte jej tooOneDrive* šablony.</span><span class="sxs-lookup"><span data-stu-id="d2647-130">In this example, we use hello *When a new file is created in Dropbox, copy it tooOneDrive* template.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

<span data-ttu-id="d2647-131">Pokud si zvolíte toouse hello šablony, vyberte možnost hello *tuto šablonu použít* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2647-131">If you choose toouse hello template, just select hello *use this template* button.</span></span> <span data-ttu-id="d2647-132">Zobrazí se výzva toosign v účtech tooyour podle využívá šablonu hello konektory.</span><span class="sxs-lookup"><span data-stu-id="d2647-132">You'll be asked toosign in tooyour accounts based on which connectors hello template utilizes.</span></span> <span data-ttu-id="d2647-133">Nebo, pokud jste dříve vytvořili připojení pomocí těchto konektorů, můžete si vybrat pokračovat, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="d2647-133">Or, if you've previously established a connection with these connectors, you can select continue as seen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

<span data-ttu-id="d2647-134">Po vytvoření připojení hello a výběrem *pokračovat*, aplikace logiky hello se otevře v návrháře zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d2647-134">After establishing hello connection and selecting *continue*, hello logic app opens in designer view.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

<span data-ttu-id="d2647-135">V předchozím příkladu hello stejně jako hello případě mnoho šablony, některá pole povinná vlastnost hello může doplnit v rámci hello konektory; ale některé může stále vyžadují hodnotu bylo možné tooproperly nasazení aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="d2647-135">In hello example above, as is hello case with many templates, some of hello mandatory property fields may be filled out within hello connectors; however, some might still require a value before being able tooproperly deploy hello logic app.</span></span> <span data-ttu-id="d2647-136">Pokud se pokusíte toodeploy bez zadávání některé hello chybějící pole, zobrazí se upozornění s chybovou zprávou.</span><span class="sxs-lookup"><span data-stu-id="d2647-136">If you try toodeploy without entering some of hello missing fields, you'll be notified with an error message.</span></span>

<span data-ttu-id="d2647-137">Pokud chcete tooreturn toohello šablony prohlížeč, vyberte hello *šablony* tlačítko v horním navigačním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="d2647-137">If you wish tooreturn toohello template viewer, select hello *Templates* button in hello top navigation bar.</span></span> <span data-ttu-id="d2647-138">Přepnutím back toohello šablony prohlížeč ztratit všechny neuložené průběh.</span><span class="sxs-lookup"><span data-stu-id="d2647-138">By switching back toohello template viewer, you lose any unsaved progress.</span></span> <span data-ttu-id="d2647-139">Předchozí tooswitching zpět do šablony prohlížeče se zobrazí upozornění zpráva s upozorněním tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="d2647-139">Prior tooswitching back into template viewer, you'll see a warning message notifying you of this.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-toodeploy-a-logic-app-created-from-a-template"></a><span data-ttu-id="d2647-140">Jak toodeploy aplikace logiky vytvořené ze šablony</span><span class="sxs-lookup"><span data-stu-id="d2647-140">How toodeploy a logic app created from a template</span></span>
<span data-ttu-id="d2647-141">Po načtení šablony a všechny požadované změny, vyberte v levém horním rohu hello hello tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="d2647-141">Once you have loaded your template and made any desired changes, select hello save button in hello upper left corner.</span></span> <span data-ttu-id="d2647-142">Tím se uloží a publikuje aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d2647-142">This saves and publishes your logic app.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

<span data-ttu-id="d2647-143">Pokud by jako další informace o tom, jak tooadd další kroky do stávající šablony aplikace logiky nebo proveďte úpravy obecně, přečtěte si informace v [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d2647-143">If you would like more information on how tooadd more steps into an existing logic app template, or make edits in general, read more at [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

