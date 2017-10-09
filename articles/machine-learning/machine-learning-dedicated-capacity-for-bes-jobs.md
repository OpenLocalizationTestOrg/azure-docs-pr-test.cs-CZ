---
title: "aaaDedicated kapacity pro Machine Learning dávky spuštění služby úlohy | Microsoft Docs"
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
ms.openlocfilehash: bba7970bb31c50e5b0b9d5f4ff4f0d2dacf942e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a><span data-ttu-id="2f84c-103">Služba Azure Batch pro Machine Learning úlohy</span><span class="sxs-lookup"><span data-stu-id="2f84c-103">Azure Batch service for Machine Learning jobs</span></span>

<span data-ttu-id="2f84c-104">Zpracování Machine Learning fondu Batch poskytuje spravované zákazníkem škálování pro hello Azure Machine Learning dávky spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="2f84c-104">Machine Learning Batch Pool processing provides customer-managed scale for hello Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="2f84c-105">Classic dávkového zpracování pro machine learning probíhá v prostředí s více klienty, které omezení hello počet souběžných úloh, můžete odeslat a úlohy se zařadí do fronty na základě objektů first in first out.</span><span class="sxs-lookup"><span data-stu-id="2f84c-105">Classic batch processing for machine learning takes place in a multi-tenant environment, which limits hello number of concurrent jobs you can submit, and jobs are queued on a first-in-first-out basis.</span></span> <span data-ttu-id="2f84c-106">Tato nejistoty znamená, že nemůžete vědět, když se spustí úlohu.</span><span class="sxs-lookup"><span data-stu-id="2f84c-106">This uncertainty means that you can't accurately predict when your job will run.</span></span>

<span data-ttu-id="2f84c-107">Zpracování fondu batch vám umožní toocreate fondy, na kterých můžete odeslat úlohy batch.</span><span class="sxs-lookup"><span data-stu-id="2f84c-107">Batch Pool processing allows you toocreate pools on which you can submit batch jobs.</span></span> <span data-ttu-id="2f84c-108">Řízení hello velikost fondu hello a toowhich fondu hello úloha odeslána.</span><span class="sxs-lookup"><span data-stu-id="2f84c-108">You control hello size of hello pool, and toowhich pool hello job is submitted.</span></span> <span data-ttu-id="2f84c-109">Úlohu BES běží ve vlastní zpracování poskytuje místo zpracování předvídatelný výkon a hello možnost toocreate prostředků fondy, které odpovídají toohello zatížení, která odešlete.</span><span class="sxs-lookup"><span data-stu-id="2f84c-109">Your BES job runs in its own processing space providing predictable processing performance and hello ability toocreate resource pools that correspond toohello processing load that you submit.</span></span>

## <a name="how-toouse-batch-pool-processing"></a><span data-ttu-id="2f84c-110">Jak toouse fondu Batch zpracování</span><span class="sxs-lookup"><span data-stu-id="2f84c-110">How toouse Batch Pool processing</span></span>

<span data-ttu-id="2f84c-111">Konfigurace zpracování fondu Batch není aktuálně k dispozici prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2f84c-111">Configuration of Batch Pool Processing is not currently available through hello Azure portal.</span></span> <span data-ttu-id="2f84c-112">toouse fondu Batch zpracování, musíte:</span><span class="sxs-lookup"><span data-stu-id="2f84c-112">toouse Batch Pool processing, you must:</span></span>

-   <span data-ttu-id="2f84c-113">Volání šablon stylů CSS toocreate účtu fondu Batch a získat adresu URL služby fondu a autorizační klíč</span><span class="sxs-lookup"><span data-stu-id="2f84c-113">Call CSS toocreate a Batch Pool Account and obtain a Pool Service URL and an authorization key</span></span>
-   <span data-ttu-id="2f84c-114">Vytvoření nového správce prostředků na základě webové služby a fakturační plán</span><span class="sxs-lookup"><span data-stu-id="2f84c-114">Create a New Resource Manager based web service and billing plan</span></span>

<span data-ttu-id="2f84c-115">toocreate účtu, volejte Microsoft zákaznický servis a podporu (CSS) a zadejte ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="2f84c-115">toocreate your account, call Microsoft Customer Service and Support (CSS) and provide your Subscription ID.</span></span> <span data-ttu-id="2f84c-116">Šablon stylů CSS bude fungovat s jste toodetermine hello odpovídající kapacitu pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="2f84c-116">CSS will work with you toodetermine hello appropriate capacity for your scenario.</span></span> <span data-ttu-id="2f84c-117">Šablon stylů CSS, nakonfiguruje váš účet s hello maximální počet fondy můžete vytvořit a hello maximální počet virtuálních počítačů (VM), které můžete umístit v každém fondu.</span><span class="sxs-lookup"><span data-stu-id="2f84c-117">CSS then configures your account with hello maximum number of pools you can create and hello maximum number of virtual machines (VMs) that you can place in each pool.</span></span> <span data-ttu-id="2f84c-118">Jakmile je váš účet nakonfigurován, jsou k dispozici adresu URL služby fondu a autorizační klíč.</span><span class="sxs-lookup"><span data-stu-id="2f84c-118">Once your account is configured, you are provided your Pool Service URL and an authorization key.</span></span>

<span data-ttu-id="2f84c-119">Po vytvoření účtu používáte hello adresa URL služby fondu a autorizace klíče tooperform fondu operace správy ve vašem fondu Batch.</span><span class="sxs-lookup"><span data-stu-id="2f84c-119">After your account is created, you use hello Pool Service URL and authorization key tooperform pool management operations on your Batch Pool.</span></span>

![Architektura fondu služby batch.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

<span data-ttu-id="2f84c-121">Vytvořit fondy voláním operace vytvořit fond hello na adresu URL služby fondu hello této tooyou poskytuje šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="2f84c-121">You create pools by calling hello Create Pool operation on hello pool service URL that CSS provided tooyou.</span></span> <span data-ttu-id="2f84c-122">Když vytvoříte fond, zadejte počet hello virtuální počítače a adresu URL hello hello swagger.json nového správce zdrojů na základě webové službě Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2f84c-122">When you create a pool, specify hello number of VMs and hello URL of hello swagger.json of a New Resource Manager based Machine Learning web service.</span></span> <span data-ttu-id="2f84c-123">Tato webová služba je k dispozici tooestablish hello fakturace přidružení.</span><span class="sxs-lookup"><span data-stu-id="2f84c-123">This web service is provided tooestablish hello billing association.</span></span> <span data-ttu-id="2f84c-124">Hello služby fondu Batch používá hello swagger.json tooassociate hello fondu s plán fakturace.</span><span class="sxs-lookup"><span data-stu-id="2f84c-124">hello Batch Pool service uses hello swagger.json tooassociate hello pool with a billing plan.</span></span> <span data-ttu-id="2f84c-125">Můžete spustit všechny BES webové služby, na základě i nové Resource Manager a classic, můžete zvolit na hello fondu.</span><span class="sxs-lookup"><span data-stu-id="2f84c-125">You can run any BES web service, both New Resource Manager based and classic, you choose on hello pool.</span></span>

<span data-ttu-id="2f84c-126">Můžete použít žádné nové správce prostředků na základě webové služby, ale mějte na paměti, že hello fakturace pro úlohy hello připsány hello fakturační plán spojené s touto službou.</span><span class="sxs-lookup"><span data-stu-id="2f84c-126">You can use any New Resource Manager based web service, but be aware that hello billing for hello jobs are charged against hello billing plan associated with that service.</span></span> <span data-ttu-id="2f84c-127">Konkrétně pro spuštění úlohy fondu Batch můžete toocreate webové služby a fakturace nový plán.</span><span class="sxs-lookup"><span data-stu-id="2f84c-127">You may want toocreate a web service and new billing plan specifically for running Batch Pool jobs.</span></span>

<span data-ttu-id="2f84c-128">Další informace o vytváření webových služeb najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="2f84c-128">For more information on creating web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="2f84c-129">Jakmile vytvoříte fond, odešlete hello BES pomocí úlohy hello žádosti Batch adresu URL pro webovou službu hello.</span><span class="sxs-lookup"><span data-stu-id="2f84c-129">Once you have created a pool, you submit hello BES job using hello Batch Requests URL for hello web service.</span></span> <span data-ttu-id="2f84c-130">Můžete zvolit toosubmit ho fond nebo tooclassic tooa dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="2f84c-130">You can choose toosubmit it tooa pool or tooclassic batch processing.</span></span> <span data-ttu-id="2f84c-131">toosubmit zpracování úlohy tooBatch fondu, přidejte následující parametr toohello úlohy odeslání žádosti o textu hello:</span><span class="sxs-lookup"><span data-stu-id="2f84c-131">toosubmit a job tooBatch Pool processing, you add hello following parameter toohello job submission request body:</span></span>

<span data-ttu-id="2f84c-132">"AzureBatchPoolId": "&lt;fond ID&gt;"</span><span class="sxs-lookup"><span data-stu-id="2f84c-132">"AzureBatchPoolId":"&lt;pool ID&gt;"</span></span>

<span data-ttu-id="2f84c-133">Pokud je nemůžete přidat parametr hello, hello úlohy běží v prostředí procesu classic batch hello.</span><span class="sxs-lookup"><span data-stu-id="2f84c-133">If you do not add hello parameter, hello job is run in hello classic batch process environment.</span></span> <span data-ttu-id="2f84c-134">Pokud fond hello nemá k dispozici prostředky, hello úlohy spuštění okamžitě.</span><span class="sxs-lookup"><span data-stu-id="2f84c-134">If hello pool has available resources, hello job starts running immediately.</span></span> <span data-ttu-id="2f84c-135">Pokud hello fond nemá volné prostředky, vaše úloha je zařazena do fronty dokud prostředek k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2f84c-135">If hello pool does not have free resources, your job is queued until a resource is available.</span></span>

<span data-ttu-id="2f84c-136">Pokud zjistíte, že nedostanete pravidelně hello kapacitu fondech a je třeba zvýšené kapacity, můžete volat šablon stylů CSS a pracovat s reprezentativní tooincrease vaší kvóty.</span><span class="sxs-lookup"><span data-stu-id="2f84c-136">If you find that you regularly reach hello capacity of your pools, and you need increased capacity, you can call CSS and work with a representative tooincrease your quotas.</span></span>

<span data-ttu-id="2f84c-137">Příklad žádost:</span><span class="sxs-lookup"><span data-stu-id="2f84c-137">Example Request:</span></span>

<span data-ttu-id="2f84c-138">https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0</span><span class="sxs-lookup"><span data-stu-id="2f84c-138">https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0</span></span>

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

## <a name="considerations-when-using-batch-pool-processing"></a><span data-ttu-id="2f84c-139">Informace týkající se použití zpracování fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="2f84c-139">Considerations when using Batch Pool processing</span></span>

<span data-ttu-id="2f84c-140">Zpracování fondu batch je vždy na fakturovatelný služba a který vyžaduje, abyste tooassociate ji pomocí Správce prostředků na základě plán fakturace.</span><span class="sxs-lookup"><span data-stu-id="2f84c-140">Batch Pool Processing is an always-on billable service and that requires you tooassociate it with a Resource Manager based billing plan.</span></span> <span data-ttu-id="2f84c-141">Fakturuje se pouze pro hello počet – výpočetní hodiny, které běží hello fondu; bez ohledu na to hello počet úloh spustit během tohoto fondu čas.</span><span class="sxs-lookup"><span data-stu-id="2f84c-141">You are only billed for hello number of compute hours hello pool is running; regardless of hello number of jobs run during that time pool.</span></span> <span data-ttu-id="2f84c-142">Pokud vytvoříte fond, se vám účtuje hello výpočetní hodiny každého virtuálního počítače ve fondu hello až do odstranění fondu hello i v případě, že nebudou spuštěny žádné úlohy batch ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="2f84c-142">If you create a pool, you are billed for hello compute hours of each virtual machine in hello pool until hello pool is deleted, even if no batch jobs are running in hello pool.</span></span> <span data-ttu-id="2f84c-143">Fakturace pro virtuální počítače hello spustí, když jejich dokončení zřizování a zastaví, když byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="2f84c-143">Billing for hello virtual machines starts when they have finished provisioning and stops when they have been deleted.</span></span> <span data-ttu-id="2f84c-144">Můžete použít některou z plánů hello najít na hello [Machine Learning – ceny stránky](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="2f84c-144">You can use any of hello plans found on hello [Machine Learning Pricing page](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>

<span data-ttu-id="2f84c-145">Příklad fakturace:</span><span class="sxs-lookup"><span data-stu-id="2f84c-145">Billing example:</span></span>

<span data-ttu-id="2f84c-146">Pokud chcete vytvořit fondu služby Batch s 2 virtuální počítače a odstranit po 24 hodinách cenového plánu je odečte 48 hodin výpočetní; bez ohledu na to, kolik úlohy byly spuštěné během této doby.</span><span class="sxs-lookup"><span data-stu-id="2f84c-146">If you create a Batch Pool with 2 virtual machines and delete it after 24 hours your billing plan is debited 48 compute hours; regardless of how many jobs were run during that period.</span></span>

<span data-ttu-id="2f84c-147">Pokud jste vytvoření fondu služby Batch s 4 virtuální počítače a odstranit po 12 hodinách, cenového plánu je také MD 48 výpočetní hodiny.</span><span class="sxs-lookup"><span data-stu-id="2f84c-147">If you create a Batch Pool with 4 virtual machines and delete it after 12 hours, your billing plan is also debited 48 compute hours.</span></span>

<span data-ttu-id="2f84c-148">Doporučujeme, abyste dotazování toodetermine stav úlohy hello, po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="2f84c-148">We recommend that you poll hello job status toodetermine when jobs complete.</span></span> <span data-ttu-id="2f84c-149">Po dokončení všech úloh spuštěna, volání hello hello číslo tooset operace změny velikosti fondu virtuálních počítačů v toozero fondu hello.</span><span class="sxs-lookup"><span data-stu-id="2f84c-149">When all your jobs have finished running, call hello Resize Pool operation tooset hello number of virtual machines in hello pool toozero.</span></span> <span data-ttu-id="2f84c-150">Pokud jste krátké u fondu prostředků a potřebujete toocreate nový fond, například toobill proti jiný fakturační plán, můžete odstranit hello fondu místo při spuštění dokončení všech úloh.</span><span class="sxs-lookup"><span data-stu-id="2f84c-150">If you are short on pool resources and you need toocreate a new pool, for example toobill against a different billing plan, you can delete hello pool instead when all your jobs have finished running.</span></span>


| <span data-ttu-id="2f84c-151">**Použít při zpracování fondu služby Batch**</span><span class="sxs-lookup"><span data-stu-id="2f84c-151">**Use Batch Pool Processing when**</span></span>    | <span data-ttu-id="2f84c-152">**Použít classic při zpracování dávky**</span><span class="sxs-lookup"><span data-stu-id="2f84c-152">**Use classic batch processing when**</span></span>  |
|---|---|
|<span data-ttu-id="2f84c-153">Je třeba toorun velký počet úloh</span><span class="sxs-lookup"><span data-stu-id="2f84c-153">You need toorun a large number of jobs</span></span><br><span data-ttu-id="2f84c-154">Nebo</span><span class="sxs-lookup"><span data-stu-id="2f84c-154">Or</span></span><br/><span data-ttu-id="2f84c-155">Je třeba tooknow, který vaše úlohy se spustí okamžitě</span><span class="sxs-lookup"><span data-stu-id="2f84c-155">You need tooknow that your jobs will run immediately</span></span><br/><span data-ttu-id="2f84c-156">Nebo</span><span class="sxs-lookup"><span data-stu-id="2f84c-156">Or</span></span><br/><span data-ttu-id="2f84c-157">Je nutné zaručenou propustnost.</span><span class="sxs-lookup"><span data-stu-id="2f84c-157">You need guaranteed throughput.</span></span> <span data-ttu-id="2f84c-158">Například potřebovat toorun určitý počet úloh v zadaném časovém období a chcete tooscale se vaše výpočetní prostředky toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="2f84c-158">For example, you need toorun a number of jobs in a given time frame, and want tooscale out your compute resources toomeet your needs.</span></span>    | <span data-ttu-id="2f84c-159">Používáte několika úloh</span><span class="sxs-lookup"><span data-stu-id="2f84c-159">You are running just a few jobs</span></span><br/><span data-ttu-id="2f84c-160">A</span><span class="sxs-lookup"><span data-stu-id="2f84c-160">And</span></span><br/> <span data-ttu-id="2f84c-161">Nepotřebujete hello úlohy toorun okamžitě</span><span class="sxs-lookup"><span data-stu-id="2f84c-161">You don’t need hello jobs toorun immediately</span></span> |
