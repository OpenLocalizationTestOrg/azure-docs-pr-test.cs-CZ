---
title: "aaaConfigure služby API Management pomocí Git - Azure | Microsoft Docs"
description: "Zjistěte, jak toosave a nakonfigurujte konfigurace služby API Management pomocí Git."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ef7d4c18f2ea3f5c9b86403349a83aef240f979b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosave-and-configure-your-api-management-service-configuration-using-git"></a><span data-ttu-id="2ccdf-103">Jak toosave a nakonfigurujte konfigurace služby API Management pomocí Git</span><span class="sxs-lookup"><span data-stu-id="2ccdf-103">How toosave and configure your API Management service configuration using Git</span></span>
> 
> 

<span data-ttu-id="2ccdf-104">Každá instance služby API Management udržuje databázi konfigurace, která obsahuje informace o konfiguraci hello a metadata pro instance služby hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-104">Each API Management service instance maintains a configuration database that contains information about hello configuration and metadata for hello service instance.</span></span> <span data-ttu-id="2ccdf-105">Můžete provádět změny instance služby toohello Změna nastavení hello portálu vydavatele, pomocí rutiny prostředí PowerShell nebo volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-105">Changes can be made toohello service instance by changing a setting in hello publisher portal, using a PowerShell cmdlet, or making a REST API call.</span></span> <span data-ttu-id="2ccdf-106">Kromě toho toothese metod, můžete také spravovat konfigurace instance služby pomocí Git, například povolení scénářů správy služby:</span><span class="sxs-lookup"><span data-stu-id="2ccdf-106">In addition toothese methods, you can also manage your service instance configuration using Git, enabling service management scenarios such as:</span></span>

* <span data-ttu-id="2ccdf-107">Konfigurace správy verzí - stažení a uložení různých verzích konfigurace služby</span><span class="sxs-lookup"><span data-stu-id="2ccdf-107">Configuration versioning - download and store different versions of your service configuration</span></span>
* <span data-ttu-id="2ccdf-108">Hromadné změny konfigurace – proveďte změny toomultiple součástí konfigurace služby v místním úložišti a integrovat hello změny zpět toohello server s jedinou operací.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-108">Bulk configuration changes - make changes toomultiple parts of your service configuration in your local repository and integrate hello changes back toohello server with a single operation</span></span>
* <span data-ttu-id="2ccdf-109">Známých nástrojů Git a pracovní postup - používat hello Git nástrojů a pracovní postupy, které jste již obeznámeni s</span><span class="sxs-lookup"><span data-stu-id="2ccdf-109">Familiar Git toolchain and workflow - use hello Git tooling and workflows that you are already familiar with</span></span>

<span data-ttu-id="2ccdf-110">Hello následující diagram ukazuje přehled hello různé způsoby tooconfigure instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-110">hello following diagram shows an overview of hello different ways tooconfigure your API Management service instance.</span></span>

![Konfigurace Git][api-management-git-configure]

<span data-ttu-id="2ccdf-112">Pokud provedete změny tooyour služby pomocí portálu vydavatele hello, rutiny prostředí PowerShell nebo hello REST API, kterou spravujete konfigurační databáze služby service pomocí hello `https://{name}.management.azure-api.net` koncový bod, jak je znázorněno na pravé straně hello hello diagramu.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-112">When you make changes tooyour service using hello publisher portal, PowerShell cmdlets, or hello REST API, you are managing your service configuration database using hello `https://{name}.management.azure-api.net` endpoint, as shown on hello right side of hello diagram.</span></span> <span data-ttu-id="2ccdf-113">Hello levé straně hello diagram znázorňuje, jak můžete spravovat konfiguraci služby pomocí Git a úložiště Git pro vaši službu na `https://{name}.scm.azure-api.net`.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-113">hello left side of hello diagram illustrates how you can manage your service configuration using Git and Git repository for your service located at `https://{name}.scm.azure-api.net`.</span></span>

<span data-ttu-id="2ccdf-114">Hello následující kroky poskytují přehled správy pomocí Git instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-114">hello following steps provide an overview of managing your API Management service instance using Git.</span></span>

1. <span data-ttu-id="2ccdf-115">Konfigurace přístupu Git ve službě</span><span class="sxs-lookup"><span data-stu-id="2ccdf-115">Access Git configuration in your service</span></span>
2. <span data-ttu-id="2ccdf-116">Uložit úložiště Git tooyour databáze služby konfigurace</span><span class="sxs-lookup"><span data-stu-id="2ccdf-116">Save your service configuration database tooyour Git repository</span></span>
3. <span data-ttu-id="2ccdf-117">Klonování hello Git úložišti tooyour místního počítače</span><span class="sxs-lookup"><span data-stu-id="2ccdf-117">Clone hello Git repo tooyour local machine</span></span>
4. <span data-ttu-id="2ccdf-118">Nejnovější úložišti hello dolů tooyour místního počítače a potvrzení a posílejte nabízená oznámení změny zpět tooyour úložišti pro vyžádání obsahu</span><span class="sxs-lookup"><span data-stu-id="2ccdf-118">Pull hello latest repo down tooyour local machine, and commit and push changes back tooyour repo</span></span>
5. <span data-ttu-id="2ccdf-119">Nasadit hello změny z vašeho úložiště do konfigurační databáze služby service</span><span class="sxs-lookup"><span data-stu-id="2ccdf-119">Deploy hello changes from your repo into your service configuration database</span></span>

<span data-ttu-id="2ccdf-120">Tento článek popisuje, jak tooenable pomocí Git toomanage konfigurace služby a poskytuje odkaz pro hello soubory a složky v úložišti Git hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-120">This article describes how tooenable and use Git toomanage your service configuration and provides a reference for hello files and folders in hello Git repository.</span></span>

## <a name="access-git-configuration-in-your-service"></a><span data-ttu-id="2ccdf-121">Konfigurace přístupu Git ve službě</span><span class="sxs-lookup"><span data-stu-id="2ccdf-121">Access Git configuration in your service</span></span>
<span data-ttu-id="2ccdf-122">Můžete rychle zobrazit stav hello konfiguraci Git zobrazením hello Git ikonu v pravém horním rohu hello portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-122">You can quickly view hello status of your Git configuration by viewing hello Git icon in hello upper-right corner of hello publisher portal.</span></span> <span data-ttu-id="2ccdf-123">V tomto příkladu hello stavová zpráva znamená, že se úložiště toohello neuložené změny.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-123">In this example, hello status message indicates that there are unsaved changes toohello repository.</span></span> <span data-ttu-id="2ccdf-124">Je to proto, že konfigurační databázi služby API Management hello ještě nebyl uložen toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-124">This is because hello API Management service configuration database has not yet been saved toohello repository.</span></span>

![Stav Git][api-management-git-icon-enable]

<span data-ttu-id="2ccdf-126">tooview a nakonfigurovat nastavení konfigurace Git, můžete kliknutím na ikonu hello Git, nebo klikněte na tlačítko hello **zabezpečení** nabídky a přejděte toohello **konfigurace úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-126">tooview and configure your Git configuration settings, you can either click hello Git icon, or click hello **Security** menu and navigate toohello **Configuration repository** tab.</span></span>

![Povolit GIT][api-management-enable-git]

> [!IMPORTANT]
> <span data-ttu-id="2ccdf-128">Všechny tajné klíče, které nejsou definovány vlastnosti bude uložen v úložišti hello a zůstane v historii dokud zakázat a znovu povolte přístup Git.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-128">Any secrets that are not defined as properties will be stored in hello repository and will remain in its history until you disable and re-enable Git access.</span></span> <span data-ttu-id="2ccdf-129">Vlastnosti poskytují bezpečné místo toomanage konstantní řetězcové hodnoty, včetně tajných klíčů, ve všech konfigurací rozhraní API a zásady, takže není nutné toostore je přímo v příkazech jazyka zásad.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-129">Properties provide a secure place toomanage constant string values, including secrets, across all API configuration and policies, so you don't have toostore them directly in your policy statements.</span></span> <span data-ttu-id="2ccdf-130">Další informace najdete v tématu [jak toouse vlastnosti v Azure API Management zásady](api-management-howto-properties.md).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-130">For more information, see [How toouse properties in Azure API Management policies](api-management-howto-properties.md).</span></span>
> 
> 

<span data-ttu-id="2ccdf-131">Informace o povolení nebo zakázání Git přístup pomocí hello REST API najdete v tématu [povolit nebo zakázat Git přístup pomocí rozhraní REST API hello](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-131">For information on enabling or disabling Git access using hello REST API, see [Enable or disable Git access using hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span></span>

## <a name="toosave-hello-service-configuration-toohello-git-repository"></a><span data-ttu-id="2ccdf-132">toosave hello služby konfigurace toohello úložiště Git</span><span class="sxs-lookup"><span data-stu-id="2ccdf-132">toosave hello service configuration toohello Git repository</span></span>
<span data-ttu-id="2ccdf-133">prvním krokem Hello před klonováním hello úložiště je toosave hello aktuální stav hello služby konfigurace toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-133">hello first step before cloning hello repository is toosave hello current state of hello service configuration toohello repository.</span></span> <span data-ttu-id="2ccdf-134">Klikněte na tlačítko **uložit konfiguraci toorepository**.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-134">Click **Save configuration toorepository**.</span></span>

![Uložte konfiguraci][api-management-save-configuration]

<span data-ttu-id="2ccdf-136">Všechny požadované změny provádějte na potvrzovací obrazovce a hello a klikněte na tlačítko **Ok** toosave.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-136">Make any desired changes on hello confirmation screen and click **Ok** toosave.</span></span>

![Uložte konfiguraci][api-management-save-configuration-confirm]

<span data-ttu-id="2ccdf-138">Po chvíli hello konfiguraci uložit, a se zobrazí stav konfigurace hello hello úložiště, včetně hello datum a čas poslední změny konfigurace hello a hello poslední synchronizace mezi hello konfigurace služby a hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-138">After a few moments hello configuration is saved, and hello configuration status of hello repository is displayed, including hello date and time of hello last configuration change and hello last synchronization between hello service configuration and hello repository.</span></span>

![Stav konfigurace][api-management-configuration-status]

<span data-ttu-id="2ccdf-140">Po konfiguraci hello je uložit toohello úložiště, můžete klonovat.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-140">Once hello configuration is saved toohello repository, it can be cloned.</span></span>

<span data-ttu-id="2ccdf-141">Informace o provedení této operace pomocí hello REST API najdete v tématu [potvrzení konfigurace snímek pomocí hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-141">For information on performing this operation using hello REST API, see [Commit configuration snapshot using hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span></span>

## <a name="tooclone-hello-repository-tooyour-local-machine"></a><span data-ttu-id="2ccdf-142">tooclone hello úložiště tooyour místního počítače</span><span class="sxs-lookup"><span data-stu-id="2ccdf-142">tooclone hello repository tooyour local machine</span></span>
<span data-ttu-id="2ccdf-143">tooclone úložiště, musíte úložiště tooyour hello adresu URL, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-143">tooclone a repository, you need hello URL tooyour repository, a user name, and a password.</span></span> <span data-ttu-id="2ccdf-144">Hello uživatelské jméno a adresa URL se zobrazí v horní hello části hello **konfigurace úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-144">hello user name and URL are displayed near hello top of hello **Configuration repository** tab.</span></span>

![Klonu Git][api-management-configuration-git-clone]

<span data-ttu-id="2ccdf-146">heslo Hello je vygenerován v hello dolní části hello **konfigurace úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-146">hello password is generated at hello bottom of hello **Configuration repository** tab.</span></span>

![Generovat heslo][api-management-generate-password]

<span data-ttu-id="2ccdf-148">toogenerate heslo, nejdříve se ujistěte, že hello **vypršení platnosti** je nastavit toohello potřeby datum a čas vypršení a pak klikněte na tlačítko **vygenerovat Token**.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-148">toogenerate a password, first ensure that hello **Expiry** is set toohello desired expiration date and time, and then click **Generate Token**.</span></span>

![Heslo][api-management-password]

> [!IMPORTANT]
> <span data-ttu-id="2ccdf-150">Toto heslo si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-150">Make a note of this password.</span></span> <span data-ttu-id="2ccdf-151">Po opuštění této stránky hello heslo se znovu nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-151">Once you leave this page hello password will not be displayed again.</span></span>
> 
> 

<span data-ttu-id="2ccdf-152">Následující příklady použití hello Git Bash Hello nástroje z [Git pro Windows](http://www.git-scm.com/downloads) ale můžete použít jakýkoli Git nástroj, který jste se seznámili s.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-152">hello following examples use hello Git Bash tool from [Git for Windows](http://www.git-scm.com/downloads) but you can use any Git tool that you are familiar with.</span></span>

<span data-ttu-id="2ccdf-153">Otevřete svůj nástroj Git do požadované složky hello a spusťte následující příkaz tooclone hello git úložiště tooyour místního počítače, pomocí příkazu hello poskytované portál vydavatele hello hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-153">Open your Git tool in hello desired folder and run hello following command tooclone hello git repository tooyour local machine, using hello command provided by hello publisher portal.</span></span>

```
git clone https://bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="2ccdf-154">Zadejte hello uživatelské jméno a heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-154">Provide hello user name and password when prompted.</span></span>

<span data-ttu-id="2ccdf-155">Pokud se zobrazí všechny chyby, zkuste upravit vaše `git clone` příkaz tooinclude hello uživatelské jméno a heslo, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-155">If you receive any errors, try modifying your `git clone` command tooinclude hello user name and password, as shown in hello following example.</span></span>

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="2ccdf-156">Pokud to poskytuje k chybě, zkuste kódování hello heslo část příkazu hello adres URL.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-156">If this provides an error, try URL encoding hello password portion of hello command.</span></span> <span data-ttu-id="2ccdf-157">Jeden toodo rychlý způsob, jak je to tooopen Visual Studio, a problém hello následující příkaz v hello **hodnot proměnných**.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-157">One quick way toodo this is tooopen Visual Studio, and issue hello following command in hello **Immediate Window**.</span></span> <span data-ttu-id="2ccdf-158">tooopen hello **hodnot proměnných**, otevřete jakékoli řešení nebo produktu project v sadě Visual Studio (nebo vytvořte novou prázdnou konzolovou aplikaci) a zvolte **Windows**, **Immediate** z Hello **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-158">tooopen hello **Immediate Window**, open any solution or project in Visual Studio (or create a new empty console application), and choose **Windows**, **Immediate** from hello **Debug** menu.</span></span>

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

<span data-ttu-id="2ccdf-159">Použijte heslo hello kódovaný společně s uživatelské jméno a úložiště umístění tooconstruct hello git příkazu.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-159">Use hello encoded password along with your user name and repository location tooconstruct hello git command.</span></span>

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="2ccdf-160">Jakmile je klonovat úložiště hello můžete zobrazit a pracovat v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-160">Once hello repository is cloned you can view and work with it in your local file system.</span></span> <span data-ttu-id="2ccdf-161">Další informace najdete v tématu [souborů a složek struktury odkaz místní úložiště Git](#file-and-folder-structure-reference-of-local-git-repository).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-161">For more information, see [File and folder structure reference of local Git repository](#file-and-folder-structure-reference-of-local-git-repository).</span></span>

## <a name="tooupdate-your-local-repository-with-hello-most-current-service-instance-configuration"></a><span data-ttu-id="2ccdf-162">tooupdate místní úložiště s hello nejaktuálnější konfigurace instance služby</span><span class="sxs-lookup"><span data-stu-id="2ccdf-162">tooupdate your local repository with hello most current service instance configuration</span></span>
<span data-ttu-id="2ccdf-163">Pokud provedete instance služby API Management tooyour změny v portálu vydavatele hello nebo pomocí hello REST API, musíte uložit tyto změny toohello úložiště, než budete moct aktualizovat místní úložiště s nejnovější změny hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-163">If you make changes tooyour API Management service instance in hello publisher portal or using hello REST API, you must save these changes toohello repository before you can update your local repository with hello latest changes.</span></span> <span data-ttu-id="2ccdf-164">toodo tento, klikněte na tlačítko **uložit konfiguraci toorepository** na hello **konfigurace úložiště** v hello portálu vydavatele a potom vydat hello následující příkaz v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-164">toodo this, click **Save configuration toorepository** on hello **Configuration repository** tab in hello publisher portal, and then issue hello following command in your local repository.</span></span>

```
git pull
```

<span data-ttu-id="2ccdf-165">Dřív, než spustíte `git pull` ujistit, že jste hello složku pro místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-165">Before running `git pull` ensure that you are in hello folder for your local repository.</span></span> <span data-ttu-id="2ccdf-166">Pokud jste právě dokončili hello `git clone` pak hello directory tooyour úložišti musíte změnit spuštěním příkazu jako hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-166">If you have just completed hello `git clone` command, then you must change hello directory tooyour repo by running a command like hello following.</span></span>

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="toopush-changes-from-your-local-repo-toohello-server-repo"></a><span data-ttu-id="2ccdf-167">toopush změn z vaší místní úložiště toohello serveru úložiště</span><span class="sxs-lookup"><span data-stu-id="2ccdf-167">toopush changes from your local repo toohello server repo</span></span>
<span data-ttu-id="2ccdf-168">toopush změní z úložiště Místní úložiště toohello serveru, je nutné potvrdit změny a vložit je toohello serveru úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-168">toopush changes from your local repository toohello server repository, you must commit your changes and then push them toohello server repository.</span></span> <span data-ttu-id="2ccdf-169">toocommit změny, otevřete Git příkaz nástroj, přepínač toohello adresáře vaší místní úložiště a problém hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-169">toocommit your changes, open your Git command tool, switch toohello directory of your local repository, and issue hello following commands.</span></span>

```
git add --all
git commit -m "Description of your changes"
```

<span data-ttu-id="2ccdf-170">toopush všechny hello potvrdí toohello server, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-170">toopush all of hello commits toohello server, run hello following command.</span></span>

```
git push
```

## <a name="toodeploy-any-service-configuration-changes-toohello-api-management-service-instance"></a><span data-ttu-id="2ccdf-171">toodeploy všechny služby konfigurace změny toohello instance služby API Management</span><span class="sxs-lookup"><span data-stu-id="2ccdf-171">toodeploy any service configuration changes toohello API Management service instance</span></span>
<span data-ttu-id="2ccdf-172">Jakmile jsou tyto místní změny potvrzeny a stisknutí toohello server úložiště, můžete je nasadit tooyour instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-172">Once your local changes are committed and pushed toohello server repository, you can deploy them tooyour API Management service instance.</span></span>

![Nasazení][api-management-configuration-deploy]

<span data-ttu-id="2ccdf-174">Informace o provedení této operace pomocí hello REST API najdete v tématu [nasazení Git změny tooconfiguration databázi pomocí hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-174">For information on performing this operation using hello REST API, see [Deploy Git changes tooconfiguration database using hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span></span>

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a><span data-ttu-id="2ccdf-175">Odkaz struktury souborů a složek místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="2ccdf-175">File and folder structure reference of local Git repository</span></span>
<span data-ttu-id="2ccdf-176">Hello soubory a složky v úložišti místní git hello obsahovat informace o konfiguraci hello o hello instance služby.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-176">hello files and folders in hello local git repository contain hello configuration information about hello service instance.</span></span>

| <span data-ttu-id="2ccdf-177">Položka</span><span class="sxs-lookup"><span data-stu-id="2ccdf-177">Item</span></span> | <span data-ttu-id="2ccdf-178">Popis</span><span class="sxs-lookup"><span data-stu-id="2ccdf-178">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2ccdf-179">Kořenová složka api management</span><span class="sxs-lookup"><span data-stu-id="2ccdf-179">root api-management folder</span></span> |<span data-ttu-id="2ccdf-180">Obsahuje nejvyšší úrovně konfiguraci pro instance služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-180">Contains top-level configuration for hello service instance</span></span> |
| <span data-ttu-id="2ccdf-181">rozhraní API složky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-181">apis folder</span></span> |<span data-ttu-id="2ccdf-182">Obsahuje hello konfiguraci pro rozhraní API hello v instanci služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-182">Contains hello configuration for hello apis in hello service instance</span></span> |
| <span data-ttu-id="2ccdf-183">složka skupiny</span><span class="sxs-lookup"><span data-stu-id="2ccdf-183">groups folder</span></span> |<span data-ttu-id="2ccdf-184">Obsahuje konfiguraci hello hello skupin v instanci služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-184">Contains hello configuration for hello groups in hello service instance</span></span> |
| <span data-ttu-id="2ccdf-185">Složka zásad</span><span class="sxs-lookup"><span data-stu-id="2ccdf-185">policies folder</span></span> |<span data-ttu-id="2ccdf-186">Obsahuje hello zásady v instanci služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-186">Contains hello policies in hello service instance</span></span> |
| <span data-ttu-id="2ccdf-187">portalStyles složky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-187">portalStyles folder</span></span> |<span data-ttu-id="2ccdf-188">Obsahuje konfiguraci hello přizpůsobení portálu hello vývojáře v instanci služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-188">Contains hello configuration for hello developer portal customizations in hello service instance</span></span> |
| <span data-ttu-id="2ccdf-189">produkty složky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-189">products folder</span></span> |<span data-ttu-id="2ccdf-190">Obsahuje konfiguraci hello produktů hello v instanci služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-190">Contains hello configuration for hello products in hello service instance</span></span> |
| <span data-ttu-id="2ccdf-191">složka šablon</span><span class="sxs-lookup"><span data-stu-id="2ccdf-191">templates folder</span></span> |<span data-ttu-id="2ccdf-192">Obsahuje konfiguraci hello hello e-mailových šablon v instanci služby hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-192">Contains hello configuration for hello email templates in hello service instance</span></span> |

<span data-ttu-id="2ccdf-193">Každé složky může obsahovat jeden nebo více souborů a v některých případech jeden nebo více složek, například do složky pro každé rozhraní API, produkty nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-193">Each folder can contain one or more files, and in some cases one or more folders, for example a folder for each API, product, or group.</span></span> <span data-ttu-id="2ccdf-194">Hello souborů v rámci každé složky jsou specifické pro typ entity hello popsaného hello název složky.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-194">hello files within each folder are specific for hello entity type described by hello folder name.</span></span>

| <span data-ttu-id="2ccdf-195">Typ souboru</span><span class="sxs-lookup"><span data-stu-id="2ccdf-195">File type</span></span> | <span data-ttu-id="2ccdf-196">Účel</span><span class="sxs-lookup"><span data-stu-id="2ccdf-196">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="2ccdf-197">JSON</span><span class="sxs-lookup"><span data-stu-id="2ccdf-197">json</span></span> |<span data-ttu-id="2ccdf-198">Informace o konfiguraci o hello odpovídající entity</span><span class="sxs-lookup"><span data-stu-id="2ccdf-198">Configuration information about hello respective entity</span></span> |
| <span data-ttu-id="2ccdf-199">HTML</span><span class="sxs-lookup"><span data-stu-id="2ccdf-199">html</span></span> |<span data-ttu-id="2ccdf-200">Popisy o hello entity, často zobrazeny v portálu pro vývojáře hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-200">Descriptions about hello entity, often displayed in hello developer portal</span></span> |
| <span data-ttu-id="2ccdf-201">xml</span><span class="sxs-lookup"><span data-stu-id="2ccdf-201">xml</span></span> |<span data-ttu-id="2ccdf-202">Příkazy zásad</span><span class="sxs-lookup"><span data-stu-id="2ccdf-202">Policy statements</span></span> |
| <span data-ttu-id="2ccdf-203">šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="2ccdf-203">css</span></span> |<span data-ttu-id="2ccdf-204">Šablony stylů pro přizpůsobení portálu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="2ccdf-204">Style sheets for developer portal customization</span></span> |

<span data-ttu-id="2ccdf-205">Tyto soubory lze vytvořit, odstranit, upravit a spravovat v místním systému souborů a změny hello nasazené back toohello instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-205">These files can be created, deleted, edited, and managed on your local file system, and hello changes deployed back toohello your API Management service instance.</span></span>

> [!NOTE]
> <span data-ttu-id="2ccdf-206">Hello následující entity nejsou obsaženy v úložišti Git hello a nelze konfigurovat pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-206">hello following entities are not contained in hello Git repository and cannot be configured using Git.</span></span>
> 
> * <span data-ttu-id="2ccdf-207">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="2ccdf-207">Users</span></span>
> * <span data-ttu-id="2ccdf-208">Předplatná</span><span class="sxs-lookup"><span data-stu-id="2ccdf-208">Subscriptions</span></span>
> * <span data-ttu-id="2ccdf-209">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2ccdf-209">Properties</span></span>
> * <span data-ttu-id="2ccdf-210">Vývojáře portálu entity než styly</span><span class="sxs-lookup"><span data-stu-id="2ccdf-210">Developer portal entities other than styles</span></span>
> 
> 

### <a name="root-api-management-folder"></a><span data-ttu-id="2ccdf-211">Kořenová složka api management</span><span class="sxs-lookup"><span data-stu-id="2ccdf-211">Root api-management folder</span></span>
<span data-ttu-id="2ccdf-212">kořenové Hello `api-management` složka obsahuje `configuration.json` soubor, který obsahuje nejvyšší úrovně informace o instanci služby hello v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-212">hello root `api-management` folder contains a `configuration.json` file that contains top-level information about hello service instance in hello following format.</span></span>

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

<span data-ttu-id="2ccdf-213">první čtyři nastavení Hello (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, a `UserRegistrationTermsConsentRequired`) mapování toohello následující nastavení na hello **identity** ve hello **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-213">hello first four settings (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, and `UserRegistrationTermsConsentRequired`) map toohello following settings on hello **Identities** tab in hello **Security** section.</span></span>

| <span data-ttu-id="2ccdf-214">Nastavení identity</span><span class="sxs-lookup"><span data-stu-id="2ccdf-214">Identity setting</span></span> | <span data-ttu-id="2ccdf-215">Mapuje příliš</span><span class="sxs-lookup"><span data-stu-id="2ccdf-215">Maps too</span></span>|
| --- | --- |
| <span data-ttu-id="2ccdf-216">RegistrationEnabled</span><span class="sxs-lookup"><span data-stu-id="2ccdf-216">RegistrationEnabled</span></span> |<span data-ttu-id="2ccdf-217">**Přesměrování toosign stránku anonymní uživatelé** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="2ccdf-217">**Redirect anonymous users toosign-in page** checkbox</span></span> |
| <span data-ttu-id="2ccdf-218">UserRegistrationTerms</span><span class="sxs-lookup"><span data-stu-id="2ccdf-218">UserRegistrationTerms</span></span> |<span data-ttu-id="2ccdf-219">**Podmínky použití na registraci uživatele** textbox</span><span class="sxs-lookup"><span data-stu-id="2ccdf-219">**Terms of use on user signup** textbox</span></span> |
| <span data-ttu-id="2ccdf-220">UserRegistrationTermsEnabled</span><span class="sxs-lookup"><span data-stu-id="2ccdf-220">UserRegistrationTermsEnabled</span></span> |<span data-ttu-id="2ccdf-221">**Zobrazit podmínky použití na přihlašovací stránce** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="2ccdf-221">**Show terms of use on signup page** checkbox</span></span> |
| <span data-ttu-id="2ccdf-222">UserRegistrationTermsConsentRequired</span><span class="sxs-lookup"><span data-stu-id="2ccdf-222">UserRegistrationTermsConsentRequired</span></span> |<span data-ttu-id="2ccdf-223">**Vyžadovat souhlas** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="2ccdf-223">**Require consent** checkbox</span></span> |

![Nastavení identity][api-management-identity-settings]

<span data-ttu-id="2ccdf-225">Hello další čtyři nastavení (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, a `DelegationValidationKey`) mapování toohello následující nastavení na hello **delegování** ve hello **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-225">hello next four settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, and `DelegationValidationKey`) map toohello following settings on hello **Delegation** tab in hello **Security** section.</span></span>

| <span data-ttu-id="2ccdf-226">Nastavení delegování</span><span class="sxs-lookup"><span data-stu-id="2ccdf-226">Delegation setting</span></span> | <span data-ttu-id="2ccdf-227">Mapuje příliš</span><span class="sxs-lookup"><span data-stu-id="2ccdf-227">Maps too</span></span>|
| --- | --- |
| <span data-ttu-id="2ccdf-228">DelegationEnabled</span><span class="sxs-lookup"><span data-stu-id="2ccdf-228">DelegationEnabled</span></span> |<span data-ttu-id="2ccdf-229">**Delegát přihlášení a registrace** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="2ccdf-229">**Delegate sign-in & sign-up** checkbox</span></span> |
| <span data-ttu-id="2ccdf-230">DelegationUrl</span><span class="sxs-lookup"><span data-stu-id="2ccdf-230">DelegationUrl</span></span> |<span data-ttu-id="2ccdf-231">**Adresa URL koncového bodu delegování** textbox</span><span class="sxs-lookup"><span data-stu-id="2ccdf-231">**Delegation endpoint URL** textbox</span></span> |
| <span data-ttu-id="2ccdf-232">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="2ccdf-232">DelegatedSubscriptionEnabled</span></span> |<span data-ttu-id="2ccdf-233">**Delegovat odběru produktů** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="2ccdf-233">**Delegate product subscription** checkbox</span></span> |
| <span data-ttu-id="2ccdf-234">DelegationValidationKey</span><span class="sxs-lookup"><span data-stu-id="2ccdf-234">DelegationValidationKey</span></span> |<span data-ttu-id="2ccdf-235">**Delegovat ověřovací klíč** textbox</span><span class="sxs-lookup"><span data-stu-id="2ccdf-235">**Delegate Validation Key** textbox</span></span> |

![Nastavení delegování][api-management-delegation-settings]

<span data-ttu-id="2ccdf-237">Hello poslední nastavení `$ref-policy`, mapuje toohello globální zásady příkazy soubor pro instance služby hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-237">hello final setting, `$ref-policy`, maps toohello global policy statements file for hello service instance.</span></span>

### <a name="apis-folder"></a><span data-ttu-id="2ccdf-238">rozhraní API složky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-238">apis folder</span></span>
<span data-ttu-id="2ccdf-239">Hello `apis` složka obsahuje složku pro každé rozhraní API v instanci služby hello, který obsahuje hello následující položky.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-239">hello `apis` folder contains a folder for each API in hello service instance which contains hello following items.</span></span>

* <span data-ttu-id="2ccdf-240">`apis\<api name>\configuration.json`-Toto je konfigurace hello hello rozhraní API a obsahuje informace o hello operace a adresa URL služby back-end hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-240">`apis\<api name>\configuration.json` - this is hello configuration for hello API and contains information about hello backend service URL and hello operations.</span></span> <span data-ttu-id="2ccdf-241">Toto je hello stejné informace, které by byla vrácena, pokud byste byli toocall [získat konkrétní rozhraní API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) s `export=true` v `application/json` formátu.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-241">This is hello same information that would be returned if you were toocall [Get a specific API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) with `export=true` in `application/json` format.</span></span>
* <span data-ttu-id="2ccdf-242">`apis\<api name>\api.description.html`-Toto je popis hello hello rozhraní API a odpovídá toohello `description` vlastnost hello [rozhraní API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-242">`apis\<api name>\api.description.html` - this is hello description of hello API and corresponds toohello `description` property of hello [API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span></span>
* <span data-ttu-id="2ccdf-243">`apis\<api name>\operations\`– Tato složka obsahuje `<operation name>.description.html` soubory, které mapují toohello operace v hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-243">`apis\<api name>\operations\` - this folder contains `<operation name>.description.html` files that map toohello operations in hello API.</span></span> <span data-ttu-id="2ccdf-244">Každý soubor obsahuje popis hello jedné operace v hello rozhraní API, která se mapuje toohello `description` vlastnost hello [operaci entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) v hello REST API.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-244">Each file contains hello description of a single operation in hello API which maps toohello `description` property of hello [operation entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in hello REST API.</span></span>

### <a name="groups-folder"></a><span data-ttu-id="2ccdf-245">složka skupiny</span><span class="sxs-lookup"><span data-stu-id="2ccdf-245">groups folder</span></span>
<span data-ttu-id="2ccdf-246">Hello `groups` složka obsahuje složku pro každou skupinu definované v instanci služby hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-246">hello `groups` folder contains a folder for each group defined in hello service instance.</span></span>

* <span data-ttu-id="2ccdf-247">`groups\<group name>\configuration.json`-Toto je hello konfigurace pro skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-247">`groups\<group name>\configuration.json` - this is hello configuration for hello group.</span></span> <span data-ttu-id="2ccdf-248">Toto je hello stejné informace, které by byla vrácena, pokud byste byli toocall hello [získat určité skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operaci.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-248">This is hello same information that would be returned if you were toocall hello [Get a specific group](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operation.</span></span>
* <span data-ttu-id="2ccdf-249">`groups\<group name>\description.html`-Toto je popis hello hello skupiny a odpovídá toohello `description` vlastnost hello [skupiny entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-249">`groups\<group name>\description.html` - this is hello description of hello group and corresponds toohello `description` property of hello [group entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span></span>

### <a name="policies-folder"></a><span data-ttu-id="2ccdf-250">Složka zásad</span><span class="sxs-lookup"><span data-stu-id="2ccdf-250">policies folder</span></span>
<span data-ttu-id="2ccdf-251">Hello `policies` složka obsahuje příkazy hello zásad pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-251">hello `policies` folder contains hello policy statements for your service instance.</span></span>

* <span data-ttu-id="2ccdf-252">`policies\global.xml`-obsahuje zásady definované v globálním oboru pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-252">`policies\global.xml` - contains policies defined at global scope for your service instance.</span></span>
* <span data-ttu-id="2ccdf-253">`policies\apis\<api name>\`– Pokud máte jakékoli zásady definované v oboru rozhraní API, jsou obsažené v této složce.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-253">`policies\apis\<api name>\` - if you have any policies defined at API scope, they are contained in this folder.</span></span>
* <span data-ttu-id="2ccdf-254">`policies\apis\<api name>\<operation name>\`Složka – Pokud máte jakékoli zásady definované v oboru operaci, jsou obsažené v této složce v `<operation name>.xml` soubory, které mapují toohello příkazy zásad pro každou operaci.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-254">`policies\apis\<api name>\<operation name>\` folder - if you have any policies defined at operation scope, they are contained in this folder in `<operation name>.xml` files that map toohello policy statements for each operation.</span></span>
* <span data-ttu-id="2ccdf-255">`policies\products\`– Pokud máte jakékoli zásady definované v produktu oboru, jsou obsažené v této složce, která obsahuje `<product name>.xml` soubory, které mapují toohello příkazy zásad pro jednotlivé produkty.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-255">`policies\products\` - if you have any policies defined at product scope, they are contained in this folder, which contains `<product name>.xml` files that map toohello policy statements for each product.</span></span>

### <a name="portalstyles-folder"></a><span data-ttu-id="2ccdf-256">portalStyles složky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-256">portalStyles folder</span></span>
<span data-ttu-id="2ccdf-257">Hello `portalStyles` složka obsahuje konfiguraci a stylu stylů pro vývojáře přizpůsobení portálu pro instance služby hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-257">hello `portalStyles` folder contains configuration and style sheets for developer portal customizations for hello service instance.</span></span>

* <span data-ttu-id="2ccdf-258">`portalStyles\configuration.json`-obsahuje názvy hello používá portál pro vývojáře hello hello šablony stylů</span><span class="sxs-lookup"><span data-stu-id="2ccdf-258">`portalStyles\configuration.json` - contains hello names of hello style sheets used by hello developer portal</span></span>
* <span data-ttu-id="2ccdf-259">`portalStyles\<style name>.css`-Každý `<style name>.css` soubor obsahuje styly pro portál pro vývojáře hello (`Preview.css` a `Production.css` ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="2ccdf-259">`portalStyles\<style name>.css` - each `<style name>.css` file contains styles for hello developer portal (`Preview.css` and `Production.css` by default).</span></span>

### <a name="products-folder"></a><span data-ttu-id="2ccdf-260">produkty složky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-260">products folder</span></span>
<span data-ttu-id="2ccdf-261">Hello `products` složka obsahuje složku pro každý produkt definované v instanci služby hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-261">hello `products` folder contains a folder for each product defined in hello service instance.</span></span>

* <span data-ttu-id="2ccdf-262">`products\<product name>\configuration.json`-Toto je konfigurace hello hello produktu.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-262">`products\<product name>\configuration.json` - this is hello configuration for hello product.</span></span> <span data-ttu-id="2ccdf-263">Toto je hello stejné informace, které by byla vrácena, pokud byste byli toocall hello [získat určitý produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operaci.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-263">This is hello same information that would be returned if you were toocall hello [Get a specific product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operation.</span></span>
* <span data-ttu-id="2ccdf-264">`products\<product name>\product.description.html`-Toto je popis hello hello produktu a odpovídá toohello `description` vlastnost hello [entity produktu](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) v hello REST API.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-264">`products\<product name>\product.description.html` - this is hello description of hello product and corresponds toohello `description` property of hello [product entity](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in hello REST API.</span></span>

### <a name="templates"></a><span data-ttu-id="2ccdf-265">šablony</span><span class="sxs-lookup"><span data-stu-id="2ccdf-265">templates</span></span>
<span data-ttu-id="2ccdf-266">Hello `templates` složka obsahuje konfiguraci pro hello [e-mailových šablon](api-management-howto-configure-notifications.md) hello instance služby.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-266">hello `templates` folder contains configuration for hello [email templates](api-management-howto-configure-notifications.md) of hello service instance.</span></span>

* <span data-ttu-id="2ccdf-267">`<template name>\configuration.json`-Toto je konfigurace hello hello e-mailové šablony.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-267">`<template name>\configuration.json` - this is hello configuration for hello email template.</span></span>
* <span data-ttu-id="2ccdf-268">`<template name>\body.html`-Toto je hello text šablony e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="2ccdf-268">`<template name>\body.html` - this is hello body of hello email template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ccdf-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ccdf-269">Next steps</span></span>
<span data-ttu-id="2ccdf-270">Informace o dalších způsobů toomanage instanci služby, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="2ccdf-270">For information on other ways toomanage your service instance, see:</span></span>

* <span data-ttu-id="2ccdf-271">Spravovat instanci služby pomocí následující rutiny prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-271">Manage your service instance using hello following PowerShell cmdlets</span></span>
  * [<span data-ttu-id="2ccdf-272">Referenční informace k rutinám PowerShellu pro nasazení služeb</span><span class="sxs-lookup"><span data-stu-id="2ccdf-272">Service deployment PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [<span data-ttu-id="2ccdf-273">Správa služeb referenční informace o rutinách prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ccdf-273">Service management PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* <span data-ttu-id="2ccdf-274">Spravovat instanci služby v portálu vydavatele hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-274">Manage your service instance in hello publisher portal</span></span>
  * [<span data-ttu-id="2ccdf-275">Správa vašeho prvního rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2ccdf-275">Manage your first API</span></span>](api-management-get-started.md)
* <span data-ttu-id="2ccdf-276">Spravovat instanci služby pomocí rozhraní REST API hello</span><span class="sxs-lookup"><span data-stu-id="2ccdf-276">Manage your service instance using hello REST API</span></span>
  * [<span data-ttu-id="2ccdf-277">Referenční dokumentace rozhraní API REST API Management</span><span class="sxs-lookup"><span data-stu-id="2ccdf-277">API Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="2ccdf-278">Podívejte se na video s přehledem</span><span class="sxs-lookup"><span data-stu-id="2ccdf-278">Watch a video overview</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




