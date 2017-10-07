---
title: "aaaHow toouse Blitline pro zpracování obrázků - Azure funkce Průvodce"
description: "Zjistěte, jak toouse hello Blitline služby tooprocess bitové kopie v rámci aplikace Azure."
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
ms.openlocfilehash: 328fd177e25f45f29f8ad8e142d02b46017a858e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blitline-with-azure-and-azure-storage"></a><span data-ttu-id="d68a0-103">Jak toouse Blitline s Azure a Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d68a0-103">How toouse Blitline with Azure and Azure Storage</span></span>
<span data-ttu-id="d68a0-104">Tato příručka vysvětluje, jak tooaccess Blitline služby a jak toosubmit úlohy tooBlitline.</span><span class="sxs-lookup"><span data-stu-id="d68a0-104">This guide will explain how tooaccess Blitline services and how toosubmit jobs tooBlitline.</span></span>

## <a name="what-is-blitline"></a><span data-ttu-id="d68a0-105">Co je Blitline?</span><span class="sxs-lookup"><span data-stu-id="d68a0-105">What is Blitline?</span></span>
<span data-ttu-id="d68a0-106">Blitline je bitové kopie založené na cloudu zpracování služba, která poskytuje podnikové úrovni image zpracování za zlomek ceny hello, že ho náklady toobuild ho sami.</span><span class="sxs-lookup"><span data-stu-id="d68a0-106">Blitline is a cloud-based image processing service that provides enterprise level image processing at a fraction of hello price that it would cost toobuild it yourself.</span></span>

<span data-ttu-id="d68a0-107">fakt Hello je, že zpracování obrázků bylo provedeno opakovaně, obvykle znovu sestavit z hello základů pro každý web.</span><span class="sxs-lookup"><span data-stu-id="d68a0-107">hello fact is that image processing has been done over and over again, usually rebuilt from hello ground up for each and every website.</span></span> <span data-ttu-id="d68a0-108">Uvědomujeme si to vzhledem k tomu, že jsme sestavili je mil. časy příliš.</span><span class="sxs-lookup"><span data-stu-id="d68a0-108">We realize this because we’ve built them a million times too.</span></span> <span data-ttu-id="d68a0-109">Jeden den, které jsme se rozhodli, možná je čas jsme právě provést pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="d68a0-109">One day we decided that perhaps it‘s time we just do it for everyone.</span></span> <span data-ttu-id="d68a0-110">Víme, jak toodo ho toodo je rychlé a efektivní a uložte všechny fungovat v hello té doby.</span><span class="sxs-lookup"><span data-stu-id="d68a0-110">We know how toodo it, toodo it fast and efficiently, and save everyone work in hello meantime.</span></span>

<span data-ttu-id="d68a0-111">Další informace najdete v tématu [http://www.blitline.com](http://www.blitline.com).</span><span class="sxs-lookup"><span data-stu-id="d68a0-111">For more information, see [http://www.blitline.com](http://www.blitline.com).</span></span>

## <a name="what-blitline-is-not"></a><span data-ttu-id="d68a0-112">Co Blitline není...</span><span class="sxs-lookup"><span data-stu-id="d68a0-112">What Blitline is NOT...</span></span>
<span data-ttu-id="d68a0-113">tooclarify co Blitline je užitečné pro je často jednodušší tooidentify co Blitline neprovádí než budete pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="d68a0-113">tooclarify what Blitline is useful for, it is often easier tooidentify what Blitline does NOT do before moving forward.</span></span>

* <span data-ttu-id="d68a0-114">Blitline nemá HTML pomůcky tooupload bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="d68a0-114">Blitline does NOT have HTML widgets tooupload images.</span></span> <span data-ttu-id="d68a0-115">Bitové kopie, které musí mít veřejně nebo s omezenými oprávněními, které jsou k dispozici pro Blitline tooreach.</span><span class="sxs-lookup"><span data-stu-id="d68a0-115">You must have images available publicly or with restricted permissions available for Blitline tooreach.</span></span>
* <span data-ttu-id="d68a0-116">Blitline neprovádí zpracování, jako je Aviary.com za provozu obrázku</span><span class="sxs-lookup"><span data-stu-id="d68a0-116">Blitline does NOT do live image processing, like Aviary.com</span></span>
* <span data-ttu-id="d68a0-117">Blitline nepřijímá odesílání obrázků, nemůžete nabízet vaší bitové kopie tooBlitline přímo.</span><span class="sxs-lookup"><span data-stu-id="d68a0-117">Blitline does NOT accept image uploads, you cannot push your images tooBlitline directly.</span></span> <span data-ttu-id="d68a0-118">Musíte vložit je tooAzure úložiště nebo z jiných míst, který podporuje Blitline a pak dali pokyn Blitline, kde je toogo získat.</span><span class="sxs-lookup"><span data-stu-id="d68a0-118">You must push them tooAzure Storage or other places Blitline supports and then tell Blitline where toogo get them.</span></span>
* <span data-ttu-id="d68a0-119">Blitline je massively parallel a neprovádí žádné synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="d68a0-119">Blitline is massively parallel and does NOT do any synchronous processing.</span></span> <span data-ttu-id="d68a0-120">Což znamená, že musí poskytnout nám postback_url a jsme vám řeknou jsme se po dokončení zpracování.</span><span class="sxs-lookup"><span data-stu-id="d68a0-120">Meaning you must give us a postback_url and we can tell you when we are done processing.</span></span>

## <a name="create-a-blitline-account"></a><span data-ttu-id="d68a0-121">Vytvoření účtu Blitline</span><span class="sxs-lookup"><span data-stu-id="d68a0-121">Create a Blitline account</span></span>
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-toocreate-a-blitline-job"></a><span data-ttu-id="d68a0-122">Jak toocreate Blitline úlohy</span><span class="sxs-lookup"><span data-stu-id="d68a0-122">How toocreate a Blitline job</span></span>
<span data-ttu-id="d68a0-123">Blitline používá JSON toodefine hello akce, které má tootake na bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="d68a0-123">Blitline uses JSON toodefine hello actions you want tootake on an image.</span></span> <span data-ttu-id="d68a0-124">Tento formát JSON se skládá z několika jednoduchých polí.</span><span class="sxs-lookup"><span data-stu-id="d68a0-124">This JSON is composed of a few simple fields.</span></span>

<span data-ttu-id="d68a0-125">Příklad nejjednodušší Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d68a0-125">hello simplest example is as follows:</span></span>

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

<span data-ttu-id="d68a0-126">Tady bychom měli formátu JSON, který bude trvat bitovou kopii "src" "... boys.jpeg" a potom změňte velikost too240x140 této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="d68a0-126">Here we have JSON that will take a "src" image "...boys.jpeg" and then resize that image too240x140.</span></span>

<span data-ttu-id="d68a0-127">ID aplikace Hello se něco můžete najít v vaše **informace o připojení** nebo **SPRAVOVAT** karty v Azure.</span><span class="sxs-lookup"><span data-stu-id="d68a0-127">hello Application ID is something you can find in your **CONNECTION INFO** or **MANAGE** tab on Azure.</span></span> <span data-ttu-id="d68a0-128">Je váš tajný identifikátor, který vám umožní toorun úlohy na Blitline.</span><span class="sxs-lookup"><span data-stu-id="d68a0-128">It is your secret identifier that allows you toorun jobs on Blitline.</span></span>

<span data-ttu-id="d68a0-129">Hello "uložit" parametr identifikuje informace o místo, kam chcete tooput hello image po jsme ho mít zpracování.</span><span class="sxs-lookup"><span data-stu-id="d68a0-129">hello "save" parameter identifies information about where you want tooput hello image once we have processed it.</span></span> <span data-ttu-id="d68a0-130">V tomto případě trivial jsme nebyly definovány jedna.</span><span class="sxs-lookup"><span data-stu-id="d68a0-130">In this trivial case, we haven't defined one.</span></span> <span data-ttu-id="d68a0-131">Pokud je definována žádná umístění Blitline uloží ho místně (a dočasně) v umístění jedinečný cloudu.</span><span class="sxs-lookup"><span data-stu-id="d68a0-131">If no location is defined Blitline will store it locally (and temporarily) at a unique cloud location.</span></span> <span data-ttu-id="d68a0-132">Bude moct tooget, která umístění, ze hello JSON vrácený Blitline při provádění hello Blitline.</span><span class="sxs-lookup"><span data-stu-id="d68a0-132">You will be able tooget that location from hello JSON returned by Blitline when you make hello Blitline.</span></span> <span data-ttu-id="d68a0-133">identifikátor "image" Hello je potřeba a tooyou je vrácena, pokud tooidentify uložit tento konkrétní obrázek.</span><span class="sxs-lookup"><span data-stu-id="d68a0-133">hello "image" identifier is required and is returned tooyou when tooidentify this particular saved image.</span></span>

<span data-ttu-id="d68a0-134">Můžete najít další informace o hello *funkce* podporujeme zde: <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="d68a0-134">You can find more information about hello *functions* we support here: <http://www.blitline.com/docs/functions></span></span>

<span data-ttu-id="d68a0-135">Můžete také vyhledat dokumentaci o hello možnosti úlohy zde: <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="d68a0-135">You can also find documentation about hello job options here: <http://www.blitline.com/docs/api></span></span>

<span data-ttu-id="d68a0-136">Až budete mít vaše struktury JSON stačí toodo je **POST** je příliš`http://api.blitline.com/job`</span><span class="sxs-lookup"><span data-stu-id="d68a0-136">Once you have your JSON all you need toodo is **POST** it too`http://api.blitline.com/job`</span></span>

<span data-ttu-id="d68a0-137">Zobrazí se zpět JSON, která vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="d68a0-137">You will get JSON back that looks something like this:</span></span>

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


<span data-ttu-id="d68a0-138">Znamená to, Blitline přijal žádost, se pozastavil ve frontě zpracování a po jeho dokončení hello image budou k dispozici na: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikace\_ID /CK3f0xBF_2bV6wf7gEZE8w.jpg**</span><span class="sxs-lookup"><span data-stu-id="d68a0-138">This tells you that Blitline has recieved your request, it has put it in a processing queue, and when it has completed hello image will be available at: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span></span>

## <a name="how-toosave-an-image-tooyour-azure-storage-account"></a><span data-ttu-id="d68a0-139">Jak toosave tooyour image účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="d68a0-139">How toosave an image tooyour Azure Storage account</span></span>
<span data-ttu-id="d68a0-140">Pokud máte účet úložiště Azure, můžete snadno mít Blitline nabízené hello zpracovat bitové kopie do Azure container.</span><span class="sxs-lookup"><span data-stu-id="d68a0-140">If you have an Azure Storage account, you can easily have Blitline push hello processed images into your Azure container.</span></span> <span data-ttu-id="d68a0-141">Přidáním "azure_destination" definujete hello umístění a oprávnění pro Blitline toopush k.</span><span class="sxs-lookup"><span data-stu-id="d68a0-141">By adding an "azure_destination" you define hello location and permissions for Blitline toopush to.</span></span>

<span data-ttu-id="d68a0-142">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="d68a0-142">Here is an example:</span></span>

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


<span data-ttu-id="d68a0-143">Vyplněním hello CAPITALIZED hodnoty vlastními, můžete odeslat tento toohttp://api.blitline.com/job JSON a hello "src" image budou zpracovány pomocí filtru rozostření a pak poslat tooyou Azure cílový.</span><span class="sxs-lookup"><span data-stu-id="d68a0-143">By filling in hello CAPITALIZED values with your own, you can submit this JSON toohttp://api.blitline.com/job and hello "src" image will be processed with a blur filter and then pushed tooyou Azure destination.</span></span>

### <a name="please-note"></a><span data-ttu-id="d68a0-144">Poznámka:</span><span class="sxs-lookup"><span data-stu-id="d68a0-144">Please note:</span></span>
<span data-ttu-id="d68a0-145">Hello SAS musí obsahovat hello celý SAS adresu url, včetně hello filename hello cílový soubor.</span><span class="sxs-lookup"><span data-stu-id="d68a0-145">hello SAS must contain hello entire SAS url, including hello filename of hello destination file.</span></span>

<span data-ttu-id="d68a0-146">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d68a0-146">Example:</span></span>

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


<span data-ttu-id="d68a0-147">Můžete si také přečíst nejnovější verzi dokumentace Azure Storage na Blitline, hello [zde](http://www.blitline.com/docs/azure_storage).</span><span class="sxs-lookup"><span data-stu-id="d68a0-147">You can also read hello latest edition of Blitline's Azure Storage docs [here](http://www.blitline.com/docs/azure_storage).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d68a0-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d68a0-148">Next Steps</span></span>
<span data-ttu-id="d68a0-149">Navštivte blitline.com tooread o všechny naše další funkce:</span><span class="sxs-lookup"><span data-stu-id="d68a0-149">Visit blitline.com tooread about all our other features:</span></span>

* <span data-ttu-id="d68a0-150">Dokumentace pro koncový bod rozhraní API Blitline <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="d68a0-150">Blitline API Endpoint Docs <http://www.blitline.com/docs/api></span></span>
* <span data-ttu-id="d68a0-151">Funkce Blitline rozhraní API <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="d68a0-151">Blitline API Functions <http://www.blitline.com/docs/functions></span></span>
* <span data-ttu-id="d68a0-152">Příklady Blitline rozhraní API <http://www.blitline.com/docs/examples></span><span class="sxs-lookup"><span data-stu-id="d68a0-152">Blitline API Examples <http://www.blitline.com/docs/examples></span></span>
* <span data-ttu-id="d68a0-153">Třetí součástí Nuget knihovny <http://nuget.org/packages/Blitline.Net></span><span class="sxs-lookup"><span data-stu-id="d68a0-153">Third Part Nuget Library <http://nuget.org/packages/Blitline.Net></span></span>

