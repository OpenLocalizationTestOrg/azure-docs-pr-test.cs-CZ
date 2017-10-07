---
title: "aaaHow toocreate sdílenou složku Azure | Microsoft Docs"
description: "Jak toocreate Azure soubor sdílet v Azure File storage pomocí hello portálu Azure, PowerShell a hello rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 816694e411a993dae881816fc62173e2b7afe990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a>Vytvoření sdílené složky ve službě Azure File Storage
Můžete vytvořit sdílené složky Azure File pomocí [portál Azure](https://portal.azure.com/)hello rutin Powershellu pro úložiště Azure, hello knihovny klienta Azure Storage nebo REST API pro Azure Storage hello. V tomto kurzu se dozvíte:
* [Jak toocreate Azure File sdílet pomocí hello portálu Azure](#Create file share through hello Portal)
* [Jak toocreate Azure File sdílet pomocí prostředí Powershell](#Create file share using PowerShell)
* [Jak toocreate Azure File sdílet pomocí rozhraní příkazového řádku](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Požadavky
toocreate sdílenou složku Azure, můžete použít účet úložiště, který již existuje, nebo [vytvořit nový účet úložiště Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). toocreate sdílenou Azure pomocí prostředí PowerShell, budete potřebovat název svého účtu úložiště a klíč účtu hello... Pokud máte v plánu toouse prostředí Powershell nebo rozhraní příkazového řádku, budete potřebovat klíč účtu úložiště.

## <a name="create-file-share-through-hello-portal"></a>Vytvoření sdílené složky prostřednictvím hello portálu
1. **Okno účtu tooStorage přejděte na portálu Azure**:    
    ![Okno Účet úložiště](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Klikněte na tlačítko Přidat sdílenou složku:**    
    ![Klikněte na tlačítko hello přidat sdílené složky souboru – tlačítko](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Zadejte název a kvótu. Aktuálně může být kvóta maximálně 5 TB:**    
    ![Zadejte název a požadované kvótu pro hello nové sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Zobrazte novou sdílenou složku:**![Zobrazení nové sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Nahrajte soubor:**![Nahrání souboru](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Přejděte do sdílené složky a spravujte adresáře a soubory:**![Procházení sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Vytvoření sdílené složky prostřednictvím PowerShellu
tooprepare toouse prostředí PowerShell, stáhněte a nainstalujte hello rutin prostředí Azure PowerShell. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) hello nainstalovat bod a pokyny k instalaci.

> [!Note]  
> Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější modul Azure PowerShell.

1. **Vytvoření kontextu pro účet úložiště a klíč** hello kontext obsahuje hello klíč účtu úložiště účet a název. Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com/) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Vytvořte novou sdílenou složku:**    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> Hello název sdílené složky musí být psaný malými písmeny. Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-file-share-through-command-line-interface-cli"></a>Vytvoření sdílené složky prostřednictvím rozhraní příkazového řádku (CLI)
1. **tooprepare toouse rozhraní příkazového řádku (CLI), stáhněte a nainstalujte hello rozhraní příkazového řádku Azure.**  
    Viz témata [Instalace Azure CLI 2.0](/cli/azure/install-az-cli2.md) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).

2. **Vytvoření účtu úložiště toohello řetězec připojení místo toocreate hello sdílené složky.**  
    Nahraďte ```<storage-account>``` a ```<resource_group>``` s váš účet název a prostředků skupiny úložišť v hello následující ukázka.

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. **Vytvoření sdílené složky**
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a>Další kroky
* [Připojení ke sdílené složce a její připojení – Windows](storage-how-to-use-files-windows.md)
* [Připojení ke sdílené složce a její připojení – Linux](../storage-how-to-use-files-linux.md)
* [Připojení ke sdílené složce a její připojení – macOS](storage-how-to-use-files-mac.md)

Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Nejčastější dotazy](../storage-files-faq.md)
* [Řešení potíží ve Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Řešení potíží v Linuxu](storage-troubleshoot-linux-file-connection-problems.md)   