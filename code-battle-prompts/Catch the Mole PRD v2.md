---
name: "Catch the Mole"
about: "Arcade-style 'Whack-a-Mole' browser game with score tracking, sounds, and animations"
date_created: "2024-03-15"
project_name: "MoleGame"
tech_stack: [
  "NextJS 14 (App Router)",
  "TypeScript",
  "Shadcn",
  "Tailwind CSS (optional)",
  "Howler.js",
  "framer-motion"
]
version: "1.0"
---

# 🎯 Catch the Mole – Arcade Whack-a-Mole Game

An arcade-style browser game where moles pop up randomly on the screen. Users must “whack” them quickly (via clicks or taps) to score points. The game features increasing difficulty, fun animations, and playful sound effects using **Howler.js** and **framer-motion**.

---

## 1. **Success Criteria**

### 1.1 **Core Functionality**
- [ ] **Mole Appearance**: Moles appear in random positions or “holes.”
- [ ] **Scoring System**: Each successful whack increments the user’s score.
- [ ] **Difficulty Progression**: Game speeds up (shorter mole visibility) after certain score thresholds.
- [ ] **Audio Feedback**: A “bonk” sound on whack, “game over” sound, etc.
- [ ] **Animations**: Moles animate in and out using **framer-motion**. Possibly confetti or flashy animation on high score.
- [ ] **Responsive/Touch-Friendly**: Works on both desktop (mouse clicks) and mobile (taps).

### 1.2 **Validation Checklist**
- [ ] **Build & Run**: Fresh `npm install && npm run dev` launches without errors.
- [ ] **Random Mole Spawn**: Moles appear in random holes or grid positions at set intervals.
- [ ] **Scoring & Difficulty**: Score increments on click/tap. Mole speed or spawn rate increases after, say, every 10 points.
- [ ] **Audio Playback**: Howler.js triggers correct sounds (e.g., `bonk.mp3`) when the user hits a mole.
- [ ] **Animations**: Moles use **framer-motion** for smooth transitions in/out. Optionally show confetti or an effect on high scores.
- [ ] **Game Over State**: The game stops or transitions to a “results” screen after a time limit or certain conditions (e.g., missed moles).

---

## 2. **User Stories**

1. **Arcade Gameplay**
   - **As a user**, I want a quick, fun game where moles pop up randomly, so I can challenge my reflexes.
   - **Given** the game has started,
   - **When** I tap/click on a mole within the time it’s visible,
   - **Then** I gain a point and hear a “bonk” sound.

2. **Progressive Difficulty**
   - **As a user**, I want the game to get harder as I improve, so it stays challenging.
   - **Given** I have reached 10 points,
   - **When** a new mole spawns,
   - **Then** it appears and disappears faster, making it harder to whack.

3. **Game Over & Feedback**
   - **As a user**, I want to know when the game ends and see my final score.
   - **Given** the time limit or certain conditions are reached,
   - **When** the game ends,
   - **Then** I see a “Game Over” screen or dialog with my total score, and a sound effect plays.

---

## 4. **Additional Game Details (Audio/Animation)**

- **Audio Files**: Place `bonk.mp3`, `game-over.mp3`, etc., in `public/sounds/`.
- **Howler.js**:
	```typescript
	import { Howl } from 'howler';
	
	const bonkSound = new Howl({ src: ['/sounds/bonk.mp3'] });
	const gameOverSound = new Howl({ src: ['/sounds/game-over.mp3'] });
	```

- **framer-motion**:
	- Use `motion.div` or `motion.img` to animate the mole’s position or scale in/out.
	- Possibly add a shake or bounce effect when the user whacks a mole.

5. **File & Folder Structure**
```
mole-game/
├── app/
│   ├── game/
│   │   └── page.tsx          # Main game UI route
│   ├── layout.tsx            # Global layout, includes NavBar if needed
│   └── globals.css           # Tailwind global styles (if used)
├── components/
│   ├── GameBoard.tsx         # Main board with mole "holes" or positions
│   ├── Mole.tsx              # Renders an individual mole, handles framer-motion
│   ├── ScoreDisplay.tsx      # Displays current score/time
│   └── NavBar.tsx            # (Optional) top navigation if needed
├── lib/
│   ├── gameLogic.ts          # Handles spawn rate, difficulty progression, etc.
│   └── audio.ts              # Howler.js instances or helper functions
├── public/
│   └── sounds/
│       ├── bonk.mp3
│       └── game-over.mp3
├── tailwind.config.js
└── package.json
```


### **Explanation of Key Files**

1. **(game)/page.tsx**
    
    - **Route**: `GET /game` (or `GET /` if you prefer the game as homepage)
    - **Purpose**:
        - Renders the `GameBoard` component.
        - Handles game start/stop logic, sets up the main “Play” or “Restart” button.
2. **GameBoard.tsx**
    
    - **Purpose**:
        - Contains a grid or set positions where **Mole** components can appear.
        - Tracks whether a mole is currently visible at each position.
        - Receives a callback (e.g., `onMoleWhacked`) to increment score.
3. **Mole.tsx**
    
    - **Purpose**:
        - Visual representation of a single mole.
        - Uses **framer-motion** for fade/slide in/out animations.
        - On click/tap, triggers a “whack” event and plays the “bonk” sound.
4. **ScoreDisplay.tsx**
    
    - **Purpose**:
        - Shows current score, optional timer, or any game status info.
        - Could update in real-time as the user whacks moles.
5. **lib/gameLogic.ts**
    
    - **Purpose**:
        - Manages the logic for mole spawn intervals, difficulty scaling (faster spawns or shorter visibility after X points).
        - Potentially includes a “game loop” via `requestAnimationFrame` or `setInterval`.
6. **lib/audio.ts**
    
    - **Purpose**:
        - Import and create instances of howler `Howl` objects (e.g., `bonkSound`, `gameOverSound`).
        - Provide helper functions to play these sounds in a consistent manner.

---

## 6. **Core Functionalities & Implementation**

### 6.1 **Mole Spawning & Difficulty**

- Use `setInterval` or a custom “game loop” in **`gameLogic.ts`** to spawn moles in random positions every few seconds.
- After the user reaches a certain score (e.g., 10 points), reduce the spawn interval or reduce the mole’s “visible time.”

### 6.2 **Scoring & Game Over**

- Each successfully whacked mole increments the score.
- You can implement a countdown timer (e.g., 60 seconds) or a certain number of misses to end the game.
- On game end, `gameOverSound` plays, and the user sees a results screen with final score.

### 6.3 **Sound & Animation**

- **Howler.js**:
    - Play `bonkSound.play()` whenever the user clicks a mole.
    - Play `gameOverSound.play()` upon game conclusion.
- **framer-motion**:
    - Animate the mole’s Y position or scale from 0 to 1 when spawning, then back down to 0 if it disappears or is whacked.
    - Optionally add a **confetti** or a short celebratory animation if the user hits a high score threshold.

### 6.4 **Responsive Layout**

- **Tailwind** or custom CSS:
    - `GameBoard` should scale or wrap for smaller screens.
    - Moles remain clickable/tappable without requiring a mouse.

---

## 7. **Completion Criteria**

- **Random Mole Spawn**: Moles appear in random “holes” or positions at intervals.
- **Scoring & Progression**: Score increments on whack; game difficulty ramps up after set thresholds.
- **Animation**: Moles animate into view using **framer-motion**; a “bonk” effect or bounce might occur on whack.
- **Audio Feedback**: Tapping a mole plays “bonk.mp3;” game over triggers “game-over.mp3.”
- **Game Over Screen**: User sees final score or “Game Over” message.
- **Mobile-Friendly**: The entire game is playable via taps, with no layout breaks on smaller devices.
- **No TypeScript Errors**: The project compiles cleanly with `npm run dev`.