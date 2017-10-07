---
title: "aaaUsing hello Azure CDN tooaccess blobů s vlastní domény přes protokol HTTPS"
description: "Zjistěte, jak toointegrate hello Azure CDN s tooaccess objektu blob úložiště objektů BLOB s vlastní domény přes protokol HTTPS"
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mihauss
ms.openlocfilehash: f6cee36ca5495983545f2f6a8ff140677cf6914b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cdn-tooaccess-blobs-with-custom-domains-over-https"></a>Pomocí vlastních domén objekty BLOB tooaccess hello Azure CDN prostřednictvím protokolu HTTPS

Azure Content Delivery Network (CDN) teď podporuje protokol HTTPS pro vlastní názvy domén.
Můžete využít tuto funkci tooaccess úložiště objektů BLOB používání vlastní domény přes protokol HTTPS. toodo Ano, je nutné nejdříve tooenable Azure CDN na objekt blob koncový bod a mapy hello CDN tooa vlastního názvu domény. Jakmile je provést tyto kroky, povolení protokolu HTTPS pro vaše vlastní doména je zjednodušený prostřednictvím povolování jedním kliknutím, kompletní certifikát správy a všechny s bez dalších nákladů toonormal CDN ceny.

Tato možnost je důležité, protože umožňuje vám tooprotect hello ochrany osobních údajů a integritu dat dat vaše citlivé webové aplikace v průběhu přenosu. Použití provozu tooserve protokolu SSL hello přes HTTPS zajišťuje, že data se šifrují při posílání přes hello Internetu. HTTPS poskytuje vztah důvěryhodnosti a ověřování a chrání vaše webové aplikace před útoky.

> [!NOTE]
> Kromě toho tooproviding SSL podpora vlastních názvů domén, hello Azure CDN můžete škálovat obsahu velkou šířku pásma aplikace toodeliver kolem hello, world.
> toolearn víc, podívejte se na [přehled hello Azure CDN](../../cdn/cdn-overview.md).
>
>

## <a name="quick-start"></a>Rychlý start

Toto jsou hello kroky požadované tooenable HTTPS pro koncový bod služby vlastní objekt blob storage:

1.  [Účet úložiště Azure integrovat Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).
    Tento článek vás provede procesem vytvoření účtu úložiště v hello portálu Azure, pokud jste tak již neučinili.
2.  [Mapa Azure CDN obsahu tooa vlastní domény](../../cdn/cdn-map-content-to-custom-domain.md).
3.  [Povolit protokol HTTPS na vlastní domény služby Azure CDN](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Sdílené přístupové podpisy

Pokud koncový bod služby blob storage je nakonfigurované toodisallow anonymní přístup pro čtení, budete potřebovat tooprovide [sdíleného přístupového podpisu (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) v každé žádosti o token provedete tooyour vlastní doménu. Koncové body úložiště objektů blob ve výchozím nastavení zakáže anonymní přístup pro čtení. V tématu [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md) Další informace o sdílených přístupových podpisů.

Azure CDN nedodržuje tokenu SAS toohello přidané žádné omezení. Například všechny tokeny SAS mít čas vypršení platnosti. To znamená, že obsah můžete dál dostat s vypršenou platností SAS dokud tento obsah se vyprazdňují z hello CDN edge uzlů. Můžete řídit, jak dlouho je uložen v mezipaměti na hello CDN podle nastavení hlavička odpovědi hello mezipaměti. V tématu [Správa vypršení platnosti Azure úložiště objektů BLOB v Azure CDN](../../cdn/cdn-manage-expiration-of-blob-content.md) pokyny.

Pokud vytvoříte více adres URL SAS pro hello stejný koncový bod objektů blob, doporučujeme vám zapnout dotazu řetězec ukládání do mezipaměti pro Azure CDN. Toto je tooensure, jednotlivé adresy URL je považován za jedinečné entity. V tématu [řízení Azure CDN ukládání do mezipaměti chování řetězce dotazu](../../cdn/cdn-query-string.md) Další informace.

## <a name="http-toohttps-redirection"></a>TooHTTPS přesměrování protokolu HTTP

Můžete vybrat, zda tooHTTPS provoz tooredirect HTTP. To vyžaduje použití hello Azure CDN premium nabídky od společnosti Verizon. Je třeba příliš[chování přepsat HTTP pomocí stroj pravidel Azure CDN](../../cdn/cdn-rules-engine.md) s následující pravidlo:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Cdn-koncový bod name" odkazuje toohello název, který jste nakonfigurovali pro koncový bod CDN. Tuto hodnotu můžete vybrat z rozevíracího seznamu hello. "Cesta počátku" odkazuje na cestu hello v rámci účtu původní úložiště, kde je umístěn statický obsah.
Pokud hostujete všechny statického obsahu v jediný kontejner, nahraďte "cesta počátku" hello název kontejneru.

Podrobnější prohlídku do pravidla, najdete v tématu hello [Azure CDN pravidla modul funkce](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Ceny a fakturace

Při přístupu k objektům BLOB pomocí Azure CDN, platíte [objektu Blob úložiště ceny](https://azure.microsoft.com/pricing/details/storage/blobs/) pro provoz mezi uzly okraj hello a původu hello (úložiště objektů Blob), a [ceny CDN](https://azure.microsoft.com/pricing/details/cdn/) pro data z uzly okraj hello.

Řekněme například, že máte účet úložiště na západní USA, který je právě přistupovat pomocí Azure CDN. Pokud někdo v hello UK pokusí tooaccess mezi hello objektů BLOB v daném účtu úložiště prostřednictvím hello CDN, Azure nejprve ověří hello hraniční uzel nejblíže k hello UK pro tento objekt blob. Pokud najde, se používá tuto kopii objektu hello blob a použije ceny CDN, protože je přistupuje na hello CDN. Pokud není nalezen, Azure zkopíruje hello blob toohello hraniční uzel, což bude mít za následek odchozí a transakce poplatky jako hello zadaný v objektu Blob storage – ceny a přejděte k souboru hello na hello hraniční uzel, který bude mít za následek CDN fakturace.

Při prohlížení hello [CDN stránce s cenami](https://azure.microsoft.com/pricing/details/cdn/), Všimněte si, že podpora HTTPS u vlastních názvů domén je dostupná pouze pro Azure CDN z produktů Verizon (Standard a Premium).

## <a name="next-steps"></a>Další kroky

[Konfigurace vlastního názvu doménu pro koncový bod služby Blob storage](storage-custom-domain-name.md)
