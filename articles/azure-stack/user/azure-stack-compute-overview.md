---
title: "Úvod do Azure zásobníku virtuálních počítačů"
description: "Další informace o virtuálních počítačích Azure zásobníku"
services: azure-stack
author: anjayajodha
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: c37ad8ac5b6c37261e22237e843dd97e2bbd09f9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Úvod do Azure zásobníku virtuálních počítačů

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

## <a name="overview"></a>Přehled
Virtuální počítač Azure zásobníku (VM) je jeden typ na vyžádání, škálovatelný výpočetní prostředek, který nabízí zásobník Azure. Obvykle zvolíte virtuální počítač, když potřebujete větší kontrolu nad výpočetním prostředí, než nabízí jiné možnosti. Tento článek obsahuje informace o tom, co byste měli zvážit před vytvořením virtuálního počítače, jak ho vytvořit a jak ho spravovat.

Virtuální počítač Azure zásobníku poskytuje flexibilitu virtualizace bez nutnosti spravovat jednotlivé clustery nebo počítače. Nevyhnete se však údržbě virtuálního počítače prováděním úloh, jako jsou konfigurace, aplikování oprav chyb a instalace softwaru, který na něm běží.

Virtuální počítače Azure zásobníku lze různými způsoby. Například:

* **Vývoj a testování** – virtuální počítače Azure zásobníku nabízejí nějakou rychlou a jednoduchý způsob, jak vytvořit počítač s určitou konfiguraci vyžaduje kód a testování aplikací.

* **Aplikace v cloudu** – protože vyžádání pro vaši aplikaci můžete kolísá, může mít ekonomický smysl ji spustit na virtuálním počítači v Azure zásobníku. Za další virtuální počítače platíte, když je potřebujete, a když ne, tak je vypnete.

* **Rozšířené datacenter** – virtuální počítače ve virtuální síť Azure zásobníku lze snadno připojit k síti vaší organizace nebo Azure.

Počet virtuálních počítačů, které vaše aplikace používá, lze vertikálně nebo horizontálně navýšit pro splnění vašich požadavků.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Co je třeba zvážit před vytvořením virtuálního počítače?

Stále existují různé aspekty návrhu při sestavování na infrastruktuře aplikace v Azure zásobníku. Než začnete, je důležité zvážit následující aspekty virtuálního počítače:

- Názvy prostředků vaší aplikace
- Velikost virtuálního počítače
- Maximální počet virtuálních počítačů, které lze vytvořit
- Operační systém, který běží na virtuálním počítači
- Konfigurace virtuálního počítače po jeho spuštění 
- Související prostředky, které virtuální počítač potřebuje

### <a name="naming"></a>Pojmenování

Virtuální počítač má přiřazen název a má název počítače, který je nakonfigurovaný jako součást operačního systému. Název virtuálního může být až 15 znaků dlouhý.

Pokud použijete zásobník Azure k vytvoření disku operačního systému, název počítače a název virtuálního počítače jsou stejné. Názvy nahrát, používat vlastní image, která obsahuje dříve nakonfigurované operačního systému a použít k vytvoření virtuálního počítače, se může lišit. Při nahrávání souboru bitové kopie operačního systému zkontrolujte název počítače a název virtuálního počítače stejný jako osvědčený postup.

### <a name="vm-size"></a>Velikost virtuálního počítače

Velikost virtuálního počítače, který používáte, je dáno se zatížením, které chcete spustit. Velikost, kterou vyberete, pak určuje další faktory, jako například výpočetní výkon, paměť a kapacitu úložiště. Zásobník Azure nabízí širokou škálu velikostí, které podporují mnoho typů použití.

### <a name="vm-limits"></a>Omezení virtuálních počítačů

Vaše předplatné má výchozí kvótami v místě, ke kterému může mít vliv nasazení hodně virtuálních počítačů pro váš projekt. Aktuální limit jednoho předplatného je 20 virtuálních počítačů na oblast.

### <a name="operating-system-disks-and-images"></a>Disky a image operačních systémů

Virtuální počítače používají virtuální pevné disky (VHD), na které ukládají svůj operační systém (OS) a data. Virtuální pevné disky se používají i pro image, ze kterých si můžete nainstalovat operační systém.
Zásobník Azure poskytuje marketplace používat s různými verzemi a typy operačních systémů. Image pořízené na webu Marketplace jsou identifikované vydavatelem image, nabídkou, skladovou jednotkou (SKU) a verzí (verze je obvykle uvedena jako poslední).

Následující tabulka uvádí některé způsoby, abyste nalezli informace pro bitovou kopii:


|Metoda|Popis|
|---------|---------|
|Portál Azure zásobníku|Hodnoty se pro vás zadají automaticky, když vyberete image, která se má použít.|
|Azure Stack PowerShell|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|Rozhraní REST API     |[Vypsat vydavatele imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Vypsat nabídky imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Obrázek seznam SKU](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

Můžete nahrát a používat vlastní image. V takovém případě se nepoužívají název vydavatele, nabídky a sku.

### <a name="extensions"></a>Rozšíření

Rozšíření virtuálního počítače poskytují další možnosti vašeho virtuálního počítače prostřednictvím konfigurace nasazení post a automatizované úlohy.
Pomocí rozšíření můžete provádět tyto běžné úlohy:

* Spustit vlastní skripty – rozšíření vlastních skriptů vám pomůže nakonfigurovat úlohy na virtuálním počítači tak, že spustíte skript při zřízení virtuálního počítače.
* Nasazení a správě konfigurace – rozšíření prostředí PowerShell požadovaného stavu konfigurace (DSC) vám pomůže nastavit DSC na virtuálním počítači ke správě konfigurace a prostředí.
* Shromažďování diagnostická data – rozšíření diagnostiky Azure vám pomůže nakonfigurovat virtuální počítač ke shromažďování dat diagnostiky, který slouží k monitorování stavu aplikace.

### <a name="related-resources"></a>Související prostředky

Prostředky v následující tabulce se používají ve virtuálním počítači a musí existovat nebo vytvořit při vytváření virtuálního počítače.


|Prostředek|Požaduje se|Popis|
|---------|---------|---------|
|Skupina prostředků|Ano|Virtuální počítač musí být součástí skupiny prostředků.|
|Účet úložiště|Ano|Virtuální počítač potřebuje účet úložiště k ukládání svých virtuálních pevných disků.|
|Virtuální síť|Ano|Virtuální počítač musí být členem virtuální sítě.|
|Veřejná IP adresa|Ne|Virtuální počítač může mít přiřazenou veřejnou IP adresu pro umožnění vzdáleného přístupu.|
|Síťové rozhraní|Ano|Virtuální počítač potřebuje síťové rozhraní ke komunikaci v síti.|
|Datové disky|Ne|Virtuální počítač může zahrnovat datové disky pro rozšíření možností úložiště.|

## <a name="how-do-i-create-my-first-vm"></a>Jak vytvořím svůj první virtuální počítač?

Máte několik možností vytvoření virtuálního počítače. Výběr závisí na vašem prostředí.
Následující tabulka obsahuje informace, které vám pomůžou spuštěna, vytvoření virtuálního počítače.


|Metoda|Článek|
|---------|---------|
|Portál Azure zásobníku|Vytvoření virtuálního počítače s Windows pomocí portálu Azure zásobníku<br>[Vytvořit virtuální počítač s Linuxem pomocí portálu Azure zásobníku](azure-stack-quick-linux-portal.md)|
|Šablony|Šablony Azure rychlý start zásobníku jsou umístěné na adrese:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell v Azure zásobníku](azure-stack-quick-create-vm-windows-powershell.md)<br>[Vytvořit virtuální počítač s Linuxem pomocí prostředí PowerShell v Azure zásobníku](azure-stack-quick-create-vm-linux-powershell.md)|
|Rozhraní příkazového řádku|[Vytvoření virtuálního počítače s Windows pomocí rozhraní příkazového řádku v Azure zásobníku](azure-stack-quick-create-vm-windows-cli.md)<br>[Vytvořit virtuální počítač s Linuxem pomocí rozhraní příkazového řádku v Azure zásobníku](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Jak můžu spravovat vytvořený virtuální počítač?

Virtuální počítače můžete spravovat pomocí portálu (přes prohlížeč), nástrojů příkazového řádku s podporou pro skriptování, nebo přímo prostřednictvím rozhraní API. Mezi obvyklé úlohy správy, které můžete provádět, patří získávání informací o virtuálním počítači, připojování k virtuálnímu počítači, správa dostupnosti a provádění zálohování.

### <a name="get-information-about-a-vm"></a>Získání informací o virtuálním počítači

Následující tabulka ukazuje některé způsoby, které můžete získat informace o virtuální počítač.


|Metoda|Popis|
|---------|---------|
|Portál Azure zásobníku|V nabídce centra klikněte na virtuální počítače a potom vyberte virtuální počítač ze seznamu. Na stránce pro virtuální počítač máte přístup k přehled informací, monitorování metriky a nastavení hodnoty.|
|Azure PowerShell|Správa virtuálních počítačů je podobný v Azure a Azure zásobníku. Další informace o použití prostředí PowerShell najdete v následujícím tématu Azure:<br>[Vytvoření a správa virtuálních počítačů Windows s modulu Azure PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|Klientské sady SDK|Použití jazyka C# ke správě virtuálních počítačů je podobný v Azure a Azure zásobníku. Další informace najdete v následujícím tématu Azure:<br>[Vytvářet a spravovat virtuální počítače Windows v Azure pomocí jazyka C#](https://docs.microsoft.com/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-the-vm"></a>Připojení k virtuálnímu počítači

Můžete použít **Connect** tlačítko na portálu Azure zásobníku se připojit k virtuálnímu počítači.

## <a name="next-steps"></a>Další kroky
* [Důležité informace pro virtuální počítače v Azure zásobníku](azure-stack-vm-considerations.md)

