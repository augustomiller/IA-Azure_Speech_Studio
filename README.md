<div align="center">
 <h1>IA Azure Speech Studio</h1>
</div>

 <p align="center">
  <a href="#Tecnologias">Tecnologias</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#Documentação">Documentação</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#Speech">Speech</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#License">License</a></p>

![_4de59358-f93c-43e9-92de-2803f650b842](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/da5ff3f2-2480-4737-bcc1-04497da7901f)

<p align="center">

</p>

## Tecnologias

- [Azure Speech Studio](https://speech.microsoft.com/portal)
- [Microsoft Copilot](https://copilot.microsoft.com/)
- [VS Code](https://code.visualstudio.com/)
- [Bash](https://www.gnu.org/software/bash/)
- [Git](https://git-scm.com/)

  ## Documentação

- [Azure Speech Studio Doc](https://learn.microsoft.com/pt-br/azure/ai-services/speech-service/)
- [Microsoft Copilot Doc](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [VS Code Doc](https://code.visualstudio.com/Docs)
- [Bash Doc](https://www.gnu.org/software/bash/manual/bash.html)
- [Git Doc](https://git-scm.com/doc)
- [Markdown Doc](https://google.github.io/styleguide/docguide/style.html)

</br>


## Principais Funcionalidades

### Recursos de fala por situação

![recursos](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/1b3a8bff-11e0-4ed2-8306-5c7b362e9f48)

### Conversão de fala em texto

![recurso2](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/1c7c70e9-a28f-4f87-8445-de38ef5b150e)

### Conversão de texto em fala

![recurso3](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/85f3f37d-cb6b-444f-b1fd-5864b2201545)


# Vamos lá 🚀

## Speech 

- 1 Acessar o portal do Azure Speech Studio: https://speech.microsoft.com/portal
- 2 Selecione o ícone de engrenagem "configurações" 
- 3 Selecione o seu recurso, caso não tenha é só criá-lo em "+ Criar novo recurso"
- 4 Após selecionar o recurso clique no botão "Usar recurso"
- 5 Voltamos para a página principal e selecionamos o recurso "Conversão de fala  em texto  em tempo real"

<div align="center">

![005](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/708cba99-afa5-4ab8-a20a-e2c3a225e62b)

</div>

- 6 Selecione o indioma do texto que vai ser convertido

![006](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/2ab96137-e9d9-4314-a0c3-8e34448b9e00)

- 7 Faça o upload do arquivo que será transcrito.

![007](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/87146fce-ebaa-4720-97bd-13e429a028e1)
![007.1](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/9bbf932f-a2a3-49c1-adbe-31a8ee23da15)


- 8 Assim que o upload é efetuado a transcrição já começa automaticamente.

![008](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/4d8c03ca-3af6-42a7-93f9-2de059eb317e)
![008.1](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/39c496e7-9e71-4122-bd7b-ed2d0c663a9b)

### JSON gerado

```json

Veja o arquivo: WhatAICanDo.json

```

- Gostou? conheça melhos os detalhes de implementação:

![009](https://github.com/augustomiller/IA-Azure_Speech_Studio/assets/990877/87065b40-2257-40ec-b6b2-51ede8bb9a46)


## Guia de início rápido: Reconhecer a fala através de um microfone utilizando o Serviço de Fala na linguagem Python 🐍

https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/python/from-microphone

## Exemplo de como fazer uma transcrição utilizando Python
<details>
  <summary>
    <h2>Expanda para ver o código 🚀</h2>
  </summary>

  ```python
#!/usr/bin/env python
# coding: utf-8

# Copyright (c) Microsoft. All rights reserved.
# Licensed under the MIT license. See LICENSE.md file in the project root for full license information.
"""
Conversation transcription samples for the Microsoft Cognitive Services Speech SDK
"""

import time
import uuid

from scipy.io import wavfile

try:
    import azure.cognitiveservices.speech as speechsdk
except ImportError:
    print("""
    Importing the Speech SDK for Python failed.
    Refer to
    https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-python for
    installation instructions.
    """)
    import sys
    sys.exit(1)

# Set up the subscription info for the Speech Service:
# Replace with your own subscription key and service region (e.g., "centralus").
# See the limitations in supported regions,
# https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-conversation-transcription
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"

# This sample uses a wavfile which is captured using a supported Speech SDK devices (8 channel, 16kHz, 16-bit PCM)
# See https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-microphone
conversationfilename = "YourConversationWavFile"


# This sample demonstrates how to use conversation transcription.
def conversation_transcription():
    """transcribes a conversation"""
    # Creates speech configuration with subscription information
    speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

    channels = 1
    bits_per_sample = 16
    samples_per_second = 16000

    # Create audio configuration using the push stream
    wave_format = speechsdk.audio.AudioStreamFormat(samples_per_second, bits_per_sample, channels)
    stream = speechsdk.audio.PushAudioInputStream(stream_format=wave_format)
    audio_config = speechsdk.audio.AudioConfig(stream=stream)

    transcriber = speechsdk.transcription.ConversationTranscriber(speech_config, audio_config)

    done = False

    def stop_cb(evt: speechsdk.SessionEventArgs):
        """callback that signals to stop continuous transcription upon receiving an event `evt`"""
        print('CLOSING {}'.format(evt))
        nonlocal done
        done = True

    # Subscribe to the events fired by the conversation transcriber
    transcriber.transcribed.connect(lambda evt: print('TRANSCRIBED: {}'.format(evt)))
    transcriber.session_started.connect(lambda evt: print('SESSION STARTED: {}'.format(evt)))
    transcriber.session_stopped.connect(lambda evt: print('SESSION STOPPED {}'.format(evt)))
    transcriber.canceled.connect(lambda evt: print('CANCELED {}'.format(evt)))
    # stop continuous transcription on either session stopped or canceled events
    transcriber.session_stopped.connect(stop_cb)
    transcriber.canceled.connect(stop_cb)

    transcriber.start_transcribing_async()

    # Read the whole wave files at once and stream it to sdk
    _, wav_data = wavfile.read(conversationfilename)
    stream.write(wav_data.tobytes())
    stream.close()
    while not done:
        time.sleep(.5)

    transcriber.stop_transcribing_async()


# This sample demonstrates how to use conversation transcription.
def conversation_transcription_from_microphone():
    """transcribes a conversation"""
    # Creates speech configuration with subscription information
    speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
    transcriber = speechsdk.transcription.ConversationTranscriber(speech_config)

    done = False

    def stop_cb(evt: speechsdk.SessionEventArgs):
        """callback that signals to stop continuous transcription upon receiving an event `evt`"""
        print('CLOSING {}'.format(evt))
        nonlocal done
        done = True

    # Subscribe to the events fired by the conversation transcriber
    transcriber.transcribed.connect(lambda evt: print('TRANSCRIBED: {}'.format(evt)))
    transcriber.session_started.connect(lambda evt: print('SESSION STARTED: {}'.format(evt)))
    transcriber.session_stopped.connect(lambda evt: print('SESSION STOPPED {}'.format(evt)))
    transcriber.canceled.connect(lambda evt: print('CANCELED {}'.format(evt)))
    # stop continuous transcription on either session stopped or canceled events
    transcriber.session_stopped.connect(stop_cb)
    transcriber.canceled.connect(stop_cb)

    transcriber.start_transcribing_async()

    while not done:
        # No real sample parallel work to do on this thread, so just wait for user to type stop.
        # Can't exit function or transcriber will go out of scope and be destroyed while running.
        print('type "stop" then enter when done')
        stop = input()
        if (stop.lower() == "stop"):
            print('Stopping async recognition.')
            transcriber.stop_transcribing_async()
            break
```


</details>


## License

<div align="center">
  
<p>This project is licensed under the MIT License. See the
  <a href="https://mit-license.org/">
    <img src="https://img.shields.io/static/v1?label=license&message=MIT&color=5965E0&labelColor=121214" alt="License"></a> file for details.</p>
<p>Made with&nbsp;💙 &nbsp;by Augusto Miller</p>
  
<div>
