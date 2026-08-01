# I recorded (309) 431-5222, ran a calibrated control call, and compared it against OP's clip. Here's what's actually in it.


**TL;DR:** u/Expensive_Pen_3217 and u/foxtrot7azv called it — the main sound is 2G GSM interference. It sits at **216.7 Hz**, exactly the GSM TDMA frame rate, and the harmonic structure proves it's a 1-of-8 timeslot pulse train. My own call and OP's posted clip are **the same audio file**, matched with zero drift, so the line plays a fixed recording. There's no binary and no encoded data — but there **is** a heartbeat, and it's a sound effect: four beats at 48.5 BPM with a lub-dub gap that is identical to the millisecond every time, which only happens if one sample was pasted repeatedly. But there **is** US dial tone at both ends of the call, on a path that physically cannot produce it — which is the tell that this is an assembled clip.

---

## How I recorded it

FaceTime on a Mac, audio routed through BlackHole 2ch, captured in Audacity at 44.1 kHz. Fully digital loopback — no microphone, no cable, no analog stage anywhere in my rig. That matters.

## Why I made a control call, and why you should care

Anyone can post a spectrogram. The problem is that **you can't tell your recording rig's artifacts from the source's artifacts** unless you first record something whose answer you already know.

So I called **(303) 499-7111** — NIST's WWV time service — through the exact same chain, changed nothing, and measured it. WWV broadcasts tones locked to atomic clocks, so it's a known reference.

**1. My chain is frequency-accurate.** WWV's 600 Hz standard tone came back at **599.9959 Hz** and **599.9983 Hz** over two separate windows — errors of −6.8 and −2.8 parts per million. So when I say "216.7 Hz," that number is good to about a thousandth of a hertz. No resampling error is shifting anything.

**2. My chain doesn't flatten pitch.** The WWV announcer's voice swung from 99 Hz to 320 Hz (5th–95th percentile), standard deviation 65.5 Hz. Normal speech, reproduced faithfully.

**3. My chain does not generate dial tone.** Across all 213 seconds of the control there is not one frame containing both 350 and 440 Hz. Zero. But ringback (440 + 480 Hz) *did* appear at t ≈ 21 s, exactly when WWV was ringing. So the chain passes real call-progress tones when they exist — it just never invents dial tone.

Hold onto that last one.

---

## Finding 1: my call and OP's clip are the same recording

I converted OP's posted mp4 to WAV and cross-correlated it against my own call.

Raw result: waveform |r| = **0.796**, envelope |r| = **0.722**, at an alignment offset of 3.605 s. For a baseline, an unrelated fake ARG clip against either of them scores 0.22 and 0.16.

But both files contain the same loud 216.7 Hz buzz, and a shared periodic component can inflate correlation on its own. So I notched the entire GSM harmonic stack out of all three files and ran it again:

| pair | waveform \|r\| | envelope \|r\| |
|---|---|---|
| **OP's clip vs my call** | **0.705** | **0.654** |
| OP's clip vs unrelated fake | 0.055 | 0.161 |
| my call vs unrelated fake | 0.051 | 0.228 |

With the buzz removed the unrelated pairs collapse to nothing and mine holds. The match is in the content underneath, not the interference.

Then I checked whether the alignment drifts, in 2-second windows:

| t in my call | best offset | local \|r\| |
|---|---|---|
| 0 s | 3.6050 s | 0.588 |
| 4 s | 3.6049 s | 0.691 |
| 8 s | 3.6049 s | 0.893 |
| 12 s | 3.6047 s | 0.796 |
| 16 s | 3.6047 s | 0.884 |

Constant across all nine windows — standard deviation 3.2 ms, total spread 10 ms over 18 seconds. **Zero drift.** Two separate captures of a live sound would wander apart; a stored file played back twice does exactly this.

So the number plays a fixed audio file, and my 20 seconds sits inside OP's 27-second clip starting at 3.605 s. (Side note: the thread title says calling gets you something different from the posted clip. My capture doesn't support that — it's the same recording.)

## Finding 2: the buzz is confirmed 2G GSM

The dominant sound has a fundamental at **216.72 Hz** in my recording and **216.758 Hz** in OP's, held to within 0.3 Hz across 20 straight seconds. Nothing organic holds a pitch that stable — that's a clock, not a throat.

GSM's TDMA frame is 60/13 ms = 4.61538 ms, giving a frame rate of **216.667 Hz**. Both measurements land within a few hundred ppm of that, well inside error for a burst signal. This is the "di-di-di-dit" you get when a 2G handset sits near unshielded audio gear — the phone slams its transmitter on for one timeslot in every eight, and that pulsing induces into the audio.

The clincher: a pulse train with 1/8 duty cycle has a sinc envelope, which puts **nulls at every 8th harmonic**. Measured off OP's clip, relative to the fundamental:

| harmonic | freq (Hz) | level (dB) |
|---|---|---|
| 7 | 1516.7 | −36.2 |
| **8** | **1733.3** | **−44.3** ← null |
| 9 | 1950.0 | −34.1 |
| 15 | 3250.0 | −54.2 |
| **16** | **3466.7** | **−68.2** ← null |

Harmonic 8 sits ~9 dB below both neighbours, harmonic 16 sits ~14 dB below. Exactly where one-timeslot-in-eight predicts. This is GSM, not ambiguity.

That buzz is about **75% of all energy** in my recording — notching the harmonic stack removes 6.1 dB of 12.8 dB total. Everything people hear as "whispers" is the broadband noise left underneath a very loud pulse train.

The anachronism point already raised in this thread stands. GSM was standardised around 1991. Peoria State Hospital (properly Bartonville State Hospital / Illinois Asylum for the Incurable Insane) had closure announced in 1972 and shut in 1973, and it's in Bartonville, not Peoria proper. Nothing about that building can produce a GSM artifact.

Also worth knowing: US 2G is basically gone. AT&T killed it January 1, 2017, and **T-Mobile's 2G GSM network shut down August 3, 2026**. So live GSM interference in the US now is vanishingly unlikely — this points to a sampled or archived buzz rather than something happening in real time.

## Finding 3: the dial tone shouldn't exist

Both ends of my recording contain **349.9 Hz and 441.4 Hz** — that's 350 + 440, standard North American precise dial tone. Before the content, and again after it.

FaceTime doesn't synthesise dial tone. macOS doesn't. And dialling out from FaceTime rides a cellular relay, and cell phones have no dial tone at all. My control call proves it directly: zero dial tone in 213 seconds, but ringback came through fine when the far end was actually ringing.

The suspect file is the exact inverse — **dial tone present, ringback absent, no dialled digits, no answer supervision.** The sequence runs: 1.3 s of dial tone → 0.2 s gap → content at full level. A real call cannot go in that order.

Simplest explanation that fits: the dial tone is **part of the audio file**, dressed onto both ends so the clip reads as a phone call.

## Finding 4: there is no hidden data

I checked properly, not by ear:

- **DTMF** — Goertzel scan across the whole file. Apparent "hits" were broadband energy tripping the detector; decoded digits are inconsistent frame to frame, which is what noise does.
- **FSK / modem / fax** — no stable two-tone handshake anywhere.
- **SSTV or spectrogram steganography** — an image drawn into the spectrum shows clean geometric edges. Nothing like that exists here.
- **Repeating loop** — envelope autocorrelation peaks nowhere above r = 0.25.
- **Cadenced busy/reorder or SIT tones** — none.

No binary, whispered or otherwise.

## Finding 5: the heartbeat is real, and it's edited in

I originally reported no heartbeat. **I was wrong, and the method was the reason** — I ran envelope autocorrelation across the whole 20 seconds, which averages away anything that only happens briefly. Someone pointed out the subtitle at the 11 s mark, so I redid it with 4-second sliding windows in a 30–160 Hz band:

| window | periodicity r | rate |
|---|---|---|
| 6–10 s | 0.020 | — |
| 8–12 s | 0.013 | — |
| **11–15 s** | **0.630** | **48.8 BPM** |
| 12–16 s | 0.607 | 48.8 BPM |
| 14–18 s | 0.020 | — |

Everywhere outside 9–17 s sits between 0.00 and 0.22. Inside it, 0.63. Autocorrelation over that span gives a 1.238 s cycle (48.5 BPM) with clean 2× and 3× multiples at r = 0.366 and 0.208 — a genuine repeating event.

Picking out the individual thumps, there are four beats, eight onsets:

```
10.864  11.159 | 12.136  12.430 | 13.355  13.650 | 14.591  14.886
intervals: 0.294  0.978  0.294  0.925  0.294  0.942  0.294
```

**Look at the short gap.** The lub-dub interval is 0.294 s every single time, across four beats, constant to the millisecond — while the beat-to-beat spacing wanders by about 5% (0.925, 0.942, 0.978). A real heart's S1–S2 interval varies beat to beat with physiology. One that's frozen to ±1 ms while the tempo drifts around it means **the same two-thump sample was pasted four times**. The thump energy sits around 81 Hz, which is where a heartbeat sound effect lives.

And it's in the source file, not an artifact of OP's mp4 — the identical eight onsets appear in my own independent FaceTime recording, mapping to 10.866 / 11.159 / 12.136 / 12.431 / 13.356 / 13.649 / 14.591 / 14.887. Agreement within 2 ms across the board.

So a heartbeat SFX was deliberately placed in there for roughly four seconds in the middle. Phone lines do not do this. It's production.

(For contrast, I ran the same test on a different ARG clip that's already confirmed fabricated, where you can plainly hear knocking: 0.620 s period, 96.8 BPM, with its own 2× and 3× ladder. Same class of edit, just less carefully hidden.)

The whispered ones and zeros are a different story — those I tested for properly and there's nothing there. Loud pulsing buzz over broadband noise is close to the ideal stimulus for phantom speech, and it gets stronger once you've been told what to listen for. But given I got the heartbeat wrong by using too coarse a method, take that with appropriate salt and check my work.

## Things that look suspicious but aren't

Being honest about my own corrections, because this is what the control was for:

- **The channels are bit-identical.** Looks like a flag. Isn't. BlackHole 2ch carries a mono stream on two channels.
- **The silence at head and tail is literal zeros, not a noise floor.** Also not a flag. On a digital loopback that's just what silence is.
- **There's a 168 ms run of exact zeros mid-content.** I originally called this a splice. **I was wrong.** The WWV control has interior dropouts too, including a 561 ms all-zero run — longer than the one I flagged — plus a 5.9 ms gap mid-call. That's the recording chain glitching, not an edit.

If you're going to analyse audio like this, run the control. Half of what I thought I'd found was my own equipment.

---

## What it adds up to

An ARG, and a decently made one. The number plays a stored audio file — proven, not inferred, by the zero-drift match against OP's clip. That file is built around a loud 2G GSM buzz, either sampled or captured off genuinely old hardware, with noise underneath and dial tone topped and tailed so it plays as a live call. Every element has a mundane explanation and nothing in the signal is unexplained.

u/Rare_agency101 reporting the call now drops instantly is consistent with the line being taken down or reassigned. These viral numbers usually end up landing on some real person's phone, so probably worth leaving alone at this point. Although no evidence has been found since I've been testing the number all day, but it will probably happen.

## Caveats

I can't tell you who made it, or whether the buzz was captured live versus sampled — the audio can't distinguish those. I have one recording of the number plus OP's clip; a third independent capture would be useful, because if the same file comes back again it's conclusively a stored playback.
