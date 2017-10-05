---
ms.assetid: 
title: "Azure Key Vault obnovitelného odstranění | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/10/2017
ms.openlocfilehash: c873b153ef9c7d5f55672a5918c9dc4fb7256701
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-key-vault-soft-delete-overview"></a>Přehled konfigurace soft odstranění služby Azure Key Vault

Key Vault obnovitelného odstranění funkce umožňuje obnovení odstraněné trezory a objekty trezoru, označuje jako obnovitelného odstranění. Konkrétně tento problém řeší následující scénáře:

- Podpora pro obnovitelné odstranění trezoru klíčů
- Podpora pro obnovitelné odstranění trezoru klíčů objektů (např. klíče, certifikáty a tajné klíče)

## <a name="supporting-interfaces"></a>Podpora rozhraní

Funkci soft odstranění je původně k dispozici prostřednictvím REST, .NET / C# a prostředí PowerShell rozhraní. Najdete odkazy na tyto další podrobnosti, [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Scénáře

Azure trezory klíč jsou sledované prostředky, spravovat pomocí Správce prostředků Azure. Azure Resource Manager určuje také dobře definovaný chování pro odstranění, které vyžaduje, aby úspěšná operace odstranění musí mít za následek prostředku nebude přístupný už. Funkci soft odstranění řeší obnovení odstraněného objektu, zda byl odstraněn náhodnému nebo záměrnému.

1. V typických možností uživatele nechtěně odstraněno trezoru klíčů nebo objekt trezoru klíčů; Pokud, klíč trezoru, nebo klíč trezoru objekt měla být použitelná pro obnovení po předem určenou dobu, může odstranění vrátit zpět a obnovit svá data uživatele.

2. V případě různých neautorizovaný uživatel může pokusu o odstranění trezoru klíčů nebo objekt trezoru klíčů, například klíč v trezoru, způsobit přerušení obchodní. Oddělení odstranění trezoru klíčů nebo objekt trezoru klíčů ze skutečného odstranění zadaných dat můžete použít jako bezpečnostní opatření, například omezení oprávnění k odstranění dat na jiný důvěryhodné role. Tento přístup efektivně vyžaduje kvora pro operace, které může jinak dojít ke ztrátě okamžité data.

### <a name="soft-delete-behavior"></a>Chování konfigurace soft odstranění

Pomocí této funkce je operace odstranění trezoru klíčů objektů nebo trezoru klíčů konfigurace soft odstranění, efektivně podržíte prostředky pro danou uchování období, při poskytnutí vzhled, že je tento objekt odstranit. Další služby poskytuje mechanismus pro obnovení odstraněného objektu, a to v podstatě vrácení zpět k odstranění. 

Konfigurace soft odstranění je volitelné chování Key Vault a je **není ve výchozím nastavení povolené** v této verzi. Podrobnosti o povolení konfigurace soft odstranění pro váš trezor klíčů najdete v tématu konkrétní pokyny v dokumentaci pro rozhraní podle svého výběru [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).

### <a name="key-vault-recovery"></a>Obnovení klíče trezoru

Po odstranění trezoru klíčů, vytvoří služba proxy prostředků v rámci předplatného, přidání dostatečná metadata pro obnovení. Prostředek proxy je objekt uložené, k dispozici ve stejném umístění jako trezor klíčů odstraněné. 

### <a name="key-vault-object-recovery"></a>Obnovení objektu trezoru klíčů

Při odstraňování objekt trezoru klíčů, například klíč, službu umístí objekt v odstraněném stavu, díky čemuž dostupná pro všechny operace načtení. V tomto stavu, objekt trezoru klíčů může pouze být uvedený, obnovené nebo vynuceně nebo trvale odstraněn. 

Ve stejnou dobu bude Key Vault naplánovat odstranění základní data odpovídající odstraněné trezoru klíčů nebo objekt trezoru klíčů pro provádění po předem určenou dobu. Záznam DNS odpovídající úložišti se také uchovávají po dobu trvání intervalu uchování.

### <a name="soft-delete-retention-period"></a>Doba uchování dat konfigurace soft odstranění

Logicky odstraněné prostředky jsou uchovány nastavte dobu, 90 dní. Během intervalu uchování konfigurace soft odstranění následujících podmínek:

- Můžete seznam všech trezorů klíčů a klíče trezoru objekty ve stavu obnovitelného odstranění pro vaše předplatné a také přístup k odstranění a obnovení informace o nich.
    - Pouze uživatelé s speciální oprávnění vytvořit seznam odstraněných trezorů. Doporučujeme uživatelům vytvořit vlastní roli s tyto speciální oprávnění pro zpracování odstranit trezorů.
- Nelze vytvořit trezoru klíčů se stejným názvem do stejného umístění; odpovídajícím způsobem trezor klíčů objekt nelze vytvořit v dané trezoru Pokud tento trezor klíčů obsahuje objekt se stejným názvem a který je v odstraněném stavu 
- Jenom uživatel, konkrétně privilegované může obnovit trezoru klíčů nebo objekt trezoru klíčů vydání příkazu Obnovit na odpovídající prostředek proxy serveru.
    - Uživatel členem vlastní role, který má oprávnění k vytvoření trezoru klíčů ve skupině prostředků, můžete obnovit trezoru.
- Jenom uživatel, konkrétně privilegované může vynuceně odstranění trezoru klíčů nebo trezoru klíčů objektu po vydání příkazu delete na odpovídající prostředek proxy serveru.

Pokud je obnovena na trezor klíčů nebo objekt trezoru klíčů, provádí služba na konci intervalu uchování vyprazdňování obnovitelné odstranění trezoru klíčů nebo objekt trezoru klíčů a její obsah. Odstranění prostředku nemusí přeplánovat.

### <a name="permitted-purge"></a>Povolené vyprázdnění

Trvale odstraníte, čištění, trezoru klíčů je možné prostřednictvím operaci POST na prostředku proxy serveru a vyžaduje speciální oprávnění. Obecně platí pouze vlastník předplatného bude moci mazání trezoru klíčů. Operaci POST spustí okamžitou a nezotavitelné odstranění tohoto trezoru. 

Výjimkou je případ, kdy Azure odběr byl označen jako *neodstranitelný*. V takovém případě pouze služby pak může provádět skutečného odstranění a nemá tak jako naplánované proces. 



