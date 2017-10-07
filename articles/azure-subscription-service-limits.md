---
title: "předplatné aaaAzure omezení a kvóty | Microsoft Docs"
description: "Poskytuje seznam běžných předplatného Azure a omezení služby, kvóty a omezení. To zahrnuje informace o tom, jak tooincrease omezuje spolu s maximální hodnoty."
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: byvinyal
ms.openlocfilehash: a754d56124520791254ab8f1729808f0750ff222
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Limity, kvóty a omezení předplatného a služeb Azure
Tento dokument uvádí některé z hello nejběžnější Microsoft Azure omezení, což se taky někdy označují jako kvóty. Tento dokument nepokrývá aktuálně všech služeb Azure. V průběhu času hello seznam bude rozšířena a aktualizovat toocover další platformy hello.

Navštivte [ceny přehled Azure](https://azure.microsoft.com/pricing/) toolearn informace o cenách Azure. Zde můžete odhadnout náklady na používání hello [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) nebo návštěvou hello ceny stránce s podrobnostmi o službu (například [virtuálních počítačů Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Pro tipy toohelp spravovat vaše náklady, najdete v části [zabránit neočekávané náklady s Azure fakturace a náklady na správu](billing/billing-getting-started.md).

> [!NOTE]
> Pokud chcete tooraise hello limit nebo kvóty výše hello **výchozí Limit**, [otevřete žádosti o podporu online zákazníka zdarma](azure-supportability/resource-manager-core-quotas-request.md). Hello omezení nelze zvýšit nad hello **maximální Limit** hodnota použitá v hello následující tabulky. Pokud není žádná **maximální Limit** sloupce, pak hello prostředků nemá nastavitelná omezení. 
> 
> Bezplatné předplatné zkušební verze nejsou způsobilé k omezení nebo zvyšuje kvóty. Pokud máte bezplatnou zkušební verzi, můžete upgradovat tooa [průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) předplatné. Další informace najdete v tématu [Upgrade bezplatné zkušební verze Azure tooPay jako-vám-přejděte](billing/billing-upgrade-azure-subscription.md).
> 

## <a name="limits-and-hello-azure-resource-manager"></a>Omezení a hello Azure Resource Manager
Nyní je možné toocombine několik prostředků Azure v tooa jedna skupina prostředků Azure. Při použití skupin prostředků, omezení, které byly jednou globální spravovanou na místní úrovni s hello Azure Resource Manager. Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](azure-resource-manager/resource-group-overview.md).

V omezení hello pod novou tabulku byl přidaný tooreflect případné rozdíly v omezení při použití hello Azure Resource Manager. Například, že je **limity předplatného** tabulky a **limity předplatného – Azure Resource Manager** tabulky. Pokud limit tooboth scénáře, se zobrazí pouze v první tabulce hello. Pokud není uvedeno jinak, jsou omezení globální přes všechny oblasti.

> [!NOTE]
> Je důležité tooemphasize, že jsou na oblast přístupný pro vaše předplatné kvóty pro prostředky ve skupinách prostředků Azure a nejsou za předplatné, jsou hello služby správy kvóty. Jako příklad použijeme základní kvóty. Pokud potřebujete toorequest zvýšit kvótu s podporou pro počet jader, je nutné toodecide jak velký počet jader toouse v oblasti, které chcete a proveďte konkrétního požadavku pro skupinu prostředků Azure pro základní kvóty pro hello objemy a oblastí, které chcete. Proto pokud potřebujete toouse 30 cores v oblasti západní Evropa toorun aplikace Konkrétně měli požádat o 30 jader v oblasti západní Evropa. Ale nebudete mít základní kvóta zvyšují v jiné oblasti – pouze západní Evropa budou mít 30 – základní kvóta hello.
> <!-- -->
> V důsledku toho může být užitečné tooconsider při rozhodování o tom, co vaše skupina prostředků Azure kvóty třeba toobe pro úlohy v jakékoli jedné oblasti a žádost o které částka v každé oblasti, do kterého uvažujete o nasazení. V tématu [potíží s nasazením](resource-manager-common-deployment-errors.md) další pomoc zjišťování vaše aktuální kvóty pro konkrétní oblasti.
> 
> 

## <a name="service-specific-limits"></a>Omezení specifickou pro službu
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automation](#automation-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Azure událostí mřížky](#azure-event-grid-limits)
* [Azure Redis Cache](#azure-redis-cache-limits)
* [Azure RemoteApp](#azure-remoteapp-limits)
* [Backup](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk Services](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Cloud Services](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [IoT Hub](#iot-hub-limits)
* [Key Vault](#key-vault-limits)
* [Analýza protokolu / provozní statistiky](#log-analytics-limits)
* [Media Services](#media-services-limits)
* [Mobile Engagement](#mobile-engagement-limits)
* [Mobilní služby](#mobile-services-limits)
* [Monitorování](#monitor-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Sítě](#networking-limits)
* [Sledovací proces sítě](#network-watcher-limits)
* [Služby centra oznámení](#notification-hub-service-limits)
* [Skupina prostředků](#resource-group-limits)
* [Scheduler](#scheduler-limits)
* [Search](#search-limits)
* [Service Bus](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [Úložiště](#storage-limits)
* [Systém StorSimple](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Předplatné](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Virtual Machines](#virtual-machines-limits)
* [Škálovací sady virtuálních počítačů](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Limity předplatného
#### <a name="subscription-limits"></a>Limity předplatného
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Limity předplatného – Azure Resource Manager
Hello následující omezení platí při použití hello Azure Resource Manager a skupiny prostředků Azure. Limity, které nebyly změněny s hello Azure Resource Manager nejsou uvedené níže. Naleznete v předchozí tabulce toohello těchto omezení.

Informace o zpracování omezení pro správce prostředků požadavky najdete v tématu [omezení Resource Manager požadavky](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Omezení skupiny prostředků
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limity virtuální počítače
#### <a name="virtual-machine-limits"></a>Limity virtuální počítač
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuální počítače omezení - Azure Resource Manager
Hello následující omezení platí při použití hello Azure Resource Manager a skupiny prostředků Azure. Limity, které nebyly změněny s hello Azure Resource Manager nejsou uvedené níže. Naleznete v předchozí tabulce toohello těchto omezení.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Omezení sady škálování virtuálního počítače
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Kontejner instancí omezení
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a>Síťová omezení
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Síťová omezení
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Omezuje aplikační brány
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Sledovací proces sítě omezuje
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Omezení Traffic Manageru
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>Omezení DNS
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Limity úložiště
Další informace o limity účtu úložiště najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage/common/storage-scalability-targets.md).
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a>Omezení služby úložiště
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Omezení disku virtuálního počítače 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

V tématu [velikostí virtuálních počítačů](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další podrobnosti.

#### <a name="managed-virtual-machine-disks"></a>Disky spravovaných virtuálních počítačů

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Disky nespravované virtuálního počítače

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Omezení poskytovatele prostředků úložiště
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a>Omezení cloudové služby
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>Omezení služby App Service
Následující text Hello, které omezení služby App Service zahrnují omezení pro webové aplikace, mobilní aplikace, aplikace API a Logic Apps.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Omezení
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Omezení dávky
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>Omezení služby BizTalk Services
Hello následující tabulka uvádí hello omezení služby Azure Biztalk Services.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Omezení Azure Cosmos DB
Azure Cosmos DB je globálním měřítku databáze, ve které propustnost a úložiště lze škálovat toohandle ať vaše aplikace vyžaduje. Pokud máte jakékoli dotazy týkající se hello škálování, které poskytuje Azure Cosmos DB, pošlete e-mail tooaskcosmosdb@microsoft.com.

### <a name="mobile-engagement-limits"></a>Omezení Mobile Engagement
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a>Omezení vyhledávání
Cenové úrovně určit hello kapacitu a omezení služby search. Úrovně zahrnují:

* *Volné* víceklientské služby, sdílené s další předplatitelé služby Azure, určený pro malé vývoj a testování projektů.
* *Základní* poskytuje vyhrazený výpočetní prostředky pro úlohy v produkčním prostředí v menším měřítku, s toothree repliky pro vysokou dostupnost dotazu úlohy.
* *Standard (S1, S2, S3, S3 s vysokou hustotou)* je pro větší úlohy v produkčním prostředí. Více úrovní existují v rámci úrovně standard hello, takže můžete konfiguraci prostředků, který nejlépe odpovídá váš profil zatížení.

**Omezení podle předplatného**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Omezení jednu službu vyhledávání**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

toolearn Další informace o omezení na podrobnější úrovni, jako je například velikost dokumentu, dotazů za sekundu, klíčů, požadavků a odpovědí, najdete v části [omezení ve službě Azure Search služby](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Omezení služby Media Services
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>Omezení CDN
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Omezení Mobile Services
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>Omezení monitorování
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Omezení služby centra oznámení
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Omezení centra událostí
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Omezení služby Service Bus
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Omezení služby IoT Hub
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Omezení pro vytváření dat
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Omezuje data Lake Analytics
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Omezení data Lake Store
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a>Omezuje služby Stream Analytics
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Omezuje služby Active Directory
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Azure omezení událostí mřížky
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a>Omezení Azure RemoteApp
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Omezení systému StorSimple
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Omezuje analýzy protokolů
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Zálohování omezení
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Omezení Site Recovery
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Omezení Application Insights
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Omezení API Management
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Omezení Azure Redis Cache
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Omezení Key Vault
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Omezení automatizace
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Omezení databáze SQL
Omezení SQL Database, najdete v části [limitů prostředků databáze SQL](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Viz také
[Seznámení s Azure omezení a zvyšuje](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuální počítač velikost služeb a Cloud pro Azure.](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Velikosti pro cloudové služby](cloud-services/cloud-services-sizes-specs.md)

