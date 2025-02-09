---
name: "Mood Tracker"
about: "Modern mood tracking web app with data visualization"
date_created: "2025-01-26"
project_name: "MoodTracker"
tech_stack: ["NextJS 15", "TypeScript", "Shadcn", "Tailwind CSS", "Chart.js", "date-fns"]
version: "1.3"
---

# 🎯 Mood Tracker PRD

A modern web application for logging daily moods and visualizing emotional trends with charts.

---

## 1. **Success Criteria**

1. **Core Functionality**
   - [ ] **Clickable Calendar**: Users can select a date to log or edit a mood entry.
   - [ ] **Emoji & Note Input**: A modal or dialog with an emoji picker and text field.
   - [ ] **Local Data Storage**: Persist mood entries between sessions.
   - [ ] **Data Visualization**: At least two Chart.js charts to display weekly, monthly, or overall trends.
   - [ ] **Mobile-Responsive**: Layout should adjust for smaller screens without major issues.

2. **Validation Checklist**
   - [ ] **Build & Run**: Fresh `npm install && npm run dev` works without errors.
   - [ ] **Calendar Interaction**: Clicking a calendar date opens the mood logging UI.
   - [ ] **Color Coding**: Each date cell or icon changes based on mood score or emoji.
   - [ ] **Chart Page**: A separate page or section to visualize stats (e.g., line chart + pie chart).
   - [ ] **Data Persistence**: Entries remain available if the user navigates away and comes back later.

---

## 2. **Tech Stack**

- **NextJS 15** (App Router) for site structure
- **TypeScript** for type safety
- **Shadcn** UI components (dialogs, buttons, forms)
- **Tailwind CSS** for styling
- **Chart.js** for data visualization
- **date-fns** for date operations
- **localforage** (or equivalent) for local data storage
- **@emoji-mart/react** for an emoji picker

### **Why These Choices?**
- **NextJS + TypeScript**: Great for server/client flexibility and type safety
- **Shadcn + Tailwind**: Rapid UI development with consistent design
- **Chart.js**: Straightforward library for rendering charts
- **date-fns**: Lightweight date utilities

---

## 3. **Design & Mood Scores**

| MoodScore | Mood       | Tailwind Color | Emoji     |
|-----------|------------|----------------|-----------|
| 1         | Angry      | `red-500`      | 😡         |
| 2         | Sad        | `orange-400`   | 😞         |
| 3         | Neutral    | `yellow-300`   | 😐         |
| 4         | Happy      | `lime-400`     | 😊         |
| 5         | Ecstatic   | `emerald-500`  | 😄         |

> You can style each date cell background or display an icon to indicate the logged mood.

---

## 4. **User Stories**

1. **Daily Mood Logging**
   - **As a user**, I want to quickly log how I feel each day so I can track my emotional journey.
     - **Given** I click on a specific date
     - **When** I choose an emoji and type a note
     - **Then** the date on the calendar updates visually to reflect my mood

2. **Mood Analysis**
   - **As a user**, I want to see a higher-level overview of my moods so I can spot trends.
     - **Given** I navigate to a “Stats” page
     - **When** I select a timeframe (weekly, monthly, etc.)
     - **Then** I see at least two types of charts illustrating changes or distributions in my mood data

---

## 5. **Data Structures**

```typescript
export interface MoodEntry {
  date: string;    // e.g. "2025-01-23"
  emoji: string;   // e.g. "😊"
  note: string;
  moodScore: 1 | 2 | 3 | 4 | 5;
}
```

- Store mood entries in `lib/storage.ts` using local data storage (e.g., localforage).
- Components like `MoodCalendar` and `MoodChart` can import these entries to display logs.

### 6. File Structure
```

mood-tracker/
├── app/
│   ├── (dashboard)/
│   │   └── page.tsx     # main calendar view
│   ├── stats/
│   │   └── page.tsx     # charts & statistics
│   └── layout.tsx       # global layout or shared UI
├── components/
│   ├── MoodCalendar.tsx
│   ├── MoodChart.tsx
│   └── EmojiPicker.tsx
├── lib/
│   ├── storage.ts
│   └── mood.ts          # data types
└── styles/
    └── globals.css
```

### 7. Additional Notes
- **Shadcn**: Ideal for modals (Dialog component), buttons, forms, etc.
- **Chart.js**: Use a line chart, bar chart, pie chart, or any combination to showcase data trends.
- **Optional**: You can add a hover tooltip on each calendar day to preview the note or emoji.

