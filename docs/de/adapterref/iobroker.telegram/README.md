---
translatedFrom: en
translatedWarning: Wenn Sie dieses Dokument bearbeiten möchten, löschen Sie bitte das Feld "translationsFrom". Andernfalls wird dieses Dokument automatisch erneut übersetzt
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/de/adapterref/iobroker.telegram/README.md
title: ioBroker-Telegrammadapter
hash: jWi7uWMh/Jbm7Sb2TWH5+ijaXdhBq3K/rLyGsDaqitw=
---
![Logo](../../../en/adapterref/iobroker.telegram/admin/telegram.png)

![Anzahl der Installationen](http://iobroker.live/badges/telegram-stable.svg)
![NPM-Version](http://img.shields.io/npm/v/iobroker.telegram.svg)
![Downloads](https://img.shields.io/npm/dm/iobroker.telegram.svg)

# IoBroker-Telegrammadapter
![Testen und freigeben](https://github.com/iobroker-community-adapters/iobroker.telegram/workflows/Test%20and%20Release/badge.svg) [![Übersetzungsstatus](https://weblate.iobroker.net/widgets/adapters/-/telegram/svg-badge.svg)](https://weblate.iobroker.net/engage/adapters/?utm_source=widget)

**Dieser Adapter verwendet Sentry-Bibliotheken, um Ausnahmen und Codefehler automatisch an die Entwickler zu melden.** Weitere Details und Informationen zum Deaktivieren der Fehlerberichterstattung finden Sie unter [Sentry-Plugin-Dokumentation](https://github.com/ioBroker/plugin-sentry#plugin-sentry)! Sentry Reporting wird ab js-controller 3.0 verwendet.

## Aufbau
Bitten Sie [@BotVater](https://telegram.me/botfather), einen neuen Bot ```/newbot``` zu erstellen.

Sie werden aufgefordert, den Namen des Bots und dann den Benutzernamen einzugeben.
Danach erhalten Sie den Token.

![Bildschirmfoto](../../../en/adapterref/iobroker.telegram/img/chat.png)

Sie sollten das Passwort für die Kommunikation im Konfigurationsdialog festlegen. Starten Sie danach den Adapter.

Um eine Unterhaltung mit Ihrem Bot zu beginnen, müssen Sie den Benutzer mit `/password phrase` authentifizieren, wobei **`phrase`** Ihr konfiguriertes Passwort ist. Öffnen Sie also eine neue Konversation mit Ihrem generierten Bot in Telegram und geben Sie dann das Passwort als ersten Befehl ein.

**Hinweis:** Sie können die Kurzform `/p phrase` verwenden.

Um ein nettes Avatar-Bild hinzuzufügen, gib `/setuserpic` im **BotFather**-Chat ein und lade das gewünschte Bild (512x512 Pixel) hoch, wie dieses hier [Logo](img/logo.png).

Sie können die Nachricht an alle authentifizierten Benutzer über die messageBox `sendTo('telegram', 'Test message')` oder an einen bestimmten Benutzer `sendTo('telegram', '@userName Test message')` senden.
Der Benutzer muss zuvor authentifiziert werden.

Sie können den Benutzer auch auf diese Weise angeben:

```
sendTo('telegram', {user: 'UserName', text: 'Test message'}, function (res) {
    console.log('Sent to ' + res + ' users');
});
```

Wenn Sie das obige Beispiel verwenden, beachten Sie, dass Sie 'UserName' entweder durch den Vornamen oder den Public-Telegram-Benutzernamen des Benutzers ersetzen müssen, an den Sie die Nachricht senden möchten. (Hängt davon ab, ob die Einstellung 'Benutzername nicht Vorname speichern' in den Adaptereinstellungen aktiviert ist oder nicht) Wenn die Option gesetzt ist und der Benutzer keinen öffentlichen Benutzernamen in seinem Telegrammkonto angegeben hat, verwendet der Adapter weiterhin den Vornamen von der Benutzer. Denken Sie daran, dass, wenn der Benutzer später (nach der Authentifizierung bei Ihrem Bot) einen öffentlichen Benutzernamen festlegt, der gespeicherte Vorname durch den Benutzernamen ersetzt wird, wenn der Benutzer das nächste Mal eine Nachricht an den Bot sendet.

Es ist möglich, mehr als einen Empfänger anzugeben (trennen Sie einfach die Benutzernamen durch Kommas).
Beispiel: Empfänger: "Benutzer1,Benutzer4,Benutzer5"

Sie können die Nachricht auch über den Status senden, setzen Sie einfach den Status *"telegram.INSTANCE.communicate.response"* mit dem Wert *"@userName Test message"* oder mit einem JSON-Objekt:

```
{
    text: "Test message"
}
```

Die JSON-Syntax erlaubt auch das Hinzufügen von Optionen aus [Telegramm-Bots-API](https://core.telegram.org/bots/api) sowie das Setzen der Benutzer- oder ChatId:

```
{
    text: "Test message, but with *bold*",
    parse_mode: "Markdown",
    chatId: "1234567890",
    user: "UserName"
}
```

Um Nachrichten an die Gruppen zu senden, müssen Sie den Bot in die Gruppe einladen, in der der Bot posten soll.
Durch Bereitstellen von `chat_id` für die JSON-Nachrichtennutzlast können Sie tatsächlich Nachrichten an diese Gruppen senden.

Um die `chat_id` herauszufinden, müssen Sie den Loglevel des Adapters auf `debug` setzen.
Sie können Ihren Bot dann einfach in den Gruppen anpingen, an die der Bot Nachrichten senden soll.
Stellen Sie sicher, dass Sie der Nachricht ein `/` voranstellen, damit der Bot die Nachricht sehen kann ([wenn die Privatsphäre des Bots eingeschaltet ist](#How-to-receive-messages-in-group-chats-using-telegram-adapter)).
Das iobroker-Protokoll zeigt Ihnen dann die Chat-ID in den Protokollen.

## Verwendungszweck
Sie können das Telegramm mit dem Adapter [Text2Befehl](https://github.com/ioBroker/ioBroker.text2command) verwenden. Es gibt vordefinierte Kommunikationsschemata und Sie können sie in Textform nach Hause befehlen.

Um ein Foto zu senden, senden Sie einfach einen Pfad zur Datei anstelle von Text oder URL: `sendTo('telegram', 'absolute/path/file.png')` oder `sendTo('telegram', 'https://telegram.org/img/t_logo.png')`.

Beispiel, wie man einen Screenshot von einer Webcam an ein Telegramm sendet:

```
var request = require('request');
var fs      = require('fs');

function sendImage() {
    request.get({url: 'http://login:pass@ipaddress/web/tmpfs/snap.jpg', encoding: 'binary'}, function (err, response, body) {
        fs.writeFile("/tmp/snap.jpg", body, 'binary', function(err) {

        if (err) {
            console.error(err);
        } else {
            console.log('Snapshot sent');
            sendTo('telegram.0', '/tmp/snap.jpg');
            //sendTo('telegram.0', {text: '/tmp/snap.jpg', caption: 'Snapshot'});
        }
      });
    });
}
on("someState", function (obj) {
    if (obj.state.val) {
        // send 4 images: immediately, in 5, 15 and 30 seconds
        sendImage();
        setTimeout(sendImage, 5000);
        setTimeout(sendImage, 15000);
        setTimeout(sendImage, 30000);
    }
});
```

Folgende Meldungen sind für Aktionen reserviert:

- *tippen* - für Textnachrichten,
- *upload_photo* - für Fotos,
- *upload_video* - für Videos,
- *record_video* - für Videos,
- *record_audio* - für Audio,
- *upload_audio* - für Audio,
- *upload_document* - für Dokumente,
- *find_location* - für Standortdaten

In diesem Fall wird der Aktionsbefehl gesendet.

Die Beschreibung für die Telegramm-API finden Sie in [Hier](https://core.telegram.org/bots/api) und Sie können alle in dieser API definierten Optionen verwenden, indem Sie diese einfach in das Sendeobjekt aufnehmen. Z.B.:

```
sendTo('telegram.0', {
    text:                   '/tmp/snap.jpg',
    caption:                'Snapshot',
    disable_notification:   true
});
```

**Möglichkeiten**:

- *disable_notification*: Sendet die Nachricht still. Die iOS-Benutzer erhalten keine Benachrichtigung, Android-Benutzer erhalten eine Benachrichtigung ohne Ton. (alle Arten)
- *parse_mode*: Senden Sie Markdown oder HTML, wenn Sie möchten, dass Telegram-Apps fett, kursiv, Text mit fester Breite oder Inline-URLs in der Nachricht Ihres Bots anzeigen. Mögliche Werte: "Markdown", "MarkdownV2", "HTML" (Nachricht)
- *disable_web_page_preview*: Deaktiviert Linkvorschauen für Links in dieser Nachricht (Nachricht)
- *Beschriftung*: Beschriftung für das Dokument, Foto oder Video, 0-200 Zeichen (Video, Audio, Foto, Dokument)
- *duration*: Dauer des gesendeten Videos oder Audios in Sekunden (Audio, Video)
- *performer*: Interpret der Audiodatei (Audio)
- *title*: Titelname der Audiodatei (Audio)
- *Breite*: Videobreite (Video)
- *height*: Videohöhe (Video)

Der Adapter versucht, den Typ der Nachricht (Foto, Video, Audio, Dokument, Aufkleber, Aktion, Ort) zu erkennen, hängt vom Text in der Nachricht ab, wenn der Text ein Pfad zu einer vorhandenen Datei ist, wird er als entsprechender Typ gesendet.

Der Standort wird anhand des Breitengradattributs erkannt:

```
sendTo('telegram.0', {
    latitude:               52.522430,
    longitude:              13.372234,
    disable_notification:   true
});
```

### Explizite Arten von Nachrichten
Sie haben die Möglichkeit, den Typ der Nachricht zusätzlich zu definieren, falls Sie die Daten als Puffer senden möchten.

Folgende Typen sind möglich: *Sticker*, *Video*, *Dokument*, *Audio*, *Foto*.

```
sendTo('telegram.0', {
    text: fs.readFileSync('/opt/path/picture.png'),
    type: 'photo'
});
```

### Klaviatur
Sie können die Tastatur **ReplyKeyboardMarkup** im Client anzeigen:

```
sendTo('telegram.0', {
    text:   'Press button',
    reply_markup: {
        keyboard: [
            ['Line 1, Button 1', 'Line 1, Button 2'],
            ['Line 2, Button 3', 'Line 2, Button 4']
        ],
        resize_keyboard:   true,
        one_time_keyboard: true
    }
});
```

Sie können mehr lesen [hier](https://core.telegram.org/bots/api#replykeyboardmarkup) und [hier](https://core.telegram.org/bots#keyboards).

Sie können die Tastatur **InlineKeyboardMarkup** im Client anzeigen:

```
sendTo('telegram', {
    user: user,
    text: 'Click the button',
    reply_markup: {
        inline_keyboard: [
            [{ text: 'Button 1_1', callback_data: '1_1' }],
            [{ text: 'Button 1_2', callback_data: '1_2' }]
        ]
    }
});
```

Sie können mehr lesen [hier](https://core.telegram.org/bots/api#inlinekeyboardmarkup) und [hier](https://core.telegram.org/bots#inline-keyboards-and-on-the-fly-updating).

**HINWEIS:** *Nachdem der Benutzer eine Rückruftaste gedrückt hat, zeigen Telegram-Clients einen Fortschrittsbalken an, bis Sie answerCallbackQuery aufrufen. Daher muss auch dann mit dem Aufruf von answerCallbackQuery reagiert werden, wenn keine Benachrichtigung an den Benutzer erforderlich ist (z. B. ohne Angabe eines der optionalen Parameter).*

### AnswerCallbackQuery
Verwenden Sie diese Methode, um Antworten auf Rückrufabfragen zu senden, die von Inline-Tastaturen gesendet werden. Die Antwort wird dem Benutzer als Benachrichtigung oben im Chat-Bildschirm oder als Alarm angezeigt. Bei Erfolg wird *True* zurückgegeben.

```
if (command ==="1_2") {
    sendTo('telegram', {
        user: user,
        answerCallbackQuery: {
            text: "Pressed!",
            showAlert: false // Optional parameter
        }
   });
}
```

Sie können mehr lesen [Hier](https://github.com/yagop/node-telegram-bot-api/blob/release/doc/api.md#telegrambotanswercallbackquerycallbackqueryid-text-showalert-options--promise).

### Frage
Sie können die Nachricht per Telegramm senden und die nächste Antwort wird im Rückruf zurückgegeben.
Timeout kann in der Konfiguration eingestellt werden und beträgt standardmäßig 60 Sekunden.

```
sendTo('telegram.0', 'ask', {
    user: user, // optional
    text: 'Are you sure?',
    reply_markup: {
        inline_keyboard: [
            // two buttons could be on one line too, but here they are on different
            [{ text: 'Yes!',  callback_data: '1' }], // first line
            [{ text: 'No...', callback_data: '0' }]  // second line
        ]
    }
}, msg => {
    console.log('user says ' + msg.data);
});
```

## Chat-ID
Ab Version 0.4.0 können Sie die Chat-ID verwenden, um Nachrichten an den Chat zu senden.

`sendTo('telegram.0', {text: 'Message to chat', chatId: 'SOME-CHAT-ID-123');`

## Nachrichten aktualisieren
Mit den folgenden Methoden können Sie eine vorhandene Nachricht im Nachrichtenverlauf ändern, anstatt eine neue Nachricht als Ergebnis einer Aktion zu senden. Dies ist am nützlichsten für Nachrichten mit *Inline-Tastaturen*, die Rückrufabfragen verwenden, kann aber auch dazu beitragen, Unordnung in Gesprächen mit normalen Chat-Bots zu reduzieren.

### Nachrichtentext bearbeiten
Verwenden Sie diese Methode, um vom Bot oder über den Bot (für Inline-Bots) gesendeten Text zu bearbeiten. Bei Erfolg, wenn eine bearbeitete Nachricht vom Bot gesendet wird, wird die bearbeitete Nachricht zurückgegeben, andernfalls wird *True* zurückgegeben.

```
if (command === "1_2") {
    sendTo('telegram', {
        user: user,
        text: 'New text before buttons',
        editMessageText: {
            options: {
                chat_id: getState("telegram.0.communicate.requestChatId").val,
                message_id: getState("telegram.0.communicate.requestMessageId").val,
                reply_markup: {
                    inline_keyboard: [
                        [{ text: 'Button 1', callback_data: '2_1' }],
                        [{ text: 'Button 2', callback_data: '2_2' }]
                    ],
                }
            }
        }
    });
}
```

*oder neuer Text für letzte Nachricht:*

```
if (command ==="1_2") {
    sendTo('telegram', {
        user: user,
        text: 'New text message',
        editMessageText: {
            options: {
                chat_id: getState("telegram.0.communicate.requestChatId").val,
                message_id: getState("telegram.0.communicate.requestMessageId").val,
            }
        }
    });
}
```

Sie können mehr lesen [Hier](https://github.com/yagop/node-telegram-bot-api/blob/release/doc/api.md#telegramboteditmessagetexttext-options--promise).

### EditMessageCaption
Verwenden Sie diese Methode, um die Beschriftung der vom Bot oder über den Bot (für Inline-Bots) gesendeten Nachricht zu bearbeiten.
Bei Erfolg, wenn eine bearbeitete Nachricht vom Bot gesendet wird, wird die bearbeitete Nachricht zurückgegeben, andernfalls wird *True* zurückgegeben.

```
if (command === "1_2") {
    sendTo('telegram', {
        user, // optional
        text: 'New caption',
        editMessageCaption: {
            options: {
                chat_id: getState("telegram.0.communicate.requestChatId").val,
                message_id: getState("telegram.0.communicate.requestMessageId").val
            }
        }
    });
}
```

Sie können mehr lesen [Hier](https://github.com/yagop/node-telegram-bot-api/blob/release/doc/api.md#telegramboteditmessagetexttext-options--promise).

### EditMessageMedia
Verwenden Sie diese Methode, um das Bild der vom Bot oder über den Bot (für Inline-Bots) gesendeten Nachricht zu bearbeiten.
Bei Erfolg, wenn eine bearbeitete Nachricht vom Bot gesendet wird, wird die bearbeitete Nachricht zurückgegeben, andernfalls wird *True* zurückgegeben.

```
if (command === "1_2") {
    sendTo('telegram', {
        user, // optional
        text: 'picture.jpg',
        editMessageMedia: {
            options: {
                chat_id: (await getStateAsync('telegram.0.communicate.botSendChatId')).val,
                message_id: (await getStateAsync('telegram.0.communicate.botSendMessageId')).val
            }
        }
    });
}
```

Unterstützt werden folgende Medientypen: `photo`, `animation`, `audio`, `document`, `video`.

Sie können mehr lesen [Hier](https://core.telegram.org/bots/api#editmessagemedia).

### EditMessageReplyMarkup
Verwenden Sie diese Methode, um nur das Antwort-Markup von Nachrichten zu bearbeiten, die vom Bot oder über den Bot (für Inline-Bots) gesendet wurden. Bei Erfolg, wenn eine bearbeitete Nachricht vom Bot gesendet wird, wird die bearbeitete Nachricht zurückgegeben, andernfalls wird *True* zurückgegeben.

```
if (command === "1_2") {
    sendTo('telegram', {
        user: user,
        text: 'New text before buttons',
        editMessageReplyMarkup: {
            options: {
                chat_id: getState("telegram.0.communicate.botSendChatId").val,
                message_id: getState("telegram.0.communicate.botSendMessageId").val,
                reply_markup: {
                    inline_keyboard: [
                        [{ text: 'Button 1', callback_data: '2_1' }],
                        [{ text: 'Button 2', callback_data: '2_2' }]
                    ],
                }
            }
        }
    });
}
```

Sie können mehr lesen [Hier](https://github.com/yagop/node-telegram-bot-api/blob/release/doc/api.md#telegramboteditmessagereplymarkupreplymarkup-options--promise).

### Nachricht löschen
Verwenden Sie diese Methode zum Löschen einer Nachricht, einschließlich Servicenachrichten, mit den folgenden Einschränkungen:

- Eine Nachricht kann nur gelöscht werden, wenn sie vor weniger als 48 Stunden gesendet wurde.

Gibt bei Erfolg *True* zurück.

```
if (command === "delete") {
    sendTo('telegram', {
        user: user,
        deleteMessage: {
            options: {
                chat_id: getState("telegram.0.communicate.requestChatId").val,
                message_id: getState("telegram.0.communicate.requestMessageId").val
            }
        }
    });
}
```

Sie können mehr lesen [Hier](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#TelegramBot+deleteMessage).

## Auf Benutzerantworten/Nachrichten reagieren
Angenommen, Sie verwenden nur JavaScript ohne *text2command*. Sie haben bereits eine Nachricht/Frage an Ihren Benutzer gesendet, indem Sie *sendTo()* wie oben beschrieben verwendet haben. Der Benutzer antwortet darauf, indem er einen Knopf drückt oder eine Nachricht schreibt. Sie können den Befehl extrahieren und Ihrem Benutzer Feedback geben, Befehle ausführen oder den Status in iobroker ändern.

 -telegram.0 ist Ihre iobroker-Telegram-Instanz, die Sie verwenden möchten
 - Benutzer ist der bei Ihrem TelegramBot registrierte Benutzer, der die Nachricht gesendet hat
 - Befehl ist der Befehl, den Ihr TelegramBot erhalten hat

```
on({id: 'telegram.0.communicate.request', change: 'any'}, function (obj) {
    var stateval = getState('telegram.0.communicate.request').val;              // save Statevalue received from your Bot
    var user = stateval.substring(1,stateval.indexOf("]"));                 // extract user from the message
    var command = stateval.substring(stateval.indexOf("]")+1,stateval.length);   // extract command/text from the message

    switch (command) {
        case "1_2":
            //... see example above ...
            break;
        case "delete":
            //... see example above
            break;
        //.... and so on ...
    }
});

```

## Sonderbefehle
### /state stateName - Statuswert lesen
Sie können den Statuswert anfordern, wenn Sie jetzt die ID:

```
/state system.adapter.admin.0.memHeapTotal
> 56.45
```

### /state stateName value - Statuswert festlegen
Sie können den Wert von state festlegen, wenn Sie nun die ID:

```
/state hm-rpc.0.JEQ0ABCDE.3.STOP true
> Done
```

## Abfrage- oder Servermodus
Wenn der Polling-Modus verwendet wird, fragt der Adapter standardmäßig alle 300 ms den Telegrammserver nach Updates ab. Es verwendet Datenverkehr und Nachrichten können bis zum Abfrageintervall verzögert werden. Das Abfrageintervall kann in der Adapterkonfiguration definiert werden.

Um den Servermodus zu verwenden, muss Ihre ioBroker-Instanz aus dem Internet erreichbar sein (z. B. mit `noip.com` dynamischem DNS-Dienst).

Telegram funktioniert nur mit HTTPS-Servern, aber Sie können **let’s encrypt**-Zertifikate verwenden.

Für den Servermodus müssen folgende Einstellungen vorgenommen werden:

- URL - im Format https://ihredomain.com:8443.
- IP - IP-Adresse, an die der Server gebunden wird. Standard 0.0.0.0. Ändern Sie es nicht, wenn Sie sich nicht sicher sind.
- Port - tatsächlich werden nur 443, 80, 88, 8443 Ports per Telegramm unterstützt, aber Sie können Ports über Ihren Router an jeden weiterleiten.
- Öffentliches Zertifikat - erforderlich, wenn **let's encrypt** deaktiviert ist.
- Privater Schlüssel - erforderlich, wenn **let's encrypt** deaktiviert ist.
- Kettenzertifikat (optional)
- Let's encrypt-Optionen - Es ist sehr einfach, **let's encrypt**-Zertifikate einzurichten. Bitte lesen Sie [hier](https://github.com/ioBroker/ioBroker.admin#lets-encrypt-certificates) darüber.

## Erweiterte Sicherheit
Die Authentifizierung von Benutzern konnte deaktiviert werden. So kann sich niemand neu authentifizieren.

Um eine Liste vertrauenswürdiger Benutzer zu erstellen, deaktivieren Sie zunächst die Option „Neue Benutzer nicht authentifizieren“ und authentifizieren Sie alle Benutzer, die in der vertrauenswürdigen Liste enthalten sein sollen, indem Sie die Nachricht `/password <PASSWORD>` senden.

Die Benutzer, die ein gültiges Passwort gesendet haben, werden in der Vertrauensliste gespeichert.

Danach könnte die Option „Neue Benutzer nicht authentifizieren“ aktiviert werden und es können sich keine neuen Benutzer mehr authentifizieren.

Um diese Option nutzen zu können, muss die Option „Authentifizierte Benutzer merken“ aktiviert sein.

## Anrufe per Telegramm
Dank der [callmebot](https://www.callmebot.com/)-API können Sie Ihr Telegrammkonto anrufen und einige Texte werden über die TTS-Engine gelesen.

Rufen Sie dazu einfach vom Javascript-Adapter aus auf:

```
sendTo('telegram.0', 'call', 'Some text');
```

oder

```
sendTo('telegram.0', 'call', {
    text: 'Some text',
    user: '@Username', // optional and the call will be done to the first user in telegram.0.communicate.users.
    language: 'de-DE-Standard-A' // optional and the system language will be taken
    repeats: 0, // number of repeats
});
```

oder

```
sendTo('telegram.0', 'call', {
    text: 'Some text',
    users: ['@Username1', '+49xxxx'] // Array of `users' or telephone numbers.
});
```

oder

```
sendTo('telegram.0', 'call', {
    file: 'url of mp3 file that is accessible from internet',
    users: ['@Username1', '@Username2'] // Array of `users' or telephone numbers.
});
```

Mögliche Werte für Sprache:

- `ar-XA-Standard-A` - Arabisch (weibliche Stimme)
- `ar-XA-Standard-B` - Arabisch (Männerstimme)
- `ar-XA-Standard-C` - Arabisch (Männerstimme 2)
- `cs-CZ-Standard-A` - Tschechisch (Tschechische Republik) (weibliche Stimme)
- `da-DK-Standard-A` - Dänisch (Dänemark) (weibliche Stimme)
- `nl-NL-Standard-A` - Niederländisch (Niederlande) (weibliche Stimme - wird verwendet, wenn die Systemsprache NL ist und keine Sprache angegeben wurde)
- `nl-NL-Standard-B` - Niederländisch (Niederlande) (Männerstimme)
- `nl-NL-Standard-C` - Niederländisch (Niederlande) (Männerstimme 2)
- `nl-NL-Standard-D` - Niederländisch (Niederlande) (Frauenstimme 2)
- `nl-NL-Standard-E` - Niederländisch (Niederlande) (Frauenstimme 3)
- `en-AU-Standard-A` - Englisch (Australien) (weibliche Stimme)
- `en-AU-Standard-B` - Englisch (Australien) (Männerstimme)
- `en-AU-Standard-C` - Englisch (Australien) (Frauenstimme 2)
- `en-AU-Standard-D` - Englisch (Australien) (männlich 2 Stimmen)
- `en-IN-Standard-A` - Englisch (Indien) (weibliche Stimme)
- `en-IN-Standard-B` - Englisch (Indien) (Männerstimme)
- `en-IN-Standard-C` - Englisch (Indien) (Männerstimme 2)
- `en-GB-Standard-A` - Englisch (UK) (weibliche Stimme - wird verwendet, wenn die Systemsprache EN ist und keine Sprache angegeben wurde)
- `en-GB-Standard-B` - Englisch (UK) (Männerstimme)
- `en-GB-Standard-C` - Englisch (UK) (Frauenstimme 2)
- `en-GB-Standard-D` - Englisch (UK) (männlich 2 Stimmen)
- `en-US-Standard-B` - Englisch (US) (Männerstimme)
- `en-US-Standard-C` - Englisch (US) (weibliche Stimme)
- `en-US-Standard-D` - Englisch (US) (männlich 2 Stimmen)
- `en-US-Standard-E` - Englisch (US) (Frauenstimme 2)
- `fil-PH-Standard-A` - Filipino (Philippinen) (weibliche Stimme)
- `fi-FI-Standard-A` - Finnisch (Finnland) (weibliche Stimme)
- `fr-CA-Standard-A` - Französisch (Kanada) (weibliche Stimme)
- `fr-CA-Standard-B` - Französisch (Kanada) (Männerstimme)
- `fr-CA-Standard-C` - Französisch (Kanada) (Frauenstimme 2)
- `fr-CA-Standard-D` - Französisch (Kanada) (Männerstimme 2)
- `fr-FR-Standard-A` - Französisch (Frankreich) (Frauenstimme - wird verwendet, wenn die Systemsprache FR ist und keine Sprache angegeben wurde)
- `fr-FR-Standard-B` - Französisch (Frankreich) (Männerstimme)
- `fr-FR-Standard-C` - Französisch (Frankreich) (Frauenstimme 2)
- `fr-FR-Standard-D` - Französisch (Frankreich) (Männerstimme 2)
- `de-DE-Standard-A` - Deutsch (Deutschland) (Frauenstimme - wird verwendet, wenn die Systemsprache DE ist und keine Sprache angegeben wurde)
- `de-DE-Standard-B` - Deutsch (Deutschland) (Männerstimme)
- `el-GR-Standard-A` - Griechisch (Griechenland) (weibliche Stimme)
- `hi-IN-Standard-A` - Hindi (Indien) (weibliche Stimme)
- `hi-IN-Standard-B` - Hindi (Indien) (Männerstimme)
- `hi-IN-Standard-C` - Hindi (Indien) (Männerstimme 2)
- `hu-HU-Standard-A` - Ungarisch (Ungarn) (weibliche Stimme)
- `id-ID-Standard-A` - Indonesisch (Indonesien) (weibliche Stimme)
- `id-ID-Standard-B` - Indonesisch (Indonesien) (Männerstimme)
- `id-ID-Standard-C` - Indonesisch (Indonesien) (Männerstimme 2)
- `it-IT-Standard-A` - Italienisch (Italien) (weibliche Stimme - wird verwendet, wenn die Systemsprache IT ist und keine Sprache angegeben wurde)
- `it-IT-Standard-B` - Italienisch (Italien) (Frauenstimme 2)
- `it-IT-Standard-C` - Italienisch (Italien) (Männerstimme)
- `it-IT-Standard-D` - Italienisch (Italien) (Männer 2 Stimme)
- `ja-JP-Standard-A` - Japanisch (Japan) (weibliche Stimme)
- `ja-JP-Standard-B` - Japanisch (Japan) (Frauenstimme 2)
- `ja-JP-Standard-C` - Japanisch (Japan) (Männerstimme)
- `ja-JP-Standard-D` - Japanisch (Japan) (Männerstimme 2)
- `ko-KR-Standard-A` - Koreanisch (Südkorea) (weibliche Stimme)
- `ko-KR-Standard-B` - Koreanisch (Südkorea) (Frauenstimme 2)
- `ko-KR-Standard-C` - Koreanisch (Südkorea) (Männerstimme)
- `ko-KR-Standard-D` - Koreanisch (Südkorea) (Männerstimme 2)
- `cmn-CN-Standard-A` - Mandarin-Chinesisch (weibliche Stimme)
- `cmn-CN-Standard-B` - Mandarin-Chinesisch (Männerstimme)
- `cmn-CN-Standard-C` - Mandarin-Chinesisch (männliche 2-Stimme)
- `nb-NO-Standard-A` - Norwegisch (Norwegen) (weibliche Stimme)
- `nb-NO-Standard-B` - Norwegisch (Norwegen) (Männerstimme)
- `nb-NO-Standard-C` - Norwegisch (Norwegen) (Frauenstimme 2)
- `nb-NO-Standard-D` - Norwegisch (Norwegen) (Männerstimme 2)
- `nb-no-Standard-E` - Norwegisch (Norwegen) (weibliche 3-Stimme)
- `pl-PL-Standard-A` - Polnisch (Polen) (weibliche Stimme - wird verwendet, wenn die Systemsprache PL ist und keine Sprache angegeben wurde)
- `pl-PL-Standard-B` - Polnisch (Polen) (Männerstimme)
- `pl-PL-Standard-C` - Polnisch (Polen) (Männerstimme 2)
- `pl-PL-Standard-D` - Polnisch (Polen) (Frauenstimme 2)
- `pl-PL-Standard-E` - Polnisch (Polen) (Frauenstimme 3)
- `pt-BR-Standard-A` - Portugiesisch (Brasilien) (weibliche Stimme - wird verwendet, wenn die Systemsprache PT ist und keine Sprache angegeben wurde)
- `pt-PT-Standard-A` - Portugiesisch (Portugal) (weibliche Stimme)
- `pt-PT-Standard-B` - Portugiesisch (Portugal) (Männerstimme)
- `pt-PT-Standard-C` - Portugiesisch (Portugal) (Männerstimme 2)
- `pt-PT-Standard-D` - Portugiesisch (Portugal) (Frauenstimme 2)
- `ru-RU-Standard-A` - Russisch (Russland) (weibliche Stimme - wird verwendet, wenn die Systemsprache RU ist und keine Sprache angegeben wurde)
- `ru-RU-Standard-B` - Russisch (Russland) (Männerstimme)
- `ru-RU-Standard-C` - Russisch (Russland) (Frauenstimme 2)
- `ru-RU-Standard-D` - Russisch (Russland) (männliche 2-Stimme)
- `sk-SK-Standard-A` - Slowakisch (Slowakei) (weibliche Stimme)
- `es-ES-Standard-A` - Spanisch (Spanien) (Frauenstimme - wird verwendet, wenn die Systemsprache ES ist und keine Sprache angegeben wurde)
- `sv-SE-Standard-A` - Schwedisch (Schweden) (weibliche Stimme)
- `tr-TR-Standard-A` - Türkisch (Türkei) (weibliche Stimme)
- `tr-TR-Standard-B` - Türkisch (Türkei) (Männerstimme)
- `tr-TR-Standard-C` - Türkisch (Türkei) (Frauenstimme 2)
- `tr-TR-Standard-D` - Türkisch (Türkei) (weibliche 3-Stimme)
- `tr-TR-Standard-E` - Türkisch (Türkei) (Männerstimme)
- `uk-UA-Standard-A` - Ukrainisch (Ukraine) (weibliche Stimme)
- `vi-VN-Standard-A` - Vietnamesisch (Vietnam) (weibliche Stimme)
- `vi-VN-Standard-B` - Vietnamesisch (Vietnam) (Männerstimme)
- `vi-VN-Standard-C` - Vietnamesisch (Vietnam) (Frauenstimme 2)
- `vi-VN-Standard-D` - Vietnamesisch (Vietnam) (Männerstimme 2)

MACHEN:

- Veranstaltungsort

## Auto-Inline-Tastatur basierend auf Einstellungen im Admin (Easy-Keyboard)
Für jeden Zustand können die zusätzlichen Einstellungen aktiviert werden:

![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings.png)

Durch die Eingabe von `/cmds` wird folgende Tastatur im Telegramm angezeigt:

![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings1.png)

`/cmds` kann im Konfigurationsdialog des Telegrammadapters durch einen beliebigen Text (z.B. "?") ersetzt werden.

Wenn im Konfigurationsdialog des Telegrammadapters die Option **Räume im Tastaturbefehl verwenden** aktiviert ist, wird im ersten Schritt die Raumliste angezeigt. ***Noch nicht implementiert***

### Einstellungen im Zustand
Zuerst muss die Konfiguration aktiviert werden.

#### Alias
Name des Geräts. Wenn der Name leer ist, wird der Name vom Objekt übernommen.
Durch Eingabe von "Türlampe" wird das folgende Menü für den booleschen Zustand angezeigt.
![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings2.png)

Sie können das Gerät einschalten, das Gerät ausschalten oder den Zustand abfragen.
Wenn Sie auf `Door lamp ?` klicken, erhalten Sie `Door lamp  => switched off`.

### Schreibgeschützt
Wenn aktiviert, werden die EIN/AUS-Schaltflächen nicht angezeigt, sondern nur ein `Door lamp ?`.

### Änderungen melden
Wenn sich der Status des Geräts geändert hat (z. B. jemand hat die Lampe physisch eingeschaltet), wird der neue Status an das Telegramm übermittelt.
Z.B. `Door lamp  => switched on`.

### Schaltflächen in Reihe
Wie viele Tasten müssen in der Zeile für ein Gerät angezeigt werden.
Wegen des langen Namens ist es vielleicht besser, nur 2 (oder sogar nur einen) Button in der Zeile anzuzeigen.

![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings3.png)

### Nur schreiben
Falls aktiviert, wird die Schaltfläche Statusabfrage (`Door lamp ?`) nicht angezeigt.
![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings4.png)

### Auf Befehl
Welcher Text wird auf der Schaltfläche `ON` angezeigt.
Wie hier: ![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings5.png)

Erzeugt folgende Tastatur: ![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings6.png)

### EIN Text
Der Text, der im Zustandsbericht angezeigt wird.
Z.B. `Door lamp => activated` wenn der Status des Geräts auf wahr geändert wurde und der **ON-Text** `activated` ist

Die EIN/AUS-Texte werden nur angezeigt, wenn **Änderungen melden** aktiviert ist.

### AUS-Befehl
Dasselbe wie **EIN-Befehl**, aber für AUS.

### AUS-Text
Dasselbe wie **ON Text**, aber für OFF.
Z.B. `Door lamp => deactivated` wenn sich der Status des Geräts auf „false“ geändert hat und der **OFF-Text** `deactivated` ist

### Nur wahr
Z.B. bei Tasten haben sie keinen AUS-Zustand. In diesem Fall wird die Schaltfläche AUS nicht angezeigt.

![die Einstellungen](../../../en/adapterref/iobroker.telegram/img/stateSettings7.png)

## So empfangen Sie Nachrichten in Gruppenchats mit dem Telegrammadapter
Wenn der Telegramm-Bot Nachrichten empfängt, die vom Benutzer an den Bot in privaten Chats gesendet werden, aber keine Nachrichten empfängt, die von Benutzern in Gruppenchats gesendet werden.
In diesem Fall müssen Sie mit `@botfather` sprechen und den Datenschutzmodus deaktivieren.

BotFather-Chat:

```
You: /setprivacy

BotFather: Choose a bot to change group messages settings.

You: @your_name_bot

BotFather: 'Enable' - your bot will only receive messages that either start with the '/' symbol or mention the bot by username.

'Disable' - your bot will receive all messages that people send to groups.

Current status is: ENABLED

You: Disable

BotFather: Success! The new status is: DISABLED. /help
```

## So senden Sie Nachrichten über node-red
Für einfache Textnachrichten an alle Benutzer fügen Sie den Text einfach in die Nutzlast der Nachricht ein und senden Sie ihn an den ioBroker-Status `telegram.INSTANCE.communicate.response`.

Wenn Sie zusätzliche Optionen festlegen möchten, füllen Sie die Payload mit einem JSON-Objekt, z. B.:

```
msg.payload = {
    // text is the only mandatory field here
    "text": "*bold _italic bold ~italic bold strikethrough~ __underline italic bold___ bold*",
    // optional chatId or user, the recipient of the message
    "chatId": "1234567890",
    // optional settings from the telegram bots API
    "parse_mode": "MarkdownV2"
}
```

Vor dem Senden an `telegram.INSTANCE.communicate.responseJson you need to stringify the object!`

<!-- Platzhalter für die nächste Version (am Zeilenanfang):

### __LAUFENDE ARBEIT__ -->

## Changelog
### 1.12.0 (2022-03-21)
* (Apollon77) Add new JSON states communication.responseJson and communication.responseSilentJson to also accept json structures (stringified!) to send messages
* (Apollon77) Try to prevent adapter crashes when internet is not available 
* (Apollon77) Add Sentry for crash reporting

### 1.11.1 (2022-01-27)
* (bluefox) fixed the receiving files

### 1.11.0 (2022-01-26)
* (bluefox) Added bruteforce protection
* (bluefox) Extended blockly with `disable_web_preview` option
* (bluefox) added `communicate.responseSilent` state to answer silently

### 1.10.1 (2022-01-26)
* (bluefox) Updated telegram library

### 1.10.0 (2021-07-30)
* (PeterVoronov) Add botSendRaw state to allow processing of the RAW data send by bot
* (Apollon77) Add tier for js-controller 3.3
* (bluefox) Fixed the control of the states

### 1.9.0 (2021-06-26)
* (bluefox) Added the option to not authenticate the new users
* (bluefox) Added the option to disable system messages for specific users

### 1.8.3 (2021-06-26)
* (Nahasapeemapetilon) corrected bug with many simultaneous requests 
* (bluefox) formatting
* (bluefox) implemented editMessageMedia and editMessageCaption
* (bluefox) Encrypt token 
* (bluefox) Corrected error with password
* (bluefox) Corrected error with boolean easy controls

### 1.8.2 (2021-05-28)
* (Diginix) fixed data types

### 1.8.1 (2021-04-20)
* (bluefox) added the admin5 support

### 1.8.0 (2021-02-22)
* (Apollon77/Nahasapeemapetilon) catch several API error cases to hopefully get around  adapter crashes on network errors
* (Nahasapeemapetilon) add support for media groups and multiple image qualities

### 1.7.0 (2021-01-08)
* (bluefox) Support of new Let's Encrypt (only with js-controller 3.2.x)

### 1.6.2 (2020-12-27)
* (fincha) Fixing error with keyboard

### 1.6.1 (2020-12-01)
* (ChristianB86) Added option to set the amount of repeats for telegram call.

### 1.6.0 (2020-11-09)
* (MarkRohrbacher) Allow overriding chatId / user when writing JSON objects to telegram.INSTANCE.communicate.response
* (blazeis) Fix Send message via Response field with Username
* (Garfonso) fill requestRaw also for callbackQuery

### 1.5.9 (2020-05-04)
* (Apollon77) potential error fixed when sending messages
* (Apollon77) webserver initialization optimized again to prevent errors with invalid certificates

### 1.5.8 (2020-04-30)
* (Apollon77) errors on webserver initialization are handled properly

### 1.5.6 (2020-04-04)
* (bluefox) Fixed missing languages for blockly
* (bluefox) Added description of easy-keyboard

### 1.5.5 (2020-04-04)
* (alutov) Fixed bug for telegram users with an empty username
* (Mark Rohrbacher) Allowed JSON objects in telegram.*.communicate.response

### 1.5.4 (2020-03-11)
* (bluefox) Improvement of `callmebot`

### 1.5.3 (2020-02-23)
* (foxriver76) removed usage of adapter.objects
* (Haba) Fix of the response for the "callback_query" event

### 1.5.1 (2020-02-09)
* (bluefox) Invalid parameters were checked

### 1.5.0 (2020-02-03)
* (bluefox) Added voice calls

### 1.4.7 (2019-12-27)
* (Apollon77) Make compatible with js-controller 2.3

### 1.4.6 (2019-12-09)
* (bluefox) Allowed writeOnly states in telegram

### 1.4.4 (2019-11-27)
* (bluefox) New sendTo message "ask" was added (see [Question](#question) )

### 1.4.3 (2019-02-21)
* (BuZZy1337) Bugfix for not yet completely implemented feature

### 1.4.2 (2019-02-18)
* (BuZZy1337) fix for recipients containing spaces
* (BuZZy1337) change loglevel of "getMe" info-messages to debug
* (bluefox) fix scroll in firefox

### 1.4.1 (2019-01-12)
* (simatec) Support for Compact mode

### 1.4.0 (2019-01-06)
* (bluefox) Custom settings for states were added

### 1.3.6 (2018-12-01)
* (Apollon77) fix #78

### 1.3.5 (2018-11-04)
* (BuZZy1337) Fix a small error caused by previous commit

### 1.3.4 (2018-11-04)
* (BuZZy1337) Ask if saved users should be wiped when password is changed.

### 1.3.3 (2018-11-03)
* (BuZZy1337) Show warning if no password is set.

### 1.3.2 (2018-10-28)
* (BuZZy1337) Just minor cosmetic fixes/changes

### 1.3.1 (2018-10-08)
* (bluefox) The ability of enable/disable of states controlling was added

### 1.3.0 (2018-09-19)
* (BuZZy1337) Added possibility to delete authenticated users in the Adapter-Config screen (via Messages tab)
* (BuZZy1337) fixed a problem "building" the Blockly `sendto` block when no adapter instance exists.

### 1.2.7 (2018-08-29)
* (BuZZy1337) Added "disable notification" checkbox to blockly block.
* (BuZZy1337) Added "parse_mode" selector to blockly block.

### 1.2.6 (2018-07-30)
* (BuZZy1337) Added support for sending Messages to Group-Chats via Blockly.

### 1.2.5 (2018-07-11)
* (BuZZy1337) Added possibility to specify more than one recipient. (separated by comma)

### 1.2.4 (2018-06-02)
* (BuZZy1337) remove HTML Tags from Logerror-Messages
* (Apollon77) fix misleading error when setting a value for a state

### 1.2.3 (2018-04-26)
* (Osrx) Added Socks5 settings to config dialog on machines running admin 2.

### 1.2.2 (2018-04-25)
* (kirovilya) Changed library for Proxy Socks5

### 1.2.1 (2018-04-17)
* (Haba) Added support for Proxy Socks5.

### 1.2.0 (2018-03-21)
* (AlGu) Possibility to define polling interval in configuration wizard. Default is 300ms.

### 1.1.4 (2018-03-20)
* (BasGo) Added checks before accessing non-existing options

### 1.1.3 (2018-03-19)
* (BasGo) Fixed issue preventing adapter to terminate correctly
* (BasGo) Fixed issue with wrong callback query id

### 1.1.2 (2018-03-16)
* (BasGo) Reworked configuration and translation

### 1.1.1 (2018-01-26)
* (Haba) New objects: botSendChatId, botSendMessageId

### 1.1.0 (2018-01-24)
* (bluefox) Possibility to send photo, video, document, audio as buffer.

### 1.0.11 (2018-01-23)
* (Haba) Sending an image without intermediate caching

### 1.0.10 (2018-01-18)
* (Haba) Updating for Admin3

### 1.0.9 (2017-11-27)
* (kirovilya) Allow the sending of GIF via sendDocument

### 1.0.8 (2017-10-03)
* (Haba1234) initPolling() this is deprecated. -> startPolling()
* (Haba1234) Add log polling_error and webhook_error.

### 1.0.7 (2017-09-27)
* (Haba) New function: deleteMessage. Update version lib node-telegram-bot-api

### 1.0.6 (2017-07-19)
* (Haba) Fix an incorrect order of writing variables

### 1.0.5 (2017-07-18)
* (Haba) inline keyboard and new functions: answerCallbackQuery, editMessageText, editMessageReplyMarkup

### 1.0.4 (2017-06-22)
* (dwm) Fix longitude and latitude

### 1.0.3 (2017-05-24)
* (bluefox) Fix position message

### 1.0.2 (2017-01-13)
* (bluefox) show only installed instances in blockly

### 1.0.1 (2016-11-04)
* (bluefox) Show user name in error message

### 1.0.0 (2016-10-31)
* (bluefox) server mode with web hooks

### 0.4.4 (2016-10-12)
* (bluefox) support of blockly

### 0.4.3 (2016-08-28)
* (bluefox) filter out double messages

### 0.4.2 (2016-08-22)
* (bluefox) translations
* (bluefox) configurable restarting/started texts

### 0.4.1 (2016-07-29)
* (bluefox) response to chatId and not to userId
* (bluefox) cut messages with @
* (bluefox) add new states: requestChatId and requestUserId

### 0.4.0 (2016-07-21)
* (bluefox) allow sending of messages to chats via chat-ID
* (bluefox) support of video(mp4), audio, document, location, sticker, action

### 0.3.0 (2016-05-31)
* (bluefox) restart connection every hour

### 0.2.4 (2016-05-08)
* (bluefox) replace "_" with " " when sending to text2command

### 0.2.3 (2016-05-04)
* (bluefox) replace "/" with "#" when sending to text2command

### 0.2.2 (2016-04-14)
* (Jonas) fix unload

### 0.2.1 (2016-04-13)
* (Jonas) fix configuration and send to more than one user

### 0.2.0 (2016-04-12)
* (bluefox) add send photo possibility

### 0.1.0 (2016-02-20)
* (bluefox) fix double responses.
* (bluefox) inform about new start

### 0.0.2 (2016-02-15)
* (bluefox) fix error with sendTo

### 0.0.1 (2016-02-13)
* (bluefox) initial commit

## License

The MIT License (MIT)

Copyright (c) 2016-2022, bluefox <dogafox@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.