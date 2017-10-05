---
title: "Vytváření pracovních postupů z šablon - Azure Logic Apps | Microsoft Docs"
description: "Začínáme – rychlé vytváření pracovních postupů pomocí šablony aplikace logiky Azure k připojení aplikace a integrovat data."
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
ms.openlocfilehash: 89272869f7dfaa34cbd2ad32d67dca0955e6158b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-to-get-started-quickly"></a><span data-ttu-id="89760-103">Konfigurace pracovního postupu pomocí předdefinovaných šablon nebo vzor rychle začít</span><span class="sxs-lookup"><span data-stu-id="89760-103">Configure a workflow using a pre-built template or pattern to get started quickly</span></span>

## <a name="what-are-logic-app-templates"></a><span data-ttu-id="89760-104">Co jsou šablony aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="89760-104">What are logic app templates</span></span>
<span data-ttu-id="89760-105">Šablona aplikace logiky je předdefinovaných logiku aplikace, která vám pomůže rychle začít vytvářet své vlastní pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="89760-105">A logic app template is a pre-built logic app that you can use to quickly get started creating your own workflow.</span></span> 

<span data-ttu-id="89760-106">Tyto šablony jsou vhodný způsob, jak zjistit různých vzorů, které mohou být vytvořeny pomocí aplikací logiky.</span><span class="sxs-lookup"><span data-stu-id="89760-106">These templates are a good way to discover various patterns that can be built using logic apps.</span></span> <span data-ttu-id="89760-107">Můžete použít tyto šablony jako-je nebo upravit tak, aby vyhovovaly vašemu scénáři.</span><span class="sxs-lookup"><span data-stu-id="89760-107">You can either use these templates as-is or modify them to fit your scenario.</span></span>

## <a name="overview-of-available-templates"></a><span data-ttu-id="89760-108">Přehled dostupných šablon</span><span class="sxs-lookup"><span data-stu-id="89760-108">Overview of available templates</span></span>
<span data-ttu-id="89760-109">Existuje mnoho dostupných šablon aktuálně publikované v platformě aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="89760-109">There are many available templates currently published in the logic app platform.</span></span> <span data-ttu-id="89760-110">Některé kategorie příklad, stejně jako typy konektorů použít v nich, jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="89760-110">Some example categories, as well as the type of connectors used in them, are listed below.</span></span>

### <a name="enterprise-cloud-templates"></a><span data-ttu-id="89760-111">Šablony organizace cloudu</span><span class="sxs-lookup"><span data-stu-id="89760-111">Enterprise cloud templates</span></span>
<span data-ttu-id="89760-112">Šablony, které se integrují Dynamics CRM Salesforce, pole, objektů Blob v Azure a ostatní konektory pro vaše potřeby enterprise cloud.</span><span class="sxs-lookup"><span data-stu-id="89760-112">Templates that integrate Dynamics CRM, Salesforce, Box, Azure Blob, and other connectors for your enterprise cloud needs.</span></span> <span data-ttu-id="89760-113">Některé příklady z co můžete udělat pomocí těchto šablony zahrnují uspořádání zájemců a zálohování dat podnikových souborů.</span><span class="sxs-lookup"><span data-stu-id="89760-113">Some examples of what can be done with these templates include organizing your leads and backing up your corporate file data.</span></span>

### <a name="enterprise-integration-pack-templates"></a><span data-ttu-id="89760-114">Šablony organizace integration pack</span><span class="sxs-lookup"><span data-stu-id="89760-114">Enterprise integration pack templates</span></span>
<span data-ttu-id="89760-115">Konfigurace VETER (ověřit, extrakce, transformace, zlepšit komunikaci oddělení, směrovat) kanály, přijetí X12 EDI dokumentu přes AS2 a převod do formátu XML, stejně jako zprávy X12 a AS2 zpracování.</span><span class="sxs-lookup"><span data-stu-id="89760-115">Configurations of VETER (validate, extract, transform, enrich, route) pipelines, receiving an X12 EDI document over AS2 and transforming it to XML, as well as X12 and AS2 message handling.</span></span>

### <a name="protocol-pattern-templates"></a><span data-ttu-id="89760-116">Protokol vzor šablony</span><span class="sxs-lookup"><span data-stu-id="89760-116">Protocol pattern templates</span></span>
<span data-ttu-id="89760-117">Tyto šablony se skládají logic Apps, které obsahují vzory protokolu, jako je požadavků a odpovědí prostřednictvím protokolu HTTP, jakož i integrace napříč FTP a SFTP.</span><span class="sxs-lookup"><span data-stu-id="89760-117">These templates consist of logic apps that contain protocol patterns such as request-response over HTTP as well as integrations across FTP and SFTP.</span></span> <span data-ttu-id="89760-118">Použijte tyto, které jsou, nebo jako základ pro vytváření složitějších vzory protokolu.</span><span class="sxs-lookup"><span data-stu-id="89760-118">Use these as they exist, or as a basis for creating more complex protocol patterns.</span></span>  

### <a name="personal-productivity-templates"></a><span data-ttu-id="89760-119">Osobních kancelářských šablony</span><span class="sxs-lookup"><span data-stu-id="89760-119">Personal productivity templates</span></span>
<span data-ttu-id="89760-120">Vzory k vylepšení osobních kancelářských zahrnují šablony, které nastavit denní připomenutí, zapněte důležité pracovní položky do seznamu úkolů a automatizaci úloh zdlouhavé dolů krok schválení jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="89760-120">Patterns to help improve personal productivity include templates that set daily reminders, turn important work items into to-do lists, and automate lengthy tasks down to a single user approval step.</span></span>

### <a name="consumer-cloud-templates"></a><span data-ttu-id="89760-121">Šablony příjemce cloudu</span><span class="sxs-lookup"><span data-stu-id="89760-121">Consumer cloud templates</span></span>
<span data-ttu-id="89760-122">Jednoduché šablony, které integrovat služby sociálních médií jako Twitter, Slack a e-mailu, nakonec schopná posílení sociálních médií marketingové iniciativy.</span><span class="sxs-lookup"><span data-stu-id="89760-122">Simple templates that integrate with social media services such as Twitter, Slack, and email, ultimately capable of strengthening social media marketing initiatives.</span></span> <span data-ttu-id="89760-123">Patří sem také šablony například zakalená kopírování, které může pomoci zvýšit produktivitu uložením čas strávený tradičně opakovaných úloh.</span><span class="sxs-lookup"><span data-stu-id="89760-123">These also include templates such as cloudy copying, which can help increase productivity by saving time spent on traditionally repetitive tasks.</span></span> 

## <a name="how-to-create-a-logic-app-using-a-template"></a><span data-ttu-id="89760-124">Postup vytvoření aplikace logiky pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="89760-124">How to create a logic app using a template</span></span>
<span data-ttu-id="89760-125">Chcete-li začít používat šablonu aplikace logiky, přejděte do návrháře logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="89760-125">To get started using a logic app template, go into the logic app designer.</span></span> <span data-ttu-id="89760-126">Pokud zadáváte návrháře otevřením existující aplikace logiky, aplikaci logiky automaticky načte v návrháře zobrazení.</span><span class="sxs-lookup"><span data-stu-id="89760-126">If you're entering the designer by opening an existing logic app, the logic app automatically loads in your designer view.</span></span> <span data-ttu-id="89760-127">Pokud vytváříte novou aplikaci logiky, ale zobrazí na obrazovce níže.</span><span class="sxs-lookup"><span data-stu-id="89760-127">However, if you're creating a new logic app, you see the screen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

<span data-ttu-id="89760-128">Prostřednictvím této obrazovky buď můžete začít s prázdnou logiku aplikace nebo předem připravených šablon.</span><span class="sxs-lookup"><span data-stu-id="89760-128">From this screen, you can either choose to start with a blank logic app or a pre-built template.</span></span> <span data-ttu-id="89760-129">Pokud vyberete jednu z šablon, jsou k dispozici další informace.</span><span class="sxs-lookup"><span data-stu-id="89760-129">If you select one of the templates, you are provided with additional information.</span></span> <span data-ttu-id="89760-130">V tomto příkladu používáme *v Dropbox je vytvořen nový soubor, zkopírujte jej do OneDrive* šablony.</span><span class="sxs-lookup"><span data-stu-id="89760-130">In this example, we use the *When a new file is created in Dropbox, copy it to OneDrive* template.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

<span data-ttu-id="89760-131">Pokud se rozhodnete použít šablonu, stačí vybrat *tuto šablonu použít* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="89760-131">If you choose to use the template, just select the *use this template* button.</span></span> <span data-ttu-id="89760-132">Budete vyzváni k přihlášení do svých účtů na základě na konektory, které využívá šablony.</span><span class="sxs-lookup"><span data-stu-id="89760-132">You'll be asked to sign in to your accounts based on which connectors the template utilizes.</span></span> <span data-ttu-id="89760-133">Nebo, pokud jste dříve vytvořili připojení pomocí těchto konektorů, můžete si vybrat pokračovat, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="89760-133">Or, if you've previously established a connection with these connectors, you can select continue as seen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

<span data-ttu-id="89760-134">Po navázání připojení a výběrem *pokračovat*, otevře aplikaci logiky v návrháře zobrazení.</span><span class="sxs-lookup"><span data-stu-id="89760-134">After establishing the connection and selecting *continue*, the logic app opens in designer view.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

<span data-ttu-id="89760-135">V příkladu nahoře jako je tomu u mnoha šablony, některá pole povinná vlastnost může doplnit v rámci konektory; ale některé může stále vyžadují hodnotu bylo možné správně nasadit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="89760-135">In the example above, as is the case with many templates, some of the mandatory property fields may be filled out within the connectors; however, some might still require a value before being able to properly deploy the logic app.</span></span> <span data-ttu-id="89760-136">Pokud se pokusíte nasadit bez zadávání některá pole chybí, zobrazí se upozornění s chybovou zprávou.</span><span class="sxs-lookup"><span data-stu-id="89760-136">If you try to deploy without entering some of the missing fields, you'll be notified with an error message.</span></span>

<span data-ttu-id="89760-137">Pokud chcete vrátit do prohlížeče šablony, vyberte *šablony* tlačítko v horním navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="89760-137">If you wish to return to the template viewer, select the *Templates* button in the top navigation bar.</span></span> <span data-ttu-id="89760-138">Přepnutím zpět do prohlížeče šablony ztratit všechny neuložené průběh.</span><span class="sxs-lookup"><span data-stu-id="89760-138">By switching back to the template viewer, you lose any unsaved progress.</span></span> <span data-ttu-id="89760-139">Před přepnout zpět do prohlížeče šablony, se zobrazí upozornění zpráva s upozorněním tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="89760-139">Prior to switching back into template viewer, you'll see a warning message notifying you of this.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a><span data-ttu-id="89760-140">Postup nasazení aplikace logiky vytvořené ze šablony</span><span class="sxs-lookup"><span data-stu-id="89760-140">How to deploy a logic app created from a template</span></span>
<span data-ttu-id="89760-141">Po načtení šablony a všechny požadované změny, vyberte uložení tlačítko v levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="89760-141">Once you have loaded your template and made any desired changes, select the save button in the upper left corner.</span></span> <span data-ttu-id="89760-142">Tím se uloží a publikuje aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="89760-142">This saves and publishes your logic app.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

<span data-ttu-id="89760-143">Pokud vás zajímají další informace o tom, jak přidat další kroky do stávající šablony aplikace logiky, nebo proveďte úpravy obecně, přečtěte si informace v [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="89760-143">If you would like more information on how to add more steps into an existing logic app template, or make edits in general, read more at [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

