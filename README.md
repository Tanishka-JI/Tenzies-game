# 🎯 Assembly: Endgame

A Hangman-style word guessing game with a programming twist — 
every wrong guess eliminates a programming language from existence. 
Guess the word before Assembly takes over the world!


🔗 [Live Demo](#) https://tenzies-game-y3zz.vercel.app/
📁 [GitHub Repo](#) https://github.com/Tanishka-JI/Tenzies-game

---

## 🎮 How to Play

1. A random word is selected — you see blank spaces for each letter
2. Click letters on the on-screen keyboard to guess
3. **Correct guess** → letter reveals in the word
4. **Wrong guess** → a programming language gets eliminated (left to right)
5. **WIN** → guess the full word before all languages die 🎉
6. **LOSE** → run out of attempts — Assembly wins 😭

---

## ✨ Features

- Random word selection on every new game
- Visual language "chip" death chain (each wrong guess eliminates the next language)
- Color-coded keyboard (green = correct, red = wrong, grey = unused)
- Full word reveal on loss (shows missed letters highlighted)
- Farewell messages for each eliminated language
- Confetti celebration on win
- Screen reader accessible (aria-live regions)
- Instant new game reset

---

## 🧠 Concepts Demonstrated

- **Derived state** — win/loss conditions, wrong guess count, 
  last guessed letter all calculated from 2 core states
- **Lazy initial state** — `useState(() => getRandomWord())` 
  runs the word picker only ONCE, not on every render
- **`.every()`** for win detection across all word letters
- **`.filter()`** for counting wrong guesses from guessedLetters array
- **`.split("")` + `.join()`** for string-to-array transformations
- **`clsx`** for conditional className building across 
  keyboard, letter display, and language chips
- **Conditional rendering** (`&&`, ternary) for game status messages
- **Helper function returning JSX** (`renderGameStatus()`) 
  for complex multi-branch conditional UI
- **Accessibility** — `aria-live`, `aria-label`, `sr-only` 
  hidden region for screen reader announcements

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| React.js | UI framework |
| JavaScript (ES6+) | Core game logic |
| clsx | Conditional className utility |
| react-confetti | Win celebration animation |
| CSS | Styling |
| Vite | Build tool |

---

## 🚀 Run Locally


git clone https://github.com/Tanishka-JI/Tenzies-game
cd assembly-endgame
npm install
npm run dev
```

---

## 🔍 Key Technical Decisions

**Why only 2 `useState` values for an entire game?**

The entire game runs on just two pieces of state:
```js
const [currentWord, setCurrentWord] = useState(() => getRandomWord())
const [guessedLetters, setGuessedLetters] = useState([])
```

Everything else — win condition, loss condition, wrong guess 
count, last guessed letter, game over status — is **derived** 
from these two values on every render:

```js
const wrongGuessCount = 
    guessedLetters.filter(letter => !currentWord.includes(letter)).length

const isGameWon = 
    currentWord.split("").every(letter => guessedLetters.includes(letter))

const isGameLost = wrongGuessCount >= numGuessesLeft
```

This guarantees perfect synchronization — derived values 
can NEVER get "out of sync" with actual game state because 
they're recalculated fresh on every render.

**Why `useState(() => getRandomWord())` instead of `useState(getRandomWord())`?**

```js
// ❌ Calls getRandomWord() on EVERY render (wasteful, 
//    could cause unexpected word changes)
useState(getRandomWord())

// ✅ Only calls it ONCE — on first mount (lazy initialization)
useState(() => getRandomWord())
```

**Why a helper function (`renderGameStatus()`) instead of inline JSX?**

The status section has 3+ different possible outputs 
(farewell message, win message, loss message, or nothing). 
Nested ternaries for 3+ conditions become unreadable. 
A named function with clear `if` branches keeps the 
return JSX clean and each condition easy to reason about.

**The language chip "death chain" — how it works:**
```js
const isLanguageLost = index < wrongGuessCount
```
Each language's "dead" status is derived from whether 
its position in the array is LESS THAN the current 
wrong guess count — no separate tracking needed.

---

## 📊 Game Logic Flow