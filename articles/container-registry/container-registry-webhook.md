---
title: aaaAzure kontejneru registru webhooky | Microsoft Docs
description: Kontejner Azure registru webhooky
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: Docker, kontejnery, ACR
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.openlocfilehash: adc2afec486007e2d54cd689e6f7ef8b1098db06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a><span data-ttu-id="275a5-104">Pomocí webhooků registru kontejner Azure – portál Azure</span><span class="sxs-lookup"><span data-stu-id="275a5-104">Using Azure Container Registry webhooks - Azure portal</span></span>

<span data-ttu-id="275a5-105">Azure kontejneru registru uchovává a spravuje privátní Docker kontejner imagí, podobné toohello způsob úložiště Docker Hub ukládá veřejné imagí Dockeru.</span><span class="sxs-lookup"><span data-stu-id="275a5-105">An Azure container registry stores and manages private Docker container images, similar toohello way Docker Hub stores public Docker images.</span></span> <span data-ttu-id="275a5-106">Použijete webhooky tootrigger událostí v případě, že některé akce provést v jednom z vašeho úložiště registru.</span><span class="sxs-lookup"><span data-stu-id="275a5-106">You use webhooks tootrigger events when certain actions take place in one of your registry repositories.</span></span> <span data-ttu-id="275a5-107">Webhooky může reagovat tooevents na úrovni registru hello nebo může být obor dolů značky tooa konkrétní úložiště.</span><span class="sxs-lookup"><span data-stu-id="275a5-107">Webhooks can respond tooevents at hello registry level or they can be scoped down tooa specific repository tag.</span></span> 

<span data-ttu-id="275a5-108">Pozadí a koncepty, najdete v části hello [přehled](./container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="275a5-108">For more background and concepts, see hello [overview](./container-registry-intro.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="275a5-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="275a5-109">Prerequisites</span></span> 

- <span data-ttu-id="275a5-110">Kontejner Azure spravované registru - vytvořit spravované kontejneru registru ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="275a5-110">Azure container managed registry - Create a managed container registry in your Azure subscription.</span></span> <span data-ttu-id="275a5-111">Například použijte hello portál Azure nebo Azure CLI 2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="275a5-111">For example, use hello Azure portal or hello Azure CLI 2.0.</span></span> 
- <span data-ttu-id="275a5-112">Docker rozhraní příkazového řádku - tooset do místního počítače jako Docker hostitele a přístup hello příkazového řádku Dockeru příkazy, nainstalujte modul Docker.</span><span class="sxs-lookup"><span data-stu-id="275a5-112">Docker CLI - tooset up your local computer as a Docker host and access hello Docker CLI commands, install Docker Engine.</span></span> 

## <a name="create-webhook-azure-portal"></a><span data-ttu-id="275a5-113">Vytvoření webhooku portálu Azure</span><span class="sxs-lookup"><span data-stu-id="275a5-113">Create webhook Azure portal</span></span>

1. <span data-ttu-id="275a5-114">Přihlaste se toohello portál Azure a přejděte toohello registru, ve kterém chcete toocreate webhooky.</span><span class="sxs-lookup"><span data-stu-id="275a5-114">Log in toohello Azure portal and navigate toohello registry in which you want toocreate webhooks.</span></span> 

2. <span data-ttu-id="275a5-115">V okně kontejner hello vyberte kartu "Webhooky" hello.</span><span class="sxs-lookup"><span data-stu-id="275a5-115">In hello container blade, select hello "Webhooks" tab.</span></span> 

3. <span data-ttu-id="275a5-116">Vyberte z panelu nástrojů okna webhooku hello "Přidat".</span><span class="sxs-lookup"><span data-stu-id="275a5-116">Select "Add" from hello webhook blade toolbar.</span></span> 

4. <span data-ttu-id="275a5-117">Dokončení hello *vytvoření Webhooku* formulář hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="275a5-117">Complete hello *Create Webhook* form with hello following information:</span></span>

| <span data-ttu-id="275a5-118">Hodnota</span><span class="sxs-lookup"><span data-stu-id="275a5-118">Value</span></span> | <span data-ttu-id="275a5-119">Popis</span><span class="sxs-lookup"><span data-stu-id="275a5-119">Description</span></span> |
|---|---|
| <span data-ttu-id="275a5-120">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="275a5-120">Name</span></span> | <span data-ttu-id="275a5-121">Název Hello chcete toogive toohello webhooku.</span><span class="sxs-lookup"><span data-stu-id="275a5-121">hello name you want toogive toohello webhook.</span></span> <span data-ttu-id="275a5-122">Může obsahovat pouze malá písmena a čísla a 5 – 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="275a5-122">It can only contain lowercase letters and numbers and between 5-50 characters.</span></span> |
| <span data-ttu-id="275a5-123">URI služby</span><span class="sxs-lookup"><span data-stu-id="275a5-123">Service URI</span></span> | <span data-ttu-id="275a5-124">Hello identifikátor URI, kde by měli hello webhooku poslat oznámení POST.</span><span class="sxs-lookup"><span data-stu-id="275a5-124">hello URI where hello webhook should send POST notifications.</span></span> |
| <span data-ttu-id="275a5-125">Vlastní hlavičky</span><span class="sxs-lookup"><span data-stu-id="275a5-125">Custom headers</span></span> | <span data-ttu-id="275a5-126">Hlavičky chcete toopass společně s hello požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="275a5-126">Headers you want toopass along with hello POST request.</span></span> <span data-ttu-id="275a5-127">Musí být v "klíč: hodnota" formátu.</span><span class="sxs-lookup"><span data-stu-id="275a5-127">They should be in "key: value" format.</span></span> |
| <span data-ttu-id="275a5-128">Akce aktivace</span><span class="sxs-lookup"><span data-stu-id="275a5-128">Trigger actions</span></span> | <span data-ttu-id="275a5-129">Akce, které aktivují webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="275a5-129">Actions that trigger hello webhook.</span></span> <span data-ttu-id="275a5-130">Aktuálně webhooky se aktivuje nabízené nebo odstranit image tooan akce.</span><span class="sxs-lookup"><span data-stu-id="275a5-130">Currently webhooks can be triggered by push and/or delete actions tooan image.</span></span> |
| <span data-ttu-id="275a5-131">Status</span><span class="sxs-lookup"><span data-stu-id="275a5-131">Status</span></span> | <span data-ttu-id="275a5-132">Stav Hello hello webhooku po jejím vytvoření.</span><span class="sxs-lookup"><span data-stu-id="275a5-132">hello status for hello webhook after it's created.</span></span> <span data-ttu-id="275a5-133">Ve výchozím nastavení je povolená.</span><span class="sxs-lookup"><span data-stu-id="275a5-133">It's enabled by default.</span></span> |
| <span data-ttu-id="275a5-134">Rozsah</span><span class="sxs-lookup"><span data-stu-id="275a5-134">Scope</span></span> | <span data-ttu-id="275a5-135">Hello rozsahu, na které hello webhooku funguje.</span><span class="sxs-lookup"><span data-stu-id="275a5-135">hello scope at which hello webhook works.</span></span> <span data-ttu-id="275a5-136">Ve výchozím nastavení je hello obor pro všechny události v registru hello.</span><span class="sxs-lookup"><span data-stu-id="275a5-136">By default hello scope is for all events in hello registry.</span></span> <span data-ttu-id="275a5-137">Může se zadat pro úložiště nebo značku ve formátu hello "úložiště: značka".</span><span class="sxs-lookup"><span data-stu-id="275a5-137">It can be specified for a repository or a tag by using hello format "repository: tag".</span></span> |

<span data-ttu-id="275a5-138">Příklad webhooku formuláře:</span><span class="sxs-lookup"><span data-stu-id="275a5-138">Example webhook form:</span></span>

![ORCHESTRÁTORU DC/OS UŽIVATELSKÉHO ROZHRANÍ](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a><span data-ttu-id="275a5-140">Vytvoření webhooku rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="275a5-140">Create webhook Azure CLI</span></span>

<span data-ttu-id="275a5-141">hello toocreate webhooku pomocí rozhraní příkazového řádku Azure, použijte hello [vytvoření webhooku acr az](/cli/azure/acr/webhook#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="275a5-141">toocreate a webhook using hello Azure CLI, use hello [az acr webhook create](/cli/azure/acr/webhook#create) command.</span></span>

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a><span data-ttu-id="275a5-142">Test webhooku</span><span class="sxs-lookup"><span data-stu-id="275a5-142">Test webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="275a5-143">portál Azure</span><span class="sxs-lookup"><span data-stu-id="275a5-143">Azure portal</span></span>

<span data-ttu-id="275a5-144">Předchozí toousing hello webhooku na kontejneru bitovou kopii nabízené a odstranění akcí, může být testována pomocí hello **Ping** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="275a5-144">Prior toousing hello webhook on container image push and delete actions, it can be tested using hello **Ping** button.</span></span> <span data-ttu-id="275a5-145">Pokud se použije, odešle hello Ping že toohello žádost o obecný post zadaný koncový bod a protokoly hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="275a5-145">When used, hello Ping sends a generic post request toohello specified endpoint and logs hello response.</span></span> <span data-ttu-id="275a5-146">To je užitečné tooverify, který hello webhooku byla nastavena správně.</span><span class="sxs-lookup"><span data-stu-id="275a5-146">This is helpful tooverify that hello webhook has been set up correctly.</span></span>

1. <span data-ttu-id="275a5-147">Vyberte, chcete tootest webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="275a5-147">Select hello webhook you want tootest.</span></span> 
2. <span data-ttu-id="275a5-148">V horním panelu nástrojů hello vybráním akce "Příkazem ping otestovat" hello.</span><span class="sxs-lookup"><span data-stu-id="275a5-148">In hello top toolbar, select hello "Ping" action.</span></span> 
3. <span data-ttu-id="275a5-149">Zkontrolujte hello žádostí a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="275a5-149">Check hello request and response.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="275a5-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="275a5-150">Azure CLI</span></span>

<span data-ttu-id="275a5-151">tootest webhook ACR s hello rozhraní příkazového řádku Azure, použijte hello [az acr webhooku ping](/cli/azure/acr/webhook#ping) příkaz.</span><span class="sxs-lookup"><span data-stu-id="275a5-151">tootest an ACR webhook with hello Azure CLI, use hello [az acr webhook ping](/cli/azure/acr/webhook#ping) command.</span></span>

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

<span data-ttu-id="275a5-152">výsledky hello toosee, použijte hello [az acr webhooku seznam události](/cli/azure/acr/webhook#list-events) příkaz.</span><span class="sxs-lookup"><span data-stu-id="275a5-152">toosee hello results, use hello [az acr webhook list-events](/cli/azure/acr/webhook#list-events) command.</span></span> 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a><span data-ttu-id="275a5-153">Odstranit webhook</span><span class="sxs-lookup"><span data-stu-id="275a5-153">Delete webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="275a5-154">portál Azure</span><span class="sxs-lookup"><span data-stu-id="275a5-154">Azure portal</span></span>

<span data-ttu-id="275a5-155">Každý webhooku se dá odstranit výběrem hello webhooku a potom na tlačítko Odstranit hello na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="275a5-155">Each webhook can be deleted by selecting hello webhook and then hello delete button on hello Azure portal.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="275a5-156">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="275a5-156">Azure CLI</span></span>

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```