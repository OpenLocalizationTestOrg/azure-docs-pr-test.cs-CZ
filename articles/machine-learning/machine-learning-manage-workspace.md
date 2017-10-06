---
title: "aaaManage pracovní prostor Machine Learning | Microsoft Docs"
description: "Spravovat přístup tooAzure Machine Learning pracovní prostory a nasadit a spravovat rozhraní API pro ML webové služby"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 7bd378d82aa46f4340ca974c7dc5a612c089cdca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a><span data-ttu-id="0b1dd-103">Správa pracovního prostoru Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0b1dd-103">Manage an Azure Machine Learning workspace</span></span>

> [!NOTE]
> <span data-ttu-id="0b1dd-104">Informace týkající se správy webové služby hello webové služby Machine Learning portálu najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="0b1dd-104">For information on managing Web services in hello Machine Learning Web Services portal, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span>
> 
> 

<span data-ttu-id="0b1dd-105">Můžete spravovat pracovní prostory Machine Learning v portálu Azure buď hello nebo hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-105">You can manage Machine Learning workspaces in either hello Azure portal or hello Azure classic portal.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-hello-azure-portal"></a><span data-ttu-id="0b1dd-106">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0b1dd-106">Use hello Azure portal</span></span>

<span data-ttu-id="0b1dd-107">toomanage pracovního prostoru v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-107">toomanage a workspace in hello Azure portal:</span></span>

1. <span data-ttu-id="0b1dd-108">Přihlaste se toohello [portál Azure](https://portal.azure.com/) pomocí účtu správce předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-108">Sign in toohello [Azure portal](https://portal.azure.com/) using an Azure subscription administrator account.</span></span>
2. <span data-ttu-id="0b1dd-109">Hello vyhledávacího pole v horní části hello hello stránky, zadejte "počítač pracovní prostory learning" a potom vyberte **Machine Learning pracovních prostorů**.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-109">In hello search box at hello top of hello page, enter "machine learning workspaces" and then select **Machine Learning Workspaces**.</span></span>
3. <span data-ttu-id="0b1dd-110">Klikněte na pracovní prostor hello chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-110">Click hello workspace you want toomanage.</span></span>

<span data-ttu-id="0b1dd-111">Kromě informací o správu toohello standardní prostředku a možnosti, které jsou k dispozici, můžete:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-111">In addition toohello standard resource management information and options available, you can:</span></span>

- <span data-ttu-id="0b1dd-112">Zobrazení **vlastnosti** – Tato stránka zobrazuje informace o hello prostoru a prostředků, a můžete změnit hello předplatné a skupina prostředků, které tento pracovní prostor je propojená s.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-112">View **Properties** - This page displays hello workspace and resource information, and you can change hello subscription and resource group that this workspace is connected with.</span></span>
- <span data-ttu-id="0b1dd-113">**Nové synchronizace klíčů k úložišti** -hello prostoru udržuje účtu úložiště toohello klíče.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-113">**Resync Storage Keys** - hello workspace maintains keys toohello storage account.</span></span> <span data-ttu-id="0b1dd-114">Pokud účet úložiště hello změny klíčů, pak můžete kliknout na **nové synchronizace klíče** toosynchronize hello klíče s hello prostoru.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-114">If hello storage account changes keys, then you can click **Resync keys** toosynchronize hello keys with hello workspace.</span></span>

<span data-ttu-id="0b1dd-115">toomanage hello webové služby přidružené k tomuto pracovnímu prostoru pomocí portálu hello webové služby Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-115">toomanage hello web services associated with this workspace, use hello Machine Learning Web Services portal.</span></span> <span data-ttu-id="0b1dd-116">V tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md) úplné informace.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-116">See [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md) for complete information.</span></span>

> [!NOTE]
> <span data-ttu-id="0b1dd-117">toodeploy spravovat nové webové služby musí být přiřazena Přispěvatel nebo je nasazená role správce na hello předplatné toowhich hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-117">toodeploy or manage New web services you must be assigned a contributor or administrator role on hello subscription toowhich hello web service is deployed.</span></span> <span data-ttu-id="0b1dd-118">Pokud můžete vyzvat jiné uživatele tooa strojového učení pracovní prostor, musíte je přiřadit role Přispěvatel nebo správce tooa v předplatném hello před jejich nasadit nebo spravovat webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-118">If you invite another user tooa machine learning workspace, you must assign them tooa contributor or administrator role on hello subscription before they can deploy or manage web services.</span></span> 
> 
><span data-ttu-id="0b1dd-119">Další informace o nastavení oprávnění přístupu najdete v tématu [zobrazení přiřazení přístupu pro uživatele a skupiny v hello portál Azure – ve verzi Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span><span class="sxs-lookup"><span data-stu-id="0b1dd-119">For more information on setting access permissions, see [View access assignments for users and groups in hello Azure portal - Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span></span>

## <a name="use-hello-azure-classic-portal"></a><span data-ttu-id="0b1dd-120">Hello použijte portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="0b1dd-120">Use hello Azure classic portal</span></span>

<span data-ttu-id="0b1dd-121">Hello portál Azure classic můžete spravovat vaše Machine Learning pracovních prostorů můžete:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-121">Using hello Azure classic portal, you can manage your Machine Learning workspaces to:</span></span>

* <span data-ttu-id="0b1dd-122">Sledování využití prostoru hello</span><span class="sxs-lookup"><span data-stu-id="0b1dd-122">Monitor how hello workspace is being used</span></span>
* <span data-ttu-id="0b1dd-123">Konfigurace pracovního prostoru tooallow hello nebo odepřít přístup</span><span class="sxs-lookup"><span data-stu-id="0b1dd-123">Configure hello workspace tooallow or deny access</span></span>
* <span data-ttu-id="0b1dd-124">Správa webové služby vytvořené v pracovním prostoru hello</span><span class="sxs-lookup"><span data-stu-id="0b1dd-124">Manage Web services created in hello workspace</span></span>
* <span data-ttu-id="0b1dd-125">Odstranit pracovní prostor hello</span><span class="sxs-lookup"><span data-stu-id="0b1dd-125">Delete hello workspace</span></span>

<span data-ttu-id="0b1dd-126">Kromě toho hello karty řídicí panel poskytuje přehled vaší využití prostoru a rychlého přehledu vašich údajů pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-126">In addition, hello dashboard tab provides an overview of your workspace usage and a quick glance of your workspace information.</span></span>  

> [!TIP]
> <span data-ttu-id="0b1dd-127">V Azure Machine Learning Studio, na hello **webové služby** kartě, můžete přidat, aktualizovat nebo odstranit Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-127">In Azure Machine Learning Studio, on hello **WEB SERVICES** tab, you can add, update, or delete a Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="0b1dd-128">toomanage pracovního prostoru v hello portál Azure classic:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-128">toomanage a workspace in hello Azure classic portal:</span></span>

1. <span data-ttu-id="0b1dd-129">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí Microsoft Azure účet – použijte hello účet, který je spojen s hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-129">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) using your Microsoft Azure account - use hello account that's associated with hello Azure subscription.</span></span>
2. <span data-ttu-id="0b1dd-130">V panelu služby Microsoft Azure hello, klikněte na **MACHINE LEARNING**.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-130">In hello Microsoft Azure services panel, click **MACHINE LEARNING**.</span></span>
3. <span data-ttu-id="0b1dd-131">Klikněte na pracovní prostor hello chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-131">Click hello workspace you want toomanage.</span></span>

<span data-ttu-id="0b1dd-132">stránka pracovní prostor Hello má tři karty:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-132">hello workspace page has three tabs:</span></span>

* <span data-ttu-id="0b1dd-133">**Řídicí panel** -umožňuje využití prostoru tooview informace</span><span class="sxs-lookup"><span data-stu-id="0b1dd-133">**DASHBOARD** - Allows you tooview workspace usage and information</span></span>
* <span data-ttu-id="0b1dd-134">**KONFIGURACE** -vám umožní toomanage přístup toohello prostoru</span><span class="sxs-lookup"><span data-stu-id="0b1dd-134">**CONFIGURE** - Allows you toomanage access toohello workspace</span></span>
* <span data-ttu-id="0b1dd-135">**WEBOVÉ služby** -vám umožní toomanage webové služby, které byly publikovány z tohoto pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="0b1dd-135">**WEB SERVICES** - Allows you toomanage Web services that have been published from this workspace</span></span>

### <a name="toomonitor-how-hello-workspace-is-being-used"></a><span data-ttu-id="0b1dd-136">toomonitor využití prostoru hello</span><span class="sxs-lookup"><span data-stu-id="0b1dd-136">toomonitor how hello workspace is being used</span></span>
<span data-ttu-id="0b1dd-137">Klikněte na tlačítko hello **řídicí panel** kartě.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-137">Click hello **DASHBOARD** tab.</span></span>

<span data-ttu-id="0b1dd-138">Z řídicího panelu hello můžete zobrazit celkový použití pracovního prostoru a získat rychlý přehled informací pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-138">From hello dashboard, you can view overall usage of your workspace and get a quick glance of workspace information.</span></span>

* <span data-ttu-id="0b1dd-139">Hello **výpočetní** graf znázorňuje hello výpočetní prostředky používá hello prostoru.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-139">hello **COMPUTE** chart shows hello compute resources being used by hello workspace.</span></span> <span data-ttu-id="0b1dd-140">Můžete změnit toodisplay zobrazení hello relativní nebo absolutní hodnoty a můžete změnit časový rámec hello zobrazí v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-140">You can change hello view toodisplay relative or absolute values, and you can change hello timeframe displayed in hello chart.</span></span>
* <span data-ttu-id="0b1dd-141">**Přehled využití** zobrazí používá hello prostoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-141">**Usage overview** displays Azure storage being used by hello workspace.</span></span>
* <span data-ttu-id="0b1dd-142">**Rychlého přehledu** poskytuje souhrnné informace o pracovním prostoru a užitečné odkazy.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-142">**Quick glance** provides a summary of workspace information and useful links.</span></span>

> [!NOTE]
> <span data-ttu-id="0b1dd-143">Hello **přihlášení tooML Studio** odkaz otevře Machine Learning Studio pomocí hello Account Microsoft jste aktuálně přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-143">hello **Sign-in tooML Studio** link opens Machine Learning Studio using hello Microsoft Account you are currently signed into.</span></span> <span data-ttu-id="0b1dd-144">Hello Account Microsoft používá toosign v toohello Azure classic portálu toocreate pracovního prostoru nemá automaticky tooopen oprávnění tohoto pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-144">hello Microsoft Account you used toosign in toohello Azure classic portal toocreate a workspace does not automatically have permission tooopen that workspace.</span></span> <span data-ttu-id="0b1dd-145">tooopen pracovního prostoru, musíte být přihlášeni toohello Account Microsoft, který byl definován jako vlastník hello hello pracovního prostoru, nebo musíte tooreceive Pozvánka z pracovního prostoru hello vlastníka toojoin hello.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-145">tooopen a workspace, you must be signed in toohello Microsoft Account that was defined as hello owner of hello workspace, or you need tooreceive an invitation from hello owner toojoin hello workspace.</span></span>
> 
> 

### <a name="toogrant-or-suspend-access-for-users"></a><span data-ttu-id="0b1dd-146">toogrant nebo pozastavit přístup pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0b1dd-146">toogrant or suspend access for users</span></span>
<span data-ttu-id="0b1dd-147">Klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-147">Click hello **CONFIGURE** tab.</span></span>

<span data-ttu-id="0b1dd-148">Na kartě Konfigurace hello můžete:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-148">From hello configuration tab you can:</span></span>

* <span data-ttu-id="0b1dd-149">Pozastavení pracovního prostoru Machine Learning toohello přístup kliknutím na tlačítko zakázat.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-149">Suspend access toohello Machine Learning workspace by clicking DENY.</span></span> <span data-ttu-id="0b1dd-150">Uživatelé již nebude možné tooopen hello prostoru v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-150">Users will no longer be able tooopen hello workspace in Machine Learning Studio.</span></span> <span data-ttu-id="0b1dd-151">toorestore přístup, klikněte na tlačítko Povolit.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-151">toorestore access, click ALLOW.</span></span>

<span data-ttu-id="0b1dd-152">toomanage další účty, kteří mají přístup toohello prostoru v nástroji Machine Learning Studio, klikněte na tlačítko **přihlášení tooML Studio** v hello **řídicí panel** karta (viz poznámka předcházející hello ohledně  **Sign-in tooML Studio**).</span><span class="sxs-lookup"><span data-stu-id="0b1dd-152">toomanage additional accounts who have access toohello workspace in Machine Learning Studio, click **Sign-in tooML Studio** in hello **DASHBOARD** tab (see hello preceeding note regarding **Sign-in tooML Studio**).</span></span> <span data-ttu-id="0b1dd-153">Otevře se hello prostoru v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-153">This opens hello workspace in Machine Learning Studio.</span></span> <span data-ttu-id="0b1dd-154">Zde klikněte na tlačítko hello **nastavení** kartu a potom **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-154">From here, click hello **SETTINGS** tab and then **USERS**.</span></span> <span data-ttu-id="0b1dd-155">Můžete kliknout na **POZVAT uživatele více** toogive uživatelé přistupovat toohello prostoru, nebo vyberte uživatele a klikněte na **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-155">You can click **INVITE MORE USERS** toogive users access toohello workspace, or select a user and click **REMOVE**.</span></span>

### <a name="toomanage-web-services-in-this-workspace"></a><span data-ttu-id="0b1dd-156">toomanage webových služeb v tomto pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="0b1dd-156">toomanage web services in this workspace</span></span>
<span data-ttu-id="0b1dd-157">Klikněte na tlačítko hello **webové služby** kartě.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-157">Click hello **WEB SERVICES** tab.</span></span>

<span data-ttu-id="0b1dd-158">Zobrazí se seznam webových služeb publikována z tohoto pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-158">This displays a list of web services published from this workspace.</span></span>
<span data-ttu-id="0b1dd-159">toomanage webové služby, klikněte na název hello v stránku hello seznamu tooopen hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-159">toomanage a web service, click hello name in hello list tooopen hello Web service page.</span></span>

<span data-ttu-id="0b1dd-160">Webové služby může mít jeden nebo více koncové body definované.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-160">A Web service may have one or more endpoints defined.</span></span>

* <span data-ttu-id="0b1dd-161">Můžete definovat další koncové body v přidání toohello "Výchozí" koncový bod.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-161">You can define more endpoints in addition toohello "Default" endpoint.</span></span> <span data-ttu-id="0b1dd-162">tooadd hello koncový bod, klikněte na tlačítko **spravovat koncové body** dolnímu hello hello řídicí panel tooopen hello webové služby Azure Machine Learning portálu.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-162">tooadd hello endpoint, click **Manage Endpoints** at hello bottom of hello dashboard tooopen hello Azure Machine Learning Web Services portal.</span></span>
* <span data-ttu-id="0b1dd-163">toodelete koncový bod (nejde odstranit koncový bod "Výchozí" hello), zaškrtněte políčko hello od začátku hello hello koncový bod řádku a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-163">toodelete an endpoint (you cannot delete hello "Default" endpoint), click hello check box at hello beginning of hello endpoint row, and click **DELETE**.</span></span> <span data-ttu-id="0b1dd-164">Tím se odebere koncový bod hello z hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-164">This removes hello endpoint from hello Web service.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="0b1dd-165">Pokud aplikace používá koncový bod webové služby hello při odstranění hello koncový bod, hello aplikace obdrží hello k chybě při příštím pokusí tooaccess hello služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-165">If an application is using hello web service endpoint when hello endpoint is deleted, hello application will receive an error hello next time it tries tooaccess hello service.</span></span>
  > 
  > 

<span data-ttu-id="0b1dd-166">Klikněte na název hello tooopen koncový bod webové služby je.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-166">Click hello name of a Web service endpoint tooopen it.</span></span> 

<span data-ttu-id="0b1dd-167">Z řídicího panelu hello můžete zobrazit celkové využití webové služby v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-167">From hello dashboard, you can view overall usage of your Web service over a period of time.</span></span> <span data-ttu-id="0b1dd-168">Období tooview hello můžete vybrat z hello období rozevírací nabídce v pravé horní hello hello využití grafů.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-168">You can select hello period tooview from hello Period dropdown menu in hello upper right of hello usage charts.</span></span> <span data-ttu-id="0b1dd-169">řídicí panel Hello zobrazuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-169">hello dashboard shows hello following information:</span></span>

* <span data-ttu-id="0b1dd-170">**Požadavky v čase** zobrazuje graf krok hello počet požadavků, které přes hello vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-170">**Requests Over Time** displays a step graph of hello number of requests over hello selected time period.</span></span> <span data-ttu-id="0b1dd-171">Může pomoct určit, zda dochází k špičky využití.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-171">It can help identify if you are experiencing spikes in usage.</span></span>
* <span data-ttu-id="0b1dd-172">**Požadavky požadavků a odpovědí** zobrazí hello celkový počet požadavků a odpovědí volání hello Služba obdržela přes hello vybrané časové období a kolik z nich se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-172">**Request-Response Requests** displays hello total number of Request-Response calls hello service has received over hello selected time period and how many of them failed.</span></span>
* <span data-ttu-id="0b1dd-173">**Průměrná doba výpočetní požadavků a odpovědí** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-173">**Average Request-Response Compute Time** displays an average of hello time needed tooexecute hello received requests.</span></span>
* <span data-ttu-id="0b1dd-174">**Požadavky služby batch** zobrazí celkový počet hello dávkových žádostí hello služby přijal přes hello vybrané časové období a kolik z nich se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-174">**Batch Requests** displays hello total number of Batch Requests hello service has received over hello selected time period and how many of them failed.</span></span>
* <span data-ttu-id="0b1dd-175">**Průměrná latence úlohy** zobrazí na průměrný čas hello potřeby tooexecute hello přijatých požadavků.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-175">**Average Job Latency** displays an average of hello time needed tooexecute hello received requests.</span></span>
* <span data-ttu-id="0b1dd-176">**Chyby** zobrazí hello agregační počet chyb, k nimž došlo na volání toohello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-176">**Errors** displays hello aggregate number of errors that have occurred on calls toohello web service.</span></span>
* <span data-ttu-id="0b1dd-177">**Služby náklady** zobrazí hello poplatky za fakturační plán hello související se službou hello.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-177">**Services Costs** displays hello charges for hello billing plan associated with hello service.</span></span>

<span data-ttu-id="0b1dd-178">Na stránce konfigurace hello můžete aktualizovat hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0b1dd-178">From hello Configure page, you can update hello following properties:</span></span>

* <span data-ttu-id="0b1dd-179">**Popis** vám umožní tooenter popis hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-179">**Description** allows you tooenter a description for hello Web service.</span></span> <span data-ttu-id="0b1dd-180">Popis je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-180">Description is a required field.</span></span>
* <span data-ttu-id="0b1dd-181">**Protokolování** vám umožní tooenable nebo zakázat protokolování na koncovém bodu hello chyb.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-181">**Logging** allows you tooenable or disable error logging on hello endpoint.</span></span> <span data-ttu-id="0b1dd-182">Další informace o protokolování naleznete v tématu Povolení [protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="0b1dd-182">For more information on Logging, see Enable [logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>
* <span data-ttu-id="0b1dd-183">**Povolit ukázková data** vám umožní tooprovide ukázková data, které můžete použít službu tootest hello požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-183">**Enable Sample data** allows you tooprovide sample data that you can use tootest hello Request-Response service.</span></span> <span data-ttu-id="0b1dd-184">Pokud jste vytvořili hello webové služby v nástroji Machine Learning Studio, hello ukázková data jsou převzaty z dat hello vaší použité tootrain modelu.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-184">If you created hello web service in Machine Learning Studio, hello sample data is taken from hello data your used tootrain your model.</span></span> <span data-ttu-id="0b1dd-185">Pokud jste vytvořili hello služby prostřednictvím kódu programu hello dat je převzat ze hello příklad dat, která jste zadali jako součást balíčku JSON hello.</span><span class="sxs-lookup"><span data-stu-id="0b1dd-185">If you created hello service programmatically, hello data is taken from hello example data you provided as part of hello JSON package.</span></span>

