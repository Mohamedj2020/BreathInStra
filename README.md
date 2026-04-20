# Breath Instrument

A minimal browser-based instrument for live performance. The performer blows into the microphone, and breath pressure controls a continuous synth sound in real time.

## Run

Open `breath-instrument.html` in a modern browser.

The page loads Tone.js from a CDN, so the first load needs an internet connection. After the page opens:

1. Click `Start Audio`.
2. Allow microphone permission.
3. Blow into the microphone.
4. Use `Fullscreen` for performance mode.

## How It Works

- `Tone.UserMedia` asks for microphone access.
- `Tone.Analyser` reads live waveform samples from the mic.
- A `requestAnimationFrame` loop computes RMS volume from the waveform.
- Smoothed volume readings are compared against two thresholds:
  - `thresholdOn` starts the sound.
  - `thresholdOff` releases the sound.
- `isBlowing` prevents rapid retriggering while breath is active.
- Breath volume maps to synth frequency and synth volume.
- `Tone.Reverb` adds a subtle room effect.
- The center circle scales and glows with breath intensity.

## Test Checklist

Use this checklist before a performance:

1. Open `breath-instrument.html` in Chrome, Edge, or Firefox.
2. Click `Start Audio` and confirm the browser asks for microphone permission.
3. Allow permission and confirm the start panel fades away.
4. Stay silent and confirm no sound plays.
5. Blow gently and confirm a low, quiet tone starts.
6. Blow harder and confirm the pitch rises smoothly.
7. Stop blowing and confirm the sound fades out instead of clicking off.
8. Try normal room noise and confirm it does not trigger the sound.
9. Click `Fullscreen` and confirm the visual fills the screen.
10. Test with the actual performance microphone and room before the show.

## Tuning

The main controls are in the `config` object inside `breath-instrument.html`.

- Increase `thresholdOn` if background noise triggers the synth.
- Decrease `thresholdOn` if the performer has to blow too hard.
- Increase `thresholdOff` if the sound hangs after breath stops.
- Adjust `minFrequency` and `maxFrequency` to change the pitch range.
- Adjust `reverbWet` to make the reverb drier or wetter.

## Notes

Some browsers restrict microphone access from local files. If opening the file directly does not request microphone permission, serve the folder locally:

```powershell
python -m http.server 8000
```

Then open `http://localhost:8000/breath-instrument.html`.
