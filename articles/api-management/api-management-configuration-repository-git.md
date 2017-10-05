---
title: "Konfigurace služby API Management pomocí Git - Azure | Microsoft Docs"
description: "Zjistěte, jak uložit a nakonfigurovat konfigurace služby API Management pomocí Git."
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
ms.openlocfilehash: f5d6bb7ccbf15424e9940ccda2fac668a2af5a57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a><span data-ttu-id="b91e0-103">Uložte a konfiguraci konfigurace služby API Management pomocí Git</span><span class="sxs-lookup"><span data-stu-id="b91e0-103">How to save and configure your API Management service configuration using Git</span></span>
> 
> 

<span data-ttu-id="b91e0-104">Každá instance služby API Management udržuje databázi konfigurace, která obsahuje informace o konfiguraci a metadat pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-104">Each API Management service instance maintains a configuration database that contains information about the configuration and metadata for the service instance.</span></span> <span data-ttu-id="b91e0-105">Změna nastavení na portálu vydavatele, pomocí rutiny prostředí PowerShell nebo volání rozhraní REST API, můžete provedeny změny v instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-105">Changes can be made to the service instance by changing a setting in the publisher portal, using a PowerShell cmdlet, or making a REST API call.</span></span> <span data-ttu-id="b91e0-106">Kromě těchto metod můžete také spravovat konfigurace instance služby pomocí Git, například povolení scénářů správy služby:</span><span class="sxs-lookup"><span data-stu-id="b91e0-106">In addition to these methods, you can also manage your service instance configuration using Git, enabling service management scenarios such as:</span></span>

* <span data-ttu-id="b91e0-107">Konfigurace správy verzí - stažení a uložení různých verzích konfigurace služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-107">Configuration versioning - download and store different versions of your service configuration</span></span>
* <span data-ttu-id="b91e0-108">Hromadné změny konfigurace – provádět změny více částí konfigurace služby v místním úložišti a integrovat změny zpět na server s jedinou operací.</span><span class="sxs-lookup"><span data-stu-id="b91e0-108">Bulk configuration changes - make changes to multiple parts of your service configuration in your local repository and integrate the changes back to the server with a single operation</span></span>
* <span data-ttu-id="b91e0-109">Známých nástrojů Git a pracovní postup - používat Git nástrojů a pracovní postupy, které jste již obeznámeni s</span><span class="sxs-lookup"><span data-stu-id="b91e0-109">Familiar Git toolchain and workflow - use the Git tooling and workflows that you are already familiar with</span></span>

<span data-ttu-id="b91e0-110">Následující diagram ukazuje přehled různé způsoby, jak nakonfigurovat instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b91e0-110">The following diagram shows an overview of the different ways to configure your API Management service instance.</span></span>

![Konfigurace Git][api-management-git-configure]

<span data-ttu-id="b91e0-112">Když provedete změny služby pomocí portálu vydavatele, rutiny prostředí PowerShell nebo rozhraní REST API, kterou spravujete vaší služby konfigurace databáze pomocí `https://{name}.management.azure-api.net` koncový bod, jak je znázorněno na pravé straně diagramu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-112">When you make changes to your service using the publisher portal, PowerShell cmdlets, or the REST API, you are managing your service configuration database using the `https://{name}.management.azure-api.net` endpoint, as shown on the right side of the diagram.</span></span> <span data-ttu-id="b91e0-113">Levé straně diagram znázorňuje, jak můžete spravovat konfiguraci služby pomocí Git a úložiště Git pro vaši službu na `https://{name}.scm.azure-api.net`.</span><span class="sxs-lookup"><span data-stu-id="b91e0-113">The left side of the diagram illustrates how you can manage your service configuration using Git and Git repository for your service located at `https://{name}.scm.azure-api.net`.</span></span>

<span data-ttu-id="b91e0-114">Následující kroky poskytují přehled správy pomocí Git instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b91e0-114">The following steps provide an overview of managing your API Management service instance using Git.</span></span>

1. <span data-ttu-id="b91e0-115">Konfigurace přístupu Git ve službě</span><span class="sxs-lookup"><span data-stu-id="b91e0-115">Access Git configuration in your service</span></span>
2. <span data-ttu-id="b91e0-116">Uložit konfigurační databáze služby služby do úložiště Git</span><span class="sxs-lookup"><span data-stu-id="b91e0-116">Save your service configuration database to your Git repository</span></span>
3. <span data-ttu-id="b91e0-117">Naklonujte úložiště Git do místního počítače</span><span class="sxs-lookup"><span data-stu-id="b91e0-117">Clone the Git repo to your local machine</span></span>
4. <span data-ttu-id="b91e0-118">Stáhněte nejnovější úložišti na místním počítači a potvrzení a posílejte nabízená oznámení změn zpět do vašeho úložiště</span><span class="sxs-lookup"><span data-stu-id="b91e0-118">Pull the latest repo down to your local machine, and commit and push changes back to your repo</span></span>
5. <span data-ttu-id="b91e0-119">Nasazení změny z vašeho úložiště do konfigurační databáze služby service</span><span class="sxs-lookup"><span data-stu-id="b91e0-119">Deploy the changes from your repo into your service configuration database</span></span>

<span data-ttu-id="b91e0-120">Tento článek popisuje, jak povolit a používat Git ke správě konfigurace služby a poskytuje odkaz pro soubory a složky do úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="b91e0-120">This article describes how to enable and use Git to manage your service configuration and provides a reference for the files and folders in the Git repository.</span></span>

## <a name="access-git-configuration-in-your-service"></a><span data-ttu-id="b91e0-121">Konfigurace přístupu Git ve službě</span><span class="sxs-lookup"><span data-stu-id="b91e0-121">Access Git configuration in your service</span></span>
<span data-ttu-id="b91e0-122">Můžete rychle zobrazit stav konfigurace Git zobrazením ikonu Git v pravém horním rohu na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b91e0-122">You can quickly view the status of your Git configuration by viewing the Git icon in the upper-right corner of the publisher portal.</span></span> <span data-ttu-id="b91e0-123">V tomto příkladu označuje stavovou zprávu, která jsou neuložené změny do úložiště.</span><span class="sxs-lookup"><span data-stu-id="b91e0-123">In this example, the status message indicates that there are unsaved changes to the repository.</span></span> <span data-ttu-id="b91e0-124">Je to proto, že konfigurační databázi služby API Management ještě nebyl uložen do úložiště.</span><span class="sxs-lookup"><span data-stu-id="b91e0-124">This is because the API Management service configuration database has not yet been saved to the repository.</span></span>

![Stav Git][api-management-git-icon-enable]

<span data-ttu-id="b91e0-126">Zobrazit a konfigurovat nastavení konfigurace Git, můžete buď klikněte na ikonu Git, nebo klikněte na tlačítko **zabezpečení** nabídky a přejděte do **konfigurace úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="b91e0-126">To view and configure your Git configuration settings, you can either click the Git icon, or click the **Security** menu and navigate to the **Configuration repository** tab.</span></span>

![Povolit GIT][api-management-enable-git]

> [!IMPORTANT]
> <span data-ttu-id="b91e0-128">Všechny tajné klíče, které nejsou definovány vlastnosti bude uložen v úložišti a zůstane v historii dokud zakázat a znovu povolte přístup Git.</span><span class="sxs-lookup"><span data-stu-id="b91e0-128">Any secrets that are not defined as properties will be stored in the repository and will remain in its history until you disable and re-enable Git access.</span></span> <span data-ttu-id="b91e0-129">Vlastnosti poskytují na bezpečné místo pro správu konstantní řetězcové hodnoty, včetně tajných klíčů, ve všech konfigurací rozhraní API a zásady, takže není nutné ukládat je přímo do vaší příkazy zásad.</span><span class="sxs-lookup"><span data-stu-id="b91e0-129">Properties provide a secure place to manage constant string values, including secrets, across all API configuration and policies, so you don't have to store them directly in your policy statements.</span></span> <span data-ttu-id="b91e0-130">Další informace najdete v tématu [použití vlastností v zásadách Azure API Management](api-management-howto-properties.md).</span><span class="sxs-lookup"><span data-stu-id="b91e0-130">For more information, see [How to use properties in Azure API Management policies](api-management-howto-properties.md).</span></span>
> 
> 

<span data-ttu-id="b91e0-131">Informace o povolení nebo zakázání Git přístup pomocí rozhraní REST API najdete v tématu [povolit nebo zakázat Git přístup pomocí rozhraní REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span><span class="sxs-lookup"><span data-stu-id="b91e0-131">For information on enabling or disabling Git access using the REST API, see [Enable or disable Git access using the REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span></span>

## <a name="to-save-the-service-configuration-to-the-git-repository"></a><span data-ttu-id="b91e0-132">Chcete-li uložit konfiguraci služby do úložiště Git</span><span class="sxs-lookup"><span data-stu-id="b91e0-132">To save the service configuration to the Git repository</span></span>
<span data-ttu-id="b91e0-133">Prvním krokem před klonováním úložiště je uložit aktuální stav konfigurace služby do úložiště.</span><span class="sxs-lookup"><span data-stu-id="b91e0-133">The first step before cloning the repository is to save the current state of the service configuration to the repository.</span></span> <span data-ttu-id="b91e0-134">Klikněte na tlačítko **uložit konfiguraci úložiště**.</span><span class="sxs-lookup"><span data-stu-id="b91e0-134">Click **Save configuration to repository**.</span></span>

![Uložte konfiguraci][api-management-save-configuration]

<span data-ttu-id="b91e0-136">Všechny požadované změny provádějte na potvrzovací obrazovce a klepněte na tlačítko **Ok** uložit.</span><span class="sxs-lookup"><span data-stu-id="b91e0-136">Make any desired changes on the confirmation screen and click **Ok** to save.</span></span>

![Uložte konfiguraci][api-management-save-configuration-confirm]

<span data-ttu-id="b91e0-138">Po chvíli konfiguraci uložit, a stav konfigurace úložiště, ve kterém se zobrazí, včetně datum a čas poslední změny konfigurace a poslední synchronizace mezi konfiguraci služby a úložiště.</span><span class="sxs-lookup"><span data-stu-id="b91e0-138">After a few moments the configuration is saved, and the configuration status of the repository is displayed, including the date and time of the last configuration change and the last synchronization between the service configuration and the repository.</span></span>

![Stav konfigurace][api-management-configuration-status]

<span data-ttu-id="b91e0-140">Po konfiguraci je uložen do úložiště, můžete klonovat.</span><span class="sxs-lookup"><span data-stu-id="b91e0-140">Once the configuration is saved to the repository, it can be cloned.</span></span>

<span data-ttu-id="b91e0-141">Informace o provedení této operace pomocí rozhraní REST API najdete v tématu [potvrzení konfigurace snímek pomocí rozhraní REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span><span class="sxs-lookup"><span data-stu-id="b91e0-141">For information on performing this operation using the REST API, see [Commit configuration snapshot using the REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span></span>

## <a name="to-clone-the-repository-to-your-local-machine"></a><span data-ttu-id="b91e0-142">Klonování úložiště do místního počítače</span><span class="sxs-lookup"><span data-stu-id="b91e0-142">To clone the repository to your local machine</span></span>
<span data-ttu-id="b91e0-143">Klonovat úložiště, musíte adresu URL úložiště, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="b91e0-143">To clone a repository, you need the URL to your repository, a user name, and a password.</span></span> <span data-ttu-id="b91e0-144">Uživatelské jméno a adresa URL se zobrazí v horní části **konfigurace úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="b91e0-144">The user name and URL are displayed near the top of the **Configuration repository** tab.</span></span>

![Klonu Git][api-management-configuration-git-clone]

<span data-ttu-id="b91e0-146">Heslo je vygenerován v dolní části **konfigurace úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="b91e0-146">The password is generated at the bottom of the **Configuration repository** tab.</span></span>

![Generovat heslo][api-management-generate-password]

<span data-ttu-id="b91e0-148">Vygenerovat heslo, nejdřív ověřte, že **vypršení platnosti** je nastavená na požadované vypršení platnosti datum a čas a potom klikněte na **vygenerovat Token**.</span><span class="sxs-lookup"><span data-stu-id="b91e0-148">To generate a password, first ensure that the **Expiry** is set to the desired expiration date and time, and then click **Generate Token**.</span></span>

![Heslo][api-management-password]

> [!IMPORTANT]
> <span data-ttu-id="b91e0-150">Toto heslo si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="b91e0-150">Make a note of this password.</span></span> <span data-ttu-id="b91e0-151">Jakmile tuto stránku opustíte heslo se znovu nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="b91e0-151">Once you leave this page the password will not be displayed again.</span></span>
> 
> 

<span data-ttu-id="b91e0-152">Následující příklady použít nástroj Git Bash z [Git pro Windows](http://www.git-scm.com/downloads) ale můžete použít jakýkoli Git nástroj, který jste se seznámili s.</span><span class="sxs-lookup"><span data-stu-id="b91e0-152">The following examples use the Git Bash tool from [Git for Windows](http://www.git-scm.com/downloads) but you can use any Git tool that you are familiar with.</span></span>

<span data-ttu-id="b91e0-153">Otevřete svůj nástroj Git do požadované složky a spusťte následující příkaz klonovat úložiště git do místního počítače, pomocí příkazu poskytované portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b91e0-153">Open your Git tool in the desired folder and run the following command to clone the git repository to your local machine, using the command provided by the publisher portal.</span></span>

```
git clone https://bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="b91e0-154">Zadejte uživatelské jméno a heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="b91e0-154">Provide the user name and password when prompted.</span></span>

<span data-ttu-id="b91e0-155">Pokud se zobrazí všechny chyby, zkuste upravit vaše `git clone` příkazu zahrnovat uživatelské jméno a heslo, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-155">If you receive any errors, try modifying your `git clone` command to include the user name and password, as shown in the following example.</span></span>

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="b91e0-156">Pokud to poskytuje k chybě, zkuste URL kódování heslo část příkazu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-156">If this provides an error, try URL encoding the password portion of the command.</span></span> <span data-ttu-id="b91e0-157">Jeden rychlý způsob, jak to udělat je otevřete Visual Studio a vydejte následující příkaz v **hodnot proměnných**.</span><span class="sxs-lookup"><span data-stu-id="b91e0-157">One quick way to do this is to open Visual Studio, and issue the following command in the **Immediate Window**.</span></span> <span data-ttu-id="b91e0-158">Chcete-li otevřít **hodnot proměnných**, otevřete jakékoli řešení nebo produktu project v sadě Visual Studio (nebo vytvořte novou prázdnou konzolovou aplikaci) a zvolte **Windows**, **Immediate** z **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="b91e0-158">To open the **Immediate Window**, open any solution or project in Visual Studio (or create a new empty console application), and choose **Windows**, **Immediate** from the **Debug** menu.</span></span>

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

<span data-ttu-id="b91e0-159">Použijte zakódované heslo společně s vaše uživatelské jméno a úložiště umístění k vytvoření příkazu git.</span><span class="sxs-lookup"><span data-stu-id="b91e0-159">Use the encoded password along with your user name and repository location to construct the git command.</span></span>

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="b91e0-160">Jakmile je klonovat úložiště můžete zobrazit a pracovat v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="b91e0-160">Once the repository is cloned you can view and work with it in your local file system.</span></span> <span data-ttu-id="b91e0-161">Další informace najdete v tématu [souborů a složek struktury odkaz místní úložiště Git](#file-and-folder-structure-reference-of-local-git-repository).</span><span class="sxs-lookup"><span data-stu-id="b91e0-161">For more information, see [File and folder structure reference of local Git repository](#file-and-folder-structure-reference-of-local-git-repository).</span></span>

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a><span data-ttu-id="b91e0-162">Chcete-li aktualizovat místní úložiště s nejnovější konfigurací instance služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-162">To update your local repository with the most current service instance configuration</span></span>
<span data-ttu-id="b91e0-163">Pokud provedete změny instanci služby API Management na portálu vydavatele nebo pomocí rozhraní REST API, musíte uložit tyto změny do úložiště než budete moct aktualizovat místní úložiště s nejnovější změny.</span><span class="sxs-lookup"><span data-stu-id="b91e0-163">If you make changes to your API Management service instance in the publisher portal or using the REST API, you must save these changes to the repository before you can update your local repository with the latest changes.</span></span> <span data-ttu-id="b91e0-164">Chcete-li to provést, klikněte na tlačítko **uložit konfiguraci úložiště** na **konfigurace úložiště** v portálu vydavatele a potom vydat příkaz v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="b91e0-164">To do this, click **Save configuration to repository** on the **Configuration repository** tab in the publisher portal, and then issue the following command in your local repository.</span></span>

```
git pull
```

<span data-ttu-id="b91e0-165">Dřív, než spustíte `git pull` Ujistěte se, že jste ve složce pro místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="b91e0-165">Before running `git pull` ensure that you are in the folder for your local repository.</span></span> <span data-ttu-id="b91e0-166">Pokud jste právě dokončili `git clone` příkaz, pak musí změňte adresář na vašem úložišti spuštěním příkazu jako následující.</span><span class="sxs-lookup"><span data-stu-id="b91e0-166">If you have just completed the `git clone` command, then you must change the directory to your repo by running a command like the following.</span></span>

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a><span data-ttu-id="b91e0-167">K replikaci změn z vaší místní úložiště na server úložiště</span><span class="sxs-lookup"><span data-stu-id="b91e0-167">To push changes from your local repo to the server repo</span></span>
<span data-ttu-id="b91e0-168">K replikaci změn z místního úložiště do úložiště serveru, musí potvrdit změny a vložit je na serveru úložiště.</span><span class="sxs-lookup"><span data-stu-id="b91e0-168">To push changes from your local repository to the server repository, you must commit your changes and then push them to the server repository.</span></span> <span data-ttu-id="b91e0-169">Potvrzení změny, otevřete svůj nástroj příkazu Git, přejděte do adresáře, ve vašem místním úložišti a vydat následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="b91e0-169">To commit your changes, open your Git command tool, switch to the directory of your local repository, and issue the following commands.</span></span>

```
git add --all
git commit -m "Description of your changes"
```

<span data-ttu-id="b91e0-170">K replikaci všechna potvrzení na serveru, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="b91e0-170">To push all of the commits to the server, run the following command.</span></span>

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a><span data-ttu-id="b91e0-171">K nasazení změny konfigurace služby v instanci služby API Management</span><span class="sxs-lookup"><span data-stu-id="b91e0-171">To deploy any service configuration changes to the API Management service instance</span></span>
<span data-ttu-id="b91e0-172">Jakmile jsou místní změny potvrzeny a instaluje do úložiště serveru, můžete nasadit instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b91e0-172">Once your local changes are committed and pushed to the server repository, you can deploy them to your API Management service instance.</span></span>

![Nasazení][api-management-configuration-deploy]

<span data-ttu-id="b91e0-174">Informace o provedení této operace pomocí rozhraní REST API najdete v tématu [nasazení Git změny konfigurační databáze pomocí rozhraní REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span><span class="sxs-lookup"><span data-stu-id="b91e0-174">For information on performing this operation using the REST API, see [Deploy Git changes to configuration database using the REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span></span>

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a><span data-ttu-id="b91e0-175">Odkaz struktury souborů a složek místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="b91e0-175">File and folder structure reference of local Git repository</span></span>
<span data-ttu-id="b91e0-176">Soubory a složky v úložišti místní git obsahovat informace o konfiguraci o instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-176">The files and folders in the local git repository contain the configuration information about the service instance.</span></span>

| <span data-ttu-id="b91e0-177">Položka</span><span class="sxs-lookup"><span data-stu-id="b91e0-177">Item</span></span> | <span data-ttu-id="b91e0-178">Popis</span><span class="sxs-lookup"><span data-stu-id="b91e0-178">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b91e0-179">Kořenová složka api management</span><span class="sxs-lookup"><span data-stu-id="b91e0-179">root api-management folder</span></span> |<span data-ttu-id="b91e0-180">Obsahuje nejvyšší úrovně konfiguraci pro instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-180">Contains top-level configuration for the service instance</span></span> |
| <span data-ttu-id="b91e0-181">rozhraní API složky</span><span class="sxs-lookup"><span data-stu-id="b91e0-181">apis folder</span></span> |<span data-ttu-id="b91e0-182">Obsahuje konfiguraci pro rozhraní API v instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-182">Contains the configuration for the apis in the service instance</span></span> |
| <span data-ttu-id="b91e0-183">složka skupiny</span><span class="sxs-lookup"><span data-stu-id="b91e0-183">groups folder</span></span> |<span data-ttu-id="b91e0-184">Obsahuje konfiguraci skupin v instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-184">Contains the configuration for the groups in the service instance</span></span> |
| <span data-ttu-id="b91e0-185">Složka zásad</span><span class="sxs-lookup"><span data-stu-id="b91e0-185">policies folder</span></span> |<span data-ttu-id="b91e0-186">Obsahuje zásady v instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-186">Contains the policies in the service instance</span></span> |
| <span data-ttu-id="b91e0-187">portalStyles složky</span><span class="sxs-lookup"><span data-stu-id="b91e0-187">portalStyles folder</span></span> |<span data-ttu-id="b91e0-188">Obsahuje konfiguraci pro přizpůsobení portálu vývojáře v instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-188">Contains the configuration for the developer portal customizations in the service instance</span></span> |
| <span data-ttu-id="b91e0-189">produkty složky</span><span class="sxs-lookup"><span data-stu-id="b91e0-189">products folder</span></span> |<span data-ttu-id="b91e0-190">Obsahuje konfiguraci pro produkty v instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-190">Contains the configuration for the products in the service instance</span></span> |
| <span data-ttu-id="b91e0-191">složka šablon</span><span class="sxs-lookup"><span data-stu-id="b91e0-191">templates folder</span></span> |<span data-ttu-id="b91e0-192">Obsahuje konfiguraci e-mailových šablon v instanci služby</span><span class="sxs-lookup"><span data-stu-id="b91e0-192">Contains the configuration for the email templates in the service instance</span></span> |

<span data-ttu-id="b91e0-193">Každé složky může obsahovat jeden nebo více souborů a v některých případech jeden nebo více složek, například do složky pro každé rozhraní API, produkty nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="b91e0-193">Each folder can contain one or more files, and in some cases one or more folders, for example a folder for each API, product, or group.</span></span> <span data-ttu-id="b91e0-194">Soubory v rámci každé složky jsou specifické pro typ entity popsaného název složky.</span><span class="sxs-lookup"><span data-stu-id="b91e0-194">The files within each folder are specific for the entity type described by the folder name.</span></span>

| <span data-ttu-id="b91e0-195">Typ souboru</span><span class="sxs-lookup"><span data-stu-id="b91e0-195">File type</span></span> | <span data-ttu-id="b91e0-196">Účel</span><span class="sxs-lookup"><span data-stu-id="b91e0-196">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="b91e0-197">JSON</span><span class="sxs-lookup"><span data-stu-id="b91e0-197">json</span></span> |<span data-ttu-id="b91e0-198">Informace o konfiguraci o odpovídající entity</span><span class="sxs-lookup"><span data-stu-id="b91e0-198">Configuration information about the respective entity</span></span> |
| <span data-ttu-id="b91e0-199">HTML</span><span class="sxs-lookup"><span data-stu-id="b91e0-199">html</span></span> |<span data-ttu-id="b91e0-200">Popisy o entitě, často zobrazeny v portálu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="b91e0-200">Descriptions about the entity, often displayed in the developer portal</span></span> |
| <span data-ttu-id="b91e0-201">xml</span><span class="sxs-lookup"><span data-stu-id="b91e0-201">xml</span></span> |<span data-ttu-id="b91e0-202">Příkazy zásad</span><span class="sxs-lookup"><span data-stu-id="b91e0-202">Policy statements</span></span> |
| <span data-ttu-id="b91e0-203">šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="b91e0-203">css</span></span> |<span data-ttu-id="b91e0-204">Šablony stylů pro přizpůsobení portálu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="b91e0-204">Style sheets for developer portal customization</span></span> |

<span data-ttu-id="b91e0-205">Tyto soubory lze vytvořit, odstranit, upravit a spravovat v místním systému souborů a nasazení změn zpět do instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b91e0-205">These files can be created, deleted, edited, and managed on your local file system, and the changes deployed back to the your API Management service instance.</span></span>

> [!NOTE]
> <span data-ttu-id="b91e0-206">Následující entity, které nejsou obsaženy v úložišti Git a nelze konfigurovat pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="b91e0-206">The following entities are not contained in the Git repository and cannot be configured using Git.</span></span>
> 
> * <span data-ttu-id="b91e0-207">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="b91e0-207">Users</span></span>
> * <span data-ttu-id="b91e0-208">Předplatná</span><span class="sxs-lookup"><span data-stu-id="b91e0-208">Subscriptions</span></span>
> * <span data-ttu-id="b91e0-209">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b91e0-209">Properties</span></span>
> * <span data-ttu-id="b91e0-210">Vývojáře portálu entity než styly</span><span class="sxs-lookup"><span data-stu-id="b91e0-210">Developer portal entities other than styles</span></span>
> 
> 

### <a name="root-api-management-folder"></a><span data-ttu-id="b91e0-211">Kořenová složka api management</span><span class="sxs-lookup"><span data-stu-id="b91e0-211">Root api-management folder</span></span>
<span data-ttu-id="b91e0-212">Kořenové `api-management` složka obsahuje `configuration.json` soubor, který obsahuje nejvyšší úrovně informace o instanci služby v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-212">The root `api-management` folder contains a `configuration.json` file that contains top-level information about the service instance in the following format.</span></span>

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

<span data-ttu-id="b91e0-213">První čtyři nastavení (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, a `UserRegistrationTermsConsentRequired`) mapování na následující nastavení na **identity** ve **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="b91e0-213">The first four settings (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, and `UserRegistrationTermsConsentRequired`) map to the following settings on the **Identities** tab in the **Security** section.</span></span>

| <span data-ttu-id="b91e0-214">Nastavení identity</span><span class="sxs-lookup"><span data-stu-id="b91e0-214">Identity setting</span></span> | <span data-ttu-id="b91e0-215">Se mapuje na</span><span class="sxs-lookup"><span data-stu-id="b91e0-215">Maps to</span></span> |
| --- | --- |
| <span data-ttu-id="b91e0-216">RegistrationEnabled</span><span class="sxs-lookup"><span data-stu-id="b91e0-216">RegistrationEnabled</span></span> |<span data-ttu-id="b91e0-217">**Anonymní uživatelé přesměruje na přihlašovací stránce** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="b91e0-217">**Redirect anonymous users to sign-in page** checkbox</span></span> |
| <span data-ttu-id="b91e0-218">UserRegistrationTerms</span><span class="sxs-lookup"><span data-stu-id="b91e0-218">UserRegistrationTerms</span></span> |<span data-ttu-id="b91e0-219">**Podmínky použití na registraci uživatele** textbox</span><span class="sxs-lookup"><span data-stu-id="b91e0-219">**Terms of use on user signup** textbox</span></span> |
| <span data-ttu-id="b91e0-220">UserRegistrationTermsEnabled</span><span class="sxs-lookup"><span data-stu-id="b91e0-220">UserRegistrationTermsEnabled</span></span> |<span data-ttu-id="b91e0-221">**Zobrazit podmínky použití na přihlašovací stránce** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="b91e0-221">**Show terms of use on signup page** checkbox</span></span> |
| <span data-ttu-id="b91e0-222">UserRegistrationTermsConsentRequired</span><span class="sxs-lookup"><span data-stu-id="b91e0-222">UserRegistrationTermsConsentRequired</span></span> |<span data-ttu-id="b91e0-223">**Vyžadovat souhlas** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="b91e0-223">**Require consent** checkbox</span></span> |

![Nastavení identity][api-management-identity-settings]

<span data-ttu-id="b91e0-225">Nastavení další čtyři (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, a `DelegationValidationKey`) mapování na následující nastavení na **delegování** ve **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="b91e0-225">The next four settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, and `DelegationValidationKey`) map to the following settings on the **Delegation** tab in the **Security** section.</span></span>

| <span data-ttu-id="b91e0-226">Nastavení delegování</span><span class="sxs-lookup"><span data-stu-id="b91e0-226">Delegation setting</span></span> | <span data-ttu-id="b91e0-227">Se mapuje na</span><span class="sxs-lookup"><span data-stu-id="b91e0-227">Maps to</span></span> |
| --- | --- |
| <span data-ttu-id="b91e0-228">DelegationEnabled</span><span class="sxs-lookup"><span data-stu-id="b91e0-228">DelegationEnabled</span></span> |<span data-ttu-id="b91e0-229">**Delegát přihlášení a registrace** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="b91e0-229">**Delegate sign-in & sign-up** checkbox</span></span> |
| <span data-ttu-id="b91e0-230">DelegationUrl</span><span class="sxs-lookup"><span data-stu-id="b91e0-230">DelegationUrl</span></span> |<span data-ttu-id="b91e0-231">**Adresa URL koncového bodu delegování** textbox</span><span class="sxs-lookup"><span data-stu-id="b91e0-231">**Delegation endpoint URL** textbox</span></span> |
| <span data-ttu-id="b91e0-232">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="b91e0-232">DelegatedSubscriptionEnabled</span></span> |<span data-ttu-id="b91e0-233">**Delegovat odběru produktů** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="b91e0-233">**Delegate product subscription** checkbox</span></span> |
| <span data-ttu-id="b91e0-234">DelegationValidationKey</span><span class="sxs-lookup"><span data-stu-id="b91e0-234">DelegationValidationKey</span></span> |<span data-ttu-id="b91e0-235">**Delegovat ověřovací klíč** textbox</span><span class="sxs-lookup"><span data-stu-id="b91e0-235">**Delegate Validation Key** textbox</span></span> |

![Nastavení delegování][api-management-delegation-settings]

<span data-ttu-id="b91e0-237">Nastavení konečného `$ref-policy`, se mapuje na soubor globální zásady příkazy pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-237">The final setting, `$ref-policy`, maps to the global policy statements file for the service instance.</span></span>

### <a name="apis-folder"></a><span data-ttu-id="b91e0-238">rozhraní API složky</span><span class="sxs-lookup"><span data-stu-id="b91e0-238">apis folder</span></span>
<span data-ttu-id="b91e0-239">`apis` Složka obsahuje složku pro každé rozhraní API v instanci služby, který obsahuje následující položky.</span><span class="sxs-lookup"><span data-stu-id="b91e0-239">The `apis` folder contains a folder for each API in the service instance which contains the following items.</span></span>

* <span data-ttu-id="b91e0-240">`apis\<api name>\configuration.json`-Toto je konfigurace pro rozhraní API a obsahuje informace o adresu URL back-end služby a činnosti.</span><span class="sxs-lookup"><span data-stu-id="b91e0-240">`apis\<api name>\configuration.json` - this is the configuration for the API and contains information about the backend service URL and the operations.</span></span> <span data-ttu-id="b91e0-241">Toto je stejné informace, které by byla vrácena, pokud byste chtěli volání [získat konkrétní rozhraní API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) s `export=true` v `application/json` formátu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-241">This is the same information that would be returned if you were to call [Get a specific API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) with `export=true` in `application/json` format.</span></span>
* <span data-ttu-id="b91e0-242">`apis\<api name>\api.description.html`-Toto je popis rozhraní API a odpovídá `description` vlastnost [rozhraní API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="b91e0-242">`apis\<api name>\api.description.html` - this is the description of the API and corresponds to the `description` property of the [API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span></span>
* <span data-ttu-id="b91e0-243">`apis\<api name>\operations\`– Tato složka obsahuje `<operation name>.description.html` soubory, které se mapují na operace v rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b91e0-243">`apis\<api name>\operations\` - this folder contains `<operation name>.description.html` files that map to the operations in the API.</span></span> <span data-ttu-id="b91e0-244">Každý soubor obsahuje popis jedné operace v rozhraní API, která se mapuje na `description` vlastnost [operaci entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) v rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="b91e0-244">Each file contains the description of a single operation in the API which maps to the `description` property of the [operation entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in the REST API.</span></span>

### <a name="groups-folder"></a><span data-ttu-id="b91e0-245">složka skupiny</span><span class="sxs-lookup"><span data-stu-id="b91e0-245">groups folder</span></span>
<span data-ttu-id="b91e0-246">`groups` Složka obsahuje složku pro každou skupinu definované v instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-246">The `groups` folder contains a folder for each group defined in the service instance.</span></span>

* <span data-ttu-id="b91e0-247">`groups\<group name>\configuration.json`-jedná o konfiguraci pro skupinu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-247">`groups\<group name>\configuration.json` - this is the configuration for the group.</span></span> <span data-ttu-id="b91e0-248">Toto je stejné informace, které by byla vrácena, pokud byste chtěli volání [získat určité skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operaci.</span><span class="sxs-lookup"><span data-stu-id="b91e0-248">This is the same information that would be returned if you were to call the [Get a specific group](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operation.</span></span>
* <span data-ttu-id="b91e0-249">`groups\<group name>\description.html`-Toto je popis skupiny a odpovídá `description` vlastnost [skupiny entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="b91e0-249">`groups\<group name>\description.html` - this is the description of the group and corresponds to the `description` property of the [group entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span></span>

### <a name="policies-folder"></a><span data-ttu-id="b91e0-250">Složka zásad</span><span class="sxs-lookup"><span data-stu-id="b91e0-250">policies folder</span></span>
<span data-ttu-id="b91e0-251">`policies` Složka obsahuje příkazy zásad pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-251">The `policies` folder contains the policy statements for your service instance.</span></span>

* <span data-ttu-id="b91e0-252">`policies\global.xml`-obsahuje zásady definované v globálním oboru pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-252">`policies\global.xml` - contains policies defined at global scope for your service instance.</span></span>
* <span data-ttu-id="b91e0-253">`policies\apis\<api name>\`– Pokud máte jakékoli zásady definované v oboru rozhraní API, jsou obsažené v této složce.</span><span class="sxs-lookup"><span data-stu-id="b91e0-253">`policies\apis\<api name>\` - if you have any policies defined at API scope, they are contained in this folder.</span></span>
* <span data-ttu-id="b91e0-254">`policies\apis\<api name>\<operation name>\`Složka – Pokud máte jakékoli zásady definované v oboru operaci, jsou obsažené v této složce v `<operation name>.xml` soubory, které jsou mapovány na příkazy zásad pro každou operaci.</span><span class="sxs-lookup"><span data-stu-id="b91e0-254">`policies\apis\<api name>\<operation name>\` folder - if you have any policies defined at operation scope, they are contained in this folder in `<operation name>.xml` files that map to the policy statements for each operation.</span></span>
* <span data-ttu-id="b91e0-255">`policies\products\`– Pokud máte jakékoli zásady definované v produktu oboru, jsou obsažené v této složce, která obsahuje `<product name>.xml` soubory, které jsou mapovány na příkazy zásad pro jednotlivé produkty.</span><span class="sxs-lookup"><span data-stu-id="b91e0-255">`policies\products\` - if you have any policies defined at product scope, they are contained in this folder, which contains `<product name>.xml` files that map to the policy statements for each product.</span></span>

### <a name="portalstyles-folder"></a><span data-ttu-id="b91e0-256">portalStyles složky</span><span class="sxs-lookup"><span data-stu-id="b91e0-256">portalStyles folder</span></span>
<span data-ttu-id="b91e0-257">`portalStyles` Složka obsahuje konfiguraci a stylu stylů pro vývojáře přizpůsobení portálu pro instance služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-257">The `portalStyles` folder contains configuration and style sheets for developer portal customizations for the service instance.</span></span>

* <span data-ttu-id="b91e0-258">`portalStyles\configuration.json`-obsahuje názvy stylů používá portál pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="b91e0-258">`portalStyles\configuration.json` - contains the names of the style sheets used by the developer portal</span></span>
* <span data-ttu-id="b91e0-259">`portalStyles\<style name>.css`-Každý `<style name>.css` soubor obsahuje styly pro portál pro vývojáře (`Preview.css` a `Production.css` ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="b91e0-259">`portalStyles\<style name>.css` - each `<style name>.css` file contains styles for the developer portal (`Preview.css` and `Production.css` by default).</span></span>

### <a name="products-folder"></a><span data-ttu-id="b91e0-260">produkty složky</span><span class="sxs-lookup"><span data-stu-id="b91e0-260">products folder</span></span>
<span data-ttu-id="b91e0-261">`products` Složka obsahuje složku pro každý produkt definované v instanci služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-261">The `products` folder contains a folder for each product defined in the service instance.</span></span>

* <span data-ttu-id="b91e0-262">`products\<product name>\configuration.json`-Toto je konfigurace produktu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-262">`products\<product name>\configuration.json` - this is the configuration for the product.</span></span> <span data-ttu-id="b91e0-263">Toto je stejné informace, které by byla vrácena, pokud byste chtěli volání [získat určitý produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operaci.</span><span class="sxs-lookup"><span data-stu-id="b91e0-263">This is the same information that would be returned if you were to call the [Get a specific product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operation.</span></span>
* <span data-ttu-id="b91e0-264">`products\<product name>\product.description.html`-Toto je popis produktu a odpovídá `description` vlastnost [entity produktu](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) v rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="b91e0-264">`products\<product name>\product.description.html` - this is the description of the product and corresponds to the `description` property of the [product entity](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in the REST API.</span></span>

### <a name="templates"></a><span data-ttu-id="b91e0-265">šablony</span><span class="sxs-lookup"><span data-stu-id="b91e0-265">templates</span></span>
<span data-ttu-id="b91e0-266">`templates` Složka obsahuje konfiguraci [e-mailových šablon](api-management-howto-configure-notifications.md) instance služby.</span><span class="sxs-lookup"><span data-stu-id="b91e0-266">The `templates` folder contains configuration for the [email templates](api-management-howto-configure-notifications.md) of the service instance.</span></span>

* <span data-ttu-id="b91e0-267">`<template name>\configuration.json`-Toto je konfigurace pro e-mailové šablony.</span><span class="sxs-lookup"><span data-stu-id="b91e0-267">`<template name>\configuration.json` - this is the configuration for the email template.</span></span>
* <span data-ttu-id="b91e0-268">`<template name>\body.html`-Toto je text šablony e-mailu.</span><span class="sxs-lookup"><span data-stu-id="b91e0-268">`<template name>\body.html` - this is the body of the email template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b91e0-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b91e0-269">Next steps</span></span>
<span data-ttu-id="b91e0-270">Informace o jiných způsobech spravovat instanci služby najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="b91e0-270">For information on other ways to manage your service instance, see:</span></span>

* <span data-ttu-id="b91e0-271">Spravovat instanci služby pomocí následující rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b91e0-271">Manage your service instance using the following PowerShell cmdlets</span></span>
  * [<span data-ttu-id="b91e0-272">Referenční informace k rutinám PowerShellu pro nasazení služeb</span><span class="sxs-lookup"><span data-stu-id="b91e0-272">Service deployment PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [<span data-ttu-id="b91e0-273">Správa služeb referenční informace o rutinách prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b91e0-273">Service management PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* <span data-ttu-id="b91e0-274">Spravovat instanci služby na portálu vydavatele</span><span class="sxs-lookup"><span data-stu-id="b91e0-274">Manage your service instance in the publisher portal</span></span>
  * [<span data-ttu-id="b91e0-275">Správa vašeho prvního rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b91e0-275">Manage your first API</span></span>](api-management-get-started.md)
* <span data-ttu-id="b91e0-276">Spravovat instanci služby pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="b91e0-276">Manage your service instance using the REST API</span></span>
  * [<span data-ttu-id="b91e0-277">Referenční dokumentace rozhraní API REST API Management</span><span class="sxs-lookup"><span data-stu-id="b91e0-277">API Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="b91e0-278">Podívejte se na video s přehledem</span><span class="sxs-lookup"><span data-stu-id="b91e0-278">Watch a video overview</span></span>
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




