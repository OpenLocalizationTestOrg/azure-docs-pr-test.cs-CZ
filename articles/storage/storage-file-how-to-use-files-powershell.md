---
title: "aaaHow toouse prostředí PowerShell toomanage Azure File storage | Microsoft Docs"
description: "Přečtěte si toouse prostředí PowerShell toomanage Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 0e30e8796cf8bbf5f9249b26179d5e0f9077c8fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a>Jak toouse prostředí PowerShell toomanage Azure File storage
Můžete použít Azure PowerShell toocreate a spravovat sdílené složky.

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a>Instalace hello rutin prostředí PowerShell pro Azure Storage
tooprepare toouse prostředí PowerShell, stáhněte a nainstalujte hello rutin prostředí Azure PowerShell. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) hello nainstalovat bod a pokyny k instalaci.

> [!NOTE]
> Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější modul Azure PowerShell.
> 
> 

Klikněte na **Start** a zadejte **Windows PowerShell**, tím otevřete okno Azure PowerShell. okno PowerShell Hello načte modul Azure Powershell hello za vás.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Vytvoření kontextu pro účet úložiště a klíč
Vytvořte kontext účtu úložiště hello. kontext Hello obsahuje hello klíč účtu úložiště účet a název. Pokyny pro zkopírování klíče účtu z hello [portál Azure](https://portal.azure.com), najdete v části [zobrazení a zkopírování přístupových klíčů úložiště](storage-create-storage-account.md#view-and-copy-storage-access-keys).

Nahraďte `storage-account-name` a `storage-account-key` s název účtu úložiště a klíč v hello následující ukázka.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Vytvoření nové sdílené složky
Vytvoření hello novou sdílenou složku s názvem `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Teď máte sdílenou složku v úložišti File. Dál přidáme nový adresář a soubor.

> [!IMPORTANT]
> Hello název sdílené složky musí být psaný malými písmeny. Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a>Vytvoření adresáře ve sdílené složce hello
Vytvořte adresář ve sdílené složce hello. V následujícím příkladu hello, adresář hello název `CustomLogs`.

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a>Nahrát adresář toohello místního souboru
Teď nahrajte adresář toohello místního souboru. Hello následujícím příkladu se uloží soubor z `C:\temp\Log1.txt`. Cesta k souboru hello upravte tak, aby ukazoval tooa platný soubor na místním počítači.

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a>Seznam hello soubory v adresáři hello
toosee hello soubor v adresáři hello, můžete seznam všech souborů hello adresáře. Tento příkaz vrátí hello soubory a podadresáře (pokud existují) v adresáři CustomLogs hello.

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile vrátí seznam souborů a adresářů pro jakýkoli adresář, ve kterém se objekt předá. "Get-AzureStorageFile-Share $s" vrátí seznam souborů a adresářů v kořenovém adresáři hello. seznam souborů v podadresáři tooget, máte toopass hello podadresáři tooGet-AzureStorageFile. Se to udělá – první část příkazu hello až toohello kanálu hello vrátí instanci adresáře podadresáře hello CustomLogs. To se potom předá do Get-AzureStorageFile, která vrátí hodnotu hello soubory a adresáře v CustomLogs.

## <a name="copy-files"></a>Kopírování souborů
Od verze 0.9.7 prostředí Azure PowerShell, můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob. Dole ukážeme, jak se tooperform tyto zkopírovat operace pomocí rutin prostředí PowerShell.

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)