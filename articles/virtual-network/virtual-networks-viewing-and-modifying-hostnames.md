---
title: "aaaViewing a úprava názvy hostitelů | Microsoft Docs"
description: "Jak tooview a změňte názvy hostitelů pro virtuální počítače Azure, webové a rolí pracovního procesu pro překlad"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 17d0dd7911754a94db3f37b924b4687da1c70aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="viewing-and-modifying-hostnames"></a>Zobrazení a úprava názvy hostitelů
tooallow vaše toobe instancí role odkazuje název hostitele, je nutné nastavit hello hodnotu pro název hostitele hello v konfiguračním souboru služby hello pro každou roli. To uděláte tak, že přidáte hello požadovaného hostitele název toohello **vmName** atribut hello **Role** elementu. Hello hodnotu hello **vmName** atribut slouží jako základ pro název hostitele hello každá instance role. Například pokud **vmName** je *webrole* a jsou tři instance dané role, bude názvy hostitelů hello instancí hello *webrole0*, *webrole1* , a *webrole2*. Není nutné toospecify název hostitele pro virtuální počítače v konfiguračním souboru hello, protože je vyplněný hello název hostitele pro virtuální počítač, na základě názvu virtuálního počítače hello. Další informace o konfiguraci služby Microsoft Azure najdete v tématu [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Zobrazení názvů hostitelů
Hello názvy hostitelů virtuálních počítačů a instancí rolí můžete zobrazit v cloudové službě pomocí některé z hello nástrojů uvedených dole.

### <a name="azure-portal"></a>Azure Portal
Můžete použít hello [portál Azure](http://portal.azure.com) tooview hello názvy hostitelů pro virtuální počítače v okně Přehled hello pro virtuální počítač. Mějte na paměti, která hello okno zobrazuje hodnotu pro **název** a **název hostitele**. I když jsou původně hello stejné, změna názvu hostitele hello nedojde ke změně názvu hello hello virtuálního počítače nebo role instance.

Instance rolí lze také zobrazit v hello portálu Azure, ale když můžete seznam instancí hello v cloudové službě, název hostitele hello se nezobrazí. Zobrazí se název pro každou instanci, ale tento název nepředstavuje hello název hostitele.

### <a name="service-configuration-file"></a>Konfigurační soubor služby
Hello konfigurační soubor služby pro službu nasazenou si můžete stáhnout z hello **konfigurace** okno služby hello v hello portálu Azure. Potom můžete vyhledat hello **vmName** atribut pro hello **název Role** název hostitele hello toosee elementu. Uvědomte si, že tento název hostitele slouží jako základ pro název hostitele hello každá instance role. Například pokud **vmName** je *webrole* a jsou tři instance dané role, bude názvy hostitelů hello instancí hello *webrole0*, *webrole1* , a *webrole2*.

### <a name="remote-desktop"></a>Vzdálená plocha
Po povolení vzdálené plochy (Windows), vzdálené komunikace Windows Powershellu (Windows), nebo SSH (Linux a Windows) připojení tooyour virtuálních počítačů nebo instancí rolí, můžete zobrazit název hostitele hello z aktivního připojení vzdálené plochy různými způsoby:

* Zadejte název hostitele hello příkazový řádek nebo SSH terminálu.
* Zadáním ipconfig/vše na hello příkazového řádku (jenom Windows).
* Název počítače hello zobrazení v systému hello nastavení (jenom Windows).

### <a name="azure-service-management-rest-api"></a>Rozhraní REST API pro správu služby Azure
Z klienta REST postupujte podle těchto pokynů:

1. Ujistěte se, že máte toohello tooconnect certifikát klienta portálu Azure. tooobtain klientský certifikát, postupujte podle kroků hello uvedené v [postupy: stahování a Import nastavení publikování a informace o předplatném](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Nastavte položku záhlaví s názvem x-ms-version s hodnotou 2013-11-01.
3. Poslat žádost o hello následující formát: https://management.core.windows.net/\<subscrition id\>/services/hostedservices/\<název služby\>? vložení podrobností = true
4. Vyhledejte hello **HostName** element pro každou **RoleInstance** element.

> [!WARNING]
> Můžete také zobrazit příponu hello interní domény pro cloudové služby z hello odpovědi volání REST kontrolou hello **InternalDnsSuffix** element, nebo spuštěním ipconfig/všechny z příkazového řádku v relaci vzdálené plochy (Windows) nebo spuštěné cat /etc/resolv.conf z Terminálové SSH (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Úprava název hostitele
Název hostitele hello u virtuálního počítače nebo role instance můžete upravit tím, že nahrajete konfigurační soubor upravené služby nebo přejmenováním hello počítače z relace vzdálené plochy.

## <a name="next-steps"></a>Další kroky
[Překlad adres (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Schéma konfigurace služby Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Schéma konfigurace virtuální sítě Azure](http://go.microsoft.com/fwlink/?LinkId=248093)

[Zadejte nastavení DNS pomocí konfiguračních souborů síť](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

