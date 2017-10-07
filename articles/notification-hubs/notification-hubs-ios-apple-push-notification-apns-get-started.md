---
title: "aaaSending nabízená oznámení tooiOS pomocí Azure Notification Hubs | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan iOS aplikace."
services: notification-hubs
documentationcenter: ios
keywords: "nabízené oznámení;nabízená oznámení;nabízená oznámení ios"
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: d8bb47fee4c229b3ed2a7a4dbff25a56a7a7d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooios-with-azure-notification-hubs"></a>Odesílání nabízených oznámení tooiOS pomocí Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan iOS aplikace. Vytvoříte prázdnou aplikaci iOS, která přijímá nabízená oznámení pomocí hello [služby nabízených oznámení Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.

## <a name="before-you-begin"></a>Než začnete
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Hello dokončit kód v tomto kurzu lze najít [na Githubu](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* [Mobile Services iOS SDK verze 1.2.4]
* Poslední verze jazyka [Xcode]
* Zařízení podporující iOS 8 (nebo novější verze)
* Členství v [programu pro vývojáře Apple](https://developer.apple.com/programs/).
  
  > [!NOTE]
  > Z důvodu požadavky na konfiguraci pro nabízená oznámení musíte nasadit a otestovat nabízená oznámení na fyzickém zařízení iOS (iPhone nebo iPad) namísto simulátoru iOS hello.
  > 
  > 

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurace centra oznámení pro nabízená oznámení iOS
Tato část vás provede vytvořením nového centra oznámení a konfiguraci ověřování pomocí služby APNS pomocí hello **.p12** nabízený certifikát, který jste vytvořili. Pokud chcete toouse Centrum oznámení, které jste už vytvořili, můžete přeskočit toostep 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Klikněte na tlačítko hello <b>služby oznámení</b> tlačítka na hello <b>nastavení</b> okně vyberte <b>Apple (APNS)</b>. Klikněte na <b>nahrát certifikát</b> a vyberte hello <b>.p12</b> soubor, který jste dříve exportovali. Ujistěte se, že zadáváte správné heslo hello.</p>

<p>Ujistěte se, že tooselect <b>izolovaného prostoru</b> režimu, protože se jedná o vývoj. Používat pouze hello <b>produkční</b> Pokud chcete, aby toousers toosend nabízená oznámení, kteří zakoupili aplikaci z úložiště hello.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![Konfigurace služby APN na portálu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;&emsp;&emsp;![Konfigurace certifikační služby APNS na portálu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

Vaše centrum oznámení je nyní nakonfigurované toowork pomocí služby APNS a máte hello připojovací řetězce tooregister vaší aplikace a odesílání nabízených oznámení.

## <a name="connect-your-ios-app-toonotification-hubs"></a>Připojit vaše iOS aplikace tooNotification rozbočovače
1. V Xcode, vytvořte nový projekt iOS a vyberte hello **jediné zobrazení aplikace** šablony.
   
    ![Xcode – jediné zobrazení aplikace][8]
    
2. Při nastavování hello možnosti pro nový projekt, ujistěte se, toouse hello stejné **název produktu** a **identifikátor organizace** jste použili při předchozím nastavení sady ID hello hello vývojáře Apple portál.
   
    ![Xcode – možnosti projektu][11]
    
3. V části **cíle**, klikněte na název projektu, klikněte na hello **nastavení sestavení** a rozbalte **identitu podepisování kódu**a potom v části **ladění**, nastavte svoji identitu podepisování kódu. Přepnutí **úrovně** z **základní** příliš**všechny**a nastavte **profil zřizování** toohello profil, který jste předtím vytvořili pro zřizování .
   
    Pokud nevidíte hello nový zajišťování profil, který jste vytvořili v Xcode, pokuste se aktualizovat hello profily pro podpisové identity. Klikněte na tlačítko **Xcode** v řádku nabídek hello, klikněte na tlačítko **Předvolby**, klikněte na tlačítko hello **účet** , klikněte na hello **zobrazit podrobnosti** tlačítko, klikněte na vaše podepisování identity a pak klikněte na tlačítko Aktualizovat hello v pravém dolním rohu hello.
   
    ![Xcode – profil zřizování][9]
4. Stáhnout hello [Mobile Services iOS SDK verze 1.2.4] a rozbalte soubor hello. V Xcode, klikněte pravým tlačítkem na projekt a klikněte na tlačítko hello **přidat soubory do** možnost tooadd hello **WindowsAzureMessaging.framework** projektu Xcode tooyour složky. Vyberte možnost **Kopírovat položky v případě potřeby** a pak klikněte na tlačítko **Přidat**.
   
   > [!NOTE]
   > Hello centra oznámení SDK aktuálně nepodporuje bitcode na Xcode 7.  Je nutné nastavit **povolit Bitcode** příliš**ne** v hello **sestavení možnosti** pro váš projekt.
   > 
   > 
   
    ![Rozbalte Azure SDK][10]
5. Přidat nový hlavičky souboru tooyour projekt s názvem `HubInfo.h`. Tento soubor bude obsahovat hello konstanty pro vaše Centrum oznámení.  Přidejte následující definice hello a nahraďte s hello zástupné symboly literálu řetězce vaše *název centra* a hello *DefaultListenSharedAccessSignature* který jste si předtím poznamenali.
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter hello name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. Otevřete váš `AppDelegate.h` souboru přidejte následující direktivy importu hello:
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. Ve vaší `AppDelegate.m file`, přidejte následující kód v hello hello `didFinishLaunchingWithOptions` metoda založené na vaší verzi iOS. Tento kód zaregistruje popisovač vašeho zařízení do APN:
   
    Pro iOS 8:
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    Pro too8 předchozí verze iOS:
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. V hello stejný soubor, přidejte následující metody hello. Tento kód se připojí toohello centra oznámení pomocí hello informace o připojení, který jste zadali v souboru HubInfo.h. Poté přiřadí centra oznámení tokenu toohello hello zařízení tak, aby hello centra oznámení můžete odesílat oznámení:
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. V hello stejný soubor, přidejte následující metodu toodisplay hello **UIAlert** Pokud bylo přijato oznámení hello je hello aplikace aktivní:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. Sestavení a spuštění aplikace hello na vašem zařízení tooverify, že neexistují žádné chyby.

## <a name="send-test-push-notifications"></a>Odešlete nabízená oznámení
Příjem oznámení ve vaší aplikaci odesláním nabízených oznámení v hello můžete otestovat [portálu Azure] prostřednictvím hello **Poradce při potížích s** část v okně centra hello (použijte hello *testovací odeslání* možnost).

![Portál Azure – testovací odeslání][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-hello-app"></a>(Volitelné) Odesílání nabízených oznámení z aplikace hello
> [!IMPORTANT]
> Tady je příklad odesílání oznámení z klienta aplikace hello se poskytuje pro učení pouze pro účely. Vzhledem k tomu, že to bude vyžadovat hello `DefaultFullSharedAccessSignature` toobe v hello klientskou aplikaci, zpřístupňuje riziko toohello centra oznámení, že uživatel může získat oznámení toosend neoprávněného přístupu tooyour klientů.
> 
> 

Pokud chcete toosend nabízená oznámení z aplikace, tato část poskytuje příklad toodo této konfigurace pomocí hello rozhraní REST.

1. V Xcode otevřete `Main.storyboard` a přidejte následující součásti uživatelského rozhraní z hello objekt knihovny tooallow hello uživatele toosend nabízená oznámení v aplikaci hello hello:
   
   * Popisek bez textu popisku. Bude použité tooreport chyb v odesílání oznámení. Hello **řádky** vlastnost musí být nastavená příliš**0** tak, aby se automaticky velikost omezené toohello práva a levý okraj a horní hello hello zobrazení.
   * Textové pole s **zástupný symbol** text nastaven příliš**zadejte zprávu oznámení**. Omezte hello pole přímo pod hello štítek, jak je uvedeno níže. Nastavte hello řadiče zobrazení jako delegáta výstupu hello.
   * Tlačítko s názvem **odeslat oznámení** omezené pod textové pole hello a ve vodorovném centru hello.
     
     zobrazení Hello by měla vypadat takto:
     
     ![Návrhář Xcode][32]
2. [Přidejte výstupy](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) pro hello popisek a textové pole připojené k vašemu zobrazení a aktualizovat vaše `interface` definice toosupport `UITextFieldDelegate` a `NSXMLParserDelegate`. Přidejte hello tři vlastnosti deklarací uvedené níže toohelp podporu volání hello REST API a analyzování odpovědi hello.
   
    Váš soubor ViewController.h by měl vypadat následovně:
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected tooyour UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. Otevřete `HubInfo.h` a přidejte následující konstanty, které se použije pro odesílání oznámení tooyour rozbočovače hello. Nahraďte hello zástupný symbol řetězcového literálu skutečným *DefaultFullSharedAccessSignature* připojovací řetězec.
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. Přidejte následující hello `#import` tooyour příkazy `ViewController.h` souboru.
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. V `ViewController.m` přidejte následující kód implementace rozhraní toohello hello. Tento kód provede analýzu vašeho připojovacího řetězce *DefaultFullSharedAccessSignature*. Jak je uvedeno v hello [odkazu k REST API](http://msdn.microsoft.com/library/azure/dn495627.aspx), tyto analyzované informace budou použité toogenerate tokenu SaS pro hello **autorizace** hlavičky žádosti.
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. V `ViewController.m`, aktualizace hello `viewDidLoad` metoda tooparse hello připojovacího řetězce při načtení zobrazení hello. Také přidat hello pomocné metody uvedené níže, toohello implementaci rozhraní.  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. V `ViewController.m`, přidejte následující kód toohello rozhraní implementace toogenerate hello tokenu autorizace SaS, bude k dispozici v hello hello **autorizace** záhlaví, jak je uvedeno v hello [REST API Referenční dokumentace](http://msdn.microsoft.com/library/azure/dn495627.aspx).
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. CTRL + přetažení z hello **odeslat oznámení** tlačítko příliš`ViewController.m` tooadd akci s názvem **SendNotificationMessage** pro hello **Touch Down** událostí. Metoda aktualizace pomocí následujícího kódu toosend hello oznámení pomocí rozhraní REST API hello hello.
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of hello notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct hello message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate hello token toobe used in hello authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello APNs notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                       [xmlParser parse];
                }
            }];
            [dataTask resume];
        }
3. V `ViewController.m`, přidejte následující toosupport metoda delegáta zavřením hello klávesnice pro textové pole hello hello. CTRL + přetažení z ikonu řadiče zobrazení hello textové pole toohello v hello rozhraní návrháře tooset hello zobrazení jako delegáta výstupu hello řadiče.
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. V `ViewController.m`, přidejte následující hello delegovat metody toosupport analýzy hello odpovědi pomocí `NSXMLParser`.
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set hello status label text on hello UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. Sestavení projektu hello a ověřte, zda nejsou žádné chyby.

> [!NOTE]
> Pokud dojde k chybě sestavení v Xcode7 o podpoře bitcode, měli byste změnit hello **nastavení sestavení** > **povolit Bitcode (ENABLE_BITCODE)** příliš**ne** v Xcode. Hello centra oznámení SDK aktuálně nepodporuje bitcode. 
> 
> 

Můžete najít všechna možná oznámení datových částí hello v hello Apple [průvodci místních a nabízených oznámení programování].

## <a name="checking-if-your-app-can-receive-push-notifications"></a>Kontrola, zda vaše aplikace může přijímat nabízená oznámení
tootest nabízená oznámení na iOS, musíte nasadit hello aplikace tooa fyzickém zařízení iOS. Nabízená oznámení Apple nelze odeslat pomocí simulátoru iOS hello.

1. Spuštění aplikace hello a ověřte, zda byla registrace úspěšná a stiskněte klávesu **OK**.
   
    ![Test registrace nabízených oznámení aplikace iOS][33]
2. Můžete odeslat testovací nabízená oznámení z hello [portálu Azure], jak je popsáno výše. Pokud jste přidali kód pro odesílání nabízených oznámení v aplikaci hello, klepněte do hello textové pole tooenter zprávu oznámení. Stiskněte klávesu hello **odeslat** na klávesnici hello nebo hello tlačítko **odeslat oznámení** tlačítka na hello zobrazení toosend hello oznámení.
   
    ![Test odeslání nabízených oznámení aplikace iOS][34]
3. Hello nabízená oznámení se odesílá tooall zařízení, která jsou registrovaná tooreceive hello oznámení z konkrétní centra oznámení hello.
   
    ![Test příjmu nabízených oznámení aplikace iOS][35]

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste vysílali nabízená oznámení tooall vaše registrovaná zařízení iOS. Doporučujeme jako další krok ve svém studiu, že přejdete toohello [Azure upozornění uživatelů centra oznámení pro iOS pomocí backendu .NET] kurz, který vás provede procesem vytvoření back-end toosend nabízená oznámení pomocí značek. 

Pokud chcete toosegment uživatele podle zájmových skupin, můžete se navíc přesunout na toohello [toosend použití centra oznámení nejnovější zprávy přes] kurzu. 

Obecné informace o centrech oznámení naleznete v tématu [Průvodce centry oznámení].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK verze 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure upozornění uživatelů centra oznámení pro iOS pomocí backendu .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[průvodci místních a nabízených oznámení programování]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[portálu Azure]: https://portal.azure.com
