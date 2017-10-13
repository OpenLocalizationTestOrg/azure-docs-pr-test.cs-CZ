---
title: "Vytvoření sdílené složky Azure | Dokumentace Microsoftu"
description: "Postup vytvoření sdílené složky Azure ve službě Soubory Azure pomocí webu Azure Portal, PowerShellu a Azure CLI."
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
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: bc01e5427f32e9532e39694f6de9f0b1146eda35
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-file-share-in-azure-files"></a>Vytvoření sdílené složky ve službě Soubory Azure
Sdílené složky Azure můžete vytvářet pomocí webu [Azure Portal](https://portal.azure.com/), rutin PowerShellu pro Azure Storage, klientských knihoven Azure Storage nebo rozhraní REST API pro Azure Storage. V tomto kurzu se dozvíte:
* [Jak vytvořit sdílenou složku Azure pomocí webu Azure Portal](#Create file share through the Portal)
* [Jak vytvořit sdílenou složku Azure pomocí PowerShellu](#Create file share using PowerShell)
* [Jak vytvořit sdílenou složku Azure pomocí rozhraní příkazového řádku](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Požadavky
Pokud chcete vytvořit sdílenou složku Azure, můžete použít už existující účet úložiště nebo [vytvořit nový účet služby Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Pokud chcete vytvořit sdílenou složku Azure pomocí PowerShellu, budete potřebovat klíč účtu a název účtu úložiště. Pokud se chystáte použít PowerShell nebo rozhraní příkazového řádku, budete potřebovat klíč účtu úložiště.

## <a name="create-file-share-through-the-azure-portal"></a>Vytvoření sdílené složky prostřednictvím webu Azure Portal
1. **Přejděte do okna Účet úložiště na webu Azure Portal:**    
    ![Okno Účet úložiště](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Klikněte na tlačítko Přidat sdílenou složku:**    
    ![Kliknutí na tlačítko Přidat sdílenou složku](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Zadejte název a kvótu. Aktuálně může být kvóta maximálně 5 TiB:**    
    ![Zadání názvu a požadované kvóty pro novou sdílenou složku](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Zobrazte novou sdílenou složku:**![Zobrazení nové sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Nahrajte soubor:**![Nahrání souboru](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Přejděte do sdílené složky a spravujte adresáře a soubory:**![Procházení sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Vytvoření sdílené složky prostřednictvím PowerShellu
K používání PowerShellu budete potřebovat stáhnout a nainstalovat rutiny modulu Azure PowerShell. Bod instalace a pokyny pro instalaci jsou popsané v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).

> [!Note]  
> Doporučuje se stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell.

1. **Vytvořte kontext pro účet úložiště a klíč.** Kontext obsahuje název účtu úložiště a klíč účtu. Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com/) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Vytvořte novou sdílenou složku:**    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> Název vaší sdílené složky musí obsahovat jen malá písmena. Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-file-share-through-command-line-interface-cli"></a>Vytvoření sdílené složky prostřednictvím rozhraní příkazového řádku (CLI)
1. **V rámci přípravy na použití rozhraní příkazového řádku (CLI) stáhněte a nainstalujte Azure CLI.**  
    Viz témata [Instalace Azure CLI 2.0](/cli/azure/install-az-cli2.md) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).

2. **Vytvořte připojovací řetězec k účtu úložiště, kde chcete sdílenou složku vytvořit.**  
    V následujícím příkladu nahraďte ```<storage-account>``` a ```<resource_group>``` názvem svého účtu úložiště a skupinou prostředků:

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
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

Další informace o službě Soubory Azure najdete na těchto odkazech.

* [Nejčastější dotazy](../storage-files-faq.md)
* [Řešení potíží ve Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Řešení potíží v Linuxu](storage-troubleshoot-linux-file-connection-problems.md)   