---
layout: post
title: "[EN] Playing with SpeechSynthesis"
date: 2017-09-20
categories: [English]
tags: [webdev, javascript, web speech api, SpeechSynthesis]
---
[SpeechSynthesis][MDN-SpeechSynthesis] is a recent specification that allows us to use [speech synthesis][Wiki-SpeechSynthesis] inside the browser.
It is [part][W3-SpeechAPI] of the general [Web Speech API][MDN-Speech], and while it is still just in draft status (since 2014), and not an official W3C standard, it is already [well supported][caniuse] in all major browsers, both desktop and mobile.

Using this API we can simply "speak" a text to the user in javascript. This may be useful for example to notify the user or build special apps like chats, assistant or language learning. To do so, we use the [`speak()` method][MDN-speak] of the `window.speechSynthesis` controller. In the `speak()` method we have to pass a special object of the class [`SpeechSynthesisUtterance`][MDN-SpeechSynthesisUtterance], where we specify the text to speak and we can set other options like the voice to use, the volume or the speed.

In its simplest form, we can create an utterance with just the text to speak:

```javascript
let utterance = new SpeechSynthesisUtterance('Hello World')
```

This way, the browser will use all default values (e.g.: an english voice with maximum volume and average speed and pitch).

```javascript
window.speechSynthesis.speak(utterance)
```

We can also `pause()`, `resume()` and `cancel()` the speech synthesis and get the  `paused`, `pending` and `speaking` properties.
In addition we can use the `getVoices()` method to get the list of available voices, in the form of [`SpeechSynthesisVoice`][MDN-SpeechSynthesisVoice] objects (more on this later).

As already said, in the utterance object we can set properties like:
* `pitch`: changes the pitch of the voice (range: `0` - `2`, default value: `1`)
* `rate`: the speed of the speech (range: `0.1` - `10`, default value: `1`)
* `volume`: the volume of the speech (range: `0` - `1`, default value: `1`)

Another important property that we can set is the language we want to speak in. To do so, we use the `lang` property, specifying the language with a [BCP 47 tag][IETF-BCP-47] (e.g. `'en-GB'`, `'pt-BR'`, `'it-IT'` and so on). If unset, the app's (i.e. the <html> lang value) lang will be used, or the user-agent default if that is unset too.

```javascript
utterance.lang = 'en-US'
```

Setting this parameter, the browser will automatically decide which voice to use within the ones available (if there is more than one voice for the selected language).

The last property we can set to further customize the speech synthesis is explicitly choose the voice we want. We can get a list of all available voices calling the method [`window.speechSynthesis.getVoices()`][MDN-getVoices]. This returns an array of [`SpeechSynthesisVoice`][MDN-SpeechSynthesisVoice] objects.

The (readonly) properties available for those objects are:
* `lang`: the [BCP 47 language tag][IETF-BCP-47]
* `name`: a name identifying the "speaker" (e.g. Alice, Bob, ...)
* `localService`: a boolean indicating if the voice is native to the local system or is a remote service on the web
* `voiceURI`: an URI for the voice
* `default`: a boolean telling if this is the default voice for the current context or not

**[TO BE CONTINUED]**

[MDN-SpeechSynthesis]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis
[Wiki-SpeechSynthesis]: https://en.wikipedia.org/wiki/Speech_synthesis
[MDN-Speech]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API
[W3-SpeechAPI]: https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html#tts-section
[caniuse]: http://caniuse.com/#feat=speech-synthesis
[MDN-speak]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis/speak
[MDN-SpeechSynthesisUtterance]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisUtterance
[MDN-SpeechSynthesisVoice]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisVoice
[IETF-BCP-47]: https://tools.ietf.org/html/bcp47
[MDN-getVoices]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis/getVoices
