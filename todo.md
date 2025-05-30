# 🛠️ Development Roadmap for "Jazz Improvisation Randomizer"

## 1. 🎨 **UI Design (Swing)**

### 1.1 Main Window

* [ ] Set up the main application window (`JFrame`)
* [ ] Use `JTabbedPane` or `CardLayout` to separate features (chord progressions, note sequences, favorites)

### 1.2 Components for ii-V-I Generator

* [ ] Dropdown for selecting root key
* [ ] Button: “Generate ii-V-I”
* [ ] Display area (`JTextArea` or `JLabel`) for generated progression

### 1.3 Components for Motif Generator

* [ ] Dropdown for selecting scale/mode (Dorian, Mixolydian, etc.)
* [ ] Button: “Generate Motif”
* [ ] Display area for notes
* [ ] Optional: simple visual piano or fretboard display

### 1.4 Playback Controls

* [ ] Play/Stop buttons
* [ ] Volume and tempo sliders (optional)

### 1.5 Favorites Section

* [ ] List view to show saved sequences
* [ ] Buttons: “Save to Favorites,” “Remove,” “Export”

### 1.6 Menu Bar (Optional)

* [ ] File (Export favorites)
* [ ] Help (Show app info or instructions)

---

## 2. 🎼 **Core Logic**

### 2.1 ii-V-I Progression Generator

* [ ] Define all 12 keys and their ii-V-I chords
* [ ] Randomly select a key and build correct ii-V-I
* [ ] Display result in jazz notation (e.g., Dm7 – G7 – Cmaj7)

### 2.2 Motif Generator

* [ ] Define common jazz scales/modes (Major, Dorian, Mixolydian, etc.)
* [ ] Generate random sequences of notes (e.g., 4–8 notes long)
* [ ] Option to limit notes to a specific range (e.g., one octave)

### 2.3 Formatting

* [ ] Use proper notation (e.g., ♭ and ♯ symbols)
* [ ] Ensure enharmonic correctness (e.g., no E# in C major)

---

### 2.4 🧠 **Motif Generation Algorithm**

**Goal:** Generate realistic, musically useful motifs (short melodic phrases) within a selected jazz scale or mode, optionally with jazz-like phrasing or directionality (e.g., ascending/descending).

#### Step 1: Define Musical Constraints

* [ ] Decide motif length (number of notes, e.g., 4–8)
* [ ] Determine the pitch range (e.g., one octave from root, or MIDI note numbers 60–72)
* [ ] Support directionality (e.g., random, ascending, descending, arpeggiated)

#### Step 2: Define Scales and Modes

* [ ] Create data structures for each scale/mode:

  * Major (Ionian)
  * Dorian
  * Phrygian
  * Lydian
  * Mixolydian
  * Aeolian (Natural Minor)
  * Locrian
* [ ] Each mode should be a list of scale degrees, e.g., for C Dorian:

  ```java
  List<Integer> scaleDegrees = Arrays.asList(0, 2, 3, 5, 7, 9, 10); // Semitone offsets
  ```

#### Step 3: Motif Construction Logic

* [ ] Choose a root note (from user input or random)
* [ ] Use the selected scale degrees to build a pool of valid notes (e.g., MIDI notes or note names)
* [ ] Apply one of the following construction methods:

##### 🎲 Method A: Random Walk

* Start from a note in the scale
* Move up/down a random interval within the scale (1–3 scale steps)
* Add note to motif list
* Repeat for `N` notes

##### 📈 Method B: Directional Motif

* Choose a random direction: ascending or descending
* Walk through the scale with small intervals in the chosen direction
* Possibly reverse direction mid-way for variation

##### 🎹 Method C: Interval Patterning

* Predefine interval patterns (e.g., +3, -2, +4) and apply to notes in the scale
* Ensures motifs have a coherent shape or "hook"

#### Step 4: Rhythm (Optional – Advanced)

* [ ] Assign durations (quarter, eighth, dotted notes) to notes
* [ ] Implement swing feel by delaying off-beats slightly
* [ ] For playback: convert rhythm into MIDI timing

#### Step 5: Output & Formatting

* [ ] Convert MIDI notes to standard note names (e.g., “E♭4”)
* [ ] Display in readable format (text or optional visual staff later)
* [ ] Store motif as `List<String>` or `List<MidiNote>` for playback

---

### ✅ Example Output

For C Dorian:

* `D4 – F4 – G4 – A4 – G4 – F4`
  For G Mixolydian:
* `G4 – A4 – C5 – B4 – A4`

### ✨ Optional Enhancements

* [ ] Add a “melodic contour” selector: smooth, jumpy, zig-zag
* [ ] Support key modulation within a motif
* [ ] Style selector: bebop, modal, bluesy

---

## 3. 🎹 **MIDI Playback Integration**

### 3.1 Basic Setup

* [ ] Set up `javax.sound.midi` sequencer and synthesizer
* [ ] Load standard instrument (e.g., acoustic piano)

### 3.2 Chord Playback

* [ ] Map chord symbols to MIDI note groups
* [ ] Play all notes of each chord simultaneously

### 3.3 Motif Playback

* [ ] Convert note sequences to MIDI note events
* [ ] Add basic timing/duration (e.g., quarter notes)

### 3.4 Playback Controls

* [ ] Start, stop, and tempo adjustment
* [ ] Handle playback in a separate thread to avoid freezing UI

---

## 4. 💾 **Saving & Favorites**

### 4.1 Internal Data Model

* [ ] Create classes for `ChordProgression` and `NoteSequence`
* [ ] Store list of favorites in memory

### 4.2 Saving Favorites

* [ ] Add current progression/motif to favorites list
* [ ] Display favorites in list/table

### 4.3 File I/O

* [ ] Serialize favorites to a text or JSON file
* [ ] Load favorites on app startup
* [ ] Allow export of selected favorites to a text file

---

## 5. 🧪 **Testing & Debugging**

### 5.1 Unit Testing

* [ ] Test chord generation logic
* [ ] Test motif generation for correct scale tones
* [ ] Test MIDI note mappings

### 5.2 UI Testing

* [ ] Ensure buttons trigger correct actions
* [ ] Validate dropdown selections and edge cases

### 5.3 Playback Testing

* [ ] Ensure MIDI playback doesn’t block UI
* [ ] Test timing and note accuracy

### 5.4 Persistence Testing

* [ ] Verify favorite saving/loading works correctly

### 5.5 🧩 **Optional: Extras & Polish**

* [ ] Add icons or visual feedback (e.g., simple keyboard animation)
* [ ] Dark mode or theme support
* [ ] Display jazz theory tips (e.g., “Try resolving V7 to I!”)

---

## ✅ Suggested Class Structure

```java
class JazzImprovisationRandomizer { ... } // main app
class ChordProgression { String key; List<String> chords; ... }
class NoteSequence { String scale; List<String> notes; ... }
class MidiPlayer { void playChord(...); void playSequence(...); }
class FavoritesManager { List<ChordProgression> progressions; ... }
```

---

## 6. 📚 **Library and API Selection (No External Libraries)**

### A. 🎨 User Interface (Java Swing)

**Core Classes to Use:**

* `JFrame`, `JPanel`, `Box`, `GridBagLayout`
* `JButton`, `JComboBox`, `JTextArea`, `JList`, `JLabel`
* `JTabbedPane`, `JMenuBar`, `JMenu`, `JMenuItem`
* `JFileChooser`

**Event Handling:**

* `ActionListener`, `ItemListener`, `KeyListener` (optional)

---

### B. 🎵 MIDI Playback (`javax.sound.midi`)

**Core Classes to Use:**

* `Synthesizer`, `MidiChannel[]`
* `Sequencer`, `Sequence`, `Track`
* `ShortMessage`, `MidiEvent`

---

### C. 💾 Saving and Loading Favorites (`java.io` & `java.util`)

**Core Classes to Use:**

* `File`, `FileWriter`, `FileReader`
* `BufferedReader`, `BufferedWriter`, `PrintWriter`, `Scanner`
* `ArrayList`, `HashMap`, `Collections`

---

### D. ⏱ Timing and Threading

**Core Classes:**

* `javax.swing.Timer`
* `Thread`, `Runnable`, `SwingUtilities.invokeLater()`

---

### E. 🧠 Randomization and Utility Functions

* `Random`, `Math`

---

### F. 🧪 Debugging and Testing

* `System.out.println()`, `assert`
* Optional: Write custom test utilities


---

## ✅ Summary Table

| Purpose            | Java Package               | Key Classes                                  |
| ------------------ | -------------------------- | -------------------------------------------- |
| GUI                | `javax.swing`              | `JFrame`, `JPanel`, `JButton`, `JTabbedPane` |
| MIDI Playback      | `javax.sound.midi`         | `Synthesizer`, `MidiChannel`, `Sequencer`    |
| File I/O           | `java.io`                  | `File`, `FileWriter`, `BufferedReader`       |
| Data Storage/Logic | `java.util`                | `ArrayList`, `HashMap`, `Random`             |
| Threading & Timers | `java.lang`, `javax.swing` | `Thread`, `Timer`, `Runnable`                |
