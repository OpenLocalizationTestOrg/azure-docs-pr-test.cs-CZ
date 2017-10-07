---
title: "šablony Azure Resource Manageru aaaExport | Microsoft Docs"
description: "Pomocí Azure Resource Manageru tooexport šablonu z existující skupinu prostředků."
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
ms.openlocfilehash: 94daa4812da2fec705044ca31c8e74e6d59bd53f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a><span data-ttu-id="db932-103">Vyexportování šablony Azure Resource Manageru z existujících prostředků</span><span class="sxs-lookup"><span data-stu-id="db932-103">Export an Azure Resource Manager template from existing resources</span></span>
<span data-ttu-id="db932-104">V tomto článku se dozvíte, jak tooexport šablony Resource Manageru ze stávajících prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="db932-104">In this article, you learn how tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="db932-105">Můžete použít tento vygenerované šablony toogain lépe porozumět syntaxe šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-105">You can use that generated template toogain a better understanding of template syntax.</span></span>

<span data-ttu-id="db932-106">Existují dva způsoby tooexport šablonu:</span><span class="sxs-lookup"><span data-stu-id="db932-106">There are two ways tooexport a template:</span></span>

* <span data-ttu-id="db932-107">Můžete exportovat hello **skutečné šablonu použít pro nasazení**.</span><span class="sxs-lookup"><span data-stu-id="db932-107">You can export hello **actual template used for deployment**.</span></span> <span data-ttu-id="db932-108">Hello vyexportované šablony zahrnuje všechny hello parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="db932-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="db932-109">Tento přístup je užitečné, pokud jste nasadili prostředky prostřednictvím portálu hello a chcete toosee hello šablony toocreate tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="db932-109">This approach is helpful when you deployed resources through hello portal, and want toosee hello template toocreate those resources.</span></span> <span data-ttu-id="db932-110">Tato šablona je ihned použitelná.</span><span class="sxs-lookup"><span data-stu-id="db932-110">This template is readily usable.</span></span> 
* <span data-ttu-id="db932-111">Můžete exportovat **generované šablonu, která představuje hello aktuální stav skupiny prostředků hello**.</span><span class="sxs-lookup"><span data-stu-id="db932-111">You can export a **generated template that represents hello current state of hello resource group**.</span></span> <span data-ttu-id="db932-112">Exportovaná šablona Hello není založena na všechny šablony, který jste použili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-112">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="db932-113">Místo toho vytvoří šablonu, která je snímek hello skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="db932-113">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="db932-114">Exportovaná šablona Hello má mnoho hodnot pevně a pravděpodobně ne tolik parametrů by obvykle definujete.</span><span class="sxs-lookup"><span data-stu-id="db932-114">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="db932-115">Tento přístup je užitečné, když jste změnili skupiny prostředků hello po nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-115">This approach is useful when you have modified hello resource group after deployment.</span></span> <span data-ttu-id="db932-116">Tato šablona obvykle vyžaduje úpravy, než je možné ji použít.</span><span class="sxs-lookup"><span data-stu-id="db932-116">This template usually requires modifications before it is usable.</span></span>

<span data-ttu-id="db932-117">Toto téma ukazuje obou přístupů prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-117">This topic shows both approaches through hello portal.</span></span>

## <a name="deploy-resources"></a><span data-ttu-id="db932-118">Nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="db932-118">Deploy resources</span></span>
<span data-ttu-id="db932-119">Začněme nasazením tooAzure prostředky, které můžete použít pro export jako šablona.</span><span class="sxs-lookup"><span data-stu-id="db932-119">Let's start by deploying resources tooAzure that you can use for exporting as a template.</span></span> <span data-ttu-id="db932-120">Pokud už máte skupinu prostředků v rámci vašeho předplatného, které chcete tooexport tooa šablony, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="db932-120">If you already have a resource group in your subscription that you want tooexport tooa template, you can skip this section.</span></span> <span data-ttu-id="db932-121">Hello zbývající část tohoto článku předpokládá, že jste nasadili hello webovou aplikaci a SQL database řešení uvedené v této části.</span><span class="sxs-lookup"><span data-stu-id="db932-121">hello remainder of this article assumes you have deployed hello web app and SQL database solution shown in this section.</span></span> <span data-ttu-id="db932-122">Pokud používáte jiné řešení, prostředí může být jen málo liší, ale hello hello kroky tooexport šablony jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="db932-122">If you use a different solution, your experience might be a little different, but hello steps tooexport a template are hello same.</span></span> 

1. <span data-ttu-id="db932-123">V hello [portál Azure](https://portal.azure.com), vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="db932-123">In hello [Azure portal](https://portal.azure.com), select **New**.</span></span>
   
      ![výběr možnosti Nový](./media/resource-manager-export-template/new.png)
2. <span data-ttu-id="db932-125">Vyhledejte **web app + SQL** a vyberte z dostupných možností hello.</span><span class="sxs-lookup"><span data-stu-id="db932-125">Search for **web app + SQL** and select it from hello available options.</span></span>
   
      ![vyhledání řešení Webová aplikace a SQL](./media/resource-manager-export-template/webapp-sql.png)

3. <span data-ttu-id="db932-127">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="db932-127">Select **Create**.</span></span>

      ![výběr možnosti Vytvořit](./media/resource-manager-export-template/create.png)

4. <span data-ttu-id="db932-129">Zadejte hodnoty hello požadované hello webovou aplikaci a databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="db932-129">Provide hello required values for hello web app and SQL database.</span></span> <span data-ttu-id="db932-130">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="db932-130">Select **Create**.</span></span>

      ![zadání hodnot webu a SQL](./media/resource-manager-export-template/provide-web-values.png)

<span data-ttu-id="db932-132">Hello nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="db932-132">hello deployment may take a minute.</span></span> <span data-ttu-id="db932-133">Po dokončení hello nasazení obsahuje vaše předplatné hello řešení.</span><span class="sxs-lookup"><span data-stu-id="db932-133">After hello deployment finishes, your subscription contains hello solution.</span></span>

## <a name="view-template-from-deployment-history"></a><span data-ttu-id="db932-134">Zobrazení šablony z historie nasazení</span><span class="sxs-lookup"><span data-stu-id="db932-134">View template from deployment history</span></span>
1. <span data-ttu-id="db932-135">Přejděte toohello okně skupiny prostředků pro nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="db932-135">Go toohello resource group blade for your new resource group.</span></span> <span data-ttu-id="db932-136">Všimněte si, že hello okno zobrazí hello výsledek posledního nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="db932-136">Notice that hello blade shows hello result of hello last deployment.</span></span> <span data-ttu-id="db932-137">Vyberte tento odkaz.</span><span class="sxs-lookup"><span data-stu-id="db932-137">Select this link.</span></span>
   
      ![okno skupiny prostředků](./media/resource-manager-export-template/select-deployment.png)
2. <span data-ttu-id="db932-139">Zobrazí historii nasazení pro skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-139">You see a history of deployments for hello group.</span></span> <span data-ttu-id="db932-140">Ve vašem případě hello okno pravděpodobně uvádí jenom jedno nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-140">In your case, hello blade probably lists only one deployment.</span></span> <span data-ttu-id="db932-141">Vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="db932-141">Select this deployment.</span></span>
   
     ![poslední nasazení](./media/resource-manager-export-template/select-history.png)
3. <span data-ttu-id="db932-143">Hello zobrazuje souhrn hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-143">hello blade displays a summary of hello deployment.</span></span> <span data-ttu-id="db932-144">Hello souhrn obsahuje stav hello hello nasazení a jeho operací a hello hodnoty, které jste zadali pro parametry.</span><span class="sxs-lookup"><span data-stu-id="db932-144">hello summary includes hello status of hello deployment and its operations and hello values that you provided for parameters.</span></span> <span data-ttu-id="db932-145">toosee hello šablonu, kterou jste použili pro hello nasazení, vyberte **zobrazit šablonu**.</span><span class="sxs-lookup"><span data-stu-id="db932-145">toosee hello template that you used for hello deployment, select **View template**.</span></span>
   
     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/view-template.png)
4. <span data-ttu-id="db932-147">Resource Manager načte následující sedm soubory pro vás hello:</span><span class="sxs-lookup"><span data-stu-id="db932-147">Resource Manager retrieves hello following seven files for you:</span></span>
   
   1. <span data-ttu-id="db932-148">**Šablona** -hello šablonu, která definuje hello infrastrukturu pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="db932-148">**Template** - hello template that defines hello infrastructure for your solution.</span></span> <span data-ttu-id="db932-149">Při vytváření účtu úložiště hello prostřednictvím portálu hello Resource Manager používá šablony toodeploy ho a tuto šablonu uložil pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="db932-149">When you created hello storage account through hello portal, Resource Manager used a template toodeploy it and saved that template for future reference.</span></span>
   2. <span data-ttu-id="db932-150">**Parametry** -soubor s parametry, můžete použít toopass hodnoty během nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-150">**Parameters** - A parameter file that you can use toopass in values during deployment.</span></span> <span data-ttu-id="db932-151">Obsahuje hello hodnoty, které jste zadali při prvním nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="db932-151">It contains hello values that you provided during hello first deployment.</span></span> <span data-ttu-id="db932-152">Při opětovném nasazování šablony hello můžete tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="db932-152">You can change any of these values when you redeploy hello template.</span></span>
   3. <span data-ttu-id="db932-153">**Rozhraní příkazového řádku** -Azure rozhraní příkazového řádku (CLI) souboru skriptu, které můžete použít toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-153">**CLI** - An Azure command-line-interface (CLI) script file that you can use toodeploy hello template.</span></span>
   3. <span data-ttu-id="db932-154">**Rozhraní příkazového řádku 2.0** -Azure rozhraní příkazového řádku (CLI) souboru skriptu, které můžete použít toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-154">**CLI 2.0** - An Azure command-line-interface (CLI) script file that you can use toodeploy hello template.</span></span>
   4. <span data-ttu-id="db932-155">**Prostředí PowerShell** -souboru skriptu prostředí Azure PowerShell, které můžete použít toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-155">**PowerShell** - An Azure PowerShell script file that you can use toodeploy hello template.</span></span>
   5. <span data-ttu-id="db932-156">**Rozhraní .NET** -třídy A rozhraní .NET, které můžete použít toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-156">**.NET** - A .NET class that you can use toodeploy hello template.</span></span>
   6. <span data-ttu-id="db932-157">**Ruby** -A Ruby třídy, které můžete použít toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-157">**Ruby** - A Ruby class that you can use toodeploy hello template.</span></span>
      
      <span data-ttu-id="db932-158">Hello soubory jsou k dispozici prostřednictvím odkazů v okně hello.</span><span class="sxs-lookup"><span data-stu-id="db932-158">hello files are available through links across hello blade.</span></span> <span data-ttu-id="db932-159">Ve výchozím nastavení zobrazí okno hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-159">By default, hello blade displays hello template.</span></span>
      
       ![zobrazení šablony](./media/resource-manager-export-template/see-template.png)
      
<span data-ttu-id="db932-161">Tato šablona je skutečný šablona hello používá toocreate vaší webové aplikace a SQL database.</span><span class="sxs-lookup"><span data-stu-id="db932-161">This template is hello actual template used toocreate your web app and SQL database.</span></span> <span data-ttu-id="db932-162">Všimněte si, že obsahuje parametry, které umožňují tooprovide různé hodnoty během nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-162">Notice it contains parameters that enable you tooprovide different values during deployment.</span></span> <span data-ttu-id="db932-163">toolearn Další informace o struktuře hello šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="db932-163">toolearn more about hello structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

## <a name="export-hello-template-from-resource-group"></a><span data-ttu-id="db932-164">Exportovat šablonu hello ze skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="db932-164">Export hello template from resource group</span></span>
<span data-ttu-id="db932-165">Pokud máte ručně změnit vaše prostředky nebo přidat prostředky do více nasazení, načítání šablonu z historie nasazení hello neodráží hello aktuální stav skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="db932-165">If you have manually changed your resources or added resources in multiple deployments, retrieving a template from hello deployment history does not reflect hello current state of hello resource group.</span></span> <span data-ttu-id="db932-166">Tato část uvádí, jak hello tooexport šablonu, která odráží aktuální stav skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="db932-166">This section shows you how tooexport a template that reflects hello current state of hello resource group.</span></span> 

> [!NOTE]
> <span data-ttu-id="db932-167">Nejde exportovat šablonu pro skupinu prostředků, která má víc než 200 prostředků.</span><span class="sxs-lookup"><span data-stu-id="db932-167">You cannot export a template for a resource group that has more than 200 resources.</span></span>
> 
> 

1. <span data-ttu-id="db932-168">Šablona hello tooview pro skupinu prostředků, vyberte **skriptu pro automatizaci**.</span><span class="sxs-lookup"><span data-stu-id="db932-168">tooview hello template for a resource group, select **Automation script**.</span></span>
   
      ![export skupiny prostředků](./media/resource-manager-export-template/select-automation.png)
   
     <span data-ttu-id="db932-170">Správce prostředků vyhodnocuje hello prostředky ve skupině prostředků hello a generuje šablonu pro tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="db932-170">Resource Manager evaluates hello resources in hello resource group, and generates a template for those resources.</span></span> <span data-ttu-id="db932-171">Ne všechny typy prostředků podporují funkce exportu šablony hello.</span><span class="sxs-lookup"><span data-stu-id="db932-171">Not all resource types support hello export template function.</span></span> <span data-ttu-id="db932-172">Může se zobrazit chyba oznamující, že došlo k potížím s hello export.</span><span class="sxs-lookup"><span data-stu-id="db932-172">You may see an error stating that there is a problem with hello export.</span></span> <span data-ttu-id="db932-173">Zjistíte, jak toohandle ty problémy v hello [opravte problémy při exportu](#fix-export-issues) části.</span><span class="sxs-lookup"><span data-stu-id="db932-173">You learn how toohandle those issues in hello [Fix export issues](#fix-export-issues) section.</span></span>
2. <span data-ttu-id="db932-174">Znovu naleznete v tématu hello šest souborů, které můžete použít tooredeploy hello řešení.</span><span class="sxs-lookup"><span data-stu-id="db932-174">You again see hello six files that you can use tooredeploy hello solution.</span></span> <span data-ttu-id="db932-175">Tato šablona hello čas je však jen málo liší.</span><span class="sxs-lookup"><span data-stu-id="db932-175">However, this time hello template is a little different.</span></span> <span data-ttu-id="db932-176">Všimněte si, že hello vygenerované šablony obsahuje méně parametrů, než hello šablony v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="db932-176">Notice that hello generated template contains fewer parameters than hello template in previous section.</span></span> <span data-ttu-id="db932-177">Navíc mnoho hodnot hello (např. umístění a hodnoty SKU) jsou pevně zakódovaná v této šabloně místo přijetí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="db932-177">Also, many of hello values (like location and SKU values) are hard-coded in this template rather than accepting a parameter value.</span></span> <span data-ttu-id="db932-178">Před opětovným použitím této šablony, můžete chtít tooedit hello šablony toomake lepší použití parametrů.</span><span class="sxs-lookup"><span data-stu-id="db932-178">Before reusing this template, you might want tooedit hello template toomake better use of parameters.</span></span> 
   
3. <span data-ttu-id="db932-179">Máte několik možností pro pokračování toowork s touto šablonou.</span><span class="sxs-lookup"><span data-stu-id="db932-179">You have a couple of options for continuing toowork with this template.</span></span> <span data-ttu-id="db932-180">Můžete stáhnout hello šablonu a pracovat na něm místně JSON editor.</span><span class="sxs-lookup"><span data-stu-id="db932-180">You can either download hello template and work on it locally with a JSON editor.</span></span> <span data-ttu-id="db932-181">Nebo můžete uložit hello šablona tooyour knihovny a na něm pracovat prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-181">Or, you can save hello template tooyour library and work on it through hello portal.</span></span>
   
     <span data-ttu-id="db932-182">Pokud umíte pomocí editoru JSON jako [VS Code](https://code.visualstudio.com/) nebo [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), možná budete chtít stažení šablony hello místně a pomocí tohoto editoru.</span><span class="sxs-lookup"><span data-stu-id="db932-182">If you are comfortable using a JSON editor like [VS Code](https://code.visualstudio.com/) or [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), you might prefer downloading hello template locally and using that editor.</span></span> <span data-ttu-id="db932-183">toowork místně, vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="db932-183">toowork locally, select **Download**.</span></span>
   
      ![stažení šablony](./media/resource-manager-export-template/download-template.png)
   
     <span data-ttu-id="db932-185">Pokud nemáte nastavení v editoru JSON, možná budete chtít úpravy šablony hello prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-185">If you are not set up with a JSON editor, you might prefer editing hello template through hello portal.</span></span> <span data-ttu-id="db932-186">Hello zbývající část tohoto tématu se předpokládá, že jste uložili hello šablona tooyour knihovny portálu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-186">hello remainder of this topic assumes you have saved hello template tooyour library in hello portal.</span></span> <span data-ttu-id="db932-187">Však provedete hello stejná syntaxe změny šablony toohello zda lokálně pomocí JSON editor nebo přes portál hello.</span><span class="sxs-lookup"><span data-stu-id="db932-187">However, you make hello same syntax changes toohello template whether working locally with a JSON editor or through hello portal.</span></span> <span data-ttu-id="db932-188">Vyberte toowork prostřednictvím portálu hello **přidat toolibrary**.</span><span class="sxs-lookup"><span data-stu-id="db932-188">toowork through hello portal, select **Add toolibrary**.</span></span>
   
      ![Přidat toolibrary](./media/resource-manager-export-template/add-to-library.png)
   
     <span data-ttu-id="db932-190">Při přidávání knihovnu toohello šablony, zadejte název a popis šablony hello.</span><span class="sxs-lookup"><span data-stu-id="db932-190">When adding a template toohello library, give hello template a name and description.</span></span> <span data-ttu-id="db932-191">Potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="db932-191">Then, select **Save**.</span></span>
   
     ![nastavení hodnot šablony](./media/resource-manager-export-template/save-library-template.png)
4. <span data-ttu-id="db932-193">Vyberte tooview šablonu uložit v knihovně, **další služby**, typ **šablony** toofilter výsledky, vyberte **šablony**.</span><span class="sxs-lookup"><span data-stu-id="db932-193">tooview a template saved in your library, select **More services**, type **Templates** toofilter results, select **Templates**.</span></span>
   
      ![vyhledání šablon](./media/resource-manager-export-template/find-templates.png)
5. <span data-ttu-id="db932-195">Vyberte šablonu hello hello názvem, který jste uložili.</span><span class="sxs-lookup"><span data-stu-id="db932-195">Select hello template with hello name you saved.</span></span>
   
      ![výběr šablony](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-hello-template"></a><span data-ttu-id="db932-197">Přizpůsobení šablony hello</span><span class="sxs-lookup"><span data-stu-id="db932-197">Customize hello template</span></span>
<span data-ttu-id="db932-198">Hello exportovat šablonu funguje dobře, že pokud chcete, aby toocreate hello stejné webové aplikaci a SQL database pro všechna nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-198">hello exported template works fine if you want toocreate hello same web app and SQL database for every deployment.</span></span> <span data-ttu-id="db932-199">Resource Manager ale nabízí možnosti pro ještě flexibilnější nasazení šablon.</span><span class="sxs-lookup"><span data-stu-id="db932-199">However, Resource Manager provides options so that you can deploy templates with a lot more flexibility.</span></span> <span data-ttu-id="db932-200">Tento článek ukazuje, jak tooadd parametry pro hello databáze jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="db932-200">This article shows you how tooadd parameters for hello database administrator name and password.</span></span> <span data-ttu-id="db932-201">Tento stejný tooadd přístupu můžete použít pro ostatní hodnoty v šabloně hello větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="db932-201">You can use this same approach tooadd more flexibility for other values in hello template.</span></span>

1. <span data-ttu-id="db932-202">toocustomize hello šablony, vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="db932-202">toocustomize hello template, select **Edit**.</span></span>
   
     ![zobrazení šablony](./media/resource-manager-export-template/select-edit.png)
2. <span data-ttu-id="db932-204">Vyberte šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-204">Select hello template.</span></span>
   
     ![Úprava šablony](./media/resource-manager-export-template/select-added-template.png)
3. <span data-ttu-id="db932-206">toobe možné toopass hello hodnoty, můžete chtít toospecify během nasazení, přidejte následující dva parametry toohello hello **parametry** oddílu v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="db932-206">toobe able toopass hello values that you might want toospecify during deployment, add hello following two parameters toohello **parameters** section in hello template:</span></span>

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. <span data-ttu-id="db932-207">toouse hello nové parametry, nahraďte hello SQL server definice v hello **prostředky** části.</span><span class="sxs-lookup"><span data-stu-id="db932-207">toouse hello new parameters, replace hello SQL server definition in hello **resources** section.</span></span> <span data-ttu-id="db932-208">Všimněte si, že **administratorLogin** a **administratorLoginPassword** jsou teď hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="db932-208">Notice that **administratorLogin** and **administratorLoginPassword** now use parameter values.</span></span>

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

6. <span data-ttu-id="db932-209">Vyberte **OK** po dokončení úprav hello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-209">Select **OK** when you are done editing hello template.</span></span>
7. <span data-ttu-id="db932-210">Vyberte **Uložit** toosave hello změny toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-210">Select **Save** toosave hello changes toohello template.</span></span>
   
     ![uložení šablony](./media/resource-manager-export-template/save-template.png)
8. <span data-ttu-id="db932-212">tooredeploy hello aktualizované šablony, vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="db932-212">tooredeploy hello updated template, select **Deploy**.</span></span>
   
     ![nasazení šablony](./media/resource-manager-export-template/redeploy-template.png)
9. <span data-ttu-id="db932-214">Zadejte hodnoty parametrů a vyberte prostředek skupiny toodeploy hello prostředků pro.</span><span class="sxs-lookup"><span data-stu-id="db932-214">Provide parameter values, and select a resource group toodeploy hello resources to.</span></span>


## <a name="fix-export-issues"></a><span data-ttu-id="db932-215">Oprava problémů s exportem</span><span class="sxs-lookup"><span data-stu-id="db932-215">Fix export issues</span></span>
<span data-ttu-id="db932-216">Ne všechny typy prostředků podporují funkce exportu šablony hello.</span><span class="sxs-lookup"><span data-stu-id="db932-216">Not all resource types support hello export template function.</span></span> <span data-ttu-id="db932-217">tooresolve-li tento problém, ručně přidejte chybějící prostředky, které hello zpět do šablony.</span><span class="sxs-lookup"><span data-stu-id="db932-217">tooresolve this issue, manually add hello missing resources back into your template.</span></span> <span data-ttu-id="db932-218">Hello chybová zpráva obsahuje hello typů prostředků, které nelze exportovat.</span><span class="sxs-lookup"><span data-stu-id="db932-218">hello error message includes hello resource types that cannot be exported.</span></span> <span data-ttu-id="db932-219">Vyhledejte tyto typy prostředků v [referenčních informacích k šablonám](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="db932-219">Find that resource type in [Template reference](/azure/templates/).</span></span> <span data-ttu-id="db932-220">Například toomanually přidat bránu virtuální sítě najdete v tématu [odkaz na šablonu Microsoft.Network/virtualNetworkGateways](/azure/templates/microsoft.network/virtualnetworkgateways).</span><span class="sxs-lookup"><span data-stu-id="db932-220">For example, toomanually add a virtual network gateway, see [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways).</span></span>

> [!NOTE]
> <span data-ttu-id="db932-221">K problémům s exportem může dojít pouze při exportu ze skupiny prostředků, ne z historie nasazení.</span><span class="sxs-lookup"><span data-stu-id="db932-221">You only encounter export issues when exporting from a resource group rather than from your deployment history.</span></span> <span data-ttu-id="db932-222">Pokud vaše poslední nasazení přesně představuje hello aktuální stav skupiny prostředků hello, je třeba exportovat šablonu hello z historie nasazení hello místo skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="db932-222">If your last deployment accurately represents hello current state of hello resource group, you should export hello template from hello deployment history rather than from hello resource group.</span></span> <span data-ttu-id="db932-223">Exportujte pouze ze skupiny prostředků po provedení změny toohello skupiny prostředků nejsou definovány v jediné šabloně.</span><span class="sxs-lookup"><span data-stu-id="db932-223">Only export from a resource group when you have made changes toohello resource group that are not defined in a single template.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="db932-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db932-224">Next steps</span></span>
<span data-ttu-id="db932-225">Jste se naučili, jak tooexport šablonu z prostředků, které jste vytvořili v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="db932-225">You have learned how tooexport a template from resources that you created in hello portal.</span></span>

* <span data-ttu-id="db932-226">Šablonu můžete nasadit pomocí těchto možností: [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) nebo [REST API](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="db932-226">You can deploy a template through [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md), or [REST API](resource-group-template-deploy-rest.md).</span></span>
* <span data-ttu-id="db932-227">jak zjistit, tooexport šablonu prostřednictvím Powershellu, toosee [použití Azure Powershellu s Azure Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="db932-227">toosee how tooexport a template through PowerShell, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="db932-228">jak tooexport šablonu prostřednictvím rozhraní příkazového řádku Azure, najdete v části toosee [hello použití Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="db932-228">toosee how tooexport a template through Azure CLI, see [Use hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>

