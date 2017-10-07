---
title: "aaaService kvóty a omezení pro Azure Batch | Microsoft Docs"
description: "Další informace o výchozích Azure Batch kvót, omezení a omezení a jak zvyšuje toorequest kvóty"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6035d1c7618cfe97ebca3780e02a4ee34f54e534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch

Jako s jinými službami Azure neexistují omezení na některé prostředky přidružené k hello služby Batch. Mnoho z těchto omezení je výchozí kvóty používané službou Azure na úrovni účtu nebo hello předplatného. Tento článek popisuje tyto výchozí hodnoty a jak můžete vyžádat kvóty zvyšuje.

Pamatujte na tyto kvóty při navrhování a škálování zatížení vašich úloh Batch. Například pokud váš fond není dosažení hello cílovým počtem výpočetních uzlů, které jste určili, může bylo dosaženo maximální kvóta základní hello vašeho účtu Batch nebo regionální kvóta jader virtuálních počítačů pro vaše předplatné.

Můžete spustit několik úloh služby Batch v jednom účtu Batch nebo úlohy distribuovat mezi účty Batch, které jsou v hello stejného předplatného, ale v různých oblastech Azure.

Pokud máte v plánu úlohy v produkčním prostředí toorun v dávce, musíte tooincrease jeden nebo více hello kvóty nad výchozí hello. Pokud chcete tooraise kvótu, můžete otevřít online [zákazníka žádost o podporu](#increase-a-quota) zdarma.

> [!NOTE]
> Kvótu je limit platební, není záruku kapacity. Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure.
> 
> 

## <a name="resource-quotas"></a>Kvóty prostředků
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a>Kvóty v uživatelském režimu předplatného

Pro účet Batch s režimem přidělení fondu nastavit příliš**uživatele předplatné**, dávky virtuální počítače a další prostředky, například účty úložiště, jsou vytvořené přímo v rámci vašeho předplatného, když je vytvořen fond. kvóta jader Azure Batch Hello nevztahuje tooan účet vytvořený v tomto režimu. Místo toho se použijí hello kvóty ve vašem předplatném pro místní výpočetní jader a dalším prostředkům. Další informace o těchto kvót v [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

Při plánování využití prostředků pro účet je vytvořen v uživatelském režimu předplatné, jsou vyžadovány pro každých 40 virtuální počítače s Linuxem nebo 20 virtuálních počítačů Windows hello Poznámka následující prostředky Batch (v přidání toocompute jádra):

| Prostředek | Kvóta | Poskytovatel |
| --- | ---| --- |
| Jeden účet úložiště | Účty úložiště | Microsoft.Storage |
| Jedna veřejná IP adresa | Veřejné IP adresy | Microsoft.Network | 
| Jedna virtuální síť | Virtuální sítě | Microsoft.Network | 
| Jedna síť skupiny zabezpečení | Network Security Groups (Skupiny zabezpečení sítě) | Microsoft.Network | 
| Jeden škálovací sadu virtuálních počítačů | Škálovací sady virtuálních počítačů | Microsoft.Compute | 
| Jedna služba Vyrovnávání zatížení | Nástroje pro vyrovnávání zatížení | Microsoft.Network | 

Hello jader kvóta na místní úrovni nebo na virtuální počítač rodiny by měla být sada podle toohello velikost virtuálního počítače vyžaduje pro fondu Batch nebo fondy:

| Kvóta | Poskytovatel |
| --- | ---- |
| Celkový počet jader místní | Microsoft.Compute |
| … Rodiny jader | Microsoft.Compute |



## <a name="other-limits"></a>Další omezení
| **Prostředek** | **Maximální omezení** |
| --- | --- |
| [Souběžné úlohy](batch-parallel-node-tasks.md) na výpočetním uzlu |4 x počet jader na uzel |
| [Aplikace](batch-application-packages.md) na účtu Batch |20 |
| Balíčky aplikací na aplikaci. |40 |
| Velikost balíčku aplikace, (všechny) |Poli 195GB<sup>1</sup> |
| Velikost maximální spuštění úloh | 32768 znaků<sup>2</sup> |

<sup>1</sup> azure Storage limit pro velikost objektu blob maximální bloku<br />
<sup>2</sup> zahrnuje soubory prostředků a proměnných prostředí

## <a name="view-batch-quotas"></a>Zobrazení dávky kvóty
Zobrazení vaší kvóty účtu Batch v hello [portál Azure][portal].

1. Vyberte **účty Batch** hello portálu, pak vyberte účet Batch hello vás zajímá.
2. Vyberte **vlastnosti** v okně účtu Batch hello nabídky.
3. Zobrazí okno Vlastnosti Hello hello **kvóty** aktuálně použité toohello účtu Batch
   
    ![Kvóty účtu batch][account_quotas]

Pro účet Batch vytvořit v uživatelském režimu předplatné hello zobrazení související s kvóty předplatného v hello portálu Azure.

1. Vyberte **odběry**a vyberte hello předplatné, který používáte pro hello účtu Batch.

2. Na hello **předplatné** vyberte **využití + kvóty**.



## <a name="increase-a-quota"></a>Navýšení kvóty
Postupujte podle těchto kroků toorequest zvýšit kvótu pro vašeho účtu Batch nebo předplatné pomocí hello [portál Azure][portal]. Typ Hello zvýšení kvóty závisí na režimu přidělení fondu hello účtu Batch.

### <a name="increase-a-batch-cores-quota"></a>Navýšení kvóty jader Batch 

Pokud je váš účet Batch vytvořený v **služba Batch** režimu, postupujte podle těchto kroků toorequest zvýšení kvóty jader Batch:

1. Vyberte hello **Nápověda a podpora** dlaždici na řídicím panelu portálu nebo hello otazník (**?**) v pravém horním rohu hello hello portálu.
2. Vyberte **nová žádost o podporu** > **Základy**.
3. Na hello **Základy** okno:
   
    a. **Vydávání typu** > **kvóty**
   
    b. Vyberte své předplatné.
   
    c. **Typ kvóty** > **Batch**
   
    d. **Podporují plán** > **kvóty podporu - zahrnuté**
   
    Klikněte na **Další**.
4. Na hello **problém** okno:
   
    a. Vyberte **závažnost** podle tooyour [dopad na chod firmy][support_sev].
   
    b. V **podrobnosti**, zadejte každé kvótu chcete toochange, název účtu Batch hello a hello nový limit.
   
    Klikněte na **Další**.
5. Na hello **kontaktní informace** okno:
   
    a. Vyberte **způsob kontaktu preferované**.
   
    b. Zkontrolujte a zadejte požadované hello kontaktní údaje.
   
    Klikněte na tlačítko **vytvořit** žádost o podporu toosubmit hello.

Jakmile jste odeslali vaši žádost o podporu, bude vás kontaktovat podporu Azure. Všimněte si, že dokončení požadavku hello může trvat až too2 pracovních dnů.

### <a name="increase-a-subscription-cores-quota"></a>Navýšení kvóty předplatného jader

Pokud váš účet Batch byl vytvořen v **uživatele předplatné** režimu a potřebujete další místní nebo rodiny jader virtuálního počítače, žádost a kvótu zvýšit v rámci vašeho předplatného. Pokyny najdete v tématu [základní kvóta správce prostředků zvýšit požadavky](../azure-supportability/resource-manager-core-quotas-request.md).



## <a name="related-topics"></a>Související témata
* [Vytvoření účtu Azure Batch pomocí hello portálu Azure](batch-account-create-portal.md)
* [Přehled funkcí Azure Batch](batch-api-basics.md)
* [Předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
