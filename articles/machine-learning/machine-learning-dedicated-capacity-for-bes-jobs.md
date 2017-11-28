---
title: "Vyhrazené kapacity pro Machine Learning dávky spuštění služby úlohy | Microsoft Docs"
description: "Přehled služby Azure Batch pro Machine Learning úlohy."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3879eb3d0c6fa9d74fff01b07f5c07d3991dfbbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a><span data-ttu-id="0c3dc-103">Služba Azure Batch pro Machine Learning úlohy</span><span class="sxs-lookup"><span data-stu-id="0c3dc-103">Azure Batch service for Machine Learning jobs</span></span>

<span data-ttu-id="0c3dc-104">Zpracování Machine Learning fondu Batch poskytuje spravované zákazníkem škálování pro službu Azure Machine Learning dávky spuštění.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-104">Machine Learning Batch Pool processing provides customer-managed scale for the Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="0c3dc-105">Classic dávkové zpracování pro strojové učení probíhá v prostředí více klientů, které omezuje počet souběžných úloh, které vám mohou odesílat a úlohy se zařadí do fronty na základě objektů first in first out.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-105">Classic batch processing for machine learning takes place in a multi-tenant environment, which limits the number of concurrent jobs you can submit, and jobs are queued on a first-in-first-out basis.</span></span> <span data-ttu-id="0c3dc-106">Tato nejistoty znamená, že nemůžete vědět, když se spustí úlohu.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-106">This uncertainty means that you can't accurately predict when your job will run.</span></span>

<span data-ttu-id="0c3dc-107">Zpracování fondu batch můžete vytvářet fondy, na kterých můžete odeslat úlohy batch.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-107">Batch Pool processing allows you to create pools on which you can submit batch jobs.</span></span> <span data-ttu-id="0c3dc-108">Velikost fondu a do které fondu je úloha odeslána.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-108">You control the size of the pool, and to which pool the job is submitted.</span></span> <span data-ttu-id="0c3dc-109">Vaše BES úloha spouští ve vlastním prostoru zpracování poskytnutí zpracování předvídatelný výkon a schopnost vytvářet fondy zdrojů, které odpovídají zpracování zatížení, která odešlete.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-109">Your BES job runs in its own processing space providing predictable processing performance and the ability to create resource pools that correspond to the processing load that you submit.</span></span>

## <a name="how-to-use-batch-pool-processing"></a><span data-ttu-id="0c3dc-110">Jak používat zpracování fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="0c3dc-110">How to use Batch Pool processing</span></span>

<span data-ttu-id="0c3dc-111">Konfigurace zpracování fondu Batch není aktuálně k dispozici prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-111">Configuration of Batch Pool Processing is not currently available through the Azure portal.</span></span> <span data-ttu-id="0c3dc-112">Chcete-li použít zpracování fondu Batch, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0c3dc-112">To use Batch Pool processing, you must:</span></span>

-   <span data-ttu-id="0c3dc-113">Volání šablon stylů CSS k vytvoření účtu fondu Batch a získat adresu URL služby fondu a autorizační klíč</span><span class="sxs-lookup"><span data-stu-id="0c3dc-113">Call CSS to create a Batch Pool Account and obtain a Pool Service URL and an authorization key</span></span>
-   <span data-ttu-id="0c3dc-114">Vytvoření nového správce prostředků na základě webové služby a fakturační plán</span><span class="sxs-lookup"><span data-stu-id="0c3dc-114">Create a New Resource Manager based web service and billing plan</span></span>

<span data-ttu-id="0c3dc-115">K vytvoření účtu, volejte Microsoft zákaznický servis a podporu (CSS) a zadejte ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-115">To create your account, call Microsoft Customer Service and Support (CSS) and provide your Subscription ID.</span></span> <span data-ttu-id="0c3dc-116">Šablon stylů CSS bude fungovat s určit příslušné kapacitu pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-116">CSS will work with you to determine the appropriate capacity for your scenario.</span></span> <span data-ttu-id="0c3dc-117">Šablon stylů CSS, nakonfiguruje váš účet s maximální počet fondy, které můžete vytvořit a maximální počet virtuálních počítačů (VM), které můžete umístit v každém fondu.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-117">CSS then configures your account with the maximum number of pools you can create and the maximum number of virtual machines (VMs) that you can place in each pool.</span></span> <span data-ttu-id="0c3dc-118">Jakmile je váš účet nakonfigurován, jsou k dispozici adresu URL služby fondu a autorizační klíč.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-118">Once your account is configured, you are provided your Pool Service URL and an authorization key.</span></span>

<span data-ttu-id="0c3dc-119">Po vytvoření účtu použijete adresu URL služby fondu a autorizační klíč mohli provádět operace správy fondu na vaše fondu služby Batch.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-119">After your account is created, you use the Pool Service URL and authorization key to perform pool management operations on your Batch Pool.</span></span>

![Architektura fondu služby batch.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

<span data-ttu-id="0c3dc-121">Vytvořit fondy voláním operace vytvořit fond na adresu URL služby fondu, které jste získali šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-121">You create pools by calling the Create Pool operation on the pool service URL that CSS provided to you.</span></span> <span data-ttu-id="0c3dc-122">Když vytvoříte fond, zadejte že počet virtuálních počítačů a adresu URL swagger.json nového správce zdrojů na základě webové službě Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-122">When you create a pool, specify the number of VMs and the URL of the swagger.json of a New Resource Manager based Machine Learning web service.</span></span> <span data-ttu-id="0c3dc-123">Této webové služby je určen k vytvoření přidružení fakturace.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-123">This web service is provided to establish the billing association.</span></span> <span data-ttu-id="0c3dc-124">Služba fondu Batch používá swagger.json k fondu přidružit plán fakturace.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-124">The Batch Pool service uses the swagger.json to associate the pool with a billing plan.</span></span> <span data-ttu-id="0c3dc-125">Můžete spustit všechny BES webové služby, na základě i nové Resource Manager a classic, zvolíte ve fondu.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-125">You can run any BES web service, both New Resource Manager based and classic, you choose on the pool.</span></span>

<span data-ttu-id="0c3dc-126">Můžete použít žádné nové správce prostředků na základě webové služby, ale mějte na paměti, že fakturace pro úlohy připsány fakturační plán spojené s touto službou.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-126">You can use any New Resource Manager based web service, but be aware that the billing for the jobs are charged against the billing plan associated with that service.</span></span> <span data-ttu-id="0c3dc-127">Můžete vytvořit nový plán fakturace speciálně pro spuštění úlohy fondu služby Batch a webové služby.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-127">You may want to create a web service and new billing plan specifically for running Batch Pool jobs.</span></span>

<span data-ttu-id="0c3dc-128">Další informace o vytváření webových služeb najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="0c3dc-128">For more information on creating web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="0c3dc-129">Jakmile vytvoříte fond, odešlete úlohu BES pomocí adresy URL žádosti Batch pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-129">Once you have created a pool, you submit the BES job using the Batch Requests URL for the web service.</span></span> <span data-ttu-id="0c3dc-130">Můžete ho odeslat do fondu nebo classic dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-130">You can choose to submit it to a pool or to classic batch processing.</span></span> <span data-ttu-id="0c3dc-131">Se odeslat úlohu pro zpracování fondu Batch, přidáte do textu úlohy odeslání požadavku na následující parametr:</span><span class="sxs-lookup"><span data-stu-id="0c3dc-131">To submit a job to Batch Pool processing, you add the following parameter to the job submission request body:</span></span>

<span data-ttu-id="0c3dc-132">"AzureBatchPoolId": "&lt;fond ID&gt;"</span><span class="sxs-lookup"><span data-stu-id="0c3dc-132">"AzureBatchPoolId":"&lt;pool ID&gt;"</span></span>

<span data-ttu-id="0c3dc-133">Pokud je nemůžete přidat parametr, úloha běží v prostředí procesu classic batch.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-133">If you do not add the parameter, the job is run in the classic batch process environment.</span></span> <span data-ttu-id="0c3dc-134">Pokud fondu dostupné prostředky, úloha spuštění okamžitě.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-134">If the pool has available resources, the job starts running immediately.</span></span> <span data-ttu-id="0c3dc-135">Pokud fond nemá volné prostředky, vaše úloha je zařazena do fronty dokud prostředek k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-135">If the pool does not have free resources, your job is queued until a resource is available.</span></span>

<span data-ttu-id="0c3dc-136">Pokud zjistíte, že nedostanete pravidelně kapacitu fondech a je třeba zvýšené kapacity, můžete volat šablon stylů CSS a pracovat s zástupce ke zvýšení vaší kvóty.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-136">If you find that you regularly reach the capacity of your pools, and you need increased capacity, you can call CSS and work with a representative to increase your quotas.</span></span>

<span data-ttu-id="0c3dc-137">Příklad žádost:</span><span class="sxs-lookup"><span data-stu-id="0c3dc-137">Example Request:</span></span>

<span data-ttu-id="0c3dc-138">https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0</span><span class="sxs-lookup"><span data-stu-id="0c3dc-138">https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0</span></span>

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a><span data-ttu-id="0c3dc-139">Informace týkající se použití zpracování fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="0c3dc-139">Considerations when using Batch Pool processing</span></span>

<span data-ttu-id="0c3dc-140">Zpracování fondu batch je vždy na fakturovatelný služba a který vyžaduje, abyste ji přidružit plán fakturace na základě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-140">Batch Pool Processing is an always-on billable service and that requires you to associate it with a Resource Manager based billing plan.</span></span> <span data-ttu-id="0c3dc-141">Fakturuje se pouze pro počet – výpočetní hodiny, které běží fondu; bez ohledu na počet úloh spustit během tohoto fondu čas.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-141">You are only billed for the number of compute hours the pool is running; regardless of the number of jobs run during that time pool.</span></span> <span data-ttu-id="0c3dc-142">Pokud vytvoříte fond, se vám účtuje výpočetní hodiny každého virtuálního počítače ve fondu až do odstranění fondu i v případě, že nebudou spuštěny žádné úlohy batch ve fondu.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-142">If you create a pool, you are billed for the compute hours of each virtual machine in the pool until the pool is deleted, even if no batch jobs are running in the pool.</span></span> <span data-ttu-id="0c3dc-143">Fakturace pro virtuální počítače, spustí, když jejich dokončení zřizování a zastaví, když byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-143">Billing for the virtual machines starts when they have finished provisioning and stops when they have been deleted.</span></span> <span data-ttu-id="0c3dc-144">Můžete použít některou z plánů v nalezen [Machine Learning – ceny stránky](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="0c3dc-144">You can use any of the plans found on the [Machine Learning Pricing page](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>

<span data-ttu-id="0c3dc-145">Příklad fakturace:</span><span class="sxs-lookup"><span data-stu-id="0c3dc-145">Billing example:</span></span>

<span data-ttu-id="0c3dc-146">Pokud chcete vytvořit fondu služby Batch s 2 virtuální počítače a odstranit po 24 hodinách cenového plánu je odečte 48 hodin výpočetní; bez ohledu na to, kolik úlohy byly spuštěné během této doby.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-146">If you create a Batch Pool with 2 virtual machines and delete it after 24 hours your billing plan is debited 48 compute hours; regardless of how many jobs were run during that period.</span></span>

<span data-ttu-id="0c3dc-147">Pokud jste vytvoření fondu služby Batch s 4 virtuální počítače a odstranit po 12 hodinách, cenového plánu je také MD 48 výpočetní hodiny.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-147">If you create a Batch Pool with 4 virtual machines and delete it after 12 hours, your billing plan is also debited 48 compute hours.</span></span>

<span data-ttu-id="0c3dc-148">Doporučujeme, abyste cyklické dotazování stav úlohy k určení, kdy dokončení úloh.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-148">We recommend that you poll the job status to determine when jobs complete.</span></span> <span data-ttu-id="0c3dc-149">Po dokončení všech úloh spuštěna, volání operaci změnit velikost fondu nastavit počet virtuálních počítačů ve fondu na nulu.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-149">When all your jobs have finished running, call the Resize Pool operation to set the number of virtual machines in the pool to zero.</span></span> <span data-ttu-id="0c3dc-150">Pokud jste krátké u fondu prostředků a je nutné vytvořit nový fond, například k vyúčtování proti jiný fakturační plán, můžete odstranit fondu místo při spuštění dokončení všech úloh.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-150">If you are short on pool resources and you need to create a new pool, for example to bill against a different billing plan, you can delete the pool instead when all your jobs have finished running.</span></span>


| <span data-ttu-id="0c3dc-151">**Použít při zpracování fondu služby Batch**</span><span class="sxs-lookup"><span data-stu-id="0c3dc-151">**Use Batch Pool Processing when**</span></span>    | <span data-ttu-id="0c3dc-152">**Použít classic při zpracování dávky**</span><span class="sxs-lookup"><span data-stu-id="0c3dc-152">**Use classic batch processing when**</span></span>  |
|---|---|
|<span data-ttu-id="0c3dc-153">Budete muset spustit velký počet úloh</span><span class="sxs-lookup"><span data-stu-id="0c3dc-153">You need to run a large number of jobs</span></span><br><span data-ttu-id="0c3dc-154">Nebo</span><span class="sxs-lookup"><span data-stu-id="0c3dc-154">Or</span></span><br/><span data-ttu-id="0c3dc-155">Je třeba vědět, že vaše úlohy se spustí okamžitě</span><span class="sxs-lookup"><span data-stu-id="0c3dc-155">You need to know that your jobs will run immediately</span></span><br/><span data-ttu-id="0c3dc-156">Nebo</span><span class="sxs-lookup"><span data-stu-id="0c3dc-156">Or</span></span><br/><span data-ttu-id="0c3dc-157">Je nutné zaručenou propustnost.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-157">You need guaranteed throughput.</span></span> <span data-ttu-id="0c3dc-158">Například musíte spustit určitý počet úloh v zadaném časovém období a chcete škálovat svoje výpočetní prostředky podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="0c3dc-158">For example, you need to run a number of jobs in a given time frame, and want to scale out your compute resources to meet your needs.</span></span>    | <span data-ttu-id="0c3dc-159">Používáte několika úloh</span><span class="sxs-lookup"><span data-stu-id="0c3dc-159">You are running just a few jobs</span></span><br/><span data-ttu-id="0c3dc-160">A</span><span class="sxs-lookup"><span data-stu-id="0c3dc-160">And</span></span><br/> <span data-ttu-id="0c3dc-161">Nepotřebujete úlohy, které mají spustit okamžitě</span><span class="sxs-lookup"><span data-stu-id="0c3dc-161">You don’t need the jobs to run immediately</span></span> |
