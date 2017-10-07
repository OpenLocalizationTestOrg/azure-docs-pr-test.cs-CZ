---
title: "Přehled aaaAzure prostředí cloudu (Preview) | Microsoft Docs"
description: "Přehled hello prostředí cloudu Azure."
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
ms.openlocfilehash: 45c6c85b167a90947a333f44a9186e2c01b4fa7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Přehled prostředí cloudu Azure (Preview)
Prostředí Azure Cloud je interaktivní, přístupných prohlížeče prostředí pro správu prostředků Azure.

![](media/overview-pic.png)

## <a name="features"></a>Funkce
### <a name="browser-based-shell-experience"></a>Prostředí shell založené na prohlížeči
Cloudové prostředí umožňuje přístup tooa založené na prohlížeči příkazového řádku prostředí vytvořené s nástroji úlohy správy Azure v paměti. Využívejte cloudové prostředí toowork untethered z místního počítače způsobem, který může poskytnout jenom hello cloud.

### <a name="pre-configured-azure-workstation"></a>Předem nakonfigurovaná Azure pracovní stanice
Cloudové prostředí předem nainstalovaný pomocí oblíbené nástroje příkazového řádku a podpora jazyků, abyste mohli pracovat rychleji.

[Zobrazení hello úplné nástrojů seznam pro cloudové prostředí Azure sem.](features.md#tools)

### <a name="automatic-authentication"></a>Automatické ověření
Cloudové prostředí bezpečně ověřuje automaticky na každou relaci pro rychlé přístup k prostředkům tooyour přes hello 2.0 rozhraní příkazového řádku Azure.

### <a name="connect-your-azure-file-storage"></a>Připojení Azure File storage
Cloudové prostředí počítače jsou dočasné a v důsledku vyžadují toobe sdílenou složku Azure file připojit jako `clouddrive` toopersist adresáře $Home.
Při prvním spuštění prostředí cloudu vyzve k zadání toocreate, které skupiny prostředků, účet úložiště a soubor sdílet vaším jménem. To je jednorázový krok a bude automaticky připojen pro všechny relace. 

#### <a name="create-new-storage"></a>Vytvoření nového úložiště
![](media/basic-storage.png)

Místně redundantní úložiště (LRS) účet může být vytvořen vaším jménem pomocí sdílenou složku Azure obsahující výchozí obrázek 5-GB místa na disku. sdílenou složku Hello připojí jako `clouddrive` pro soubor sdílet interakci s image disku hello je použité toosync a zachovat adresáře $Home. Náklady na úložiště regulární použít.

Vaším jménem vytvoří tři zdroje:
1. Skupinu prostředků s názvem:`cloud-shell-storage-<region>`
2. Účet úložiště s názvem:`cs<uniqueGuid>`
3. Sdílené složky s názvem:`cs-<user>-<domain>-com-uniqueGuid`

> [!Note]
> Všechny soubory v adresáři $Home například klíče SSH zůstávají v bitové kopii disku uživatele uložený ve sdílené složce vaší připojeného souboru. Použít osvědčené postupy při ukládání souborů v adresáři $Home a připojené sdílené složky.

#### <a name="use-existing-resources"></a>Používat existující prostředky
![](media/advanced-storage.png)

Upřesňující možnost je k dispozici také povolení jste tooassociate existující prostředky tooCloud prostředí. Při s výzvou instalace hello úložiště, klikněte na tlačítko "Zobrazit rozšířená nastavení" tooselect další možnosti. Rozevírací seznamy jsou filtrovány přiřazené oblast prostředí cloudu a místně nebo globálně redundantní úložiště účtů.

[Informace o prostředí cloudové úložiště, aktualizace sdílené složky a nahrávání nebo stahování souborů.] (uložením prostředí storage.md)

## <a name="concepts"></a>Koncepty
* Cloudové prostředí běží na dočasné počítači k dispozici na každou relaci, jednotlivých uživatelů
* Cloudové prostředí vyprší po 20 minutách bez interaktivní aktivity
* Cloudové prostředí lze přistupovat pouze u sdílené složky připojit
* Cloudové prostředí je přiřazený jeden počítač na uživatelský účet
* Máte nastavená oprávnění jako běžný uživatel Linux

[Další informace o všech funkcích cloudové prostředí.](features.md)

## <a name="examples"></a>Příklady
* Vytvořte nebo upravte skripty tooautomate správu Azure
* Současně spravovat prostředky prostřednictvím portálu Azure a Azure CLI 2.0
* Test-Drive rozhraní příkazového řádku Azure 2.0

[Vyzkoušejte všech těchto příkladech v prostředí cloudu hello rychlý start.](quickstart.md)

## <a name="pricing"></a>Ceny
počítač Hello hostování prostředí cloudu je volné, s předpoklad připojeného souboru Azure sdílet toopersist adresáře $Home. Náklady na úložiště regulární použít.

## <a name="supported-browsers"></a>Podporované prohlížeče
Cloudové prostředí se doporučuje pro Chrome, okraj a Safari. Když cloudové prostředí je podporované pro Chrome, Firefox, Safari, aplikace Internet Explorer a okraj, je cloudové prostředí nastavení prohlížeče toospecific subjektu.

## <a name="troubleshooting"></a>Řešení potíží
1. Pokud používáte předplatné služby Azure Active Directory, nelze vytvořit úložiště kvůli tooError: 400 DisallowedOperation. tooresolve, použijte prosím předplatné Azure podporuje vytváření prostředků úložiště. Odběry AD se nepodařilo toocreate prostředků Azure.

Konkrétní známá omezení, najdete v článku [omezení cloudové prostředí](limitations.md).
