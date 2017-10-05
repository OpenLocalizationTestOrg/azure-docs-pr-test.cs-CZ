---
title: "Výstupní datový proud analýza Data Lake Store | Microsoft Docs"
description: "Konfigurace ověřování a autorizace Azure Data Lake Store v úloze Stream Analytics"
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 3d867df3ef875d5cc41de418c3d1d269ff751fda
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="stream-analytics-data-lake-store-output"></a><span data-ttu-id="55724-103">Výstupní datový proud analýza Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="55724-103">Stream Analytics Data Lake Store output</span></span>
<span data-ttu-id="55724-104">Úlohy Stream Analytics podporovat několik metod pro výstup, jeden se [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="55724-104">Stream Analytics jobs support several output methods, one being an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span> <span data-ttu-id="55724-105">Azure Data Lake Store je celopodnikové, flexibilně škálovatelné úložiště pro analytické úlohy s velkými objemy dat.</span><span class="sxs-lookup"><span data-stu-id="55724-105">Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.</span></span> <span data-ttu-id="55724-106">Data Lake Store umožňuje ukládání dat z jakékoli velikosti, typu a rychlosti příjmu pro provozní a zjišťovací analýzy.</span><span class="sxs-lookup"><span data-stu-id="55724-106">Data Lake Store enables you to store data of any size, type and ingestion speed for operational and exploratory analytics.</span></span>

## <a name="authorize-a-data-lake-store-account"></a><span data-ttu-id="55724-107">Autorizace účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="55724-107">Authorize a Data Lake Store account</span></span>
1. <span data-ttu-id="55724-108">Pokud Data Lake Store je vybraná jako výstupu na portálu Azure, zobrazí se výzva k autorizaci použití existující Data Lake Store nebo abychom požádali o přístup k Data Lake Store prostřednictvím portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="55724-108">When Data Lake Store is selected as an output in the Azure portal, you will be prompted to authorize use of your existing Data Lake Store or to request access to the Data Lake Store via the Classic Portal.</span></span>
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. <span data-ttu-id="55724-109">Pokud již máte přístup do Data Lake Store, klikněte na tlačítko Autorizovat a po krátkou dobu a stránky objeví se označující "Přesměrováním na autorizace".</span><span class="sxs-lookup"><span data-stu-id="55724-109">If you already have access to Data Lake Store, click “Authorize Now” and for a brief time a page will pop up indicating “Redirecting to authorization”.</span></span> <span data-ttu-id="55724-110">Stránce se automaticky zavře a zobrazí se stránka, ke které by umožňují konfigurovat výstupní Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="55724-110">The page will automatically close and you will be presented with the page that would allow you to configure the Data Lake Store output.</span></span>

<span data-ttu-id="55724-111">Pokud nemáte registraci pro Data Lake Store, můžete pomocí následujícího odkazu "Přihlásit" zahájit požadavku nebo postupujte podle [získají pokyny k Začínáme](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="55724-111">If you have not signed up for Data Lake Store, you can follow the “Sign up now” link to initiate the request, or follow the [get started instructions](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="configure-the-data-lake-store-output-properties"></a><span data-ttu-id="55724-112">Konfigurovat vlastnosti výstupní Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="55724-112">Configure the Data Lake Store output properties</span></span>
<span data-ttu-id="55724-113">Jakmile máte účet Data Lake Store, ověření, můžete konfigurovat vlastnosti pro výstup do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="55724-113">Once you have the Data Lake Store account authenticated, you can configure the properties for your Data Lake Store output.</span></span> <span data-ttu-id="55724-114">V následující tabulce je seznam názvů vlastností a jejich popis konfigurace výstup do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="55724-114">The table below is the list of property names and their description to configure your Data Lake Store output.</span></span>

<table>
<tbody>
<tr>
<td><span data-ttu-id="55724-115"><B>NÁZEV VLASTNOSTI</B></span><span class="sxs-lookup"><span data-stu-id="55724-115"><B>PROPERTY NAME</B></span></span></td>
<td><span data-ttu-id="55724-116"><B>POPIS</B></span><span class="sxs-lookup"><span data-stu-id="55724-116"><B>DESCRIPTION</B></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-117">Alias pro výstup</span><span class="sxs-lookup"><span data-stu-id="55724-117">Output Alias</span></span></td>
<td><span data-ttu-id="55724-118">Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do tohoto Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="55724-118">This is a friendly name used in queries to direct the query output to this Data Lake Store.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-119">Účet data Lake Store</span><span class="sxs-lookup"><span data-stu-id="55724-119">Data Lake Store Account</span></span></td>
<td><span data-ttu-id="55724-120">Název účtu úložiště, kde jsou odesílání výstupu.</span><span class="sxs-lookup"><span data-stu-id="55724-120">The name of the storage account where you are sending your output.</span></span> <span data-ttu-id="55724-121">Zobrazí seznam účtů Data Lake Store, které má přihlášený uživatel přístup k.</span><span class="sxs-lookup"><span data-stu-id="55724-121">You will be presented with a list of Data Lake Store accounts  the logged in user has access to.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-122">Předpony vzorek cesty [<I>volitelné</I>]</span><span class="sxs-lookup"><span data-stu-id="55724-122">Path Prefix Pattern [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="55724-123">Cesta k souboru používaná k zápisu souborů v rámci zadaného účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="55724-123">The file path used to write your files within the specified Data Lake Store Account.</span></span> <BR><span data-ttu-id="55724-124">{date}, {time}</span><span class="sxs-lookup"><span data-stu-id="55724-124">{date}, {time}</span></span><BR><span data-ttu-id="55724-125">Příklad 1: složku1/logs / {date} / {time}</span><span class="sxs-lookup"><span data-stu-id="55724-125">Example 1: folder1/logs/{date}/{time}</span></span><BR><span data-ttu-id="55724-126">Příklad 2: složku1/logs / {date}</span><span class="sxs-lookup"><span data-stu-id="55724-126">Example 2: folder1/logs/{date}</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-127">Datum formátu [<I>volitelné</I>]</span><span class="sxs-lookup"><span data-stu-id="55724-127">Date Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="55724-128">Pokud se v cestě předponu používá token kalendářního data, můžete vybrat formát data, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="55724-128">If the date token is used in the prefix path, you can select the date format in which your files are organized.</span></span> <span data-ttu-id="55724-129">Příklad: Rrrr/MM/DD</span><span class="sxs-lookup"><span data-stu-id="55724-129">Example: YYYY/MM/DD</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-130">Formát času [<I>volitelné</I>]</span><span class="sxs-lookup"><span data-stu-id="55724-130">Time Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="55724-131">Pokud token čas se používá v cestě předponu, zadejte formát času, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="55724-131">If the time token is used in the prefix path, specify the time format in which your files are organized.</span></span> <span data-ttu-id="55724-132">Aktuálně jedinou podporovanou hodnotou je HH.</span><span class="sxs-lookup"><span data-stu-id="55724-132">Currently the only supported value is HH.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-133">Formát serializace událostí</span><span class="sxs-lookup"><span data-stu-id="55724-133">Event Serialization Format</span></span></td>
<td><span data-ttu-id="55724-134">Formát serializace pro výstupní data.</span><span class="sxs-lookup"><span data-stu-id="55724-134">Serialization format for output data.</span></span> <span data-ttu-id="55724-135">Jsou podporovány JSON, CSV a Avro.</span><span class="sxs-lookup"><span data-stu-id="55724-135">JSON, CSV, and Avro are supported.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-136">Encoding</span><span class="sxs-lookup"><span data-stu-id="55724-136">Encoding</span></span></td>
<td><span data-ttu-id="55724-137">Pokud formátu CSV nebo formátu JSON, kódování musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="55724-137">If CSV or JSON format, an encoding must be specified.</span></span> <span data-ttu-id="55724-138">V tuto chvíli je jediným podporovaným formátem kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="55724-138">UTF-8 is the only supported encoding format at this time.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-139">Oddělovač</span><span class="sxs-lookup"><span data-stu-id="55724-139">Delimiter</span></span></td>
<td><span data-ttu-id="55724-140">Platí jenom pro serializaci sdílených svazků clusteru.</span><span class="sxs-lookup"><span data-stu-id="55724-140">Only applicable for CSV serialization.</span></span> <span data-ttu-id="55724-141">Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="55724-141">Stream Analytics supports a number of common delimiters for serializing CSV data.</span></span> <span data-ttu-id="55724-142">Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</span><span class="sxs-lookup"><span data-stu-id="55724-142">Supported values are comma, semicolon, space, tab and vertical bar.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="55724-143">Formát</span><span class="sxs-lookup"><span data-stu-id="55724-143">Format</span></span></td>
<td><span data-ttu-id="55724-144">Platí jenom pro serializaci JSON.</span><span class="sxs-lookup"><span data-stu-id="55724-144">Only applicable for JSON serialization.</span></span> <span data-ttu-id="55724-145">Řádcích: Určuje, že výstup se bude formátovat tak, že každý objekt JSON oddělených nový řádek.</span><span class="sxs-lookup"><span data-stu-id="55724-145">Line separated specifies that the output will be formatted by having each JSON object separated by a new line.</span></span> <span data-ttu-id="55724-146">Pole určuje, že výstup naformátovaný jako pole objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="55724-146">Array specifies that the output will be formatted as an array of JSON objects.</span></span></td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a><span data-ttu-id="55724-147">Obnovit autorizační Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="55724-147">Renew Data Lake Store Authorization</span></span>
<span data-ttu-id="55724-148">V současné době není omezení kde ověřovací token, je potřeba ručně aktualizovat každých 90 dní pro všechny úlohy s výstupem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="55724-148">Currently, there is a limitation where the authentication token needs to be manually refreshed every 90 days for all jobs with Data Lake Store output.</span></span> <span data-ttu-id="55724-149">Musíte se také znovu provést ověření účtu Data Lake Store, pokud jste změnili heslo, protože úlohu vytvoření nebo poslední ověření.</span><span class="sxs-lookup"><span data-stu-id="55724-149">You will also need to re-authenticate your Data Lake Store account if you have changed your password since your job was created or last authenticated.</span></span> <span data-ttu-id="55724-150">Příznakem tohoto problému je žádný výstup úlohy a chybu v protokolech operaci označující potřebu opětovná autorizace.</span><span class="sxs-lookup"><span data-stu-id="55724-150">A symptom of this issue is no job output and an error in the Operation Logs indicating need for re-authorization.</span></span>

<span data-ttu-id="55724-151">Chcete-li vyřešit tento problém, přejděte na výstup do Data Lake Store a zastavit spuštěná úloha.</span><span class="sxs-lookup"><span data-stu-id="55724-151">To resolve this issue, stop your running job and go to your Data Lake Store output.</span></span> <span data-ttu-id="55724-152">Klikněte na odkaz "Obnovit autorizace" a po krátkou dobu a stránky objeví se označující "Přesměrováním autorizace …".</span><span class="sxs-lookup"><span data-stu-id="55724-152">Click the “Renew authorization” link, and for a brief time a page will pop up indicating “Redirecting to authorization..”.</span></span> <span data-ttu-id="55724-153">Stránka se automaticky zavře a v případě úspěšného označí "Autorizace byl úspěšně obnoven".</span><span class="sxs-lookup"><span data-stu-id="55724-153">The page will automatically close and if successful, will indicate “Authorization has been successfully renewed”.</span></span> <span data-ttu-id="55724-154">Je pak potřeba v dolní části stránky klikněte na tlačítko "Uložit" a restartováním úlohu od poslední zastavena ztráty dat pokračovat.</span><span class="sxs-lookup"><span data-stu-id="55724-154">You then need to click “Save” at the bottom of the page, and can proceed by restarting your job from the Last Stopped Time to avoid data loss.</span></span>

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

