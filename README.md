# Google Translate TTS - with extra prepending delay

This is a fork of Home Assistant's built-in Google Translate Text-to-Speech component as a custom component, with extra option for prepending delay to the audio stream.

Many people complain about network media players cutting down the first bits of the TTS audio because their devices need a little time to wake up from standby or idle state when they receive a network stream to play. The only good solution for this is to add a configurable amount of silence at the beginning of the audio stream. The proposed solution does this locally, and the generated files stay like that in the cache.

I submitted this as a PR originally, but it's not yet accepted.

Adding this as a custom component to your Home Assistant instance will override the internal component with the same name so you can still use it as before, with the extended functionality as below. You can install it also with HACS if you add it as a [custom repository like this](https://hacs.xyz/docs/faq/custom_repositories).

## Configuration

To enable text-to-speech with Google, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
tts:
  - platform: google_translate
    language: "de"
    delay: 1000
```
Configuration options:
```
language:
  description: "The default speech language to use."
  required: false
  type: string
  default: "`en`"

delay:
  description: "Prepend the generated speech with this amount of silence (miliseconds)."
  required: false
  type: int
  default: 0
```

Check the [complete list of supported languages](https://translate.google.com/intl/en_ALL/about/languages/) (languages where "Talk" feature is enabled in Google Translate) for allowed values.
Use the 2 digit language code which you can find at the end of URL when you click on Language name.

With the `delay` option you can add some silence to the beginning of the rendered speech, to cope with audio systems which need time to wake up their speakers when starting a network stream. Value is in miliseconds, maximum is 15000 (15s).

When `delay` is set, the audio stream is sent to the player in uncompressed `wav` format to preserve audio quality (avoid recompression). Without `delay` set, the stream is sent unmodified, in its original format.

