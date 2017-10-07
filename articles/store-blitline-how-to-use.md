---
title: "aaaHow toouse Blitline pro zpracování obrázků - Azure funkce Průvodce"
description: "Zjistěte, jak toouse hello Blitline služby tooprocess bitové kopie v rámci aplikace Azure."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 328fd177e25f45f29f8ad8e142d02b46017a858e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blitline-with-azure-and-azure-storage"></a>Jak toouse Blitline s Azure a Azure Storage
Tato příručka vysvětluje, jak tooaccess Blitline služby a jak toosubmit úlohy tooBlitline.

## <a name="what-is-blitline"></a>Co je Blitline?
Blitline je bitové kopie založené na cloudu zpracování služba, která poskytuje podnikové úrovni image zpracování za zlomek ceny hello, že ho náklady toobuild ho sami.

fakt Hello je, že zpracování obrázků bylo provedeno opakovaně, obvykle znovu sestavit z hello základů pro každý web. Uvědomujeme si to vzhledem k tomu, že jsme sestavili je mil. časy příliš. Jeden den, které jsme se rozhodli, možná je čas jsme právě provést pro všechny uživatele. Víme, jak toodo ho toodo je rychlé a efektivní a uložte všechny fungovat v hello té doby.

Další informace najdete v tématu [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Co Blitline není...
tooclarify co Blitline je užitečné pro je často jednodušší tooidentify co Blitline neprovádí než budete pokračovat dál.

* Blitline nemá HTML pomůcky tooupload bitové kopie. Bitové kopie, které musí mít veřejně nebo s omezenými oprávněními, které jsou k dispozici pro Blitline tooreach.
* Blitline neprovádí zpracování, jako je Aviary.com za provozu obrázku
* Blitline nepřijímá odesílání obrázků, nemůžete nabízet vaší bitové kopie tooBlitline přímo. Musíte vložit je tooAzure úložiště nebo z jiných míst, který podporuje Blitline a pak dali pokyn Blitline, kde je toogo získat.
* Blitline je massively parallel a neprovádí žádné synchronní zpracování. Což znamená, že musí poskytnout nám postback_url a jsme vám řeknou jsme se po dokončení zpracování.

## <a name="create-a-blitline-account"></a>Vytvoření účtu Blitline
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-toocreate-a-blitline-job"></a>Jak toocreate Blitline úlohy
Blitline používá JSON toodefine hello akce, které má tootake na bitovou kopii. Tento formát JSON se skládá z několika jednoduchých polí.

Příklad nejjednodušší Hello vypadá takto:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Tady bychom měli formátu JSON, který bude trvat bitovou kopii "src" "... boys.jpeg" a potom změňte velikost too240x140 této bitové kopie.

ID aplikace Hello se něco můžete najít v vaše **informace o připojení** nebo **SPRAVOVAT** karty v Azure. Je váš tajný identifikátor, který vám umožní toorun úlohy na Blitline.

Hello "uložit" parametr identifikuje informace o místo, kam chcete tooput hello image po jsme ho mít zpracování. V tomto případě trivial jsme nebyly definovány jedna. Pokud je definována žádná umístění Blitline uloží ho místně (a dočasně) v umístění jedinečný cloudu. Bude moct tooget, která umístění, ze hello JSON vrácený Blitline při provádění hello Blitline. identifikátor "image" Hello je potřeba a tooyou je vrácena, pokud tooidentify uložit tento konkrétní obrázek.

Můžete najít další informace o hello *funkce* podporujeme zde: <http://www.blitline.com/docs/functions>

Můžete také vyhledat dokumentaci o hello možnosti úlohy zde: <http://www.blitline.com/docs/api>

Až budete mít vaše struktury JSON stačí toodo je **POST** je příliš`http://api.blitline.com/job`

Zobrazí se zpět JSON, která vypadá přibližně takto:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Znamená to, Blitline přijal žádost, se pozastavil ve frontě zpracování a po jeho dokončení hello image budou k dispozici na: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikace\_ID /CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-toosave-an-image-tooyour-azure-storage-account"></a>Jak toosave tooyour image účtu úložiště Azure
Pokud máte účet úložiště Azure, můžete snadno mít Blitline nabízené hello zpracovat bitové kopie do Azure container. Přidáním "azure_destination" definujete hello umístění a oprávnění pro Blitline toopush k.

Zde naleznete příklad:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Vyplněním hello CAPITALIZED hodnoty vlastními, můžete odeslat tento toohttp://api.blitline.com/job JSON a hello "src" image budou zpracovány pomocí filtru rozostření a pak poslat tooyou Azure cílový.

### <a name="please-note"></a>Poznámka:
Hello SAS musí obsahovat hello celý SAS adresu url, včetně hello filename hello cílový soubor.

Příklad:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Můžete si také přečíst nejnovější verzi dokumentace Azure Storage na Blitline, hello [zde](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Další kroky
Navštivte blitline.com tooread o všechny naše další funkce:

* Dokumentace pro koncový bod rozhraní API Blitline <http://www.blitline.com/docs/api>
* Funkce Blitline rozhraní API <http://www.blitline.com/docs/functions>
* Příklady Blitline rozhraní API <http://www.blitline.com/docs/examples>
* Třetí součástí Nuget knihovny <http://nuget.org/packages/Blitline.Net>

