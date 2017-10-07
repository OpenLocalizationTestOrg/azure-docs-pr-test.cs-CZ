---
title: "aaaMigrate uživatelská data z Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak toomigrate uživatelská data do/z Azure Remoteappu."
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
ms.openlocfilehash: aefc6ccc2c6173754acf6cad06102f27c8cb1d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-data-into-and-out-of-azure-remoteapp"></a>Jak toomigrate dat do a z Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Můžete použít řadu různých nástrojů a metody tootransfer [uživatelská data](remoteapp-upd.md) do a z Azure Remoteappu. Tady je několik metod:

* Zkopírujte a vložte pomocí sdílení schránky
* Zkopírujte soubory a data tooa souborového serveru
* Zkopírujte soubory tooOneDrive pro firmy prostřednictvím prohlížeče
* Zkopírujte soubory pomocí přesměrování

> [!NOTE]
> Nelze povolit hello OneDrive pro firmy nebo příjemce synchronizace agenty - jejich [nejsou podporovány](remoteapp-onedrive.md) v Azure Remoteappu.
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopírování a vkládání v Průzkumníku souborů
Kopírování a vkládání použití schránky hello je povolena v nasazeních vzdálené aplikace RemoteApp [ve výchozím nastavení](remoteapp-redirection.md). To umožňuje uživatelům kopírovat soubory mezi místním počítači a vzdálené aplikace RemoteApp aplikace. Často prostřednictvím hello běžném průběhu používání aplikace v Remoteappu, uživatelé uložit soubory tootheir UPD - přesunutí, že je snadné data ze vzdálené aplikace RemoteApp:

1. [Publikování Průzkumníka souborů jako aplikace](remoteapp-publish.md) v kolekci vzdálené aplikace RemoteApp. (Všimněte si, že toto je Správce úloh.)
2. Přímé uživatelé toolaunch hello Průzkumníka souborů aplikace, kterou jste publikovali a toouse této toocopy a vkládání souborů do jejich UPD i mimo ho.

## <a name="upload-files-and-data-tooa-file-server-by-using-standard-network-file-copy"></a>Odeslání souborů a dat tooa souborový server s použitím standardního síťového kopírování souborů
Často organizace používat soubor servery toostore obecné data. Pokud znáte název serveru hello nebo umístění, můžete uživatelům procházet hello místní sítě pro hello server a poté zkopírujte své soubory, jako tomu bylo výše. Budete znovu chcete tooRemoteApp toopublish Průzkumníka souborů a sdílet je s uživateli.

> [!NOTE]
> Hello souborový server musí být na hello směrovatelné síti, který byl nasazen vzdálené aplikace RemoteApp do.
> 
> 

## <a name="copy-files-tooonedrive-for-business"></a>Zkopírujte soubory tooOneDrive pro firmy
I když hello OneDrive pro firmy agenta synchronizace v Remoteappu nelze povolit, můžete kopírovat soubory z vaší tooOneDrive UPD pro firmy prostřednictvím prohlížeče. 

1. Publikování tooRemoteApp Průzkumníka souborů a potom říct, že uživatelé tooaccess hello soubory pomocí této aplikace. 
2. Je nejjednodušší soubory tootransfer Pokud byla komprimovaná, aby uživatelé měli vytvořit soubor .zip, který obsahuje všechny tooOneDrive toomove soubory hello pro firmy.
3. Požádejte uživatele toogo toohello Office 365 portál a potom přejděte tooOneDrive a nahrajte soubor ZIP hello.

## <a name="copy-files-by-using-drive-redirection"></a>Zkopírujte soubory pomocí přesměrování jednotky
Pokud jste povolili [jednotka přesměrování](remoteapp-redirection.md), jste již vytvořili mapované jednotky pro vaše uživatele. V takovém případě můžete své soubory na disku hello přesměrováno zip a uložit je tootheir místní počítač.

## <a name="how-administrators-can-export-data"></a>Jak mohou správci exportovat data

Spravuje pro Azure RemoteApp můžete exportovat všechny disky profilu uživatele (UPD) pro všechny kolekce v rámci předplatného tooAzure úložiště pomocí prostředí Azure PowerShell rutiny Export-AzureRemoteAppUserDisk.  Neexistuje žádná možnost tooselect jednotlivých UPD společnosti.  Po provedení hello příkaz prostředí PowerShell každého disku, uživatel bude 50gb velikost pevného disku a být exportovaný tooAzure úložiště.  Pro toto úložiště je možné hned za náklady na úložiště Azure.  Při spuštění tohoto příkazu zkontrolujte neexistují žádné relace, jinak export hello selžou.

UPD je připojený k doméně Azure RemoteApp nasazení lze použít pouze znovu v nasazení služby Vzdálená plocha, nelze použít nasazení připojený k doméně.  Pokud tyto disky se použije v nasazení služby Vzdálená plocha doporučujeme toouse naše [automatizované skripty](https://github.com/arcadiahlyy/aramigration) , bude exportovat, převod a import hello na UPD do nasazení služby Vzdálená plocha.

