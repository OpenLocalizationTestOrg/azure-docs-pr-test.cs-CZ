---
title: "Migrace dat uživatele z Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak migrovat uživatelská data do/z Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d7e4fbf1-cb42-4430-94a0-ed6d4676fc86
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ba3cf4c6834279bbd7f94d666fd8abbb7ac05bf0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Migrace dat do a z Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Můžete použít řadu různých nástrojů a metody pro přenos [uživatelská data](remoteapp-upd.md) do a z Azure Remoteappu. Tady je několik metod:

* Zkopírujte a vložte pomocí sdílení schránky
* Zkopírujte soubory a data na souborovém serveru
* Zkopírujte soubory na OneDrive pro firmy prostřednictvím prohlížeče
* Zkopírujte soubory pomocí přesměrování

> [!NOTE]
> Nelze povolit Onedrivu pro firmy nebo příjemce synchronizace agenty - jejich [nejsou podporovány](remoteapp-onedrive.md) v Azure Remoteappu.
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopírování a vkládání v Průzkumníku souborů
Kopírování a vkládání použití schránky je povolena v nasazeních vzdálené aplikace RemoteApp [ve výchozím nastavení](remoteapp-redirection.md). To umožňuje uživatelům kopírovat soubory mezi místním počítači a vzdálené aplikace RemoteApp aplikace. Často prostřednictvím běžném průběhu používání aplikace v Remoteappu, uživatelé uložit soubory k jejich UPD - přesunutí, že je snadné data ze vzdálené aplikace RemoteApp:

1. [Publikování Průzkumníka souborů jako aplikace](remoteapp-publish.md) v kolekci vzdálené aplikace RemoteApp. (Všimněte si, že toto je Správce úloh.)
2. Směrovat své uživatele, spusťte aplikaci Průzkumník souborů, které jste publikovali a použít pro kopírování a vkládání souborů do jejich UPD i mimo ho.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Nahrajte soubory a data na souborový server s použitím standardního síťového kopírování souborů
Často organizace používat k ukládání dat obecné souborové servery. Pokud znáte název serveru nebo umístění, můžete uživatelům procházet místní sítě pro server a poté zkopírujte své soubory, jako tomu bylo výše. Můžete znovu publikovat Průzkumníka souborů do vzdálené aplikace RemoteApp a sdílet je s uživateli.

> [!NOTE]
> Souborový server musí být na směrovatelné síti, který byl nasazen vzdálené aplikace RemoteApp do.
> 
> 

## <a name="copy-files-to-onedrive-for-business"></a>Zkopírujte soubory na OneDrive pro firmy
I když Onedrivu pro firmy agenta synchronizace v Remoteappu nelze povolit, můžete kopírovat soubory z vaší UPD na OneDrive pro firmy prostřednictvím prohlížeče. 

1. Publikování Průzkumníka souborů do vzdálené aplikace RemoteApp a pak říct uživatelům přístup k souborům prostřednictvím této aplikace. 
2. Je to nejjednodušší provádět přenos souborů, pokud byla komprimovaná, aby uživatelé měli vytvořit soubor .zip, který obsahuje všechny soubory pro přesun do Onedrivu pro firmy.
3. Požádejte uživatele, přejděte na portál Office 365 a pak přejděte na OneDrive a nahrát soubor .zip.

## <a name="copy-files-by-using-drive-redirection"></a>Zkopírujte soubory pomocí přesměrování jednotky
Pokud jste povolili [jednotka přesměrování](remoteapp-redirection.md), jste již vytvořili mapované jednotky pro vaše uživatele. V takovém případě můžete své soubory na tato jednotka zip a uložit je do svého místního počítače.

## <a name="how-administrators-can-export-data"></a>Jak mohou správci exportovat data

Všechny kolekce v rámci předplatného pro Azure Storage pomocí rutiny Azure Powershellu, Export AzureRemoteAppUserDisk slouží ke správě pro Azure RemoteApp můžete exportovat všechny disky profilu uživatele (UPD).  Neexistuje žádná možnost vybrat jednotlivé UPD.  Když je proveden příkaz prostředí PowerShell, každého disku, uživatel bude 50gb velikost pevného disku a exportovat do úložiště Azure.  Pro toto úložiště je možné hned za náklady na úložiště Azure.  Při spuštění tohoto příkazu zkontrolujte neexistují žádné relace, jinak hodnota export se nezdaří.

UPD je připojený k doméně Azure RemoteApp nasazení lze použít pouze znovu v nasazení služby Vzdálená plocha, nelze použít nasazení připojený k doméně.  Pokud tyto disky se použije v nasazení služby Vzdálená plocha doporučujeme používat naše [automatizované skripty](https://github.com/arcadiahlyy/aramigration) , bude exportovat, převod a import UPD do nasazení služby Vzdálená plocha.

