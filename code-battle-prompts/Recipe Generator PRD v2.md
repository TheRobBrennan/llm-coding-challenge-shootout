[Conversation](https://chatgpt.com/c/6795f658-57e8-8013-b7ef-b06db8f976e9)
---
name: "Random Food Recipe Generator"
about: "Web app that calls an external API (TheMealDB) to generate and filter recipes"
date_created: "2025-01-28"
project_name: "RandomRecipeApp"
tech_stack: [
  "NextJS (App Router)",
  "TypeScript",
  "Shadcn",
  "Tailwind CSS",
  "localforage",
]
version: "1.2"

---

# 🍽️ Random Food Recipe Generator (with External API)

A modern web application that fetches recipes from an **external API** (TheMealDB) and provides randomized suggestions based on user-selected categories and dietary preferences (where possible). Users can view ingredients, cooking instructions, and optionally save recipes to a Favorites list.

---

## 1. **Success Criteria**

### 1.1 **Core Functionality**
- [ ] **External API Integration**: The app calls **TheMealDB** to fetch recipe data (random or filtered).
- [ ] **Filter Dialog**: A modal/dialog for selecting meal category (e.g., “Beef,” “Vegan,” “Dessert,” etc.) and basic dietary preferences.
- [ ] **Random Recipe Generation**: When the user clicks “Generate Recipe,” the system picks a random result from the API’s response.
- [ ] **Recipe Display**: Show the recipe’s name, ingredients, instructions, and image (if available).
- [ ] **Favorites Management**: Save or remove recipes from a local Favorites list (using `localforage`).
- [ ] **Mobile-Responsive Layout**: Interface adapts for various screen sizes.

### 1.2 **Validation Checklist**
- [ ] **API Call**: A successful response from TheMealDB’s endpoints (either random or filtered by category).
- [ ] **Random Generation**: The user sees a random dish from the fetched results.
- [ ] **Filter/Category Support**: The user can select from categories that TheMealDB supports (e.g., “Seafood,” “Dessert,” “Vegan,” etc.), and only relevant meals are returned.
- [ ] **Favorites Page**: `/favorites` displays saved recipes; removing an item updates the list.
- [ ] **Data Persistence**: Favorites remain in local storage after page refresh.

---

## 2. **User Stories**

1. **Random Meal Inspiration (API-Based)**
   - **As a user**, I want to fetch fresh recipes from a real service so I can discover new meal ideas.
   - **Given** I click “Generate Recipe” after optionally selecting a category (e.g., “Seafood”),
   - **When** the app calls TheMealDB and obtains a random dish matching that category,
   - **Then** I see the recipe’s title, ingredients, instructions, and an image (if provided by the API).

2. **Managing Favorites**
   - **As a user**, I want to save recipes that interest me so I can quickly revisit them.
   - **Given** I’m viewing a recipe fetched from TheMealDB,
   - **When** I click “Save to Favorites,”
   - **Then** it’s added to my local list of favorites, visible at `/favorites`.

---

## 3. **Project Setup & Dependencies**

1. **Initialize Next.js ** with `create-next-app`


2. **Install Required Packages**
```bash
# Tailwind CSS
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Shadcn UI + localforage for storing favorites
npm install shadcn localforage

# (Optional) Chart.js for any stats or visuals
npm install chart.js react-chartjs-2

# If needed for date manipulation
npm install date-fns

```

3. **Tailwind & Shadcn Configuration**

- In `tailwind.config.js`, include paths for `app/**/*.{js,ts,jsx,tsx}` and `components/**/*.{js,ts,jsx,tsx}`.
- Generate or configure Shadcn components per official instructions.

4. **Global CSS**

- In `app/globals.css`, import Tailwind
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

```

## **External API Details (TheMealDB)**

We will use **TheMealDB** for this project. Key endpoints:

- **Random Meal**: `https://www.themealdb.com/api/json/v1/1/random.php`
    
    - Returns a single random meal.
    - You can call it multiple times or once if you want multiple suggestions.
- **Filter by Category**: `https://www.themealdb.com/api/json/v1/1/filter.php?c={CATEGORY}`
    
    - For example: `?c=Seafood`, `?c=Dessert`, etc.
    - Returns a list of meals with minimal data (idMeal, strMeal, strMealThumb).
- **Lookup Meal by ID**: `https://www.themealdb.com/api/json/v1/1/lookup.php?i={MEAL_ID}`
    
    - Fetch full details (ingredients, instructions, etc.) after filtering.

**Implementation Approach**:

1. **If user selects a category**:
    - Call the filter endpoint for that category to get a list of meal IDs.
    - Randomly pick one meal ID from that list.
    - Call the lookup endpoint to get detailed info (ingredients, instructions, etc.).
2. **If no category is selected**:
    - Use `random.php` to get a random meal directly, or you could fetch categories first.

## 5. **File & Folder Structure**

```
random-recipe-app/
├── app/
│   ├── (home)/
│   │   └── page.tsx               # Main recipe generator UI
│   ├── favorites/
│   │   └── page.tsx               # Displays saved (favorite) recipes
│   ├── layout.tsx                 # Global layout, includes NavBar if needed
│   └── globals.css                # Tailwind global styles
├── components/
│   ├── RecipeGenerator.tsx        # Core random generator component (Generate Recipe button, calls API)
│   ├── RecipeFilterDialog.tsx     # Dialog for choosing category/diet preferences (where possible)
│   ├── RecipeDisplay.tsx          # Shows recipe details (title, ingredients, instructions, image)
│   ├── RecipeCard.tsx             # Layout for displaying favorite recipes
│   └── NavBar.tsx                 # Navigation bar (links to Home & Favorites)
├── lib/
│   ├── storage.ts                 # localforage wrapper (getFavorites, addFavorite, removeFavorite)
│   ├── api.ts                     # Functions to call TheMealDB (fetchRandomMeal, fetchMealsByCategory, lookupMealById)
│   └── filters.ts                 # (Optional) any custom logic for partial dietary filtering
├── public/
│   └── images/                    # Optional placeholder images if needed
├── tailwind.config.js
└── package.json

```

## 6. **Core Functionalities & Implementation**

### 6.1 **API Integration & Randomization**

- **`lib/api.ts`**:
```typescript
const BASE_URL = "https://www.themealdb.com/api/json/v1/1";

export async function fetchRandomMeal() {
  const response = await fetch(`${BASE_URL}/random.php`);
  const data = await response.json();
  return data.meals?.[0] || null;
}

export async function fetchMealsByCategory(category: string) {
  // e.g. category could be "Seafood", "Dessert", etc.
  const response = await fetch(`${BASE_URL}/filter.php?c=${category}`);
  const data = await response.json();
  return data.meals || [];
}

export async function lookupMealById(mealId: string) {
  const response = await fetch(`${BASE_URL}/lookup.php?i=${mealId}`);
  const data = await response.json();
  return data.meals?.[0] || null;
}

```

- **User Flow**:
    
    1. If **no category** is chosen, call `fetchRandomMeal()` directly.
    2. If **category** is chosen, call `fetchMealsByCategory(category)`, pick one meal ID at random, then call `lookupMealById(mealId)` to get full details.

### 6.2 **Recipe Display & Favorites**

- **`RecipeDisplay.tsx`**:
    
    - Renders the meal’s name (`strMeal`), thumbnail (`strMealThumb`), instructions (`strInstructions`), etc.
    - “Save to Favorites” button calls `addFavorite(mealObject)` from `storage.ts`.
- **`storage.ts`**:
    
    - Provides `getFavorites()`, `addFavorite()`, `removeFavorite()`, storing each meal object under a single localforage key (e.g., `"favoriteMeals"`).

### 6.3 **Favorites Page**

- **`favorites/page.tsx`**:
    - Calls `getFavorites()` on load to retrieve a list of saved meals.
    - Renders each meal in a `RecipeCard` with a “Remove” button that calls `removeFavorite(meal.idMeal)`.


## 7. **Completion Criteria**

- **Random Generation**: The system successfully calls TheMealDB to fetch random or category-based meals.
- **Display Data**: Each recipe’s relevant fields (title, image, ingredients, instructions) appear correctly.
- **Favorites Persist**: The user can add a recipe to Favorites, see it in `/favorites`, and remove it; changes persist across page refresh.
- **Layout**: The UI is functional and readable on mobile and desktop.
- **No TypeScript Errors**: Project compiles cleanly with `npm run dev`.