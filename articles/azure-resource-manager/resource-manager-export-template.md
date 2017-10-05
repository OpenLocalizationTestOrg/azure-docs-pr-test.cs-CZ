---
title: "Vyexportování šablony Azure Resource Manageru | Dokumentace Microsoftu"
description: "Pomocí Azure Resource Manageru si můžete vyexportovat šablonu z existující skupiny prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: 1801ef47e5b182e0bcd5b23970a2999633b4a852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a><span data-ttu-id="38811-103">Vyexportování šablony Azure Resource Manageru z existujících prostředků</span><span class="sxs-lookup"><span data-stu-id="38811-103">Export an Azure Resource Manager template from existing resources</span></span>
<span data-ttu-id="38811-104">V tomto článku se naučíte, jak exportovat šablonu Resource Manageru z existujících prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="38811-104">In this article, you learn how to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="38811-105">Tuto vygenerovanou šablonu můžete použít k lepšímu pochopení syntaxe šablon.</span><span class="sxs-lookup"><span data-stu-id="38811-105">You can use that generated template to gain a better understanding of template syntax.</span></span>

<span data-ttu-id="38811-106">Existují dva způsoby, jak exportovat šablonu:</span><span class="sxs-lookup"><span data-stu-id="38811-106">There are two ways to export a template:</span></span>

* <span data-ttu-id="38811-107">Můžete exportovat **skutečnou šablonu použitou k nasazení**.</span><span class="sxs-lookup"><span data-stu-id="38811-107">You can export the **actual template used for deployment**.</span></span> <span data-ttu-id="38811-108">Exportovaná šablona zahrnuje všechny parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně.</span><span class="sxs-lookup"><span data-stu-id="38811-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="38811-109">Tento přístup je užitečný, pokud jste nasadili prostředky prostřednictvím portálu a chcete vidět šablonu, která tyto prostředky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="38811-109">This approach is helpful when you deployed resources through the portal, and want to see the template to create those resources.</span></span> <span data-ttu-id="38811-110">Tato šablona je ihned použitelná.</span><span class="sxs-lookup"><span data-stu-id="38811-110">This template is readily usable.</span></span> 
* <span data-ttu-id="38811-111">Můžete exportovat **vygenerovanou šablonu, která představuje aktuální stav skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="38811-111">You can export a **generated template that represents the current state of the resource group**.</span></span> <span data-ttu-id="38811-112">Exportovaná šablona není založena na žádné šabloně, kterou jste použili k nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-112">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="38811-113">Místo toho export vytvoří šablonu, která je snímkem aktuálního stavu skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="38811-113">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="38811-114">Exportovaná šablona má řadu pevně definovaných hodnot a pravděpodobně méně parametrů, než byste obvykle definovali.</span><span class="sxs-lookup"><span data-stu-id="38811-114">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="38811-115">Tento přístup je užitečný, pokud jste po nasazení upravili skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="38811-115">This approach is useful when you have modified the resource group after deployment.</span></span> <span data-ttu-id="38811-116">Tato šablona obvykle vyžaduje úpravy, než je možné ji použít.</span><span class="sxs-lookup"><span data-stu-id="38811-116">This template usually requires modifications before it is usable.</span></span>

<span data-ttu-id="38811-117">Toto téma ukazuje oba přístupy prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="38811-117">This topic shows both approaches through the portal.</span></span>

## <a name="deploy-resources"></a><span data-ttu-id="38811-118">Nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="38811-118">Deploy resources</span></span>
<span data-ttu-id="38811-119">Začněme tím, že do Azure nasadíme prostředky, které můžete použít k exportu v podobě šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-119">Let's start by deploying resources to Azure that you can use for exporting as a template.</span></span> <span data-ttu-id="38811-120">Pokud už v předplatném máte skupinu prostředků, kterou chcete exportovat do šablony, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="38811-120">If you already have a resource group in your subscription that you want to export to a template, you can skip this section.</span></span> <span data-ttu-id="38811-121">Zbývající část tohoto článku předpokládá, že jste nasadili webovou aplikaci a řešení databáze SQL uvedené v této části.</span><span class="sxs-lookup"><span data-stu-id="38811-121">The remainder of this article assumes you have deployed the web app and SQL database solution shown in this section.</span></span> <span data-ttu-id="38811-122">Pokud používáte jiné řešení, vaše prostředí může být trochu jiné, ale postup exportu šablony je stejný.</span><span class="sxs-lookup"><span data-stu-id="38811-122">If you use a different solution, your experience might be a little different, but the steps to export a template are the same.</span></span> 

1. <span data-ttu-id="38811-123">Na webu [Azure Portal](https://portal.azure.com) vyberte **Nový**.</span><span class="sxs-lookup"><span data-stu-id="38811-123">In the [Azure portal](https://portal.azure.com), select **New**.</span></span>
   
      ![výběr možnosti Nový](./media/resource-manager-export-template/new.png)
2. <span data-ttu-id="38811-125">Vyhledejte řešení **Webová aplikace a SQL** a vyberte ho z dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="38811-125">Search for **web app + SQL** and select it from the available options.</span></span>
   
      ![vyhledání řešení Webová aplikace a SQL](./media/resource-manager-export-template/webapp-sql.png)

3. <span data-ttu-id="38811-127">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38811-127">Select **Create**.</span></span>

      ![výběr možnosti Vytvořit](./media/resource-manager-export-template/create.png)

4. <span data-ttu-id="38811-129">Zadejte požadované hodnoty pro webovou aplikaci a databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="38811-129">Provide the required values for the web app and SQL database.</span></span> <span data-ttu-id="38811-130">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38811-130">Select **Create**.</span></span>

      ![zadání hodnot webu a SQL](./media/resource-manager-export-template/provide-web-values.png)

<span data-ttu-id="38811-132">Nasazení může trvat minutu.</span><span class="sxs-lookup"><span data-stu-id="38811-132">The deployment may take a minute.</span></span> <span data-ttu-id="38811-133">Po dokončení nasazení vaše předplatné obsahuje řešení.</span><span class="sxs-lookup"><span data-stu-id="38811-133">After the deployment finishes, your subscription contains the solution.</span></span>

## <a name="view-template-from-deployment-history"></a><span data-ttu-id="38811-134">Zobrazení šablony z historie nasazení</span><span class="sxs-lookup"><span data-stu-id="38811-134">View template from deployment history</span></span>
1. <span data-ttu-id="38811-135">Přejděte do okna nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="38811-135">Go to the resource group blade for your new resource group.</span></span> <span data-ttu-id="38811-136">Všimněte si, že okno zobrazuje výsledek posledního nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-136">Notice that the blade shows the result of the last deployment.</span></span> <span data-ttu-id="38811-137">Vyberte tento odkaz.</span><span class="sxs-lookup"><span data-stu-id="38811-137">Select this link.</span></span>
   
      ![okno skupiny prostředků](./media/resource-manager-export-template/select-deployment.png)
2. <span data-ttu-id="38811-139">Zobrazí se vám historie nasazení pro skupinu.</span><span class="sxs-lookup"><span data-stu-id="38811-139">You see a history of deployments for the group.</span></span> <span data-ttu-id="38811-140">Ve vašem případě bude okno pravděpodobně uvádět jenom jedno nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-140">In your case, the blade probably lists only one deployment.</span></span> <span data-ttu-id="38811-141">Vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="38811-141">Select this deployment.</span></span>
   
     ![poslední nasazení](./media/resource-manager-export-template/select-history.png)
3. <span data-ttu-id="38811-143">Okno zobrazí souhrn vybraného nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-143">The blade displays a summary of the deployment.</span></span> <span data-ttu-id="38811-144">Souhrn obsahuje stav nasazení a jeho operací a také hodnoty, které jste zadali pro parametry.</span><span class="sxs-lookup"><span data-stu-id="38811-144">The summary includes the status of the deployment and its operations and the values that you provided for parameters.</span></span> <span data-ttu-id="38811-145">Pokud chcete zobrazit šablonu, kterou jste použili k nasazení, vyberte možnost **Zobrazit šablonu**.</span><span class="sxs-lookup"><span data-stu-id="38811-145">To see the template that you used for the deployment, select **View template**.</span></span>
   
     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/view-template.png)
4. <span data-ttu-id="38811-147">Resource Manager pro vás načte následujících sedm souborů:</span><span class="sxs-lookup"><span data-stu-id="38811-147">Resource Manager retrieves the following seven files for you:</span></span>
   
   1. <span data-ttu-id="38811-148">**Template** - Šablona, která definuje infrastrukturu pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="38811-148">**Template** - The template that defines the infrastructure for your solution.</span></span> <span data-ttu-id="38811-149">Když jste prostřednictvím portálu vytvářeli účet úložiště, Resource Manager k jeho nasazení použil šablonu a tuto šablonu uložil pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="38811-149">When you created the storage account through the portal, Resource Manager used a template to deploy it and saved that template for future reference.</span></span>
   2. <span data-ttu-id="38811-150">**Parameters** - Soubor s parametry, který slouží k předávání hodnot během nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-150">**Parameters** - A parameter file that you can use to pass in values during deployment.</span></span> <span data-ttu-id="38811-151">Obsahuje hodnoty, které jste zadali při prvním nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-151">It contains the values that you provided during the first deployment.</span></span> <span data-ttu-id="38811-152">Kteroukoli z těchto hodnot můžete při opětovném nasazování šablony změnit.</span><span class="sxs-lookup"><span data-stu-id="38811-152">You can change any of these values when you redeploy the template.</span></span>
   3. <span data-ttu-id="38811-153">**CLI** - Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-153">**CLI** - An Azure command-line-interface (CLI) script file that you can use to deploy the template.</span></span>
   3. <span data-ttu-id="38811-154">**CLI 2.0** – Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="38811-154">**CLI 2.0** - An Azure command-line-interface (CLI) script file that you can use to deploy the template.</span></span>
   4. <span data-ttu-id="38811-155">**PowerShell** - Soubor skriptu Azure PowerShellu, který můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-155">**PowerShell** - An Azure PowerShell script file that you can use to deploy the template.</span></span>
   5. <span data-ttu-id="38811-156">**.NET** - Třída .NET, kterou můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-156">**.NET** - A .NET class that you can use to deploy the template.</span></span>
   6. <span data-ttu-id="38811-157">**Ruby** - Třída Ruby, kterou můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-157">**Ruby** - A Ruby class that you can use to deploy the template.</span></span>
      
      <span data-ttu-id="38811-158">Soubory jsou k dispozici prostřednictvím odkazů v rámci okna.</span><span class="sxs-lookup"><span data-stu-id="38811-158">The files are available through links across the blade.</span></span> <span data-ttu-id="38811-159">Ve výchozím nastavení zobrazí okno šablonu.</span><span class="sxs-lookup"><span data-stu-id="38811-159">By default, the blade displays the template.</span></span>
      
       ![zobrazení šablony](./media/resource-manager-export-template/see-template.png)
      
<span data-ttu-id="38811-161">Toto je skutečná šablona použitá k vytvoření vaší webové aplikace a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="38811-161">This template is the actual template used to create your web app and SQL database.</span></span> <span data-ttu-id="38811-162">Všimněte si, že obsahuje parametry, které vám umožňují během nasazení zadat jiné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="38811-162">Notice it contains parameters that enable you to provide different values during deployment.</span></span> <span data-ttu-id="38811-163">Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="38811-163">To learn more about the structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

## <a name="export-the-template-from-resource-group"></a><span data-ttu-id="38811-164">Export šablony ze skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="38811-164">Export the template from resource group</span></span>
<span data-ttu-id="38811-165">Pokud jste ručně změnili prostředky nebo přidali prostředky ve více nasazeních, získání šablony z historie nasazení nebude odrážet aktuální stav skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="38811-165">If you have manually changed your resources or added resources in multiple deployments, retrieving a template from the deployment history does not reflect the current state of the resource group.</span></span> <span data-ttu-id="38811-166">V této části se dozvíte, jak exportovat šablonu, která odráží aktuální stav skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="38811-166">This section shows you how to export a template that reflects the current state of the resource group.</span></span> 

> [!NOTE]
> <span data-ttu-id="38811-167">Nejde exportovat šablonu pro skupinu prostředků, která má víc než 200 prostředků.</span><span class="sxs-lookup"><span data-stu-id="38811-167">You cannot export a template for a resource group that has more than 200 resources.</span></span>
> 
> 

1. <span data-ttu-id="38811-168">Pokud chcete zobrazit šablonu pro skupinu prostředků, vyberte **Skript automatizace**.</span><span class="sxs-lookup"><span data-stu-id="38811-168">To view the template for a resource group, select **Automation script**.</span></span>
   
      ![export skupiny prostředků](./media/resource-manager-export-template/select-automation.png)
   
     <span data-ttu-id="38811-170">Resource Manager vyhodnotí prostředky ve skupině prostředků a vygeneruje pro tyto prostředky šablonu.</span><span class="sxs-lookup"><span data-stu-id="38811-170">Resource Manager evaluates the resources in the resource group, and generates a template for those resources.</span></span> <span data-ttu-id="38811-171">Ne všechny typy prostředků podporují funkci exportu šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-171">Not all resource types support the export template function.</span></span> <span data-ttu-id="38811-172">Může se zobrazit chyba oznamující, že došlo k problému s exportem.</span><span class="sxs-lookup"><span data-stu-id="38811-172">You may see an error stating that there is a problem with the export.</span></span> <span data-ttu-id="38811-173">Postup řešení těchto problémů najdete v části [Oprava problémů s exportem](#fix-export-issues).</span><span class="sxs-lookup"><span data-stu-id="38811-173">You learn how to handle those issues in the [Fix export issues](#fix-export-issues) section.</span></span>
2. <span data-ttu-id="38811-174">Znovu se zobrazí šest souborů, které můžete použít k opětovnému nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="38811-174">You again see the six files that you can use to redeploy the solution.</span></span> <span data-ttu-id="38811-175">Tentokrát je však šablona trochu jiná.</span><span class="sxs-lookup"><span data-stu-id="38811-175">However, this time the template is a little different.</span></span> <span data-ttu-id="38811-176">Všimněte si, že vygenerovaná šablona obsahuje méně parametrů než šablona v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="38811-176">Notice that the generated template contains fewer parameters than the template in previous section.</span></span> <span data-ttu-id="38811-177">Navíc je v této šabloně mnoho hodnot (jako hodnoty umístění a skladové položky) pevně zakódovaných místo toho, aby přijímaly hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="38811-177">Also, many of the values (like location and SKU values) are hard-coded in this template rather than accepting a parameter value.</span></span> <span data-ttu-id="38811-178">Před opětovným použitím této šablony možná budete chtít šablonu upravit, aby lépe využívala parametry.</span><span class="sxs-lookup"><span data-stu-id="38811-178">Before reusing this template, you might want to edit the template to make better use of parameters.</span></span> 
   
3. <span data-ttu-id="38811-179">Máte pár možností, jak s touto šablonou dále pracovat.</span><span class="sxs-lookup"><span data-stu-id="38811-179">You have a couple of options for continuing to work with this template.</span></span> <span data-ttu-id="38811-180">Šablonu si můžete stáhnout a pracovat na ní místně v editoru JSON</span><span class="sxs-lookup"><span data-stu-id="38811-180">You can either download the template and work on it locally with a JSON editor.</span></span> <span data-ttu-id="38811-181">nebo si ji můžete uložit do knihovny a pracovat s ní prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="38811-181">Or, you can save the template to your library and work on it through the portal.</span></span>
   
     <span data-ttu-id="38811-182">Pokud se vám s editorem JSON, jako je [VS Code](https://code.visualstudio.com/) nebo [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), dobře pracuje, můžete si ji stáhnout místně a použít tento editor.</span><span class="sxs-lookup"><span data-stu-id="38811-182">If you are comfortable using a JSON editor like [VS Code](https://code.visualstudio.com/) or [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), you might prefer downloading the template locally and using that editor.</span></span> <span data-ttu-id="38811-183">Pokud chcete pracovat místně, vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="38811-183">To work locally, select **Download**.</span></span>
   
      ![stažení šablony](./media/resource-manager-export-template/download-template.png)
   
     <span data-ttu-id="38811-185">Pokud editor JSON nechcete používat, můžete preferovat úpravy šablony prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="38811-185">If you are not set up with a JSON editor, you might prefer editing the template through the portal.</span></span> <span data-ttu-id="38811-186">Ve zbývající části tohoto tématu se předpokládá, že máte šablonu uloženou v knihovně na portálu.</span><span class="sxs-lookup"><span data-stu-id="38811-186">The remainder of this topic assumes you have saved the template to your library in the portal.</span></span> <span data-ttu-id="38811-187">Stejné změny syntaxe ale můžete v šabloně provést, ať pracujete místně v editoru JSON nebo prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="38811-187">However, you make the same syntax changes to the template whether working locally with a JSON editor or through the portal.</span></span> <span data-ttu-id="38811-188">Pokud chcete pracovat přes portál, vyberte **Přidat do knihovny**.</span><span class="sxs-lookup"><span data-stu-id="38811-188">To work through the portal, select **Add to library**.</span></span>
   
      ![přidání do knihovny](./media/resource-manager-export-template/add-to-library.png)
   
     <span data-ttu-id="38811-190">Když přidáváte šablonu do knihovny, uveďte její název a popis.</span><span class="sxs-lookup"><span data-stu-id="38811-190">When adding a template to the library, give the template a name and description.</span></span> <span data-ttu-id="38811-191">Potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="38811-191">Then, select **Save**.</span></span>
   
     ![nastavení hodnot šablony](./media/resource-manager-export-template/save-library-template.png)
4. <span data-ttu-id="38811-193">Pokud chcete šablonu uloženou v knihovně zobrazit, vyberte **Další služby**, napište **Šablony**, abyste vyfiltrovali výsledky, a vyberte **Šablony**.</span><span class="sxs-lookup"><span data-stu-id="38811-193">To view a template saved in your library, select **More services**, type **Templates** to filter results, select **Templates**.</span></span>
   
      ![vyhledání šablon](./media/resource-manager-export-template/find-templates.png)
5. <span data-ttu-id="38811-195">Vyberte šablonu s názvem, který jste uložili.</span><span class="sxs-lookup"><span data-stu-id="38811-195">Select the template with the name you saved.</span></span>
   
      ![výběr šablony](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-the-template"></a><span data-ttu-id="38811-197">Přizpůsobení šablony</span><span class="sxs-lookup"><span data-stu-id="38811-197">Customize the template</span></span>
<span data-ttu-id="38811-198">Vyexportovaná šablona funguje správně, pokud chcete vytvořit stejnou webovou aplikaci a databázi SQL pro všechna nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-198">The exported template works fine if you want to create the same web app and SQL database for every deployment.</span></span> <span data-ttu-id="38811-199">Resource Manager ale nabízí možnosti pro ještě flexibilnější nasazení šablon.</span><span class="sxs-lookup"><span data-stu-id="38811-199">However, Resource Manager provides options so that you can deploy templates with a lot more flexibility.</span></span> <span data-ttu-id="38811-200">V tomto článku zjistíte, jak přidat parametry pro jméno a heslo správce databáze.</span><span class="sxs-lookup"><span data-stu-id="38811-200">This article shows you how to add parameters for the database administrator name and password.</span></span> <span data-ttu-id="38811-201">Stejný postup můžete použít k přidání větší flexibility pro ostatní hodnoty v šabloně.</span><span class="sxs-lookup"><span data-stu-id="38811-201">You can use this same approach to add more flexibility for other values in the template.</span></span>

1. <span data-ttu-id="38811-202">Vyberte **Upravit** pro přizpůsobení šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-202">To customize the template, select **Edit**.</span></span>
   
     ![zobrazení šablony](./media/resource-manager-export-template/select-edit.png)
2. <span data-ttu-id="38811-204">Vyberte šablonu.</span><span class="sxs-lookup"><span data-stu-id="38811-204">Select the template.</span></span>
   
     ![Úprava šablony](./media/resource-manager-export-template/select-added-template.png)
3. <span data-ttu-id="38811-206">Abyste mohli předat hodnoty, které byste mohli chtít zadat během nasazování, přidejte následující dva parametry do části **parameters** v šabloně:</span><span class="sxs-lookup"><span data-stu-id="38811-206">To be able to pass the values that you might want to specify during deployment, add the following two parameters to the **parameters** section in the template:</span></span>

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. <span data-ttu-id="38811-207">Pokud chcete použít nové parametry, nahraďte definici SQL serveru v části **resources**.</span><span class="sxs-lookup"><span data-stu-id="38811-207">To use the new parameters, replace the SQL server definition in the **resources** section.</span></span> <span data-ttu-id="38811-208">Všimněte si, že **administratorLogin** a **administratorLoginPassword** jsou teď hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="38811-208">Notice that **administratorLogin** and **administratorLoginPassword** now use parameter values.</span></span>

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. <span data-ttu-id="38811-209">Po dokončení úprav šablony vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="38811-209">Select **OK** when you are done editing the template.</span></span>
7. <span data-ttu-id="38811-210">Uložte změny šablony kliknutím na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="38811-210">Select **Save** to save the changes to the template.</span></span>
   
     ![uložení šablony](./media/resource-manager-export-template/save-template.png)
8. <span data-ttu-id="38811-212">Pokud chcete aktualizovanou šablonu znovu nasadit, vyberte **Nasadit**.</span><span class="sxs-lookup"><span data-stu-id="38811-212">To redeploy the updated template, select **Deploy**.</span></span>
   
     ![nasazení šablony](./media/resource-manager-export-template/redeploy-template.png)
9. <span data-ttu-id="38811-214">Zadejte hodnoty parametrů a vyberte skupinu prostředků, do které prostředky nasadíte.</span><span class="sxs-lookup"><span data-stu-id="38811-214">Provide parameter values, and select a resource group to deploy the resources to.</span></span>


## <a name="fix-export-issues"></a><span data-ttu-id="38811-215">Oprava problémů s exportem</span><span class="sxs-lookup"><span data-stu-id="38811-215">Fix export issues</span></span>
<span data-ttu-id="38811-216">Ne všechny typy prostředků podporují funkci exportu šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-216">Not all resource types support the export template function.</span></span> <span data-ttu-id="38811-217">Tento problém můžete vyřešit ručním přidáním chybějících prostředků zpět do šablony.</span><span class="sxs-lookup"><span data-stu-id="38811-217">To resolve this issue, manually add the missing resources back into your template.</span></span> <span data-ttu-id="38811-218">Chybová zpráva obsahuje typy prostředků, které se nedají exportovat.</span><span class="sxs-lookup"><span data-stu-id="38811-218">The error message includes the resource types that cannot be exported.</span></span> <span data-ttu-id="38811-219">Vyhledejte tyto typy prostředků v [referenčních informacích k šablonám](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="38811-219">Find that resource type in [Template reference](/azure/templates/).</span></span> <span data-ttu-id="38811-220">Pokud například chcete ručně přidat bránu virtuální sítě, přečtěte si [referenční informace k šablonám o prostředku Microsoft.Network/virtualNetworkGateways](/azure/templates/microsoft.network/virtualnetworkgateways).</span><span class="sxs-lookup"><span data-stu-id="38811-220">For example, to manually add a virtual network gateway, see [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways).</span></span>

> [!NOTE]
> <span data-ttu-id="38811-221">K problémům s exportem může dojít pouze při exportu ze skupiny prostředků, ne z historie nasazení.</span><span class="sxs-lookup"><span data-stu-id="38811-221">You only encounter export issues when exporting from a resource group rather than from your deployment history.</span></span> <span data-ttu-id="38811-222">Pokud vaše poslední nasazení přesně reprezentuje aktuální stav skupiny prostředků, měli byste šablonu exportovat z historie nasazení, ne ze skupiny zdrojů.</span><span class="sxs-lookup"><span data-stu-id="38811-222">If your last deployment accurately represents the current state of the resource group, you should export the template from the deployment history rather than from the resource group.</span></span> <span data-ttu-id="38811-223">Ze skupiny zdrojů exportujte pouze tehdy, pokud jste ve skupině prostředků provedli změny, které nejsou definovány v jedné šabloně.</span><span class="sxs-lookup"><span data-stu-id="38811-223">Only export from a resource group when you have made changes to the resource group that are not defined in a single template.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="38811-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38811-224">Next steps</span></span>
<span data-ttu-id="38811-225">Naučili jste se, jak vyexportovat šablonu z prostředků, které jste vytvořili na portálu.</span><span class="sxs-lookup"><span data-stu-id="38811-225">You have learned how to export a template from resources that you created in the portal.</span></span>

* <span data-ttu-id="38811-226">Šablonu můžete nasadit pomocí těchto možností: [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) nebo [REST API](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="38811-226">You can deploy a template through [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md), or [REST API](resource-group-template-deploy-rest.md).</span></span>
* <span data-ttu-id="38811-227">Informace o tom, jak vyexportovat šablonu prostřednictvím PowerShellu, najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="38811-227">To see how to export a template through PowerShell, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="38811-228">Informace o tom, jak vyexportovat šablonu prostřednictvím rozhraní příkazového řádku Azure CLI, najdete v tématu věnovaném [Použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="38811-228">To see how to export a template through Azure CLI, see [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>

