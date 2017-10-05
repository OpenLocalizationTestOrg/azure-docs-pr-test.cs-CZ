---
title: "Stáhnout do počítače - Azure Media Services prostředky | Microsoft Docs"
description: "Další informace se stáhne prostředků do vašeho počítače. Ukázky kódu jsou napsané v jazyce C# a pomocí sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: d8e740e969f68c85842f42c109328423da1b4414
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-deliver-an-asset-by-download"></a><span data-ttu-id="81f46-104">Postupy: poskytování prostředek službou</span><span class="sxs-lookup"><span data-stu-id="81f46-104">How to: Deliver an Asset by Download</span></span>
<span data-ttu-id="81f46-105">Toto téma popisuje možnosti pro doručování média prostředky nahrán do Media Services.</span><span class="sxs-lookup"><span data-stu-id="81f46-105">This topic discusses options for delivering media assets uploaded to Media Services.</span></span> <span data-ttu-id="81f46-106">V mnoha případech aplikace můžete doručování obsahu Media Services.</span><span class="sxs-lookup"><span data-stu-id="81f46-106">You can deliver Media Services content in numerous application scenarios.</span></span> <span data-ttu-id="81f46-107">Můžete stáhnout média prostředky, nebo k nim přistupovat pomocí lokátoru.</span><span class="sxs-lookup"><span data-stu-id="81f46-107">You can download media assets, or access them by using a locator.</span></span> <span data-ttu-id="81f46-108">Můžete odeslat mediální obsah do jiná aplikace nebo do jiného poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="81f46-108">You can send media content to another application or to another content provider.</span></span> <span data-ttu-id="81f46-109">Pro lepší výkon a škálovatelnost můžete také doručovat obsah pomocí sítě doručování obsahu (CDN).</span><span class="sxs-lookup"><span data-stu-id="81f46-109">For improved performance and scalability, you can also deliver content by using a Content Delivery Network (CDN).</span></span>

<span data-ttu-id="81f46-110">Tento příklad ukazuje, jak stáhnout datových zdrojů médií z Media Services do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="81f46-110">This example shows how to download media assets from Media Services to your local computer.</span></span> <span data-ttu-id="81f46-111">Kód dotazuje úlohy přidružené k účtu Media Services tak, že ID úlohy a přístupů jeho **OutputMediaAssets** kolekce (což je sada jeden nebo více prostředků výstup média, která je výsledkem spuštění úlohy).</span><span class="sxs-lookup"><span data-stu-id="81f46-111">The code queries the jobs associated with the Media Services account by job ID and accesses its **OutputMediaAssets** collection (which is the set of one or more output media assets that results from running a job).</span></span> <span data-ttu-id="81f46-112">Tento příklad ukazuje, jak stáhnout prostředky média výstup z úlohy, ale můžete použít ve stejný přístup ke stažení dalších prostředků.</span><span class="sxs-lookup"><span data-stu-id="81f46-112">This  example shows how to download output media assets from a job, but you can apply the same approach to download other assets.</span></span>

>[!NOTE]
><span data-ttu-id="81f46-113">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="81f46-113">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="81f46-114">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="81f46-114">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="81f46-115">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="81f46-115">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="81f46-116">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="81f46-116">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="81f46-117">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="81f46-117">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="81f46-118">Viz také</span><span class="sxs-lookup"><span data-stu-id="81f46-118">See Also</span></span>
[<span data-ttu-id="81f46-119">Doručování streamování obsahu</span><span class="sxs-lookup"><span data-stu-id="81f46-119">Deliver streaming content</span></span>](media-services-deliver-streaming-content.md)

