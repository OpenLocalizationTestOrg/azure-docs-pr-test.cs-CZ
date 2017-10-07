---
title: "aaaStream analýza Data Lake Store výstup | Microsoft Docs"
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
ms.openlocfilehash: 183cf51edb2e49ac3e42257e67a8077b95777258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-data-lake-store-output"></a><span data-ttu-id="c82b8-103">Výstupní datový proud analýza Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c82b8-103">Stream Analytics Data Lake Store output</span></span>
<span data-ttu-id="c82b8-104">Úlohy Stream Analytics podporovat několik metod pro výstup, jeden se [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="c82b8-104">Stream Analytics jobs support several output methods, one being an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span> <span data-ttu-id="c82b8-105">Azure Data Lake Store je celopodnikové, flexibilně škálovatelné úložiště pro analytické úlohy s velkými objemy dat.</span><span class="sxs-lookup"><span data-stu-id="c82b8-105">Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.</span></span> <span data-ttu-id="c82b8-106">Data Lake Store umožňuje toostore data jakékoli velikosti, typu a rychlosti příjmu pro provozní a zjišťovací analýzy.</span><span class="sxs-lookup"><span data-stu-id="c82b8-106">Data Lake Store enables you toostore data of any size, type and ingestion speed for operational and exploratory analytics.</span></span>

## <a name="authorize-a-data-lake-store-account"></a><span data-ttu-id="c82b8-107">Autorizace účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c82b8-107">Authorize a Data Lake Store account</span></span>
1. <span data-ttu-id="c82b8-108">Pokud Data Lake Store je vybraná jako výstupu v hello portálu Azure, zobrazí se výzva tooauthorize použití existující Data Lake Store nebo toorequest přístup toohello Data Lake Store prostřednictvím hello portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="c82b8-108">When Data Lake Store is selected as an output in hello Azure portal, you will be prompted tooauthorize use of your existing Data Lake Store or toorequest access toohello Data Lake Store via hello Classic Portal.</span></span>
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. <span data-ttu-id="c82b8-109">Pokud již máte přístup k tooData Lake Store, klikněte na tlačítko Autorizovat a po krátkou dobu a stránky objeví se označující "Přesměrováním tooauthorization".</span><span class="sxs-lookup"><span data-stu-id="c82b8-109">If you already have access tooData Lake Store, click “Authorize Now” and for a brief time a page will pop up indicating “Redirecting tooauthorization”.</span></span> <span data-ttu-id="c82b8-110">stránku Hello se automaticky zavře a zobrazí se stránka hello, která by umožnilo tooconfigure hello Data Lake Store výstup.</span><span class="sxs-lookup"><span data-stu-id="c82b8-110">hello page will automatically close and you will be presented with hello page that would allow you tooconfigure hello Data Lake Store output.</span></span>

<span data-ttu-id="c82b8-111">Pokud nemáte registraci pro Data Lake Store, můžete podle hello "Přihlásit" odkaz tooinitiate hello požadavku nebo postupujte podle hello [získají pokyny k Začínáme](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c82b8-111">If you have not signed up for Data Lake Store, you can follow hello “Sign up now” link tooinitiate hello request, or follow hello [get started instructions](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="configure-hello-data-lake-store-output-properties"></a><span data-ttu-id="c82b8-112">Konfigurovat vlastnosti výstup hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c82b8-112">Configure hello Data Lake Store output properties</span></span>
<span data-ttu-id="c82b8-113">Jakmile máte účet Data Lake Store hello ověřený, můžete nakonfigurovat vlastnosti hello pro výstup do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c82b8-113">Once you have hello Data Lake Store account authenticated, you can configure hello properties for your Data Lake Store output.</span></span> <span data-ttu-id="c82b8-114">Následující tabulka Hello je hello seznam názvů vlastností a jejich popis tooconfigure, které výstupní Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c82b8-114">hello table below is hello list of property names and their description tooconfigure your Data Lake Store output.</span></span>

<table>
<tbody>
<tr>
<td><span data-ttu-id="c82b8-115"><B>NÁZEV VLASTNOSTI</B></span><span class="sxs-lookup"><span data-stu-id="c82b8-115"><B>PROPERTY NAME</B></span></span></td>
<td><span data-ttu-id="c82b8-116"><B>POPIS</B></span><span class="sxs-lookup"><span data-stu-id="c82b8-116"><B>DESCRIPTION</B></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-117">Alias pro výstup</span><span class="sxs-lookup"><span data-stu-id="c82b8-117">Output Alias</span></span></td>
<td><span data-ttu-id="c82b8-118">Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c82b8-118">This is a friendly name used in queries toodirect hello query output toothis Data Lake Store.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-119">Účet data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c82b8-119">Data Lake Store Account</span></span></td>
<td><span data-ttu-id="c82b8-120">Hello název účtu úložiště hello kde odesílání výstupu.</span><span class="sxs-lookup"><span data-stu-id="c82b8-120">hello name of hello storage account where you are sending your output.</span></span> <span data-ttu-id="c82b8-121">Zobrazí seznam účtů Data Lake Store, který má přístup k hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c82b8-121">You will be presented with a list of Data Lake Store accounts  hello logged in user has access to.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-122">Předpony vzorek cesty [<I>volitelné</I>]</span><span class="sxs-lookup"><span data-stu-id="c82b8-122">Path Prefix Pattern [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="c82b8-123">Hello souboru cesta používaná toowrite vaše soubory v rámci hello zadaný účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c82b8-123">hello file path used toowrite your files within hello specified Data Lake Store Account.</span></span> <BR><span data-ttu-id="c82b8-124">{date}, {time}</span><span class="sxs-lookup"><span data-stu-id="c82b8-124">{date}, {time}</span></span><BR><span data-ttu-id="c82b8-125">Příklad 1: složku1/logs / {date} / {time}</span><span class="sxs-lookup"><span data-stu-id="c82b8-125">Example 1: folder1/logs/{date}/{time}</span></span><BR><span data-ttu-id="c82b8-126">Příklad 2: složku1/logs / {date}</span><span class="sxs-lookup"><span data-stu-id="c82b8-126">Example 2: folder1/logs/{date}</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-127">Datum formátu [<I>volitelné</I>]</span><span class="sxs-lookup"><span data-stu-id="c82b8-127">Date Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="c82b8-128">Pokud token kalendářního data hello se používá v cestě hello předponu, můžete vybrat formát data hello, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="c82b8-128">If hello date token is used in hello prefix path, you can select hello date format in which your files are organized.</span></span> <span data-ttu-id="c82b8-129">Příklad: Rrrr/MM/DD</span><span class="sxs-lookup"><span data-stu-id="c82b8-129">Example: YYYY/MM/DD</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-130">Formát času [<I>volitelné</I>]</span><span class="sxs-lookup"><span data-stu-id="c82b8-130">Time Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="c82b8-131">Pokud token čas hello se používá v cestě hello předponu, zadejte formát času hello, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="c82b8-131">If hello time token is used in hello prefix path, specify hello time format in which your files are organized.</span></span> <span data-ttu-id="c82b8-132">Aktuálně hello podporována pouze hodnota je HH.</span><span class="sxs-lookup"><span data-stu-id="c82b8-132">Currently hello only supported value is HH.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-133">Formát serializace událostí</span><span class="sxs-lookup"><span data-stu-id="c82b8-133">Event Serialization Format</span></span></td>
<td><span data-ttu-id="c82b8-134">Formát serializace pro výstupní data.</span><span class="sxs-lookup"><span data-stu-id="c82b8-134">Serialization format for output data.</span></span> <span data-ttu-id="c82b8-135">Jsou podporovány JSON, CSV a Avro.</span><span class="sxs-lookup"><span data-stu-id="c82b8-135">JSON, CSV, and Avro are supported.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-136">Encoding</span><span class="sxs-lookup"><span data-stu-id="c82b8-136">Encoding</span></span></td>
<td><span data-ttu-id="c82b8-137">Pokud formátu CSV nebo formátu JSON, kódování musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="c82b8-137">If CSV or JSON format, an encoding must be specified.</span></span> <span data-ttu-id="c82b8-138">Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování.</span><span class="sxs-lookup"><span data-stu-id="c82b8-138">UTF-8 is hello only supported encoding format at this time.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-139">Oddělovač</span><span class="sxs-lookup"><span data-stu-id="c82b8-139">Delimiter</span></span></td>
<td><span data-ttu-id="c82b8-140">Platí jenom pro serializaci sdílených svazků clusteru.</span><span class="sxs-lookup"><span data-stu-id="c82b8-140">Only applicable for CSV serialization.</span></span> <span data-ttu-id="c82b8-141">Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="c82b8-141">Stream Analytics supports a number of common delimiters for serializing CSV data.</span></span> <span data-ttu-id="c82b8-142">Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</span><span class="sxs-lookup"><span data-stu-id="c82b8-142">Supported values are comma, semicolon, space, tab and vertical bar.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c82b8-143">Formát</span><span class="sxs-lookup"><span data-stu-id="c82b8-143">Format</span></span></td>
<td><span data-ttu-id="c82b8-144">Platí jenom pro serializaci JSON.</span><span class="sxs-lookup"><span data-stu-id="c82b8-144">Only applicable for JSON serialization.</span></span> <span data-ttu-id="c82b8-145">Řádcích: Určuje, že se bude formátovat výstup hello tak, že každý objekt JSON oddělených nový řádek.</span><span class="sxs-lookup"><span data-stu-id="c82b8-145">Line separated specifies that hello output will be formatted by having each JSON object separated by a new line.</span></span> <span data-ttu-id="c82b8-146">Pole určuje, že hello výstup naformátovaný jako pole objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="c82b8-146">Array specifies that hello output will be formatted as an array of JSON objects.</span></span></td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a><span data-ttu-id="c82b8-147">Obnovit autorizační Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c82b8-147">Renew Data Lake Store Authorization</span></span>
<span data-ttu-id="c82b8-148">V současné době není omezení kde hello ověřovací token musí toobe ručně aktualizovat každých 90 dní pro všechny úlohy s výstupem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c82b8-148">Currently, there is a limitation where hello authentication token needs toobe manually refreshed every 90 days for all jobs with Data Lake Store output.</span></span> <span data-ttu-id="c82b8-149">Budete také potřebovat toore-ověření svého účtu Data Lake Store, pokud jste změnili heslo, protože úlohu vytvoření nebo poslední ověření.</span><span class="sxs-lookup"><span data-stu-id="c82b8-149">You will also need toore-authenticate your Data Lake Store account if you have changed your password since your job was created or last authenticated.</span></span> <span data-ttu-id="c82b8-150">Příznakem tohoto problému je žádný výstup úlohy a chybu v protokolech operaci hello označující potřebu opětovná autorizace.</span><span class="sxs-lookup"><span data-stu-id="c82b8-150">A symptom of this issue is no job output and an error in hello Operation Logs indicating need for re-authorization.</span></span>

<span data-ttu-id="c82b8-151">tooresolve tento problém, zastavte spuštěná úloha a přejděte výstup tooyour Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c82b8-151">tooresolve this issue, stop your running job and go tooyour Data Lake Store output.</span></span> <span data-ttu-id="c82b8-152">Kliknutím na odkaz "Autorizace obnovení" hello a po krátkou dobu a stránky objeví se označující "Přesměrováním tooauthorization..".</span><span class="sxs-lookup"><span data-stu-id="c82b8-152">Click hello “Renew authorization” link, and for a brief time a page will pop up indicating “Redirecting tooauthorization..”.</span></span> <span data-ttu-id="c82b8-153">stránku Hello se automaticky zavře a v případě úspěšného označí "Autorizace byl úspěšně obnoven".</span><span class="sxs-lookup"><span data-stu-id="c82b8-153">hello page will automatically close and if successful, will indicate “Authorization has been successfully renewed”.</span></span> <span data-ttu-id="c82b8-154">Potom potřebovat tooclick "Uložit" v dolní části hello hello stránky a můžete pokračovat restartováním úlohu z hello ztráty dat tooavoid naposledy zastaveno.</span><span class="sxs-lookup"><span data-stu-id="c82b8-154">You then need tooclick “Save” at hello bottom of hello page, and can proceed by restarting your job from hello Last Stopped Time tooavoid data loss.</span></span>

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

