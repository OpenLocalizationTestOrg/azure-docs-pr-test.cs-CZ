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
# <a name="azure-batch-service-for-machine-learning-jobs"></a>Služba Azure Batch pro Machine Learning úlohy

Zpracování Machine Learning fondu Batch poskytuje spravované zákazníkem škálování pro hello Azure Machine Learning dávky spuštění služby. Classic dávkového zpracování pro machine learning probíhá v prostředí s více klienty, které omezení hello počet souběžných úloh, můžete odeslat a úlohy se zařadí do fronty na základě objektů first in first out. Tato nejistoty znamená, že nemůžete vědět, když se spustí úlohu.

Zpracování fondu batch vám umožní toocreate fondy, na kterých můžete odeslat úlohy batch. Řízení hello velikost fondu hello a toowhich fondu hello úloha odeslána. Úlohu BES běží ve vlastní zpracování poskytuje místo zpracování předvídatelný výkon a hello možnost toocreate prostředků fondy, které odpovídají toohello zatížení, která odešlete.

## <a name="how-toouse-batch-pool-processing"></a>Jak toouse fondu Batch zpracování

Konfigurace zpracování fondu Batch není aktuálně k dispozici prostřednictvím hello portálu Azure. toouse fondu Batch zpracování, musíte:

-   Volání šablon stylů CSS toocreate účtu fondu Batch a získat adresu URL služby fondu a autorizační klíč
-   Vytvoření nového správce prostředků na základě webové služby a fakturační plán

toocreate účtu, volejte Microsoft zákaznický servis a podporu (CSS) a zadejte ID předplatného. Šablon stylů CSS bude fungovat s jste toodetermine hello odpovídající kapacitu pro váš scénář. Šablon stylů CSS, nakonfiguruje váš účet s hello maximální počet fondy můžete vytvořit a hello maximální počet virtuálních počítačů (VM), které můžete umístit v každém fondu. Jakmile je váš účet nakonfigurován, jsou k dispozici adresu URL služby fondu a autorizační klíč.

Po vytvoření účtu používáte hello adresa URL služby fondu a autorizace klíče tooperform fondu operace správy ve vašem fondu Batch.

![Architektura fondu služby batch.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

Vytvořit fondy voláním operace vytvořit fond hello na adresu URL služby fondu hello této tooyou poskytuje šablon stylů CSS. Když vytvoříte fond, zadejte počet hello virtuální počítače a adresu URL hello hello swagger.json nového správce zdrojů na základě webové službě Machine Learning. Tato webová služba je k dispozici tooestablish hello fakturace přidružení. Hello služby fondu Batch používá hello swagger.json tooassociate hello fondu s plán fakturace. Můžete spustit všechny BES webové služby, na základě i nové Resource Manager a classic, můžete zvolit na hello fondu.

Můžete použít žádné nové správce prostředků na základě webové služby, ale mějte na paměti, že hello fakturace pro úlohy hello připsány hello fakturační plán spojené s touto službou. Konkrétně pro spuštění úlohy fondu Batch můžete toocreate webové služby a fakturace nový plán.

Další informace o vytváření webových služeb najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Jakmile vytvoříte fond, odešlete hello BES pomocí úlohy hello žádosti Batch adresu URL pro webovou službu hello. Můžete zvolit toosubmit ho fond nebo tooclassic tooa dávkové zpracování. toosubmit zpracování úlohy tooBatch fondu, přidejte následující parametr toohello úlohy odeslání žádosti o textu hello:

"AzureBatchPoolId": "&lt;fond ID&gt;"

Pokud je nemůžete přidat parametr hello, hello úlohy běží v prostředí procesu classic batch hello. Pokud fond hello nemá k dispozici prostředky, hello úlohy spuštění okamžitě. Pokud hello fond nemá volné prostředky, vaše úloha je zařazena do fronty dokud prostředek k dispozici.

Pokud zjistíte, že nedostanete pravidelně hello kapacitu fondech a je třeba zvýšené kapacity, můžete volat šablon stylů CSS a pracovat s reprezentativní tooincrease vaší kvóty.

Příklad žádost:

https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0

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

## <a name="considerations-when-using-batch-pool-processing"></a>Informace týkající se použití zpracování fondu služby Batch

Zpracování fondu batch je vždy na fakturovatelný služba a který vyžaduje, abyste tooassociate ji pomocí Správce prostředků na základě plán fakturace. Fakturuje se pouze pro hello počet – výpočetní hodiny, které běží hello fondu; bez ohledu na to hello počet úloh spustit během tohoto fondu čas. Pokud vytvoříte fond, se vám účtuje hello výpočetní hodiny každého virtuálního počítače ve fondu hello až do odstranění fondu hello i v případě, že nebudou spuštěny žádné úlohy batch ve fondu hello. Fakturace pro virtuální počítače hello spustí, když jejich dokončení zřizování a zastaví, když byla odstraněna. Můžete použít některou z plánů hello najít na hello [Machine Learning – ceny stránky](https://azure.microsoft.com/pricing/details/machine-learning/).

Příklad fakturace:

Pokud chcete vytvořit fondu služby Batch s 2 virtuální počítače a odstranit po 24 hodinách cenového plánu je odečte 48 hodin výpočetní; bez ohledu na to, kolik úlohy byly spuštěné během této doby.

Pokud jste vytvoření fondu služby Batch s 4 virtuální počítače a odstranit po 12 hodinách, cenového plánu je také MD 48 výpočetní hodiny.

Doporučujeme, abyste dotazování toodetermine stav úlohy hello, po dokončení úlohy. Po dokončení všech úloh spuštěna, volání hello hello číslo tooset operace změny velikosti fondu virtuálních počítačů v toozero fondu hello. Pokud jste krátké u fondu prostředků a potřebujete toocreate nový fond, například toobill proti jiný fakturační plán, můžete odstranit hello fondu místo při spuštění dokončení všech úloh.


| **Použít při zpracování fondu služby Batch**    | **Použít classic při zpracování dávky**  |
|---|---|
|Je třeba toorun velký počet úloh<br>Nebo<br/>Je třeba tooknow, který vaše úlohy se spustí okamžitě<br/>Nebo<br/>Je nutné zaručenou propustnost. Například potřebovat toorun určitý počet úloh v zadaném časovém období a chcete tooscale se vaše výpočetní prostředky toomeet vašim potřebám.    | Používáte několika úloh<br/>A<br/> Nepotřebujete hello úlohy toorun okamžitě |
