---
title: "aaaSecurity funkce, které lze použít s Azure Storage | Microsoft Docs"
description: " Tento článek obsahuje přehled funkcí Azure zabezpečení jádra hello, které lze použít s Azure Storage. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 663cd2705527957d21ff9475a6322b42a16c95e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-security-overview"></a>Přehled zabezpečení úložiště Azure
Azure Storage je řešení cloudového úložiště pro moderní aplikace, které jsou závislé na odolnost, dostupnost a škálovatelnost toomeet hello potřebám zákazníků hello. Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení:

* účet úložiště Hello lze zabezpečit pomocí řízení přístupu na základě Role a Azure Active Directory.
* Data lze zabezpečit během přenosu mezi aplikací a Azure pomocí šifrování na straně klienta, HTTPS nebo SMB 3.0.
* Data lze nastavit toobe šifrují automaticky při zápisu tooAzure úložiště pomocí šifrování služby úložiště.
* Operačního systému a datové disky používat virtuální počítače lze nastavit toobe šifrovat pomocí Azure Disk Encryption.
* Delegovaný přístup toohello datových objektů ve službě Azure Storage můžete udělit pomocí podpisy sdíleného přístupu.
* Metoda ověřování Hello používá někdo při přístupu k úložiště lze sledovat pomocí analytika úložiště.

Pro podrobnější pohled na zabezpečení ve službě Azure Storage, najdete v části hello [Průvodce zabezpečením Azure Storage](../storage/common/storage-security-guide.md). Tato příručka obsahuje podrobné informace do funkce zabezpečení hello Azure Storage například klíče účtu úložiště, šifrování dat při transitu i v rest a analytika úložiště.

Tento článek obsahuje přehled funkcí Azure zabezpečení, které lze použít s Azure Storage. Tooarticles, které poskytují podrobnosti o každé funkce tak další informace jsou k dispozici odkazy.

Zde jsou hello základní funkce toobe popsaná v tomto článku:

* Řízení přístupu na základě rolí
* Objekty toostorage Delegovaný přístup
* Šifrování během přenosu
* Šifrování na rest/šifrování služby úložiště
* Azure Disk Encryption
* Azure Key Vault

## <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Můžete zabezpečit váš účet úložiště pomocí řízení přístupu na základě rolí (RBAC). Omezení přístupu podle hello [potřebovat tooknow](https://en.wikipedia.org/wiki/Need_to_know) a [nejnižší oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) Principy zabezpečení je nutné pro organizace, které mají tooenforce zásady zabezpečení pro přístup k datům. Tato oprávnění jsou uděluje přiřazením toogroups hello odpovídající RBAC role a aplikace v určité oboru. Můžete použít [předdefinované role RBAC](../active-directory/role-based-access-built-in-roles.md), jako je Přispěvatel účet úložiště, tooassign oprávnění toousers.

Další informace:

* [Řízení přístupu na základě role v Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-toostorage-objects"></a>Objekty toostorage Delegovaný přístup
Sdílený přístupový podpis (SAS) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště. Hello SAS znamená udělit, že klient omezené oprávnění tooobjects ve vašem účtu úložiště v zadaném časovém intervalu a se zadanou sadou oprávnění. Tato omezená oprávnění můžete udělit bez nutnosti tooshare klíče pro přístup k účtu. Hello SAS je identifikátor URI, který zahrnuje všechny hello informace potřebné pro ověřený přístup tooa úložiště prostředků v jeho parametry dotazu. tooaccess prostředky úložiště s hello SAS, klient hello pouze potřebuje tooprovide hello SAS toohello odpovídající konstruktor nebo metoda.

Další informace:

* [Vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Vytvoření a použití SAS s úložištěm Blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Šifrování během přenosu
Šifrování během přenosu je mechanismus ochrany dat při přenosu v sítích. S Azure Storage můžete zabezpečit pomocí dat:

* [Šifrování transportní vrstvy](../storage/common/storage-security-guide.md#encryption-in-transit), jako je například HTTPS při přenosu dat do nebo z Azure Storage.
* [Propojit šifrování](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), jako je například šifrování protokolu SMB 3.0 pro sdílené složky Azure File.
* [Šifrování na straně klienta](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), tooencrypt hello data předtím, než bude převedena do úložiště a toodecrypt hello dat po se přenáší z úložiště.

Další informace o šifrování na straně klienta:

* [Šifrování na straně klienta pro Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Ovládací prvky zabezpečení cloudu řady: šifrování dat při přenosu](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Pro mnoho společností [šifrování dat v klidovém stavu](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) je povinný krok k suverenity data o ochraně osobních údajů a dodržování předpisů a data. Existují tři Azure funkcí, které poskytují šifrování dat, která je "v klidovém stavu":

* [Šifrování služby úložiště](../storage/common/storage-security-guide.md#encryption-at-rest) vám umožní toorequest, že služba úložiště hello automaticky šifrování dat při zápisu ho tooAzure úložiště.
* [Šifrování na straně klienta](../storage/common/storage-security-guide.md#client-side-encryption) taky nabízí funkci hello šifrování v klidovém stavu.
* [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) vám umožní disky tooencrypt hello operačního systému a datové disky používané virtuálním počítačem IaaS.

Další informace o šifrování služby úložiště:

* [Azure šifrování služby úložiště](https://azure.microsoft.com/services/storage/) je k dispozici pro [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/). Podrobnosti na jiné typy úložiště Azure najdete v tématu [soubor](https://azure.microsoft.com/services/storage/files/), [disku (Premium Storage)](https://azure.microsoft.com/services/storage/premium-storage/), [tabulky](https://azure.microsoft.com/services/storage/tables/), a [fronty](https://azure.microsoft.com/services/storage/queues/).
* [Šifrování služby úložiště Azure pro Data v klidovém stavu](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption pro virtuální počítače (VM) vám pomůže adresu zabezpečení organizace a požadavky na dodržování předpisů šifrováním vaše disky virtuálního počítače (včetně spouštěcí a datovými disky) s klíči a zásad řízení v [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Šifrování disku pro virtuální počítače funguje pro operační systémy Linux a Windows. Používá také Key Vault toohelp zabezpečit, správu a audit použití klíče pro šifrování disku. Všechna data hello v vaše disky virtuálních počítačů je zašifrovaná přinejmenším pomocí šifrování standardní technologie v účtech úložiště Azure. Šifrování disku řešení pro systém Windows Hello je založena na [Microsoft BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx), a hello Linux řešení je založena na [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Další informace:

* [Azure Disk Encryption pro systém Windows a virtuální počítače Linux IaaS](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Používá Azure Disk Encryption [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) toohelp můžete řídit a spravovat disku šifrovacích klíčů a tajných klíčů v trezoru klíčů předplatného, a zajistit, že jsou všechna data v disky virtuálního počítače hello zašifrovaná přinejmenším ve vaší službě Azure Úložiště. Měli byste použít Key Vault tooaudit klíčů a použití zásad.

Další informace:

* [Co je Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md)
