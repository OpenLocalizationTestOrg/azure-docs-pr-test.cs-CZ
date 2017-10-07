---
title: "Podpora brány firewall Cosmos DB aaaAzure & IP řízení přístupu | Microsoft Docs"
description: "Zjistěte, jak zásady řízení přístupu toouse IP brány firewall podporují v Azure Cosmos DB databáze účtů."
keywords: "Řízení přístupu IP, podporu brány firewall"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ankshah
ms.openlocfilehash: b5cdbdb28e9d7ee0fd0ea54aad277167b699929f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-firewall-support"></a>Podpora brány firewall Azure Cosmos DB
toosecure data uložená v účtu Azure Cosmos DB databáze, Azure Cosmos DB poskytl podporu pro tajný klíč na základě [modelu autorizace](https://msdn.microsoft.com/library/azure/dn783368.aspx) , využívá ověřovací kód silné Hash-based zprávy (HMAC). Nyní kromě tajný klíč toohello na základě modelu autorizace, Azure Cosmos DB podporuje zásady řízené řízení přístupu na základě IP pro podporu brány firewall pro příchozí. Tento model je velmi podobné toohello pravidla brány firewall systému tradiční databáze a poskytuje další úroveň zabezpečení toohello databázový účet Azure Cosmos DB. Tento model můžete teď konfigurovat dostupný jenom z schválené sadu počítačů Azure Cosmos DB toobe účtu databáze nebo cloudové služby. Přístup k prostředkům Cosmos DB tooAzure ze tyto schválené sady počítačů a služeb stále vyžadují hello volající toopresent token platný autorizace.

## <a name="ip-access-control-overview"></a>Přehled řízení přístupu IP
Ve výchozím nastavení je přístupné z veřejného Internetu účet Azure Cosmos DB databáze, tak dlouho, dokud hello žádosti je přiložena token platný autorizace. řízení přístupu na základě zásad tooconfigure IP, hello uživatel musí poskytnout sadu hello IP adresy nebo rozsahy IP adres v toobe formuláře CIDR zahrnuty jako hello klienta IP adres pro danou databázi účet seznamu povolených aplikací. Jakmile tato konfigurace se použije, všechny požadavky z počítače mimo tento seznam povolených se zablokuje serverem hello.  připojení Hello zpracování tok pro hello řízení přístupu na základě IP je popsáno v následujícím diagramu hello.

![Diagram zobrazující hello proces připojení u řízení přístupu na základě IP](./media/firewall-support/firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Připojení z cloudové služby
Cloudové služby v Azure, jsou velmi běžný způsob, jak hostování logikou střední úrovně služby pomocí Azure Cosmos DB. tooenable přístup tooan databázový účet Azure Cosmos DB z cloudové služby, hello veřejnou IP adresu hello cloudové služby musí být přidané toohello povolené seznam IP adres, které jsou spojené s vaším účtem Azure Cosmos DB databáze pomocí [konfigurace Hello zásad řízení přístupu IP](#configure-ip-policy).  To zajistí, že všechny instance role cloudové služby mít přístup tooyour databázový účet Azure Cosmos DB. IP adresy můžete načíst pro vaše cloudové služby v hello portál Azure, jak ukazuje následující snímek obrazovky hello.

![Snímek obrazovky zobrazující hello veřejné IP adresy pro cloudové služby zobrazí v hello portálu Azure](./media/firewall-support/public-ip-addresses.png)

Když cloudové služby můžete škálovat tak, že přidáte další role instance, tyto nové instance bude automaticky mít účet přístupu k toohello Azure Cosmos DB databáze vzhledem k tomu, že jsou součástí hello stejné cloudové služby.

## <a name="connections-from-virtual-machines"></a>Připojení z virtuálních počítačů
[Virtuální počítače](https://azure.microsoft.com/services/virtual-machines/) nebo [sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) lze také použít toohost střední vrstvy služeb pomocí Azure Cosmos DB.  tooconfigure hello Azure Cosmos DB databázový účet tooallow přístup z virtuálních počítačů, veřejné IP adresy virtuálního počítače nebo škálovací sadu virtuálních počítačů musí být nakonfigurován jako jeden z hello povolené IP adresy pro váš účet Azure Cosmos DB databáze pomocí [konfigurace zásad řízení přístupu IP hello](#configure-ip-policy). IP adresy pro virtuální počítače v hello portál Azure, můžete načíst, jak ukazuje následující snímek obrazovky hello.

![Snímek obrazovky s veřejnou IP adresu pro virtuální počítač, který se zobrazí v hello portálu Azure](./media/firewall-support/public-ip-addresses-dns.png)

Když přidáte další virtuální počítač instancí toohello skupiny, jsou automaticky poskytnuty přístup tooyour Azure Cosmos DB databázového účtu.

## <a name="connections-from-hello-internet"></a>Hello připojení z Internetu
Při přístupu Azure DB Cosmos databáze účtu z počítače na hello internet, hello klienta IP adresu nebo rozsah IP adres hello počítače musí být přidané toohello povolené seznam IP adresu pro hello databázový účet Azure Cosmos DB. 

## <a id="configure-ip-policy"></a>Konfigurace zásad řízení přístupu IP hello
zásada řízení přístupu IP Hello lze nastavit v hello portál Azure, nebo programově pomocí [rozhraní příkazového řádku Azure](cli-samples.md), [prostředí Azure Powershell](powershell-samples.md), nebo hello [REST API](/rest/api/documentdb/) aktualizací hello `ipRangeFilter` vlastnost. IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery. Příklad: "13.91.6.132,13.91.6.1/24". Při aktualizaci databázový účet prostřednictvím těchto metod, musí být jisti toopopulate všechny tooprevent vlastnosti hello resetování toodefault nastavení.

> [!NOTE]
> Povolením IP zásad řízení přístupu pro váš účet Azure Cosmos DB databáze povoleny všechny tooyour přístup k databázi Azure Cosmos databázového účtu z počítače mimo nakonfigurovaný hello seznam rozsahů IP adres jsou zablokované. Na základě tento model procházení hello datové roviny operace z portálu hello budou i blokované tooensure hello integritu řízení přístupu.

vývoj toosimplify hello portál Azure vám pomůže identifikovat a přidejte IP hello toohello počítač vašeho klienta povoleny seznamu, aby aplikace běžící počítači přístup hello účet Azure Cosmos DB. Všimněte si, že hello IP adresa klienta zde byla nalezena registrovaného portálem hello. Může být IP adresa klienta hello počítače, ale mohou být také hello IP adresu brány sítě. Nevynechali tooremove ho před tooproduction probíhající.

zásada řízení tooset hello IP přístup v hello portál Azure, přejděte okně účtu Azure Cosmos DB toohello, klikněte na **brány Firewall** v navigační nabídce hello klikněte **ON** 

![Snímek obrazovky ukazující, jak tooopen hello okno brány Firewall v hello portálu Azure](./media/firewall-support/azure-portal-firewall.png)

V nové podokno hello, určete, zda hello portálu Azure můžete přístup k účtu hello a podle potřeby přidat další adresy a rozsahy a pak klikněte na tlačítko **Uložit**.  

> [!NOTE]
> Když povolíte IP zásad řízení přístupu, musíte tooadd hello IP adresa pro přístup k portálu Azure toomaintain hello. Hello portálu IP adresy jsou:
> |Oblast|IP adresa|
> |------|----------|
> |Všechny oblasti s výjimkou těchto zadané níže| 104.42.195.92|
> |Německo|51.4.229.218|
> |Čína|139.217.8.252|
> |USA (Gov) – Arizona|52.244.48.71|
>

![Snímek obrazovky znázorňující nastavení brány firewall tooconfigure v hello portálu Azure](./media/firewall-support/azure-portal-firewall-configure.png)

## <a name="troubleshooting-hello-ip-access-control-policy"></a>Řešení potíží s zásad řízení přístupu IP hello
### <a name="portal-operations"></a>Operace portálu
Povolením IP zásad řízení přístupu pro váš účet Azure Cosmos DB databáze povoleny všechny tooyour přístup k databázi Azure Cosmos databázového účtu z počítače mimo nakonfigurovaný hello seznam rozsahů IP adres jsou zablokované. Proto pokud chcete, aby operace roviny tooenable portálu data, jako je procházení kolekcí a dokumenty dotaz, je třeba tooexplicitly povolit Azure přístup k portálu pomocí hello **brány Firewall** okna portálu hello. 

![Snímek obrazovky ukazující, jak toohello tooenable přístup k portálu Azure](./media/firewall-support/azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>Sadu SDK a Rest API
Pro z bezpečnostních důvodů, přístup prostřednictvím sady SDK nebo REST API z počítače není na seznamu povolených aplikací hello vrátí obecné 404 nebyl nalezen odpověď se žádné další podrobnosti. Zkontrolujte, zda že text hello IP povolená, seznam nakonfigurovaný pro Azure Cosmos DB databáze konfiguraci vašeho účtu tooensure hello správné zásady je použité tooyour databázový účet Azure Cosmos DB.

## <a name="next-steps"></a>Další kroky
Informace o síti tipy pro zvýšení výkonu související, najdete v článku [tipy pro zvýšení výkonu](performance-tips.md).

