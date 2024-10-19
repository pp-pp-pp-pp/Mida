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

#### Braids, Trusses, and Ribbons

- **Braids**: Copy and modify sections, allowing unrelated parts to play in the same sequence.
  ```mida
  Audicle~Braid: *Hello!~3-*
  Braid; *Braid*~*Hello again!*
  ```

- **Trusses**: Multi-channel systems with predefined channels (N, n, Z, z) for parallel playback.
  ```mida
  Audicle~Truss: 
  N: *Hello*
  n: *Hello~Again!*
  Z: *C4*
  z: *E4*
  Audicle: *Truss~Z:*
  Audicle: *Truss~n:~N:*
  // The audicle truss is already a 4 channel group type object, but we can call parts of the truss to play individually as well in the following programs. Remember each audicle is a program even if it doesn't use the program keyword!
  // Trusses are kind of like snakes or balens, providing a powerful way to control multiple parallel channels while retaining the flexibility to address each channel independently.
  ```

- **Ribbons**: Highest-level structure, representing entire projects or sessions with multiple Programs.
  ```mida
  Audicle~Ribbon: *myRibbon!*
  Ribbon; *Program1; Program2*
  Program1; *Bass; Drums~DCA*
  Bass; *C1~16-*
  Drums; *CHH~0db~3-*
  Program2; **
  
  Audicle~Ribbon: *anotherRibbon!*
  Program1; *Bass; Drums~DCA*
  Bass; *C1~16-*
  Drums; *CHH~0db~3-*
  ``

#### Starried (Reverb Effect)

**Starried** is a special keyword in Mida that allows you to add reverb effects to your notation. It provides an intuitive way to represent reverb settings and apply them to notes directly within the text.

**Example of Starried Usage:**
```mida
Audicle~Starried: 
*Pre:40ms~RT60:5s~mix%:100*
*C4~7-A4~7-*
```

- **Pre**: Pre-delay time in milliseconds (`40ms` in this example).
- **RT60**: Reverb time, indicating how long it takes for reverb to decay by 60 dB (`5s` in this example).
- **mix%**: Mix percentage.

The `Starried` keyword adds an expressive layer to Mida, giving users the ability to incorporate reverb information in a clear and straightforward manner. Unlike traditional MIDI CC, which cannot easily represent complex effects, Mida's flexibility allows for detailed and human-readable control over these effects.

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
