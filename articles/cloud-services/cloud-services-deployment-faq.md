---
title: "Problémy při nasazení pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy o nasazení pro Microsoft Azure Cloud Services."
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
ms.openlocfilehash: cb62cd4c4635d9e837dff81a9c4543781dd11adb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy při nasazení pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy týkající se problémy při nasazení pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Proč nasazení cloudové služby na přípravný slot někdy dojde k chybě Chyba přidělení prostředků Pokud je již existující nasazení v produkčním slotu?
Cloudové služby má v buď slotu nasazení, je do clusteru s podporou konkrétní připnuli celý cloudové služby. To znamená, že pokud nasazení již existuje v produkčním slotu, nové pracovní nasazení lze pouze rozdělit ve stejném clusteru jako produkční slot.

Pokud cluster, kde se nachází Cloudová služba nemá dostatek fyzické výpočetní prostředky, které splňují vaše žádost o nasazení dojde k selhání přidělení.

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Proč vertikálním navýšení kapacity nebo škálování na více systémů nasazení služby cloud někdy dojít k selhání přidělení?
Když se nasadí Cloudová služba, je obvykle získá připnuli do clusteru s podporou konkrétní. To znamená, že škálování nahoru nebo se na existující službu cloud přidělíte nové instance ve stejném clusteru. Pokud cluster se blíží obsazení nebo požadovanou velikost a typ virtuálního počítače není k dispozici, požadavek se pravděpodobně nezdaří.

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Proč někdy nasazení cloudové služby do skupiny vztahů mít za následek došlo k chybě přidělení?
Nové nasazení pro prázdný cloudové služby může být přidělen podle prostředků infrastruktury v jakémkoliv clusteru v této oblasti, pokud cloudové služby je připojena do skupiny vztahů. Ve stejném clusteru se pokusí nasazení pro stejnou skupinu vztahů. Pokud cluster se blíží obsazení, požadavek se pravděpodobně nezdaří.

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Proč Změna velikosti virtuálního počítače nebo přidání nového virtuálního počítače do existující cloudové služby někdy za následek došlo k chybě přidělení?
Clustery v datacentru může mít různé konfigurace počítače typy (například řady, Av2 řady, D řady, Dv2 řady, řady G, H řady, atd.). Ale ne všechny clustery by měla mít nutně všechny typy virtuálních počítačů. Například pokud se pokusíte přidat řadu D virtuálního počítače do cloudové služby, který už je nasazený na clusteru A jen řady, si všimnete došlo k chybě přidělení. Pokud se pokusíte změnit proběhne také, že virtuální počítač SKU velikosti (například změna A series na řadu D).

Zmírnění takové chyby v přidělení pomoc najdete v tématu [došlo k chybě přidělení cloudové služby: řešení](cloud-services-allocation-failures.md#solutions).

Kontrola velikosti, která je k dispozici ve vašem regionu, najdete v tématu [Microsoft Azure: produkty podle oblasti](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Proč chvíli nasazení cloudové služby nezdaří z důvodu omezení nebo kvóty nebo omezení pro Moje předplatné nebo služby?
Nasazení cloudové služby může selhat v případě, že prostředky, které jsou nutné k přidělit překročí výchozí nebo maximální kvóty, které jsou povoleny služby na úrovni oblasti nebo datacenter. Další informace najdete v tématu [omezuje cloudové služby](../azure-subscription-service-limits.md#cloud-services-limits).

Může také sledovat aktuální využití nebo kvótu pro vaše předplatné na portálu: portál Azure = > odběry = > \<příslušné předplatné > = > "Využití + kvóty".

Informace o využití, spotřebu související prostředku můžete také načíst prostřednictvím rozhraní API Azure fakturace. V tématu [využití prostředků Azure (Preview) rozhraní API](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Jak můžete změnit velikost virtuální počítač nasazený cloudové služby bez jejího opakovaného nasazení?
Velikost virtuálního počítače nasazeného cloudové služby nelze změnit bez opětovného nasazení ho. Velikost virtuálního počítače je integrovaná do CSDEF, které lze aktualizovat pouze s znovu ho zaveďte.

Další informace najdete v tématu [jak aktualizovat Cloudová služba](cloud-services-update-azure-service.md).

 
