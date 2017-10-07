---
title: "aaaResource Manager a nasazení classic | Microsoft Docs"
description: "Popisuje model nasazení hello rozdíly mezi hello modelu nasazení Resource Manager a hello classic (nebo Správa služby)."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7ae0ffa3-c8da-4151-bdcc-8f4f69290fb4
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: tomfitz
ms.openlocfilehash: fbf1959991b100547a459bf88a29c0afbc8592e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-hello-state-of-your-resources"></a>Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků
V tomto tématu se dozvíte o Azure Resource Manageru a model nasazení classic, hello stav svých prostředků, a proto vaše prostředky, které byly nasazeny s jedním nebo hello jiné. Hello Resource Manager a modely nasazení classic představují dva různé způsoby nasazení a správě řešení Azure. Práce s nimi prostřednictvím dvě rozhraní API sady a hello nasazení prostředků může obsahovat důležitých rozdílů. dva modely Hello nejsou vzájemně zcela kompatibilní. Toto téma popisuje tyto rozdíly.

toosimplify hello nasazení a správu prostředků, společnost Microsoft doporučuje použít Správce prostředků pro všechny nové prostředky. Pokud je to možné Microsoft doporučuje, znovu nasaďte existujících prostředků prostřednictvím Resource Manager.

Pokud jste nový tooResource správce, může být vhodné toofirst zkontrolujte hello terminologie definované v hello [přehled Azure Resource Manageru](resource-group-overview.md).

## <a name="history-of-hello-deployment-models"></a>Historie nasazení modelů hello
Azure původně zadat pouze hello modelu nasazení classic. V tomto modelu všechny prostředky existovaly nezávisle; nebyl způsob toogroup související prostředky společně. Místo toho musíte toomanually sledovat prostředky, ke kterým skládá řešení nebo aplikace a zapamatovat si toomanage je v koordinovaný přístup. toodeploy řešení, bylo tooeither vytvořit každého prostředku jednotlivě prostřednictvím portálu classic hello nebo skript, který nasadit všechny prostředky hello ve správném pořadí hello. toodelete řešení, bylo toodelete každého prostředku jednotlivě. Není snadno můžete použít a aktualizovat zásady řízení přístupu pro související prostředky. Nakonec nelze aplikovat značky tooresources toolabel je s podmínkami, které vám pomůžou sledovat vaše prostředky a spravovat fakturace.

V roce 2014 si uvedla Azure Resource Manager, která přidá hello konceptu skupinu prostředků. Skupina prostředků je kontejner pro prostředky, které sdílejí společné životního cyklu. Hello modelu nasazení Resource Manager poskytuje několik výhod:

* Můžete nasadit, spravovat a monitorovat všechny hello služby pro vaše řešení jako skupina, nikoli samostatně zpracování těchto služeb.
* Můžete opakovaně nasadit řešení v průběhu životního cyklu a mít přitom jistotu, že vaše prostředky jsou nasazeny v konzistentním stavu.
* Přístup k řízení tooall prostředkům můžete použít ve vaší skupině prostředků a tyto zásady budou automaticky použita při přidávání nových prostředků toohello skupinu prostředků.
* Můžete použít značky tooresources toologically uspořádat všechny prostředky hello ve vašem předplatném.
* JavaScript Object Notation (JSON) toodefine hello infrastruktury můžete použít pro vaše řešení. soubor JSON Hello se označuje jako šablony Resource Manageru.
* Můžete definovat hello závislosti mezi prostředky, takže se nasadí ve správném pořadí hello.

Pokud jste přidali Resource Manager, všechny prostředky byly přidány zpětně toodefault skupiny prostředků. Pokud vytvoříte prostředek prostřednictvím teď nasazení classic, hello prostředků se automaticky vytvoří ve výchozí skupině prostředků pro tuto službu, i když jste nezadali příslušné skupině prostředků v nasazení. Ale že právě existující ve skupině prostředků neznamená, že hello prostředku byl převeden toohello modelu Resource Manager. Podíváme jak každá služba zpracovává hello dva modely nasazení v další části hello. 

## <a name="understand-support-for-hello-models"></a>Pochopení podporu pro modely hello
Při rozhodování, které toouse model nasazení pro vaše prostředky, existují tři scénáře toobe vědět:

1. Služba Hello podporuje Resource Manager a obsahuje pouze jeden typ.
2. Hello služba podporuje Resource Manager ale nabízí dva typy – jeden pro Resource Manager a jeden pro classic. Tento scénář se vztahuje pouze toovirtual počítače, účty úložiště a virtuální sítě.
3. Služba Hello nepodporuje Resource Manager.

toodiscover jestli služba podporuje Resource Manager, najdete v části [zprostředkovatelé prostředků a typy](resource-manager-supported-services.md).

Pokud chcete toouse služby hello nepodporuje Resource Manager, musí pokračovat, pomocí nasazení classic.

Pokud služba hello podporuje Resource Manager a **není** virtuální počítač, účet úložiště nebo virtuální sítě, správce prostředků lze použít bez jakékoli komplikace.

Pro virtuální počítače, účty úložiště a virtuální sítě Pokud hello prostředek se vytvořil prostřednictvím nasazení classic, musíte dál toooperate na něm prostřednictvím klasické operace. Resource Manager operations musí používat hello virtuální počítač, účet úložiště nebo virtuální sítě vytvořený prostřednictvím nasazení Resource Manager. Pokud vaše předplatné obsahuje kombinaci prostředky vytvořené prostřednictvím Resource Manager a nasazení classic, můžete získat tento rozdíl matoucí. Tato kombinace prostředků můžete vytvořit neočekávané výsledky, protože prostředky hello nepodporují hello stejné operace.

V některých případech se příkaz Resource Manager můžete načíst informace o zdroji, vytvořené pomocí nasazení classic, nebo můžete při správě například přesun skupiny prostředků tooanother klasický prostředek. Ale těchto případech by neměl poskytnout hello dojem, že typ hello podporuje operace Resource Manager. Předpokládejme například, že máte skupinu prostředků, která obsahuje virtuální počítač, který byl vytvořen s nasazení classic. Pokud spustíte následující příkaz prostředí PowerShell Resource Manager hello:

```powershell
Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines
```

Vrátí hello virtuálního počítače:

```powershell
Name              : ExampleClassicVM
ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
ResourceName      : ExampleClassicVM
ResourceType      : Microsoft.ClassicCompute/virtualMachines
ResourceGroupName : ExampleGroup
Location          : westus
SubscriptionId    : {guid}
```

Ale hello rutiny správce prostředků **Get-AzureRmVM** vrátí pouze virtuální počítače nasazené prostřednictvím Resource Manager. Následující příkaz Hello nevrací hello virtuálního počítače vytvořena prostřednictvím nasazení classic.

```powershell
Get-AzureRmVM -ResourceGroupName ExampleGroup
```

Pouze prostředky vytvořené pomocí značek podporu správce prostředků. Nelze použít značky tooclassic prostředky.

## <a name="resource-manager-characteristics"></a>Vlastnosti Správce prostředků
toohelp pochopit hello dva modely, pojďme si hello charakteristiky typů Resource Manager:

* Vytvořena prostřednictvím hello [portál Azure](https://portal.azure.com/).
  
     ![portál Azure](./media/resource-manager-deployment-model/portal.png)
  
     Pro výpočty, úložiště a síťové prostředky máte možnost hello pomocí nasazení Resource Manager nebo Classic. Vyberte **správce prostředků**.
  
     ![Nasazení Resource Manager](./media/resource-manager-deployment-model/select-resource-manager.png)
* Vytvořen hello Resource Manager verzi hello rutin prostředí Azure PowerShell. Tyto příkazy mají hello formátu *příkaz AzureRmNoun*.

  ```powershell
  New-AzureRmResourceGroupDeployment
  ```

* Vytvořena prostřednictvím hello [REST API služby Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) pro operace REST.
* Vytvořené pomocí rozhraní příkazového řádku Azure spustit v hello **arm** režimu.
  
  ```azurecli
  azure config mode arm
  azure group deployment create
  ```

* Typ prostředku Hello nezahrnuje **(klasické)** v názvu hello. Hello následující obrázek ukazuje typ hello jako **účet úložiště**.
  
    ![web app](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Vlastnosti nasazení Classic
Můžete také vědět modelu nasazení classic hello jako model Service Management hello.

Prostředky vytvořené v hello nasazení classic modelu sdílené složky hello následující vlastnosti:

* Vytvořena prostřednictvím hello [portál classic](https://manage.windowsazure.com)
  
     ![Portál Classic](./media/resource-manager-deployment-model/classic-portal.png)
  
     Nebo, hello portál Azure a zadáte **Classic** nasazení (pro výpočty, úložiště a sítě).
  
     ![Nasazení Classic](./media/resource-manager-deployment-model/select-classic.png)
* Vytvořena prostřednictvím verze Service Management hello hello rutin prostředí Azure PowerShell. Tyto příkazu názvy mají formát hello *příkaz AzureNoun*.

  ```powershell
  New-AzureVM
  ```

* Vytvořena prostřednictvím hello [REST API pro správu služby](https://msdn.microsoft.com/library/azure/ee460799.aspx) pro operace REST.
* Vytvořené pomocí rozhraní příkazového řádku Azure spustit **asm** režimu.

  ```azurecli
  azure config mode asm
  azure vm create
  ```
   
* Typ prostředku Hello zahrnuje **(klasické)** v názvu hello. Hello následující obrázek ukazuje typ hello jako **účet úložiště (klasické)**.
  
    ![klasického typu](./media/resource-manager-deployment-model/classic-type.png)

Můžete použít hello Azure portálu toomanage prostředky, které byly vytvořeny prostřednictvím nasazení classic.

## <a name="changes-for-compute-network-and-storage"></a>Změny pro výpočty, síť a úložiště
Hello následující diagram zobrazuje výpočty, síť a úložiště prostředky nasazené prostřednictvím Resource Manager.

![Správce prostředků – architektura](./media/resource-manager-deployment-model/arm_arch3.png)

Vezměte na vědomí následující vztahy mezi prostředky hello hello:

* Všechny prostředky hello existovat v rámci skupiny prostředků.
* Hello virtuálního počítače závisí na konkrétní úložiště účet definované v toostore poskytovatele prostředků úložiště hello jeho disky v objektu blob úložiště (povinné).
* virtuální počítač Hello odkazuje na konkrétní síťový adaptér definované v hello poskytovatele síťových prostředků (povinné) a dostupnosti nastavení definované v zprostředkovateli hello výpočetních prostředků (volitelné).
* Hello seskupování odkazy hello virtuálního počítače přiřazen IP adresa (povinné), podsíť hello hello virtuální sítě pro virtuální počítač hello (povinné) a tooa skupinu zabezpečení sítě (volitelné).
* Hello podsítí v rámci virtuální sítě odkazuje na skupinu zabezpečení sítě (volitelné).
* instance služby Vyrovnávání zatížení Hello odkazuje hello back-endový fond IP adres, které zahrnují hello síťový adaptér virtuálního počítače (volitelné) a odkazuje adrese služby Vyrovnávání zatížení veřejných nebo privátních IP (volitelné).

Zde jsou komponenty hello a jejich vztahů pro nasazení classic:

![Architektura Classic](./media/resource-manager-deployment-model/arm_arch1.png)

Hello classic řešení pro hostování virtuálního počítače zahrnuje:

* Požadované Cloudová služba, která slouží jako kontejner pro hostování virtuálních počítačů (výpočetní). Virtuální počítače jsou automaticky součástí síťová karta (NIC) a IP adresu přiřadila Azure. Kromě toho hello Cloudová služba obsahuje instance nástroje pro vyrovnávání zatížení externí, veřejnou IP adresu a výchozí koncové body tooallow Vzdálená plocha a Vzdálená prostředí PowerShell provozu pro virtuální počítače se systémem Windows a provoz Secure Shell (SSH) pro systémem Linux virtuální počítače.
* Účet úložiště vyžaduje, aby úložiště hello virtuální pevné disky pro virtuální počítač, včetně hello operačního systému, dočasné a další datové disky (úložiště).
* Volitelné virtuální sítě, který funguje jako další kontejner, ve kterém můžete vytvořit strukturu podsítěmi a určit hello podsíť, ve které hello je umístěn virtuální počítač (síť).

Hello následující tabulka popisuje změny v interakci poskytovatele prostředků výpočty, síť a úložiště:

| Položka | Classic | Resource Manager |
| --- | --- | --- |
| Cloudová služba pro službu Virtual Machines |Cloudová služba byla kontejnerem pro uložení hello virtuálních počítačů, která vyžadovala dostupnost z hello platformy a vyrovnávání zatížení. |Cloudová služba už není objektem vyžadovaným pro vytvoření virtuálního počítače pomocí nového modelu hello. |
| Virtuální sítě |Virtuální síť je volitelný hello virtuálního počítače. Pokud zahrnuty, hello virtuální sítě, nelze nasadit pomocí Správce prostředků. |Virtuální počítač vyžaduje virtuální síť, která byla nasazena s Resource Managerem. |
| Účty úložiště |Hello virtuální počítač vyžaduje účet úložiště, který ukládá virtuální pevné disky hello pro hello operačního systému, dočasné a dalších datových disků. |Hello virtuální počítač vyžaduje toostore účet úložiště jeho disky v úložišti objektů blob. |
| Skupiny dostupnosti |Dostupnost toohello platformu byla označovaná konfigurací stejného parametru "AvailabilitySetName" na hello virtuální počítače hello. Hello maximální počet domén selhání byl 2. |Skupina dostupnosti je prostředek vystavený poskytovatelem Microsoft.Compute. Virtuální počítače, které vyžadují vysokou dostupnost, musí být součástí hello sady dostupnosti. maximální počet domén selhání Hello je teď 3. |
| Skupiny vztahů |Skupiny vztahů byly nezbytné k vytváření služeb Virtual Network. Nicméně s hello zavedením regionálních virtuálních sítí, který nebyl požadován už. |toosimplify, hello koncept skupin vztahů neexistuje v hello rozhraní API vystavené prostřednictvím Správce Azure Resource Manager. |
| Vyrovnávání zatížení |Vytvoření cloudové služby poskytuje k nástroji pro vyrovnávání zatížení implicitní pro hello virtuální počítače nasazené. |Hello nástroj pro vyrovnávání zatížení je prostředek vystavený poskytovatelem Microsoft.Network hello. Hello primární síťové rozhraní služeb hello virtuálních počítačů, které potřebuje Vyrovnávání zatížení toobe musí odkazovat na nástroj pro vyrovnávání zatížení hello. Nástroje pro vyrovnávání zatížení můžou být interní nebo externí. Na instanci služby Vyrovnávání zatížení odkazuje hello back-endový fond IP adres, které zahrnují hello síťový adaptér virtuálního počítače (volitelné) a odkazuje adrese služby Vyrovnávání zatížení veřejných nebo privátních IP (volitelné). [Další informace.](../virtual-network/resource-groups-networking.md) |
| Virtuální IP adresa |Cloudové služby získat výchozí VIP (virtuální IP adresy), když virtuální počítač se přidá tooa cloudové služby. Hello virtuální IP adresa je adresa hello přidružené hello implicitní vyrovnávání zátěže. |Veřejná IP adresa je prostředek vystavený poskytovatelem Microsoft.Network hello. Veřejná IP adresa může být statická (vyhrazená) nebo dynamická. Dynamické veřejné IP adresy lze přiřadit tooa nástroj pro vyrovnávání zatížení. Veřejné IP adresy můžete zabezpečit pomocí skupin zabezpečení. |
| Vyhrazená IP adresa |Je možné rezervovat IP adresu v Azure a přidružit ji s tooensure Cloudová služba, která hello IP adresa zůstane dynamická. |Veřejnou IP adresu lze vytvořit v režimu "Statická" a hello nabízí stejné funkce jako "Vyhrazená IP adresa". Statické veřejné IP adresy lze přiřadit pouze nástroj pro vyrovnávání zatížení tooa hned teď. |
| Veřejná IP adresa (PIP) na virtuální počítač |Veřejné IP adresy může být také přidružené tooa virtuálních počítačů přímo. |Veřejná IP adresa je prostředek vystavený poskytovatelem Microsoft.Network hello. Veřejná IP adresa může být statická (vyhrazená) nebo dynamická. Jenom dynamické veřejné IP adresy však může být přiřazené tooa síťové rozhraní tooget veřejnou IP adresu na virtuální počítač ihned. |
| Koncové body |Vstupní koncové body potřeby toobe nakonfigurovaný na virtuálním počítači toobe otevře připojení pro určité porty. Jeden z běžných režimů hello připojení toovirtual počítačům se provádí nastavením vstupní koncových bodů. |Příchozí pravidla NAT se dá konfigurovat na nástroje pro vyrovnávání zatížení tooachieve hello stejné možnosti povolování koncových bodů na konkrétních portech za účelem připojení toohello virtuálních počítačů. |
| Název DNS |Cloudová služba by získala implicitní, globálně jedinečný název DNS. Například: `mycoffeeshop.cloudapp.net`. |Názvy DNS jsou volitelné parametry, které můžete nastavit na prostředku veřejné IP adresy. Hello plně kvalifikovaný název domény je v hello následující formát: `<domainlabel>.<region>.cloudapp.azure.com`. |
| Síťová rozhraní |Primární a sekundární síťové rozhraní a jeho vlastnosti byly definované jako síťová konfigurace virtuálního počítače. |Síťové rozhraní je prostředek vystavený poskytovatelem Microsoft.Network. životní cyklus Hello hello síťového rozhraní není vázaný tooa virtuálního počítače. Odkazuje na hello virtuálního počítače přiřazenou IP adresu (povinné), podsíť hello hello virtuální sítě pro virtuální počítač hello (povinné) a tooa skupinu zabezpečení sítě (volitelné). |

toolearn o připojení virtuální sítě z různé modely nasazení, najdete v části [připojení virtuální sítě z různé modely nasazení portálu hello](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-tooresource-manager"></a>Migrace z klasického tooResource Manager
Pokud jste připravené toomigrate vaše prostředky z nasazení classic tooResource Manager deployment, najdete v části:

1. [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-deep-dive.md)
2. [Platforma podporovaná migrace prostředky infrastruktury z klasického tooAzure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-overview.md)
3. [Migraci prostředky infrastruktury z classic tooAzure správce prostředků pomocí Azure PowerShell](../virtual-machines/windows/migration-classic-resource-manager-ps.md)
4. [Migrovat prostředky infrastruktury z classic tooAzure Resource Manager pomocí rozhraní příkazového řádku Azure](../virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**Můžete vytvořit virtuální počítač pomocí Azure Resource Manager toodeploy ve virtuální síti vytvořené pomocí klasického nasazení?**

To není podporováno. Azure Resource Manager toodeploy virtuální počítač nelze použít do virtuální sítě, která byla vytvořena pomocí nasazení classic.

**Můžete vytvořit virtuální počítač pomocí hello Azure Resource Manager z uživatelského image, která byla vytvořena pomocí hello rozhraní Azure Service Management API?**

To není podporováno. Však můžete zkopírovat soubory virtuálního pevného disku hello z účtu úložiště, který byl vytvořen pomocí hello rozhraní API pro správu služby a přidat je tooa nový účet vytvořený prostřednictvím Správce Azure Resource Manager.

**Co je hello dopad na hello kvótu pro Moje předplatné?**

Hello kvóty pro hello virtuálních počítačů, virtuálních sítí a účty úložiště vytvořené prostřednictvím hello Azure Resource Manager jsou oddělené od dalších kvóty. Každé předplatné získá kvóty toocreate hello prostředků pomocí hello nových rozhraní API. Další informace o dodatečných kvótách hello [zde](../azure-subscription-service-limits.md).

**Můžete pokračovat v toouse své automatizované skripty pro zřizování virtuálních počítačů, virtuálních sítí a účty úložiště prostřednictvím hello rozhraní API Resource Manager?**

Všechny hello automatizace a skripty, které jste vytvořili pokračovat toowork hello existující virtuální počítače, virtuální sítě vytvořené v režimu Azure Service Management hello. Skripty hello však mít toobe aktualizované toouse hello nové schéma pro vytváření stejných prostředků prostřednictvím režimu Resource Manager hello hello.

**Kde najdu Příklady šablon Azure Resource Manager?**

Ucelenou sadu úvodních šablon najdete na [rychlých šablonách správce Azure Resource Manager](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Další kroky
* toowalk prostřednictvím hello vytvoření šablony, která definuje virtuální počítač, účet úložiště a virtuální síti, najdete v tématu [názorný Průvodce šablonou Resource Manageru](resource-manager-template-walkthrough.md).
* příkazy hello toosee pro nasazení šablony, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

