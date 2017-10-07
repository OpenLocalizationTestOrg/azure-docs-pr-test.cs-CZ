---
title: aaaHow tooadminister Azure Redis Cache | Microsoft Docs
description: "Další informace, jak tooperform úlohy správy, jako restartovat a naplánovat aktualizace pro Azure Redis Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: eb24668a3f6264444e7d4daf1ac43b41b12dfe66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadminister-azure-redis-cache"></a>Jak tooadminister Azure mezipaměti Redis
Toto téma popisuje, jak Správa tooperform úkoly, jako [restartování](#reboot) a [plánování aktualizace](#schedule-updates) pro vaše instance služby Azure Redis Cache.

## <a name="reboot"></a>Restartování
Hello **restartovat** okno vám umožní tooreboot jeden nebo více uzlů svojí mezipaměti. Tato možnost restartování umožňuje vám tootest Pokud dojde k selhání uzlu mezipaměti aplikace pro odolnost proti chybám.

![Restartování](./media/cache-administration/redis-cache-administration-reboot.png)

Vyberte hello uzly tooreboot a klikněte na **restartovat**.

![Restartování](./media/cache-administration/redis-cache-reboot.png)

Pokud máte s povoleným clusteringem cache ve verzi premium, můžete vybrat které horizontálních tooreboot mezipaměti hello.

![Restartování](./media/cache-administration/redis-cache-reboot-cluster.png)

tooreboot jeden nebo více uzlů vaší mezipaměti, vyberte uzel hello potřeby a klikněte na tlačítko **restartovat**. Pokud máte cache ve verzi premium s povoleným clusteringem, vyberte tooreboot hello potřeby horizontálních oddílů a pak klikněte na **restartovat**. Po několika minutách hello restartování vybrané uzly a jsou zpět do režimu online několik minut později.

Hello dopad na klientské aplikace se liší v závislosti na tom, které uzly restartovat.

* **Hlavní** – Pokud hello hlavní uzel selže restartovaný, Azure Redis Cache prostřednictvím toohello repliky uzlu a zvýší úroveň toomaster. Během této převzetí služeb při selhání může být krátký interval, ve kterém může připojení selhat toohello mezipaměti.
* **Podřízený** – Pokud je restartován hello podřízený uzel, je obvykle žádní klienti toocache dopad.
* **Hlavní a podřízený** – Pokud oba uzly mezipaměti se restartují, všechna data dojde ke ztrátě v hello mezipaměti a připojení toohello mezipaměti selžou, dokud hello primární uzel přejde do režimu online. Pokud jste nakonfigurovali [trvalosti dat](cache-how-to-premium-persistence.md), hello nejnovější zálohy se obnoví, když mezipaměť hello přejde do režimu online, ale žádné mezipaměti zápisů, které došlo k chybě po poslední záloze hello budou ztraceny.
* **Uzly prémiový mezipaměti s povoleným clusteringem** – Pokud restartujete jeden nebo více uzlů premium mezipaměti s clusteringem povoleno, hello chování pro hello vybrané uzly je hello stejný jako při restartování uzlu odpovídající hello nebo uzlů neclusterované mezipaměti.

> [!IMPORTANT]
> Restart je nyní k dispozici pro všechny cenové úrovně.
> 
> 

## <a name="reboot-faq"></a>Nejčastější dotazy k restartování počítače
* [Uzel musí restartování tootest Moje aplikace?](#which-node-should-i-reboot-to-test-my-application)
* [Můžete restartovat připojení klienta tooclear hello mezipaměti?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Dojde I ke ztrátě dat z mé mezipaměti restartu dělat?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Můžete restartovat Moje mezipaměti pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Jaké cenové úrovně můžete použít funkci restartování hello?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-tootest-my-application"></a>Uzel musí restartování tootest Moje aplikace?
tootest hello odolnost vaší aplikace proti selhání primárního uzlu hello vaší mezipaměti restartování hello **hlavní** uzlu. tootest hello odolnost vaší aplikace proti selhání sekundárního uzlu hello restartování hello **podřízený** uzlu. tootest hello odolnost vaší aplikace proti celkový počet selhání hello mezipaměti restartovat **i** uzlů.

### <a name="can-i-reboot-hello-cache-tooclear-client-connections"></a>Můžete restartovat připojení klienta tooclear hello mezipaměti?
Ano, pokud restartujete hello mezipaměti jsou vymazány všechna připojení klientů. Restartování může být užitečné v případě hello použití všechna připojení klientů z důvodu chyby logiku tooa nebo chyb v aplikaci klienta hello. Každá cenová úroveň má jiný [omezení připojení klienta](cache-configure.md#default-redis-server-configuration) pro hello různé velikosti a po dosažení těchto omezení jsou přijata žádná další připojení klienta. Restartování hello mezipaměti poskytuje způsob tooclear všechna připojení klientů.

> [!IMPORTANT]
> Pokud restartujete připojení klienta mezipaměti tooclear, StackExchange.Redis automaticky znovu připojí po hello Redis uzel je zpět do režimu online. Není hello základní problém vyřešen, pravděpodobně připojení klienta hello toobe vyčerpáte.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Dojde I ke ztrátě dat z mé mezipaměti restartu dělat?
Pokud restartujete obou hello **hlavní** a **podřízený** uzly, dojde ke ztrátě všech dat v mezipaměti hello (nebo horizontálního oddílu Pokud používáte cache ve verzi premium s povoleným clusteringem). Pokud jste nakonfigurovali [trvalosti dat](cache-how-to-premium-persistence.md), hello nejnovější zálohy se obnoví, když mezipaměť hello přejde do režimu online, ale všech mezipaměti zápisu, které nastaly po hello zálohy jsou ztraceny.

Pokud restartujete právě jeden z uzlů hello, data nejsou obvykle ztraceny, ale stále může být. Například pokud hello hlavní uzel restartuje, a mezipaměti zápisu právě probíhá hello data z mezipaměti zápisu hello je ztraceny. Jiné scénáře ztráty dat by, pokud restartování jeden uzel a dalších hello uzlu se stane toogo dolů z důvodu selhání tooa v hello stejnou dobu. Další informace o možných příčinách ztráty dat najdete v tématu [jaká data se stalo toomy v Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Můžete restartovat Moje mezipaměti pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?
Ano, pro prostředí PowerShell pokyny naleznete v části [tooreboot mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-hello-reboot-functionality"></a>Jaké cenové úrovně můžete použít funkci restartování hello?
Restart je k dispozici pro všechny cenové úrovně.

## <a name="schedule-updates"></a>Aktualizace plánu
Hello **naplánovat aktualizace** okno vám umožní toodesignate údržby pro mezipaměť úrovně Premium. Pokud je zadána hello údržby, všechny aktualizace serveru Redis probíhají během této doby. 

> [!NOTE] 
> Hello časové období údržby platí pouze tooRedis serveru aktualizace a ne tooany Azure aktualizace nebo aktualizace operačního systému toohello hello virtuálních počítačů, které jsou hostiteli hello mezipaměti.
> 
> 

![Aktualizace plánu](./media/cache-administration/redis-schedule-updates.png)

toospecify údržby, zkontrolujte hello potřeby dnů hodina spouštění údržby okno hello za každý den, zadejte a klikněte na tlačítko **OK**. Všimněte si, že hello časového období údržby se ve standardu UTC. 

> [!NOTE]
> Hello výchozí údržby pro aktualizace je pět hodin. Tato hodnota se nedá konfigurovat z hello portálu Azure, ale můžete ji nakonfigurovat v prostředí PowerShell pomocí hello `MaintenanceWindow` parametr hello [New-AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry) rutiny. Další informace najdete v tématu [můžete spravovat plánovaných aktualizací pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)
> 
> 

## <a name="schedule-updates-faq"></a>Plán aktualizace – nejčastější dotazy
* [Pokud aktualizace dojít, pokud nepoužívám hello plán aktualizací funkce?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Jaký typ aktualizací probíhají během hello naplánované údržby?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Můžete spravovat plánovaných aktualizací pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Jaké cenové úrovně pomocí funkce aktualizace plánu hello?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-hello-schedule-updates-feature"></a>Pokud aktualizace dojít, pokud nepoužívám hello plán aktualizací funkce?
Pokud nezadáte údržby, můžete kdykoli provedeny aktualizace.

### <a name="what-type-of-updates-are-made-during-hello-scheduled-maintenance-window"></a>Jaký typ aktualizací probíhají během hello naplánované údržby?
Pouze Redis serveru, které jsou provedeny aktualizace během plánované údržby hello. Hello údržby doporučení se netýká tooAzure aktualizací nebo aktualizací toohello virtuálních počítačů operačního systému.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Můžete spravované plánovaných aktualizací pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?
Ano, můžete spravovat vaše naplánované aktualizace pomocí hello následující rutiny prostředí PowerShell:

* [Get-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/get-azurermrediscachepatchschedule)
* [Nové AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/new-azurermrediscachepatchschedule)
* [Nové AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry)
* [Odebrat AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/remove-azurermrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-hello-schedule-updates-functionality"></a>Jaké cenové úrovně pomocí funkce aktualizace plánu hello?
Hello **naplánovat aktualizace** funkce je k dispozici v cenová úroveň premium hello.

## <a name="next-steps"></a>Další kroky
* Prozkoumat více [Azure Redis Cache premium vrstvy](cache-premium-tier-intro.md) funkce.

