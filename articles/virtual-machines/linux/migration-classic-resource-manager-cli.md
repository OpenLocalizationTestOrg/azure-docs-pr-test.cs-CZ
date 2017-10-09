---
title: "virtuální počítače tooResource aaaMigrate Manager pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek vás provede hello platformy podporované migrace prostředků z classic tooAzure Resource Manager pomocí rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 0b11f4bb1b4658b3e88bf7629147ed953b678556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-cli"></a>Migrovat prostředky infrastruktury z classic tooAzure Resource Manager pomocí rozhraní příkazového řádku Azure
Tyto kroky vám ukážou, jak toouse rozhraní příkazového řádku Azure (CLI) příkazy toomigrate infrastruktury jako služby (IaaS) prostředky z modelu nasazení classic hello, toohello modelu nasazení Azure Resource Manager. článek Hello vyžaduje hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).

> [!NOTE]
> Všechny operace hello postupu popsaného tady jsou idempotent. Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme, abyste se znovu pokusíte hello připravit, zrušení nebo potvrzení operace. Hello platformy a zkuste to znovu hello akce.
> 
> 

<br>
Tady je pořadí tooidentify hello vývojový diagram, ve kterém kroky nutné toobe provést během procesu migrace

![Snímek obrazovky, který zobrazuje kroky migrace hello](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>Krok 1: Příprava na migraci
Tady je několik osvědčených postupů, které doporučujeme jak vyhodnotit migrace prostředky infrastruktury z classic tooResource Manager:

* Pročtěte hello [seznam nepodporovaných konfigurací nebo funkce](../windows/migration-classic-resource-manager-overview.md). Pokud máte virtuální počítače, které používají nepodporované konfigurace nebo funkce, doporučujeme počkejte hello konfigurace funkcí nebo podporu toobe oznámeno. Alternativně můžete odebrat tuto funkci nebo přesunout mimo tuto tooenable migraci konfigurace, pokud ji vyhovuje vašim potřebám.
* Pokud máte automatizované skripty, které jsou dnes nasazení infrastruktury a aplikace, zkuste toocreate podobné nastavení testu pomocí těchto skriptů pro migraci. Alternativně můžete nastavit ukázkové prostředí pomocí hello portálu Azure.

> [!IMPORTANT]
> Application Gateway nejsou aktuálně podporovány pro migraci z classic tooResource správce. Před spuštěním síť Příprava operace toomove hello odebrat toomigrate klasickou virtuální síť s aplikační brány, brány hello. Po dokončení migrace hello znovu hello brány ve službě Správce prostředků Azure. 
>
>Připojení tooExpressRoute okruhů v jiné předplatné brány ExpressRoute se nedají automaticky migrovat. V takových případech odeberte hello brány ExpressRoute, migrovat hello virtuální síť a znovu hello brány. Najdete v tématu [migrovat ExpressRoute okruhy a přidružené virtuální sítě z modelu nasazení Resource Manager classic toohello hello](../../expressroute/expressroute-migration-classic-resource-manager.md) Další informace.
> 
> 

## <a name="step-2-set-your-subscription-and-register-hello-provider"></a>Krok 2: Nastavte předplatné a zaregistrujte zprostředkovatele hello
Pro scénáře migrace, je třeba tooset prostředí pro obě classic a Resource Manager. [Instalace rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) a [vyberte své předplatné](../../xplat-cli-connect.md).

Tooyour přihlašovací účet.

    azure login

Vyberte hello předplatného Azure pomocí hello následující příkaz.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Registrace je v jednu chvíli krok ale vyžaduje toobe provádí jednou před pokusem o migraci. Bez registrace se zobrazí následující chybová zpráva hello 
> 
> *Struktura BadRequest: Předplatné není zaregistrované pro migraci.* 
> 
> 

Zaregistrovat u zprostředkovatele prostředků migrace hello pomocí hello následující příkaz. Všimněte si, že v některých případech tento příkaz časového limitu. Registrace hello však bude úspěšné.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Počkejte pět minut, než toofinish registrace hello. Stav hello hello schválení můžete zkontrolovat pomocí hello následující příkaz. Ujistěte se, že je RegistrationState `Registered` než budete pokračovat.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Nyní přepínač příkazového řádku toohello `asm` režimu.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a>Krok 3: Zkontrolujte, zda že máte dostatečný počet jader virtuálního počítače Azure Resource Manager v hello oblast Azure vaše aktuální nasazení nebo virtuální sítě
V tomto kroku budete potřebovat tooswitch příliš`arm` režimu. To lze proveďte pomocí hello následující příkaz.

```
azure config mode arm
```

Můžete použít hello následující rozhraní příkazového řádku příkaz toocheck hello aktuální velikost jádra, které máte ve službě Správce prostředků Azure. toolearn Další informace o základní kvóty, najdete v části [omezení a hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Po dokončení ověření tento krok, můžete přepnout zpět příliš`asm` režimu.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Krok 4: Možnost 1 - migraci virtuálních počítačů v rámci cloudové služby
Získání seznamu hello cloudových služeb pomocí hello následující příkaz a potom vyberte hello cloudové služby, které chcete toomigrate. Všimněte si, že pokud hello virtuálních počítačů v rámci hello cloudové služby jsou ve virtuální síti nebo mají webové/role pracovního procesu, zobrazí se chybová zpráva.

    azure service list

Spusťte následující příkaz tooget hello název nasazení pro cloudovou službu hello z podrobný výstup hello hello. Ve většině případů je hello název nasazení hello stejný jako název hello cloudové služby.

    azure service show <serviceName> -vv

Nejprve ověřte, jestli je možné migrovat hello cloudové služby pomocí hello následující příkazy:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Připravte hello virtuálních počítačů v rámci hello cloudové služby pro migraci. Máte dvě možnosti toochoose z.

Pokud chcete toomigrate hello virtuální počítače tooa platformy vytvořit virtuální síť, použijte následující příkaz hello.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Pokud chcete stávající virtuální sítě v modelu nasazení Resource Manager hello tooan toomigrate, použijte následující příkaz hello.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Po přípravě hello operace proběhne úspěšně, můžete projděte tooget podrobný výstup hello hello migrace stavu hello virtuální počítače a ujistěte se, že jsou v hello `Prepared` stavu.

    azure vm show <vmName> -vv

Zkontrolujte konfiguraci hello hello připravenou prostředky pomocí rozhraní CLI nebo hello portálu Azure. Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte následující příkaz hello.

    azure service deployment abort-migration <serviceName> <deploymentName>

Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Krok 4: Možnost 2 - migrovat virtuální počítače ve virtuální síti
Vybrat hello virtuální sítě, které chcete toomigrate. Všimněte si, že pokud hello virtuální síť obsahuje role web nebo worker nebo virtuální počítače s nepodporované konfigurace, obdržíte chybovou zprávu ověření.

Získáte všechny hello virtuální sítě v rámci předplatného hello pomocí hello následující příkaz.

    azure network vnet list

Hello výstup bude vypadat přibližně takto:

![Snímek obrazovky hello příkazového řádku s zvýrazněná název hello celý virtuální sítě.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

V hello výše příklad hello **virtualNetworkName** je celý název hello **"Classicubuntu16 classicubuntu16 skupiny"**.

Nejprve ověřte, jestli je možné migrovat hello virtuální sítě pomocí hello následující příkaz:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Připravte hello virtuální síť podle svého výběru pro migraci pomocí hello následující příkaz.

    azure network vnet prepare-migration <virtualNetworkName>

Zkontrolujte konfiguraci hello hello připravit virtuální počítače pomocí rozhraní příkazového řádku nebo hello portálu Azure. Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte následující příkaz hello.

    azure network vnet abort-migration <virtualNetworkName>

Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Krok 5: Migrace účet úložiště
Po dokončení migrace hello virtuálních počítačů, doporučujeme, abyste že migrujete hello účet úložiště.

Příprava na migraci účtu úložiště hello pomocí hello následující příkaz

    azure storage account prepare-migration <storageAccountName>

Zkontrolujte konfiguraci hello hello připravenou účet úložiště pomocí rozhraní CLI nebo hello portálu Azure. Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte následující příkaz hello.

    azure storage account abort-migration <storageAccountName>

Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Další kroky

* [Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Pomocí prostředí PowerShell toomigrate IaaS prostředky z classic tooAzure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Komunita nástroje asistence s migrace prostředky infrastruktury z classic tooAzure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Běžné chyby při migraci](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
