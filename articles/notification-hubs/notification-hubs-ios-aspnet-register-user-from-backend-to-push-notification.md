---
title: "aktuální uživatel hello aaaRegister pro nabízená oznámení pomocí webového rozhraní API | Microsoft Docs"
description: "Zjistěte, jak toorequest registrace nabízených oznámení v aplikaci pro iOS pomocí Azure Notification Hubs při registrace se provádí pomocí rozhraní ASP.NET Web API."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f859feb436093e703d7e1db38354dd356fff8efe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="register-hello-current-user-for-push-notifications-by-using-aspnet"></a>Aktuální uživatel hello zaregistrovat pro nabízená oznámení pomocí technologie ASP.NET
> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak toorequest registrace nabízených oznámení pomocí Azure Notification Hubs při registraci se provádí pomocí rozhraní ASP.NET Web API. Toto téma rozšiřuje hello kurzu [upozorněte uživatele s Notification Hubs]. Musí jste již dokončili hello požadované kroky v tomto kurzu toocreate hello ověření mobilní služby. Další informace o hello upozornit uživatele scénář, najdete v části [upozorněte uživatele s Notification Hubs].

## <a name="update-your-app"></a>Aktualizace aplikace
1. V MainStoryboard_iPhone.storyboard přidejte následující součásti z objektu knihovny hello hello:
   
   * **Popisek**: "Push tooUser s centry oznámení"
   * **Popisek**: "InstallationId"
   * **Popisek**: "User"
   * **Textové pole**: "User"
   * **Popisek**: "Password"
   * **Textové pole**: "Password"
   * **Tlačítko**: "Přihlášení"
     
     V tomto okamžiku by váš scénáře vypadá hello následující:
     
      ![][0]
2. V editoru pomocníka hello vytvořit výstupy pro všechny ovládací prvky hello přepnout a volat, připojit hello textové pole s hello View Controller (delegát) a vytvořte **akce** pro hello **přihlášení** tlačítko.
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain hello following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. Vytvoření třídy s názvem **DeviceInfo**, a kopírování hello následující kód do části rozhraní hello hello souboru DeviceInfo.h:
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. Zkopírujte následující kód v oddílu implementace hello hello DeviceInfo.m souboru hello:
   
            @synthesize installationId = _installationId;
   
            - (id)init {
                if (!(self = [super init]))
                    return nil;
   
                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);
   
                    //store hello install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }
   
                return self;
            }
   
            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }
5. V PushToUserAppDelegate.h přidejte následující vlastnost singleton hello:
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. V hello **didFinishLaunchingWithOptions** metoda v PushToUserAppDelegate.m, přidejte následující kód hello:
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    první řádek Hello inicializuje hello **DeviceInfo** typu singleton. Hello druhý řádek spustí hello registrace pro nabízená oznámení, které se již nachází je jste už dokončili hello [Začínáme s Notification Hubs] kurzu.
7. V PushToUserAppDelegate.m, implementujte metodu hello **didRegisterForRemoteNotificationsWithDeviceToken** ve vaší AppDelegate a přidejte následující kód hello:
   
        self.deviceInfo.deviceToken = deviceToken;
   
    Toto nastaví hello token zařízení pro žádost hello.
   
   > [!NOTE]
   > V tomto okamžiku by neměly existovat jiný kód v této metodě. Pokud již máte volání toohello **registerNativeWithDeviceToken** metoda, která byla přidána po dokončení hello [Začínáme s Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) kurz, musíte okomentujte nebo odebrat volání.
   > 
   > 
8. Hello PushToUserAppDelegate.m souboru přidejte následující metodu obslužná rutina hello:
   
   * (void) aplikace:(UIApplication *) aplikace didReceiveRemoteNotification:(NSDictionary *) informací o uživateli {NSLog (@"% @", informací o uživateli);   UIAlertView * výstraha = [zpráva initWithTitle:@"Notification" [UIAlertView alokační]: cancelButtonTitle delegáta: nil [informací o uživateli objectForKey:@"inAppMessage"]: @"OK" otherButtonTitles:nil, nil];   [Zobrazit výstrahy]; }
   
   Tato metoda zobrazí výstrahu v hello uživatelského rozhraní, když aplikace obdrží oznámení, když je spuštěná.
9. Otevřete soubor PushToUserViewController.m hello a návratové hello klávesnice v hello následující implementace:
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. V hello **viewDidLoad** metoda v souboru PushToUserViewController.m hello inicializovat hello installationId popisek následujícím způsobem:
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. Přidejte následující vlastnosti v rozhraní v PushToUserViewController.m hello:
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. Pak přidejte následující implementace hello:
    
            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }
    
            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];
    
                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
    
                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;
    
                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;
    
                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }
    
                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }
    
                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }
13. Kopírování hello následující kód do hello **přihlášení** vytvořené XCode metoda obslužné rutiny:
    
            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    
            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];
    
            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];
    
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];
    
            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];
    
    Tato metoda získá ID instalace a kanál pro nabízená oznámení a odešle ji, společně s hello typu zařízení, toohello ověření webového rozhraní API metoda, která vytvoří registraci v Notification Hubs. Toto webové rozhraní API byla definovaná v [upozorněte uživatele s Notification Hubs].

Teď, když hello klientskou aplikaci se aktualizovala, vrátí toohello [upozorněte uživatele s Notification Hubs] a aktualizovat hello mobilní služby toosend oznámení pomocí centra oznámení.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[upozorněte uživatele s Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet

[Začínáme s Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
