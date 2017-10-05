---
title: "Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "Další informace o všech funkcí, které jsou k dispozici v nástroji pro modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 621ff305d7e782f85eeaae6c3fb02031673549c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="threat-modeling-tool-feature-overview"></a><span data-ttu-id="e47e4-103">Přehled funkcí nástroje modelování hrozeb</span><span class="sxs-lookup"><span data-stu-id="e47e4-103">Threat Modeling Tool feature overview</span></span>

<span data-ttu-id="e47e4-104">Jsme rádi, že jste se rozhodli použít nástroj modelování hrozeb pro vaše threat modelování potřebám!</span><span class="sxs-lookup"><span data-stu-id="e47e4-104">We are glad you chose to use the Threat Modeling Tool for your threat modeling needs!</span></span> <span data-ttu-id="e47e4-105">Pokud jste tak dosud neučinili, navštivte  **[Začínáme s nástrojem modelování hrozeb](./azure-security-threat-modeling-tool-getting-started.md)**  základní informace.</span><span class="sxs-lookup"><span data-stu-id="e47e4-105">If you haven’t done so, visit **[Getting Started with the Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md)** to learn the basics.</span></span>

> <span data-ttu-id="e47e4-106">Naše nástroj se často aktualizuje, proto tento často najdete v příručce pro naše nejnovější funkce a vylepšení.</span><span class="sxs-lookup"><span data-stu-id="e47e4-106">Our tool is updated often, so check this guide often to see our latest features and improvements.</span></span>

<span data-ttu-id="e47e4-107">Kliknutím na tlačítko "Vytvořit nový Model" otevře prázdné úvodní stránka, podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="e47e4-107">Clicking on the "Create a New Model" button opens a blank start page, similar to the image below:</span></span>

![Prázdné úvodní stránky](./media/azure-security-threat-modeling-tool/tmtstart.png)

<span data-ttu-id="e47e4-109">Pomocí model hrozeb vytvořené náš tým v  **[Začínáme](./azure-security-threat-modeling-tool-getting-started.md)**  příkladu budeme najdete všechny funkce, které jsou k dispozici v nástroji ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="e47e4-109">Using the threat model created by our team in the **[Getting Started](./azure-security-threat-modeling-tool-getting-started.md)** example, let’s check out all the features available in the tool today.</span></span>

![Model základní hrozeb](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a><span data-ttu-id="e47e4-111">Navigace</span><span class="sxs-lookup"><span data-stu-id="e47e4-111">Navigation</span></span>

<span data-ttu-id="e47e4-112">Než začnete integrované funkce, přejděte přes hlavními součástmi najít v nástroji</span><span class="sxs-lookup"><span data-stu-id="e47e4-112">Before diving into the built-in features, let’s go over the main components found in the tool</span></span>

### <a name="menu-items"></a><span data-ttu-id="e47e4-113">Položky nabídky</span><span class="sxs-lookup"><span data-stu-id="e47e4-113">Menu items</span></span>

<span data-ttu-id="e47e4-114">Prostředí by měl vypadat přibližně další produkty společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e47e4-114">The experience should be similar to other Microsoft products.</span></span> <span data-ttu-id="e47e4-115">Začněme prostřednictvím položky nabídky nejvyšší úrovně:</span><span class="sxs-lookup"><span data-stu-id="e47e4-115">Let’s begin by going through the top-level menu items:</span></span>

![Položky nabídky](./media/azure-security-threat-modeling-tool/menuitems.png)

| <span data-ttu-id="e47e4-117">Štítek</span><span class="sxs-lookup"><span data-stu-id="e47e4-117">Label</span></span>                               | <span data-ttu-id="e47e4-118">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-118">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e47e4-119">**File**</span><span class="sxs-lookup"><span data-stu-id="e47e4-119">**File**</span></span> | <ul><li><span data-ttu-id="e47e4-120">Otevřít, uložte a zavřete soubory</span><span class="sxs-lookup"><span data-stu-id="e47e4-120">Open, Save and Close Files</span></span></li><li><span data-ttu-id="e47e4-121">Účty přihlášení volitelném OneDrive</span><span class="sxs-lookup"><span data-stu-id="e47e4-121">Sign In/Out of OneDrive accounts</span></span></li><li><span data-ttu-id="e47e4-122">Sdílet odkazy (zobrazení a úpravy)</span><span class="sxs-lookup"><span data-stu-id="e47e4-122">Share Links (View + Edit)</span></span></li><li><span data-ttu-id="e47e4-123">Zobrazení informací o souboru</span><span class="sxs-lookup"><span data-stu-id="e47e4-123">View File Information</span></span></li><li><span data-ttu-id="e47e4-124">Použít novou šablonu na existující modely</span><span class="sxs-lookup"><span data-stu-id="e47e4-124">Apply New Template to Existing Models</span></span></li></ul> |
| <span data-ttu-id="e47e4-125">**Upravit**</span><span class="sxs-lookup"><span data-stu-id="e47e4-125">**Edit**</span></span> | <span data-ttu-id="e47e4-126">Zpět/opakování akce, jako dobře kopii, vkládání a odstraňování</span><span class="sxs-lookup"><span data-stu-id="e47e4-126">Undo/redo actions, as well a copy, paste and delete</span></span> |
| <span data-ttu-id="e47e4-127">**Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="e47e4-127">**View**</span></span> | <ul><li><span data-ttu-id="e47e4-128">Přepínání mezi **Analysis** a **návrhu** zobrazení</span><span class="sxs-lookup"><span data-stu-id="e47e4-128">Switch between **Analysis** and **Design** views</span></span></li><li><span data-ttu-id="e47e4-129">Otevřete uzavřené windows (e.g.stencils, vlastností elementů a zprávy)</span><span class="sxs-lookup"><span data-stu-id="e47e4-129">Open closed windows (e.g.stencils, element properties and messages)</span></span></li><li><span data-ttu-id="e47e4-130">Obnovit výchozí nastavení rozložení</span><span class="sxs-lookup"><span data-stu-id="e47e4-130">Reset layout to default settings</span></span></li></ul> |
| <span data-ttu-id="e47e4-131">**Diagram**</span><span class="sxs-lookup"><span data-stu-id="e47e4-131">**Diagram**</span></span> | <span data-ttu-id="e47e4-132">Přidání nebo odstranění diagramy a procházení "karty" diagramů</span><span class="sxs-lookup"><span data-stu-id="e47e4-132">Add/Delete diagrams and navigate through “tabs” of diagrams</span></span> |
| <span data-ttu-id="e47e4-133">**Sestavy**</span><span class="sxs-lookup"><span data-stu-id="e47e4-133">**Reports**</span></span> | <span data-ttu-id="e47e4-134">Vytváření sestav HTML sdílet s ostatními uživateli</span><span class="sxs-lookup"><span data-stu-id="e47e4-134">Create HTML reports to share with others</span></span> |
| <span data-ttu-id="e47e4-135">**Pomoc**</span><span class="sxs-lookup"><span data-stu-id="e47e4-135">**Help**</span></span> | <span data-ttu-id="e47e4-136">Provede můžete použít nástroj</span><span class="sxs-lookup"><span data-stu-id="e47e4-136">Guides to help you use the tool</span></span> |

<span data-ttu-id="e47e4-137">Ikony jsou zkratky pro nejvyšší úrovně nabídky:</span><span class="sxs-lookup"><span data-stu-id="e47e4-137">The icons are shortcuts for the top-level menus:</span></span>

| <span data-ttu-id="e47e4-138">Ikona</span><span class="sxs-lookup"><span data-stu-id="e47e4-138">Icon</span></span>                               | <span data-ttu-id="e47e4-139">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-139">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e47e4-140">**Otevřete**</span><span class="sxs-lookup"><span data-stu-id="e47e4-140">**Open**</span></span> | <span data-ttu-id="e47e4-141">Otevře se nový soubor</span><span class="sxs-lookup"><span data-stu-id="e47e4-141">Opens a new file</span></span> |
| <span data-ttu-id="e47e4-142">**Uložit**</span><span class="sxs-lookup"><span data-stu-id="e47e4-142">**Save**</span></span> | <span data-ttu-id="e47e4-143">Uloží aktuální soubor</span><span class="sxs-lookup"><span data-stu-id="e47e4-143">Saves current file</span></span> |
| <span data-ttu-id="e47e4-144">**Návrh**</span><span class="sxs-lookup"><span data-stu-id="e47e4-144">**Design**</span></span> | <span data-ttu-id="e47e4-145">Klient se přepne do zobrazení návrhu, kde můžete vytvořit modely</span><span class="sxs-lookup"><span data-stu-id="e47e4-145">Goes into design view, where you can create models</span></span> |
| <span data-ttu-id="e47e4-146">**Analýza**</span><span class="sxs-lookup"><span data-stu-id="e47e4-146">**Analyze**</span></span> | <span data-ttu-id="e47e4-147">Ukazuje generované hrozby a jejich vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-147">Shows generated threats and their properties</span></span> |
| <span data-ttu-id="e47e4-148">**Přidat Diagram**</span><span class="sxs-lookup"><span data-stu-id="e47e4-148">**Add Diagram**</span></span> | <span data-ttu-id="e47e4-149">Přidá nový diagram (podobně jako nové karty v aplikaci Excel)</span><span class="sxs-lookup"><span data-stu-id="e47e4-149">Adds new diagram (similar to new tabs in Excel)</span></span> |
| <span data-ttu-id="e47e4-150">**Odstranit Diagram**</span><span class="sxs-lookup"><span data-stu-id="e47e4-150">**Delete Diagram**</span></span> | <span data-ttu-id="e47e4-151">Odstraní aktuální diagram</span><span class="sxs-lookup"><span data-stu-id="e47e4-151">Deletes current diagram</span></span> |
| <span data-ttu-id="e47e4-152">**Vyjmutí/kopírování/vkládání**</span><span class="sxs-lookup"><span data-stu-id="e47e4-152">**Copy/Cut/Paste**</span></span> | <span data-ttu-id="e47e4-153">Kopie nebo kusy/vloží elementy</span><span class="sxs-lookup"><span data-stu-id="e47e4-153">Copies/cuts/pastes elements</span></span> |
| <span data-ttu-id="e47e4-154">**Vrátit/opakovat**</span><span class="sxs-lookup"><span data-stu-id="e47e4-154">**Undo/Redo**</span></span> | <span data-ttu-id="e47e4-155">Vrátí zpět nebo znovu provede akce</span><span class="sxs-lookup"><span data-stu-id="e47e4-155">Undoes/redoes actions</span></span> |
| <span data-ttu-id="e47e4-156">**Přiblížení / oddálení**</span><span class="sxs-lookup"><span data-stu-id="e47e4-156">**Zoom In/Zoom Out**</span></span> | <span data-ttu-id="e47e4-157">Zvětší směřující diagram pro lepší zobrazení</span><span class="sxs-lookup"><span data-stu-id="e47e4-157">Zooms in and out of the diagram for a better view</span></span> |
| <span data-ttu-id="e47e4-158">**Váš názor**</span><span class="sxs-lookup"><span data-stu-id="e47e4-158">**Feedback**</span></span> | <span data-ttu-id="e47e4-159">Otevře na fóru MSDN</span><span class="sxs-lookup"><span data-stu-id="e47e4-159">Opens the MSDN Forum</span></span> |

### <a name="canvas"></a><span data-ttu-id="e47e4-160">Plátno</span><span class="sxs-lookup"><span data-stu-id="e47e4-160">Canvas</span></span>

<span data-ttu-id="e47e4-161">Místa, kde můžete přetáhnout myší elementy do.</span><span class="sxs-lookup"><span data-stu-id="e47e4-161">The space where you drag and drop elements into.</span></span> <span data-ttu-id="e47e4-162">Přetažení je nejrychlejší a nejúčinnější způsob, jak vytvářet modely.</span><span class="sxs-lookup"><span data-stu-id="e47e4-162">Drag and drop is the quickest and most efficient way to build models.</span></span> <span data-ttu-id="e47e4-163">Může také klikněte pravým tlačítkem a vyberte v nabídce, která přidává obecné verzích prvky, které používáte, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="e47e4-163">You may also right click and select from the menu, which adds generic versions of the elements you’re using, as shown below.</span></span>

#### <a name="dropping-the-stencil-on-the-canvas"></a><span data-ttu-id="e47e4-164">Vyřazení vzorníku na plátno</span><span class="sxs-lookup"><span data-stu-id="e47e4-164">Dropping the stencil on the canvas</span></span>

![Vyřaďte plátno](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-the-stencil"></a><span data-ttu-id="e47e4-166">Kliknutím na vzorníku</span><span class="sxs-lookup"><span data-stu-id="e47e4-166">Clicking on the stencil</span></span>

![Vlastnosti elementů](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a><span data-ttu-id="e47e4-168">Vzorníky</span><span class="sxs-lookup"><span data-stu-id="e47e4-168">Stencils</span></span>

<span data-ttu-id="e47e4-169">Kde můžete najít všechny vzorníky k dispozici pro použití podle vybraná šablona.</span><span class="sxs-lookup"><span data-stu-id="e47e4-169">Where you can find all stencils available to use based on the template selected.</span></span> <span data-ttu-id="e47e4-170">Pokud nemůžete najít správné elementy, zkuste použít jinou šablonu nebo měnit podle vlastních potřeb.</span><span class="sxs-lookup"><span data-stu-id="e47e4-170">If you can’t find the right elements, try using another template, or modify one to fit your needs.</span></span> <span data-ttu-id="e47e4-171">Obecně byste měli nemůže najít kombinaci kategorie, jako jsou následující:</span><span class="sxs-lookup"><span data-stu-id="e47e4-171">Generally, you should be able to find a combination of categories like the ones below:</span></span>

| <span data-ttu-id="e47e4-172">Název vzorníku</span><span class="sxs-lookup"><span data-stu-id="e47e4-172">Stencil Name</span></span>                               | <span data-ttu-id="e47e4-173">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-173">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e47e4-174">**Proces**</span><span class="sxs-lookup"><span data-stu-id="e47e4-174">**Process**</span></span> | <span data-ttu-id="e47e4-175">Aplikace, moduly plug-in prohlížeče, vláken, virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="e47e4-175">Applications, Browser Plugins, Threads, Virtual Machines</span></span> |
| <span data-ttu-id="e47e4-176">**Externí interakce**</span><span class="sxs-lookup"><span data-stu-id="e47e4-176">**External Interactor**</span></span> | <span data-ttu-id="e47e4-177">Zprostředkovatelé ověřování, prohlížečů, uživatelé, webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e47e4-177">Authentication Providers, Browsers, Users, Web Applications</span></span> |
| <span data-ttu-id="e47e4-178">**Úložiště dat**</span><span class="sxs-lookup"><span data-stu-id="e47e4-178">**Data Store**</span></span> | <span data-ttu-id="e47e4-179">Mezipaměť, úložiště, konfigurační soubory, databáze, registru</span><span class="sxs-lookup"><span data-stu-id="e47e4-179">Cache, Storage, Configuration Files, Databases, Registry</span></span> |
| <span data-ttu-id="e47e4-180">**Tok dat**</span><span class="sxs-lookup"><span data-stu-id="e47e4-180">**Data Flow**</span></span> | <span data-ttu-id="e47e4-181">Binární, ALPC, HTTP, HTTPS, protokol TLS/SSL, IOCTL, IPSec, s názvem kanálu, RPC/DCOM, protokol SMB, UDP</span><span class="sxs-lookup"><span data-stu-id="e47e4-181">Binary, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP</span></span> |
| <span data-ttu-id="e47e4-182">**Vztah důvěryhodnosti hranic čáry či ohraničení**</span><span class="sxs-lookup"><span data-stu-id="e47e4-182">**Trust Line/Border Boundary**</span></span> | <span data-ttu-id="e47e4-183">Podnikové sítě, Internet, počítače, izolovaného prostoru, režimu uživatele nebo jádra</span><span class="sxs-lookup"><span data-stu-id="e47e4-183">Corporate Networks, Internet, Machine, Sandbox, User/Kernel Mode</span></span> |

### <a name="notesmessages"></a><span data-ttu-id="e47e4-184">Poznámky a zprávy</span><span class="sxs-lookup"><span data-stu-id="e47e4-184">Notes/Messages</span></span>

| <span data-ttu-id="e47e4-185">Komponenta</span><span class="sxs-lookup"><span data-stu-id="e47e4-185">Component</span></span>                               | <span data-ttu-id="e47e4-186">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-186">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e47e4-187">**Zprávy**</span><span class="sxs-lookup"><span data-stu-id="e47e4-187">**Messages**</span></span> | <span data-ttu-id="e47e4-188">Interní nástroj pro logiku, která upozorní uživatele, vždy, když dojde k chybě, jako je například žádná data proudí mezi elementy</span><span class="sxs-lookup"><span data-stu-id="e47e4-188">Internal tool logic that alerts users whenever there is an error, such as no data flows between elements</span></span> |
| <span data-ttu-id="e47e4-189">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="e47e4-189">**Notes**</span></span> | <span data-ttu-id="e47e4-190">Ruční poznámky přidaných do souboru vývojové týmy během celého procesu návrhu a revize</span><span class="sxs-lookup"><span data-stu-id="e47e4-190">Manual notes added to the file by engineering teams throughout the design and review process</span></span> |

### <a name="element-properties"></a><span data-ttu-id="e47e4-191">Vlastnosti elementů</span><span class="sxs-lookup"><span data-stu-id="e47e4-191">Element properties</span></span>

<span data-ttu-id="e47e4-192">To se liší podle vybrané elementy.</span><span class="sxs-lookup"><span data-stu-id="e47e4-192">These vary by the elements selected.</span></span> <span data-ttu-id="e47e4-193">Kromě hranice vztahu důvěryhodnosti všechny ostatní elementy obsahovat 3 obecné možnosti:</span><span class="sxs-lookup"><span data-stu-id="e47e4-193">Apart from Trust Boundaries, all other elements contain 3 general selections:</span></span>

| <span data-ttu-id="e47e4-194">Vlastnost element</span><span class="sxs-lookup"><span data-stu-id="e47e4-194">Element Property</span></span>                               | <span data-ttu-id="e47e4-195">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-195">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e47e4-196">**Název**</span><span class="sxs-lookup"><span data-stu-id="e47e4-196">**Name**</span></span> | <span data-ttu-id="e47e4-197">Užitečné pro pojmenování vaší procesů, úložiště, interactors a toky snadno rozpoznat</span><span class="sxs-lookup"><span data-stu-id="e47e4-197">Useful for naming your processes, stores, interactors and flows to be easily recognized</span></span> |
| <span data-ttu-id="e47e4-198">**Mimo rozsah**</span><span class="sxs-lookup"><span data-stu-id="e47e4-198">**Out of Scope**</span></span> | <span data-ttu-id="e47e4-199">Pokud vyberete, element se dostala mimo matici hrozbu generace (nedoporučuje se)</span><span class="sxs-lookup"><span data-stu-id="e47e4-199">If selected, the element is taken out of the threat generation matrix (not recommended)</span></span> |
| <span data-ttu-id="e47e4-200">**Důvod mimo rozsah**</span><span class="sxs-lookup"><span data-stu-id="e47e4-200">**Reason for Out of Scope**</span></span> | <span data-ttu-id="e47e4-201">Při zarovnání do bloku pole umožníte uživatelům vědět, proč mimo obor nebyla vybrána.</span><span class="sxs-lookup"><span data-stu-id="e47e4-201">Justification field to let users know why out of scope was selected</span></span> |

<span data-ttu-id="e47e4-202">Vlastnosti jsou změnit v rámci každé kategorie elementu.</span><span class="sxs-lookup"><span data-stu-id="e47e4-202">Properties are changed under each element category.</span></span> <span data-ttu-id="e47e4-203">Klikněte na každý prvek zkontrolovat dostupné možnosti, nebo otevřete šablonu, kterou chcete získat další informace.</span><span class="sxs-lookup"><span data-stu-id="e47e4-203">Click on each element to inspect the available options, or open the template to learn more.</span></span> <span data-ttu-id="e47e4-204">Pojďme do funkcí.</span><span class="sxs-lookup"><span data-stu-id="e47e4-204">Let’s get into the features.</span></span>

## <a name="welcome-screen"></a><span data-ttu-id="e47e4-205">Úvodní obrazovka</span><span class="sxs-lookup"><span data-stu-id="e47e4-205">Welcome screen</span></span>

<span data-ttu-id="e47e4-206">Na úvodní obrazovce je první věcí, kterou najdete v části při otevření aplikace.</span><span class="sxs-lookup"><span data-stu-id="e47e4-206">The welcome screen is the first thing you see when you open the app.</span></span>

### <a name="open-a-model"></a><span data-ttu-id="e47e4-207">Otevřete model</span><span class="sxs-lookup"><span data-stu-id="e47e4-207">Open A model</span></span>

<span data-ttu-id="e47e4-208">Ukazatele myši na tlačítko "Otevřete modelu" ukazuje možnosti 2 Skrytá: "Otevřete z tento počítač" a "Otevřete z Onedrivu."</span><span class="sxs-lookup"><span data-stu-id="e47e4-208">Hovering over “Open a Model” button shows you 2 hidden options: “Open From this Computer” and “Open from OneDrive.”</span></span> <span data-ttu-id="e47e4-209">První otevře soubor otevřete obrazovku, zatímco druhý vás provede v procesu přihlašování pro OneDrive, umožní vám vybrat složky a soubory. Po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="e47e4-209">The first opens the File Open screen, while the second takes you through the sign in process for OneDrive, allowing you to pick folders and files after a successful authentication.</span></span>

![Otevřete modelu](./media/azure-security-threat-modeling-tool/openmodel.png)

![Otevřete v počítači nebo OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a><span data-ttu-id="e47e4-212">Zpětná vazba, návrhy a problémy</span><span class="sxs-lookup"><span data-stu-id="e47e4-212">Feedback, suggestions and issues</span></span>

<span data-ttu-id="e47e4-213">Výběrem této možnosti se dostanete na fórech MSDN nástroje SDL.</span><span class="sxs-lookup"><span data-stu-id="e47e4-213">Selecting this option will take you to the MSDN Forums for SDL Tools.</span></span> <span data-ttu-id="e47e4-214">Je skvělým způsobem, jak podívejte se na ostatní uživatelé názory týkající se nástroje, včetně řešení a nových nápadů.</span><span class="sxs-lookup"><span data-stu-id="e47e4-214">It’s a great way to check out what other people are saying about the tool, including workarounds and new ideas.</span></span>

![Váš názor](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a><span data-ttu-id="e47e4-216">Návrhové zobrazení</span><span class="sxs-lookup"><span data-stu-id="e47e4-216">Design view</span></span>

<span data-ttu-id="e47e4-217">Při každém otevření nebo vytvořit nový model, budete přesměrováni na zobrazení návrhu.</span><span class="sxs-lookup"><span data-stu-id="e47e4-217">Whenever you open or create a new model, you’ll be taken to the design view.</span></span>

### <a name="adding-elements"></a><span data-ttu-id="e47e4-218">Přidávání elementů</span><span class="sxs-lookup"><span data-stu-id="e47e4-218">Adding elements</span></span>

<span data-ttu-id="e47e4-219">Chcete-li přidat elementů v mřížce 2 způsoby:</span><span class="sxs-lookup"><span data-stu-id="e47e4-219">There are 2 ways to add elements on the grid:</span></span>

- <span data-ttu-id="e47e4-220">**Přetáhnout myší** – přetáhněte požadované elementu k mřížce, potom použijte vlastností elementů můžete poskytnout dodatečné informace.</span><span class="sxs-lookup"><span data-stu-id="e47e4-220">**Drag and Drop** – drag the desired element to the grid, then use the element properties to provide additional information.</span></span>
- <span data-ttu-id="e47e4-221">**Klikněte pravým tlačítkem na** – klikněte pravým tlačítkem kamkoli na mřížky a vyberte z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="e47e4-221">**Right Click** – right click anywhere on the grid and select from the dropdown menu.</span></span> <span data-ttu-id="e47e4-222">Obecná reprezentace tohoto prvku se zobrazí na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="e47e4-222">A generic representation of that element will appear on the screen.</span></span>

### <a name="connecting-elements"></a><span data-ttu-id="e47e4-223">Připojení elementy</span><span class="sxs-lookup"><span data-stu-id="e47e4-223">Connecting elements</span></span>

<span data-ttu-id="e47e4-224">Prvky v nástroji propojit 2 způsoby:</span><span class="sxs-lookup"><span data-stu-id="e47e4-224">There are 2 ways to connect elements in the tool:</span></span>

- <span data-ttu-id="e47e4-225">**Přetáhnout myší** – přetáhněte požadovaného toku dat do mřížky a připojte se k příslušné elementy oba elementy end.</span><span class="sxs-lookup"><span data-stu-id="e47e4-225">**Drag and Drop** – drag the desired dataflow to the grid, and connect both ends to the appropriate elements.</span></span>
- <span data-ttu-id="e47e4-226">**Klikněte na tlačítko + Shift** – klikněte na první prvek (odesílání dat), stiskněte a podržte klávesu Shift a potom vyberte druhý prvkem (přijetí dat).</span><span class="sxs-lookup"><span data-stu-id="e47e4-226">**Click + Shift** – click on the first element (sending data), press and hold the Shift key, then select the second element (receiving data).</span></span> <span data-ttu-id="e47e4-227">Klikněte pravým tlačítkem a vyberte možnost "Připojit".</span><span class="sxs-lookup"><span data-stu-id="e47e4-227">Right click, and select “Connect.”</span></span> <span data-ttu-id="e47e4-228">Pokud používáte toku dat obousměrný, není pořadí jako důležité.</span><span class="sxs-lookup"><span data-stu-id="e47e4-228">If you’re using a bi-directional dataflow, the order is not as important.</span></span>

### <a name="properties"></a><span data-ttu-id="e47e4-229">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e47e4-229">Properties</span></span>

<span data-ttu-id="e47e4-230">Zobrazuje všechny vlastnosti, které je možné upravit v vzorníky umístěny v diagramu.</span><span class="sxs-lookup"><span data-stu-id="e47e4-230">Shows all the properties that can be modified on the stencils placed in the diagram.</span></span> <span data-ttu-id="e47e4-231">Pokud chcete zobrazit vlastnosti, právě klikněte na vzorníku a informace vyplní odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e47e4-231">To see the properties, just click on the stencil and the information will be populated accordingly.</span></span> <span data-ttu-id="e47e4-232">Následující příklad ukazuje před a po "Databáze" vzorníku je přetáhli diagramu:</span><span class="sxs-lookup"><span data-stu-id="e47e4-232">The example below shows before and after a "Database" stencil is dragged onto the diagram:</span></span>

#### <a name="before"></a><span data-ttu-id="e47e4-233">Před</span><span class="sxs-lookup"><span data-stu-id="e47e4-233">Before</span></span>

![Před](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a><span data-ttu-id="e47e4-235">Po</span><span class="sxs-lookup"><span data-stu-id="e47e4-235">After</span></span>

![Po](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a><span data-ttu-id="e47e4-237">Zprávy</span><span class="sxs-lookup"><span data-stu-id="e47e4-237">Messages</span></span>

<span data-ttu-id="e47e4-238">Pokud chcete vytvořit model hrozeb a nezapomněli připojení k prvkům toky dat, v okně zprávy upozorní vás tak, aby fungoval. Můžete ho ignorovat nebo postupujte podle pokynů a vyřešte problém.</span><span class="sxs-lookup"><span data-stu-id="e47e4-238">If you create a threat model and forget to connect data flows to elements, the message window notifies you to act. You can choose to ignore it or follow the instructions to fix the issue.</span></span> 

![Zprávy](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a><span data-ttu-id="e47e4-240">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e47e4-240">Notes</span></span>

<span data-ttu-id="e47e4-241">Přepínání karty ze zprávy k poznámkám umožňuje přidání poznámky do diagramu zaznamenat všechny své myšlenky</span><span class="sxs-lookup"><span data-stu-id="e47e4-241">Switching tabs from Messages to Notes allows you to add notes to your diagram to capture all your thoughts</span></span>

## <a name="analysis-view"></a><span data-ttu-id="e47e4-242">Zobrazení analýzy</span><span class="sxs-lookup"><span data-stu-id="e47e4-242">Analysis view</span></span>

<span data-ttu-id="e47e4-243">Jakmile jste hotovi sestavování diagramu, přejít do zobrazení analysis přejdete na možnosti horní nabídce a zvolením lupy vedle palety Malování.</span><span class="sxs-lookup"><span data-stu-id="e47e4-243">Once you're done building your diagram, switch over to analysis view by going to the top menu selections and choosing the magnifying glass next to the paint palette.</span></span>

![Zobrazení analýzy](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a><span data-ttu-id="e47e4-245">Výběr generovaného hrozeb</span><span class="sxs-lookup"><span data-stu-id="e47e4-245">Generated threat selection</span></span>

<span data-ttu-id="e47e4-246">Když kliknete na hrozbu, můžete využít tři jedinečné funkce:</span><span class="sxs-lookup"><span data-stu-id="e47e4-246">When you click on a threat, you can leverage three unique functions:</span></span>

| <span data-ttu-id="e47e4-247">Funkce</span><span class="sxs-lookup"><span data-stu-id="e47e4-247">Feature</span></span>                               | <span data-ttu-id="e47e4-248">Informace</span><span class="sxs-lookup"><span data-stu-id="e47e4-248">Info</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e47e4-249">**Indikátor pro čtení**</span><span class="sxs-lookup"><span data-stu-id="e47e4-249">**Read Indicator**</span></span> | <p><span data-ttu-id="e47e4-250">Hrozby je nyní označena jako pro čtení, který lze snadno pomoct tak sledovat položky, které už se prostřednictvím</span><span class="sxs-lookup"><span data-stu-id="e47e4-250">Threat is now marked as read, which can easily help you keep track of the items you already went through</span></span></p><p>![Pro čtení nebo nepřečtená indikátoru](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| <span data-ttu-id="e47e4-252">**Interakce fokusu**</span><span class="sxs-lookup"><span data-stu-id="e47e4-252">**Interaction Focus**</span></span> | <p><span data-ttu-id="e47e4-253">Zvýrazní interakci v diagramu patřící do této hrozby</span><span class="sxs-lookup"><span data-stu-id="e47e4-253">Interaction in the diagram belonging to that threat is highlighted</span></span></p><p>![Interakce fokusu](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| <span data-ttu-id="e47e4-255">**Vlastnosti hrozeb**</span><span class="sxs-lookup"><span data-stu-id="e47e4-255">**Threat Properties**</span></span> | <p><span data-ttu-id="e47e4-256">Další informace o riziko, že se importují v okně vlastností hrozeb</span><span class="sxs-lookup"><span data-stu-id="e47e4-256">Additional information about the threat is populated in the threat properties window</span></span></p><p>![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a><span data-ttu-id="e47e4-258">Priorita změny</span><span class="sxs-lookup"><span data-stu-id="e47e4-258">Priority change</span></span>

<span data-ttu-id="e47e4-259">Změna úrovně priority jednotlivé generovaného hrozby také změní jejich barvy snadno identifikovat vysoká, střední a nízké priority hrozeb.</span><span class="sxs-lookup"><span data-stu-id="e47e4-259">Changing the priority level of each generated threat also changes their colors to make it easy to identify high, medium and low priority threats.</span></span>

![Priorita změny](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a><span data-ttu-id="e47e4-261">Upravitelné pole vlastnosti hrozeb</span><span class="sxs-lookup"><span data-stu-id="e47e4-261">Threat properties editable fields</span></span>

<span data-ttu-id="e47e4-262">Jak je vidět na předchozím obrázku, uživatelé mohou změnit informace generovaný nástrojem také přidat informace do určitá pole, jako je například zarovnání do bloku.</span><span class="sxs-lookup"><span data-stu-id="e47e4-262">As seen in the image above, users can change the information generated by the tool an also add information to certain fields, such as justification.</span></span> <span data-ttu-id="e47e4-263">Tato pole jsou generovány šablonou, takže pokud potřebujete další informace na jednotlivé hrozby, můžete se doporučuje, aby změny.</span><span class="sxs-lookup"><span data-stu-id="e47e4-263">These fields are generated by the template, so if you need more information for each threat, you're encouraged to make modifications.</span></span>

![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a><span data-ttu-id="e47e4-265">Reports</span><span class="sxs-lookup"><span data-stu-id="e47e4-265">Reports</span></span>

<span data-ttu-id="e47e4-266">Po dokončení změny priority a aktualizaci stavu jednotlivé generovaného hrozby, můžete soubor uložit nebo vytisknout sestavu tak, že přejdete na "Zpráva" a potom "Vytvořit úplná sestava."</span><span class="sxs-lookup"><span data-stu-id="e47e4-266">Once you're done changing priorities and updating the status of each generated threat, you can save the file and/or print out a report by going to "Report" and then "Create Full Report."</span></span> <span data-ttu-id="e47e4-267">Zobrazí se výzva k název sestavy a až to uděláte, měli byste vidět něco podobného jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="e47e4-267">You'll be asked to name the report, and once you do, you should see something similar to the image below:</span></span>

![Sestava](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a><span data-ttu-id="e47e4-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e47e4-269">Next steps</span></span>

<span data-ttu-id="e47e4-270">Přispívat šablonu pro komunity, přejděte prosím do naší  **[Githubu](https://github.com/Microsoft/threat-modeling-templates)**  stránky.</span><span class="sxs-lookup"><span data-stu-id="e47e4-270">To contribute a template for the community, please go to our **[GitHub](https://github.com/Microsoft/threat-modeling-templates)** page.</span></span> <span data-ttu-id="e47e4-271">**[Stáhněte si](https://aka.ms/tmtpreview)**  nástroje a začněte ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="e47e4-271">**[Download](https://aka.ms/tmtpreview)** the tool to get started today.</span></span>
