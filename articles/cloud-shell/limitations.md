---
title: "Omezení Azure Cloud prostředí | Microsoft Docs"
description: "Přehled omezení prostředí cloudu Azure"
services: azure
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
ms.date: 01/17/2018
ms.author: juluk
ms.openlocfilehash: 7e498582d78d2807070c943dfd838dd9efeb4ed2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="limitations-of-azure-cloud-shell"></a>Omezení prostředí cloudu Azure

Prostředí Azure Cloud má tato známá omezení:

## <a name="general-limitations"></a>Obecná omezení

### <a name="system-state-and-persistence"></a>Stav systému a trvalost

Na počítač, který obsahuje vaše cloudové prostředí relace je dočasný a bude recyklována po vaše relace je neaktivní po dobu 20 minut. Cloudové prostředí vyžaduje sdílenou složku Azure chcete připojit. Vaše předplatné v důsledku toho musí být schopni nastavit prostředků úložiště pro přístup k prostředí cloudu. Mezi další aspekty patří:

* S použitím připojené úložiště, pouze změny v rámci `clouddrive` adresáře jsou nastavené jako trvalé. V Bash vaše `$Home` adresáře je také jako trvalý.
* Sdílené složky Azure může být připojen pouze z uvnitř vaší [přiřazené oblast](persisting-shell-storage.md#mount-a-new-clouddrive).
  * V Bash, spusťte `env` najít vaší oblasti nastavit jako `ACC_LOCATION`.
* Soubory Azure podporuje pouze místně redundantního úložiště a účty geograficky redundantní úložiště.

### <a name="browser-support"></a>Podpora prohlížeče

Cloudové prostředí podporuje nejnovější verze Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox a Apple Safari. Safari v privátním režimu není podporována.

### <a name="copy-and-paste"></a>Kopírování a vkládání

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Pro daného uživatele může být aktivní pouze jeden prostředí

Uživatelé mohou spouštět pouze jeden typ prostředí najednou, buď **Bash** nebo **prostředí PowerShell**. Ale může mít více instancí spuštěné v jednom okamžiku Bash nebo prostředí PowerShell. Vzájemná záměna mezi Bash nebo prostředí PowerShell příčiny cloudové prostředí k restartování, což Ukončí existující relací.

### <a name="usage-limits"></a>Limity využití

Cloudové prostředí je určen pro případy použití interaktivní. V důsledku toho ukončení relací neinteraktivní všechny dlouhodobé bez upozornění.

## <a name="bash-limitations"></a>Omezení bash

### <a name="user-permissions"></a>Uživatelská oprávnění

Máte nastavená oprávnění jako běžní uživatelé bez přístupu sudo. Všechny instalace mimo vaší `$Home` adresáře není trvalý.

### <a name="clouddrive-smb-limited-permissions"></a>Clouddrive SMB omezenými oprávněními
Některé příkazy v rámci `clouddrive` adresář, jako třeba `git clone`, ještě nemá příslušná oprávnění pro čtení/zápisu určitých souborů. Jestli jste nedosáhli tento problém, zkuste to znovu z vaší `$Home` adresář, který nemá omezení protokolu SMB.

### <a name="editing-bashrc"></a>Úpravy .bashrc

Proveďte opatrní při úpravě .bashrc, tak může způsobit neočekávané chyby v prostředí cloudu.

### <a name="bashhistory"></a>.bash_history

Historii bash příkazy může být nekonzistentní z důvodu narušení relace prostředí cloudu nebo souběžných relací.

## <a name="powershell-limitations"></a>Omezení prostředí PowerShell

### <a name="slow-startup-time"></a>Pomalé spuštění

Prostředí PowerShell v prostředí cloudu Azure (Preview) může trvat až 60 sekund k chybě při inicializaci verzi Preview.

### <a name="no-home-directory-persistence"></a>Trvalost No $Home adresáře

Data zapsaná do `$Home` všechny aplikace (například: git, vim a dalších) není zachována mezi relace prostředí PowerShell. Alternativní řešení [zde](troubleshooting.md#powershell-resolutions).

### <a name="default-file-location-when-created-from-azure-drive"></a>Výchozí umístění souboru při vytvoření z disku Azure:

Pomocí rutin prostředí PowerShell, uživatelé nemůžou vytvářet soubory v Azure jednotce. Pokud uživatelé vytvářejí nové soubory pomocí jiných nástrojů, jako je například vim nebo nano, soubory se uloží do složky C:\Users ve výchozím nastavení. 

### <a name="gui-applications-are-not-supported"></a>Grafické uživatelské rozhraní aplikací není podporované.

Pokud uživatel spustí příkaz, který by vytvořit dialogové okno Windows, jako například `Connect-AzureAD` nebo `Login-AzureRMAccount`, například jeden zobrazí chybovou zprávu: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="next-steps"></a>Další postup

[Řešení potíží s cloudové prostředí](troubleshooting.md) <br>
[Rychlý start pro Bash](quickstart.md) <br>
[Rychlý start pro PowerShell](quickstart-powershell.md)
