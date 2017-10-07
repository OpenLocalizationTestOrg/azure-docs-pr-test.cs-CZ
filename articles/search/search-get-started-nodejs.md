---
title: "aaaGet začít s Azure Search v Node.js | Microsoft Docs"
description: "Projděte si sestavení vyhledávací aplikace v hostované cloudové vyhledávací službě v Azure pomocí programovacího jazyka Node.js."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: evboyle
ms.openlocfilehash: e9c7d756c2ea191ee2a285485c90439b96aa73b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a>Začínáme se službou Azure Search v Node.js
> [!div class="op_single_selector"]
> * [Azure Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Zjistěte, jak toobuild vlastní Node.js Hledat aplikace, která používá službu Azure Search své zkušenosti vyhledávání. Tento kurz používá hello [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekty a operace, na které se použijí v tomto cvičení.

Použili jsme [Node.js](https://Nodejs.org) a NPM, [Sublime Text 3](http://www.sublimetext.com/3)a prostředí Windows PowerShell na Windows 8.1 toodevelop a testování tento kód.

toorun této ukázky musíte mít službu Azure Search, které se můžete zaregistrovat si v hello [portál Azure](https://portal.azure.com). V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) podrobné pokyny.

## <a name="about-hello-data"></a>O datech hello
Tato ukázková aplikace používá data z hello [Spojených států Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), která jsou filtrovaná na velikost datové sady hello státu Rhode Island tooreduce hello. Použijeme tato data toobuild vyhledávací aplikaci, která najde významné budovy, například nemocnice a školy, a geologické prvky, jako jsou datové proudy, jezera a vrcholy.

V této aplikaci hello **DataIndexer** program sestaví a zatížením hello index pomocí [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstrukce, načítání hello filtrovat sadu dat USGS z veřejné databáze Azure SQL Database. Přihlašovací údaje a připojení zdroje dat online toohello informace je uvedené v kódu programu hello. Není potřeba žádná další konfigurace.

> [!NOTE]
> Jsme použili filtr na tuto datovou sadu toostay pod hello 10 000 dokumentů limit hello volné cenová úroveň. Pokud používáte úroveň standard hello, toto omezení neplatí. Podrobnosti týkající se kapacity u jednotlivých cenových úrovní najdete v tématu [Omezení služby Search](search-limits-quotas-capacity.md).
> 
> 

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a>Nalezení názvu služby hello a klíč api-key služby Azure Search
Po vytvoření služby hello návratovou adresu URL hello portálu tooget toohello nebo `api-key`. Připojení tooyour službu vyhledávání vyžadují, abyste měli obě adresy URL hello a `api-key` tooauthenticate hello volání.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Hello přechod panelu klikněte na **služby vyhledávání** toolist všechny služby Azure Search zřízených pro předplatné.
3. Vyberte službu hello chcete toouse.
4. Na řídicím panelu služby hello měli byste vidět dlaždice se základními informacemi, jako je například hello ikonu klíče pro přístup ke klíčům správce hello.
5. Zkopírujte adresu URL služby hello, klíč správce a klíč dotazu. Je třeba všechny tři později při jejich přidání souboru config.js toohello.

## <a name="download-hello-sample-files"></a>Stažení ukázkových souborů hello
Použijte buď jeden z hello následující ukázka hello toodownload přístupy.

1. Přejděte příliš[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).
2. Klikněte na tlačítko **stáhnout ZIP**, uložte soubor .zip hello a extrahujte všechny soubory hello obsahuje.

Všechny následné úpravy souborů a spouštěné příkazy se provádí na souborech v této složce.

## <a name="update-hello-configjs-with-your-search-service-url-and-api-key"></a>Aktualizace souboru config.js hello. pomocí adresy URL služby Search a klíče rozhraní API
Pomocí hello adresy URL a rozhraní api-key, který jste zkopírovali dříve, zadejte adresu URL hello, klíč správce a klíč dotazu v konfiguračním souboru.

Klíče správce poskytují plnou kontrolu nad operacemi služby, včetně vytvoření nebo odstranění indexu a nahrávání dokumentů. Naproti tomu klíče dotazu jsou operacím jen pro čtení, obvykle používají v klientských aplikací, které se připojují tooAzure vyhledávání.

V této ukázce jsme zahrnují hello dotazu klíče toohelp posílit osvědčený postup hello používá klíč dotazu hello v klientských aplikacích.

Následující snímek obrazovky ukazuje Hello **config.js** otevřete v textovém editoru, s hello příslušné položky jsou označené, aby mohli zobrazit, kde tooupdate hello soubor s hello hodnoty, které jsou platné pro vaši službu vyhledávání.

![][5]

## <a name="host-a-runtime-environment-for-hello-sample"></a>Hostování běhového prostředí pro ukázku hello
Hello ukázka vyžaduje server HTTP, které můžete nainstalovat globálně pomocí npm.

Pro hello následující příkazy použijte okno prostředí PowerShell.

1. Přejděte toohello složku, která obsahuje hello **package.json** souboru.
2. Zadejte `npm install`.
3. Zadejte `npm install -g http-server`.

## <a name="build-hello-index-and-run-hello-application"></a>Sestavení indexu hello a spuštění aplikace hello
1. Zadejte `npm run indexDocuments`.
2. Zadejte `npm run build`.
3. Zadejte `npm run start_server`.
4. Nasměrování prohlížeče na adresu `http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Hledání v datech USGS
Hello sada dat USGS obsahuje záznamy, které jsou relevantní toohello státu Rhode Island. Pokud kliknete na tlačítko **vyhledávání** na prázdného vyhledávacího pole, získáte hello prvních 50 položek, což je výchozí hello.

Zadat hledaný termín poskytuje vyhledávací hello něco toogo na. Zkuste zadat místní název. "Roger Williams" byla hello prvním guvernérem státu Rhode Island. Je po něm pojmenovaná celá řada parků, budov a škol.

![][9]

Může taky zkusit kterýkoli z těchto výrazů:

* Pawtucket
* Pembroke
* goose +cape

## <a name="next-steps"></a>Další kroky
Toto je hello první kurz služby Azure Search na základě Node.js a hello sadu dat USGS. V čase budete rozšiřujeme tento kurz toodemonstrate dalších vyhledávacích funkcí, které můžete chtít toouse ve vlastních řešeních.

Pokud už službu Azure Search trochu znáte, tato ukázka vám může posloužit jako odrazový můstek k vyzkoušení modulů pro automatické návrhy (našeptávání nebo automatické dokončování dotazů), filtrů a fasetové navigace. Můžete taky zdokonalit stránku výsledků hledání hello přidáním počtů a dávkování dokumenty tak, aby hello výsledky procházet po stránkách.

Nové tooAzure hledání? Doporučujeme vyzkoušet ostatní kurzy toodevelop představu o co můžete vytvořit. Navštivte naše [stránky dokumentace, která](https://azure.microsoft.com/documentation/services/search/) toofind více prostředků. Můžete také zobrazit hello odkazy v našem [seznamu videí a kurzů](search-video-demo-tutorial-list.md) tooaccess Další informace.

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
