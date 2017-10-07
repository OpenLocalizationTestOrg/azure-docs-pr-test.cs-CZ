---
title: "Azure Active Directory B2C: Nástroj pomocné rutiny pro přizpůsobení uživatelského rozhraní stránky | Microsoft Docs"
description: "Pomocný nástroj používá funkce přizpůsobení uživatelského rozhraní stránky hello toodemonstrate v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: ae935d52-3520-4a94-b66e-b35bb40e7514
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: swkrish
ms.openlocfilehash: 5137ac112019369b4244a925df1acb96fefb758f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-toodemonstrate-hello-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Pomocný nástroj použít vlastní nastavení funkce uživatelského rozhraní (UI) toodemonstrate hello stránky
Tento článek je doprovodný toohello [hlavní článek přizpůsobení uživatelského rozhraní](active-directory-b2c-reference-ui-customization.md) v Azure Active Directory (Azure AD) B2C. Hello následující kroky popisují, jak tooexercise hello funkce přizpůsobení uživatelského rozhraní stránky pomocí ukázkový kód HTML a CSS obsah, který nabízíme.

## <a name="get-an-azure-ad-b2c-tenant"></a>Získání klienta Azure AD B2C
Před úpravou nic, budete potřebovat příliš[získat klienta Azure AD B2C](active-directory-b2c-get-started.md) Pokud jste ještě nemáte.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Vytvoření zásady registrace nebo přihlášení
Hello ukázkový obsah nabízíme lze použít toocustomze dvě stránky v [zásady registrace nebo přihlášení](active-directory-b2c-reference-policies.md): hello [jednotná přihlašovací stránce](active-directory-b2c-reference-ui-customization.md) a hello [samoobslužné uplatňovaná atributy stránky](active-directory-b2c-reference-ui-customization.md). Když [vytváření zásad vaše registrace nebo přihlášení](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), přidejte místní účet (e-mailovou adresu), Facebook, Google a Microsoft jako **zprostředkovatelů Identity**. Tyto jsou hello pouze IDPs, které bude přijímat naše ukázka obsah HTML.  Můžete také přidat podmnožinu těchto IDPs, nechcete-li.

## <a name="register-an-application"></a>Registrace aplikace
Budete potřebovat příliš[zaregistrovat aplikaci](active-directory-b2c-app-registration.md) ve vašem B2C klienta, který lze použít tooexecute vaše zásady. Po registraci vaší aplikace, máte několik možností, které můžete použít tooactually spustit svojí registrační zásadě:

* Jeden z hello Azure AD B2C úvodní aplikace uvedených v části hello "Začínáme" sestavení [přihlásit registrace a přihlašování uživatelů aplikace](active-directory-b2c-overview.md#get-started).
* Hello použití předem vytvořené [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) aplikace. Pokud si zvolíte toouse hello playground, je nutné zaregistrovat aplikaci v svého klienta B2C pomocí hello **identifikátor URI pro přesměrování** `https://aadb2cplayground.azurewebsites.net/`.
* Použití hello **spustit nyní** na vaše zásady v hello tlačítko [portál Azure](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Přizpůsobit zásady
toocustomize hello vzhledu a chování zásad, musíte toofirst vytvoření souborů HTML a CSS pomocí konvencí konkrétní hello Azure AD B2C. Potom můžete nahrát vaše veřejně dostupné umístění tooa statický obsah, se kterým můžete Azure AD B2C. To může být vlastní vyhrazený webový server, Azure Blob Storage, Azure Content Delivery Network nebo jakékoli jiné statické hostování prostředků zprostředkovatele. Hello Jediným požadavkem je, že obsah je k dispozici prostřednictvím protokolu HTTPS a je přístupný pomocí CORS. Jakmile jste vystavený statický obsah na webu hello, můžete upravit zásady toopoint toothis umístění a přítomen, že je obsah tooyour zákazníků. Hello [hlavní článek přizpůsobení uživatelského rozhraní](active-directory-b2c-reference-ui-customization.md) podrobně popisuje, jak funguje funkce přizpůsobení hello Azure AD B2C.

Pro účely tohoto kurzu hello byl již vytvořen některé ukázkový obsah a hostované v Azure Blob Storage. Hello ukázkový obsah je velmi základní přizpůsobení v motivu hello fiktivní společnosti "Adresář Wingtip Toys". tootry odhlašování do vlastní zásady, postupujte takto:

1. Přihlaste se tooyour klienta na hello [portál Azure](https://portal.azure.com/) a přejděte okno s funkcemi toohello B2C.
2. Klikněte na tlačítko **zásady registrace nebo přihlášení** a pak klikněte na zásady (například "b2c\_1\_přihlašovací\_až\_přihlašovací\_v").
3. Klikněte na tlačítko **přizpůsobení uživatelského rozhraní stránky** a potom **stránku registrace nebo přihlášení Unified**.
4. Přepnutí hello **použití vlastní stránku** přepínač příliš**Ano**. V hello **vlastní stránku URI** zadejte `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klikněte na **OK**.
5. Klikněte na tlačítko **stránku pro přihlášení místní účet**. Přepnutí hello **použít vlastní šablonu** přepínač příliš**Ano**. V hello **vlastní stránku URI** zadejte `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. Opakování hello stejný krok pro hello **stránku pro přihlášení sociálních účet**.
   Klikněte na tlačítko **OK** dvakrát tooclose hello uživatelského rozhraní přizpůsobení okna.
7. Klikněte na **Uložit**.

Nyní můžete vyzkoušet vaše vlastní zásady. Pokud chcete, ale můžete taky jednoduše klikněte na tlačítko hello, můžete použít vlastní aplikaci nebo hello playground Azure AD B2C **spustit nyní** příkazu v okně zásady hello. Vyberte svoji aplikaci v hello rozevíracího seznamu a vyberte odpovídající identifikátor URI přesměrování hello. Klikněte na tlačítko hello **spustit nyní** tlačítko. Otevřete novou kartu prohlížeče a mohou být spuštěny prostřednictvím činnost koncového uživatele hello registrace do vaší aplikace pomocí hello nový obsah v místní!

## <a name="upload-hello-sample-content-tooazure-blob-storage"></a>Nahrát hello Ukázka obsahu tooAzure úložiště objektů Blob
Pokud chcete toouse Azure Blob Storage toohost stránku obsahu, můžete vytvořit účet úložiště a soubory používat naše B2C pomocné nástroje tooupload.

### <a name="create-a-storage-account"></a>vytvořit účet úložiště
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **+ nový** > **Data + úložiště** > **účet úložiště**. Budete potřebovat předplatnému Azure toocreate účet Azure Blob Storage. Můžete si zaregistrovat bezplatnou zkušební verzi na hello [webu Azure](https://azure.microsoft.com/pricing/free-trial/).
3. Zadejte **název** pro účet úložiště hello (například "contoso") a vyberte odpovídající možnosti hello pro **cenová úroveň**, **skupiny prostředků** a  **Předplatné**. Ujistěte se, že máte hello **Pin tooStartboard** možnost zaškrtnutí. Klikněte na možnost **Vytvořit**.
4. Přejděte zpět toohello úvodním panelu a klikněte na účet úložiště hello, kterou jste právě vytvořili.
5. V hello **Souhrn** klikněte na tlačítko **kontejnery**a potom klikněte na **+ přidat**.
6. Zadejte **název** pro kontejner hello (například "b2c") a vyberte **Blob** jako hello **přistupovat typu**. Klikněte na **OK**.
7. Hello kontejneru, který jste vytvořili se zobrazí v seznamu hello na hello **objekty BLOB** okno. Poznamenejte si adresu URL hello hello kontejneru; například by měla vypadat podobně jako příliš`https://contoso.blob.core.windows.net/b2c`. Zavřít hello **objekty BLOB** okno.
8. V okně účtu úložiště hello, klikněte na tlačítko **klíče** a zapište hello hodnoty hello **název účtu úložiště** a **primární přístupový klíč** pole.

> [!NOTE]
> **Primární přístupový klíč** je důležitým bezpečnostním pověřením.
> 
> 

### <a name="download-hello-helper-tool-and-sample-files"></a>Stáhnout hello pomocné nástroje a ukázkové soubory
Můžete si stáhnout hello [Azure Blob Storage pomocné rutiny nástroje a ukázkové soubory jako soubor ZIP](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) nebo klonovat z Githubu:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Toto úložiště obsahuje `sample_templates\wingtip` adresáři, který obsahuje příklad HTML, CSS a obrázků. Pro tyto šablony tooreference svůj vlastní účet Azure Blob Storage je nutné soubory tooedit hello HTML. Otevřete `unified.html` a `selfasserted.html` a nahraďte všechny výskyty `https://localhost` s adresou URL hello vlastní kontejner, který jste si poznamenali v předchozích krocích hello. Je nutné použít absolutní cesta hello hello soubory HTML, protože v takovém případě se službou Azure AD, v rámci domény hello zpracuje hello HTML `https://login.microsoftonline.com`.

### <a name="upload-hello-sample-files"></a>Nahrát hello ukázkových souborů
V hello stejné úložiště, rozbalte `B2CAzureStorageClient.zip` a spuštění hello `B2CAzureStorageClient.exe` souboru v rámci. Tento program bude jednoduše nahrát všechny hello soubory v adresáři hello zadejte účet úložiště tooyour a povolení CORS přístup pro tyto soubory. Pokud jste postupovali podle kroků hello výše, hello HTML a CSS soubory bude nyní ukazovat tooyour účet úložiště. Všimněte si, že hello název svého účtu úložiště je hello část, která předchází `blob.core.windows.net`, například `contoso`. Můžete ověřit, že obsah hello se nahrají správně tak, že zkusíte tooaccess `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` v prohlížeči. Použít také [http://test-cors.org/](http://test-cors.org/) toomake se, že obsah hello je nyní povolení CORS. (Vyhledejte "stav XHR: 200" ve výsledku hello.)

### <a name="customize-your-policy-again"></a>Přizpůsobit zásady vaší znovu
Teď, když jste odeslali účet hello Ukázka obsahu tooyour vlastní úložiště, je nutné upravit vaší registrační zásadě tooreference ho. Opakujte kroky hello z hello ["Přizpůsobit vaše zásady"](#customize-your-policy) části výše, tentokrát pomocí adresy URL svůj vlastní účet úložiště. Například hello umístění vaší `unified.html` souboru by `<url-of-your-container>/wingtip/unified.html`.

Nyní můžete pomocí hello **spustit nyní** tlačítko nebo vlastní aplikace tooexecute vaše zásady znovu. Hello výsledek by měl vyhledejte téměř úplně stejné hello stejné – jste použili hello stejné ukázkový kód HTML a CSS v obou případech. Ale zásad nyní odkazují na vlastní instanci Azure Blob Storage, a jsou volné tooedit a nahrání souborů hello znovu jako prosím. Další informace o přizpůsobení hello HTML a CSS, naleznete toohello [hlavní článek přizpůsobení uživatelského rozhraní](active-directory-b2c-reference-ui-customization.md).

