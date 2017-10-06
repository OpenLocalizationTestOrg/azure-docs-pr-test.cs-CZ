---
title: "aaaHow toocreate, spravovat nebo odstranit účet úložiště v hello portálu Azure | Microsoft Docs"
description: "Vytvořit nový účet úložiště, spravovat klíče pro přístup k účtu nebo odstranit účet úložiště na hello portálu Azure. Získejte informace o účtech úložiště Standard a Premium."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 87c37da0-6cc6-4d88-a330-ef2896a1531d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
f1_keywords: sql13.swb.windowsazurestorage.connect.f1
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: 3a2a07c4131fbe594c7007f59950bf94339809fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-storage-accounts"></a>Informace o účtech Azure Storage
[!INCLUDE [storage-selector-portal-create-storage-account](../../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Přehled
Účet úložiště Azure poskytuje jedinečný obor názvů toostore a přístup k vaší datové objekty Azure Storage. Všechny objekty v rámci účtu úložiště se fakturují společně jako skupina. Ve výchozím nastavení je hello data ve vašem účtu dostupná pouze tooyou hello vlastníka účtu.

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Fakturace účtu úložiště
[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

> [!NOTE]
> Když vytvoříte virtuální počítač Azure, účet úložiště se vytvoří pro vás automaticky v umístění nasazení hello Pokud již nemáte účet úložiště v tomto umístění. Takže není nutné toofollow hello kroků toocreate účet úložiště pro disky virtuálního počítače. název účtu úložiště Hello budou založeny na název virtuálního počítače hello. V tématu hello [dokumentace Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) další podrobnosti.
> 
> 

## <a name="storage-account-endpoints"></a>Koncové body účtu úložiště
Každý objekt, který uložíte v úložišti Azure Storage, má jedinečnou adresu URL. forms název účtu úložiště Hello hello subdoménu dané adresy. Hello kombinace názvu domény a subdomény, která je služba konkrétní tooeach, tvoří *koncový bod* pro váš účet úložiště.

Například pokud je název účtu úložiště *můj_účet_úložiště*, pak jsou výchozí koncové body hello pro váš účet úložiště:

* Služba Blob service: http://*můj_účet_úložiště*.blob.core.windows.net
* Služba Table: http://*můj_účet_úložiště*.table.core.windows.net
* Služba front: http://*můj_účet_úložiště*.queue.core.windows.net
* Služba souborů: http://*můj_účet_úložiště*.file.core.windows.net

> [!NOTE]
> Účet úložiště Blob pouze zpřístupní koncový bod služby objektů Blob hello.
> 
> 

Hello adresu URL pro přístup k objektu v účtu úložiště je sestavena připojením umístění objektu hello v hello koncový bod toohello účtu úložiště. Například adresa účtu pro objekty blob může mít tento formát: http://*můj_účet_úložiště*.blob.core.windows.net/*můj_kontejner*/*můj_objekt_blob*.

Můžete také nakonfigurovat toouse název vlastní doménu se svým účtem úložiště. Další informace najdete v tématu [Konfigurace vlastního názvu doménu pro koncový bod služby Blob Storage](../blobs/storage-custom-domain-name.md). Konfiguraci je možné také provést pomocí PowerShellu. Další informace najdete v tématu hello [Set AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) rutiny.  


## <a name="create-a-storage-account"></a>vytvořit účet úložiště
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello vyberte **nový** -> **úložiště** -> **účet úložiště**.
3. Zadejte název účtu úložiště. V tématu [koncové body účtu úložiště](#storage-account-endpoints) pro podrobnosti o tom, jak bude název účtu úložiště hello být použité tooaddress vašich objektů v Azure Storage.
   
   > [!NOTE]
   > Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.
   > 
   > Váš název účtu úložiště musí být jedinečný v rámci Azure. Hello portál Azure označí, pokud hello název účtu úložiště, kterou vyberete, je již používán.
   > 
   > 
4. Zadejte toobe model nasazení hello používá: **Resource Manager** nebo **Classic**. **Správce prostředků** doporučuje hello modelu nasazení. Další informace najdete v tématu [Vysvětlení nasazení pomocí modelu nasazení Resource Manager a modelu Classic](../../azure-resource-manager/resource-manager-deployment-model.md).
   
   > [!NOTE]
   > Účty úložiště BLOB mohou být vytvořeny pouze pomocí modelu nasazení Resource Manager hello.

5. Vyberte typ účtu úložiště hello: **obecné účely** nebo **úložiště objektů Blob**. **Obecné účely** je výchozí hello.
   
    Pokud **obecné účely** byla vybrána, a zadejte úroveň výkonu hello: **standardní** nebo **Premium**. Výchozí hodnota Hello je **standardní**. Další podrobnosti o účtech úložiště standard a premium najdete v tématu [tooMicrosoft Úvod Azure Storage](storage-introduction.md) a [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).
   
    Pokud **úložiště objektů Blob** byla vybrána, a zadejte úroveň přístupu hello: **horká** nebo **nástrojů**. Výchozí hodnota Hello je **horká**. Další podrobnosti najdete v tématu [Azure Blob Storage: úrovně Cool a Hot](../blobs/storage-blob-storage-tiers.md).
6. Vyberte možnost hello replikace pro účet úložiště hello: **LRS**, **GRS**, **RA-GRS**, nebo **ZRS**. Výchozí hodnota Hello je **RA-GRS**. Další informace o možnostech replikace pro Azure Storage najdete v tématu [Replikace Azure Storage](storage-redundancy.md).
7. Vyberte hello předplatné, ve kterém chcete toocreate hello nový účet úložiště.
8. Zadejte novou skupinu prostředků nebo vyberte existující skupinu prostředků. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).
9. Vyberte hello zeměpisné umístění účtu úložiště. V článku [Oblasti Azure](https://azure.microsoft.com/regions/#services) najdete další informace o tom, které služby jsou dostupné v daných oblastech.
10. Klikněte na tlačítko **vytvořit** účet úložiště toocreate hello.

## <a name="manage-your-storage-account"></a>Správa účtu úložiště
### <a name="change-your-account-configuration"></a>Změna konfigurace účtu
Po vytvoření účtu úložiště, můžete upravit jeho konfiguraci, jako je například změna hello možnost replikace používanou pro účet hello nebo změna hello úroveň přístupu pro účet úložiště Blob. V hello [portál Azure](https://portal.azure.com), přejděte tooyour účet úložiště, vyhledejte a klikněte na tlačítko **konfigurace** pod **nastavení** tooview nebo změna konfigurace účtu hello.

> [!NOTE]
> V závislosti na úroveň výkonu hello, kterou jste zvolili při vytváření účtu úložiště hello nemusí být některé možnosti replikace dostupné.
> 
> 

Změna hello možnost replikace se změní cena za předplatné. Další informace najdete na stránce [s cenami za Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Pro objekt Blob, které účty úložiště, změna úrovně přístupu hello mohou být účtovány poplatky za hello změní kromě toochanging cena za předplatné. Najdete v tématu hello [účty Blob storage – ceny a fakturace](../blobs/storage-blob-storage-tiers.md#pricing-and-billing) další podrobnosti.

### <a name="manage-your-storage-access-keys"></a>Správa přístupových klíčů k úložišti
Při vytváření účtu úložiště vygeneruje Azure dva 512bitové přístupové klíče k úložišti, které se použijí k ověření při přístupu k účtu úložiště hello. Poskytnutím dvou přístupových klíčů k úložišti Azure vám umožní tooregenerate hello klíče bez přerušení tooyour úložiště služby a služby toothat přístup.

> [!NOTE]
> Doporučujeme vám přístupové klíče k úložišti s nikým nesdílet. toopermit toostorage přístup k prostředkům bez poskytnutí přístupových klíčů, můžete použít *sdílený přístupový podpis*. Sdílený přístupový podpis poskytuje přístup tooa prostředků ve vašem účtu pro časový interval, který definujete a s hello oprávnění, které zadáte. Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md).
> 
> 
<a id="view-and-copy-storage-access-keys"/></a>
#### <a name="view-and-copy-storage-access-keys"></a>Zobrazení a zkopírování přístupových klíčů k úložišti
V hello [portál Azure](https://portal.azure.com)přejděte tooyour účet úložiště, klikněte na tlačítko **všechna nastavení** a pak klikněte na **přístupové klíče** tooview, kopírovat a opět vytvořit klíče pro přístup k účtu. Hello **přístupové klíče** okno také obsahuje předem nakonfigurované připojovací řetězce, pomocí vaší primární a sekundární klíče, můžete zkopírovat toouse ve svých aplikacích.

#### <a name="regenerate-storage-access-keys"></a>Opětovné vygenerování přístupových klíčů k úložišti
Doporučujeme vám, že změníte hello přístupové klíče účtu úložiště tooyour pravidelně toohelp zajistit zabezpečení připojení k úložišti. Dva přístupové klíče se přiřazují proto, abyste mohli pro účet úložiště toohello připojení pomocí jeden přístupový klíč, zatímco si znovu vygenerujete hello druhý přístupový klíč.

> [!WARNING]
> Opětovné generování přístupových klíčů může ovlivnit služby v Azure, jakož i vlastní aplikace, které jsou závislé na účtu úložiště hello. Všichni klienti, kteří používají účet úložiště hello přístupu k klíče tooaccess hello musí být aktualizované toouse hello nový klíč.
> 
> 

**Služba Media services** – Pokud máte služby media services, které jsou závislé na vašem účtu úložiště, musíte znovu synchronizovat hello přístupové klíče s touto službou po opětovném vygenerování klíčů hello.

**Aplikace** – Pokud máte webové aplikace nebo cloudových služeb daný účet úložiště hello použijte, pokud, ztratíte připojení hello obnovit klíče, klíče nezaregistrujete.

**Průzkumníci úložiště** – Pokud používáte některou [aplikací Průzkumníka úložiště](storage-explorers.md), budete pravděpodobně potřebovat tooupdate hello úložiště klíč používaný těmito aplikacemi.

Tady je hello proces pro výměnu přístupových klíčů úložiště:

1. Aktualizujte připojovací řetězce hello ve vaší aplikaci kód tooreference hello sekundární přístupový klíč účtu úložiště hello.
2. Znovu vygenerujte primární přístupový klíč hello vašeho účtu úložiště. Na hello **přístupové klíče** okně klikněte na tlačítko **znovu vygenerovat klíč1**a potom klikněte na **Ano** tooconfirm, které chcete toogenerate nový klíč.
3. Aktualizujte hello připojovacích řetězců v kódu tooreference hello nové primární přístupový klíč.
4. Opětovné vytváření hello sekundární přístupový klíč v hello stejný způsobem.

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště
tooremove účet úložiště, který už nepoužíváte, přejděte toohello účet úložiště v hello [portál Azure](https://portal.azure.com)a klikněte na tlačítko **odstranit**. Odstranění účtu úložiště se odstraní celý účet hello, včetně všech dat v účtu hello.

> [!WARNING]
> Není možné toorestore je účet úložiště se odstranil nebo načíst žádný obsah hello, který obsahuje před odstraněním. Být jisti tooback až nic chcete toosave před odstraněním účtu hello. To platí také pro všechny prostředky v hello účtu – po odstranění jsou objekt blob, tabulka, fronta nebo soubor, je trvale odstraněn.
> 

Pokud se pokusíte toodelete účet úložiště přidružený virtuální počítač Azure, můžete obdržet chybu o účtu úložiště hello používaný. Pomoc při odstraňování potíží s touto chybou najdete v tématu popisujícím [řešení chyb při odstraňování účtů úložiště](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

## <a name="next-steps"></a>Další kroky
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* [Azure Blob Storage: úrovně Cool a Hot](../blobs/storage-blob-storage-tiers.md)
* [Účet replikace Azure Storage](storage-redundancy.md)
* [Nakonfigurování připojovacích řetězců Azure Storage](../storage-configure-connection-string.md)
* [Přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md)
* Navštivte hello [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).

