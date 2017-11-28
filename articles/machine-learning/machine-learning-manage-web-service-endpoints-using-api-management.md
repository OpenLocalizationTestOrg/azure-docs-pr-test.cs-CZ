---
title: "jak toomanage AzureML webové služby pomocí rozhraní API správy aaaLearn | Microsoft Docs"
description: "Příručka znázorňující, jak toomanage AzureML webové služby pomocí rozhraní API správy."
keywords: "strojového učení, rozhraní api management"
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 6e764fbfd71be6cc908a1c8d3d8889969fc651a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-how-toomanage-azureml-web-services-using-api-management"></a><span data-ttu-id="699bc-104">Zjistěte, jak toomanage AzureML webové služby pomocí rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="699bc-104">Learn how toomanage AzureML web services using API Management</span></span>
## <a name="overview"></a><span data-ttu-id="699bc-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="699bc-105">Overview</span></span>
<span data-ttu-id="699bc-106">Tento průvodce vám ukáže, jak tooquickly začněte používat API Management toomanage AzureML webové služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-106">This guide shows you how tooquickly get started using API Management toomanage your AzureML web services.</span></span>

## <a name="what-is-azure-api-management"></a><span data-ttu-id="699bc-107">Co je Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="699bc-107">What is Azure API Management?</span></span>
<span data-ttu-id="699bc-108">Azure API Management je služba Azure, která umožňuje spravovat koncové body REST API definováním přístupu uživatele, omezení využití a řídicí panel monitorování.</span><span class="sxs-lookup"><span data-stu-id="699bc-108">Azure API Management is an Azure service that lets you manage your REST API endpoints by defining user access, usage throttling, and dashboard monitoring.</span></span> <span data-ttu-id="699bc-109">Klikněte na tlačítko [sem](https://azure.microsoft.com/services/api-management/) podrobnosti o službě Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="699bc-109">Click [here](https://azure.microsoft.com/services/api-management/) for details on Azure API Management.</span></span> <span data-ttu-id="699bc-110">Klikněte na tlačítko [sem](../api-management/api-management-get-started.md) informace o tom, jak tooget práce s Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="699bc-110">Click [here](../api-management/api-management-get-started.md) for a guide on how tooget started with Azure API Management.</span></span> <span data-ttu-id="699bc-111">Tento další průvodce, která tato příručka je založena na, popisuje další témata, včetně konfigurace oznámení, cenovou úroveň, zpracování odpovědi, ověřování uživatelů, vytvoření produktů, developer odběry a dashboarding využití.</span><span class="sxs-lookup"><span data-stu-id="699bc-111">This other guide, which this guide is based on, covers more topics, including notification configurations, tier pricing, response handling, user authentication, creating products, developer subscriptions, and usage dashboarding.</span></span>

## <a name="what-is-azureml"></a><span data-ttu-id="699bc-112">Co je AzureML?</span><span class="sxs-lookup"><span data-stu-id="699bc-112">What is AzureML?</span></span>
<span data-ttu-id="699bc-113">AzureML je služba Azure pro machine learning, která vám umožní tooeasily sestavení, nasazení a sdílet pokročilou analýzu řešení.</span><span class="sxs-lookup"><span data-stu-id="699bc-113">AzureML is an Azure service for machine learning that enables you tooeasily build, deploy, and share advanced analytics solutions.</span></span> <span data-ttu-id="699bc-114">Klikněte na tlačítko [sem](https://azure.microsoft.com/services/machine-learning/) podrobnosti o AzureML.</span><span class="sxs-lookup"><span data-stu-id="699bc-114">Click [here](https://azure.microsoft.com/services/machine-learning/) for details on AzureML.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="699bc-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="699bc-115">Prerequisites</span></span>
<span data-ttu-id="699bc-116">toocomplete této příručce, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="699bc-116">toocomplete this guide, you need:</span></span>

* <span data-ttu-id="699bc-117">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="699bc-117">An Azure account.</span></span> <span data-ttu-id="699bc-118">Pokud nemáte účet Azure, klikněte na tlačítko [sem](https://azure.microsoft.com/pricing/free-trial/) podrobnosti o tom toocreate Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="699bc-118">If you don’t have an Azure account, click [here](https://azure.microsoft.com/pricing/free-trial/) for details on how toocreate a free trial account.</span></span>
* <span data-ttu-id="699bc-119">Účet AzureML.</span><span class="sxs-lookup"><span data-stu-id="699bc-119">An AzureML account.</span></span> <span data-ttu-id="699bc-120">Pokud nemáte účet AzureML, klikněte na tlačítko [sem](https://studio.azureml.net/) podrobnosti o tom toocreate Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="699bc-120">If you don’t have an AzureML account, click [here](https://studio.azureml.net/) for details on how toocreate a free trial account.</span></span>
* <span data-ttu-id="699bc-121">Hello prostoru, služby a api_key pro experimentu AzureML nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="699bc-121">hello workspace, service, and api_key for an AzureML experiment deployed as a web service.</span></span> <span data-ttu-id="699bc-122">Klikněte na tlačítko [sem](machine-learning-create-experiment.md) podrobnosti o jak experimentovat toocreate AzureML.</span><span class="sxs-lookup"><span data-stu-id="699bc-122">Click [here](machine-learning-create-experiment.md) for details on how toocreate an AzureML experiment.</span></span> <span data-ttu-id="699bc-123">Klikněte na tlačítko [sem](machine-learning-publish-a-machine-learning-web-service.md) podrobnosti o způsobu toodeploy AzureML experimentovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="699bc-123">Click [here](machine-learning-publish-a-machine-learning-web-service.md) for details on how toodeploy an AzureML experiment as a web service.</span></span> <span data-ttu-id="699bc-124">Příloha A případně obsahuje pokyny, jak toocreate a testovací jednoduché AzureML experiment a nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="699bc-124">Alternately, Appendix A has instructions for how toocreate and test a simple AzureML experiment and deploy it as a web service.</span></span>

## <a name="create-an-api-management-instance"></a><span data-ttu-id="699bc-125">Vytvoření instance služby API Management</span><span class="sxs-lookup"><span data-stu-id="699bc-125">Create an API Management instance</span></span>
<span data-ttu-id="699bc-126">Níže jsou hello kroky pro používání rozhraní API správy toomanage AzureML webovou službu.</span><span class="sxs-lookup"><span data-stu-id="699bc-126">Below are hello steps for using API Management toomanage your AzureML web service.</span></span> <span data-ttu-id="699bc-127">Nejprve vytvořte instanci služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-127">First create a service instance.</span></span> <span data-ttu-id="699bc-128">Přihlaste se toohello [portálu Classic](https://manage.windowsazure.com/) a klikněte na tlačítko **nový** > **aplikační služby** > **API Management**  >  **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="699bc-128">Log in toohello [Classic Portal](https://manage.windowsazure.com/) and click **New** > **App Services** > **API Management** > **Create**.</span></span>

![Vytvoření instance](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

<span data-ttu-id="699bc-130">Zadejte jedinečný **URL**.</span><span class="sxs-lookup"><span data-stu-id="699bc-130">Specify a unique **URL**.</span></span> <span data-ttu-id="699bc-131">Tato příručka používá **demoazureml** – budete potřebovat toochoose něco jiný.</span><span class="sxs-lookup"><span data-stu-id="699bc-131">This guide uses **demoazureml** – you will need toochoose something different.</span></span> <span data-ttu-id="699bc-132">Zvolte hello potřeby **předplatné** a **oblast** pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-132">Choose hello desired **Subscription** and **Region** for your service instance.</span></span> <span data-ttu-id="699bc-133">Po provedení výběru klikněte na tlačítko Další hello.</span><span class="sxs-lookup"><span data-stu-id="699bc-133">After making your selections, click hello next button.</span></span>

![Vytvoření service-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

<span data-ttu-id="699bc-135">Zadejte hodnotu pro hello **název organizace**.</span><span class="sxs-lookup"><span data-stu-id="699bc-135">Specify a value for hello **Organization Name**.</span></span> <span data-ttu-id="699bc-136">Tato příručka používá **demoazureml** – budete potřebovat toochoose něco jiný.</span><span class="sxs-lookup"><span data-stu-id="699bc-136">This guide uses **demoazureml** – you will need toochoose something different.</span></span> <span data-ttu-id="699bc-137">Zadejte e-mailovou adresu v hello **e-mailu správce** pole.</span><span class="sxs-lookup"><span data-stu-id="699bc-137">Enter your email address in hello **administrator e-mail** field.</span></span> <span data-ttu-id="699bc-138">Tato e-mailová adresa se používá pro oznámení z hello systému API Management.</span><span class="sxs-lookup"><span data-stu-id="699bc-138">This email address is used for notifications from hello API Management system.</span></span>

![Vytvoření service-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

<span data-ttu-id="699bc-140">Klikněte na zaškrtávací políčko toocreate hello instanci služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-140">Click hello check box toocreate your service instance.</span></span> <span data-ttu-id="699bc-141">*Zabírají toothirty minut pro nové služby toobe vytvořit*.</span><span class="sxs-lookup"><span data-stu-id="699bc-141">*It takes up toothirty minutes for a new service toobe created*.</span></span>

## <a name="create-hello-api"></a><span data-ttu-id="699bc-142">Vytvoření rozhraní API hello</span><span class="sxs-lookup"><span data-stu-id="699bc-142">Create hello API</span></span>
<span data-ttu-id="699bc-143">Po vytvoření instance služby hello hello dalším krokem je toocreate hello API.</span><span class="sxs-lookup"><span data-stu-id="699bc-143">Once hello service instance is created, hello next step is toocreate hello API.</span></span> <span data-ttu-id="699bc-144">Rozhraní API se skládá ze sady operací, které můžete vyvolat z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="699bc-144">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="699bc-145">Operace rozhraní API jsou směrovány přes proxy server tooexisting webové služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-145">API operations are proxied tooexisting web services.</span></span> <span data-ttu-id="699bc-146">Tento průvodce vytvoří rozhraní API tohoto proxy toohello existující záznamy o prostředku AzureML a BES webové služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-146">This guide creates APIs that proxy toohello existing AzureML RRS and BES web services.</span></span>

<span data-ttu-id="699bc-147">Rozhraní API jsou vytvořena a nakonfigurována hello rozhraní API portálu vydavatele, který je přístupný prostřednictvím portálu Azure Classic hello.</span><span class="sxs-lookup"><span data-stu-id="699bc-147">APIs are created and configured from hello API publisher portal, which is accessed through hello Azure Classic Portal.</span></span> <span data-ttu-id="699bc-148">tooreach hello vydavatele portálu, vyberte instanci služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-148">tooreach hello publisher portal, select your service instance.</span></span>

![Vyberte instanci služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

<span data-ttu-id="699bc-150">Klikněte na tlačítko **spravovat** v hello portálu Azure Classic služby API Management.</span><span class="sxs-lookup"><span data-stu-id="699bc-150">Click **Manage** in hello Azure Classic Portal for your API Management service.</span></span>

![Spravovat službu](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

<span data-ttu-id="699bc-152">Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="699bc-152">Click **APIs** from hello **API Management** menu on hello left, and then click **Add API**.</span></span>

![rozhraní API. správu nabídky](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

<span data-ttu-id="699bc-154">Typ **API ukázkový AzureML** jako hello **název webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="699bc-154">Type **AzureML Demo API** as hello **Web API name**.</span></span> <span data-ttu-id="699bc-155">Typ **https://ussouthcentral.services.azureml.net** jako hello **adresu URL webové služby**.</span><span class="sxs-lookup"><span data-stu-id="699bc-155">Type **https://ussouthcentral.services.azureml.net** as hello **Web service URL**.</span></span> <span data-ttu-id="699bc-156">Typ **azureml ukázku** jako hello **přípona adresy URL webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="699bc-156">Type **azureml-demo** as hello **Web API URL suffix**.</span></span> <span data-ttu-id="699bc-157">Zkontrolujte **HTTPS** jako hello **adresy URL webového rozhraní API** schéma.</span><span class="sxs-lookup"><span data-stu-id="699bc-157">Check **HTTPS** as hello **Web API URL** scheme.</span></span> <span data-ttu-id="699bc-158">Vyberte **Starter** jako **produkty**.</span><span class="sxs-lookup"><span data-stu-id="699bc-158">Select **Starter** as **Products**.</span></span> <span data-ttu-id="699bc-159">Po dokončení klikněte na tlačítko **Uložit** toocreate hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-159">When finished, click **Save** toocreate hello API.</span></span>

![Přidat – nový – rozhraní api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-hello-operations"></a><span data-ttu-id="699bc-161">Přidání operací hello</span><span class="sxs-lookup"><span data-stu-id="699bc-161">Add hello operations</span></span>
<span data-ttu-id="699bc-162">Klikněte na tlačítko **operace přidání** toothis tooadd operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-162">Click **Add operation** tooadd operations toothis API.</span></span>

![Přidání operace](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

<span data-ttu-id="699bc-164">Hello **operaci nového** okno se zobrazí a hello **podpis** ve výchozím nastavení bude vybraná karta.</span><span class="sxs-lookup"><span data-stu-id="699bc-164">hello **New operation** window will be displayed and hello **Signature** tab will be selected by default.</span></span>

## <a name="add-rrs-operation"></a><span data-ttu-id="699bc-165">Záznamy o prostředku operace přidání</span><span class="sxs-lookup"><span data-stu-id="699bc-165">Add RRS Operation</span></span>
<span data-ttu-id="699bc-166">Nejprve vytvořte operace hello AzureML RRS služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-166">First create an operation for hello AzureML RRS service.</span></span> <span data-ttu-id="699bc-167">Vyberte **POST** jako hello **příkaz HTTP**.</span><span class="sxs-lookup"><span data-stu-id="699bc-167">Select **POST** as hello **HTTP verb**.</span></span> <span data-ttu-id="699bc-168">Typ **/services/ /workspaces/ {prostoru} {služby} / execute? api-version = {apiversion} & Podrobnosti = {Podrobnosti}** jako hello **adresa URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="699bc-168">Type **/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}** as hello **URL template**.</span></span> <span data-ttu-id="699bc-169">Typ **RRS provést** jako hello **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="699bc-169">Type **RRS Execute** as hello **Display name**.</span></span>

![přidat záznamy o prostředku operace podpisu](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

<span data-ttu-id="699bc-171">Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="699bc-171">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="699bc-172">Klikněte na tlačítko **Uložit** toosave tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="699bc-172">Click **Save** toosave this operation.</span></span>

![přidat záznamy o prostředku operace odpověď](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a><span data-ttu-id="699bc-174">Přidání BES operací</span><span class="sxs-lookup"><span data-stu-id="699bc-174">Add BES Operations</span></span>
<span data-ttu-id="699bc-175">Snímky obrazovky nejsou zahrnuty pro hello BES operací, jako jsou velmi podobné toothose pro přidání hello RRS operaci.</span><span class="sxs-lookup"><span data-stu-id="699bc-175">Screenshots are not included for hello BES operations as they are very similar toothose for adding hello RRS operation.</span></span>

### <a name="submit-but-not-start-a-batch-execution-job"></a><span data-ttu-id="699bc-176">Odeslání (ale nespustí) úlohy Batch Execution</span><span class="sxs-lookup"><span data-stu-id="699bc-176">Submit (but not start) a Batch Execution job</span></span>
<span data-ttu-id="699bc-177">Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-177">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="699bc-178">Vyberte **POST** pro hello **příkaz HTTP**.</span><span class="sxs-lookup"><span data-stu-id="699bc-178">Select **POST** for hello **HTTP verb**.</span></span> <span data-ttu-id="699bc-179">Typ **/services/ /workspaces/ {prostoru} {služby} / úloh? api-version = {apiversion}** pro hello **adresa URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="699bc-179">Type **/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="699bc-180">Typ **BES odeslání** pro hello **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="699bc-180">Type **BES Submit** for hello **Display name**.</span></span> <span data-ttu-id="699bc-181">Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="699bc-181">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="699bc-182">Klikněte na tlačítko **Uložit** toosave tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="699bc-182">Click **Save** toosave this operation.</span></span>

### <a name="start-a-batch-execution-job"></a><span data-ttu-id="699bc-183">Spuštění úlohy Batch Execution</span><span class="sxs-lookup"><span data-stu-id="699bc-183">Start a Batch Execution job</span></span>
<span data-ttu-id="699bc-184">Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-184">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="699bc-185">Vyberte **POST** pro hello **příkaz HTTP**.</span><span class="sxs-lookup"><span data-stu-id="699bc-185">Select **POST** for hello **HTTP verb**.</span></span> <span data-ttu-id="699bc-186">Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid} / start? api-version = {apiversion}** pro hello **adresa URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="699bc-186">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="699bc-187">Typ **BES spustit** pro hello **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="699bc-187">Type **BES Start** for hello **Display name**.</span></span> <span data-ttu-id="699bc-188">Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="699bc-188">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="699bc-189">Klikněte na tlačítko **Uložit** toosave tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="699bc-189">Click **Save** toosave this operation.</span></span>

### <a name="get-hello-status-or-result-of-a-batch-execution-job"></a><span data-ttu-id="699bc-190">Získat stav hello nebo výsledek úlohy Batch Execution</span><span class="sxs-lookup"><span data-stu-id="699bc-190">Get hello status or result of a Batch Execution job</span></span>
<span data-ttu-id="699bc-191">Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-191">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="699bc-192">Vyberte **získat** pro hello **příkaz HTTP**.</span><span class="sxs-lookup"><span data-stu-id="699bc-192">Select **GET** for hello **HTTP verb**.</span></span> <span data-ttu-id="699bc-193">Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid}? api-version = {apiversion}** pro hello **adresa URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="699bc-193">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="699bc-194">Typ **BES stav** pro hello **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="699bc-194">Type **BES Status** for hello **Display name**.</span></span> <span data-ttu-id="699bc-195">Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="699bc-195">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="699bc-196">Klikněte na tlačítko **Uložit** toosave tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="699bc-196">Click **Save** toosave this operation.</span></span>

### <a name="delete-a-batch-execution-job"></a><span data-ttu-id="699bc-197">Odstranit úlohu Batch Execution</span><span class="sxs-lookup"><span data-stu-id="699bc-197">Delete a Batch Execution job</span></span>
<span data-ttu-id="699bc-198">Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-198">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="699bc-199">Vyberte **odstranit** pro hello **příkaz HTTP**.</span><span class="sxs-lookup"><span data-stu-id="699bc-199">Select **DELETE** for hello **HTTP verb**.</span></span> <span data-ttu-id="699bc-200">Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid}? api-version = {apiversion}** pro hello **adresa URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="699bc-200">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="699bc-201">Typ **BES odstranit** pro hello **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="699bc-201">Type **BES Delete** for hello **Display name**.</span></span> <span data-ttu-id="699bc-202">Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="699bc-202">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="699bc-203">Klikněte na tlačítko **Uložit** toosave tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="699bc-203">Click **Save** toosave this operation.</span></span>

## <a name="call-an-operation-from-hello-developer-portal"></a><span data-ttu-id="699bc-204">Volání operace z hello portál pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="699bc-204">Call an operation from hello Developer Portal</span></span>
<span data-ttu-id="699bc-205">Operace lze volat přímo z portálu pro vývojáře hello, který nabízí pohodlný způsob tooview a otestovat hello operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="699bc-205">Operations can be called directly from hello Developer portal, which provides a convenient way tooview and test hello operations of an API.</span></span> <span data-ttu-id="699bc-206">V tomto kroku průvodce budete volat hello **RRS provést** metoda, která byla přidána toohello **API ukázkový AzureML**.</span><span class="sxs-lookup"><span data-stu-id="699bc-206">In this guide step you will call hello **RRS Execute** method that was added toohello **AzureML Demo API**.</span></span> <span data-ttu-id="699bc-207">Klikněte na tlačítko **portál pro vývojáře** hello nabídce v hello top napravo od hello portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="699bc-207">Click **Developer portal** from hello menu at hello top right of hello Classic Portal.</span></span>

![portál pro vývojáře](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

<span data-ttu-id="699bc-209">Klikněte na tlačítko **rozhraní API** z hello horní nabídce a pak klikněte na tlačítko **API ukázkový AzureML** toosee hello operations k dispozici.</span><span class="sxs-lookup"><span data-stu-id="699bc-209">Click **APIs** from hello top menu, and then click **AzureML Demo API** toosee hello operations available.</span></span>

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

<span data-ttu-id="699bc-211">Vyberte **RRS provést** pro operaci hello.</span><span class="sxs-lookup"><span data-stu-id="699bc-211">Select **RRS Execute** for hello operation.</span></span> <span data-ttu-id="699bc-212">Klikněte na tlačítko **vyzkoušet**.</span><span class="sxs-lookup"><span data-stu-id="699bc-212">Click **Try It**.</span></span>

![Try it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

<span data-ttu-id="699bc-214">Žádost o parametry, zadejte vaše **prostoru**, **služby**, **2.0** pro hello **apiversion**, a **true**pro hello **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="699bc-214">For Request parameters, type your **workspace**,  **service**, **2.0** for hello **apiversion**, and  **true** for hello **details**.</span></span> <span data-ttu-id="699bc-215">Můžete najít váš **prostoru** a **služby** na řídicím panelu hello AzureML webové služby (najdete v části **testování hello webové služby** v příloze A).</span><span class="sxs-lookup"><span data-stu-id="699bc-215">You can find your **workspace** and **service** in hello AzureML web service dashboard (see **Test hello web service** in Appendix A).</span></span>

<span data-ttu-id="699bc-216">Hlavičky žádosti, klikněte na tlačítko **přidat hlavičku** a typ **Content-Type** a **application/json**, pak klikněte na tlačítko **přidat hlavičku** a typ **autorizace** a **nosiče <YOUR AZUREML SERVICE API-KEY>** .</span><span class="sxs-lookup"><span data-stu-id="699bc-216">For Request headers, click **Add header** and type **Content-Type** and **application/json**, then click **Add header** and type **Authorization** and **Bearer <YOUR AZUREML SERVICE API-KEY>**.</span></span> <span data-ttu-id="699bc-217">Můžete najít váš **klíč rozhraní api** na řídicím panelu hello AzureML webové služby (najdete v části **testování hello webové služby** v příloze A).</span><span class="sxs-lookup"><span data-stu-id="699bc-217">You can find your **api key** in hello AzureML web service dashboard (see **Test hello web service** in Appendix A).</span></span>

<span data-ttu-id="699bc-218">Typ **{"Vstupy": {"input1": {"ColumnNames": ["Col2"], "hodnoty": [["Toto je dobrý den"]]}}, "GlobalParameters": {}}** tělo žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="699bc-218">Type **{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}** for hello request body.</span></span>

![rozhraní api azureml demo](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

<span data-ttu-id="699bc-220">Klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="699bc-220">Click **Send**.</span></span>

![Odeslat](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

<span data-ttu-id="699bc-222">Po vyvolání operace portál pro vývojáře hello zobrazí hello **požadovaná adresa URL** z back endové službě hello, hello **stav odpovědi**, hello **hlavičky odpovědi**, a jakýkoli **obsah odpovědi**.</span><span class="sxs-lookup"><span data-stu-id="699bc-222">After an operation is invoked, hello developer portal displays hello **Requested URL** from hello back-end service, hello **Response status**, hello **Response headers**, and any **Response content**.</span></span>

![Stav odpovědi](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a><span data-ttu-id="699bc-224">Příloha A - vytváření a testování jednoduché AzureML webové služby</span><span class="sxs-lookup"><span data-stu-id="699bc-224">Appendix A - Creating and testing a simple AzureML web service</span></span>
### <a name="creating-hello-experiment"></a><span data-ttu-id="699bc-225">Vytvoření experimentu hello</span><span class="sxs-lookup"><span data-stu-id="699bc-225">Creating hello experiment</span></span>
<span data-ttu-id="699bc-226">Níže jsou hello kroky pro vytvoření jednoduchého experimentu AzureML a jeho nasazení jako webové služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-226">Below are hello steps for creating a simple AzureML experiment and deploying it as a web service.</span></span> <span data-ttu-id="699bc-227">jako vstup sloupec libovolný textu Hello trvá webové služby a vrátí sadu funkcí vyjádřena jako celá čísla.</span><span class="sxs-lookup"><span data-stu-id="699bc-227">hello web service takes as input a column of arbitrary text and returns a set of features represented as integers.</span></span> <span data-ttu-id="699bc-228">Například:</span><span class="sxs-lookup"><span data-stu-id="699bc-228">For example:</span></span>

| <span data-ttu-id="699bc-229">Text</span><span class="sxs-lookup"><span data-stu-id="699bc-229">Text</span></span> | <span data-ttu-id="699bc-230">Hash textu</span><span class="sxs-lookup"><span data-stu-id="699bc-230">Hashed Text</span></span> |
| --- | --- |
| <span data-ttu-id="699bc-231">To je dobrý den</span><span class="sxs-lookup"><span data-stu-id="699bc-231">This is a good day</span></span> |<span data-ttu-id="699bc-232">1 1 2 2 0 2 0 1</span><span class="sxs-lookup"><span data-stu-id="699bc-232">1 1 2 2 0 2 0 1</span></span> |

<span data-ttu-id="699bc-233">Nejdřív pomocí prohlížeče podle vaší volby, přejděte na: [https://studio.azureml.net/](https://studio.azureml.net/) a zadejte vaše přihlašovací údaje toolog v.</span><span class="sxs-lookup"><span data-stu-id="699bc-233">First, using a browser of your choice, navigate to: [https://studio.azureml.net/](https://studio.azureml.net/) and enter your credentials toolog in.</span></span> <span data-ttu-id="699bc-234">Dále vytvořte nový prázdný experiment.</span><span class="sxs-lookup"><span data-stu-id="699bc-234">Next, create a new blank experiment.</span></span>

![Hledat experimentu – šablony](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

<span data-ttu-id="699bc-236">Přejmenujte ji příliš**SimpleFeatureHashingExperiment**.</span><span class="sxs-lookup"><span data-stu-id="699bc-236">Rename it too**SimpleFeatureHashingExperiment**.</span></span> <span data-ttu-id="699bc-237">Rozbalte položku **uložit datové sady** a přetáhněte ji **kniha recenze z Amazon** do experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-237">Expand **Saved Datasets** and drag **Book Reviews from Amazon** onto your experiment.</span></span>

![jednoduché – funkce-algoritmu hash experimentu](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

<span data-ttu-id="699bc-239">Rozbalte položku **transformaci dat** a **manipulaci s** a přetáhněte ji **výběr sloupců v datové sadě** do experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-239">Expand **Data Transformation** and **Manipulation** and drag **Select Columns in Dataset** onto your experiment.</span></span> <span data-ttu-id="699bc-240">Připojit **kniha recenze z Amazon** příliš**výběr sloupců v datové sadě**.</span><span class="sxs-lookup"><span data-stu-id="699bc-240">Connect **Book Reviews from Amazon** too**Select Columns in Dataset**.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

<span data-ttu-id="699bc-242">Klikněte na tlačítko **výběr sloupců v datové sadě** a pak klikněte na **spustit selektor sloupců** a vyberte **Col2**.</span><span class="sxs-lookup"><span data-stu-id="699bc-242">Click **Select Columns in Dataset** and then click **Launch column selector** and select **Col2**.</span></span> <span data-ttu-id="699bc-243">Klikněte na tlačítko zaškrtnutí tooapply hello tyto změny.</span><span class="sxs-lookup"><span data-stu-id="699bc-243">Click hello checkmark tooapply these changes.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

<span data-ttu-id="699bc-245">Rozbalte položku **Analýza textu** a přetáhněte ji **Hashování** do experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="699bc-245">Expand **Text Analytics** and drag **Feature Hashing** onto hello experiment.</span></span> <span data-ttu-id="699bc-246">Připojit **výběr sloupců v datové sadě** příliš**Hashování**.</span><span class="sxs-lookup"><span data-stu-id="699bc-246">Connect **Select Columns in Dataset** too**Feature Hashing**.</span></span>

![připojit sloupce projektu](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

<span data-ttu-id="699bc-248">Typ **3** pro hello **algoritmu hash bitsize**.</span><span class="sxs-lookup"><span data-stu-id="699bc-248">Type **3** for hello **Hashing bitsize**.</span></span> <span data-ttu-id="699bc-249">Tím se vytvoří 8 (23) sloupce.</span><span class="sxs-lookup"><span data-stu-id="699bc-249">This will create 8 (23) columns.</span></span>

![použití algoritmu hash bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

<span data-ttu-id="699bc-251">V tomto okamžiku může být vhodné tooclick **spustit** tootest hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-251">At this point, you may want tooclick **Run** tootest hello experiment.</span></span>

![Spustit](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a><span data-ttu-id="699bc-253">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="699bc-253">Create a web service</span></span>
<span data-ttu-id="699bc-254">Teď vytvořte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="699bc-254">Now create a web service.</span></span> <span data-ttu-id="699bc-255">Rozbalte položku **webové služby** a přetáhněte ji **vstup** do experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-255">Expand **Web Service** and drag **Input** onto your experiment.</span></span> <span data-ttu-id="699bc-256">Připojit **vstup** příliš**Hashování**.</span><span class="sxs-lookup"><span data-stu-id="699bc-256">Connect **Input** too**Feature Hashing**.</span></span> <span data-ttu-id="699bc-257">Také přetáhněte **výstup** do experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-257">Also drag **output** onto your experiment.</span></span> <span data-ttu-id="699bc-258">Připojit **výstup** příliš**Hashování**.</span><span class="sxs-lookup"><span data-stu-id="699bc-258">Connect **Output** too**Feature Hashing**.</span></span>

![výstup na--hashování](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

<span data-ttu-id="699bc-260">Klikněte na tlačítko **publikování webové služby**.</span><span class="sxs-lookup"><span data-stu-id="699bc-260">Click **Publish web service**.</span></span>

![publikování – webové služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

<span data-ttu-id="699bc-262">Klikněte na tlačítko **Ano** toopublish hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-262">Click **Yes** toopublish hello experiment.</span></span>

![Ano publikování](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-hello-web-service"></a><span data-ttu-id="699bc-264">Testování hello webové služby</span><span class="sxs-lookup"><span data-stu-id="699bc-264">Test hello web service</span></span>
<span data-ttu-id="699bc-265">Webové služby AzureML se skládá z RSS (požadavků a odpovědí služby) a koncové body BES (dávky spuštění služby).</span><span class="sxs-lookup"><span data-stu-id="699bc-265">An AzureML web service consists of RSS (request/response service) and BES (batch execution service) endpoints.</span></span> <span data-ttu-id="699bc-266">RSS je pro synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="699bc-266">RSS is for synchronous execution.</span></span> <span data-ttu-id="699bc-267">BES je pro provádění asynchronní úlohy.</span><span class="sxs-lookup"><span data-stu-id="699bc-267">BES is for asynchronous job execution.</span></span> <span data-ttu-id="699bc-268">tootest vaší webové služby s hello ukázkový Python zdroj níže, musíte toodownload a hello instalace Azure SDK pro jazyk Python (viz: [jak tooinstall Python](../python-how-to-install.md)).</span><span class="sxs-lookup"><span data-stu-id="699bc-268">tootest your web service with hello sample Python source below, you may need toodownload and install hello Azure SDK for Python (see: [How tooinstall Python](../python-how-to-install.md)).</span></span>

<span data-ttu-id="699bc-269">Budete také potřebovat hello **prostoru**, **služby**, a **api_key** experimentu pro zdroj ukázek hello níže.</span><span class="sxs-lookup"><span data-stu-id="699bc-269">You will also need hello **workspace**, **service**, and **api_key** of your experiment for hello sample source below.</span></span> <span data-ttu-id="699bc-270">Pracovní prostor hello a služby zjistíte kliknutím na možnost **požadavků a odpovědí** nebo **Batch Execution** svého experimentu v řídicím panelu hello webových služeb.</span><span class="sxs-lookup"><span data-stu-id="699bc-270">You can find hello workspace and service by clicking either **Request/Response** or **Batch Execution** for your experiment in hello web service dashboard.</span></span>

![Najít prostoru a service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

<span data-ttu-id="699bc-272">Můžete najít hello **api_key** kliknutím experimentu v řídicím panelu hello webových služeb.</span><span class="sxs-lookup"><span data-stu-id="699bc-272">You can find hello **api_key** by clicking your experiment in hello web service dashboard.</span></span>

![najít klíč rozhraní api](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a><span data-ttu-id="699bc-274">Koncový bod RRS testu</span><span class="sxs-lookup"><span data-stu-id="699bc-274">Test RRS endpoint</span></span>
##### <a name="test-button"></a><span data-ttu-id="699bc-275">Tlačítko Test</span><span class="sxs-lookup"><span data-stu-id="699bc-275">Test button</span></span>
<span data-ttu-id="699bc-276">Koncový bod snadno tootest hello RRS je tooclick **Test** na řídicí panel hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="699bc-276">An easy way tootest hello RRS endpoint is tooclick **Test** on hello web service dashboard.</span></span>

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

<span data-ttu-id="699bc-278">Typ **to je dobrý den** pro **col2**.</span><span class="sxs-lookup"><span data-stu-id="699bc-278">Type **This is a good day** for **col2**.</span></span> <span data-ttu-id="699bc-279">Klikněte na tlačítko zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="699bc-279">Click hello checkmark.</span></span>

![zadávání dat](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

<span data-ttu-id="699bc-281">Zobrazí se něco podobného jako</span><span class="sxs-lookup"><span data-stu-id="699bc-281">You will see something like</span></span>

![Ukázkový výstup](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a><span data-ttu-id="699bc-283">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="699bc-283">Sample Code</span></span>
<span data-ttu-id="699bc-284">Jiný způsob tootest vaše RRS je z vašeho kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="699bc-284">Another way tootest your RRS is from your client code.</span></span> <span data-ttu-id="699bc-285">Pokud kliknete na tlačítko **požadavků a odpovědí** hello řídicí panel a posuňte toohello dole, uvidíte ukázkový kód pro C#, Python a R. Zobrazí se také hello syntaxe hello RRS požadavku, včetně hello požadavek URI, hlavičky a text.</span><span class="sxs-lookup"><span data-stu-id="699bc-285">If you click **Request/response** on hello dashboard and scroll toohello bottom, you will see sample code for C#, Python, and R. You will also see hello syntax of hello RRS request, including hello request URI, headers, and body.</span></span>

<span data-ttu-id="699bc-286">Tato příručka ukazuje příklad Python funkční.</span><span class="sxs-lookup"><span data-stu-id="699bc-286">This guide shows a working Python example.</span></span> <span data-ttu-id="699bc-287">Budete potřebovat toomodify její hello **prostoru**, **služby**, a **api_key** experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-287">You will need toomodify it with hello **workspace**, **service**, and **api_key** of your experiment.</span></span>

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("hello request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a><span data-ttu-id="699bc-288">Koncový bod BES testu</span><span class="sxs-lookup"><span data-stu-id="699bc-288">Test BES endpoint</span></span>
<span data-ttu-id="699bc-289">Klikněte na tlačítko **spuštění dávky** hello řídicí panel a posuňte toohello dole.</span><span class="sxs-lookup"><span data-stu-id="699bc-289">Click **Batch execution** on hello dashboard and scroll toohello bottom.</span></span> <span data-ttu-id="699bc-290">Zobrazí se ukázkový kód pro C#, Python a R. Zobrazí se také hello syntaxe hello BES požadavky toosubmit úlohu, spustit úlohu, získat stav hello nebo výsledků úlohy a odstranit úlohu.</span><span class="sxs-lookup"><span data-stu-id="699bc-290">You will see sample code for C#, Python, and R. You will also see hello syntax of hello BES requests toosubmit a job, start a job, get hello status or results of a job, and delete a job.</span></span>

<span data-ttu-id="699bc-291">Tato příručka ukazuje příklad Python funkční.</span><span class="sxs-lookup"><span data-stu-id="699bc-291">This guide shows a working Python example.</span></span> <span data-ttu-id="699bc-292">Je třeba toomodify její hello **prostoru**, **služby**, a **api_key** experimentu.</span><span class="sxs-lookup"><span data-stu-id="699bc-292">You need toomodify it with hello **workspace**, **service**, and **api_key** of your experiment.</span></span> <span data-ttu-id="699bc-293">Kromě toho bude nutné toomodify hello **název účtu úložiště**, **klíč účtu úložiště**, a **název kontejneru úložiště**.</span><span class="sxs-lookup"><span data-stu-id="699bc-293">Additionally, you need toomodify hello **storage account name**, **storage account key**, and **storage container name**.</span></span> <span data-ttu-id="699bc-294">A konečně, budete potřebovat toomodify hello umístění hello **vstupní soubor** a hello umístění hello **výstupní soubor**.</span><span class="sxs-lookup"><span data-stu-id="699bc-294">Lastly, you will need toomodify hello location of hello **input file** and hello location of hello **output file**.</span></span>

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH hello API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH hello LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH hello LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("hello request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading hello result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written toohello file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("hello results for " + outputName + " are available at hello following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "hello results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading hello input tooblob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded hello input tooblob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting hello job...")
    # submit hello job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove hello enclosing double-quotes
    print("Job ID: " + job_id)
    # start hello job
    print("Starting hello job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking hello job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in hello follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
