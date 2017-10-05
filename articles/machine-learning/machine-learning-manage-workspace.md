---
title: "Spravovat pracovní prostor Machine Learning | Microsoft Docs"
description: "Správa přístupu k Azure Machine Learning pracovní prostory a nasadit a spravovat rozhraní API pro ML webové služby"
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
ms.openlocfilehash: 94800f51baf83311c33490cada5f991ff2101da9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a><span data-ttu-id="19395-103">Správa pracovního prostoru Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="19395-103">Manage an Azure Machine Learning workspace</span></span>

> [!NOTE]
> <span data-ttu-id="19395-104">Informace týkající se správy webové služby v portálu webové služby Machine Learning najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="19395-104">For information on managing Web services in the Machine Learning Web Services portal, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span>
> 
> 

<span data-ttu-id="19395-105">Machine Learning pracovní prostory v portálu Azure nebo portálu Azure classic můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="19395-105">You can manage Machine Learning workspaces in either the Azure portal or the Azure classic portal.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a><span data-ttu-id="19395-106">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="19395-106">Use the Azure portal</span></span>

<span data-ttu-id="19395-107">Správa prostoru na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="19395-107">To manage a workspace in the Azure portal:</span></span>

1. <span data-ttu-id="19395-108">Přihlaste se k [portál Azure](https://portal.azure.com/) pomocí účtu správce předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="19395-108">Sign in to the [Azure portal](https://portal.azure.com/) using an Azure subscription administrator account.</span></span>
2. <span data-ttu-id="19395-109">Do vyhledávacího pole v horní části stránky, zadejte "počítač pracovní prostory learning" a potom vyberte **Machine Learning pracovních prostorů**.</span><span class="sxs-lookup"><span data-stu-id="19395-109">In the search box at the top of the page, enter "machine learning workspaces" and then select **Machine Learning Workspaces**.</span></span>
3. <span data-ttu-id="19395-110">Klikněte na pracovní prostor, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="19395-110">Click the workspace you want to manage.</span></span>

<span data-ttu-id="19395-111">Kromě informací o správu standardní prostředku a možnosti, které jsou k dispozici můžete:</span><span class="sxs-lookup"><span data-stu-id="19395-111">In addition to the standard resource management information and options available, you can:</span></span>

- <span data-ttu-id="19395-112">Zobrazení **vlastnosti** – Tato stránka zobrazuje informace o pracovním prostoru a prostředků, a můžete změnit předplatném nebo skupině prostředků, tento pracovní prostor je propojená s.</span><span class="sxs-lookup"><span data-stu-id="19395-112">View **Properties** - This page displays the workspace and resource information, and you can change the subscription and resource group that this workspace is connected with.</span></span>
- <span data-ttu-id="19395-113">**Nové synchronizace klíčů k úložišti** -pracovním prostoru udržuje klíče k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="19395-113">**Resync Storage Keys** - The workspace maintains keys to the storage account.</span></span> <span data-ttu-id="19395-114">Pokud účet úložiště změny klíčů, pak můžete kliknout na **nové synchronizace klíče** synchronizovat klíče s pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="19395-114">If the storage account changes keys, then you can click **Resync keys** to synchronize the keys with the workspace.</span></span>

<span data-ttu-id="19395-115">Ke správě webových služeb přidružený tento pracovní prostor, použití portálu webové služby Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="19395-115">To manage the web services associated with this workspace, use the Machine Learning Web Services portal.</span></span> <span data-ttu-id="19395-116">V tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md) úplné informace.</span><span class="sxs-lookup"><span data-stu-id="19395-116">See [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md) for complete information.</span></span>

> [!NOTE]
> <span data-ttu-id="19395-117">Nasadit nebo spravovat nové webové služby musí mít přiřazenou roli Přispěvatel nebo správce na předplatné, která je nasazena webová služba.</span><span class="sxs-lookup"><span data-stu-id="19395-117">To deploy or manage New web services you must be assigned a contributor or administrator role on the subscription to which the web service is deployed.</span></span> <span data-ttu-id="19395-118">Pokud můžete pozvat jiného uživatele na pracovní prostor machine learning, musíte je přiřadit k roli Přispěvatel nebo správce na předplatné, než můžete nasadit nebo spravovat webové služby.</span><span class="sxs-lookup"><span data-stu-id="19395-118">If you invite another user to a machine learning workspace, you must assign them to a contributor or administrator role on the subscription before they can deploy or manage web services.</span></span> 
> 
><span data-ttu-id="19395-119">Další informace o nastavení oprávnění přístupu najdete v tématu [zobrazení přiřazení přístupu pro uživatele a skupiny na portálu Azure – ve verzi Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span><span class="sxs-lookup"><span data-stu-id="19395-119">For more information on setting access permissions, see [View access assignments for users and groups in the Azure portal - Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span></span>

## <a name="use-the-azure-classic-portal"></a><span data-ttu-id="19395-120">Použití portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="19395-120">Use the Azure classic portal</span></span>

<span data-ttu-id="19395-121">Pomocí portálu Azure classic, můžete spravovat vaše Machine Learning pracovních prostorů můžete:</span><span class="sxs-lookup"><span data-stu-id="19395-121">Using the Azure classic portal, you can manage your Machine Learning workspaces to:</span></span>

* <span data-ttu-id="19395-122">Sledování využití pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="19395-122">Monitor how the workspace is being used</span></span>
* <span data-ttu-id="19395-123">Nakonfigurovat v pracovním prostoru povolí nebo odepře přístup</span><span class="sxs-lookup"><span data-stu-id="19395-123">Configure the workspace to allow or deny access</span></span>
* <span data-ttu-id="19395-124">Správa webové služby vytvořené v pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="19395-124">Manage Web services created in the workspace</span></span>
* <span data-ttu-id="19395-125">Odstranit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="19395-125">Delete the workspace</span></span>

<span data-ttu-id="19395-126">Kromě toho řídicí panel poskytuje přehled vaší využití prostoru a rychlý přehled informací pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="19395-126">In addition, the dashboard tab provides an overview of your workspace usage and a quick glance of your workspace information.</span></span>  

> [!TIP]
> <span data-ttu-id="19395-127">V nástroji Azure Machine Learning Studio na **webové služby** kartě, můžete přidat, aktualizovat nebo odstranit Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="19395-127">In Azure Machine Learning Studio, on the **WEB SERVICES** tab, you can add, update, or delete a Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="19395-128">Správa prostoru na portálu Azure classic:</span><span class="sxs-lookup"><span data-stu-id="19395-128">To manage a workspace in the Azure classic portal:</span></span>

1. <span data-ttu-id="19395-129">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) pomocí Microsoft Azure účet – použijte účet, který je spojen s předplatným služby Azure.</span><span class="sxs-lookup"><span data-stu-id="19395-129">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) using your Microsoft Azure account - use the account that's associated with the Azure subscription.</span></span>
2. <span data-ttu-id="19395-130">Na panelu služby Microsoft Azure, klikněte na tlačítko **MACHINE LEARNING**.</span><span class="sxs-lookup"><span data-stu-id="19395-130">In the Microsoft Azure services panel, click **MACHINE LEARNING**.</span></span>
3. <span data-ttu-id="19395-131">Klikněte na pracovní prostor, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="19395-131">Click the workspace you want to manage.</span></span>

<span data-ttu-id="19395-132">Stránka pracovní prostor obsahuje tři karty:</span><span class="sxs-lookup"><span data-stu-id="19395-132">The workspace page has three tabs:</span></span>

* <span data-ttu-id="19395-133">**Řídicí panel** -umožňuje zobrazení využití prostoru a informace</span><span class="sxs-lookup"><span data-stu-id="19395-133">**DASHBOARD** - Allows you to view workspace usage and information</span></span>
* <span data-ttu-id="19395-134">**KONFIGURACE** -umožňuje spravovat přístup k pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="19395-134">**CONFIGURE** - Allows you to manage access to the workspace</span></span>
* <span data-ttu-id="19395-135">**WEBOVÉ služby** -vám umožní spravovat webové služby, které byly publikovány z tohoto pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="19395-135">**WEB SERVICES** - Allows you to manage Web services that have been published from this workspace</span></span>

### <a name="to-monitor-how-the-workspace-is-being-used"></a><span data-ttu-id="19395-136">Ke sledování, jak je používán pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="19395-136">To monitor how the workspace is being used</span></span>
<span data-ttu-id="19395-137">Klikněte **řídicí panel** kartě.</span><span class="sxs-lookup"><span data-stu-id="19395-137">Click the **DASHBOARD** tab.</span></span>

<span data-ttu-id="19395-138">Na řídicím panelu můžete zobrazit celkový použití pracovního prostoru a získat rychlý přehled informací pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="19395-138">From the dashboard, you can view overall usage of your workspace and get a quick glance of workspace information.</span></span>

* <span data-ttu-id="19395-139">**Výpočetní** graf znázorňuje výpočetní prostředky, které používá pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="19395-139">The **COMPUTE** chart shows the compute resources being used by the workspace.</span></span> <span data-ttu-id="19395-140">Můžete změnit zobrazení relativní nebo absolutní hodnoty a můžete změnit v grafu časový rámec.</span><span class="sxs-lookup"><span data-stu-id="19395-140">You can change the view to display relative or absolute values, and you can change the timeframe displayed in the chart.</span></span>
* <span data-ttu-id="19395-141">**Přehled využití** zobrazí používá pracovním prostoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19395-141">**Usage overview** displays Azure storage being used by the workspace.</span></span>
* <span data-ttu-id="19395-142">**Rychlého přehledu** poskytuje souhrnné informace o pracovním prostoru a užitečné odkazy.</span><span class="sxs-lookup"><span data-stu-id="19395-142">**Quick glance** provides a summary of workspace information and useful links.</span></span>

> [!NOTE]
> <span data-ttu-id="19395-143">**Přihlášení k ML Studio** odkaz otevře Machine Learning Studio pomocí Account Microsoft jste aktuálně přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="19395-143">The **Sign-in to ML Studio** link opens Machine Learning Studio using the Microsoft Account you are currently signed into.</span></span> <span data-ttu-id="19395-144">Account Microsoft, který jste použili pro přihlášení k portálu Azure classic k vytvoření pracovního prostoru automaticky nemá oprávnění k otevření tohoto pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="19395-144">The Microsoft Account you used to sign in to the Azure classic portal to create a workspace does not automatically have permission to open that workspace.</span></span> <span data-ttu-id="19395-145">Otevřete pracovní prostor, musíte být přihlášeni k Account Microsoft, která byla definována jako vlastník pracovního prostoru, nebo je potřeba přijmout pozvánku od vlastníka pro připojení k pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="19395-145">To open a workspace, you must be signed in to the Microsoft Account that was defined as the owner of the workspace, or you need to receive an invitation from the owner to join the workspace.</span></span>
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a><span data-ttu-id="19395-146">Udělení nebo pozastavit přístup pro uživatele</span><span class="sxs-lookup"><span data-stu-id="19395-146">To grant or suspend access for users</span></span>
<span data-ttu-id="19395-147">Klikněte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="19395-147">Click the **CONFIGURE** tab.</span></span>

<span data-ttu-id="19395-148">Na kartě konfigurace můžete:</span><span class="sxs-lookup"><span data-stu-id="19395-148">From the configuration tab you can:</span></span>

* <span data-ttu-id="19395-149">Přístup do pracovního prostoru Machine Learning pozastavte kliknutím na tlačítko zakázat.</span><span class="sxs-lookup"><span data-stu-id="19395-149">Suspend access to the Machine Learning workspace by clicking DENY.</span></span> <span data-ttu-id="19395-150">Uživatelé už nebude moct v nástroji Machine Learning Studio otevřete pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="19395-150">Users will no longer be able to open the workspace in Machine Learning Studio.</span></span> <span data-ttu-id="19395-151">Chcete-li obnovit přístup, klikněte na tlačítko Povolit.</span><span class="sxs-lookup"><span data-stu-id="19395-151">To restore access, click ALLOW.</span></span>

<span data-ttu-id="19395-152">Chcete-li spravovat další účty, kteří mají přístup do pracovního prostoru v nástroji Machine Learning Studio, klikněte na tlačítko **přihlášení k ML Studio** v **řídicí panel** karta (viz poznámka předcházející ohledně **přihlášení k ML Studio**).</span><span class="sxs-lookup"><span data-stu-id="19395-152">To manage additional accounts who have access to the workspace in Machine Learning Studio, click **Sign-in to ML Studio** in the **DASHBOARD** tab (see the preceeding note regarding **Sign-in to ML Studio**).</span></span> <span data-ttu-id="19395-153">Otevře se v pracovním prostoru v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="19395-153">This opens the workspace in Machine Learning Studio.</span></span> <span data-ttu-id="19395-154">Zde klikněte na tlačítko **nastavení** kartu a potom **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="19395-154">From here, click the **SETTINGS** tab and then **USERS**.</span></span> <span data-ttu-id="19395-155">Můžete kliknout na **POZVAT uživatele více** uživatelům přístup k pracovním prostoru, nebo vyberte uživatele a klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="19395-155">You can click **INVITE MORE USERS** to give users access to the workspace, or select a user and click **REMOVE**.</span></span>

### <a name="to-manage-web-services-in-this-workspace"></a><span data-ttu-id="19395-156">Ke správě webových služeb v tomto pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="19395-156">To manage web services in this workspace</span></span>
<span data-ttu-id="19395-157">Klikněte **webové služby** kartě.</span><span class="sxs-lookup"><span data-stu-id="19395-157">Click the **WEB SERVICES** tab.</span></span>

<span data-ttu-id="19395-158">Zobrazí se seznam webových služeb publikována z tohoto pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="19395-158">This displays a list of web services published from this workspace.</span></span>
<span data-ttu-id="19395-159">Chcete-li spravovat webové služby, klikněte na název v seznamu a otevřete stránku webové služby.</span><span class="sxs-lookup"><span data-stu-id="19395-159">To manage a web service, click the name in the list to open the Web service page.</span></span>

<span data-ttu-id="19395-160">Webové služby může mít jeden nebo více koncové body definované.</span><span class="sxs-lookup"><span data-stu-id="19395-160">A Web service may have one or more endpoints defined.</span></span>

* <span data-ttu-id="19395-161">Můžete definovat další koncové body kromě "Výchozí" koncový bod.</span><span class="sxs-lookup"><span data-stu-id="19395-161">You can define more endpoints in addition to the "Default" endpoint.</span></span> <span data-ttu-id="19395-162">Chcete-li přidat koncový bod, klikněte na tlačítko **spravovat koncové body** v dolní části řídicího panelu webové služby Azure Machine Learning portál otevřít.</span><span class="sxs-lookup"><span data-stu-id="19395-162">To add the endpoint, click **Manage Endpoints** at the bottom of the dashboard to open the Azure Machine Learning Web Services portal.</span></span>
* <span data-ttu-id="19395-163">Chcete-li odstranit koncový bod (nejde odstranit koncový bod "Výchozí"), klikněte na zaškrtávací políčko na začátku řádku koncový bod a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="19395-163">To delete an endpoint (you cannot delete the "Default" endpoint), click the check box at the beginning of the endpoint row, and click **DELETE**.</span></span> <span data-ttu-id="19395-164">Tím se odebere koncový bod z webové služby.</span><span class="sxs-lookup"><span data-stu-id="19395-164">This removes the endpoint from the Web service.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="19395-165">Pokud aplikace používá koncový bod webové služby se odstraní koncový bod, aplikace dojde k chybě při příštím pokusu o přístup ke službě.</span><span class="sxs-lookup"><span data-stu-id="19395-165">If an application is using the web service endpoint when the endpoint is deleted, the application will receive an error the next time it tries to access the service.</span></span>
  > 
  > 

<span data-ttu-id="19395-166">Klikněte na název koncový bod webové služby a ten se otevře.</span><span class="sxs-lookup"><span data-stu-id="19395-166">Click the name of a Web service endpoint to open it.</span></span> 

<span data-ttu-id="19395-167">Na řídicím panelu můžete zobrazit celkové využití webové služby v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="19395-167">From the dashboard, you can view overall usage of your Web service over a period of time.</span></span> <span data-ttu-id="19395-168">Můžete vybrat období zobrazení období rozevírací nabídce v pravé horní části grafy využití.</span><span class="sxs-lookup"><span data-stu-id="19395-168">You can select the period to view from the Period dropdown menu in the upper right of the usage charts.</span></span> <span data-ttu-id="19395-169">Řídicí panel zobrazuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="19395-169">The dashboard shows the following information:</span></span>

* <span data-ttu-id="19395-170">**Požadavky v čase** zobrazí graf krok počet požadavků za vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="19395-170">**Requests Over Time** displays a step graph of the number of requests over the selected time period.</span></span> <span data-ttu-id="19395-171">Může pomoct určit, zda dochází k špičky využití.</span><span class="sxs-lookup"><span data-stu-id="19395-171">It can help identify if you are experiencing spikes in usage.</span></span>
* <span data-ttu-id="19395-172">**Požadavky požadavků a odpovědí** zobrazí celkový počet požadavků a odpovědí volání, které Služba obdržela přes zvolené časové období a kolik z nich se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="19395-172">**Request-Response Requests** displays the total number of Request-Response calls the service has received over the selected time period and how many of them failed.</span></span>
* <span data-ttu-id="19395-173">**Průměrná doba výpočetní požadavků a odpovědí** Průměrná doba potřebná k provedení přijatých požadavků.</span><span class="sxs-lookup"><span data-stu-id="19395-173">**Average Request-Response Compute Time** displays an average of the time needed to execute the received requests.</span></span>
* <span data-ttu-id="19395-174">**Požadavky služby batch** zobrazí celkový počet dávkových žádostí, které Služba obdržela přes zvolené časové období a kolik z nich se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="19395-174">**Batch Requests** displays the total number of Batch Requests the service has received over the selected time period and how many of them failed.</span></span>
* <span data-ttu-id="19395-175">**Průměrná latence úlohy** Průměrná doba potřebná k provedení přijatých požadavků.</span><span class="sxs-lookup"><span data-stu-id="19395-175">**Average Job Latency** displays an average of the time needed to execute the received requests.</span></span>
* <span data-ttu-id="19395-176">**Chyby** zobrazí souhrnný počet chyb, k nimž došlo v volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="19395-176">**Errors** displays the aggregate number of errors that have occurred on calls to the web service.</span></span>
* <span data-ttu-id="19395-177">**Služby náklady** zobrazí poplatky za fakturační plán spojené s touto službou.</span><span class="sxs-lookup"><span data-stu-id="19395-177">**Services Costs** displays the charges for the billing plan associated with the service.</span></span>

<span data-ttu-id="19395-178">Na stránce konfigurace můžete aktualizovat následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="19395-178">From the Configure page, you can update the following properties:</span></span>

* <span data-ttu-id="19395-179">**Popis** můžete zadat popis pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="19395-179">**Description** allows you to enter a description for the Web service.</span></span> <span data-ttu-id="19395-180">Popis je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="19395-180">Description is a required field.</span></span>
* <span data-ttu-id="19395-181">**Protokolování** umožňuje povolit nebo zakázat protokolování na koncovém bodu.</span><span class="sxs-lookup"><span data-stu-id="19395-181">**Logging** allows you to enable or disable error logging on the endpoint.</span></span> <span data-ttu-id="19395-182">Další informace o protokolování naleznete v tématu Povolení [protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="19395-182">For more information on Logging, see Enable [logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>
* <span data-ttu-id="19395-183">**Povolit ukázková data** umožňuje poskytovat ukázková data, která můžete použít k testování služby požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="19395-183">**Enable Sample data** allows you to provide sample data that you can use to test the Request-Response service.</span></span> <span data-ttu-id="19395-184">Pokud jste vytvořili webovou službu v nástroji Machine Learning Studio, ukázkových dat je převzat ze data vaší použité k natrénování modelu.</span><span class="sxs-lookup"><span data-stu-id="19395-184">If you created the web service in Machine Learning Studio, the sample data is taken from the data your used to train your model.</span></span> <span data-ttu-id="19395-185">Pokud jste vytvořili službu prostřednictvím kódu programu, data je převzat ze příklad dat, které jste zadali jako součást balíčku JSON.</span><span class="sxs-lookup"><span data-stu-id="19395-185">If you created the service programmatically, the data is taken from the example data you provided as part of the JSON package.</span></span>

