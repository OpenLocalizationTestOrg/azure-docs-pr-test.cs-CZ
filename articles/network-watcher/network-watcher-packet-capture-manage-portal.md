---
title: "paket aaaManage zachytávali sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage hello funkce zachytávání paketů sledovací proces sítě pomocí portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 431b145ee215b8d9421fb2aacdce08a0de90b35e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-hello-portal"></a>Spravovat zachycení paketů s sledovací proces sítě Azure pomocí portálu hello

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Rozhraní API Azure REST](network-watcher-packet-capture-manage-rest.md)

Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače. Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete. Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně. Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího. Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.

Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.

- [**Spustit zachytávání paketů**](#start-a-packet-capture)
- [**Zastavit zachytávání paketů**](#stop-a-packet-capture)
- [**Odstranit zachytávání paketů**](#delete-a-packet-capture)
- [**Stáhnout zachytáváním paketů**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Než začnete

Tento článek předpokládá, že máte hello následující prostředky:

- Instance sledovací proces sítě v hello oblasti, kterou chcete toocreate zachytávání paketů
- Virtuální počítač s hello paketu zachytit rozšíření povolené.

> [!IMPORTANT]
> Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`. Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-hello-portal"></a>Rozšíření agenta zachytávání paketů přes portál hello

zachytáváním paketů hello tooinstall agenta virtuálního počítače přes portál hello přejděte tooyour virtuální počítač, klikněte na **rozšíření** > **přidat** a vyhledejte **sítě sledovacích procesů agenta pro Windows**

![zobrazení agenta][agent]

## <a name="packet-capture-overview"></a>Přehled zachytávání paketů

Přejděte toohello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sítě** > **sledovací proces sítě** > **zachytávání paketů**

stránka s přehledem Hello zobrazuje že seznam všech paketů zaznamená, který neexistuje bez ohledu na to hello stavu.

> [!NOTE]
> Zachytáváním paketů vyžaduje účet úložiště toohello připojení přes port 443.

![obrazovka Přehled zachytávání paketů][1]

## <a name="start-a-packet-capture"></a>Spustit zachytávání paketů

Klikněte na tlačítko **přidat** toocreate zachytáváním paketů.

Hello vlastnosti, které lze definovat v zachytáváním paketů jsou:

**Hlavní nastavení**

- **Předplatné** – tato hodnota je hello odběr, který se používá, je potřeba instanci sledovací proces sítě v každém předplatném.
- **Skupina prostředků** -hello hello virtuální počítač, který je cílem skupiny prostředků.
- **Cílové virtuální počítač** -hello virtuální počítač, který běží zachytáváním paketů hello
- **Název zachycení paketu** – tato hodnota je název hello zachytáváním paketů hello.

**Zachycení konfigurace**

- **Účet úložiště** -Určuje, pokud se zachytáváním paketů je uložen v účtu úložiště.
- **Soubor** -Určuje, pokud se zachytáváním paketů uložená lokálně na hello virtuálního počítače.
- **Účty úložiště** -hello vybrané zachytáváním paketů úložiště účet toosave hello v. Výchozí umístění je id name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription účtu https://{storage} /resourcegroups/ {název počítače name}/providers/microsoft.compute/virtualmachines/{virtual skupiny prostředků} / {RR} / {MM} / {DD} / {HH} packetcapture__{MM}_CAP _ {XXX} {SS}. (Aktivní, pouze pokud **úložiště** je vybraná)
- **Cesta k souboru místní** -hello místní cestu na zachycení virtuálního počítače toosave hello paketů. (Aktivní, pouze pokud **soubor** je vybraný). Je nutné zadat platnou cestu
- **Maximální počet bajtů paketu** -hello počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.
- **Maximální počet bajtů za relace** – celkový počet bajtů, které jsou zachyceny po dosažení Zastaví zachytávání paketů hello hello hodnoty.
- **Časový limit (sekundy)** -nastaví časový limit pro toostop zachytávání paketů hello. Výchozí hodnota je 18000 sekund.

> [!NOTE]
> Účty služby Premium storage aktuálně nejsou podporované pro ukládání paketu zaznamená.

**Filtrování (volitelné)**

- **Protokol** -hello toofilter protokol pro zachytávání paketů hello. Hello dostupné jsou hodnoty TCP, UDP a všechny.
- **Místní IP adresu** -tuto hodnotu filtry toopackets zachytávání paketů hello kde hello místní IP adresa odpovídá této hodnotě filtru.
- **Místní port** -tuto hodnotu filtry toopackets zachytávání paketů hello kde místního portu hello odpovídá této hodnotě filtru.
- **Vzdálené IP adresy** -tuto hodnotu filtry toopackets zachytávání paketů hello kde hello vzdálené IP odpovídá této hodnotě filtru.
- **Vzdálený port** -tuto hodnotu filtry toopackets zachytávání paketů hello kde vzdálených portů hello odpovídá této hodnotě filtru.

> [!NOTE]
> Hodnoty port a IP adres může být jedna hodnota, rozsah hodnot nebo u sady. (to znamená, 80 – 1024 pro port) Můžete definovat libovolný počet filtrů tak, jak chcete.

Jakmile jsou doplnit hello hodnoty, klikněte na možnost **OK** zachytáváním paketů toocreate hello.

![Vytvoření zachytávání paketů][2]

Po nastavení hello časový limit na zachytáváním paketů hello vypršela platnost, zachytáváním paketů hello se zastaví a můžete zkontrolovat. Můžete také ručně zastavit hello paketu zachycení relace.

## <a name="delete-a-packet-capture"></a>Odstranit zachytávání paketů

V paketu hello zaznamenat zobrazení, klikněte na tlačítko hello **kontextovou nabídku** (...) nebo klikněte pravým tlačítkem a klikněte na tlačítko **odstranit** zachytáváním paketů toostop hello

![Odstranit zachytávání paketů][3]

> [!NOTE]
> Odstraňuje se zachytáváním paketů nedojde k odstranění hello souboru v účtu úložiště hello.

Jste vyzváni tooconfirm chcete zachytáváním paketů hello toodelete, klikněte na tlačítko **Ano**

![Potvrzení][4]

## <a name="stop-a-packet-capture"></a>Zastavit zachytávání paketů

V paketu hello zaznamenat zobrazení, klikněte na tlačítko hello **kontextovou nabídku** (...) nebo klikněte pravým tlačítkem a klikněte na tlačítko **Zastavit** zachytáváním paketů toostop hello

## <a name="download-a-packet-capture"></a>Stáhnout zachytáváním paketů

Po dokončení relace zachytávání paketů hello zachycení nahrané tooblob úložiště nebo tooa místní soubor je na hello virtuálních počítačů. umístění úložiště Hello zachytáváním paketů hello se definuje při vytvoření relace hello. Vhodné nástroje tooaccess tyto zaznamenat soubory uložené tooa účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/

Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













