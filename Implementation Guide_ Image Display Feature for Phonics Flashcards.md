# Implementation Guide: Image Display Feature for Phonics Flashcards

This guide provides the necessary modifications to your `pasted_content.txt` (which appears to be an `index.html` file) to implement the requested feature: a button at the bottom-left corner that toggles the display of an image corresponding to the current word.

## 1. File Structure and Assets

First, you need to organize your image files.

1.  Create a new folder named `images` in the same directory as your HTML file.
2.  Place all your collected images inside this `images` folder.
3.  Ensure the image file names are lowercase and match the words for easy mapping (e.g., `orange.png`, `hotdog.png`, `swan.png`).

## 2. HTML Modification

We need to add two new elements inside the `<div class="card-container">`:

1.  A new `<img>` tag to display the image.
2.  A new button to toggle the image display.

**Locate the following lines (around line 111-113):**

```html
<div class="card" id="word-display">
</div>
<button id="pronounce-btn">🔊</button>
```

**Replace them with the following code:**

```html
<div class="card" id="word-display">
    <!-- NEW: Image Display Area -->
    <img id="word-image" src="" alt="Image for the current word" class="hidden">
</div>
<button id="pronounce-btn">🔊</button>
<!-- NEW: Image Toggle Button -->
<button id="image-toggle-btn">🖼️</button>
```

## 3. CSS Modification

We need to add styles for the new image element, the new button, and the class that controls image visibility.

**Add the following CSS rules inside the `<style>` block (e.g., after line 75):**

```css
/* NEW: Image Toggle Button */
#image-toggle-btn {
    position: absolute;
    bottom: 15px;
    left: 15px; /* Positioned at the bottom-left */
    background-color: #3498db; /* A distinct color for the new button */
    color: white;
    border: none;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    font-size: 1.5rem;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    transition: background-color 0.2s;
    z-index: 10; /* Ensure it's above the card */
}

#image-toggle-btn:hover {
    background-color: #2980b9;
}

/* NEW: Image Styling */
#word-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: contain; /* Ensures the image fits without cropping */
    border-radius: 12px;
    background-color: #f5f7fa; /* White background for the image */
    padding: 10px;
    box-sizing: border-box;
    z-index: 5; /* Below the buttons, above the word text */
    opacity: 0; /* Hidden by default */
    transition: opacity 0.3s ease-in-out;
}

/* NEW: Class to show the image */
#word-image.visible {
    opacity: 1;
}

/* Modify word-display to handle the image overlay */
.card {
    position: relative; /* Ensure image is positioned relative to the card */
    /* When image is visible, we want the word text to be hidden */
    transition: opacity 0.3s ease-in-out;
}

.card.image-active {
    opacity: 0; /* Hide the word text when image is active */
}
```

## 4. JavaScript Modification

We need to define the image map, get the new elements, and implement the toggle logic.

**A. Image Map (Around line 130)**

You will need to fill this map with the words and the paths to the images I will provide.

```javascript
// --- 1. Your Word List ---
// ... (existing rawWordData and words array) ...

// --- NEW: Word to Image Mapping ---
const wordToImageMap = {
    "Orange": "images/orange.png",
    "Hot Dog": "images/hotdog.png",
    "Swan": "images/swan.png",
    "Wash": "images/wash.png",
    "Want": "images/want.png",
    "Pan": "images/pan.png",
    // ... continue with all your words ...
};
```

**B. Get New HTML Elements (Around line 209)**

```javascript
// ... (existing elements) ...
const shuffleButton = document.getElementById('shuffle-btn');

// --- NEW ELEMENTS ---
const imageToggleBtn = document.getElementById('image-toggle-btn');
const wordImage = document.getElementById('word-image');
```

**C. New Functions (Around line 263)**

Add the image toggle function and modify `updateCard` to handle the image state.

```javascript
// ... (existing shuffleWords function) ...

// --- NEW: Image Toggle Function ---
function toggleImage() {
    const isVisible = wordImage.classList.toggle('visible');
    wordDisplay.classList.toggle('image-active', isVisible);
    
    // Update the button text/emoji
    imageToggleBtn.textContent = isVisible ? '🔠' : '🖼️';
}

/** Updates the text on the card and the counter */
function updateCard() {
    const currentWord = words[currentWordIndex];
    wordDisplay.textContent = currentWord;
    counterDisplay.textContent = `Word ${currentWordIndex + 1} / ${words.length}`;

    // --- NEW: Update Image Source ---
    const imagePath = wordToImageMap[currentWord];
    if (imagePath) {
        wordImage.src = imagePath;
    } else {
        // Handle words without an image (optional)
        wordImage.src = ""; 
    }

    // --- NEW: Hide image when changing word ---
    wordImage.classList.remove('visible');
    wordDisplay.classList.remove('image-active');
    imageToggleBtn.textContent = '🖼️';
}

// ... (existing showNextWord, showPrevWord, speakWord functions) ...
```

**D. Add Event Listener (Around line 285)**

```javascript
// ... (existing event listeners) ...
shuffleButton.addEventListener('click', shuffleWords);

// --- NEW EVENT LISTENER ---
imageToggleBtn.addEventListener('click', toggleImage);

document.addEventListener('keydown', handleKeyDown);
```

## 5. Next Steps

I am currently compiling the full list of images and the complete `wordToImageMap` for you. I will deliver the images and the final, complete code file shortly.

***

*This document was prepared by Manus AI.*
