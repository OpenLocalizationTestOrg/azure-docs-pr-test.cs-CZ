---
title: "Přehled služby Azure Cloud prostředí (Preview) | Microsoft Docs"
description: "Přehled prostředí cloudu Azure."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 7165633cd354eeea2e3619f839338e6af1524e56
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Přehled prostředí cloudu Azure (Preview)
Prostředí Azure Cloud je interaktivní, přístupných prohlížeče prostředí pro správu prostředků Azure.

![](media/overview-pic.png)

## <a name="features"></a>Funkce
### <a name="browser-based-shell-experience"></a>Prostředí shell založené na prohlížeči
Cloudové prostředí umožňuje přístup k založené na prohlížeči příkazového řádku prostředí, vytvořené s nástroji úlohy správy Azure v paměti. Můžete zadat využívání cloudové prostředí pro práci untethered z místního počítače způsobem, jenom do cloudu.

### <a name="pre-configured-azure-workstation"></a>Předem nakonfigurovaná Azure pracovní stanice
Cloudové prostředí předem nainstalovaný pomocí oblíbené nástroje příkazového řádku a podpora jazyků, abyste mohli pracovat rychleji.

[Sem zobrazte seznam úplné nástrojů pro cloudové prostředí Azure.](features.md#tools)

### <a name="automatic-authentication"></a>Automatické ověření
Cloudové prostředí bezpečně ověřuje automaticky na každou relaci pro okamžitý přístup k prostředkům prostřednictvím Azure CLI 2.0.

### <a name="connect-your-azure-file-storage"></a>Připojení Azure File storage
Cloudové prostředí počítače jsou dočasné a v důsledku vyžadují sdílenou složku Azure chcete připojit jako `clouddrive` udržení adresáře $Home.
Při prvním spuštění prostředí cloudu vás vyzve k vytvoření prostředku skupiny, účet úložiště a sdílených složek vaším jménem. To je jednorázový krok a bude automaticky připojen pro všechny relace. 

#### <a name="create-new-storage"></a>Vytvoření nového úložiště
![](media/basic-storage.png)

Místně redundantní úložiště (LRS) účet může být vytvořen vaším jménem pomocí sdílenou složku Azure obsahující výchozí obrázek 5-GB místa na disku. Sdílené složky připojí jako `clouddrive` pro soubor sdílet interakce se používá k synchronizaci a zachovat adresáře $Home bitové kopie disku. Náklady na úložiště regulární použít.

Vaším jménem vytvoří tři zdroje:
1. Skupinu prostředků s názvem:`cloud-shell-storage-<region>`
2. Účet úložiště s názvem:`cs<uniqueGuid>`
3. Sdílené složky s názvem:`cs-<user>-<domain>-com-uniqueGuid`

> [!Note]
> Všechny soubory v adresáři $Home například klíče SSH zůstávají v bitové kopii disku uživatele uložený ve sdílené složce vaší připojeného souboru. Použít osvědčené postupy při ukládání souborů v adresáři $Home a připojené sdílené složky.

#### <a name="use-existing-resources"></a>Používat existující prostředky
![](media/advanced-storage.png)

Upřesňující možnost je k dispozici také umožňuje přidružit existující prostředky pro cloudové prostředí. Při s výzvou nastavení úložiště, klikněte na tlačítko "Zobrazit upřesňující nastavení" a vyberte další možnosti. Rozevírací seznamy jsou filtrovány přiřazené oblast prostředí cloudu a místně nebo globálně redundantní úložiště účtů.

[Informace o prostředí cloudové úložiště, aktualizace sdílené složky a nahrávání nebo stahování souborů.] (uložením prostředí storage.md)

## <a name="concepts"></a>Koncepty
* Cloudové prostředí běží na dočasné počítači k dispozici na každou relaci, jednotlivých uživatelů
* Cloudové prostředí vyprší po 20 minutách bez interaktivní aktivity
* Cloudové prostředí lze přistupovat pouze u sdílené složky připojit
* Cloudové prostředí je přiřazený jeden počítač na uživatelský účet
* Máte nastavená oprávnění jako běžný uživatel Linux

[Další informace o všech funkcích cloudové prostředí.](features.md)

## <a name="examples"></a>Příklady
* Vytvořte nebo upravte skripty pro automatizaci správy Azure
* Současně spravovat prostředky prostřednictvím portálu Azure a Azure CLI 2.0
* Test-Drive rozhraní příkazového řádku Azure 2.0

[Vyzkoušejte všech těchto příkladech v prostředí cloudu rychlý start.](quickstart.md)

## <a name="pricing"></a>Ceny
Tento počítač hostování prostředí cloudu je bezplatná s předpoklad připojené Azure sdílené složky se zachovat adresáře $Home. Náklady na úložiště regulární použít.

## <a name="supported-browsers"></a>Podporované prohlížeče
Cloudové prostředí se doporučuje pro Chrome, okraj a Safari. Když cloudové prostředí je podporované pro Chrome, Firefox, Safari, aplikace Internet Explorer a Edge, cloudové prostředí podléhá nastavení konkrétní prohlížeče.

## <a name="troubleshooting"></a>Řešení potíží
1. Pokud používáte předplatné služby Azure Active Directory, nelze vytvořit úložiště z důvodu chyby: 400 DisallowedOperation. To vyřešit, použijte prosím předplatné Azure podporuje vytváření prostředků úložiště. Nebudou se moct vytváření prostředků Azure AD odběry.

Konkrétní známá omezení, najdete v článku [omezení cloudové prostředí](limitations.md).
