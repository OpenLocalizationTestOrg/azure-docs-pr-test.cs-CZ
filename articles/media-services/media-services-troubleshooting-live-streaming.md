---
title: "Průvodce aaaTroubleshooting pro živé streamování | Microsoft Docs"
description: "Toto téma nabízí návrhy na tom, jak tootroubleshoot živé streamování problémy."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 8549bae947ff3b225ce624220d1e48b63f90208c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a><span data-ttu-id="68399-103">Průvodce řešením potíží s živým streamováním</span><span class="sxs-lookup"><span data-stu-id="68399-103">Troubleshooting guide for live streaming</span></span>
<span data-ttu-id="68399-104">Toto téma nabízí návrhy na postupy tootroubleshoot některé živé streamování problémy.</span><span class="sxs-lookup"><span data-stu-id="68399-104">This topic gives suggestions on how tootroubleshoot some live streaming problems.</span></span>

## <a name="issues-related-tooon-premises-encoders"></a><span data-ttu-id="68399-105">Problémy související s tooon místních kodérů</span><span class="sxs-lookup"><span data-stu-id="68399-105">Issues related tooon-premises encoders</span></span>
<span data-ttu-id="68399-106">Tato část nabízí návrhy na tom, jak tootroubleshoot problémy související tooon místní kodéry, které jsou nakonfigurované toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="68399-106">This section gives suggestions on how tootroubleshoot problems related tooon-premises encoders that are configured toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>

### <a name="problem-would-like-toosee-logs"></a><span data-ttu-id="68399-107">Problém: Byste chtěli toosee protokoly</span><span class="sxs-lookup"><span data-stu-id="68399-107">Problem: Would like toosee logs</span></span>
* <span data-ttu-id="68399-108">**Potenciální problém**: Nelze najít protokoly kodér, který vám může pomoci při ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="68399-108">**Potential issue**: Can't find encoder logs that might help in debugging issues.</span></span>
  
  * <span data-ttu-id="68399-109">**Telestream Wirecast**: obvykle najdete v části C:\Users protokoly\{uživatelské jméno} \AppData\Roaming\Wirecast\\</span><span class="sxs-lookup"><span data-stu-id="68399-109">**Telestream Wirecast**: You can usually find logs under C:\Users\{username}\AppData\Roaming\Wirecast\\</span></span> 
  * <span data-ttu-id="68399-110">**Elementární živé**: můžete najít má toologs odkazy na portálu pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="68399-110">**Elemental Live**: You can find has links toologs on hello management portal.</span></span> <span data-ttu-id="68399-111">Klikněte na **statistiky**, pak **protokoly**.</span><span class="sxs-lookup"><span data-stu-id="68399-111">Click on **Stats**, then **Logs**.</span></span> <span data-ttu-id="68399-112">Na hello **soubory protokolu** stránky, se zobrazí seznam protokolů pro všechny položky LiveEvent hello; vyberte hello jeden odpovídající aktuální relace.</span><span class="sxs-lookup"><span data-stu-id="68399-112">On hello **Log Files** page, you will see a list of logs for all hello LiveEvent items; select hello one matching your current session.</span></span> 
  * <span data-ttu-id="68399-113">**Flash Live kodér médií**: můžete najít hello **adresář protokolu...**  přechodem toohello **kódování protokolu** kartě.</span><span class="sxs-lookup"><span data-stu-id="68399-113">**Flash Media Live Encoder**: You can find hello **Log Directory...** by navigating toohello **Encoding Log** tab.</span></span>

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a><span data-ttu-id="68399-114">Problém: Není žádná možnost pro výstup vytvořeného progresivní datového proudu</span><span class="sxs-lookup"><span data-stu-id="68399-114">Problem: There is no option for outputting a progressive stream</span></span>
* <span data-ttu-id="68399-115">**Potenciální problém**: kodér hello používá nebude automaticky zrušit prokládání.</span><span class="sxs-lookup"><span data-stu-id="68399-115">**Potential issue**: hello encoder being used doesn't automatically deinterlace.</span></span> 
  
    <span data-ttu-id="68399-116">**Řešení potíží**: Vyhledejte odstraňování prokládání možnost v rámci rozhraní kodér hello.</span><span class="sxs-lookup"><span data-stu-id="68399-116">**Troubleshooting steps**: Look for a de-interlacing option within hello encoder interface.</span></span> <span data-ttu-id="68399-117">Po povolení algoritmy pro odstranění prokládání znovu zkontrolujte nastavení progresivní výstup.</span><span class="sxs-lookup"><span data-stu-id="68399-117">Once de-interlacing is enabled, check again for progressive output settings.</span></span> 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-tooconnect"></a><span data-ttu-id="68399-118">Problém: Pokus o několik nastavení výstupní kodér a stále se nedaří tooconnect.</span><span class="sxs-lookup"><span data-stu-id="68399-118">Problem: Tried several encoder output settings and still unable tooconnect.</span></span>
* <span data-ttu-id="68399-119">**Potenciální problém**: Azure kódování kanál nebyl správně resetován.</span><span class="sxs-lookup"><span data-stu-id="68399-119">**Potential issue**: Azure encoding channel was not properly reset.</span></span> 
  
    <span data-ttu-id="68399-120">**Řešení potíží**: Ujistěte se, hello encoder je už vkládání tooAMS, zastavení a obnovení hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="68399-120">**Troubleshooting steps**: Make sure hello encoder is no longer pushing tooAMS, stop and reset hello channel.</span></span> <span data-ttu-id="68399-121">Jednou spustit znovu, zkuste se připojit kodér s novým nastavením hello.</span><span class="sxs-lookup"><span data-stu-id="68399-121">Once running again, try connecting your encoder with hello new settings.</span></span> <span data-ttu-id="68399-122">Pokud tento problém hello stále nevyřeší, zkuste vytvořit nový kanál zcela, někdy kanály se může stát poškozená po několika nezdařených pokusů o zadání.</span><span class="sxs-lookup"><span data-stu-id="68399-122">If this still does not correct hello issue, try creating a new channel entirely, sometimes channels can become corrupt after several failed attempts.</span></span>  
* <span data-ttu-id="68399-123">**Potenciální problém**: nejsou optimální velikost GOP hello nebo nastavení klíčových snímků.</span><span class="sxs-lookup"><span data-stu-id="68399-123">**Potential issue**: hello GOP size or key frame settings are not optimal.</span></span> 
  
    <span data-ttu-id="68399-124">**Řešení potíží**: GOP doporučená velikost nebo @keyframe, které určuje interval je 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="68399-124">**Troubleshooting steps**: Recommended GOP size or keyframe interval is 2 seconds.</span></span> <span data-ttu-id="68399-125">Některé kodéry vypočítat toto nastavení v počet snímků, jiné zase sekund.</span><span class="sxs-lookup"><span data-stu-id="68399-125">Some encoders calculate this setting in number of frames, while others use seconds.</span></span> <span data-ttu-id="68399-126">Příklad: při výstupu 30fps, hello GOP velikost bude 60 rámce, který je ekvivalentní too2 sekund.</span><span class="sxs-lookup"><span data-stu-id="68399-126">For example: When outputting 30fps, hello GOP size would be 60 frames, which is equivalent too2 seconds.</span></span>  
* <span data-ttu-id="68399-127">**Potenciální problém**: uzavřené porty blokují hello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="68399-127">**Potential issue**: Closed ports are blocking hello stream.</span></span> 
  
    <span data-ttu-id="68399-128">**Řešení potíží**: při streamování prostřednictvím RTMP, zkontrolovat brány firewall nebo proxy nastavení tooconfirm, že jsou otevřené odchozí porty 1935 a 1936.</span><span class="sxs-lookup"><span data-stu-id="68399-128">**Troubleshooting steps**: When streaming via RTMP, check firewall and/or proxy settings tooconfirm that outbound ports 1935 and 1936 are open.</span></span> <span data-ttu-id="68399-129">Při použití RTP streamování, potvrďte, že odchozí port 2010 je otevřen.</span><span class="sxs-lookup"><span data-stu-id="68399-129">When using RTP streaming, confirm that outbound port 2010 is open.</span></span> 

### <a name="problem-when-configuring-hello-encoder-toostream-with-hello-rtp-protocol-there-is-no-place-tooenter-a-host-name"></a><span data-ttu-id="68399-130">Problém: Při konfiguraci hello kodér toostream s hello protokol RTP, není prostor tooenter název hostitele.</span><span class="sxs-lookup"><span data-stu-id="68399-130">Problem: When configuring hello encoder toostream with hello RTP protocol, there is no place tooenter a host name.</span></span>
* <span data-ttu-id="68399-131">**Potenciální problém**: mnoho RTP kodéry zakázat u názvů hostitele a IP adresu, bude nutné toobe získali.</span><span class="sxs-lookup"><span data-stu-id="68399-131">**Potential issue**: Many RTP encoders do not allow for host names, and an IP address will need toobe acquired.</span></span>  
  
    <span data-ttu-id="68399-132">**Řešení potíží**: toofind hello IP adresu, otevřete příkazový řádek na libovolném počítači.</span><span class="sxs-lookup"><span data-stu-id="68399-132">**Troubleshooting steps**: toofind hello IP address, open a command prompt on any computer.</span></span> <span data-ttu-id="68399-133">toodo v systému Windows, otevře hello spustit Spouštěče (WIN + R) a zadejte tooopen "cmd".</span><span class="sxs-lookup"><span data-stu-id="68399-133">toodo this in Windows, open hello Run launcher (WIN + R) and type “cmd” tooopen.</span></span>  
  
    <span data-ttu-id="68399-134">Jakmile hello příkazového řádku je otevřený, zadejte "Ping [AMS název hostitele]".</span><span class="sxs-lookup"><span data-stu-id="68399-134">Once hello command prompt is open, type "Ping [AMS Host Name]".</span></span> 
  
    <span data-ttu-id="68399-135">název hostitele Hello lze odvodit vynecháním hello číslo portu od hello Azure Ingestované adresy URL, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="68399-135">hello host name can be derived by omitting hello port number from hello Azure Ingest URL, as highlighted in hello following example:</span></span> 
  
    <span data-ttu-id="68399-136">RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 /</span><span class="sxs-lookup"><span data-stu-id="68399-136">rtp://test2-amstest009.rtp.channel.mediaservices.windows.net:2010/</span></span> 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> <span data-ttu-id="68399-138">Pokud po kroků hello řešení potíží, které stále nemůžete použít datový proud úspěšně, odešlete lístek podpory pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="68399-138">If after following hello troubleshooting steps you still cannot successfully stream, submit a support ticket using hello Azure portal.</span></span>
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="68399-139">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="68399-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="68399-140">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="68399-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

