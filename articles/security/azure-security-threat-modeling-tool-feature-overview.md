---
title: "aaaMicrosoft nástroj pro modelování hrozeb – Azure | Microsoft Docs"
description: "Další informace o všech hello funkce dostupné v hello nástroj modelování hrozeb"
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
ms.openlocfilehash: f9ad5e623e7758063084cb7fc723c5735161a846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="threat-modeling-tool-feature-overview"></a><span data-ttu-id="e5c9c-103">Přehled funkcí nástroje modelování hrozeb</span><span class="sxs-lookup"><span data-stu-id="e5c9c-103">Threat Modeling Tool feature overview</span></span>

<span data-ttu-id="e5c9c-104">Jsme rádi, že jste zvolili toouse hello nástroj modelování hrozeb pro vaše threat modelování potřebám!</span><span class="sxs-lookup"><span data-stu-id="e5c9c-104">We are glad you chose toouse hello Threat Modeling Tool for your threat modeling needs!</span></span> <span data-ttu-id="e5c9c-105">Pokud jste tak dosud neučinili, navštivte  **[Začínáme s hello nástroj modelování hrozeb](./azure-security-threat-modeling-tool-getting-started.md)**  toolearn hello základy.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-105">If you haven’t done so, visit **[Getting Started with hello Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md)** toolearn hello basics.</span></span>

> <span data-ttu-id="e5c9c-106">Naše nástroj se často aktualizuje, takže zaškrtněte toto políčko Průvodce často toosee naše nejnovější funkce a vylepšení.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-106">Our tool is updated often, so check this guide often toosee our latest features and improvements.</span></span>

<span data-ttu-id="e5c9c-107">Kliknutím na tlačítko "Vytvořit nový Model" hello otevře prázdné úvodní stránky, podobně jako toohello obrázek níže:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-107">Clicking on hello "Create a New Model" button opens a blank start page, similar toohello image below:</span></span>

![Prázdné úvodní stránky](./media/azure-security-threat-modeling-tool/tmtstart.png)

<span data-ttu-id="e5c9c-109">Pomocí model hrozeb hello vytvořené náš tým v hello  **[Začínáme](./azure-security-threat-modeling-tool-getting-started.md)**  příkladu budeme podívejte se na všechny dostupné v nástroji hello hello funkce ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-109">Using hello threat model created by our team in hello **[Getting Started](./azure-security-threat-modeling-tool-getting-started.md)** example, let’s check out all hello features available in hello tool today.</span></span>

![Model základní hrozeb](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a><span data-ttu-id="e5c9c-111">Navigace</span><span class="sxs-lookup"><span data-stu-id="e5c9c-111">Navigation</span></span>

<span data-ttu-id="e5c9c-112">Než začnete hello integrované funkce, přejděte přes nebyly nalezeny v nástroji hello hlavní komponenty hello</span><span class="sxs-lookup"><span data-stu-id="e5c9c-112">Before diving into hello built-in features, let’s go over hello main components found in hello tool</span></span>

### <a name="menu-items"></a><span data-ttu-id="e5c9c-113">Položky nabídky</span><span class="sxs-lookup"><span data-stu-id="e5c9c-113">Menu items</span></span>

<span data-ttu-id="e5c9c-114">Hello prostředí by měl být podobné tooother produkty společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-114">hello experience should be similar tooother Microsoft products.</span></span> <span data-ttu-id="e5c9c-115">Začněme prostřednictvím položky nabídky nejvyšší úrovně hello:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-115">Let’s begin by going through hello top-level menu items:</span></span>

![Položky nabídky](./media/azure-security-threat-modeling-tool/menuitems.png)

| <span data-ttu-id="e5c9c-117">Štítek</span><span class="sxs-lookup"><span data-stu-id="e5c9c-117">Label</span></span>                               | <span data-ttu-id="e5c9c-118">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-118">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e5c9c-119">**File**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-119">**File**</span></span> | <ul><li><span data-ttu-id="e5c9c-120">Otevřít, uložte a zavřete soubory</span><span class="sxs-lookup"><span data-stu-id="e5c9c-120">Open, Save and Close Files</span></span></li><li><span data-ttu-id="e5c9c-121">Účty přihlášení volitelném OneDrive</span><span class="sxs-lookup"><span data-stu-id="e5c9c-121">Sign In/Out of OneDrive accounts</span></span></li><li><span data-ttu-id="e5c9c-122">Sdílet odkazy (zobrazení a úpravy)</span><span class="sxs-lookup"><span data-stu-id="e5c9c-122">Share Links (View + Edit)</span></span></li><li><span data-ttu-id="e5c9c-123">Zobrazení informací o souboru</span><span class="sxs-lookup"><span data-stu-id="e5c9c-123">View File Information</span></span></li><li><span data-ttu-id="e5c9c-124">Použít novou šablonu tooExisting modely</span><span class="sxs-lookup"><span data-stu-id="e5c9c-124">Apply New Template tooExisting Models</span></span></li></ul> |
| <span data-ttu-id="e5c9c-125">**Upravit**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-125">**Edit**</span></span> | <span data-ttu-id="e5c9c-126">Zpět/opakování akce, jako dobře kopii, vkládání a odstraňování</span><span class="sxs-lookup"><span data-stu-id="e5c9c-126">Undo/redo actions, as well a copy, paste and delete</span></span> |
| <span data-ttu-id="e5c9c-127">**Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-127">**View**</span></span> | <ul><li><span data-ttu-id="e5c9c-128">Přepínání mezi **Analysis** a **návrhu** zobrazení</span><span class="sxs-lookup"><span data-stu-id="e5c9c-128">Switch between **Analysis** and **Design** views</span></span></li><li><span data-ttu-id="e5c9c-129">Otevřete uzavřené windows (e.g.stencils, vlastností elementů a zprávy)</span><span class="sxs-lookup"><span data-stu-id="e5c9c-129">Open closed windows (e.g.stencils, element properties and messages)</span></span></li><li><span data-ttu-id="e5c9c-130">Resetovat nastavení toodefault rozložení</span><span class="sxs-lookup"><span data-stu-id="e5c9c-130">Reset layout toodefault settings</span></span></li></ul> |
| <span data-ttu-id="e5c9c-131">**Diagram**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-131">**Diagram**</span></span> | <span data-ttu-id="e5c9c-132">Přidání nebo odstranění diagramy a procházení "karty" diagramů</span><span class="sxs-lookup"><span data-stu-id="e5c9c-132">Add/Delete diagrams and navigate through “tabs” of diagrams</span></span> |
| <span data-ttu-id="e5c9c-133">**Sestavy**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-133">**Reports**</span></span> | <span data-ttu-id="e5c9c-134">Vytvoření sestavy tooshare HTML s ostatními uživateli</span><span class="sxs-lookup"><span data-stu-id="e5c9c-134">Create HTML reports tooshare with others</span></span> |
| <span data-ttu-id="e5c9c-135">**Pomoc**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-135">**Help**</span></span> | <span data-ttu-id="e5c9c-136">Provede toohelp použijete nástroj hello</span><span class="sxs-lookup"><span data-stu-id="e5c9c-136">Guides toohelp you use hello tool</span></span> |

<span data-ttu-id="e5c9c-137">zástupce pro hello nejvyšší úrovně nabídky jsou ikony Hello:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-137">hello icons are shortcuts for hello top-level menus:</span></span>

| <span data-ttu-id="e5c9c-138">Ikona</span><span class="sxs-lookup"><span data-stu-id="e5c9c-138">Icon</span></span>                               | <span data-ttu-id="e5c9c-139">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-139">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e5c9c-140">**Otevřete**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-140">**Open**</span></span> | <span data-ttu-id="e5c9c-141">Otevře se nový soubor</span><span class="sxs-lookup"><span data-stu-id="e5c9c-141">Opens a new file</span></span> |
| <span data-ttu-id="e5c9c-142">**Uložit**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-142">**Save**</span></span> | <span data-ttu-id="e5c9c-143">Uloží aktuální soubor</span><span class="sxs-lookup"><span data-stu-id="e5c9c-143">Saves current file</span></span> |
| <span data-ttu-id="e5c9c-144">**Návrh**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-144">**Design**</span></span> | <span data-ttu-id="e5c9c-145">Klient se přepne do zobrazení návrhu, kde můžete vytvořit modely</span><span class="sxs-lookup"><span data-stu-id="e5c9c-145">Goes into design view, where you can create models</span></span> |
| <span data-ttu-id="e5c9c-146">**Analýza**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-146">**Analyze**</span></span> | <span data-ttu-id="e5c9c-147">Ukazuje generované hrozby a jejich vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-147">Shows generated threats and their properties</span></span> |
| <span data-ttu-id="e5c9c-148">**Přidat Diagram**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-148">**Add Diagram**</span></span> | <span data-ttu-id="e5c9c-149">Přidá nový diagram (podobně jako toonew karty v aplikaci Excel)</span><span class="sxs-lookup"><span data-stu-id="e5c9c-149">Adds new diagram (similar toonew tabs in Excel)</span></span> |
| <span data-ttu-id="e5c9c-150">**Odstranit Diagram**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-150">**Delete Diagram**</span></span> | <span data-ttu-id="e5c9c-151">Odstraní aktuální diagram</span><span class="sxs-lookup"><span data-stu-id="e5c9c-151">Deletes current diagram</span></span> |
| <span data-ttu-id="e5c9c-152">**Vyjmutí/kopírování/vkládání**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-152">**Copy/Cut/Paste**</span></span> | <span data-ttu-id="e5c9c-153">Kopie nebo kusy/vloží elementy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-153">Copies/cuts/pastes elements</span></span> |
| <span data-ttu-id="e5c9c-154">**Vrátit/opakovat**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-154">**Undo/Redo**</span></span> | <span data-ttu-id="e5c9c-155">Vrátí zpět nebo znovu provede akce</span><span class="sxs-lookup"><span data-stu-id="e5c9c-155">Undoes/redoes actions</span></span> |
| <span data-ttu-id="e5c9c-156">**Přiblížení / oddálení**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-156">**Zoom In/Zoom Out**</span></span> | <span data-ttu-id="e5c9c-157">Zvětší směřující hello diagram pro lepší zobrazení</span><span class="sxs-lookup"><span data-stu-id="e5c9c-157">Zooms in and out of hello diagram for a better view</span></span> |
| <span data-ttu-id="e5c9c-158">**Váš názor**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-158">**Feedback**</span></span> | <span data-ttu-id="e5c9c-159">Otevře hello fórum MSDN</span><span class="sxs-lookup"><span data-stu-id="e5c9c-159">Opens hello MSDN Forum</span></span> |

### <a name="canvas"></a><span data-ttu-id="e5c9c-160">Plátno</span><span class="sxs-lookup"><span data-stu-id="e5c9c-160">Canvas</span></span>

<span data-ttu-id="e5c9c-161">Hello místa, kde můžete přetáhnout myší elementy do.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-161">hello space where you drag and drop elements into.</span></span> <span data-ttu-id="e5c9c-162">Přetažení je hello nejrychlejší a nejúčinnější způsob, jak toobuild modelů.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-162">Drag and drop is hello quickest and most efficient way toobuild models.</span></span> <span data-ttu-id="e5c9c-163">Může také klikněte pravým tlačítkem a vyberte z nabídky hello, které se přidá obecné verze hello prvků, které používáte, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-163">You may also right click and select from hello menu, which adds generic versions of hello elements you’re using, as shown below.</span></span>

#### <a name="dropping-hello-stencil-on-hello-canvas"></a><span data-ttu-id="e5c9c-164">Vyřazení hello vzorníku na plátně hello</span><span class="sxs-lookup"><span data-stu-id="e5c9c-164">Dropping hello stencil on hello canvas</span></span>

![Vyřaďte plátno](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-hello-stencil"></a><span data-ttu-id="e5c9c-166">Kliknutím na vzorníku hello</span><span class="sxs-lookup"><span data-stu-id="e5c9c-166">Clicking on hello stencil</span></span>

![Vlastnosti elementů](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a><span data-ttu-id="e5c9c-168">Vzorníky</span><span class="sxs-lookup"><span data-stu-id="e5c9c-168">Stencils</span></span>

<span data-ttu-id="e5c9c-169">Tam, kde můžete najít všechny předlohy, k dispozici toouse podle vybraná šablona hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-169">Where you can find all stencils available toouse based on hello template selected.</span></span> <span data-ttu-id="e5c9c-170">Pokud nemůžete najít správné elementy hello, zkuste použít jinou šablonu, nebo upravit jeden toofit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-170">If you can’t find hello right elements, try using another template, or modify one toofit your needs.</span></span> <span data-ttu-id="e5c9c-171">Obecně platí musí být schopný toofind kombinací kategorií jako hello ty, které jsou níže:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-171">Generally, you should be able toofind a combination of categories like hello ones below:</span></span>

| <span data-ttu-id="e5c9c-172">Název vzorníku</span><span class="sxs-lookup"><span data-stu-id="e5c9c-172">Stencil Name</span></span>                               | <span data-ttu-id="e5c9c-173">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-173">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e5c9c-174">**Proces**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-174">**Process**</span></span> | <span data-ttu-id="e5c9c-175">Aplikace, moduly plug-in prohlížeče, vláken, virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="e5c9c-175">Applications, Browser Plugins, Threads, Virtual Machines</span></span> |
| <span data-ttu-id="e5c9c-176">**Externí interakce**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-176">**External Interactor**</span></span> | <span data-ttu-id="e5c9c-177">Zprostředkovatelé ověřování, prohlížečů, uživatelé, webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e5c9c-177">Authentication Providers, Browsers, Users, Web Applications</span></span> |
| <span data-ttu-id="e5c9c-178">**Úložiště dat**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-178">**Data Store**</span></span> | <span data-ttu-id="e5c9c-179">Mezipaměť, úložiště, konfigurační soubory, databáze, registru</span><span class="sxs-lookup"><span data-stu-id="e5c9c-179">Cache, Storage, Configuration Files, Databases, Registry</span></span> |
| <span data-ttu-id="e5c9c-180">**Tok dat**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-180">**Data Flow**</span></span> | <span data-ttu-id="e5c9c-181">Binární, ALPC, HTTP, HTTPS, protokol TLS/SSL, IOCTL, IPSec, s názvem kanálu, RPC/DCOM, protokol SMB, UDP</span><span class="sxs-lookup"><span data-stu-id="e5c9c-181">Binary, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP</span></span> |
| <span data-ttu-id="e5c9c-182">**Vztah důvěryhodnosti hranic čáry či ohraničení**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-182">**Trust Line/Border Boundary**</span></span> | <span data-ttu-id="e5c9c-183">Podnikové sítě, Internet, počítače, izolovaného prostoru, režimu uživatele nebo jádra</span><span class="sxs-lookup"><span data-stu-id="e5c9c-183">Corporate Networks, Internet, Machine, Sandbox, User/Kernel Mode</span></span> |

### <a name="notesmessages"></a><span data-ttu-id="e5c9c-184">Poznámky a zprávy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-184">Notes/Messages</span></span>

| <span data-ttu-id="e5c9c-185">Komponenta</span><span class="sxs-lookup"><span data-stu-id="e5c9c-185">Component</span></span>                               | <span data-ttu-id="e5c9c-186">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-186">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e5c9c-187">**Zprávy**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-187">**Messages**</span></span> | <span data-ttu-id="e5c9c-188">Interní nástroj pro logiku, která upozorní uživatele, vždy, když dojde k chybě, jako je například žádná data proudí mezi elementy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-188">Internal tool logic that alerts users whenever there is an error, such as no data flows between elements</span></span> |
| <span data-ttu-id="e5c9c-189">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-189">**Notes**</span></span> | <span data-ttu-id="e5c9c-190">Ruční poznámky přidané toohello soubor engineering týmy celková hello návrhu a proces zkontrolovat</span><span class="sxs-lookup"><span data-stu-id="e5c9c-190">Manual notes added toohello file by engineering teams throughout hello design and review process</span></span> |

### <a name="element-properties"></a><span data-ttu-id="e5c9c-191">Vlastnosti elementů</span><span class="sxs-lookup"><span data-stu-id="e5c9c-191">Element properties</span></span>

<span data-ttu-id="e5c9c-192">To se liší podle vybrané elementy hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-192">These vary by hello elements selected.</span></span> <span data-ttu-id="e5c9c-193">Kromě hranice vztahu důvěryhodnosti všechny ostatní elementy obsahovat 3 obecné možnosti:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-193">Apart from Trust Boundaries, all other elements contain 3 general selections:</span></span>

| <span data-ttu-id="e5c9c-194">Vlastnost element</span><span class="sxs-lookup"><span data-stu-id="e5c9c-194">Element Property</span></span>                               | <span data-ttu-id="e5c9c-195">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-195">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e5c9c-196">**Název**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-196">**Name**</span></span> | <span data-ttu-id="e5c9c-197">Užitečné pro pojmenování vaší procesů, úložiště, interactors a toky toobe snadno rozpoznat.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-197">Useful for naming your processes, stores, interactors and flows toobe easily recognized</span></span> |
| <span data-ttu-id="e5c9c-198">**Mimo rozsah**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-198">**Out of Scope**</span></span> | <span data-ttu-id="e5c9c-199">Pokud vyberete, hello element se dostala mimo hello threat generování matice (nedoporučuje se)</span><span class="sxs-lookup"><span data-stu-id="e5c9c-199">If selected, hello element is taken out of hello threat generation matrix (not recommended)</span></span> |
| <span data-ttu-id="e5c9c-200">**Důvod mimo rozsah**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-200">**Reason for Out of Scope**</span></span> | <span data-ttu-id="e5c9c-201">Při zarovnání do bloku pole toolet uživatelé věděli, proč je mimo rozsah byla vybrána</span><span class="sxs-lookup"><span data-stu-id="e5c9c-201">Justification field toolet users know why out of scope was selected</span></span> |

<span data-ttu-id="e5c9c-202">Vlastnosti jsou změnit v rámci každé kategorie elementu.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-202">Properties are changed under each element category.</span></span> <span data-ttu-id="e5c9c-203">Klikněte na dostupné možnosti každý element tooinspect hello nebo otevřít další toolearn šablony hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-203">Click on each element tooinspect hello available options, or open hello template toolearn more.</span></span> <span data-ttu-id="e5c9c-204">Pojďme do funkce hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-204">Let’s get into hello features.</span></span>

## <a name="welcome-screen"></a><span data-ttu-id="e5c9c-205">Úvodní obrazovka</span><span class="sxs-lookup"><span data-stu-id="e5c9c-205">Welcome screen</span></span>

<span data-ttu-id="e5c9c-206">úvodní obrazovka Hello je první krok text hello, který se zobrazí při otevření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-206">hello welcome screen is hello first thing you see when you open hello app.</span></span>

### <a name="open-a-model"></a><span data-ttu-id="e5c9c-207">Otevřete model</span><span class="sxs-lookup"><span data-stu-id="e5c9c-207">Open A model</span></span>

<span data-ttu-id="e5c9c-208">Ukazatele myši na tlačítko "Otevřete modelu" ukazuje možnosti 2 Skrytá: "Otevřete z tento počítač" a "Otevřete z Onedrivu."</span><span class="sxs-lookup"><span data-stu-id="e5c9c-208">Hovering over “Open a Model” button shows you 2 hidden options: “Open From this Computer” and “Open from OneDrive.”</span></span> <span data-ttu-id="e5c9c-209">Hello nejprve otevře úvodní obrazovka otevřít soubor, zatímco hello druhý vás provede hello v procesu přihlašování pro OneDrive, což vám toopick složek a souborů po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-209">hello first opens hello File Open screen, while hello second takes you through hello sign in process for OneDrive, allowing you toopick folders and files after a successful authentication.</span></span>

![Otevřete modelu](./media/azure-security-threat-modeling-tool/openmodel.png)

![Otevřete v počítači nebo OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a><span data-ttu-id="e5c9c-212">Zpětná vazba, návrhy a problémy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-212">Feedback, suggestions and issues</span></span>

<span data-ttu-id="e5c9c-213">Výběrem této možnosti se dostanete fórech MSDN toohello nástroje SDL.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-213">Selecting this option will take you toohello MSDN Forums for SDL Tools.</span></span> <span data-ttu-id="e5c9c-214">Je toocheck skvělý způsob, jak jiní lidé produkt hello nástroj, včetně řešení a nových nápadů.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-214">It’s a great way toocheck out what other people are saying about hello tool, including workarounds and new ideas.</span></span>

![Váš názor](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a><span data-ttu-id="e5c9c-216">Návrhové zobrazení</span><span class="sxs-lookup"><span data-stu-id="e5c9c-216">Design view</span></span>

<span data-ttu-id="e5c9c-217">Při každém otevření nebo vytvořit nový model, budete přesměrováni toohello zobrazení návrhu.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-217">Whenever you open or create a new model, you’ll be taken toohello design view.</span></span>

### <a name="adding-elements"></a><span data-ttu-id="e5c9c-218">Přidávání elementů</span><span class="sxs-lookup"><span data-stu-id="e5c9c-218">Adding elements</span></span>

<span data-ttu-id="e5c9c-219">Způsoby 2 tooadd elementů v mřížce hello:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-219">There are 2 ways tooadd elements on hello grid:</span></span>

- <span data-ttu-id="e5c9c-220">**Přetáhnout myší** – přetáhněte hello požadovaný element toohello mřížky, potom použijte hello vlastnosti tooprovide Další informace o elementu.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-220">**Drag and Drop** – drag hello desired element toohello grid, then use hello element properties tooprovide additional information.</span></span>
- <span data-ttu-id="e5c9c-221">**Klikněte pravým tlačítkem na** – klikněte pravým tlačítkem kamkoli na hello mřížky a vyberte z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-221">**Right Click** – right click anywhere on hello grid and select from hello dropdown menu.</span></span> <span data-ttu-id="e5c9c-222">Obecná reprezentace tohoto prvku se zobrazí na úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-222">A generic representation of that element will appear on hello screen.</span></span>

### <a name="connecting-elements"></a><span data-ttu-id="e5c9c-223">Připojení elementy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-223">Connecting elements</span></span>

<span data-ttu-id="e5c9c-224">Způsoby 2 tooconnect prvky v nástroji hello:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-224">There are 2 ways tooconnect elements in hello tool:</span></span>

- <span data-ttu-id="e5c9c-225">**Přetáhnout myší** – přetáhněte hello požadovaného toku dat toohello mřížky a připojte oba elementy end toohello příslušných prvků.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-225">**Drag and Drop** – drag hello desired dataflow toohello grid, and connect both ends toohello appropriate elements.</span></span>
- <span data-ttu-id="e5c9c-226">**Klikněte na tlačítko + Shift** – klikněte na první prvek hello (odesílání dat), stiskněte a podržte klávesu Shift hello a pak vyberte hello druhý prvkem (přijetí dat).</span><span class="sxs-lookup"><span data-stu-id="e5c9c-226">**Click + Shift** – click on hello first element (sending data), press and hold hello Shift key, then select hello second element (receiving data).</span></span> <span data-ttu-id="e5c9c-227">Klikněte pravým tlačítkem a vyberte možnost "Připojit".</span><span class="sxs-lookup"><span data-stu-id="e5c9c-227">Right click, and select “Connect.”</span></span> <span data-ttu-id="e5c9c-228">Pokud používáte toku dat obousměrný, hello pořadí není jako důležité.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-228">If you’re using a bi-directional dataflow, hello order is not as important.</span></span>

### <a name="properties"></a><span data-ttu-id="e5c9c-229">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e5c9c-229">Properties</span></span>

<span data-ttu-id="e5c9c-230">Zobrazuje všechny hello vlastnosti, které je možné upravit v hello vzorníky umístěny v diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-230">Shows all hello properties that can be modified on hello stencils placed in hello diagram.</span></span> <span data-ttu-id="e5c9c-231">Vlastnosti hello toosee, právě klikněte na hello vzorníku a hello informace vyplní odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-231">toosee hello properties, just click on hello stencil and hello information will be populated accordingly.</span></span> <span data-ttu-id="e5c9c-232">Následující příklad Hello ukazuje před a po "Databáze" vzorníku je přetáhli hello diagram:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-232">hello example below shows before and after a "Database" stencil is dragged onto hello diagram:</span></span>

#### <a name="before"></a><span data-ttu-id="e5c9c-233">Před</span><span class="sxs-lookup"><span data-stu-id="e5c9c-233">Before</span></span>

![Před](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a><span data-ttu-id="e5c9c-235">Po</span><span class="sxs-lookup"><span data-stu-id="e5c9c-235">After</span></span>

![Po](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a><span data-ttu-id="e5c9c-237">Zprávy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-237">Messages</span></span>

<span data-ttu-id="e5c9c-238">Pokud chcete vytvořit model hrozeb a zapomněli, že tooconnect data proudí tooelements, upozorní vás okno zprávy hello tooact.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-238">If you create a threat model and forget tooconnect data flows tooelements, hello message window notifies you tooact.</span></span> <span data-ttu-id="e5c9c-239">Můžete si vybrat tooignore ji nebo postupujte podle pokynů toofix hello problém hello.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-239">You can choose tooignore it or follow hello instructions toofix hello issue.</span></span> 

![Zprávy](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a><span data-ttu-id="e5c9c-241">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e5c9c-241">Notes</span></span>

<span data-ttu-id="e5c9c-242">Přepínání karty ze zprávy tooNotes umožňuje vám tooadd poznámky tooyour diagram toocapture své myšlenky</span><span class="sxs-lookup"><span data-stu-id="e5c9c-242">Switching tabs from Messages tooNotes allows you tooadd notes tooyour diagram toocapture all your thoughts</span></span>

## <a name="analysis-view"></a><span data-ttu-id="e5c9c-243">Zobrazení analýzy</span><span class="sxs-lookup"><span data-stu-id="e5c9c-243">Analysis view</span></span>

<span data-ttu-id="e5c9c-244">Jakmile jste hotovi sestavování diagramu, zobrazení tooanalysis přejít budete vybrané možnosti toohello horní nabídce a zvolením hello lupy další toohello Malování palety.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-244">Once you're done building your diagram, switch over tooanalysis view by going toohello top menu selections and choosing hello magnifying glass next toohello paint palette.</span></span>

![Zobrazení analýzy](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a><span data-ttu-id="e5c9c-246">Výběr generovaného hrozeb</span><span class="sxs-lookup"><span data-stu-id="e5c9c-246">Generated threat selection</span></span>

<span data-ttu-id="e5c9c-247">Když kliknete na hrozbu, můžete využít tři jedinečné funkce:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-247">When you click on a threat, you can leverage three unique functions:</span></span>

| <span data-ttu-id="e5c9c-248">Funkce</span><span class="sxs-lookup"><span data-stu-id="e5c9c-248">Feature</span></span>                               | <span data-ttu-id="e5c9c-249">Informace</span><span class="sxs-lookup"><span data-stu-id="e5c9c-249">Info</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="e5c9c-250">**Indikátor pro čtení**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-250">**Read Indicator**</span></span> | <p><span data-ttu-id="e5c9c-251">Hrozby je nyní označena jako pro čtení, který lze snadno v aplikaci sledování hello položky, které už se prostřednictvím</span><span class="sxs-lookup"><span data-stu-id="e5c9c-251">Threat is now marked as read, which can easily help you keep track of hello items you already went through</span></span></p><p>![Pro čtení nebo nepřečtená indikátoru](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| <span data-ttu-id="e5c9c-253">**Interakce fokusu**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-253">**Interaction Focus**</span></span> | <p><span data-ttu-id="e5c9c-254">Zvýrazní interakci v diagramu hello patřící toothat hrozeb</span><span class="sxs-lookup"><span data-stu-id="e5c9c-254">Interaction in hello diagram belonging toothat threat is highlighted</span></span></p><p>![Interakce fokusu](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| <span data-ttu-id="e5c9c-256">**Vlastnosti hrozeb**</span><span class="sxs-lookup"><span data-stu-id="e5c9c-256">**Threat Properties**</span></span> | <p><span data-ttu-id="e5c9c-257">Další informace o ohrožení hello je vložené do hello threat vlastnosti – okno</span><span class="sxs-lookup"><span data-stu-id="e5c9c-257">Additional information about hello threat is populated in hello threat properties window</span></span></p><p>![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a><span data-ttu-id="e5c9c-259">Priorita změny</span><span class="sxs-lookup"><span data-stu-id="e5c9c-259">Priority change</span></span>

<span data-ttu-id="e5c9c-260">Změna úrovně priority hello jednotlivé generovaného hrozby také změní jejich barvy toomake ho snadno tooidentify vysoká, střední a nízké priority hrozeb.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-260">Changing hello priority level of each generated threat also changes their colors toomake it easy tooidentify high, medium and low priority threats.</span></span>

![Priorita změny](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a><span data-ttu-id="e5c9c-262">Upravitelné pole vlastnosti hrozeb</span><span class="sxs-lookup"><span data-stu-id="e5c9c-262">Threat properties editable fields</span></span>

<span data-ttu-id="e5c9c-263">Jak je vidět v hello obrázku výše, uživatelé mohou změnit hello informace generované nástrojem hello také přidat pole toocertain informace, jako je například zarovnání do bloku.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-263">As seen in hello image above, users can change hello information generated by hello tool an also add information toocertain fields, such as justification.</span></span> <span data-ttu-id="e5c9c-264">Tato pole jsou generovány šablonou hello, takže pokud potřebujete další informace na jednotlivé hrozby, jste podporovali toomake úpravy.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-264">These fields are generated by hello template, so if you need more information for each threat, you're encouraged toomake modifications.</span></span>

![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a><span data-ttu-id="e5c9c-266">Reports</span><span class="sxs-lookup"><span data-stu-id="e5c9c-266">Reports</span></span>

<span data-ttu-id="e5c9c-267">Po dokončení změny priority a aktualizuje hello stav každého z nich generuje hrozeb, můžete soubor hello uložit nebo vytisknout sestavu tak, že budete příliš "Oznamovat" a potom "vytvořit úplná sestava."</span><span class="sxs-lookup"><span data-stu-id="e5c9c-267">Once you're done changing priorities and updating hello status of each generated threat, you can save hello file and/or print out a report by going too"Report" and then "Create Full Report."</span></span> <span data-ttu-id="e5c9c-268">Zobrazí se výzva tooname hello sestavy a až to uděláte, měli byste vidět něco podobného toohello obrázku níže:</span><span class="sxs-lookup"><span data-stu-id="e5c9c-268">You'll be asked tooname hello report, and once you do, you should see something similar toohello image below:</span></span>

![Sestava](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a><span data-ttu-id="e5c9c-270">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5c9c-270">Next steps</span></span>

<span data-ttu-id="e5c9c-271">toocontribute šablonu pro hello komunity, přejděte prosím tooour  **[Githubu](https://github.com/Microsoft/threat-modeling-templates)**  stránky.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-271">toocontribute a template for hello community, please go tooour **[GitHub](https://github.com/Microsoft/threat-modeling-templates)** page.</span></span> <span data-ttu-id="e5c9c-272">**[Stáhněte si](https://aka.ms/tmtpreview)**  tooget hello nástroj spustit ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="e5c9c-272">**[Download](https://aka.ms/tmtpreview)** hello tool tooget started today.</span></span>
