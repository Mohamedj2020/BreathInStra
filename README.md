# Breath Instrument

A minimal browser-based instrument for live performance. The performer blows into the microphone to play a continuous synth sound, then uses the mouse or trackpad position like three valves to choose the note.

## Run

Open `breath-instrument.html` in a modern browser.

The page loads Tone.js from a CDN, so the first load needs an internet connection. After the page opens:

1. Click `Start Audio`.
2. Allow microphone permission.
3. Blow into the microphone.
4. Move the cursor left, middle, or right to change notes.
5. Use `Fullscreen` for performance mode.

## How It Works

- `Tone.UserMedia` asks for microphone access.
- `Tone.Analyser` reads live waveform samples from the mic.
- A `requestAnimationFrame` loop computes RMS volume from the waveform.
- Smoothed volume readings are compared against two thresholds:
  - `thresholdOn` starts the sound.
  - `thresholdOff` releases the sound.
- `isBlowing` prevents rapid retriggering while breath is active.
- Breath volume maps to synth volume and visual intensity.
- Cursor position maps to one of three vertical valve zones:
  - left = low note
  - middle = mid note
  - right = high note
- `Tone.Reverb` adds a subtle room effect.
- The center circle scales and glows with breath intensity.

## Test Checklist

Use this checklist before a performance:

1. Open `breath-instrument.html` in Chrome, Edge, or Firefox.
2. Click `Start Audio` and confirm the browser asks for microphone permission.
3. Allow permission and confirm the start panel fades away.
4. Stay silent and confirm no sound plays.
5. Blow gently and confirm a quiet tone starts.
6. Keep blowing and move the cursor across the three zones to confirm the pitch changes smoothly.
7. Stop blowing and confirm the sound fades out instead of clicking off.
8. Try normal room noise and confirm it does not trigger the sound.
9. Click `Fullscreen` and confirm the visual fills the screen.
10. Test with the actual performance microphone and room before the show.

## Tuning

The main controls are in the `config` object inside `breath-instrument.html`.

- Increase `thresholdOn` if background noise triggers the synth.
- Decrease `thresholdOn` if the performer has to blow too hard.
- Increase `thresholdOff` if the sound hangs after breath stops.
- Increase `gateOnMs` if short room noises trigger notes.
- Increase `zoneHysteresisPx` if the note flickers near zone boundaries.
- Adjust the `notes` list to change the three pitch targets.
- Adjust `reverbWet` to make the reverb drier or wetter.

## Notes

Some browsers restrict microphone access from local files. If opening the file directly does not request microphone permission, serve the folder locally:

```powershell
python -m http.server 8000
```

Then open `http://localhost:8000/breath-instrument.html`.
