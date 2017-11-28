---
title: "cloudové služby Azure s Azure CDN aaaIntegrate | Microsoft Docs"
description: "Zjistěte, jak toodeploy Cloudová služba, poskytuje obsah z integrované koncového bodu Azure CDN"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f20d60b0b5edc133adf06d010633a15f62e2b8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="09b52-103"><a name="intro"></a>Cloudové služby integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-103"><a name="intro"></a> Integrate a cloud service with Azure CDN</span></span>
<span data-ttu-id="09b52-104">Cloudové služby můžete integrovat Azure CDN, obsluhující žádný obsah z umístění hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="09b52-104">A cloud service can be integrated with Azure CDN, serving any content from hello cloud service's location.</span></span> <span data-ttu-id="09b52-105">Tento postup nabízí hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="09b52-105">This approach gives you hello following advantages:</span></span>

* <span data-ttu-id="09b52-106">Můžete snadno nasadit a aktualizaci bitové kopie, skriptů a šablon v adresářích projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="09b52-106">Easily deploy and update images, scripts, and stylesheets in your cloud service's project directories</span></span>
* <span data-ttu-id="09b52-107">Snadno upgradujte hello balíčků NuGet v rámci cloudové služby, například jQuery nebo Bootstrap verze</span><span class="sxs-lookup"><span data-stu-id="09b52-107">Easily upgrade hello NuGet packages in your cloud service, such as jQuery or Bootstrap versions</span></span>
* <span data-ttu-id="09b52-108">Správa webové aplikace a vaše CDN-poskytovaného obsahu všechny z hello stejné rozhraní sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09b52-108">Manage your Web application and your CDN-served content all from hello same Visual Studio interface</span></span>
* <span data-ttu-id="09b52-109">Pracovní postup jednotná nasazení pro webové aplikace a obsah obsluhuje CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-109">Unified deployment workflow for your Web application and your CDN-served content</span></span>
* <span data-ttu-id="09b52-110">ASP.NET sdružování a minimalizace integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-110">Integrate ASP.NET bundling and minification with Azure CDN</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="09b52-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="09b52-111">What you will learn</span></span>
<span data-ttu-id="09b52-112">V tomto kurzu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="09b52-112">In this tutorial, you will learn how to:</span></span>

* [<span data-ttu-id="09b52-113">Koncový bod Azure CDN integrovat s cloudovou službou a poskytovat statický obsah na webových stránkách z Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-113">Integrate an Azure CDN endpoint with your cloud service and serve static content in your Web pages from Azure CDN</span></span>](#deploy)
* [<span data-ttu-id="09b52-114">Konfigurace nastavení mezipaměti pro statický obsah v rámci cloudové služby</span><span class="sxs-lookup"><span data-stu-id="09b52-114">Configure cache settings for static content in your cloud service</span></span>](#caching)
* [<span data-ttu-id="09b52-115">Poskytovat obsah z akce kontroleru prostřednictvím Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-115">Serve content from controller actions through Azure CDN</span></span>](#controller)
* [<span data-ttu-id="09b52-116">Používat spojeno dohromady a minifikovaný obsah prostřednictvím Azure CDN při zachování hello skriptu ladění prostředí v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09b52-116">Serve bundled and minified content through Azure CDN while preserving hello script debugging experience in Visual Studio</span></span>](#bundling)
* [<span data-ttu-id="09b52-117">Konfigurace záložního skriptů a šablon stylů CSS při offline Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-117">Configure fallback your scripts and CSS when your Azure CDN is offline</span></span>](#fallback)

## <a name="what-you-will-build"></a><span data-ttu-id="09b52-118">Co se sestavení</span><span class="sxs-lookup"><span data-stu-id="09b52-118">What you will build</span></span>
<span data-ttu-id="09b52-119">Nasazení webové role služby cloudu pomocí hello výchozí šablony ASP.NET MVC, přidáte kód tooserve obsah z integrované CDN Azure, jako je například bitovou kopii, výsledky akce kontroleru a hello výchozí souborů JavaScript a CSS a také psát kód tooconfigure hello nouzový mechanismus, sady, které jsou poskytovány na hello událostí tohoto hello CDN je offline.</span><span class="sxs-lookup"><span data-stu-id="09b52-119">You will deploy a cloud service Web role using hello default ASP.NET MVC template, add code tooserve content from an integrated Azure CDN, such as an image, controller action results, and hello default JavaScript and CSS files, and also write code tooconfigure hello fallback mechanism for bundles served in hello event that hello CDN is offline.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="09b52-120">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="09b52-120">What you will need</span></span>
<span data-ttu-id="09b52-121">V tomto kurzu má hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="09b52-121">This tutorial has hello following prerequisites:</span></span>

* <span data-ttu-id="09b52-122">Aktivní [účtem Microsoft Azure.](/account/)</span><span class="sxs-lookup"><span data-stu-id="09b52-122">An active [Microsoft Azure account](/account/)</span></span>
* <span data-ttu-id="09b52-123">Visual Studio 2015 se [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="09b52-123">Visual Studio 2015 with [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span></span>

> [!NOTE]
> <span data-ttu-id="09b52-124">Je třeba účtu Azure toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="09b52-124">You need an Azure account toocomplete this tutorial:</span></span>
> 
> * <span data-ttu-id="09b52-125">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití až můžete hello účet ponechat a používat bezplatné služby Azure, jako jsou weby.</span><span class="sxs-lookup"><span data-stu-id="09b52-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use tootry out paid Azure services, and even after they're used up you can keep hello account and use free Azure services, such as Websites.</span></span>
> * <span data-ttu-id="09b52-126">Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="09b52-126">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a><span data-ttu-id="09b52-127">Nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="09b52-127">Deploy a cloud service</span></span>
<span data-ttu-id="09b52-128">V této části nasazení hello výchozí šablony aplikace ASP.NET MVC v sadě Visual Studio 2015 tooa cloudové služby webovou roli a potom ji integrovat s nástrojem nový koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-128">In this section, you will deploy hello default ASP.NET MVC application template in Visual Studio 2015 tooa cloud service Web role, and then integrate it with a new CDN endpoint.</span></span> <span data-ttu-id="09b52-129">Postupujte podle pokynů hello níže:</span><span class="sxs-lookup"><span data-stu-id="09b52-129">Follow hello instructions below:</span></span>

1. <span data-ttu-id="09b52-130">V sadě Visual Studio 2015, vytvořte novou službu Azure cloud z řádku nabídek hello přechodem příliš**soubor > Nový > Projekt > Cloud > Cloudová služba Azure**.</span><span class="sxs-lookup"><span data-stu-id="09b52-130">In Visual Studio 2015, create a new Azure cloud service from hello menu bar by going too**File > New > Project > Cloud > Azure Cloud Service**.</span></span> <span data-ttu-id="09b52-131">Pojmenujte ho a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="09b52-131">Give it a name and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. <span data-ttu-id="09b52-132">Vyberte **webovou roli ASP.NET** a klikněte na tlačítko hello  **>**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09b52-132">Select **ASP.NET Web Role** and click hello **>** button.</span></span> <span data-ttu-id="09b52-133">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="09b52-133">Click OK.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. <span data-ttu-id="09b52-134">Vyberte **MVC** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="09b52-134">Select **MVC** and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. <span data-ttu-id="09b52-135">Nyní publikování této webové role tooan cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="09b52-135">Now, publish this Web role tooan Azure cloud service.</span></span> <span data-ttu-id="09b52-136">Klikněte pravým tlačítkem na projekt cloudové služby hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="09b52-136">Right-click hello cloud service project and select **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. <span data-ttu-id="09b52-137">Pokud jste ještě nebyly přihlášeni Microsoft Azure, klikněte na tlačítko hello **přidat účet...**  rozevíracího seznamu a klikněte na tlačítko hello **přidat účet** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="09b52-137">If you have not yet signed into Microsoft Azure, click hello **Add an account...** dropdown and click hello **Add an account** menu item.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. <span data-ttu-id="09b52-138">Hello přihlašovací stránce Přihlaste se pomocí účtu Microsoft, které jste použili tooactivate účtu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-138">In hello sign-in page, sign in with hello Microsoft account you used tooactivate your Azure account.</span></span>
7. <span data-ttu-id="09b52-139">Jakmile jste přihlášení, klikněte na možnost **Další**.</span><span class="sxs-lookup"><span data-stu-id="09b52-139">Once you're signed in, click **Next**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. <span data-ttu-id="09b52-140">Za předpokladu, že jste nevytvořili účet cloudové služby nebo úložiště, Visual Studio vám pomůže vytvořit obě.</span><span class="sxs-lookup"><span data-stu-id="09b52-140">Assuming that you haven't created a cloud service or storage account, Visual Studio will help you create both.</span></span> <span data-ttu-id="09b52-141">V hello **vytvoření cloudové služby a účet** dialogové okno, název typu hello požadované služby a hello vyberte požadovanou oblast.</span><span class="sxs-lookup"><span data-stu-id="09b52-141">In hello **Create Cloud Service and Account** dialog, type hello desired service name and select hello desired region.</span></span> <span data-ttu-id="09b52-142">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="09b52-142">Then, click **Create**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. <span data-ttu-id="09b52-143">V hello stránky nastavení publikování, ověřte konfiguraci hello a klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="09b52-143">In hello publish settings page, verify hello configuration and click **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > <span data-ttu-id="09b52-144">proces publikování Hello pro cloudové služby trvá příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="09b52-144">hello publishing process for cloud services takes a long time.</span></span> <span data-ttu-id="09b52-145">Hello povolit nasazení webu pro všechny role možnost provádět ladění mnohem rychlejší cloudové služby tím, že poskytuje rychlé (ale dočasné) aktualizace tooyour webové role.</span><span class="sxs-lookup"><span data-stu-id="09b52-145">hello Enable Web Deploy for all roles option can make debugging your cloud service much quicker by providing fast (but temporary) updates tooyour Web roles.</span></span> <span data-ttu-id="09b52-146">Další informace o této možnosti najdete v tématu [publikování cloudové služby pomocí nástroje Azure hello](http://msdn.microsoft.com/library/ff683672.aspx).</span><span class="sxs-lookup"><span data-stu-id="09b52-146">For more information on this option, see [Publishing a Cloud Service using hello Azure Tools](http://msdn.microsoft.com/library/ff683672.aspx).</span></span>
   > 
   > 
   
    <span data-ttu-id="09b52-147">Když hello **protokoly aktivit Microsoft Azure** ukazuje, že stav publikování je **dokončeno**, vytvoří koncový bod CDN, která je integrovaná s touto cloudovou službou.</span><span class="sxs-lookup"><span data-stu-id="09b52-147">When hello **Microsoft Azure Activity Log** shows that publishing status is **Completed**, you will create a CDN endpoint that's integrated with this cloud service.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="09b52-148">Pokud po publikování, hello nasazení cloudové služby zobrazí chybové obrazovce, je pravděpodobné, protože používá hello cloudové služby, které jste nasadili [hosta operační systém, který nezahrnuje .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span><span class="sxs-lookup"><span data-stu-id="09b52-148">If, after publishing, hello deployed cloud service displays an error screen, it's likely because hello cloud service you've deployed is using a [guest OS that does not include .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span></span>  <span data-ttu-id="09b52-149">Vám může tento problém obejít tím [nasazení .NET 4.5.2 jako úloha spuštění](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="09b52-149">You can work around this issue by [deploying .NET 4.5.2 as a startup task](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span></span>
   > 
   > 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="09b52-150">Vytvoření nového profilu CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-150">Create a new CDN profile</span></span>
<span data-ttu-id="09b52-151">Profil CDN je kolekcí koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-151">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="09b52-152">Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-152">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="09b52-153">Můžete toouse více profilů tooorganize koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.</span><span class="sxs-lookup"><span data-stu-id="09b52-153">You may wish toouse multiple profiles tooorganize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!TIP]
> <span data-ttu-id="09b52-154">Pokud již máte profil CDN, že chcete toouse pro účely tohoto kurzu, pokračujte příliš[vytvořte nový koncový bod CDN](#create-a-new-cdn-endpoint).</span><span class="sxs-lookup"><span data-stu-id="09b52-154">If you already have a CDN profile that you want toouse for this tutorial, proceed too[Create a new CDN endpoint](#create-a-new-cdn-endpoint).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="09b52-155">Vytvoření nového koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-155">Create a new CDN endpoint</span></span>
<span data-ttu-id="09b52-156">**toocreate nový koncový bod CDN pro váš účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="09b52-156">**toocreate a new CDN endpoint for your storage account**</span></span>

1. <span data-ttu-id="09b52-157">V hello [portálu pro správu Azure](https://portal.azure.com), přejděte tooyour profil CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-157">In hello [Azure Management Portal](https://portal.azure.com), navigate tooyour CDN profile.</span></span>  <span data-ttu-id="09b52-158">Je může mít připnutý toohello řídicí panel v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-158">You may have pinned it toohello dashboard in hello previous step.</span></span>  <span data-ttu-id="09b52-159">Pokud není, můžete najdete ho kliknutím na **Procházet**, pak **profilů CDN**, a kliknutím na profil hello máte v plánu tooadd koncový bod.</span><span class="sxs-lookup"><span data-stu-id="09b52-159">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on hello profile you plan tooadd your endpoint to.</span></span>
   
    <span data-ttu-id="09b52-160">Otevře se okno profil CDN Hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-160">hello CDN profile blade appears.</span></span>
   
    ![Profil CDN][cdn-profile-settings]
2. <span data-ttu-id="09b52-162">Klikněte na tlačítko hello **přidat koncový bod** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09b52-162">Click hello **Add Endpoint** button.</span></span>
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    <span data-ttu-id="09b52-164">Hello **přidání koncového bodu** se zobrazí okno.</span><span class="sxs-lookup"><span data-stu-id="09b52-164">hello **Add an endpoint** blade appears.</span></span>
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. <span data-ttu-id="09b52-166">Zadejte **Název** tohoto koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-166">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="09b52-167">Tento název bude použité tooaccess prostředkům v mezipaměti v doméně hello `<EndpointName>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="09b52-167">This name will be used tooaccess your cached resources at hello domain `<EndpointName>.azureedge.net`.</span></span>
4. <span data-ttu-id="09b52-168">V hello **typ původu** rozevíracího seznamu vyberte *Cloudová služba*.</span><span class="sxs-lookup"><span data-stu-id="09b52-168">In hello **Origin type** dropdown, select *Cloud service*.</span></span>  
5. <span data-ttu-id="09b52-169">V hello **název počátečního hostitele** rozevíracího seznamu, vyberte cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="09b52-169">In hello **Origin hostname** dropdown, select your cloud service.</span></span>
6. <span data-ttu-id="09b52-170">Ponechte výchozí hodnoty hello **cesty ke zdroji**, **hlavičky hostitele počátku**, a **port protokolu nebo původu**.</span><span class="sxs-lookup"><span data-stu-id="09b52-170">Leave hello defaults for **Origin path**, **Origin host header**, and **Protocol/Origin port**.</span></span>  <span data-ttu-id="09b52-171">Je nutné zadat aspoň jeden protokol (HTTP nebo HTTPS).</span><span class="sxs-lookup"><span data-stu-id="09b52-171">You must specify at least one protocol (HTTP or HTTPS).</span></span>
7. <span data-ttu-id="09b52-172">Klikněte na tlačítko hello **přidat** tlačítko toocreate hello nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="09b52-172">Click hello **Add** button toocreate hello new endpoint.</span></span>
8. <span data-ttu-id="09b52-173">Po vytvoření koncového bodu hello se zobrazí v seznamu koncových bodů pro profil hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-173">Once hello endpoint is created, it appears in a list of endpoints for hello profile.</span></span> <span data-ttu-id="09b52-174">zobrazení seznamu Hello ukazuje, že hello URL toouse tooaccess do mezipaměti obsah, a také doménu původu hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-174">hello list view shows hello URL toouse tooaccess cached content, as well as hello origin domain.</span></span>
   
    ![Koncový bod CDN][cdn-endpoint-success]
   
   > [!NOTE]
   > <span data-ttu-id="09b52-176">Hello koncový bod nebude hned dostupný pro použití.</span><span class="sxs-lookup"><span data-stu-id="09b52-176">hello endpoint will not immediately be available for use.</span></span>  <span data-ttu-id="09b52-177">To může trvat až minut too90 hello registrace toopropagate přes síť CDN hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-177">It can take up too90 minutes for hello registration toopropagate through hello CDN network.</span></span> <span data-ttu-id="09b52-178">Uživatelů, kteří chtějí název domény CDN hello toouse okamžitě obdržet stavový kód 404, dokud nebude k dispozici prostřednictvím hello CDN hello obsah.</span><span class="sxs-lookup"><span data-stu-id="09b52-178">Users who try toouse hello CDN domain name immediately may receive status code 404 until hello content is available via hello CDN.</span></span>
   > 
   > 

## <a name="test-hello-cdn-endpoint"></a><span data-ttu-id="09b52-179">Test hello koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-179">Test hello CDN endpoint</span></span>
<span data-ttu-id="09b52-180">Pokud je stav publikování hello **dokončeno**, otevřete okno prohlížeče a přejděte příliš**http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span><span class="sxs-lookup"><span data-stu-id="09b52-180">When hello publishing status is **Completed**, open a browser window and navigate too**http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span></span> <span data-ttu-id="09b52-181">V části Moje nastavení je tato adresa URL:</span><span class="sxs-lookup"><span data-stu-id="09b52-181">In my setup, this URL is:</span></span>

    http://camservice.azureedge.net/Content/bootstrap.css

<span data-ttu-id="09b52-182">Která odpovídá toohello následující původní adresu URL na koncový bod CDN hello:</span><span class="sxs-lookup"><span data-stu-id="09b52-182">Which corresponds toohello following origin URL at hello CDN endpoint:</span></span>

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

<span data-ttu-id="09b52-183">Když přejdete příliš**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, v závislosti na prohlížeči, bude výzvami toodownload nebo otevřete hello bootstrap.css, pocházejí z vaší publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="09b52-183">When you navigate too**http://*&lt;cdnName>*.azureedge.net/Content/bootstrap.css**, depending on your browser, you will be prompted toodownload or open hello bootstrap.css that came from your published Web app.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

<span data-ttu-id="09b52-184">Stejně tak přístup k jakékoli veřejně přístupné URL u  **http://*&lt;serviceName >*.cloudapp.net/** přímo z vašeho koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-184">You can similarly access any publicly accessible URL at **http://*&lt;serviceName>*.cloudapp.net/**, straight from your CDN endpoint.</span></span> <span data-ttu-id="09b52-185">Například:</span><span class="sxs-lookup"><span data-stu-id="09b52-185">For example:</span></span>

* <span data-ttu-id="09b52-186">Soubor .js z cesty/Script hello</span><span class="sxs-lookup"><span data-stu-id="09b52-186">A .js file from hello /Script path</span></span>
* <span data-ttu-id="09b52-187">Všechny soubory obsahu z hello/Content cesta</span><span class="sxs-lookup"><span data-stu-id="09b52-187">Any content file from hello /Content path</span></span>
* <span data-ttu-id="09b52-188">Kontroler nebo akce</span><span class="sxs-lookup"><span data-stu-id="09b52-188">Any controller/action</span></span>
* <span data-ttu-id="09b52-189">Pokud řetězec dotazu hello je povoleno na koncový bod CDN, libovolná adresa URL s řetězci dotazů</span><span class="sxs-lookup"><span data-stu-id="09b52-189">If hello query string is enabled at your CDN endpoint, any URL with query strings</span></span>

<span data-ttu-id="09b52-190">Ve skutečnosti s hello výše konfigurace, může hostovat hello celý cloudové služby z  **http://*&lt;cdnName >*.azureedge.net/**. Pokud I přejděte příliš**http://camservice.azureedge.net/ ** dostat výsledek akce hello z domovské nebo Index.</span><span class="sxs-lookup"><span data-stu-id="09b52-190">In fact, with hello above configuration, you can host hello entire cloud service from **http://*&lt;cdnName>*.azureedge.net/**. If I navigate too**http://camservice.azureedge.net/**, I get hello action result from Home/Index.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

<span data-ttu-id="09b52-191">To neznamená, ale, že je vždy vhodné tooserve celý cloudové služby prostřednictvím Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-191">This does not mean, however, that it's always a good idea tooserve an entire cloud service through Azure CDN.</span></span> 

<span data-ttu-id="09b52-192">CDN s optimalizací statické doručení není zrychlení nutně doručování dynamické prostředky, které nejsou určeny toobe do mezipaměti, nebo jsou aktualizovány velmi často, protože hello CDN musí novou verzi hello asset načítat z hello zdrojový server velmi často.</span><span class="sxs-lookup"><span data-stu-id="09b52-192">A CDN with static delivery optimization does not necessarily speed up delivery of dynamic assets which are not meant toobe cached, or are updated very frequently, since hello CDN must pull a new version of hello asset from hello Origin server very often.</span></span> <span data-ttu-id="09b52-193">V tomto scénáři můžete povolit [dynamické akcelerace lokality](cdn-dynamic-site-acceleration.md) optimalizace (DSA) na váš koncový bod CDN, která využívá různé techniky toospeed až doručení neurčené dynamické prostředků.</span><span class="sxs-lookup"><span data-stu-id="09b52-193">For this scenario, you can enable [Dynamic Site Acceleration](cdn-dynamic-site-acceleration.md) optimization (DSA) on your CDN endpoint which uses various techniques toospeed up delivery of non-cacheable dynamic assets.</span></span> 

<span data-ttu-id="09b52-194">Pokud máte síť se smíšenými statické a dynamické obsah, můžete tooserve statický obsah z CDN s typem statické optimalizace (například doručení obecné webové) a tooserve dynamický obsah přímo ze serveru původu hello nebo prostřednictvím název CDN koncový bod s optimalizací DSA zapnutá případ od případu.</span><span class="sxs-lookup"><span data-stu-id="09b52-194">If you have a site with a mix of static and dynamic content, you can choose tooserve your static content from CDN with a static optimization type (such as general web delivery), and tooserve dynamic content either directly from hello origin server, or through a CDN endpoint with DSA optimization turned on a case-by-case basis.</span></span> <span data-ttu-id="09b52-195">toothat end jste už viděli, jak jednotlivé obsah tooaccess souborů z koncového bodu CDN hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-195">toothat end, you have already seen how tooaccess individual content files from hello CDN endpoint.</span></span> <span data-ttu-id="09b52-196">I vám ukáže, jak tooserve konkrétní řadič akce prostřednictvím konkrétní koncový bod CDN v sloužit obsahu z akce kontroleru prostřednictvím Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-196">I will show you how tooserve a specific controller action through a specific CDN endpoint in Serve content from controller actions through Azure CDN.</span></span>

<span data-ttu-id="09b52-197">Hello alternativou je toodetermine, který obsahu tooserve z Azure CDN v případech v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="09b52-197">hello alternative is toodetermine which content tooserve from Azure CDN on a case-by-case basis in your cloud service.</span></span> <span data-ttu-id="09b52-198">toothat end jste už viděli, jak jednotlivé obsah tooaccess souborů z koncového bodu CDN hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-198">toothat end, you have already seen how tooaccess individual content files from hello CDN endpoint.</span></span> <span data-ttu-id="09b52-199">Můžu si ukážeme, jak tooserve konkrétní řadič akce prostřednictvím hello koncový bod CDN v [poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller).</span><span class="sxs-lookup"><span data-stu-id="09b52-199">I will show you how tooserve a specific controller action through hello CDN endpoint in [Serve content from controller actions through Azure CDN](#controller).</span></span>

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a><span data-ttu-id="09b52-200">Konfigurace možností ukládání do mezipaměti pro statické soubory v rámci cloudové služby</span><span class="sxs-lookup"><span data-stu-id="09b52-200">Configure caching options for static files in your cloud service</span></span>
<span data-ttu-id="09b52-201">Díky integraci Azure CDN v cloudové službě můžete určit, jak se mají statického obsahu toobe uložené v mezipaměti v koncový bod CDN hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-201">With Azure CDN integration in your cloud service, you can specify how you want static content toobe cached in hello CDN endpoint.</span></span> <span data-ttu-id="09b52-202">toodo tuto, otevřete *Web.config* z vaší webové role projektu (např. WebRole1) a přidejte `<staticContent>` element příliš`<system.webServer>`.</span><span class="sxs-lookup"><span data-stu-id="09b52-202">toodo this, open *Web.config* from your Web role project (e.g. WebRole1) and add a `<staticContent>` element too`<system.webServer>`.</span></span> <span data-ttu-id="09b52-203">Hello XML níže nakonfiguruje hello mezipaměti tooexpire v 3 dny.</span><span class="sxs-lookup"><span data-stu-id="09b52-203">hello XML below configures hello cache tooexpire in 3 days.</span></span>  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

<span data-ttu-id="09b52-204">Jakmile to uděláte, bude sledovat všechny statické soubory v rámci cloudové služby hello stejné pravidlo v mezipaměti CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-204">Once you do this, all static files in your cloud service will observe hello same rule in your CDN cache.</span></span> <span data-ttu-id="09b52-205">K podrobnějšímu řízení nastavení mezipaměti, přidejte *Web.config* soubor do složky a přidat nastavení existuje.</span><span class="sxs-lookup"><span data-stu-id="09b52-205">For more granular control of cache settings, add a *Web.config* file into a folder and add your settings there.</span></span> <span data-ttu-id="09b52-206">Například přidejte *Web.config* souboru toohello *\Content* složky a nahradit text hello obsah s hello následující XML:</span><span class="sxs-lookup"><span data-stu-id="09b52-206">For example, add a *Web.config* file toohello *\Content* folder and replace hello content with hello following XML:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

<span data-ttu-id="09b52-207">Toto nastavení způsobí, že všechny statické soubory z hello *\Content* toobe Složka uložená v mezipaměti pro 15 dnů.</span><span class="sxs-lookup"><span data-stu-id="09b52-207">This setting causes all static files from hello *\Content* folder toobe cached for 15 days.</span></span>

<span data-ttu-id="09b52-208">Další informace o tom, tooconfigure hello `<clientCache>` elementu, najdete v části [mezipaměti klienta &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span><span class="sxs-lookup"><span data-stu-id="09b52-208">For more information on how tooconfigure hello `<clientCache>` element, see [Client Cache &lt;clientCache>](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span></span>

<span data-ttu-id="09b52-209">V [poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller), I také ukazují, jak můžete nakonfigurovat nastavení mezipaměti pro výsledky akce kontroleru v hello CDN mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="09b52-209">In [Serve content from controller actions through Azure CDN](#controller), I will also show you how you can configure cache settings for controller action results in hello CDN cache.</span></span>

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a><span data-ttu-id="09b52-210">Poskytovat obsah z akce kontroleru prostřednictvím Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-210">Serve content from controller actions through Azure CDN</span></span>
<span data-ttu-id="09b52-211">Při integraci s Azure CDN webové role cloudové služby, je poměrně snadné tooserve obsah z akce kontroleru prostřednictvím hello Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-211">When you integrate a cloud service Web role with Azure CDN, it is relatively easy tooserve content from controller actions through hello Azure CDN.</span></span> <span data-ttu-id="09b52-212">Než obsluhující vaše cloudové služby přímo přes Azure CDN (ukázán výše), [Maarten Balliauw](https://twitter.com/maartenballiauw) ukazuje, jak toodo její zábavné MemeGenerator řadiče v [sníží se latence na webu hello s hello Azure CDN ](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span><span class="sxs-lookup"><span data-stu-id="09b52-212">Other than serving your cloud service directly through Azure CDN (demonstrated above), [Maarten Balliauw](https://twitter.com/maartenballiauw) shows you how toodo it with a fun MemeGenerator controller in [Reducing latency on hello web with hello Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span></span> <span data-ttu-id="09b52-213">I bude jednoduše ho znovu vyvolali sem.</span><span class="sxs-lookup"><span data-stu-id="09b52-213">I will simply reproduce it here.</span></span>

<span data-ttu-id="09b52-214">Předpokládejme, že v rámci cloudové služby na základě malí bitové kopie Karel Norris memes toogenerate chcete (fotografii podle [Jakub Light](http://www.flickr.com/photos/alan-light/218493788/)) podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="09b52-214">Suppose in your cloud service you want toogenerate memes based on a young Chuck Norris image (photo by [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) like this:</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

<span data-ttu-id="09b52-215">Máte jednoduchou `Index` akce, která umožňuje zákazníkům hello toospecify hello jejich hello obrázku, pak generuje hello meme po vystavení toohello akce.</span><span class="sxs-lookup"><span data-stu-id="09b52-215">You have a simple `Index` action that allows hello customers toospecify hello superlatives in hello image, then generates hello meme once they post toohello action.</span></span> <span data-ttu-id="09b52-216">Vzhledem k tomu, že je Karel Norris, kterou byste očekávali tuto stránku toobecome velkým oblíbených globálně.</span><span class="sxs-lookup"><span data-stu-id="09b52-216">Since it's Chuck Norris, you would expect this page toobecome wildly popular globally.</span></span> <span data-ttu-id="09b52-217">Toto je dobrým příkladem obsluhovat polovičním dynamický obsah s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-217">This is a good example of serving semi-dynamic content with Azure CDN.</span></span>

<span data-ttu-id="09b52-218">Postupujte podle kroků hello výše toosetup tato akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="09b52-218">Follow hello steps above toosetup this controller action:</span></span>

1. <span data-ttu-id="09b52-219">V hello *\Controllers* složky, vytvořte nový soubor .cs s názvem *MemeGeneratorController.cs* a nahraďte hello obsahu s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="09b52-219">In hello *\Controllers* folder, create a new .cs file called *MemeGeneratorController.cs* and replace hello content with hello following code.</span></span> <span data-ttu-id="09b52-220">Být jisti tooreplace hello zvýrazněná část s název CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-220">Be sure tooreplace hello highlighted portion with your CDN name.</span></span>  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve hello debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. <span data-ttu-id="09b52-221">Klikněte pravým tlačítkem na výchozí hello `Index()` akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="09b52-221">Right-click in hello default `Index()` action and select **Add View**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. <span data-ttu-id="09b52-222">Přijměte hello následující nastavení a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="09b52-222">Accept hello settings below and click **Add**.</span></span>
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. <span data-ttu-id="09b52-223">Otevřete nový hello *Views\MemeGenerator\Index.cshtml* a nahraďte obsah hello hello následující jednoduché HTML pro odesílání jejich hello:</span><span class="sxs-lookup"><span data-stu-id="09b52-223">Open hello new *Views\MemeGenerator\Index.cshtml* and replace hello content with hello following simple HTML for submitting hello superlatives:</span></span>
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. <span data-ttu-id="09b52-224">Znovu publikovat hello cloudové služby a přejděte příliš**http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="09b52-224">Publish hello cloud service again and navigate too**http://*&lt;serviceName>*.cloudapp.net/MemeGenerator/Index** in your browser.</span></span>

<span data-ttu-id="09b52-225">Po odeslání formuláře hodnoty hello příliš`/MemeGenerator/Index`, hello `Index_Post` metoda akce vrací odkaz toohello `Show` metoda akce s příslušnými vstupní identifikátor hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-225">When you submit hello form values too`/MemeGenerator/Index`, hello `Index_Post` action method returns a link toohello `Show` action method with hello respective input identifier.</span></span> <span data-ttu-id="09b52-226">Při kliknutí na odkaz hello nedostanete hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="09b52-226">When you click hello link, you reach hello following code:</span></span>  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve hello debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

<span data-ttu-id="09b52-227">Pokud je připojena vaše místní ladicí program, obdržíte hello regulární ladění zkušenosti s místní přesměrování.</span><span class="sxs-lookup"><span data-stu-id="09b52-227">If your local debugger is attached, then you will get hello regular debug experience with a local redirect.</span></span> <span data-ttu-id="09b52-228">Pokud je spuštěn v hello cloudové služby, se přesměruje na:</span><span class="sxs-lookup"><span data-stu-id="09b52-228">If it's running in hello cloud service, then it will redirect to:</span></span>

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

<span data-ttu-id="09b52-229">Která odpovídá toohello původní adresu URL na koncový bod CDN následující:</span><span class="sxs-lookup"><span data-stu-id="09b52-229">Which corresponds toohello following origin URL at your CDN endpoint:</span></span>

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


<span data-ttu-id="09b52-230">Pak můžete použít hello `OutputCacheAttribute` atribut hello `Generate` metoda toospecify jak by měla být v mezipaměti hello výsledek akce, který bude respektovat Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-230">You can then use hello `OutputCacheAttribute` attribute on hello `Generate` method toospecify how hello action result should be cached, which Azure CDN will honor.</span></span> <span data-ttu-id="09b52-231">Následující kód Hello zadejte vypršení platnosti mezipaměti 1 hodina (3 600 sekund).</span><span class="sxs-lookup"><span data-stu-id="09b52-231">hello code below specify a cache expiration of 1 hour (3,600 seconds).</span></span>

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

<span data-ttu-id="09b52-232">Podobně můžete může sloužit obsah z jakékoli akce kontroleru v rámci cloudové služby prostřednictvím Azure CDN, s možností ukládání do mezipaměti hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="09b52-232">Likewise, you can serve up content from any controller action in your cloud service through your Azure CDN, with hello desired caching option.</span></span>

<span data-ttu-id="09b52-233">V další části hello I vám ukáže, jak tooserve hello dodávat a minifikovaný skriptů a šablon stylů CSS pomocí Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-233">In hello next section, I will show you how tooserve hello bundled and minified scripts and CSS through Azure CDN.</span></span>

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a><span data-ttu-id="09b52-234">ASP.NET sdružování a minimalizace integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-234">Integrate ASP.NET bundling and minification with Azure CDN</span></span>
<span data-ttu-id="09b52-235">Skripty a šablon stylů CSS předlohy se styly změňte zřídka a prvotní kandidáty pro hello mezipaměti Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-235">Scripts and CSS stylesheets change infrequently and are prime candidates for hello Azure CDN cache.</span></span> <span data-ttu-id="09b52-236">Slouží hello celou webovou roli prostřednictvím Azure CDN je nejjednodušší způsob, jak toointegrate hello sdružování a minimalizace s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-236">Serving hello entire Web role through your Azure CDN is hello easiest way toointegrate bundling and minification with Azure CDN.</span></span> <span data-ttu-id="09b52-237">Ale tak, jak má toodo nemusí být to, I vám ukáže, jak toodo je při zachování hello žádoucí vývojářský činnost ASP.NET sdružování a minimalizace, například:</span><span class="sxs-lookup"><span data-stu-id="09b52-237">However, as you may not want toodo this, I will show you how toodo it while preserving hello desired develper experience of ASP.NET bundling and minification, such as:</span></span>

* <span data-ttu-id="09b52-238">Skvělé ladění režimu prostředí</span><span class="sxs-lookup"><span data-stu-id="09b52-238">Great debug mode experience</span></span>
* <span data-ttu-id="09b52-239">Zjednodušené nasazení</span><span class="sxs-lookup"><span data-stu-id="09b52-239">Streamlined deployment</span></span>
* <span data-ttu-id="09b52-240">Okamžitá aktualizace tooclients pro upgrade verze skriptu nebo šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="09b52-240">Immediate updates tooclients for script/CSS version upgrades</span></span>
* <span data-ttu-id="09b52-241">Nouzový mechanismus, když se nezdaří koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-241">Fallback mechanism when your CDN endpoint fails</span></span>
* <span data-ttu-id="09b52-242">Minimalizovat úpravy kódu</span><span class="sxs-lookup"><span data-stu-id="09b52-242">Minimize code modification</span></span>

<span data-ttu-id="09b52-243">V hello **WebRole1** projekt, který jste vytvořili v [koncový bod Azure CDN integrovat svůj web Azure a poskytovat statický obsah na webových stránkách z Azure CDN](#deploy), otevřete *App_Start\ BundleConfig.cs* a podívejte se na hello `bundles.Add()` volání metody.</span><span class="sxs-lookup"><span data-stu-id="09b52-243">In hello **WebRole1** project that you created in [Integrate an Azure CDN endpoint with your Azure website and serve static content in your Web pages from Azure CDN](#deploy), open *App_Start\BundleConfig.cs* and take a look at hello `bundles.Add()` method calls.</span></span>

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

<span data-ttu-id="09b52-244">nejprve Hello `bundles.Add()` příkaz přidá sady skriptu na virtuální adresář hello `~/bundles/jquery`.</span><span class="sxs-lookup"><span data-stu-id="09b52-244">hello first `bundles.Add()` statement adds a script bundle at hello virtual directory `~/bundles/jquery`.</span></span> <span data-ttu-id="09b52-245">Potom otevřete *Views\Shared\_Layout.cshtml* toosee vykreslení značky sady script hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-245">Then, open *Views\Shared\_Layout.cshtml* toosee how hello script bundle tag is rendered.</span></span> <span data-ttu-id="09b52-246">Měli byste mít možnost toofind hello následující řádek kódu Razor:</span><span class="sxs-lookup"><span data-stu-id="09b52-246">You should be able toofind hello following line of Razor code:</span></span>

    @Scripts.Render("~/bundles/jquery")

<span data-ttu-id="09b52-247">Při spuštění tohoto kódu Razor v roli hello webů Azure, se budou vykreslovat `<script>` značky pro hello skript sady podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="09b52-247">When this Razor code is run in hello Azure Web role, it will render a `<script>` tag for hello script bundle similar toohello following:</span></span>

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

<span data-ttu-id="09b52-248">Ale když ji spustí v sadě Visual Studio zadáním `F5`, se budou vykreslovat každý soubor skriptu v kompletu hello jednotlivě (v případě hello výše pouze jeden soubor skriptu nachází v sady hello):</span><span class="sxs-lookup"><span data-stu-id="09b52-248">However, when it is run in Visual Studio by typing `F5`, it will render each script file in hello bundle individually (in hello case above, only one script file is in hello bundle):</span></span>

    <script src="/Scripts/jquery-1.10.2.js"></script>

<span data-ttu-id="09b52-249">To vám umožní kódu jazyka JavaScript hello toodebug ve vašem vývojovém prostředí při snížení souběžných klientských připojení (sdružování) a vylepšuje soubor stáhnout výkonu (minimalizace) v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="09b52-249">This enables you toodebug hello JavaScript code in your development environment while reducing concurrent client connections (bundling) and improving file download performance (minification) in production.</span></span> <span data-ttu-id="09b52-250">Je skvělé funkce toopreserve díky integraci Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-250">It's a great feature toopreserve with Azure CDN integration.</span></span> <span data-ttu-id="09b52-251">Navíc vzhledem k tomu, že sady hello vykresluje již obsahuje řetězec automaticky generované verze, chcete tooreplicate, který funkce proto hello při každé aktualizaci vaší verzí jQuery prostřednictvím balíčku NuGet, mohou být aktualizovány na straně klienta hello co nejrychleji možné.</span><span class="sxs-lookup"><span data-stu-id="09b52-251">Furthermore, since hello rendered bundle already contains an automatically generated version string, you want tooreplicate that functionality so hello whenever you update your jQuery version through NuGet, it can be updated at hello client side as soon as possible.</span></span>

<span data-ttu-id="09b52-252">Postupujte podle kroků hello toointegration ASP.NET sdružování a minimalizace s koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-252">Follow hello steps below toointegration ASP.NET bundling and minification with your CDN endpoint.</span></span>

1. <span data-ttu-id="09b52-253">Zpět v *App_Start\BundleConfig.cs*, upravte hello `bundles.Add()` jiné metody toouse [sady konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), ten, který určuje adresu CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-253">Back in *App_Start\BundleConfig.cs*, modify hello `bundles.Add()` methods toouse a different [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), one that specifies a CDN address.</span></span> <span data-ttu-id="09b52-254">toodo se nahradit hello `RegisterBundles` definici metody s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="09b52-254">toodo this, replace hello `RegisterBundles` method definition with hello following code:</span></span>  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you're
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="09b52-255">Být jisti tooreplace `<yourCDNName>` s názvem hello Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-255">Be sure tooreplace `<yourCDNName>` with hello name of your Azure CDN.</span></span>
   
    <span data-ttu-id="09b52-256">Uveďte stručný nastavujete `bundles.UseCdn = true` a přidat pečlivě vytvořenou sadu tooeach CDN URL.</span><span class="sxs-lookup"><span data-stu-id="09b52-256">In plain words, you are setting `bundles.UseCdn = true` and added a carefully crafted CDN URL tooeach bundle.</span></span> <span data-ttu-id="09b52-257">Například hello první konstruktor v kódu hello:</span><span class="sxs-lookup"><span data-stu-id="09b52-257">For example, hello first constructor in hello code:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    <span data-ttu-id="09b52-258">je hello totéž jako:</span><span class="sxs-lookup"><span data-stu-id="09b52-258">is hello same as:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    <span data-ttu-id="09b52-259">Tento konstruktor informuje ASP.NET sdružování a minimalizace soubory jednotlivých skriptu toorender při ladit místně, ale použití hello zadán CDN adresu tooaccess hello skriptu nejistá.</span><span class="sxs-lookup"><span data-stu-id="09b52-259">This constructor tells ASP.NET bundling and minification toorender individual script files when debugged locally, but use hello specified CDN address tooaccess hello script in question.</span></span> <span data-ttu-id="09b52-260">Pamatujte však dvě důležité charakteristiky s Tento pečlivě vytvořené CDN adresy URL:</span><span class="sxs-lookup"><span data-stu-id="09b52-260">However, note two important characteristics with this carefully crafted CDN URL:</span></span>
   
   * <span data-ttu-id="09b52-261">Hello původ pro tuto adresu URL CDN `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, který je ve skutečnosti hello virtuální adresář sady hello skript v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="09b52-261">hello origin for this CDN URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, which is actually hello virtual directory of hello script bundle in your cloud service.</span></span>
   * <span data-ttu-id="09b52-262">Vzhledem k tomu, že používáte konstruktor CDN, hello značky script CDN pro sadu hello už obsahuje hello automaticky generované řetězec verze ve hello vykresluje adresy URL.</span><span class="sxs-lookup"><span data-stu-id="09b52-262">Since you are using CDN constructor, hello CDN script tag for hello bundle no longer contains hello automatically generated version string in hello rendered URL.</span></span> <span data-ttu-id="09b52-263">Řetězec verze jedinečný musíte vygenerovat ručně pokaždé, když hello skript sady je upravený tooforce mezipaměti neproběhly v Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-263">You must manually generate a unique version string every time hello script bundle is modified tooforce a cache miss at your Azure CDN.</span></span> <span data-ttu-id="09b52-264">Na hello stejný čas, musí zůstat po nasazení sady hello konstantní prostřednictvím hello životnosti hello nasazení toomaximize přístupů do mezipaměti v Azure CDN tento řetězec jedinečný verze.</span><span class="sxs-lookup"><span data-stu-id="09b52-264">At hello same time, this unique version string must remain constant through hello life of hello deployment toomaximize cache hits at your Azure CDN after hello bundle is deployed.</span></span>
   * <span data-ttu-id="09b52-265">řetězec dotazu v Hello = < W.X.Y.Z > si z *Properties\AssemblyInfo.cs* v projektu webové role.</span><span class="sxs-lookup"><span data-stu-id="09b52-265">hello query string v=<W.X.Y.Z> pulls from *Properties\AssemblyInfo.cs* in your Web role project.</span></span> <span data-ttu-id="09b52-266">Může mít nasazení pracovního postupu, který zahrnuje zvyšování verze sestavení hello pokaždé, když publikujete tooAzure.</span><span class="sxs-lookup"><span data-stu-id="09b52-266">You can have a deployment workflow that includes incrementing hello assembly version every time you publish tooAzure.</span></span> <span data-ttu-id="09b52-267">Nebo můžete upravit pouze *Properties\AssemblyInfo.cs* ve vašem projektu tooautomatically přírůstek hello verze řetězci pokaždé, když vytvoříte, pomocí hello zástupný znak ' *'.</span><span class="sxs-lookup"><span data-stu-id="09b52-267">Or, you can just modify *Properties\AssemblyInfo.cs* in your project tooautomatically increment hello version string every time you build, using hello wildcard character '*'.</span></span> <span data-ttu-id="09b52-268">Například:</span><span class="sxs-lookup"><span data-stu-id="09b52-268">For example:</span></span>
     
        <span data-ttu-id="09b52-269">[sestavení: AssemblyVersion("1.0.0.*")]</span><span class="sxs-lookup"><span data-stu-id="09b52-269">[assembly: AssemblyVersion("1.0.0.*")]</span></span>
     
     <span data-ttu-id="09b52-270">Další strategie toostreamline generování jedinečného řetězce dobu životnosti hello nasazení bude fungovat v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="09b52-270">Any other strategy toostreamline generating a unique string for hello life of a deployment will work here.</span></span>
2. <span data-ttu-id="09b52-271">Znovu publikujte hello cloudové služby a přístup hello domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="09b52-271">Republish hello cloud service and access hello home page.</span></span>
3. <span data-ttu-id="09b52-272">Zobrazení hello kód HTML pro stránku hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-272">View hello HTML code for hello page.</span></span> <span data-ttu-id="09b52-273">Musí být schopný toosee hello CDN URL vykresleno, řetězcem jedinečný verze pokaždé, když je znovu publikovat změny tooyour cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="09b52-273">You should be able toosee hello CDN URL rendered, with a unique version string every time you republish changes tooyour cloud service.</span></span> <span data-ttu-id="09b52-274">Například:</span><span class="sxs-lookup"><span data-stu-id="09b52-274">For example:</span></span>  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. <span data-ttu-id="09b52-275">V sadě Visual Studio, ladění hello cloudové služby v sadě Visual Studio zadáním `F5`.,</span><span class="sxs-lookup"><span data-stu-id="09b52-275">In Visual Studio, debug hello cloud service in Visual Studio by typing `F5`.,</span></span>
5. <span data-ttu-id="09b52-276">Zobrazení hello kód HTML pro stránku hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-276">View hello HTML code for hello page.</span></span> <span data-ttu-id="09b52-277">Zobrazí se stále každý soubor skriptu jednotlivě vykreslen tak, že máte konzistentní ladění prostředí v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09b52-277">You will still see each script file individually rendered so that you can have a consistent debug experience in Visual Studio.</span></span>  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a><span data-ttu-id="09b52-278">Nouzový mechanismus adresy URL CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-278">Fallback mechanism for CDN URLs</span></span>
<span data-ttu-id="09b52-279">Pokud z nějakého důvodu selže koncový bod Azure CDN, budete chtít vaší webové stránky toobe inteligentní dostatek tooaccess váš počátek webový server jako záložní volbu hello načítání JavaScript nebo Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="09b52-279">When your Azure CDN endpoint fails for any reason, you want your Web page toobe smart enough tooaccess your origin Web server as hello fallback option for loading JavaScript or Bootstrap.</span></span> <span data-ttu-id="09b52-280">Je dostatečně závažné toolose obrázků na váš web z důvodu nedostupnosti tooCDN, ale mnohem závažnější funkce zásadní stránky toolose poskytované službou skriptů a šablon.</span><span class="sxs-lookup"><span data-stu-id="09b52-280">It's serious enough toolose images on your website due tooCDN unavailability, but much more severe toolose crucial page functionality provided by your scripts and stylesheets.</span></span>

<span data-ttu-id="09b52-281">Hello [sady](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) třída obsahuje vlastnost s názvem [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , která umožní tooconfigure hello nouzový mechanismus selhání CDN.</span><span class="sxs-lookup"><span data-stu-id="09b52-281">hello [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) class contains a property called [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) that enables you tooconfigure hello fallback mechanism for CDN failure.</span></span> <span data-ttu-id="09b52-282">toouse tuto vlastnost, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="09b52-282">toouse this property, follow hello steps below:</span></span>

1. <span data-ttu-id="09b52-283">Otevřete v projektu webové role *App_Start\BundleConfig.cs*, které jste přidali adresu URL CDN v každé [sady konstruktor](http://msdn.microsoft.com/library/jj646464.aspx)a proveďte následující hello zvýrazněná změny tooadd nouzový mechanismus, který toohello výchozí sady:</span><span class="sxs-lookup"><span data-stu-id="09b52-283">In your Web role project, open *App_Start\BundleConfig.cs*, where you added a CDN URL in each [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), and make hello following highlighted changes tooadd fallback mechanism toohello default bundles:</span></span>  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="09b52-284">Když `CdnFallbackExpression` je hodnotou not null, skript je vloženy do hello HTML tootest zda byla úspěšně zavedena hello sady a pokud ne, přístup k sadě hello přímo z hello původu webového serveru.</span><span class="sxs-lookup"><span data-stu-id="09b52-284">When `CdnFallbackExpression` is not null, script is injected into hello HTML tootest whether hello bundle is loaded successfully and, if not, access hello bundle directly from hello origin Web server.</span></span> <span data-ttu-id="09b52-285">Tato vlastnost musí toobe sady tooa JavaScript výraz, který kontroluje, zda hello příslušné sady CDN je načtena správně.</span><span class="sxs-lookup"><span data-stu-id="09b52-285">This property needs toobe set tooa JavaScript expression that tests whether hello respective CDN bundle is loaded properly.</span></span> <span data-ttu-id="09b52-286">výraz Hello potřeby tootest každého svazku se liší podle toohello obsah.</span><span class="sxs-lookup"><span data-stu-id="09b52-286">hello expression needed tootest each bundle differs according toohello content.</span></span> <span data-ttu-id="09b52-287">Pro výchozí sady hello výše:</span><span class="sxs-lookup"><span data-stu-id="09b52-287">For hello default bundles above:</span></span>
   
   * <span data-ttu-id="09b52-288">`window.jquery`je definována v jquery-{version} .js</span><span class="sxs-lookup"><span data-stu-id="09b52-288">`window.jquery` is defined in jquery-{version}.js</span></span>
   * <span data-ttu-id="09b52-289">`$.validator`je definována v jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="09b52-289">`$.validator` is defined in jquery.validate.js</span></span>
   * <span data-ttu-id="09b52-290">`window.Modernizr`je definována v modernizer-{version} .js</span><span class="sxs-lookup"><span data-stu-id="09b52-290">`window.Modernizr` is defined in modernizer-{version}.js</span></span>
   * <span data-ttu-id="09b52-291">`$.fn.modal`je definována v bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="09b52-291">`$.fn.modal` is defined in bootstrap.js</span></span>
     
     <span data-ttu-id="09b52-292">Možná jste si všimli I nenastavili CdnFallbackExpression pro hello `~/Cointent/css` sady.</span><span class="sxs-lookup"><span data-stu-id="09b52-292">You might have noticed that I did not set CdnFallbackExpression for hello `~/Cointent/css` bundle.</span></span> <span data-ttu-id="09b52-293">Důvodem je, že je nyní [chyb v System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) , vloží `<script>` značky pro hello záložní CSS místo hello očekává `<link>` značky.</span><span class="sxs-lookup"><span data-stu-id="09b52-293">This is because currently there is a [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) that injects a `<script>` tag for hello fallback CSS instead of hello expected `<link>` tag.</span></span>
     
     <span data-ttu-id="09b52-294">Existuje, ale velká [záložní styl sady](https://github.com/EmberConsultingGroup/StyleBundleFallback) nabízené [členskými konzultace ohledně skupiny](https://github.com/EmberConsultingGroup).</span><span class="sxs-lookup"><span data-stu-id="09b52-294">There is, however, a good [Style Bundle Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) offered by [Ember Consulting Group](https://github.com/EmberConsultingGroup).</span></span>
2. <span data-ttu-id="09b52-295">alternativní řešení hello toouse pro šablon stylů CSS, vytvořte nový soubor .cs v projektu webové role *App_Start* složku s názvem *StyleBundleExtensions.cs*a nahraďte jeho obsah hello [kódu z GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="09b52-295">toouse hello workaround for CSS, create a new .cs file in your Web role project's *App_Start* folder called *StyleBundleExtensions.cs*, and replace its content with hello [code from GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span></span>
3. <span data-ttu-id="09b52-296">V *App_Start\StyleFundleExtensions.cs*, přejmenujte hello obor názvů tooyour webové role název (například **WebRole1**).</span><span class="sxs-lookup"><span data-stu-id="09b52-296">In *App_Start\StyleFundleExtensions.cs*, rename hello namespace tooyour Web role's name (e.g. **WebRole1**).</span></span>
4. <span data-ttu-id="09b52-297">Přejděte zpět příliš`App_Start\BundleConfig.cs` a poslední změna hello `bundles.Add` příkaz s hello následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="09b52-297">Go back too`App_Start\BundleConfig.cs` and modify hello last `bundles.Add` statement with hello following highlighted code:</span></span>  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    <span data-ttu-id="09b52-298">Tato nová metoda rozšíření používá hello stejné nápad tooinject skript v modelu DOM hello toocheck hello HTML pro hello odpovídající název třídy, název pravidla a pravidla hodnota definovaná v sady hello šablon stylů CSS a spadá back toohello počátek webový server Pokud selže toofind hello shodu.</span><span class="sxs-lookup"><span data-stu-id="09b52-298">This new extension method uses hello same idea tooinject script in hello HTML toocheck hello DOM for hello a matching class name, rule name, and rule value defined in hello CSS bundle, and falls back toohello origin Web server if it fails toofind hello match.</span></span>
5. <span data-ttu-id="09b52-299">Publikujte znovu hello cloudové služby a přístup hello domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="09b52-299">Publish hello cloud service again and access hello home page.</span></span>
6. <span data-ttu-id="09b52-300">Zobrazení hello kód HTML pro stránku hello.</span><span class="sxs-lookup"><span data-stu-id="09b52-300">View hello HTML code for hello page.</span></span> <span data-ttu-id="09b52-301">Byste měli najít vloženého skripty podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="09b52-301">You should find injected scripts similar toohello following:</span></span>    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    <span data-ttu-id="09b52-302">Vložený skript pro sady šablon stylů CSS hello stále obsahuje nesprávně pracujících zbývajících hello z hello `CdnFallbackExpression` vlastnost hello řádku:</span><span class="sxs-lookup"><span data-stu-id="09b52-302">Note that injected script for hello CSS bundle still contains hello errant remnant from hello `CdnFallbackExpression` property in hello line:</span></span>

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    <span data-ttu-id="09b52-303">Ale protože hello první část hello || výraz vždy vrátí hodnotu PRAVDA (na řádku hello přímo vyšší), bude funkce document.write() hello nebude nikdy spuštěn.</span><span class="sxs-lookup"><span data-stu-id="09b52-303">But since hello first part of hello || expression will always return true (in hello line directly above that), hello document.write() function will never run.</span></span>

## <a name="more-information"></a><span data-ttu-id="09b52-304">Další informace</span><span class="sxs-lookup"><span data-stu-id="09b52-304">More Information</span></span>
* [<span data-ttu-id="09b52-305">Přehled hello Azure Content Delivery Network (CDN)</span><span class="sxs-lookup"><span data-stu-id="09b52-305">Overview of hello Azure Content Delivery Network (CDN)</span></span>](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [<span data-ttu-id="09b52-306">Používání Azure CDN</span><span class="sxs-lookup"><span data-stu-id="09b52-306">Using Azure CDN</span></span>](cdn-create-new-endpoint.md)
* [<span data-ttu-id="09b52-307">ASP.NET sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="09b52-307">ASP.NET Bundling and Minification</span></span>](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
