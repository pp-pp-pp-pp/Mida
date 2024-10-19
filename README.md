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

**Automating Trimming**:
```mida
Audicle *Molly~Label*
Molly; *Trim*~r*Ribbon!!##1#123456@abcdef*
*Trim; Start:1s; End:6s*
*Hello~World!*
Molly; *Restore*
```
- **Explanation**: In this example, Molly automates the trimming process, trims an audio file, and then restores it after playback. The `Label` helps manage the trim duration, and `Restore` brings the Audicle back to its initial state.

Another manual example:
```mida
Audicle *Trim~Label*~r*Ribbon!!##1#123456@abcdef*
*Trim; Start:1s; End:6s*
*Hello~World!*
Label; *Undo-Undo*
```
- **Explanation**: This manually trims the file and, through `Undo`, reverts the clip to its original state after playback.

This is particularly useful for trimming actions that need to be reversible, just like a DAW's non-destructive editing capabilities.

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

## Best Practices

- **Consistent Naming**: Use consistent abbreviations and naming conventions for tracks and instruments.
- **Modular Design**: Utilize groups and busses to organize and manage complex mixes.
- **Leverage AI Features**: Experiment with Tree, Seaside, and Sunbird for generative and dynamic compositions.
- **Documentation**: Keep detailed notes on your Mida files for easier collaboration and debugging.

## Glossary

- **Audicle**: The main container for Mida streams, signifying the start of a Mida command.
- **Program**: A sequence of tracks that play one after another.
- **Matrix**: The mastering stage for sub-mixes before sending to the Program feed.
- **Bus**: Handles routing and automation of audio signals.
- **Group**: Organizes multiple tracks without affecting audio directly.
- **DCA**: Controls gain across multiple tracks with a fader.
- **Braid**: Copies and modifies audio data for reuse.
- **Truss**: Manages a four-channel system for parallel playback.
- **Ribbon**: Represents an entire project or session containing multiple Programs.
- **Tree, Seaside, Sunbird**: Advanced features for AI integration, focusing on generating, simplifying, and remixing music respectively.
- **Molly**: Helper function for non-audio tasks like trimming and state management.

## Contributing

We welcome contributions to Mida! Please follow these steps to contribute:

1. Fork the repository.
2. Create a new branch for your feature or bugfix.
3. Commit your changes with clear messages.
4. Submit a pull request with a detailed description of your changes.

## License

This project is licensed under the [MIT License](LICENSE).

---

For more detailed information and advanced usage, please refer to the [Mida Documentation](docs/README.md).
