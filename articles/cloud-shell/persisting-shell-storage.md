---
title: "aaaPersist soubory v prostředí cloudu Azure (Preview) | Microsoft Docs"
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
ms.openlocfilehash: b230453d5551c545553d63559741950206e4f1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a>Zachovat soubory v prostředí cloudu Azure
Cloudové prostředí využívá soubory toopersist úložiště Azure File napříč relacemi.

## <a name="set-up-a-clouddrive-file-share"></a>Nastavení `clouddrive` sdílené složky
Při počáteční spuštění prostředí cloudu vás vyzve k tooassociate nový nebo existující sdílenou toopersist soubory mezi relace.

### <a name="create-new-storage"></a>Vytvoření nového úložiště

Když používáte základní nastavení a vyberte pouze předplatné, cloudové prostředí vytvoří tři zdroje vaším jménem v hello podporované oblasti, které je nejblíže tooyou:
* Skupina prostředků:`cloud-shell-storage-<region>`
* Účet úložiště:`cs<uniqueGuid>`
* Sdílené složky:`cs-<user>-<domain>-com-<uniqueGuid>`

![Nastavení předplatného Hello](media/basic-storage.png)

sdílenou složku Hello připojí jako `clouddrive` ve vaší `$Home` adresáře. Hello je sdílená složka také použít toostore 5 GB bitové kopie, který je automaticky vytvořen a která aktualizuje a přetrvává vaší `$Home` adresáře. Toto je jednorázová akce a sdílenou složku hello automaticky připojí v následné relace.

### <a name="use-existing-resources"></a>Používat existující prostředky

Pomocí hello rozšířené možnosti, můžete přidružit existující prostředky. Jakmile se zobrazí hello úložiště instalace řádku, vyberte **zobrazit upřesňující nastavení** tooview další možnosti. Existující sdílené složky přijímat toopersist bitové kopie 5 GB uživatele vaše `$Home` adresáře. Hello rozevíracích nabídek jsou filtrovány pro vaši oblast cloudové prostředí a pro účty místní redundantní & geograficky redundantní úložiště.

![nastavení skupiny prostředků Hello](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Omezit vytvoření prostředku zásadami prostředků Azure.
Účty úložiště, které vytvoříte v prostředí cloudu jsou označené `ms-resource-usage:azure-cloud-shell`. Pokud chcete toodisallow uživatelům ve vytváření účtů úložiště v prostředí cloudu, vytvořte [zásad prostředků Azure pro značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) jsou aktivovány tato konkrétní značka.

## <a name="how-cloud-shell-works"></a>Jak funguje cloudové prostředí
Cloudové prostředí potrvají soubory pomocí obou hello následující metody:
* Vytvoření image disku vaší `$Home` directory toopersist všechny obsah v adresáři hello. bitová kopie disku Hello je uložen v zadané sdílené složky jako `acc_<User>.img` v `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, a je automaticky synchronizuje změny.

* Připojení zadané sdílené složky jako `clouddrive` ve vaší `$Home` adresář pro přímé interakci se sdílené složky. `/Home/<User>/clouddrive`je mapován příliš`fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Všechny soubory ve vašem `$Home` adresář, jako jsou klíče SSH, jsou nastavené jako trvalé v bitové kopii disku uživatele, který je uložený v připojené sdílené složky. Použít osvědčené postupy při zachování informace ve vaší `$Home` adresář a připojené sdílené složky.

## <a name="use-hello-clouddrive-command"></a>Použití hello `clouddrive` příkaz
S cloudové prostředí, můžete spustit příkaz s názvem `clouddrive`, což umožňuje vám toomanually aktualizace hello sdílená toho připojené tooCloud prostředí.
![Spuštěním příkazu "clouddrive" hello](media/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Připojit nový`clouddrive`

### <a name="prerequisites-for-manual-mounting"></a>Předpoklady pro ruční připojení
Můžete aktualizovat hello sdílené složky, který je spojen s cloudové prostředí pomocí hello `clouddrive mount` příkaz.

Pokud připojíte existující sdílené složky, musí být účty úložiště hello:
* Místně redundantní úložiště nebo sdílené složky toosupport geograficky redundantní úložiště.
* Nachází se ve vašem regionu přiřazené. Pokud jste registrace, hello oblast, která máte přiřazená toois uvedené v název skupiny prostředků hello `cloud-shell-storage-<region>`.

### <a name="supported-storage-regions"></a>Oblasti podporované úložiště
Hello soubory Azure se musí nacházet v hello stejné oblasti jako počítač hello cloudové prostředí, který jste jim připojení. Clustery prostředí cloudu se momentálně existují v hello následující oblasti:
|Oblast|Oblast|
|---|---|
|Amerika|Východ USA, střed USA – Jih, západ USA|
|Evropa|Severní Evropa, Západní Evropa|
|Asie a Tichomoří|Indie centrální, jihovýchodní Asie|

### <a name="hello-clouddrive-mount-command"></a>Hello `clouddrive mount` příkaz

> [!NOTE]
> Pokud jste připojení nové sdílené složky, vytvoří se nové uživatelské image pro vaše `$Home` adresáře, protože váš předchozí `$Home` bitové kopie je uložen v hello předchozí sdílené složky.

Spustit hello `clouddrive mount` s hello následující parametry:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Spusťte další podrobnosti, tooview `clouddrive mount -h`, jak je vidět tady:

![Spuštěné hello ' clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a>Odpojení`clouddrive`
To je připojený tooCloud prostředí Shell kdykoli sdílené složky můžete odpojit. Po nepřipojené sdílené složky bude výzvami toomount nové tooyour předchozí sdílené složky souboru další relace.

tooremove soubor sdílet z prostředí cloudu:
1. Spusťte `clouddrive unmount`.
2. Na vědomí a potvrďte hello výzvy.

Sdílené složky bude pokračovat tooexist, pokud chcete odstranit ručně. Cloudové prostředí už Hledat pro tuto sdílenou složku na následné relace.

Spusťte další podrobnosti, tooview `clouddrive unmount -h`, jak je vidět tady:

![Spuštěné hello ' clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> Spuštění tohoto příkazu se neodstraní žádné prostředky. Ručním odstranění skupiny prostředků, účet úložiště nebo sdílené složky, která je namapovaná tooCloud dojde k trvalému odstranění prostředí vaší `$Home` directory image a další soubory do sdílené složky. Tuto akci nelze vrátit zpět.

## <a name="list-clouddrive-file-shares"></a>Seznam `clouddrive` sdílené složky
které sdílené složky je připojit jako toodiscover `clouddrive`, spusťte následující hello `df` příkaz. 

tooclouddrive cesta k souboru Hello ukazuje, že váš název účtu úložiště a sdílených složek v adrese URL hello. Například `//storageaccountname.file.core.windows.net/filesharename`.

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

## <a name="transfer-local-files-toocloud-shell"></a>Přenos místních souborů tooCloud prostředí
Hello `clouddrive` synchronizace adresáře se okno hello portálu úložiště Azure. Pomocí této tooor okno tootransfer místních souborů ze sdílené složky. Aktualizace soubory z prostředí cloudu se projeví v úložišti file hello grafického uživatelského rozhraní při aktualizaci okna hello.

### <a name="download-files"></a>Stažení souborů

![Seznam místních souborů](media/download.png)
1. V hello portálu Azure přejděte toohello připojené sdílené složky.
2. Vyberte cílový soubor hello.
3. Vyberte hello **Stáhnout** tlačítko.

### <a name="upload-files"></a>Nahrání souborů

![Nahrát toobe místních souborů](media/upload.png)
1. Přejděte tooyour připojit sdílenou složku.
2. Vyberte hello **nahrát** tlačítko.
3. Vyberte soubory, které chcete tooupload, hello.
4. Potvrďte nahrávání hello.

Teď byste měli vidět hello soubory, které jsou dostupné ve vaší `clouddrive` adresář v prostředí cloudu.

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md) <br>
[Další informace o Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Další informace o značkách úložiště](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
