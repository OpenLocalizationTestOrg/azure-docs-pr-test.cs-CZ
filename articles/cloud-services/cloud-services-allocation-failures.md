---
title: "aaaTroubleshooting došlo k chybě přidělení cloudové služby | Microsoft Docs"
description: "Řešení potíží s přidělením při nasazení Cloud Services v Azure"
services: azure-service-management, cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: dfd5cc4663ccc6ed1b27ca9df579182737363b0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Řešení potíží s přidělením při nasazení Cloud Services v Azure
## <a name="summary"></a>Souhrn
Při nasazení instance tooa Cloudovou službu nebo přidat nový web nebo instancí rolí pracovního procesu, Microsoft Azure přiřadí výpočetní prostředky. Příležitostně zobrazí chyby při provádění těchto operací ještě před dosažením hello limity předplatného Azure. Tento článek vysvětluje hello příčiny některých běžných chyb přidělení hello a navrhne možné nápravu. Hello informace může být užitečné také při plánování nasazení hello vašich služeb.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Pozadí – fungování přidělení
do clusterů do několika oddílů Hello serverů v datových centrech Azure. Žádost o nové cloudové služby přidělení dojde k pokusu o několik clusterů. Při první instance hello je nasazené tooa cloudové služby (v rámci buď pracovním nebo produkčním), která Cloudová služba získá definovaného tooa clusteru. Žádné další nasazení pro hello Cloudová služba se stane v hello stejné clusteru. V tomto článku označujeme toothis jako "definovaného tooa clusteru". Obrázek 1 níže znázorňuje hello malá normální přidělení, které dojde k pokusu o několik clusterů; Obrázek 2 znázorňuje hello malá přidělení to je vázaný tooCluster 2 protože, kde je hello existující CS_1 cloudové služby je hostovaná.

![Diagram přidělení](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Proč se stane došlo k chybě přidělení
Při požadavku na přidělení je vázaný tooa cluster, existuje vyšší pravděpodobnost selhávat toofind volných prostředků, protože fond k dispozici zdrojů hello je omezená tooa clusteru. Kromě toho pokud požadavek na přidělení definovaného tooa clusteru, ale tento cluster nepodporuje hello typ prostředku, který jste požadovali, vaši žádost se nezdaří i v případě, že má hello cluster volný prostředek. Obrázek 3 níže znázorňuje hello případu, kdy připojené přidělení nezdaří, protože hello pouze candidate cluster nebude mít uvolnění prostředků. Obrázek 4 ukazuje hello případu, kdy připojené přidělení nezdaří, protože hello pouze candidate clusteru nepodporuje hello požadovaná velikost virtuálního počítače, i když má hello cluster uvolnění prostředků.

![Došlo k chybě definovaného přidělení](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Řešení potíží s přidělením pro cloudové služby
### <a name="error-message"></a>Chybová zpráva
Mohou se zobrazit hello následující chybová zpráva:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable toosatisfy constraints in request. hello requested new service deployment is bound tooan Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains hello new deployment toospecific Azure resources. Please retry later or try reducing hello VM size or number of role instances. Alternatively, if possible, remove hello aforementioned constraints or try deploying tooa different region."

### <a name="common-issues"></a>Běžné problémy
Tady jsou hello běžné přidělení scénáře, které způsobí přidělení požadavek toobe definovaného tooa jeden cluster.

* Nasazení tooStaging slotu – cloudové služby má v buď slotu nasazení, pak hello celý Cloudová služba je vázaný tooa konkrétní clusteru.  To znamená, že pokud nasazení již existuje v produkční slot hello, pak nové pracovní nasazení lze pouze přiřadit ve hello stejné clusteru jako hello produkční slot. Pokud hello cluster se blíží obsazení, hello žádost může selhat.
* Škálování – přidání nové instance tooan stávající cloudovou službu přidělíte v hello stejné clusteru.  Malého škálování požadavků lze obvykle rozdělit, ale ne vždy. Pokud hello cluster se blíží obsazení, hello žádost může selhat.
* Skupina vztahů – nové nasazení tooan prázdný cloudové služby mohou být přiděleny přes hello fabric v jakémkoliv clusteru v této oblasti, pokud hello Cloudová služba je skupina vztahů definovaného tooan. Nasazení toohello stejnou skupinu vztahů se pokus o hello stejného clusteru. Pokud hello cluster se blíží obsazení, hello žádost může selhat.
* Skupiny vztahů vNet - starší virtuální sítě bylo vázané tooaffinity skupin místo oblasti a cloudové služby v tyto virtuální sítě budou definovaného toohello vztahů skupiny clusteru. V clusteru hello připnutý se pokusí toothis typ nasazení virtuální sítě. Pokud hello cluster se blíží obsazení, hello žádost může selhat.

## <a name="solutions"></a>Řešení
1. Znovu ho zaveďte tooa nová Cloudová služba – toto řešení je pravděpodobně toobe nejvíce úspěšné, protože umožňuje hello platformy toochoose ze všech clusterů v této oblasti.

   * Nasazení hello zatížení tooa novou cloudovou službu  
   * Aktualizovat hello CNAME nebo A záznamů toopoint provoz toohello novou cloudovou službu
   * Jakmile nulové provoz prochází toohello staré lokality, můžete odstranit hello staré cloudové služby. Toto řešení měly ukládat nepřeruší.
2. Odstranit produkční a pracovní sloty – toto řešení se zachovat svůj existující název DNS, ale může způsobit výpadek tooyour aplikace.

   * Odstranit hello produkční a pracovní sloty stávající cloudovou službu, aby hello Cloudová služba je prázdná a potom
   * Vytvořte nové nasazení v hello stávající cloudovou službu. To se znovu pokusí tooallocation na všech clusterech v oblasti hello. Zajistěte, aby hello Cloudová služba není vázaná tooan skupiny vztahů.
3. Vyhrazená IP adresa – toto řešení se zachovat stávající IP adresy, ale může způsobit výpadek tooyour aplikace.  

   * Vytvořit vyhrazenou IP adresu pro vaše stávající nasazení pomocí prostředí Powershell

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Postupujte podle #2 výše, provedení zda toospecify hello nové vyhrazené IP adresy v CSCFG hello služby.
4. Odeberte skupinu vztahů pro nová nasazení - už doporučují skupiny vztahů. Postupujte podle kroků pro #1 výše toodeploy novou cloudovou službu. Ujistěte se, Cloudová služba není ve skupině vztahů.
5. Převést tooa regionální virtuální síť – viz [jak toomigrate ze skupiny vztahů tooa regionální virtuální síť (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
