---
title: "aaaTraffic typy koncových bodů Manager | Microsoft Docs"
description: "Tento článek vysvětluje různé typy koncových bodů, které lze použít s Azure Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 787412ac6207f76791bf3ff753d1df2767b1a964
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-endpoints"></a>Koncové body Traffic Manageru
Microsoft Azure Traffic Manager vám umožní toocontrol, jak je síťový provoz distribuované nasazení tooapplication spuštěné v různých datových centrech. Konfigurace nasazení každé aplikace jako "koncový bod, v Traffic Manageru. Když Traffic Manager obdrží žádost o DNS, vybere tooreturn koncový bod k dispozici v hello odpověď DNS. Správce provozu odvodí hello volba hello aktuální stav koncového bodu a metodu směrování provozu hello. Další informace najdete v tématu [jak Traffic Manager funguje](traffic-manager-how-traffic-manager-works.md).

Existují tři typy podporované Traffic Managerem koncového bodu:
* **Koncové body Azure** se používají pro služby hostované v Azure.
* **Externí koncové body** se používají pro služby hostované mimo Azure, místně nebo pomocí jiného poskytovatele hostitelských služeb.
* **Vnořené koncové body** jsou toocreate profily Traffic Manager používané toocombine flexibilnější směrování provozu schémata toosupport hello potřebám rozsáhlejších, složitějších nasazení.

Neexistuje žádné omezení na tom, jak jsou koncové body různých typů kombinaci v jednom profilu Traffic Manageru. Každý profil může obsahovat libovolné kombinace typy koncových bodů.

Hello následující oddíly popisují každou typ koncového bodu podrobněji.

## <a name="azure-endpoints"></a>Koncové body Azure

Koncové body Azure používají služeb založených na Azure Traffic Manager. jsou podporovány následující typy prostředků Azure Hello:

* "Classic, virtuální počítače IaaS a PaaS cloudových služeb.
* Web Apps
* PublicIPAddress prostředků (mezi které může být připojené tooVMs, buď přímo nebo prostřednictvím Vyrovnávání zatížení Azure). Hello publicIpAddress musí mít název DNS přiřazené toobe použít v profilu Traffic Manageru.

PublicIPAddress prostředky jsou prostředky Azure Resource Manager. Neexistují v modelu nasazení classic hello. Proto jsou pouze podporované v Traffic Manageru na Azure Resource Manager prostředí. Hello jiné typy koncových bodů jsou podporované pomocí Resource Manageru a hello modelu nasazení classic.

Při použití koncové body Azure, Traffic Manager zjistí, že virtuální počítač IaaS "Klasickém", Cloudová služba nebo webové aplikace je zastavena a spuštěna. Tento stav se projeví ve stavu hello koncový bod. V tématu [monitorování koncového bodu Traffic Manager](traffic-manager-monitoring.md#endpoint-and-profile-status) podrobnosti. Pokud hello podkladová služba je zastavená, Traffic Manager nebude provádět kontroly stavu koncového bodu nebo přímé přenosy toohello koncový bod. Žádné Traffic Manager fakturace k událostem pro hello zastavena instance. Při restartování služby hello fakturace obnoví a koncový bod hello je vhodné tooreceive provoz. Toto zjišťování se nevztahuje tooPublicIpAddress koncové body.

## <a name="external-endpoints"></a>Externí koncové body

Externí koncové body se používají pro služby mimo Azure. Například služby hostované v místě nebo s jiným zprostředkovatelem. Externí koncové body lze použít jednotlivě nebo v kombinaci s koncovými body Azure v hello stejný profil služby Traffic Manager. Kombinování koncové body Azure s externí koncové body umožňuje různé scénáře:

* Buď model aktivní aktivní nebo aktivní – pasivní převzetí služeb při selhání, použijte Azure tooprovide vyšší redundance pro stávající místní aplikace.
* latence tooreduce aplikace pro uživatele kolem hello, world, rozšířit existující místní aplikace tooadditional zeměpisné umístění v Azure. Další informace najdete v tématu [Traffic Manager, výkon, směrování provozu](traffic-manager-routing-methods.md#performance).
* Pomocí Azure tooprovide dodatečnou kapacitu pro stávající místní aplikace, nepřetržitě nebo jako 'shluků cloudové' řešení toomeet Špička požadavků.

V některých případech je užitečné toouse tooreference externí koncové body Azure services (příklady najdete v tématu hello [– nejčastější dotazy](traffic-manager-faqs.md#traffic-manager-endpoints)). V takovém případě kontroly stavu fakturují se za hello koncové body Azure kurz, není kurz hello externí koncové body. Na rozdíl od koncové body Azure, zastavení a odstranění hello základní služby stavu Zkontrolujte však fakturace pokračuje, dokud není zakázat nebo odstranit hello koncový bod služby Traffic Manager.

## <a name="nested-endpoints"></a>Vnořené koncové body

Vnořené koncové body kombinovat více Traffic Manager profily toocreate flexibilní směrování provozu schémat a podporují hello potřebám větší a složité nasazení. S koncovými body vnořené "podřízený" profil se přidá jako profil 'nadřazeného' tooa koncový bod. Oba profily podřízenými a nadřazenými hello může obsahovat další koncové body libovolného typu, včetně jiných vnořených profilů. Další informace najdete v tématu [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Webové aplikace jako koncové body.

Při konfiguraci webové aplikace jako koncové body v Traffic Manageru, platí některé další aspekty:

1. Jsou vhodné pro použití s nástrojem Traffic Manager pouze webové aplikace hello "Standard" SKU nebo vyšší. Pokusí tooadd webovou aplikaci z nižší selhání SKU. Přechod na starší verzi hello výsledků SKU stávající webovou aplikaci v Traffic Manager už odesílání přenosů toothat webové aplikace.
2. Když koncový bod přijme požadavek HTTP, používá hello "hostitel" záhlaví v žádosti toodetermine hello které webové aplikace musí žádosti o služby hello. Hlavička hostitele Hello obsahuje hello DNS název používaný tooinitiate hello požadavku, například contosoapp.azurewebsites.net. toouse jiný název DNS s vaší webové aplikace, název DNS hello musí být registrován jako vlastní název domény pro hello aplikace. Při přidávání koncový bod webové aplikace jako koncový bod Azure, název DNS profil Traffic Manageru hello se automaticky registruje pro hello aplikace. Tato registrace je automaticky odstraněna při odstranění hello koncový bod.
3. Každý profil služby Traffic Manager může mít maximálně jednu webovou aplikaci endpoint z každé oblasti Azure. toowork kolem tomuto omezení, můžete nakonfigurovat webovou aplikaci jako externí koncový bod. Další informace najdete v tématu hello [– nejčastější dotazy](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Povolování a zakazování koncových bodů

Zakázání koncového bodu v Traffic Manager může být užitečné tootemporarily odebrat provoz z koncový bod, který je v režimu údržby nebo který je znovu nasazován. Jakmile se koncový bod hello je znovu spustit, může být znovu zapnout.

Koncové body lze povolit a zakázat prostřednictvím portálu hello Traffic Manager, prostředí PowerShell, rozhraní příkazového řádku nebo REST API, které jsou podporovány v hello modelu nasazení classic i Resource Manager.

> [!NOTE]
> Zakázání Azure koncového bodu nijak nesouvisí toodo s jeho stavem nasazení v Azure. Azure služby (například webové aplikace nebo virtuální počítač zůstane spuštěný a mít tooreceive provoz i v případě, že v Traffic Manageru zakázán. Přenosy lze řešit přímo toohello služby instance nikoli prostřednictvím hello Traffic Manager profilu název DNS. Další informace najdete v tématu [fungování Traffic Manager](traffic-manager-how-traffic-manager-works.md).

aktuální vhodnost Hello každý koncový bod tooreceive přenosy, závisí na hello následující faktory:

* Stav profilu Hello (povoleno nebo zakázáno)
* Stav koncového bodu Hello (povoleno nebo zakázáno)
* Hello výsledky kontroly stavu hello tohoto koncového bodu

Podrobnosti najdete v tématu [monitorování koncového bodu Traffic Manager](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Traffic Manager funguje v hello úroveň DNS, proto je nelze tooinfluence existující připojení tooany koncový bod. Když koncový bod není k dispozici, Traffic Manager určí, že nové připojení tooanother dostupný koncový bod. Hostitele hello za hello zakázána nebo není v pořádku koncový bod však může pokračovat tooreceive provoz prostřednictvím připojení k existující, dokud tyto relace jsou ukončeny. Aplikace měli omezit hello relace trvání tooallow provoz toodrain z existující připojení.

Pokud jsou zakázány všechny koncové body v profilu, nebo pokud je zakázán profil hello, samotné, Traffic Manager odešle 'NXDOMAIN' odpovědi tooa nové DNS dotaz.


## <a name="next-steps"></a>Další kroky

* Další informace [fungování Traffic Manager](traffic-manager-how-traffic-manager-works.md).
* Další informace o Traffic Manager [koncového bodu monitorování a automatické převzetí služeb při selhání](traffic-manager-monitoring.md).
* Další informace o Traffic Manager [metodách směrování provozu](traffic-manager-routing-methods.md).
