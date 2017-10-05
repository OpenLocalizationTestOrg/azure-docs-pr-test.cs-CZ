---
title: "Jak používat Blitline pro bitovou kopii zpracování - Azure funkce Průvodce"
description: "Zjistěte, jak používat službu Blitline zpracování obrázků v rámci aplikace Azure."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 1d90599e028b3407a513b04b878e3aefc39928a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a><span data-ttu-id="94ce3-103">Použití Blitline s Azure a Azure Storage</span><span class="sxs-lookup"><span data-stu-id="94ce3-103">How to use Blitline with Azure and Azure Storage</span></span>
<span data-ttu-id="94ce3-104">Tato příručka vysvětluje, jak získat přístup ke službám Blitline a jak k odesílání úloh do Blitline.</span><span class="sxs-lookup"><span data-stu-id="94ce3-104">This guide will explain how to access Blitline services and how to submit jobs to Blitline.</span></span>

## <a name="what-is-blitline"></a><span data-ttu-id="94ce3-105">Co je Blitline?</span><span class="sxs-lookup"><span data-stu-id="94ce3-105">What is Blitline?</span></span>
<span data-ttu-id="94ce3-106">Blitline je bitové kopie založené na cloudu zpracování služba, která poskytuje podnikové úrovni image zpracování za zlomek ceny, které by náklady vytvořit sami.</span><span class="sxs-lookup"><span data-stu-id="94ce3-106">Blitline is a cloud-based image processing service that provides enterprise level image processing at a fraction of the price that it would cost to build it yourself.</span></span>

<span data-ttu-id="94ce3-107">Skutečnost je, že zpracování obrázků bylo provedeno opakovaně, obvykle znovu sestavit od základů pro každý web.</span><span class="sxs-lookup"><span data-stu-id="94ce3-107">The fact is that image processing has been done over and over again, usually rebuilt from the ground up for each and every website.</span></span> <span data-ttu-id="94ce3-108">Uvědomujeme si to vzhledem k tomu, že jsme sestavili je mil. časy příliš.</span><span class="sxs-lookup"><span data-stu-id="94ce3-108">We realize this because we’ve built them a million times too.</span></span> <span data-ttu-id="94ce3-109">Jeden den, které jsme se rozhodli, možná je čas jsme právě provést pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="94ce3-109">One day we decided that perhaps it‘s time we just do it for everyone.</span></span> <span data-ttu-id="94ce3-110">Víme, jak to udělat, provést rychlé a efektivní a uložte všechny pracovní do té doby.</span><span class="sxs-lookup"><span data-stu-id="94ce3-110">We know how to do it, to do it fast and efficiently, and save everyone work in the meantime.</span></span>

<span data-ttu-id="94ce3-111">Další informace najdete v tématu [http://www.blitline.com](http://www.blitline.com).</span><span class="sxs-lookup"><span data-stu-id="94ce3-111">For more information, see [http://www.blitline.com](http://www.blitline.com).</span></span>

## <a name="what-blitline-is-not"></a><span data-ttu-id="94ce3-112">Co Blitline není...</span><span class="sxs-lookup"><span data-stu-id="94ce3-112">What Blitline is NOT...</span></span>
<span data-ttu-id="94ce3-113">O vysvětlení, co je užitečné pro Blitline, je často usnadňují identifikaci co Blitline neprovádí než budete pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="94ce3-113">To clarify what Blitline is useful for, it is often easier to identify what Blitline does NOT do before moving forward.</span></span>

* <span data-ttu-id="94ce3-114">Blitline nemá pomůcky HTML pro odesílání obrázků.</span><span class="sxs-lookup"><span data-stu-id="94ce3-114">Blitline does NOT have HTML widgets to upload images.</span></span> <span data-ttu-id="94ce3-115">Bitové kopie, které musí mít veřejně nebo s omezenými oprávněními pro Blitline spojit.</span><span class="sxs-lookup"><span data-stu-id="94ce3-115">You must have images available publicly or with restricted permissions available for Blitline to reach.</span></span>
* <span data-ttu-id="94ce3-116">Blitline neprovádí zpracování, jako je Aviary.com za provozu obrázku</span><span class="sxs-lookup"><span data-stu-id="94ce3-116">Blitline does NOT do live image processing, like Aviary.com</span></span>
* <span data-ttu-id="94ce3-117">Blitline nepřijímá odesílání obrázků, obrázků na Blitline nemůžete nabízet přímo.</span><span class="sxs-lookup"><span data-stu-id="94ce3-117">Blitline does NOT accept image uploads, you cannot push your images to Blitline directly.</span></span> <span data-ttu-id="94ce3-118">Musíte vložit je Azure Storage nebo z jiných míst podporuje Blitline a potom zadejte Blitline, kde je získat.</span><span class="sxs-lookup"><span data-stu-id="94ce3-118">You must push them to Azure Storage or other places Blitline supports and then tell Blitline where to go get them.</span></span>
* <span data-ttu-id="94ce3-119">Blitline je massively parallel a neprovádí žádné synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="94ce3-119">Blitline is massively parallel and does NOT do any synchronous processing.</span></span> <span data-ttu-id="94ce3-120">Což znamená, že musí poskytnout nám postback_url a jsme vám řeknou jsme se po dokončení zpracování.</span><span class="sxs-lookup"><span data-stu-id="94ce3-120">Meaning you must give us a postback_url and we can tell you when we are done processing.</span></span>

## <a name="create-a-blitline-account"></a><span data-ttu-id="94ce3-121">Vytvoření účtu Blitline</span><span class="sxs-lookup"><span data-stu-id="94ce3-121">Create a Blitline account</span></span>
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a><span data-ttu-id="94ce3-122">Jak vytvořit úlohu Blitline</span><span class="sxs-lookup"><span data-stu-id="94ce3-122">How to create a Blitline job</span></span>
<span data-ttu-id="94ce3-123">Blitline používá JSON definovat akce, které budete chtít využít na bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="94ce3-123">Blitline uses JSON to define the actions you want to take on an image.</span></span> <span data-ttu-id="94ce3-124">Tento formát JSON se skládá z několika jednoduchých polí.</span><span class="sxs-lookup"><span data-stu-id="94ce3-124">This JSON is composed of a few simple fields.</span></span>

<span data-ttu-id="94ce3-125">V nejjednodušší příkladu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="94ce3-125">The simplest example is as follows:</span></span>

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

<span data-ttu-id="94ce3-126">Tady bychom měli formátu JSON, který bude trvat bitovou kopii "src" "... boys.jpeg" a potom změňte velikost této bitové kopie do 240 x 140.</span><span class="sxs-lookup"><span data-stu-id="94ce3-126">Here we have JSON that will take a "src" image "...boys.jpeg" and then resize that image to 240x140.</span></span>

<span data-ttu-id="94ce3-127">ID aplikace se můžete najít v vaše **informace o připojení** nebo **SPRAVOVAT** karty v Azure.</span><span class="sxs-lookup"><span data-stu-id="94ce3-127">The Application ID is something you can find in your **CONNECTION INFO** or **MANAGE** tab on Azure.</span></span> <span data-ttu-id="94ce3-128">Je váš tajný identifikátor, který umožňuje spouštění úloh na Blitline.</span><span class="sxs-lookup"><span data-stu-id="94ce3-128">It is your secret identifier that allows you to run jobs on Blitline.</span></span>

<span data-ttu-id="94ce3-129">Parametr "uložit" identifikuje informace o kam chcete umístit bitovou kopii po jsme ho mít zpracování.</span><span class="sxs-lookup"><span data-stu-id="94ce3-129">The "save" parameter identifies information about where you want to put the image once we have processed it.</span></span> <span data-ttu-id="94ce3-130">V tomto případě trivial jsme nebyly definovány jedna.</span><span class="sxs-lookup"><span data-stu-id="94ce3-130">In this trivial case, we haven't defined one.</span></span> <span data-ttu-id="94ce3-131">Pokud je definována žádná umístění Blitline uloží ho místně (a dočasně) v umístění jedinečný cloudu.</span><span class="sxs-lookup"><span data-stu-id="94ce3-131">If no location is defined Blitline will store it locally (and temporarily) at a unique cloud location.</span></span> <span data-ttu-id="94ce3-132">Bude moct získat z JSON vrácený Blitline při provádění Blitline tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="94ce3-132">You will be able to get that location from the JSON returned by Blitline when you make the Blitline.</span></span> <span data-ttu-id="94ce3-133">Identifikátor "image" je vyžadován a je vrácen v případě k identifikaci této konkrétní uloženy bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="94ce3-133">The "image" identifier is required and is returned to you when to identify this particular saved image.</span></span>

<span data-ttu-id="94ce3-134">Můžete najít další informace o *funkce* podporujeme zde: <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="94ce3-134">You can find more information about the *functions* we support here: <http://www.blitline.com/docs/functions></span></span>

<span data-ttu-id="94ce3-135">Můžete také vyhledat dokumentaci o možnosti úlohy: <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="94ce3-135">You can also find documentation about the job options here: <http://www.blitline.com/docs/api></span></span>

<span data-ttu-id="94ce3-136">Až budete mít vaše struktury JSON všechny musíte udělat je **POST** jej do`http://api.blitline.com/job`</span><span class="sxs-lookup"><span data-stu-id="94ce3-136">Once you have your JSON all you need to do is **POST** it to `http://api.blitline.com/job`</span></span>

<span data-ttu-id="94ce3-137">Zobrazí se zpět JSON, která vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="94ce3-137">You will get JSON back that looks something like this:</span></span>

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


<span data-ttu-id="94ce3-138">Znamená to, Blitline přijal žádost, se pozastavil ve frontě zpracování a po jeho dokončení bitovou kopii, budou k dispozici na: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikace\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span><span class="sxs-lookup"><span data-stu-id="94ce3-138">This tells you that Blitline has recieved your request, it has put it in a processing queue, and when it has completed the image will be available at: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span></span>

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a><span data-ttu-id="94ce3-139">Jak uložit bitovou kopii do účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="94ce3-139">How to save an image to your Azure Storage account</span></span>
<span data-ttu-id="94ce3-140">Pokud máte účet úložiště Azure, budete moci snadno Blitline push zpracovaná bitové kopie do Azure container.</span><span class="sxs-lookup"><span data-stu-id="94ce3-140">If you have an Azure Storage account, you can easily have Blitline push the processed images into your Azure container.</span></span> <span data-ttu-id="94ce3-141">Přidáním "azure_destination" Zadejte umístění a oprávnění pro Blitline k.</span><span class="sxs-lookup"><span data-stu-id="94ce3-141">By adding an "azure_destination" you define the location and permissions for Blitline to push to.</span></span>

<span data-ttu-id="94ce3-142">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="94ce3-142">Here is an example:</span></span>

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


<span data-ttu-id="94ce3-143">Vyplněním CAPITALIZED hodnoty vlastními odešlete tento formát JSON na http://api.blitline.com/job a bitovou kopii "src" budou zpracování pomocí filtru rozostření a pak instaluje do Azure cílové můžete.</span><span class="sxs-lookup"><span data-stu-id="94ce3-143">By filling in the CAPITALIZED values with your own, you can submit this JSON to http://api.blitline.com/job and the "src" image will be processed with a blur filter and then pushed to you Azure destination.</span></span>

### <a name="please-note"></a><span data-ttu-id="94ce3-144">Poznámka:</span><span class="sxs-lookup"><span data-stu-id="94ce3-144">Please note:</span></span>
<span data-ttu-id="94ce3-145">SAS musí obsahovat celou SAS adresa url, včetně názvu cílového souboru.</span><span class="sxs-lookup"><span data-stu-id="94ce3-145">The SAS must contain the entire SAS url, including the filename of the destination file.</span></span>

<span data-ttu-id="94ce3-146">Příklad:</span><span class="sxs-lookup"><span data-stu-id="94ce3-146">Example:</span></span>

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


<span data-ttu-id="94ce3-147">Můžete si také přečíst nejnovější verzi dokumentace Azure Storage na Blitline [zde](http://www.blitline.com/docs/azure_storage).</span><span class="sxs-lookup"><span data-stu-id="94ce3-147">You can also read the latest edition of Blitline's Azure Storage docs [here](http://www.blitline.com/docs/azure_storage).</span></span>

## <a name="next-steps"></a><span data-ttu-id="94ce3-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94ce3-148">Next Steps</span></span>
<span data-ttu-id="94ce3-149">Blitline.com najdete další informace o všech našich dalších funkcí:</span><span class="sxs-lookup"><span data-stu-id="94ce3-149">Visit blitline.com to read about all our other features:</span></span>

* <span data-ttu-id="94ce3-150">Dokumentace pro koncový bod rozhraní API Blitline <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="94ce3-150">Blitline API Endpoint Docs <http://www.blitline.com/docs/api></span></span>
* <span data-ttu-id="94ce3-151">Funkce Blitline rozhraní API <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="94ce3-151">Blitline API Functions <http://www.blitline.com/docs/functions></span></span>
* <span data-ttu-id="94ce3-152">Příklady Blitline rozhraní API <http://www.blitline.com/docs/examples></span><span class="sxs-lookup"><span data-stu-id="94ce3-152">Blitline API Examples <http://www.blitline.com/docs/examples></span></span>
* <span data-ttu-id="94ce3-153">Třetí součástí Nuget knihovny <http://nuget.org/packages/Blitline.Net></span><span class="sxs-lookup"><span data-stu-id="94ce3-153">Third Part Nuget Library <http://nuget.org/packages/Blitline.Net></span></span>

