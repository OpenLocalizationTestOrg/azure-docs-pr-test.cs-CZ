---
title: "řezy aaaRedact v Průvodci Azure Media Analytics | Microsoft Docs"
description: "Toto téma ukazuje podrobné informace o tom, toorun úplné redigování pracovní postup pomocí Azure Media Services Explorer (AMSE) a Azure Media Redactor vizualizér (nástroj s otevřeným zdrojem)."
services: media-services
documentationcenter: 
author: Lichard
manager: cfowler
editor: 
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: ab28f4052b73fdb74fcd5766235eab35402a0c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Redigovat tyto řezy v Průvodci Azure Media Analytics

## <a name="overview"></a>Přehled

**Azure Media Redactor** je [Azure Media Analytics](media-services-analytics-overview.md) procesor médií (PP), která nabízí redigování škálovatelné řez v cloudu hello. Vzhled redigování umožňuje vám toomodify videa v pořadí tooblur řezy vybrané osob. Můžete chtít toouse hello vzhled redigování služby ve veřejné scénáře zabezpečení a média zprávy. Pár minut záznamů, která obsahuje více řezy může trvat hodiny tooredact ručně, ale s této služby hello vzhled redigování proces bude vyžadovat několika jednoduchých kroků. Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Podrobnosti o **Azure Media Redactor**, najdete v části hello [vzhled redigování přehled](media-services-face-redaction.md) tématu.

Toto téma ukazuje podrobné informace o tom, toorun úplné redigování pracovní postup pomocí Azure Media Services Explorer (AMSE) a Azure Media Redactor vizualizér (nástroj s otevřeným zdrojem).

Hello **Azure Media Redactor** MP je aktuálně ve verzi Preview. Je k dispozici ve všech veřejných oblastí Azure a také datových centrech US Government a Číně. Tato verze preview je aktuálně zdarma. V aktuální verzi hello je omezena na 10 minut na zpracování video délka.

Další informace najdete v tématu [to](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blogu.

## <a name="azure-media-services-explorer-workflow"></a>Azure Media Services Explorer pracovního postupu

Hello tooget nejjednodušší způsob, jak začít s Redactor je nástroj AMSE s otevřeným zdrojem toouse hello na githubu. Můžete spustit zjednodušené pracovního postupu prostřednictvím **kombinaci** režim, pokud nemáte potřebujete přístup toohello poznámky json nebo hello vzhled jpg bitové kopie.

### <a name="download-and-setup"></a>Stažení a instalaci

1. Stáhněte si nástroj AMSE hello z [zde](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Přihlaste se tooyour účtu Media Services pomocí klíč služby.

    tooobtain hello název účtu a klíčové informace, přejděte toohello [portál Azure](https://portal.azure.com/) a vyberte svůj účet AMS. Zvolte Nastavení > klíče. Zobrazuje název účtu hello Hello Správa klíčů windows a hello primární a sekundární klíče se zobrazí. Zkopírujte hodnoty hello název účtu a primární klíč hello.

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>První předání – analyzovat režimu

1. Nahrávání souborů médií prostřednictvím Asset –> nahrávání, nebo prostřednictvím přetažení. 
1. Klikněte pravým tlačítkem a zpracování souboru média pomocí Media Analytics –> Azure Media Redactor –> analyzovat režimu. 


![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

výstup Hello bude obsahovat soubor json poznámky se data o umístění řez, jakož i jpg každý zjištěný řezu. 

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a>Druhý předat – redigovat režimu

1. Nahrajte vašeho původního asset videa toohello výstup z první fáze hello a nastavit jako primární asset. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (Volitelné) Nahrajte soubor 'Dance_idlist.txt', který zahrnuje nový řádek oddělený seznam ID chcete tooredact hello. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (Volitelné) Zkontrolujte všechny úpravy toohello annotations.json souboru, jako je zvýšení hello ohraničující hranice pole. 
4. Klikněte pravým tlačítkem na výstupní asset hello z první fáze hello, vyberte hello Redactor a spustit s hello **Redact** režimu. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Stažení nebo sdílet hello konečné zredigované výstupní asset. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a>Nástroj Azure vizualizér Redactor médií s otevřeným zdrojem

Typu open source [vizualizér nástroj](https://github.com/Microsoft/azure-media-redactor-visualizer) je navrženou toohelp vývojáři právě spouští ve formátu poznámky hello s analýzou a pomocí výstup hello.

Po klonování hello úložišti, v pořadí toorun hello projektu, budete potřebovat toodownload FFMPEG z jejich [oficiální web](https://ffmpeg.org/download.html).

Pokud jste vývojář při tooparse hello poznámky data JSON, podívejte se do Models.MetaData příklady ukázkový kód.

### <a name="set-up-hello-tool"></a>Nastavit nástroj hello

1.  Stáhněte si a sestavte celé řešení hello. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Stáhněte si FFMPEG z [zde](https://ffmpeg.org/download.html). Tento projekt byl původně vytvořen s verze be1d324 (2016-10-04) s statické propojení. 
3.  Zkopírujte ffmpeg.exe a ffprobe.exe toohello stejnou výstupní složky jako AzureMediaRedactor.exe. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. Spusťte AzureMediaRedactor.exe. 

### <a name="use-hello-tool"></a>Pomocí nástroje hello

1. Zpracovat videa v účtu Azure Media Services s hello Redactor MP na režimu analyzovat. 
2. Stáhnout hello původní soubor videa a hello výstup hello redigování - analyzovat úlohy. 
3. Spusťte aplikaci vizualizér hello a vyberte soubory hello výše. 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Zobrazte náhled souboru. Vyberte, jenž čelí jste chtěli tooblur prostřednictvím hello bočním panelu na hello správné. 
    
    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  Vzhled hello ID aktualizuje Hello dolní textové pole. Vytvořte soubor s názvem "idlist.txt" v těchto ID jako seznam oddělený nový řádek. 

    >[!NOTE]
    > Hello idlist.txt má být uložen v ANSI. Můžete použít Poznámkový blok toosave v ANSI.
    
6.  Odešlete tento soubor toohello výstupní asset z kroku 1. Nahrání hello původní video toothis asset také a nastavit jako primární asset. 
7.  Spusťte úlohu redigování tento prostředek s "Redact" režimu tooget hello v posledním zredigované video. 

## <a name="next-steps"></a>Další kroky 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Související odkazy
[Azure Media Services Analytics – přehled](media-services-analytics-overview.md)

[Ukázky služby Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Uvedení vzhled redigování pro Azure Media Analytics](https://azure.microsoft.com/blog/azure-media-redactor/)
