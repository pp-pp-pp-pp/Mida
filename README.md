# Mida: A Flexible Music Notation Language

Welcome to **Mida**, an innovative and flexible plain text music notation language designed for deep learning use cases and seamless integration with generative AI. Mida simplifies music creation, transcription, and manipulation with its intuitive syntax and powerful features.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
  - [Basic Notation](#basic-notation)
  - [Chords](#chords)
  - [Lyrics](#lyrics)
  - [Percussion](#percussion)
  - [Multitracks](#multitracks)
  - [Advanced Features](#advanced-features)
  - [Automation](#automation)
  - [Molly and Trimming Clips](#molly-and-trimming-clips)
- [Hierarchy](#hierarchy)
  - [Programs](#programs)
  - [Matrixes](#matrixes)
  - [Busses](#busses)
  - [Groups and DCAs](#groups-and-dcas)
  - [Braids, Trusses, and Ribbons](#braids-trusses-and-ribbons)
- [AI Integration](#ai-integration)
  - [Tree](#tree)
  - [Seaside](#seaside)
  - [Sunbird](#sunbird)
- [Getting Started](#getting-started)
- [Examples](#examples)
  - [Simple Melody](#simple-melody)
  - [Chords and Guitar Tunings](#chords-and-guitar-tunings)
  - [Multitrack Setup](#multitrack-setup)
  - [Using Busses and Braids](#using-busses-and-braids)
  - [Parallel Compression](#parallel-compression)
- [Best Practices](#best-practices)
- [Glossary](#glossary)
- [Contributing](#contributing)
- [License](#license)

## Overview

**Mida** is a versatile plain text music notation language that allows musicians and developers to write, transcribe, and manipulate music in a streamlined and intuitive manner. Designed with deep learning and AI integration in mind, Mida supports a wide range of musical elements, including notes, chords, lyrics, percussion, and multitracks. Its flexible hierarchy and advanced features make it suitable for both simple compositions and complex, AI-driven music projects.

## Features

### Basic Notation

Mida uses simple symbols to represent musical elements, making it easy to read and write.

- **Notes**: Represented by their name and octave, e.g., `A4`, `Bb2`.
- **Rests and Sustains**:
  - `.`: 16th rest
  - `-`: 16th note sustain
- **Durations**: Extend note lengths using hyphens or the tilde (`~`) for conciseness.
  - `A#4---`: Quarter note A#4
  - `*A#4~3-*`: Quarter note A#4 using shorthand

**Example:**
```mida
Audicle: *A---B---C---D---E---F...G...*
```

### Chords

Chords are written by listing each individual note, allowing for flexibility without the need for chord names.

- **Single Chord**:
  ```mida
  Audicle: *C3~E3~G3~16-*
  ```
  Plays a C major chord on the third octave for 16 sixteenth notes.

- **Guitar Chords**:
  ```mida
  Audicle: *G;022000~16-*
  ```
  Represents an Em chord using guitar tab notation in standard tuning.

### Lyrics

Lyrics can be paired with melody lines or defined separately, allowing for synchronized or independent lyric placement.

- **Basic Lyrics**:
  ```mida
  Audicle: *Three---goes-to--one-which---goes-to--four----*
  ```

- **Lyrics with Notes**:
  ```mida
  Audicle: *Three~C4---Goes~E4---To~G4---*
  ```

### Percussion

Mida supports 47 percussion instruments, mapped to standard MIDI percussion notes with intuitive abbreviations.

**Percussion Table:**

| MIDI Note | Instrument         | Abbreviation |
|-----------|--------------------|--------------|
| 35        | Acoustic Bass Drum | ABD          |
| 36        | Bass Drum 1        | BD1          |
| 37        | Side Stick/Rimshot | SSR          |
| ...       | ...                | ...          |
| 81        | Open Triangle      | OTR          |

**Example Drum Beat:**
```mida
Audicle: *ABD . CHH . SSR . BD1~CHH . | CHH . BD1~SSR . CHH . CHH*
```

### Multitracks

Organize multiple tracks such as Drums, Bass, Vocals, and Others within a Program for complex compositions.

**Example Multitrack Setup:**
```mida
Audicle: *Program; Drums, Bass, Vocals, Others*
Drums; *CHH~7-CHH~7-CHH~7-CHH~7-*
Bass; *C1~31.*
Vocals; *Hello~C4~31.*
Others; *E3~G3~31-*

// Another section using Program
Audicle: *Program; Drums, Bass, Vocals, Others*
Drums; *BD1~7-SSR~7-CHH~7-CHH~7-*
Bass; *C1~31-B1~31-*
Vocals; *Goodbye~D4~31.*
Others; *E3~A3~31-*
```

### Advanced Features

#### Recording, Trimming, and Editing Clips

Mida allows users to record, trim, and edit clips directly through text commands, offering a fully featured recording experience similar to a traditional DAW. Each recording is treated as an environmental variable that can be interacted with in real time.

**Example of Recording and Braiding:**
```mida
Audicle: *Block*~*Braid*
Permissions; *Record*
Arm; *#123456* // Example channel address to arm, also arming a braid to print a debug statement
*C4*
*Braid*

// Output:
// mida stream:
// C4
// C4
```
- **Explanation**: In this example, the braid is recording itself and anything that passes through it. The braid is essentially copying the musical data captured during the recording process.
- If the recording step is omitted, the braid behaves differently:
  ```mida
  Audicle: *C4*~*Braid*
  *Braid*

  // Output:
  // mida stream:
  // C4~Braid
  // C4 // (Braided)
  ```
  Without explicit recording, the braid still copies the note but does not capture it in the same way as during the recording process.
- **Environmental Recording**: Recording is treated as an environmental state—once armed, any data that passes through is captured, providing flexibility similar to a looping station or overdub setup in a DAW.

This unique approach allows for a conceptual separation between capturing the "potential" for audio and ensuring the actual recorded data is interactable and can be played or manipulated further.

#### Effect Chains and Routing

Mida allows users to create and chain multiple effects, similar to plugin chains in traditional DAWs. You can apply EQ, compression, reverb, and more to any track, giving full flexibility in sound shaping.

**Example of Using EQ and Compression Together:**
```mida
Audicle: *EQ~Comp*
*C#3- - -*
EQ; *Default* // *Bands:{Low,Mid,High} Low; Type: Lowpass; Q: 0.5; Hz: 100; Bypassed: False; Gain: 1db; Mid; Type: Bandpass; Q: 0.5; Hz: 1000; Bypassed: False; Gain: 1db; High; Type: Highpass; Q: 0.5; Hz: 10,000, Bypassed: False; Gain: 1db*
Comp; *Default* // *Ratio:4x~Thresh:Unity~Atk:0.10ms~Rel:0.10*
*C#3- - -*
```
- **Explanation**: This example plays a note (`C#3`), sets up an EQ and compressor, and then plays the note again. The second time the note is played, it has the effects applied. The EQ is configured with multiple bands (Low, Mid, High), and the compression is set with default parameters.
- **Automatable**: Each parameter of the EQ or compressor can be automated, making it possible to shape the sound dynamically over time.

### Molly and Trimming Clips

**Molly** is a versatile helper in Mida used to facilitate non-audio actions like trimming files or acting as an agent at the Audicle level. Molly can automate otherwise cumbersome operations, such as automatically restoring a state or handling specific non-musical tasks.

**Example of Trimming with Molly**:
```mida
Audicle *Molly*
Molly; *Trim*~r*Ribbon!!##1#123456@abcdef*
*Trim; Start:1s; End:6s*
Molly; *Restore*
```
In this example, Molly is used to manage trimming on a specific audio file, identified by its ribbon, channel, and file address. Molly helps manage actions like trimming without affecting the actual sound, simplifying the process.

If you need to trim without using Molly, you can still achieve it manually:
```mida
Audicle *Trim*~r*Ribbon!!##1#123456@abcdef*
*Trim; Start:1s; End:6s*
Label; *Undo*
```
Molly's usefulness is more apparent when more advanced automation is involved.

### Automation

Mida supports advanced automation that allows for dynamic control of various musical parameters over time. The ability to automate effects like gain or reverb provides Mida users with expressive tools akin to those found in modern DAWs.

**Example of Automation Usage:**
```mida
Audicle: *Program*
Program; *Bass~DCA~Starried~Bus~Label*
Label; *Hello~*
Starried; *Default*
DCA; *Unity*
*C1~3-* 
// In this example, the program would print 'Hello' to the label channel if no output is explicitly given, but here we add a C1 note. The reverb effect set by Starried gets automated below.

Audicle: *Program*
Program; *Bus~Label*
Starried; *RT60:20s*
Label; *5s*
DCA; *-5db*
*C1~3-* 
// The DCA fader and Starried RT60 time are being automated here. The 'Label' determines the transition time for these changes.

Audicle~Starried; *Default~Label*
Starried; *- - - - - - RT60; 3s*
Label; *5s*
*C2~15-*
// In this example, the Starried audicle starts with default parameters and has a label attached. The RT60 parameter is changed after 6 sixteenth notes, with a transition time handled by 'Label', which is set to 5 seconds.
```
- **Detailed Control**: Automation in Mida is highly customizable. You can specify when an effect parameter should change, how long the transition should take, and attach labels to provide a clear and human-readable sequence for these changes.

## Parallel Compression

Mida also allows for parallel compression, a popular mixing technique where a heavily compressed version of a track is mixed with the original signal for added punch and sustain.

**Example of Parallel Compression:**
```mida
Audicle: *Drums~Comp*
Comp; *Ratio:8x~Thresh:-10db~Atk:0.5ms~Rel:50ms~Mix:50%* // Heavy parallel compression settings
*BD1---BD1---BD1---BD1---* // Four quarter notes
```
This example shows how to apply heavy compression settings to drums and mix them with the original signal (using `Mix:50%`), creating a parallel compression effect that adds power without sacrificing dynamics.

For a more traditional parallel compression setup using matrix channels:
```mida
Audicle: *Program; Matrix*
Matrix; *BD1---BD1---BD1---BD1---*
Comp; *Ratio:8x~Thresh:-10db~Atk:0.5ms~Rel:50ms~Mix:50%* // Heavy parallel compression settings
Matrix; *BD1---BD1---BD1---BD1---*
```
Here, the matrix allows both the dry and compressed versions of the track to be heard simultaneously, creating a true parallel compression effect.

## Hierarchy

### Programs

Programs are sequences that play one after another within a Ribbon. Each Program contains multiple tracks.

### Matrixes

Matrixes act as the mastering stage, managing sub-mixes before sending to the Program feed.

### Busses

Busses handle flexible routing and allow for automation of parameters. They can route signals to multiple destinations without taking exclusive control.

### Groups and DCAs

- **Groups**: Organize multiple tracks without having a fader, useful for structural organization.
- **DCAs (Digitally Controlled Amplifiers)**: Control gain across multiple tracks with a fader, without processing the signal.

### Braids, Trusses, and Ribbons

- **Braids**: Copy and modify audio data for reuse across sections.
- **Trusses**: Manage four-channel systems for parallel playback.
- **Ribbons**: Represent entire projects or sessions, containing multiple Programs.

## AI Integration

### Tree

- **Purpose**: Generate new music from existing prompts.
- **Usage**:
  ```mida
  Audicle: *Tree*
  Tree; *Generate new music based on C4-G4 melody*
  ```

### Seaside

- **Purpose**: Simplify and calm music, creating ambient or minimalist versions.
- **Usage**:
  ```mida
  Audicle: *Seaside*
  Seaside; *Simplify C4-G4 melody into a calmer form*
  ```

### Sunbird

- **Purpose**: Subvert and remix existing music in real-time.
- **Usage**:
  ```mida
  Audicle: *Sunbird*
  Sunbird; *Remix C4-G4 melody with modern EDM influences*
  ```

### Real-Time Processing

Mida allows real-time AI-driven transformations, enabling dynamic and evolving compositions.

**Example:**
```mida
Audicle~Sunbird: *Remix the master output live*
Sunbird; *Apply live remix effects on output in real time*
```

## Getting Started

1. **Install Mida**: Follow the installation instructions [here](#).
2. **Basic Syntax**: Learn the basic notation with examples.
3. **Create Your First Program**: Start by writing a simple Program with Drums, Bass, and Vocals.
4. **Explore Advanced Features**: Dive into Matrixes, Busses, and AI integrations like Tree, Seaside, and Sunbird.

## Examples

### Simple Melody

```mida
Audicle: *A---B---C---D---E---F...G...*
```

### Chords and Guitar Tunings

```mida
Audicle~G;-2.0.0.0.0.0: 
*G;520000~16v*

Audicle~G;-2.110hz.0.0.0.0: 
*G;520000~16v*
```

### Multitrack Setup

```mida
Audicle: *Program; Drums, Bass, Vocals, Others*
Drums; *CHH~7-CHH~7-CHH~7-CHH~7-*
Bass; *C1~31.*
Vocals; *Hello~C4~31.*
Others; *E3~G3~31-*
```

### Using Busses and Braids

```mida
Audicle: *Program; Drums~Bus, Bass~Bus, Vocals~Bus, Others~*
Drums; *CHH~7-CHH~7-CHH~7-CHH~7-*
Bass; *C1~31.*
Vocals; *Hello~C4~31.*
Others; *E3~G3~31-*

Audicle: *Program; Drums, Bass, Vocals, Others*
Drums; *Bus*
Bass; *Bus*
Vocals; *Goodbye~3-*
Others; *Bus*
```





