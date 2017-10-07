---
title: aaaEncryption v Azure Data Lake Store | Microsoft Docs
description: "Pochopíte, jak funguje šifrování a obměna klíčů v Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.openlocfilehash: a9f3a2dce8232deba93005594d1e6a21e9c0cbee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encryption-of-data-in-azure-data-lake-store"></a>Šifrování dat v Azure Data Lake Store

Šifrování v Azure Data Lake Store pomáhá chránit vaše data, implementovat zásady podnikového zabezpečení a splnit požadavky na dodržování legislativních předpisů. Tento článek obsahuje přehled návrhu hello a popisuje některé z hello technické aspekty implementace.

Data Lake Store podporuje šifrování jak neaktivních uložených dat, tak i přenášených dat. Pro neaktivní uložená data podporuje služba Data Lake Store „ve výchozím nastavení zapnuté“ transparentní šifrování. Tady je podrobnější vysvětlení významu těchto termínů:

* **Na ve výchozím nastavení**: Když vytvoříte nový účet Data Lake Store, hello výchozí nastavení povoluje šifrování. Data, která je uložená v Data Lake Store je následně vždy šifrované předchozí toostoring na trvalé médiu. To je hello chování pro všechna data a nelze jej změnit po vytvoření účtu.
* **Transparentní**: Data Lake Store automaticky předchozí toopersisting data šifruje a dešifruje data předchozí tooretrieval. šifrování Hello je nakonfigurované a spravované v hello Data Lake Store úroveň správce. Nebudou provedeny žádné změny dat toohello přístup k rozhraní API. Proto není nutné kvůli šifrování měnit aplikace a služby, které pracují se službou Data Lake Store.

Přenášená data se ve službě Data Lake Store také vždy šifrují. Kromě toho tooencrypting data předchozí toostoring toopersistent média, hello je také vždy zabezpečená data při přenosu pomocí protokolu HTTPS. HTTPS je hello pouze protokol, který je podporován pro rozhraní Data Lake Store REST hello. Hello následující diagram ukazuje, jak se změní na data šifrovaná v Data Lake Store:

![Diagram šifrování dat ve službě Data Lake Store](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-store"></a>Nastavení šifrování ve službě Data Lake Store

Šifrování se pro službu Data Lake Store nastavuje během vytváření účtu a ve výchozím nastavení je vždy povoleno. Můžete spravovat klíče hello sami, nebo povolit toomanage Data Lake Store je pro vás (Toto je výchozí hello).

Další informace najdete v tématu [Začínáme](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="how-encryption-works-in-data-lake-store"></a>Jak funguje šifrování ve službě Data Lake Store

Hello následující informace se týkají jak toomanage hlavní šifrovací klíče a vysvětluje hello tři různé typy klíčů, které můžete použít v šifrování dat pro Data Lake Store.

### <a name="master-encryption-keys"></a>Hlavní šifrovací klíče

Data Lake Store nabízí dva režimy pro správu hlavních šifrovacích klíčů (MEK). Teď předpokládají, že tento hello hlavní šifrovací klíč je klíč hello nejvyšší úrovně. Přístup k toohello hlavní šifrovací klíč je požadovaná toodecrypt všechna data uložená v Data Lake Store.

Hello dva režimy pro správu hello hlavní šifrovací klíč jsou následující:

*   Klíče spravované službou
*   Klíče spravované zákazníkem

V obou režimech hello hlavní šifrovací klíč zabezpečené ukládání do Azure Key Vault. Key Vault je plně spravovaná, vysoce zabezpečených služba na platformě Azure, kterou lze použít toosafeguard kryptografické klíče. Další informace najdete v tématu [Key Vault](https://azure.microsoft.com/services/key-vault).

Zde je stručný porovnání funkcí poskytovaných Správa hello MEKs hello dva režimy.

|  | Klíče spravované službou | Klíče spravované zákazníkem |
| --- | --- | --- |
|Jak se data ukládají?|Vždy šifrované předchozí toobeing uložené.|Vždy šifrované předchozí toobeing uložené.|
|Kde jsou uloženy hello hlavní šifrovací klíč?|Key Vault|Key Vault|
|Nejsou žádné šifrování klíče uložené v hello vymazat Key Vault? |Ne|Ne|
|Může načíst hello MEK Key Vault?|Ne. Po hello MEK je uložené v Key Vault lze možné použít pouze pro šifrování a dešifrování.|Ne. Po hello MEK je uložené v Key Vault lze možné použít pouze pro šifrování a dešifrování.|
|Kdo je vlastníkem hello Key Vault instance a hello MEK?|Hello služby Data Lake Store|Vlastníte hello Key Vault instance, který patří do předplatného Azure. Hello MEK v Key Vault můžou spravovat softwaru nebo hardwaru.|
|Můžete odvolat přístup toohello MEK pro hello služby Data Lake Store?|Ne|Ano. Můžete spravovat seznamy řízení přístupu v Key Vault a odeberte identitu služby přístupu k řízení položky toohello pro hello služby Data Lake Store.|
|Je možné trvale odstranit hello MEK?|Ne|Ano. Pokud odstraníte hello MEK z Key Vault, hello data v účtu Data Lake Store hello nelze dešifrovat každému uživateli, včetně služby Data Lake Store hello. <br><br> Pokud jste explicitně zálohovali hello MEK předchozí toodeleting ji z Key Vault, hello MEK lze obnovit a pak lze obnovit hello data. Ale pokud nebyla zálohována předchozí toodeleting hello MEK ho z Key Vault, hello data v hello účtu Data Lake Store můžete nikdy dešifrovat po tomto datu.|


Kromě zajištění dostatečného tento rozdíl, který spravuje hello MEK a hello Key Vault instance ve kterém se nachází, hello zbytek hello návrhu je hello stejné pro oba režimy.

Když zvolíte hello režim pro hello hlavní šifrovací klíče je důležité tooremember hello následující:

*   Můžete zvolit, jestli toouse zákazník spravuje klíče nebo klíče služby spravované při zřizování účtu Data Lake Store.
*   Po zřízení účtu Data Lake Store, nelze změnit režim hello.

### <a name="encryption-and-decryption-of-data"></a>Šifrování a dešifrování dat

Existují tři typy klíčů, které se používají v návrhu hello dat šifrování. Hello následující tabulka obsahuje souhrn:

| Klíč                   | Zkratka | Přidružený k | Umístění úložiště                             | Typ       | Poznámky                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Hlavní šifrovací klíč | MEK          | Účet Data Lake Store | Key Vault                              | Asymetrický | Může být spravovaný službou Data Lake Store nebo vámi.                                                              |
| Šifrovací klíč dat   | DEK          | Účet Data Lake Store | Trvalé úložiště spravované službou Data Lake Store | Symetrický  | hello MEK šifrování Hello klíč DEK. Dobrý den, který je šifrovaný klíč DEK, co je uložen na médiu trvalé. |
| Šifrovací klíč bloku  | BEK          | Blokem dat | Žádný                                         | Symetrický  | Hello BEK je odvozená od hello klíč DEK a hello datového bloku.                                                      |

Hello následující diagram znázorňuje tyto koncepty:

![Klíče v šifrování dat](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-toobe-decrypted"></a>Pokud je soubor toobe dešifrovat pseudo algoritmus:
1.  Zkontrolujte, jestli hello klíč DEK pro účet Data Lake Store hello je uložená v mezipaměti a připravena k použití.
    - Pokud ne, přečtěte si, že hello zašifrování klíč DEK z trvalého úložiště a odeslat toobe trezoru tooKey dešifrovat. Mezipaměť hello dešifruje klíč DEK v paměti. Je nyní připraven toouse.
2.  Pro každý blok dat v souboru hello:
    - Hello šifrované blok pro čtení dat z trvalého úložiště.
    - Generování hello BEK z hello klíč DEK a hello blok šifrovaná data.
    - Hello BEK toodecrypt data použijte.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-toobe-encrypted"></a>Pokud bloku dat je toobe šifrované pseudo algoritmus:
1.  Zkontrolujte, jestli hello klíč DEK pro účet Data Lake Store hello je uložená v mezipaměti a připravena k použití.
    - Pokud ne, přečtěte si, že hello zašifrování klíč DEK z trvalého úložiště a odeslat toobe trezoru tooKey dešifrovat. Mezipaměť hello dešifruje klíč DEK v paměti. Je nyní připraven toouse.
2.  Generovat jedinečný BEK hello bloku dat z hello klíč DEK.
3.  Datový blok hello s hello BEK, šifrovat pomocí šifrování AES-256.
4.  Ukládat blok šifrovaných dat hello dat do trvalého úložiště.

> [!NOTE] 
> Z důvodů výkonu hello vymazat klíč DEK v hello je uložené v mezipaměti v paměti po krátkou dobu a okamžitě vymazáním i později. Na trvalé média vždy uloženy ve hello MEK šifrování.

## <a name="key-rotation"></a>Obměna klíčů

Při použití klíče spravovaného zákazníkem, můžete otočit hello MEK. toolearn jak zjistit, tooset účet Data Lake Store s klíči spravované zákazníkem [Začínáme](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Požadavky

Při nastavování hello účtu Data Lake Store, vybrali jste toouse vlastní klíče. Tuto možnost nelze změnit po vytvoření účtu hello. Hello následující postup předpokládá, že používáte klíče spravovaného zákazníkem (to znamená, rozhodli jste se vlastní klíče z trezoru klíč).

Všimněte si, že pokud používáte hello výchozí možnosti pro šifrování, jsou data vždy šifrována pomocí klíčů spravuje Data Lake Store. Tuto možnost nemáte hello možnost toorotate klíče, jako jsou spravovány službou Data Lake Store.

### <a name="how-toorotate-hello-mek-in-data-lake-store"></a>Jak toorotate hello MEK v Data Lake Store

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Procházejte toohello Key Vault instance, který ukládá klíče spojené s vaším účtem Data Lake Store. Vyberte **Klíče**.

    ![Snímek obrazovky služby Key Vault](./media/data-lake-store-encryption/keyvault.png)

3.  Vyberte hello klíč přidružený k účtu Data Lake Store a vytvořit novou verzi tohoto klíče. Všimněte si, že Data Lake Store aktuálně podporuje jenom střídání klíče tooa novou verzi klíč. Nepodporuje otáčení tooa jiný klíč.

   ![Snímek obrazovky okna Klíče se zvýrazněnou možností Nová verze](./media/data-lake-store-encryption/keynewversion.png)

4.  Účet úložiště Data Lake Store toohello Procházet a vyberte **šifrování**.

    ![Snímek obrazovky okna účtu úložiště Data Lake Store se zvýrazněnou možností Šifrování](./media/data-lake-store-encryption/select-encryption.png)

5.  Zobrazí upozornění, že je k dispozici nová verze klíče hello klíče. Klikněte na tlačítko **otočit klíč** tooupdate hello klíče toohello novou verzi.

    ![Snímek obrazovky okna Data Lake Store se zvýrazněnou zprávou a možností Obměnit klíč](./media/data-lake-store-encryption/rotatekey.png)

Tato operace by měla trvat méně než dvě minuty a neexistuje žádný očekávaná doba nedostupnosti kvůli tookey otočení. Po dokončení operace hello hello novou verzi hello klíč se používá.
