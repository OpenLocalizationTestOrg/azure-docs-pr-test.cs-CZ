---
title: "Zachovat soubory v prostředí cloudu Azure (Preview) | Microsoft Docs"
description: "Návod, jak Azure Cloud prostředí potrvají soubory."
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
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 61a8bfcf3704f361432400771d8fcc8b81927b53
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a>Zachovat soubory v prostředí cloudu Azure
Cloudové prostředí využívá Azure File storage pro soubory zachová napříč relacemi.

## <a name="set-up-a-clouddrive-file-share"></a>Nastavení `clouddrive` sdílené složky
Při počáteční spuštění prostředí cloudu vás vyzve k přidružit nové nebo existující sdílenou složku souborů zachová napříč relacemi.

### <a name="create-new-storage"></a>Vytvoření nového úložiště

Když používáte základní nastavení a vyberte pouze předplatné, cloudové prostředí vytvoří tři zdroje vaším jménem v podporované oblasti, které je nejblíže můžete:
* Skupina prostředků:`cloud-shell-storage-<region>`
* Účet úložiště:`cs<uniqueGuid>`
* Sdílené složky:`cs-<user>-<domain>-com-<uniqueGuid>`

![Nastavení předplatného](media/basic-storage.png)

Sdílené složky připojí jako `clouddrive` ve vaší `$Home` adresáře. Sdílené složky se používá i k uložení 5 GB image, který je pro vás vytvořil a který automaticky aktualizuje a přetrvává vaší `$Home` adresáře. Toto je jednorázová akce a sdílené složky automaticky připojí v následné relace.

### <a name="use-existing-resources"></a>Používat existující prostředky

Pomocí pokročilé možnosti můžete přidružit existující prostředky. Po zobrazení výzvy se instalační program úložiště, vyberte **zobrazit upřesňující nastavení** zobrazíte další možnosti. Existující sdílené složky zobrazí 5 GB uživatelská image se zachovat vaše `$Home` adresáře. Rozevíracích nabídek jsou filtrovány pro vaši oblast cloudové prostředí a pro účty místní redundantní & geograficky redundantní úložiště.

![Nastavení skupiny prostředků](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Omezit vytvoření prostředku zásadami prostředků Azure.
Účty úložiště, které vytvoříte v prostředí cloudu jsou označené `ms-resource-usage:azure-cloud-shell`. Pokud chcete zakázat uživatelům ve vytváření účtů úložiště v prostředí cloudu, vytvořte [zásad prostředků Azure pro značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) jsou aktivovány tato konkrétní značka.

## <a name="how-cloud-shell-works"></a>Jak funguje cloudové prostředí
Cloudové prostředí potrvají soubory pomocí obě z následujících metod:
* Vytvoření image disku vaší `$Home` adresáře se zachovat veškerý obsah v adresáři. Bitové kopie disku je uložen v zadané sdílené složky jako `acc_<User>.img` v `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, a je automaticky synchronizuje změny.

* Připojení zadané sdílené složky jako `clouddrive` ve vaší `$Home` adresář pro přímé interakci se sdílené složky. `/Home/<User>/clouddrive`se mapuje na `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Všechny soubory ve vašem `$Home` adresář, jako jsou klíče SSH, jsou nastavené jako trvalé v bitové kopii disku uživatele, který je uložený v připojené sdílené složky. Použít osvědčené postupy při zachování informace ve vaší `$Home` adresář a připojené sdílené složky.

## <a name="use-the-clouddrive-command"></a>Použití `clouddrive` příkaz
S cloudové prostředí, můžete spustit příkaz s názvem `clouddrive`, což umožňuje ručně aktualizovat sdílené složky, která je připojena k cloudové prostředí.
![Spuštěním příkazu "clouddrive"](media/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Připojit nový`clouddrive`

### <a name="prerequisites-for-manual-mounting"></a>Předpoklady pro ruční připojení
Sdílené složky, který je spojen s cloudové prostředí pomocí můžete aktualizovat `clouddrive mount` příkaz.

Pokud připojíte existující sdílené složky, musí být účty úložiště:
* Místně redundantní úložiště nebo geograficky redundantní úložiště pro podporu sdílených složek.
* Nachází se ve vašem regionu přiřazené. Po registraci oblasti jsou přiřazeny k je uvedena v názvu skupiny prostředků `cloud-shell-storage-<region>`.

### <a name="supported-storage-regions"></a>Oblasti podporované úložiště
Soubory Azure se musí nacházet ve stejné oblasti jako počítač cloudové prostředí, které jste jim připojení. Clustery prostředí cloudu aktuálně existuje v následujících oblastech:
|Oblast|Oblast|
|---|---|
|Amerika|Východ USA, střed USA – Jih, západ USA|
|Evropa|Severní Evropa, Západní Evropa|
|Asie a Tichomoří|Indie centrální, jihovýchodní Asie|

### <a name="the-clouddrive-mount-command"></a>`clouddrive mount` Příkaz

> [!NOTE]
> Pokud jste připojení nové sdílené složky, vytvoří se nové uživatelské image pro vaše `$Home` adresáře, protože váš předchozí `$Home` bitové kopie je uložen v předchozí sdílené složky.

Spustit `clouddrive mount` příkazu s následujícími parametry:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Chcete-li zobrazit další podrobnosti, spusťte `clouddrive mount -h`, jak je vidět tady:

![Spuštění ' clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a>Odpojení`clouddrive`
Sdílení souborů, které je připojené k prostředí cloudu kdykoli můžete odpojit. Jakmile je vaše sdílená složka nepřipojené, vyzve k připojit nové sdílené složky před další relace.

Odebrat sdílenou složku z prostředí cloudu:
1. Spusťte `clouddrive unmount`.
2. Na vědomí a potvrďte výzvy.

Sdílené složky budou nadále existovat, pokud chcete odstranit ručně. Cloudové prostředí už Hledat pro tuto sdílenou složku na následné relace.

Chcete-li zobrazit další podrobnosti, spusťte `clouddrive unmount -h`, jak je vidět tady:

![Spuštění ' clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> Spuštění tohoto příkazu se neodstraní žádné prostředky. Ručním odstranění skupiny prostředků, účet úložiště nebo sdílené složky, který je namapovaný na cloudové prostředí dojde k trvalému odstranění vaší `$Home` directory image a další soubory do sdílené složky. Tuto akci nelze vrátit zpět.

## <a name="list-clouddrive-file-shares"></a>Seznam `clouddrive` sdílené složky
Chcete-li zjistit, které sdílené složky je připojit jako `clouddrive`, spusťte následující `df` příkaz. 

Cestu k souboru na clouddrive ukazuje, že váš název účtu úložiště a sdílených složek v adrese URL. Například `//storageaccountname.file.core.windows.net/filesharename`.

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-to-cloud-shell"></a>Přenos místních souborů do prostředí cloudu
`clouddrive` Synchronizace adresáře se v okně portálu úložiště Azure. Pomocí tohoto okna pro přenos místní soubory do nebo ze sdílené složky. Aktualizace soubory z prostředí cloudu se projeví v úložišti file grafického uživatelského rozhraní při aktualizaci okna.

### <a name="download-files"></a>Stažení souborů

![Seznam místních souborů](media/download.png)
1. Na portálu Azure přejděte do sdílené složky připojeného souboru.
2. Vyberte cílový soubor.
3. Vyberte **Stáhnout** tlačítko.

### <a name="upload-files"></a>Nahrání souborů

![Místní soubory k odeslání](media/upload.png)
1. Přejděte do sdílené složky připojeného souboru.
2. Vyberte **nahrát** tlačítko.
3. Vyberte soubor nebo soubory, které chcete nahrát.
4. Potvrďte nahrávání.

Teď byste měli vidět soubory, které jsou dostupné ve vaší `clouddrive` adresář v prostředí cloudu.

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md) <br>
[Další informace o Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Další informace o značkách úložiště](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
