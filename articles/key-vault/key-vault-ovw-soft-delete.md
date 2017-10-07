---
ms.assetid: 
title: "obnovitelného odstranění aaaAzure Key Vault | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/10/2017
ms.openlocfilehash: 1b402c58db6f25ae4ae5e2720786fa81eb0e839a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-soft-delete-overview"></a>Přehled konfigurace soft odstranění služby Azure Key Vault

Key Vault obnovitelného odstranění funkce umožňuje obnovení hello odstranit trezory a objekty trezoru, označuje jako obnovitelného odstranění. Konkrétně jsme adres hello následující scénáře:

- Podpora pro obnovitelné odstranění trezoru klíčů
- Podpora pro obnovitelné odstranění trezoru klíčů objektů (např. klíče, certifikáty a tajné klíče)

## <a name="supporting-interfaces"></a>Podpora rozhraní

Hello funkci soft odstranění je původně k dispozici prostřednictvím hello REST, .NET / C# a prostředí PowerShell rozhraní. Odkazovat toohello odkazy na tyto další podrobnosti, [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Scénáře

Azure trezory klíč jsou sledované prostředky, spravovat pomocí Správce prostředků Azure. Azure Resource Manager určuje také dobře definovaný chování pro odstranění, které vyžaduje, aby úspěšná operace odstranění musí mít za následek prostředku nebude přístupný už. Hello funkci soft odstranění adresy hello obnovení hello odstraněn objekt, zda text hello, co se náhodnému nebo záměrnému.

1. V typické scénáři hello uživatele nechtěně odstraněno trezoru klíčů nebo objekt trezoru klíčů; Pokud tento trezor klíčů nebo klíč trezoru objekt byly toobe použitelná pro obnovení po předem určenou dobu, může uživatel hello hello odstranění vrátit zpět a obnovit svá data.

2. V případě různých neautorizovaný uživatel pokusit toodelete trezoru klíčů nebo objekt trezoru klíčů, například klíč v trezoru, přerušení obchodní toocause. Oddělení hello odstranění trezoru klíčů hello nebo objekt trezoru klíčů z hello skutečného odstranění hello základní dat můžete použít jako bezpečnostní opatření, například omezení oprávnění na tooa odstranění dat liší, důvěryhodné role. Tento přístup efektivně vyžaduje kvora pro operace, které může jinak dojít ke ztrátě okamžité data.

### <a name="soft-delete-behavior"></a>Chování konfigurace soft odstranění

Pomocí této funkce hello operace odstranění v trezoru klíčů nebo trezoru klíčů objekt je typu soft odstranění, efektivně podržíte hello prostředky pro danou uchování období, udělíte hello vzhled tento objekt hello se odstraní. Další služby Hello poskytuje mechanismus pro obnovení hello odstraněn objekt, v podstatě vrácení zpět hello odstranění. 

Konfigurace soft odstranění je volitelné chování Key Vault a je **není ve výchozím nastavení povolené** v této verzi. Podrobnosti o povolení konfigurace soft odstranění pro váš trezor klíčů najdete v tématu hello konkrétní pokyny v hello odkaz pro rozhraní hello podle svého výběru [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).

### <a name="key-vault-recovery"></a>Obnovení klíče trezoru

Po odstranění trezoru klíčů, služba hello vytvoří prostředek proxy serveru v rámci předplatného hello, přidání dostatečná metadata pro obnovení. prostředek proxy Hello je objekt uložené, k dispozici v hello stejné umístění jako trezor klíčů odstranit hello. 

### <a name="key-vault-object-recovery"></a>Obnovení objektu trezoru klíčů

Při odstraňování objekt trezoru klíčů, například klíč, služba hello umístí hello objekt v odstraněném stavu, a tím operacemi načítání nepřístupný tooany. V tomto stavu, hello trezoru klíčů objekt můžete pouze uvedené, obnovené nebo vynuceně nebo trvale odstraněn. 

V hello stejný čas, klíče trezoru se naplánuje hello odstranění hello základní data odpovídající toohello odstranit trezor klíčů nebo objekt trezoru klíčů pro provádění po předem určenou dobu. Hello DNS záznam odpovídající toohello úložiště se také zachová hello doby trvání intervalu uchování hello.

### <a name="soft-delete-retention-period"></a>Doba uchování dat konfigurace soft odstranění

Logicky odstraněné prostředky jsou uchovány nastavte dobu, 90 dní. Během intervalu uchování konfigurace soft odstranění hello platí následující hello:

- Můžete seznam všech trezorů klíčů hello a trezoru klíčů objekty ve stavu obnovitelného odstranění hello pro vaše předplatné a také přístup k odstranění a obnovení informace o nich.
    - Pouze uživatelé s speciální oprávnění vytvořit seznam odstraněných trezorů. Doporučujeme uživatelům vytvořit vlastní roli s tyto speciální oprávnění pro zpracování odstranit trezorů.
- Trezor klíčů se stejným názvem nelze v hello hello stejné umístění; odpovídajícím způsobem, trezor klíčů objekt nelze vytvořit v dané trezoru Pokud tento trezor klíčů obsahuje objekt s hello stejný název a který je v odstraněném stavu 
- Jenom uživatel, konkrétně privilegované může obnovit trezoru klíčů nebo objekt trezoru klíčů vydání příkazu Obnovit na odpovídající proxy prostředek hello.
    - člen hello vlastní role, který má oprávnění toocreate hello trezoru klíčů ve skupině prostředků hello Hello uživatele, můžete obnovit hello trezoru.
- Jenom uživatel, konkrétně privilegované může vynuceně odstranění trezoru klíčů nebo trezoru klíčů objektu po vydání příkazu delete na odpovídající prostředek proxy hello.

Pokud je obnovena na trezor klíčů nebo objekt trezoru klíčů, provede konec hello uchování interval hello služby v hello vyprázdnění hello obnovitelné odstranění trezoru klíčů nebo objekt trezoru klíčů a její obsah. Odstranění prostředku nemusí přeplánovat.

### <a name="permitted-purge"></a>Povolené vyprázdnění

Trvale odstraníte, čištění, trezoru klíčů je možné prostřednictvím operaci POST na prostředku hello proxy serveru a vyžaduje speciální oprávnění. Obecně platí pouze vlastník předplatného hello bude možné toopurge trezoru klíčů. aktivační události operaci POST Hello hello okamžité a nezotavitelné odstranění tohoto trezoru. 

K výjimce toothis se hello nestane, pokud hello předplatného Azure byl označen jako *neodstranitelný*. V takovém případě pouze hello služby může pak provést odstranění skutečné hello a nemá tak jako naplánované proces. 



