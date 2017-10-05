---
title: "Cloudové služby Azure integrovat Azure CDN | Microsoft Docs"
description: "Informace o nasazení Cloudová služba, která poskytuje obsah z integrované koncového bodu Azure CDN"
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
ms.openlocfilehash: f2849fe25fd0d5b3dc26598ffba7591cb7433161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <span data-ttu-id="ffde1-103"><a name="intro"></a>Cloudové služby integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-103"><a name="intro"></a> Integrate a cloud service with Azure CDN</span></span>
<span data-ttu-id="ffde1-104">Cloudové služby můžete integrovat Azure CDN, obsluhující žádný obsah z cloudové služby umístění.</span><span class="sxs-lookup"><span data-stu-id="ffde1-104">A cloud service can be integrated with Azure CDN, serving any content from the cloud service's location.</span></span> <span data-ttu-id="ffde1-105">Tento přístup poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ffde1-105">This approach gives you the following advantages:</span></span>

* <span data-ttu-id="ffde1-106">Můžete snadno nasadit a aktualizaci bitové kopie, skriptů a šablon v adresářích projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ffde1-106">Easily deploy and update images, scripts, and stylesheets in your cloud service's project directories</span></span>
* <span data-ttu-id="ffde1-107">Snadno upgradujte balíčků NuGet v rámci cloudové služby, například jQuery nebo Bootstrap verze</span><span class="sxs-lookup"><span data-stu-id="ffde1-107">Easily upgrade the NuGet packages in your cloud service, such as jQuery or Bootstrap versions</span></span>
* <span data-ttu-id="ffde1-108">Správa webové aplikace a vaše všechna obsahu CDN obsluhovat ze stejné rozhraní sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffde1-108">Manage your Web application and your CDN-served content all from the same Visual Studio interface</span></span>
* <span data-ttu-id="ffde1-109">Pracovní postup jednotná nasazení pro webové aplikace a obsah obsluhuje CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-109">Unified deployment workflow for your Web application and your CDN-served content</span></span>
* <span data-ttu-id="ffde1-110">ASP.NET sdružování a minimalizace integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-110">Integrate ASP.NET bundling and minification with Azure CDN</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ffde1-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="ffde1-111">What you will learn</span></span>
<span data-ttu-id="ffde1-112">V tomto kurzu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="ffde1-112">In this tutorial, you will learn how to:</span></span>

* [<span data-ttu-id="ffde1-113">Koncový bod Azure CDN integrovat s cloudovou službou a poskytovat statický obsah na webových stránkách z Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-113">Integrate an Azure CDN endpoint with your cloud service and serve static content in your Web pages from Azure CDN</span></span>](#deploy)
* [<span data-ttu-id="ffde1-114">Konfigurace nastavení mezipaměti pro statický obsah v rámci cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ffde1-114">Configure cache settings for static content in your cloud service</span></span>](#caching)
* [<span data-ttu-id="ffde1-115">Poskytovat obsah z akce kontroleru prostřednictvím Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-115">Serve content from controller actions through Azure CDN</span></span>](#controller)
* [<span data-ttu-id="ffde1-116">Používat spojeno dohromady a minifikovaný obsah prostřednictvím Azure CDN při zachování skript ladění prostředí v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffde1-116">Serve bundled and minified content through Azure CDN while preserving the script debugging experience in Visual Studio</span></span>](#bundling)
* [<span data-ttu-id="ffde1-117">Konfigurace záložního skriptů a šablon stylů CSS při offline Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-117">Configure fallback your scripts and CSS when your Azure CDN is offline</span></span>](#fallback)

## <a name="what-you-will-build"></a><span data-ttu-id="ffde1-118">Co se sestavení</span><span class="sxs-lookup"><span data-stu-id="ffde1-118">What you will build</span></span>
<span data-ttu-id="ffde1-119">Nasazení cloudové služby webové role pomocí výchozí šablony ASP.NET MVC, přidáte kód pro práci s obsahem z integrované CDN Azure, jako je například bitovou kopii, výsledky akce kontroleru a výchozí souborů JavaScript a CSS a také napsat kód pro konfigurace záložní mechanismus pro sady zpracovat v případě, že CDN je offline.</span><span class="sxs-lookup"><span data-stu-id="ffde1-119">You will deploy a cloud service Web role using the default ASP.NET MVC template, add code to serve content from an integrated Azure CDN, such as an image, controller action results, and the default JavaScript and CSS files, and also write code to configure the fallback mechanism for bundles served in the event that the CDN is offline.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="ffde1-120">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="ffde1-120">What you will need</span></span>
<span data-ttu-id="ffde1-121">V tomto kurzu má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="ffde1-121">This tutorial has the following prerequisites:</span></span>

* <span data-ttu-id="ffde1-122">Aktivní [účtem Microsoft Azure.](/account/)</span><span class="sxs-lookup"><span data-stu-id="ffde1-122">An active [Microsoft Azure account](/account/)</span></span>
* <span data-ttu-id="ffde1-123">Visual Studio 2015 se [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="ffde1-123">Visual Studio 2015 with [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span></span>

> [!NOTE]
> <span data-ttu-id="ffde1-124">K dokončení tohoto kurzu potřebujete mít účet Azure:</span><span class="sxs-lookup"><span data-stu-id="ffde1-124">You need an Azure account to complete this tutorial:</span></span>
> 
> * <span data-ttu-id="ffde1-125">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure, jako jsou weby.</span><span class="sxs-lookup"><span data-stu-id="ffde1-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services, such as Websites.</span></span>
> * <span data-ttu-id="ffde1-126">Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ffde1-126">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a><span data-ttu-id="ffde1-127">Nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ffde1-127">Deploy a cloud service</span></span>
<span data-ttu-id="ffde1-128">V této části nasazení výchozí šablony aplikace ASP.NET MVC v sadě Visual Studio 2015 do cloudové služby webové role a potom ji integrovat s nástrojem nový koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-128">In this section, you will deploy the default ASP.NET MVC application template in Visual Studio 2015 to a cloud service Web role, and then integrate it with a new CDN endpoint.</span></span> <span data-ttu-id="ffde1-129">Postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="ffde1-129">Follow the instructions below:</span></span>

1. <span data-ttu-id="ffde1-130">V sadě Visual Studio 2015, vytvořte novou službu Azure cloud z řádku nabídek přechodem na **soubor > Nový > Projekt > Cloud > Cloudová služba Azure**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-130">In Visual Studio 2015, create a new Azure cloud service from the menu bar by going to **File > New > Project > Cloud > Azure Cloud Service**.</span></span> <span data-ttu-id="ffde1-131">Pojmenujte ho a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-131">Give it a name and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. <span data-ttu-id="ffde1-132">Vyberte **webovou roli ASP.NET** a klikněte na  **>**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ffde1-132">Select **ASP.NET Web Role** and click the **>** button.</span></span> <span data-ttu-id="ffde1-133">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="ffde1-133">Click OK.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. <span data-ttu-id="ffde1-134">Vyberte **MVC** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-134">Select **MVC** and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. <span data-ttu-id="ffde1-135">Nyní publikování této webové role do cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ffde1-135">Now, publish this Web role to an Azure cloud service.</span></span> <span data-ttu-id="ffde1-136">Klikněte pravým tlačítkem na projekt cloudové služby a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-136">Right-click the cloud service project and select **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. <span data-ttu-id="ffde1-137">Pokud jste ještě nebyly přihlášeni Microsoft Azure, klikněte na **přidat účet...**  rozevíracího seznamu a klikněte na tlačítko **přidat účet** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="ffde1-137">If you have not yet signed into Microsoft Azure, click the **Add an account...** dropdown and click the **Add an account** menu item.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. <span data-ttu-id="ffde1-138">Na stránce přihlášení Přihlaste se pomocí účtu Microsoft, který jste použili k aktivaci účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="ffde1-138">In the sign-in page, sign in with the Microsoft account you used to activate your Azure account.</span></span>
7. <span data-ttu-id="ffde1-139">Jakmile jste přihlášení, klikněte na možnost **Další**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-139">Once you're signed in, click **Next**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. <span data-ttu-id="ffde1-140">Za předpokladu, že jste nevytvořili účet cloudové služby nebo úložiště, Visual Studio vám pomůže vytvořit obě.</span><span class="sxs-lookup"><span data-stu-id="ffde1-140">Assuming that you haven't created a cloud service or storage account, Visual Studio will help you create both.</span></span> <span data-ttu-id="ffde1-141">V **vytvoření cloudové služby a účet** dialogové okno, zadejte název požadované služby a vyberte požadovanou oblast.</span><span class="sxs-lookup"><span data-stu-id="ffde1-141">In the **Create Cloud Service and Account** dialog, type the desired service name and select the desired region.</span></span> <span data-ttu-id="ffde1-142">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-142">Then, click **Create**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. <span data-ttu-id="ffde1-143">Na stránce Nastavení publikování ověřte konfiguraci a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-143">In the publish settings page, verify the configuration and click **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > <span data-ttu-id="ffde1-144">Proces publikování pro cloudové služby trvá příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="ffde1-144">The publishing process for cloud services takes a long time.</span></span> <span data-ttu-id="ffde1-145">Povolit nasazení webu pro všechny role možnost provádět ladění mnohem rychlejší cloudové služby tím, že poskytuje rychlé (ale dočasné) aktualizace webové role.</span><span class="sxs-lookup"><span data-stu-id="ffde1-145">The Enable Web Deploy for all roles option can make debugging your cloud service much quicker by providing fast (but temporary) updates to your Web roles.</span></span> <span data-ttu-id="ffde1-146">Další informace o této možnosti najdete v tématu [publikování cloudové služby pomocí nástroje Azure](http://msdn.microsoft.com/library/ff683672.aspx).</span><span class="sxs-lookup"><span data-stu-id="ffde1-146">For more information on this option, see [Publishing a Cloud Service using the Azure Tools](http://msdn.microsoft.com/library/ff683672.aspx).</span></span>
   > 
   > 
   
    <span data-ttu-id="ffde1-147">Když **protokoly aktivit Microsoft Azure** ukazuje, že stav publikování je **dokončeno**, vytvoří koncový bod CDN, která je integrovaná s touto cloudovou službou.</span><span class="sxs-lookup"><span data-stu-id="ffde1-147">When the **Microsoft Azure Activity Log** shows that publishing status is **Completed**, you will create a CDN endpoint that's integrated with this cloud service.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="ffde1-148">Pokud po publikování, nasazené cloudové služby zobrazí chybové obrazovce, je pravděpodobné, protože používá cloudové služby, které jste nasadili [hosta operační systém, který nezahrnuje .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span><span class="sxs-lookup"><span data-stu-id="ffde1-148">If, after publishing, the deployed cloud service displays an error screen, it's likely because the cloud service you've deployed is using a [guest OS that does not include .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span></span>  <span data-ttu-id="ffde1-149">Vám může tento problém obejít tím [nasazení .NET 4.5.2 jako úloha spuštění](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ffde1-149">You can work around this issue by [deploying .NET 4.5.2 as a startup task](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span></span>
   > 
   > 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="ffde1-150">Vytvoření nového profilu CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-150">Create a new CDN profile</span></span>
<span data-ttu-id="ffde1-151">Profil CDN je kolekcí koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-151">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="ffde1-152">Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-152">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="ffde1-153">Můžete použít více profilů a uspořádat koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.</span><span class="sxs-lookup"><span data-stu-id="ffde1-153">You may wish to use multiple profiles to organize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!TIP]
> <span data-ttu-id="ffde1-154">Pokud již máte profil CDN, který chcete použít pro tento kurz, pokračujte [vytvořte nový koncový bod CDN](#create-a-new-cdn-endpoint).</span><span class="sxs-lookup"><span data-stu-id="ffde1-154">If you already have a CDN profile that you want to use for this tutorial, proceed to [Create a new CDN endpoint](#create-a-new-cdn-endpoint).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="ffde1-155">Vytvoření nového koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-155">Create a new CDN endpoint</span></span>
<span data-ttu-id="ffde1-156">**Chcete-li vytvořit nový koncový bod CDN pro váš účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="ffde1-156">**To create a new CDN endpoint for your storage account**</span></span>

1. <span data-ttu-id="ffde1-157">V [portálu pro správu Azure](https://portal.azure.com), přejděte na svůj profil CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-157">In the [Azure Management Portal](https://portal.azure.com), navigate to your CDN profile.</span></span>  <span data-ttu-id="ffde1-158">Je možné, že jste si ho v předchozím kroku připnuli k řídicímu panelu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-158">You may have pinned it to the dashboard in the previous step.</span></span>  <span data-ttu-id="ffde1-159">Pokud ne, najdete ho kliknutím na položku **Procházet**, poté položku **Profily CDN** a nakonec kliknutím na profil, ke kterému plánujete přidat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ffde1-159">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on the profile you plan to add your endpoint to.</span></span>
   
    <span data-ttu-id="ffde1-160">Otevře se okno Profil CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-160">The CDN profile blade appears.</span></span>
   
    ![Profil CDN][cdn-profile-settings]
2. <span data-ttu-id="ffde1-162">Klikněte na tlačítko **Přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-162">Click the **Add Endpoint** button.</span></span>
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    <span data-ttu-id="ffde1-164">Otevře se okno **Přidání koncového bodu**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-164">The **Add an endpoint** blade appears.</span></span>
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. <span data-ttu-id="ffde1-166">Zadejte **Název** tohoto koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-166">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="ffde1-167">Tento název se použije pro přístup k prostředkům v mezipaměti v doméně `<EndpointName>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="ffde1-167">This name will be used to access your cached resources at the domain `<EndpointName>.azureedge.net`.</span></span>
4. <span data-ttu-id="ffde1-168">V **typ původu** rozevíracího seznamu vyberte *Cloudová služba*.</span><span class="sxs-lookup"><span data-stu-id="ffde1-168">In the **Origin type** dropdown, select *Cloud service*.</span></span>  
5. <span data-ttu-id="ffde1-169">V **název počátečního hostitele** rozevíracího seznamu, vyberte cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-169">In the **Origin hostname** dropdown, select your cloud service.</span></span>
6. <span data-ttu-id="ffde1-170">Ponechte výchozí nastavení pro **cesty ke zdroji**, **hlavičky hostitele počátku**, a **port protokolu nebo původu**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-170">Leave the defaults for **Origin path**, **Origin host header**, and **Protocol/Origin port**.</span></span>  <span data-ttu-id="ffde1-171">Je nutné zadat aspoň jeden protokol (HTTP nebo HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ffde1-171">You must specify at least one protocol (HTTP or HTTPS).</span></span>
7. <span data-ttu-id="ffde1-172">Kliknutím na tlačítko **Přidat** vytvořte nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ffde1-172">Click the **Add** button to create the new endpoint.</span></span>
8. <span data-ttu-id="ffde1-173">Jakmile je koncový bod vytvořený, zobrazí se v seznamu koncových bodů daného profilu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-173">Once the endpoint is created, it appears in a list of endpoints for the profile.</span></span> <span data-ttu-id="ffde1-174">Zobrazení seznamu zobrazuje adresu URL, kterou je nutné použít k přístupu k obsahu v mezipaměti, a také doménu původu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-174">The list view shows the URL to use to access cached content, as well as the origin domain.</span></span>
   
    ![Koncový bod CDN][cdn-endpoint-success]
   
   > [!NOTE]
   > <span data-ttu-id="ffde1-176">Koncový bod nebude hned dostupný pro použití.</span><span class="sxs-lookup"><span data-stu-id="ffde1-176">The endpoint will not immediately be available for use.</span></span>  <span data-ttu-id="ffde1-177">To může trvat až 90 minut, než se registrace rozšíří v síti CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-177">It can take up to 90 minutes for the registration to propagate through the CDN network.</span></span> <span data-ttu-id="ffde1-178">Uživatelé, kteří se pokusí použít název domény CDN okamžitě obdržet stavový kód 404, dokud nebude obsah k dispozici prostřednictvím CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-178">Users who try to use the CDN domain name immediately may receive status code 404 until the content is available via the CDN.</span></span>
   > 
   > 

## <a name="test-the-cdn-endpoint"></a><span data-ttu-id="ffde1-179">Testování koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-179">Test the CDN endpoint</span></span>
<span data-ttu-id="ffde1-180">Pokud je stav publikování **dokončeno**, otevřete okno prohlížeče a přejděte do  **http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-180">When the publishing status is **Completed**, open a browser window and navigate to **http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span></span> <span data-ttu-id="ffde1-181">V části Moje nastavení je tato adresa URL:</span><span class="sxs-lookup"><span data-stu-id="ffde1-181">In my setup, this URL is:</span></span>

    http://camservice.azureedge.net/Content/bootstrap.css

<span data-ttu-id="ffde1-182">Která odpovídá následující původní adresu URL na koncový bod CDN:</span><span class="sxs-lookup"><span data-stu-id="ffde1-182">Which corresponds to the following origin URL at the CDN endpoint:</span></span>

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

<span data-ttu-id="ffde1-183">Když přejdete na  **http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, v závislosti na prohlížeči, zobrazí se výzva stáhnout nebo otevřít bootstrap.css, které byly dodány z vaší publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffde1-183">When you navigate to **http://*&lt;cdnName>*.azureedge.net/Content/bootstrap.css**, depending on your browser, you will be prompted to download or open the bootstrap.css that came from your published Web app.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

<span data-ttu-id="ffde1-184">Stejně tak přístup k jakékoli veřejně přístupné URL u  **http://*&lt;serviceName >*.cloudapp.net/** přímo z vašeho koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-184">You can similarly access any publicly accessible URL at **http://*&lt;serviceName>*.cloudapp.net/**, straight from your CDN endpoint.</span></span> <span data-ttu-id="ffde1-185">Například:</span><span class="sxs-lookup"><span data-stu-id="ffde1-185">For example:</span></span>

* <span data-ttu-id="ffde1-186">Soubor .js z cesty/Script</span><span class="sxs-lookup"><span data-stu-id="ffde1-186">A .js file from the /Script path</span></span>
* <span data-ttu-id="ffde1-187">Všechny soubory obsahu z/Content cesta</span><span class="sxs-lookup"><span data-stu-id="ffde1-187">Any content file from the /Content path</span></span>
* <span data-ttu-id="ffde1-188">Kontroler nebo akce</span><span class="sxs-lookup"><span data-stu-id="ffde1-188">Any controller/action</span></span>
* <span data-ttu-id="ffde1-189">Pokud řetězec dotazu je povoleno na koncový bod CDN, libovolná adresa URL s řetězci dotazů</span><span class="sxs-lookup"><span data-stu-id="ffde1-189">If the query string is enabled at your CDN endpoint, any URL with query strings</span></span>

<span data-ttu-id="ffde1-190">Ve skutečnosti s výše konfigurací, můžete hostovat službu celý cloudu v  **http://*&lt;cdnName >*.azureedge.net/**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-190">In fact, with the above configuration, you can host the entire cloud service from **http://*&lt;cdnName>*.azureedge.net/**.</span></span> <span data-ttu-id="ffde1-191">Pokud I přejděte na **http://camservice.azureedge.net/**, výsledek akce dostat z domovské nebo Index.</span><span class="sxs-lookup"><span data-stu-id="ffde1-191">If I navigate to **http://camservice.azureedge.net/**, I get the action result from Home/Index.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

<span data-ttu-id="ffde1-192">To neznamená, ale, že je vždy vhodné k obsluze celý cloudové služby prostřednictvím Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-192">This does not mean, however, that it's always a good idea to serve an entire cloud service through Azure CDN.</span></span> 

<span data-ttu-id="ffde1-193">CDN s optimalizací statické doručení není nutně urychlí práci doručování dynamické prostředky, které nejsou určeny do mezipaměti, nebo jsou aktualizovány velmi často, protože CDN musí novou verzi asset vyžádat ze zdrojového serveru velmi často.</span><span class="sxs-lookup"><span data-stu-id="ffde1-193">A CDN with static delivery optimization does not necessarily speed up delivery of dynamic assets which are not meant to be cached, or are updated very frequently, since the CDN must pull a new version of the asset from the Origin server very often.</span></span> <span data-ttu-id="ffde1-194">V tomto scénáři můžete povolit [dynamické akcelerace lokality](cdn-dynamic-site-acceleration.md) optimalizace (DSA) na váš koncový bod CDN, která využívá různé postupy pro urychlení doručení neurčené dynamické prostředků.</span><span class="sxs-lookup"><span data-stu-id="ffde1-194">For this scenario, you can enable [Dynamic Site Acceleration](cdn-dynamic-site-acceleration.md) optimization (DSA) on your CDN endpoint which uses various techniques to speed up delivery of non-cacheable dynamic assets.</span></span> 

<span data-ttu-id="ffde1-195">Pokud máte síť se smíšenými statické a dynamické obsah, můžete poskytovat statický obsah z CDN s typem statické optimalizace (například doručení obecné webové) a poskytovat dynamický obsah přímo ze zdrojového serveru, nebo prostřednictvím w koncový bod CDN Optimalizace DSA i-tým zapnout případ od případu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-195">If you have a site with a mix of static and dynamic content, you can choose to serve your static content from CDN with a static optimization type (such as general web delivery), and to serve dynamic content either directly from the origin server, or through a CDN endpoint with DSA optimization turned on a case-by-case basis.</span></span> <span data-ttu-id="ffde1-196">Za tímto účelem jste už viděli, jak k jednotlivé soubory obsahu z koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-196">To that end, you have already seen how to access individual content files from the CDN endpoint.</span></span> <span data-ttu-id="ffde1-197">I vám ukáže, jak používat obsah z akce kontroleru prostřednictvím Azure CDN mohly konkrétní řadič akce prostřednictvím konkrétní koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-197">I will show you how to serve a specific controller action through a specific CDN endpoint in Serve content from controller actions through Azure CDN.</span></span>

<span data-ttu-id="ffde1-198">Alternativou je zjistit, které obsah v případech v cloudové službě působit z Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-198">The alternative is to determine which content to serve from Azure CDN on a case-by-case basis in your cloud service.</span></span> <span data-ttu-id="ffde1-199">Za tímto účelem jste už viděli, jak k jednotlivé soubory obsahu z koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-199">To that end, you have already seen how to access individual content files from the CDN endpoint.</span></span> <span data-ttu-id="ffde1-200">I vám ukáže, jak k obsluze konkrétní řadič akce prostřednictvím koncového bodu CDN v [poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller).</span><span class="sxs-lookup"><span data-stu-id="ffde1-200">I will show you how to serve a specific controller action through the CDN endpoint in [Serve content from controller actions through Azure CDN](#controller).</span></span>

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a><span data-ttu-id="ffde1-201">Konfigurace možností ukládání do mezipaměti pro statické soubory v rámci cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ffde1-201">Configure caching options for static files in your cloud service</span></span>
<span data-ttu-id="ffde1-202">Díky integraci Azure CDN v cloudové službě můžete určit, jak se mají statický obsah do mezipaměti v koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-202">With Azure CDN integration in your cloud service, you can specify how you want static content to be cached in the CDN endpoint.</span></span> <span data-ttu-id="ffde1-203">Chcete-li to provést, otevřete *Web.config* z vaší webové role projektu (např. WebRole1) a přidejte `<staticContent>` element `<system.webServer>`.</span><span class="sxs-lookup"><span data-stu-id="ffde1-203">To do this, open *Web.config* from your Web role project (e.g. WebRole1) and add a `<staticContent>` element to `<system.webServer>`.</span></span> <span data-ttu-id="ffde1-204">Zadaný kód XML nakonfiguruje mezipaměti vyprší za 3 dny.</span><span class="sxs-lookup"><span data-stu-id="ffde1-204">The XML below configures the cache to expire in 3 days.</span></span>  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

<span data-ttu-id="ffde1-205">Jakmile to uděláte, bude sledovat všechny statické soubory v rámci cloudové služby stejného pravidla v mezipaměti CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-205">Once you do this, all static files in your cloud service will observe the same rule in your CDN cache.</span></span> <span data-ttu-id="ffde1-206">K podrobnějšímu řízení nastavení mezipaměti, přidejte *Web.config* soubor do složky a přidat nastavení existuje.</span><span class="sxs-lookup"><span data-stu-id="ffde1-206">For more granular control of cache settings, add a *Web.config* file into a folder and add your settings there.</span></span> <span data-ttu-id="ffde1-207">Například přidejte *Web.config* do souboru *\Content* složky a nahraďte obsah s následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="ffde1-207">For example, add a *Web.config* file to the *\Content* folder and replace the content with the following XML:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

<span data-ttu-id="ffde1-208">Toto nastavení způsobí, že všechny statické soubory z *\Content* složky ukládat do mezipaměti 15 dní.</span><span class="sxs-lookup"><span data-stu-id="ffde1-208">This setting causes all static files from the *\Content* folder to be cached for 15 days.</span></span>

<span data-ttu-id="ffde1-209">Další informace o tom, jak nakonfigurovat `<clientCache>` elementu, najdete v části [mezipaměti klienta &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span><span class="sxs-lookup"><span data-stu-id="ffde1-209">For more information on how to configure the `<clientCache>` element, see [Client Cache &lt;clientCache>](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span></span>

<span data-ttu-id="ffde1-210">V [poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller), I také ukazují, jak můžete konfigurovat nastavení mezipaměti pro výsledky akce kontroleru v mezipaměti CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-210">In [Serve content from controller actions through Azure CDN](#controller), I will also show you how you can configure cache settings for controller action results in the CDN cache.</span></span>

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a><span data-ttu-id="ffde1-211">Poskytovat obsah z akce kontroleru prostřednictvím Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-211">Serve content from controller actions through Azure CDN</span></span>
<span data-ttu-id="ffde1-212">Při integraci s Azure CDN webové role cloudové služby, je poměrně snadné ho poskytovat obsah z akce kontroleru prostřednictvím Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-212">When you integrate a cloud service Web role with Azure CDN, it is relatively easy to serve content from controller actions through the Azure CDN.</span></span> <span data-ttu-id="ffde1-213">Než obsluhující vaše cloudové služby přímo přes Azure CDN (ukázán výše), [Maarten Balliauw](https://twitter.com/maartenballiauw) ukazuje, jak to udělat pomocí zábavné MemeGenerator řadiče v [sníží se latence na webu pomocí Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span><span class="sxs-lookup"><span data-stu-id="ffde1-213">Other than serving your cloud service directly through Azure CDN (demonstrated above), [Maarten Balliauw](https://twitter.com/maartenballiauw) shows you how to do it with a fun MemeGenerator controller in [Reducing latency on the web with the Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span></span> <span data-ttu-id="ffde1-214">I bude jednoduše ho znovu vyvolali sem.</span><span class="sxs-lookup"><span data-stu-id="ffde1-214">I will simply reproduce it here.</span></span>

<span data-ttu-id="ffde1-215">Předpokládejme, že ve vašem cloudu podle služby, kterou chcete vygenerovat memes bitovou kopii malí Karel Norris (fotografii podle [Jakub Light](http://www.flickr.com/photos/alan-light/218493788/)) podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="ffde1-215">Suppose in your cloud service you want to generate memes based on a young Chuck Norris image (photo by [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) like this:</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

<span data-ttu-id="ffde1-216">Máte jednoduchou `Index` akce, která umožňuje zákazníkům zadejte jejich v bitové kopii, pak vygeneruje meme po jejich odeslání na akci.</span><span class="sxs-lookup"><span data-stu-id="ffde1-216">You have a simple `Index` action that allows the customers to specify the superlatives in the image, then generates the meme once they post to the action.</span></span> <span data-ttu-id="ffde1-217">Vzhledem k tomu, že je Karel Norris, kterou byste očekávali tuto stránku k velkým oblíbených globálně.</span><span class="sxs-lookup"><span data-stu-id="ffde1-217">Since it's Chuck Norris, you would expect this page to become wildly popular globally.</span></span> <span data-ttu-id="ffde1-218">Toto je dobrým příkladem obsluhovat polovičním dynamický obsah s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-218">This is a good example of serving semi-dynamic content with Azure CDN.</span></span>

<span data-ttu-id="ffde1-219">Výše uvedený postup k nastavení této akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="ffde1-219">Follow the steps above to setup this controller action:</span></span>

1. <span data-ttu-id="ffde1-220">V *\Controllers* složky, vytvořte nový soubor .cs s názvem *MemeGeneratorController.cs* a nahraďte obsah následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="ffde1-220">In the *\Controllers* folder, create a new .cs file called *MemeGeneratorController.cs* and replace the content with the following code.</span></span> <span data-ttu-id="ffde1-221">Nezapomeňte nahradit název CDN zvýrazněná část.</span><span class="sxs-lookup"><span data-stu-id="ffde1-221">Be sure to replace the highlighted portion with your CDN name.</span></span>  
   
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
   
                    if (Debugger.IsAttached) // Preserve the debug experience
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
2. <span data-ttu-id="ffde1-222">Klikněte pravým tlačítkem na výchozí `Index()` akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-222">Right-click in the default `Index()` action and select **Add View**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. <span data-ttu-id="ffde1-223">Přijměte níže uvedených nastavení a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ffde1-223">Accept the settings below and click **Add**.</span></span>
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. <span data-ttu-id="ffde1-224">Otevřete nový *Views\MemeGenerator\Index.cshtml* a nahraďte následující jednoduché HTML pro odesílání jejich obsah:</span><span class="sxs-lookup"><span data-stu-id="ffde1-224">Open the new *Views\MemeGenerator\Index.cshtml* and replace the content with the following simple HTML for submitting the superlatives:</span></span>
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. <span data-ttu-id="ffde1-225">Znovu publikovat cloudové služby a přejděte do  **http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ffde1-225">Publish the cloud service again and navigate to **http://*&lt;serviceName>*.cloudapp.net/MemeGenerator/Index** in your browser.</span></span>

<span data-ttu-id="ffde1-226">Po odeslání formuláře hodnoty tak, aby `/MemeGenerator/Index`, `Index_Post` metoda akce vrací odkaz `Show` metoda akce s příslušnými vstupní identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ffde1-226">When you submit the form values to `/MemeGenerator/Index`, the `Index_Post` action method returns a link to the `Show` action method with the respective input identifier.</span></span> <span data-ttu-id="ffde1-227">Když kliknete na odkaz, dostanete následující kód:</span><span class="sxs-lookup"><span data-stu-id="ffde1-227">When you click the link, you reach the following code:</span></span>  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

<span data-ttu-id="ffde1-228">Pokud je připojena vaše místní ladicí program, obdržíte regulární ladění zkušenosti s místní přesměrování.</span><span class="sxs-lookup"><span data-stu-id="ffde1-228">If your local debugger is attached, then you will get the regular debug experience with a local redirect.</span></span> <span data-ttu-id="ffde1-229">Pokud je spuštěn v rámci cloudové služby, se přesměruje na:</span><span class="sxs-lookup"><span data-stu-id="ffde1-229">If it's running in the cloud service, then it will redirect to:</span></span>

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

<span data-ttu-id="ffde1-230">Která odpovídá následující původní adresu URL na koncový bod CDN:</span><span class="sxs-lookup"><span data-stu-id="ffde1-230">Which corresponds to the following origin URL at your CDN endpoint:</span></span>

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


<span data-ttu-id="ffde1-231">Pak můžete použít `OutputCacheAttribute` atributu u `Generate` metoda k určení, jak výsledek akce do mezipaměti, který bude respektovat Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-231">You can then use the `OutputCacheAttribute` attribute on the `Generate` method to specify how the action result should be cached, which Azure CDN will honor.</span></span> <span data-ttu-id="ffde1-232">Kód uvedený níže zadejte vypršení platnosti mezipaměti 1 hodina (3 600 sekund).</span><span class="sxs-lookup"><span data-stu-id="ffde1-232">The code below specify a cache expiration of 1 hour (3,600 seconds).</span></span>

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

<span data-ttu-id="ffde1-233">Podobně můžete může sloužit obsah z jakékoli akce kontroleru v rámci cloudové služby prostřednictvím Azure CDN, s požadovanou možnost ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ffde1-233">Likewise, you can serve up content from any controller action in your cloud service through your Azure CDN, with the desired caching option.</span></span>

<span data-ttu-id="ffde1-234">V další části I vám ukáže, jak k obsluze připojené a minifikovaný skriptů a šablon stylů CSS pomocí Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-234">In the next section, I will show you how to serve the bundled and minified scripts and CSS through Azure CDN.</span></span>

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a><span data-ttu-id="ffde1-235">ASP.NET sdružování a minimalizace integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-235">Integrate ASP.NET bundling and minification with Azure CDN</span></span>
<span data-ttu-id="ffde1-236">Skripty a šablon stylů CSS předlohy se styly změňte zřídka a prvotní kandidáty pro mezipaměť Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-236">Scripts and CSS stylesheets change infrequently and are prime candidates for the Azure CDN cache.</span></span> <span data-ttu-id="ffde1-237">Obsluhující celý webovou roli prostřednictvím Azure CDN je nejjednodušší způsob, jak integrovat Azure CDN sdružování a minimalizace.</span><span class="sxs-lookup"><span data-stu-id="ffde1-237">Serving the entire Web role through your Azure CDN is the easiest way to integrate bundling and minification with Azure CDN.</span></span> <span data-ttu-id="ffde1-238">Ale tak, jak chcete nemusí k tomu, I vám ukáže, jak to udělat při zachování požadované vývojářský možností ASP.NET sdružování a minimalizace, jako například:</span><span class="sxs-lookup"><span data-stu-id="ffde1-238">However, as you may not want to do this, I will show you how to do it while preserving the desired develper experience of ASP.NET bundling and minification, such as:</span></span>

* <span data-ttu-id="ffde1-239">Skvělé ladění režimu prostředí</span><span class="sxs-lookup"><span data-stu-id="ffde1-239">Great debug mode experience</span></span>
* <span data-ttu-id="ffde1-240">Zjednodušené nasazení</span><span class="sxs-lookup"><span data-stu-id="ffde1-240">Streamlined deployment</span></span>
* <span data-ttu-id="ffde1-241">Okamžitá aktualizace klientů pro upgrade verze skriptu nebo šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="ffde1-241">Immediate updates to clients for script/CSS version upgrades</span></span>
* <span data-ttu-id="ffde1-242">Nouzový mechanismus, když se nezdaří koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-242">Fallback mechanism when your CDN endpoint fails</span></span>
* <span data-ttu-id="ffde1-243">Minimalizovat úpravy kódu</span><span class="sxs-lookup"><span data-stu-id="ffde1-243">Minimize code modification</span></span>

<span data-ttu-id="ffde1-244">V **WebRole1** projekt, který jste vytvořili v [koncový bod Azure CDN integrovat svůj web Azure a poskytovat statický obsah na webových stránkách z Azure CDN](#deploy), otevřete *App_Start\ BundleConfig.cs* a podívejte se na `bundles.Add()` volání metody.</span><span class="sxs-lookup"><span data-stu-id="ffde1-244">In the **WebRole1** project that you created in [Integrate an Azure CDN endpoint with your Azure website and serve static content in your Web pages from Azure CDN](#deploy), open *App_Start\BundleConfig.cs* and take a look at the `bundles.Add()` method calls.</span></span>

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

<span data-ttu-id="ffde1-245">První `bundles.Add()` příkaz přidá na virtuální adresář sady skriptu `~/bundles/jquery`.</span><span class="sxs-lookup"><span data-stu-id="ffde1-245">The first `bundles.Add()` statement adds a script bundle at the virtual directory `~/bundles/jquery`.</span></span> <span data-ttu-id="ffde1-246">Potom otevřete *Views\Shared\_Layout.cshtml* zobrazíte vykreslení značky script sady.</span><span class="sxs-lookup"><span data-stu-id="ffde1-246">Then, open *Views\Shared\_Layout.cshtml* to see how the script bundle tag is rendered.</span></span> <span data-ttu-id="ffde1-247">Nyní byste měli mít vyhledejte následující řádek kódu Razor:</span><span class="sxs-lookup"><span data-stu-id="ffde1-247">You should be able to find the following line of Razor code:</span></span>

    @Scripts.Render("~/bundles/jquery")

<span data-ttu-id="ffde1-248">Při spuštění tohoto kódu Razor v roli webů Azure, se budou vykreslovat `<script>` značky pro sady skript, který je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="ffde1-248">When this Razor code is run in the Azure Web role, it will render a `<script>` tag for the script bundle similar to the following:</span></span>

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

<span data-ttu-id="ffde1-249">Ale když ji spustí v sadě Visual Studio zadáním `F5`, se budou vykreslovat každý soubor skriptu v sadě jednotlivě (v případě výše uvedené jenom jeden skript soubor je v sadě):</span><span class="sxs-lookup"><span data-stu-id="ffde1-249">However, when it is run in Visual Studio by typing `F5`, it will render each script file in the bundle individually (in the case above, only one script file is in the bundle):</span></span>

    <script src="/Scripts/jquery-1.10.2.js"></script>

<span data-ttu-id="ffde1-250">To umožňuje ladit kód JavaScript ve vašem vývojovém prostředí při snížení souběžných klientských připojení (sdružování) a vylepšuje soubor stáhnout výkonu (minimalizace) v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ffde1-250">This enables you to debug the JavaScript code in your development environment while reducing concurrent client connections (bundling) and improving file download performance (minification) in production.</span></span> <span data-ttu-id="ffde1-251">Je skvělé funkce, která zachovat díky integraci Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-251">It's a great feature to preserve with Azure CDN integration.</span></span> <span data-ttu-id="ffde1-252">Navíc vzhledem k tomu, že sada vykreslené již obsahuje řetězec automaticky generované verze, které chcete replikovat tato funkcionalita proto při každé aktualizaci vaší verzí jQuery prostřednictvím balíčku NuGet, může být aktualizován na straně klienta co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="ffde1-252">Furthermore, since the rendered bundle already contains an automatically generated version string, you want to replicate that functionality so the whenever you update your jQuery version through NuGet, it can be updated at the client side as soon as possible.</span></span>

<span data-ttu-id="ffde1-253">Postupujte podle těchto kroků k integraci ASP.NET sdružování a minimalizace s koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-253">Follow the steps below to integration ASP.NET bundling and minification with your CDN endpoint.</span></span>

1. <span data-ttu-id="ffde1-254">Zpět v *App_Start\BundleConfig.cs*, změnit `bundles.Add()` můžete použít jiné metody [sady konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), ten, který určuje adresu CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-254">Back in *App_Start\BundleConfig.cs*, modify the `bundles.Add()` methods to use a different [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), one that specifies a CDN address.</span></span> <span data-ttu-id="ffde1-255">Chcete-li to provést, nahraďte `RegisterBundles` definici metody s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ffde1-255">To do this, replace the `RegisterBundles` method definition with the following code:</span></span>  
   
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
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="ffde1-256">Nezapomeňte nahradit `<yourCDNName>` s názvem Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-256">Be sure to replace `<yourCDNName>` with the name of your Azure CDN.</span></span>
   
    <span data-ttu-id="ffde1-257">Uveďte stručný nastavujete `bundles.UseCdn = true` a přidat pečlivě vytvořené adresy URL CDN každého svazku.</span><span class="sxs-lookup"><span data-stu-id="ffde1-257">In plain words, you are setting `bundles.UseCdn = true` and added a carefully crafted CDN URL to each bundle.</span></span> <span data-ttu-id="ffde1-258">Například první konstruktor v kódu:</span><span class="sxs-lookup"><span data-stu-id="ffde1-258">For example, the first constructor in the code:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    <span data-ttu-id="ffde1-259">je stejný jako:</span><span class="sxs-lookup"><span data-stu-id="ffde1-259">is the same as:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    <span data-ttu-id="ffde1-260">Tento konstruktor informuje ASP.NET sdružování a minimalizace soubory jednotlivých skriptu při místně ladit vykreslit, ale zadaná adresa CDN používat pro přístup k dotyčném skriptu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-260">This constructor tells ASP.NET bundling and minification to render individual script files when debugged locally, but use the specified CDN address to access the script in question.</span></span> <span data-ttu-id="ffde1-261">Pamatujte však dvě důležité charakteristiky s Tento pečlivě vytvořené CDN adresy URL:</span><span class="sxs-lookup"><span data-stu-id="ffde1-261">However, note two important characteristics with this carefully crafted CDN URL:</span></span>
   
   * <span data-ttu-id="ffde1-262">Původ pro tuto adresu URL CDN `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, který je ve skutečnosti virtuální adresář sady skript v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ffde1-262">The origin for this CDN URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, which is actually the virtual directory of the script bundle in your cloud service.</span></span>
   * <span data-ttu-id="ffde1-263">Vzhledem k tomu, že používáte konstruktor CDN, značky script CDN pro sadu už obsahuje automaticky generovaný verze řetězec v adrese URL vykreslené.</span><span class="sxs-lookup"><span data-stu-id="ffde1-263">Since you are using CDN constructor, the CDN script tag for the bundle no longer contains the automatically generated version string in the rendered URL.</span></span> <span data-ttu-id="ffde1-264">Řetězec verze jedinečný musíte vygenerovat ručně pokaždé, když je sada skript upravit tak, aby vynutit k neúspěšnému přístupu do mezipaměti v Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-264">You must manually generate a unique version string every time the script bundle is modified to force a cache miss at your Azure CDN.</span></span> <span data-ttu-id="ffde1-265">Ve stejnou dobu musí zůstat tento řetězec jedinečný verze konstantní prostřednictvím dobu životnosti nasazení tak, aby po nasazení sady maximalizovat přístupů k mezipaměti v Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-265">At the same time, this unique version string must remain constant through the life of the deployment to maximize cache hits at your Azure CDN after the bundle is deployed.</span></span>
   * <span data-ttu-id="ffde1-266">Řetězec dotazu v = < W.X.Y.Z > si z *Properties\AssemblyInfo.cs* v projektu webové role.</span><span class="sxs-lookup"><span data-stu-id="ffde1-266">The query string v=<W.X.Y.Z> pulls from *Properties\AssemblyInfo.cs* in your Web role project.</span></span> <span data-ttu-id="ffde1-267">Může mít nasazení pracovního postupu, který zahrnuje zvyšování verze sestavení pokaždé, když publikujete do Azure.</span><span class="sxs-lookup"><span data-stu-id="ffde1-267">You can have a deployment workflow that includes incrementing the assembly version every time you publish to Azure.</span></span> <span data-ttu-id="ffde1-268">Nebo můžete upravit pouze *Properties\AssemblyInfo.cs* ve vašem projektu a automaticky zvýší řetězec verze pokaždé, když vytvoříte, pomocí zástupných znaků ' *'.</span><span class="sxs-lookup"><span data-stu-id="ffde1-268">Or, you can just modify *Properties\AssemblyInfo.cs* in your project to automatically increment the version string every time you build, using the wildcard character '*'.</span></span> <span data-ttu-id="ffde1-269">Například:</span><span class="sxs-lookup"><span data-stu-id="ffde1-269">For example:</span></span>
     
        <span data-ttu-id="ffde1-270">[sestavení: AssemblyVersion("1.0.0.*")]</span><span class="sxs-lookup"><span data-stu-id="ffde1-270">[assembly: AssemblyVersion("1.0.0.*")]</span></span>
     
     <span data-ttu-id="ffde1-271">Zde bude fungovat jakékoli strategie zefektivnění generování jedinečného řetězce po celou dobu životnosti nasazení.</span><span class="sxs-lookup"><span data-stu-id="ffde1-271">Any other strategy to streamline generating a unique string for the life of a deployment will work here.</span></span>
2. <span data-ttu-id="ffde1-272">Přístup na domovskou stránku a znovu publikovat cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ffde1-272">Republish the cloud service and access the home page.</span></span>
3. <span data-ttu-id="ffde1-273">Zobrazte kód HTML pro stránku.</span><span class="sxs-lookup"><span data-stu-id="ffde1-273">View the HTML code for the page.</span></span> <span data-ttu-id="ffde1-274">Nyní byste měli mít najdete v části Adresa URL CDN vykresleno, řetězcem jedinečný verze pokaždé, když je znovu publikovat změny do cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ffde1-274">You should be able to see the CDN URL rendered, with a unique version string every time you republish changes to your cloud service.</span></span> <span data-ttu-id="ffde1-275">Například:</span><span class="sxs-lookup"><span data-stu-id="ffde1-275">For example:</span></span>  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. <span data-ttu-id="ffde1-276">V sadě Visual Studio, ladění cloudové služby v sadě Visual Studio zadáním `F5`.,</span><span class="sxs-lookup"><span data-stu-id="ffde1-276">In Visual Studio, debug the cloud service in Visual Studio by typing `F5`.,</span></span>
5. <span data-ttu-id="ffde1-277">Zobrazte kód HTML pro stránku.</span><span class="sxs-lookup"><span data-stu-id="ffde1-277">View the HTML code for the page.</span></span> <span data-ttu-id="ffde1-278">Zobrazí se stále každý soubor skriptu jednotlivě vykreslen tak, že máte konzistentní ladění prostředí v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffde1-278">You will still see each script file individually rendered so that you can have a consistent debug experience in Visual Studio.</span></span>  
   
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

## <a name="fallback-mechanism-for-cdn-urls"></a><span data-ttu-id="ffde1-279">Nouzový mechanismus adresy URL CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-279">Fallback mechanism for CDN URLs</span></span>
<span data-ttu-id="ffde1-280">Pokud z nějakého důvodu selže koncový bod Azure CDN, chcete být dostatečně inteligentní pro přístup k webovému serveru původu jako záložní volbu pro načítání JavaScript nebo Bootstrap webové stránky.</span><span class="sxs-lookup"><span data-stu-id="ffde1-280">When your Azure CDN endpoint fails for any reason, you want your Web page to be smart enough to access your origin Web server as the fallback option for loading JavaScript or Bootstrap.</span></span> <span data-ttu-id="ffde1-281">Je dostatečně závažné, ztratí obrázků na váš web z důvodu nedostupnosti CDN, ale mnohem závažnější přijít o velmi důležitý stránky funkce poskytované službou skriptů a šablon.</span><span class="sxs-lookup"><span data-stu-id="ffde1-281">It's serious enough to lose images on your website due to CDN unavailability, but much more severe to lose crucial page functionality provided by your scripts and stylesheets.</span></span>

<span data-ttu-id="ffde1-282">[Sady](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) třída obsahuje vlastnost s názvem [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , vám umožňuje nakonfigurovat nouzový mechanismus pro selhání CDN.</span><span class="sxs-lookup"><span data-stu-id="ffde1-282">The [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) class contains a property called [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) that enables you to configure the fallback mechanism for CDN failure.</span></span> <span data-ttu-id="ffde1-283">Tato vlastnost, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="ffde1-283">To use this property, follow the steps below:</span></span>

1. <span data-ttu-id="ffde1-284">Otevřete v projektu webové role *App_Start\BundleConfig.cs*, které jste přidali adresu URL CDN v každé [sady konstruktor](http://msdn.microsoft.com/library/jj646464.aspx)a proveďte následující změny zvýrazněné přidat nouzový mechanismus, který na výchozí hodnoty sady:</span><span class="sxs-lookup"><span data-stu-id="ffde1-284">In your Web role project, open *App_Start\BundleConfig.cs*, where you added a CDN URL in each [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), and make the following highlighted changes to add fallback mechanism to the default bundles:</span></span>  
   
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
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
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
   
    <span data-ttu-id="ffde1-285">Když `CdnFallbackExpression` je hodnotou not null, skript je vložit do kódu HTML do otestovat, zda je sada úspěšně načetl a pokud ne, přístup k sadě přímo z původu webového serveru.</span><span class="sxs-lookup"><span data-stu-id="ffde1-285">When `CdnFallbackExpression` is not null, script is injected into the HTML to test whether the bundle is loaded successfully and, if not, access the bundle directly from the origin Web server.</span></span> <span data-ttu-id="ffde1-286">Tato vlastnost musí být nastavena na výraz jazyka JavaScript, který kontroluje, zda příslušné sady CDN je načtena správně.</span><span class="sxs-lookup"><span data-stu-id="ffde1-286">This property needs to be set to a JavaScript expression that tests whether the respective CDN bundle is loaded properly.</span></span> <span data-ttu-id="ffde1-287">Výraz potřebné k testování každého svazku se liší podle obsahu.</span><span class="sxs-lookup"><span data-stu-id="ffde1-287">The expression needed to test each bundle differs according to the content.</span></span> <span data-ttu-id="ffde1-288">Pro výchozí sady výše:</span><span class="sxs-lookup"><span data-stu-id="ffde1-288">For the default bundles above:</span></span>
   
   * <span data-ttu-id="ffde1-289">`window.jquery`je definována v jquery-{version} .js</span><span class="sxs-lookup"><span data-stu-id="ffde1-289">`window.jquery` is defined in jquery-{version}.js</span></span>
   * <span data-ttu-id="ffde1-290">`$.validator`je definována v jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="ffde1-290">`$.validator` is defined in jquery.validate.js</span></span>
   * <span data-ttu-id="ffde1-291">`window.Modernizr`je definována v modernizer-{version} .js</span><span class="sxs-lookup"><span data-stu-id="ffde1-291">`window.Modernizr` is defined in modernizer-{version}.js</span></span>
   * <span data-ttu-id="ffde1-292">`$.fn.modal`je definována v bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffde1-292">`$.fn.modal` is defined in bootstrap.js</span></span>
     
     <span data-ttu-id="ffde1-293">Možná jste si všimli, že I nenastavili CdnFallbackExpression pro `~/Cointent/css` sady.</span><span class="sxs-lookup"><span data-stu-id="ffde1-293">You might have noticed that I did not set CdnFallbackExpression for the `~/Cointent/css` bundle.</span></span> <span data-ttu-id="ffde1-294">Důvodem je, že je nyní [chyb v System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) , vloží `<script>` značky pro záložní šablon stylů CSS místo očekávané `<link>` značky.</span><span class="sxs-lookup"><span data-stu-id="ffde1-294">This is because currently there is a [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) that injects a `<script>` tag for the fallback CSS instead of the expected `<link>` tag.</span></span>
     
     <span data-ttu-id="ffde1-295">Existuje, ale velká [záložní styl sady](https://github.com/EmberConsultingGroup/StyleBundleFallback) nabízené [členskými konzultace ohledně skupiny](https://github.com/EmberConsultingGroup).</span><span class="sxs-lookup"><span data-stu-id="ffde1-295">There is, however, a good [Style Bundle Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) offered by [Ember Consulting Group](https://github.com/EmberConsultingGroup).</span></span>
2. <span data-ttu-id="ffde1-296">Toto řešení použít pro šablon stylů CSS, vytvořte nový soubor .cs v projektu webové role *App_Start* složku s názvem *StyleBundleExtensions.cs*a nahraďte jeho obsah s [kód z Githubu ](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="ffde1-296">To use the workaround for CSS, create a new .cs file in your Web role project's *App_Start* folder called *StyleBundleExtensions.cs*, and replace its content with the [code from GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span></span>
3. <span data-ttu-id="ffde1-297">V *App_Start\StyleFundleExtensions.cs*, přejmenujte obor názvů pro vaši webovou roli název (například **WebRole1**).</span><span class="sxs-lookup"><span data-stu-id="ffde1-297">In *App_Start\StyleFundleExtensions.cs*, rename the namespace to your Web role's name (e.g. **WebRole1**).</span></span>
4. <span data-ttu-id="ffde1-298">Přejděte zpět na `App_Start\BundleConfig.cs` a upravit poslední `bundles.Add` příkaz s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="ffde1-298">Go back to `App_Start\BundleConfig.cs` and modify the last `bundles.Add` statement with the following highlighted code:</span></span>  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    <span data-ttu-id="ffde1-299">Tato nová metoda rozšíření používá stejné nápad skriptu ve formátu HTML ke kontrole DOM pro vložení odpovídající název třídy, název pravidla a pravidla hodnota definovaná v CSS sady a vrátí k původní webový server, pokud není nalezena shoda.</span><span class="sxs-lookup"><span data-stu-id="ffde1-299">This new extension method uses the same idea to inject script in the HTML to check the DOM for the a matching class name, rule name, and rule value defined in the CSS bundle, and falls back to the origin Web server if it fails to find the match.</span></span>
5. <span data-ttu-id="ffde1-300">Přístup na domovskou stránku a znovu publikovat cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ffde1-300">Publish the cloud service again and access the home page.</span></span>
6. <span data-ttu-id="ffde1-301">Zobrazte kód HTML pro stránku.</span><span class="sxs-lookup"><span data-stu-id="ffde1-301">View the HTML code for the page.</span></span> <span data-ttu-id="ffde1-302">Vložený skripty pro byste měli najít podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="ffde1-302">You should find injected scripts similar to the following:</span></span>    
   
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

    <span data-ttu-id="ffde1-303">Všimněte si, že vloženého skriptu pro sadu šablon stylů CSS stále obsahuje nesprávně pracujících zbývajících z `CdnFallbackExpression` vlastnost v řádku:</span><span class="sxs-lookup"><span data-stu-id="ffde1-303">Note that injected script for the CSS bundle still contains the errant remnant from the `CdnFallbackExpression` property in the line:</span></span>

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    <span data-ttu-id="ffde1-304">Ale od první část || výraz vždy vrátí hodnotu PRAVDA (na řádku přímo vyšší), bude funkce document.write() nebude nikdy spuštěn.</span><span class="sxs-lookup"><span data-stu-id="ffde1-304">But since the first part of the || expression will always return true (in the line directly above that), the document.write() function will never run.</span></span>

## <a name="more-information"></a><span data-ttu-id="ffde1-305">Další informace</span><span class="sxs-lookup"><span data-stu-id="ffde1-305">More Information</span></span>
* [<span data-ttu-id="ffde1-306">Přehled sítě pro doručování obsahu Azure (CDN)</span><span class="sxs-lookup"><span data-stu-id="ffde1-306">Overview of the Azure Content Delivery Network (CDN)</span></span>](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [<span data-ttu-id="ffde1-307">Používání Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ffde1-307">Using Azure CDN</span></span>](cdn-create-new-endpoint.md)
* [<span data-ttu-id="ffde1-308">ASP.NET sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="ffde1-308">ASP.NET Bundling and Minification</span></span>](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
