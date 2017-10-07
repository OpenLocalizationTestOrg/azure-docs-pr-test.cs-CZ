---
title: "aaaDeployment problémy pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy týkající se nasazení pro Microsoft Azure Cloud Services hello."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 8d67e36aa87fb5794d358e5cc235123ac7286028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy při nasazení pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy týkající se problémy při nasazení pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-toohello-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-hello-production-slot"></a>Proč nasazení cloudové služby toohello pracovní pozici někdy dojde k chybě Chyba přidělení prostředků Pokud je již existující nasazení v produkčním slotu hello?
Pokud cloudové služby má ve buď slotu nasazení, hello celý Cloudová služba je vázaný tooa konkrétní clusteru. To znamená, že pokud nasazení již existuje v produkční slot hello, nové pracovní nasazení lze pouze přiřadit ve hello stejné clusteru jako hello produkční slot.

Když hello clusteru, kde se nachází Cloudová služba nemá dostatek fyzické výpočetní prostředky toosatisfy vaši žádost o nasazení dojde k selhání přidělení.

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Proč vertikálním navýšení kapacity nebo škálování na více systémů nasazení služby cloud někdy dojít k selhání přidělení?
Když se nasadí Cloudová služba, obvykle získá definovaného tooa konkrétní clusteru. To znamená, že vertikálním navýšení kapacity/out existující cloudové služby přidělíte nové instance v hello stejného clusteru. Pokud hello cluster se blíží obsazení nebo hello požadovaných virtuálních počítačů velikost a typ není k dispozici, hello žádost může selhat.

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Proč někdy nasazení cloudové služby do skupiny vztahů mít za následek došlo k chybě přidělení?
Nová nasazení tooan prázdný Cloudová služba může být přidělen přes hello fabric v jakémkoliv clusteru v této oblasti, pokud je skupina vztahů definovaného tooan hello cloudové služby. Nasazení toohello stejnou skupinu vztahů se pokus o hello stejného clusteru. Pokud hello cluster se blíží obsazení, hello žádost může selhat.

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-tooan-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Proč Změna velikosti virtuálního počítače nebo přidání nového virtuálního počítače tooan existující cloudové služby někdy za následek došlo k chybě přidělení?
Hello clustery v datacentru může mít různé konfigurace počítače typy (například řady, Av2 řady, D řady, Dv2 řady, řady G, H řady, atd.). Ale ne všechny clustery hello by měla mít nutně nejrůznějších hello virtuálních počítačů. Například pokud se pokusíte tooadd řadu D virtuálních počítačů tooa Cloudová služba, která je již nasazen v clusteru služby A jen řady, si všimnete došlo k chybě přidělení. Pokud se pokusíte proběhne také, že toochange virtuálních počítačů SKU velikosti (například přepínání z jako řady A řady tooa D).

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

toocheck hello velikosti k dispozici ve vašem regionu, najdete v části [Microsoft Azure: produkty podle oblasti](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-toolimitsquotasconstraints-on-my-subscription-or-service"></a>Proč nasazení cloudu služby přetrvával selhání kvůli toolimits/kvóty nebo omezení pro Moje předplatné nebo služby?
Nasazení cloudové služby může selhat, pokud hello prostředky, které jsou požadované toobe přidělené překročí hello výchozí nebo maximální kvóty, které jsou povoleny služby na úrovni hello oblasti nebo datacenter. Další informace najdete v tématu [omezuje cloudové služby](../azure-subscription-service-limits.md#cloud-services-limits).

Může také sledovat kvóty aktuální využití hello pro vaše předplatné na portálu hello: portál Azure = > odběry = > \<příslušné předplatné > = > "Využití + kvóty".

Prostřednictvím hello fakturace rozhraní API služby Azure můžete také načíst informace o využití, spotřebu související prostředek. V tématu [využití prostředků Azure (Preview) rozhraní API](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-hello-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Jak můžete změnit velikost hello nasazené cloudové služby virtuálních počítačů bez jejího opakovaného nasazení?
Velikost virtuálního počítače hello nasazené cloudové služby nelze změnit bez opětovného nasazení ho. Hello velikost virtuálního počítače je integrovaná do hello CSDEF, které lze aktualizovat pouze s znovu ho zaveďte.

Další informace najdete v tématu [jak tooupdate Cloudová služba](cloud-services-update-azure-service.md).

 
