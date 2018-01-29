---
title: "Služba kvóty a omezení pro Azure Batch | Microsoft Docs"
description: "Další informace o výchozích Azure Batch kvót, omezení a omezení a zvyšuje jak požádat o kvótu"
services: batch
documentationcenter: 
author: v-dotren
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 210ba4a90f24ce9b0b55c4565028232c2b7fd7cc
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch

Jako s jinými službami Azure neexistují omezení na některé prostředky spojené s touto službou Batch. Mnoho z těchto omezení je výchozí kvóty používané službou Azure na úrovni účet nebo předplatné. Tento článek popisuje tyto výchozí hodnoty a jak můžete vyžádat kvóty zvyšuje.

Mějte na těchto kvót podle návrhu a rozšiřování škálování využívajících vaší úloh služby Batch. Například pokud váš fond nemá dostat cílovým počtem výpočetních uzlů, které zadáte, může bylo dosaženo maximální kvóta základní vašeho účtu Batch.

Můžete spustit několik dávkových úloh služby Batch v jednom účtu Batch najednou, nebo můžete úlohy rozložit mezi více účtů Batch, které jsou v jednom předplatném, ale v různých oblastech Azure.

Pokud plánujete spouštět úlohy v produkčním prostředí ve službě Batch, musíte zvýšit jeden nebo více kvót nad výchozí. Pokud chcete kvótu zvýšit, můžete otevřít online [zákazníka žádost o podporu](#increase-a-quota) zdarma.

> [!NOTE]
> Kvótu je limit platební, není záruku kapacity. Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure.
> 
> 

## <a name="resource-quotas"></a>Kvóty prostředků
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="quotas-in-user-subscription-mode"></a>Kvóty v uživatelském režimu předplatného

Pokud jste použili k vytvoření účtu Batch s režim přidělení fondu nastavený na dřívější verzi rozhraní API služby Batch **uživatele předplatné**, kvóty se použijí jinak. V tomto režimu, který již není doporučeno, Batch virtuální počítače a další prostředky jsou vytvořené přímo v vaše předplatné při vytváření fondu. Kvóta jader Azure Batch se nevztahuje na účet je vytvořen v tomto režimu. Místo toho kvóty pro vaše předplatné pro místní výpočetní jader a dalším prostředkům se použijí. Další informace o těchto kvót v [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

## <a name="other-limits"></a>Další omezení
| **Prostředek** | **Maximální omezení** |
| --- | --- |
| [Souběžné úlohy](batch-parallel-node-tasks.md) na výpočetním uzlu |4 x počet jader na uzel |
| [Aplikace](batch-application-packages.md) na účtu Batch |20 |
| Balíčky aplikací na aplikaci. |40 |
| Velikost balíčku aplikace, (všechny) |Poli 195GB<sup>1</sup> |
| Velikost maximální spuštění úloh | 32768 znaků<sup>2</sup> |
| Úloha maximální doba platnosti | 7 dní<sup>3</sup> |

<sup>1</sup> azure Storage limit pro velikost objektu blob maximální bloku<br />
<sup>2</sup> zahrnuje soubory prostředků a proměnných prostředí<br />
<sup>3</sup> maximální životnost úlohy, když je přidán do úlohy po dokončení, je 7 dní. Dokončené úlohy zachovat po neomezenou dobu; data pro úlohy, které nejsou dokončeny v rámci maximální doba života nejsou dostupná.


## <a name="view-batch-quotas"></a>Zobrazení dávky kvóty
Zobrazení vaší kvóty účtu Batch [portál Azure][portal].

1. Vyberte **účty Batch** na portálu, pak vyberte účet Batch, co vás zajímá.
2. Vyberte **kvóty** v nabídce účtu Batch.
3. Zobrazit kvóty aktuálně používané k účtu Batch
   
    ![Kvóty účtu batch][account_quotas]



## <a name="increase-a-quota"></a>Navýšení kvóty
Postupujte podle těchto kroků k vyžádání kvótu zvýšit vašeho účtu Batch nebo předplatnému pomocí [portál Azure][portal]. Typ zvýšení kvóty závisí na režimu přidělení fondu vašeho účtu Batch.

### <a name="increase-a-batch-cores-quota"></a>Navýšení kvóty jader Batch 

1. Vyberte **Nápověda a podpora** dlaždici řídicího panelu portálu nebo otazník (**?**) v pravém horním rohu portálu.
2. Vyberte **nová žádost o podporu** > **Základy**.
3. V **Základy**:
   
    a. **Vydávání typu** > **kvóty**
   
    b. Vyberte své předplatné.
   
    c. **Typ kvóty** > **Batch**
   
    d. **Podporují plán** > **kvóty podporu - zahrnuté**
   
    Klikněte na **Další**.
4. V **problém**:
   
    a. Vyberte **závažnost** podle vaší [dopad na chod firmy][support_sev].
   
    b. V **podrobnosti**, zadejte každé kvóty, které chcete změnit, název účtu Batch a nový limit.
   
    Klikněte na **Další**.
5. V **kontaktní informace**:
   
    a. Vyberte **způsob kontaktu preferované**.
   
    b. Zkontrolujte a zadejte požadované kontaktní údaje.
   
    Kliknutím na **Vytvořit** odešlete žádost o podporu.

Jakmile jste odeslali vaši žádost o podporu, bude vás kontaktovat podporu Azure. Všimněte si, že dokončení požadavku může trvat až 2 pracovních dnů.


## <a name="related-topics"></a>Související témata
* [Vytvoření účtu Azure Batch pomocí portálu Azure](batch-account-create-portal.md)
* [Přehled funkcí Azure Batch](batch-api-basics.md)
* [Předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
