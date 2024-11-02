# Canvas Drawing and Expression Renderer

This project is a canvas drawing app that allows users to draw mathematical expressions, which are then interpreted, displayed in LaTeX format, and made draggable. Below are steps to get started, details on the core code, and an introduction to using the application.

## Live Demo
[**Check out the live version here!**](https://ai-canvas-calculator-frontend.vercel.app)

## Quick Start Guide

1. **Draw an Expression**: Choose a color from the dropdown, then draw your expression on the canvas.
2. **Run the Calculation**: Click the **Run** button to analyze your drawing and display the LaTeX-rendered output.
3. **Drag the Output**: You can move the rendered expression around on the screen by dragging it to your preferred position.

---

## Developer Setup

### Prerequisites
- **Node.js** and **npm**: Make sure these are installed.

### Installation

1. Clone the repository and navigate to the project directory.
   ```bash
   git clone https://github.com/SaiDeepak16/AI-Canvas-Calculator-Frontend
   cd AI-Canvas-Calculator-Frontend
   ```

2. Install the dependencies for the frontend:
   ```bash
   npm install
   ```

3. Configure environment variables:
   - Set up `VITE_API_URL` in a `.env` file to point to your backend URL.

4. Start the development server:
   ```bash
   npm run dev
   ```

The application will be available on `localhost:3000`.

---

## Code Overview

### File Structure
- **src/screens/home/index.jsx**: Main component file where the drawing and rendering logic are handled.
- **src/components/ui/button.jsx**: Custom button component for handling UI interactions.
- **src/constants.js**: Contains constants like color swatches used in the app.

### Canvas and Drawing Functions

The canvas is set up to enable freehand drawing, with event listeners for **mouse** and **touch** actions, making it responsive across devices.

- **`startDrawing()`**: Initializes the drawing state, setting the canvas background color and starting coordinates.
- **`draw()`**: Continuously records cursor or touch movements and draws lines in real-time based on the selected color.
- **`stopDrawing()`**: Ends the drawing state when the user lifts their finger or mouse.

  ```javascript
  const startDrawing = (e) => { ... };
  const draw = (e) => { ... };
  const stopDrawing = () => { ... };
  ```

### LaTeX Rendering on Canvas

After running calculations, results are rendered as LaTeX expressions.

- **`renderLatexToCanvas(expression, answer)`**: Converts the expression and answer into LaTeX format. MathJax is used here to interpret and render the LaTeX string. Once rendered, the output appears on the canvas, and its position can be adjusted by dragging.

  ```javascript
  const renderLatexToCanvas = (expression, answer) => { 
      const latex = `\\(\\LARGE{${expression} = ${answer}}\\)`; 
      ...
      setLatexExpression([...latexExpression, latex]); 
  };
  ```

### Draggable Elements

To offer flexible interaction, we use the **`react-draggable`** library, allowing rendered LaTeX expressions to be moved across the canvas. The `Draggable` component wraps around each LaTeX-rendered output.

- **`latexPosition`**: Tracks the position of LaTeX elements. It updates dynamically with drag actions.
- **Event Listener**: The `onStop` event listener updates `latexPosition` after dragging stops.

  ```javascript
  <Draggable 
      defaultPosition={latexPosition} 
      onStop={(_, data) => setLatexPosition({ x: data.x, y: data.y })}>
      <div className="latex-content">{latex}</div>
  </Draggable>
  ```

### Color Selection and Reset

To ensure flexibility in drawing styles, users can select colors from a dropdown. This makes it responsive for mobile users.

- **`SWATCHES`**: Defined colors are mapped and rendered as dropdown options.
- **Reset Functionality**: Clicking the **Reset** button clears the canvas, LaTeX expressions, and any stored variables.

  ```javascript
  <select onChange={(e) => setColor(e.target.value)}>
      {SWATCHES.map((color) => (
          <option key={color} value={color}>{color}</option>
      ))}
  </select>
  ```
- **Backend Call for Calculations** (`runRoute`)
  - **runRoute()**: Captures the canvas drawing as an image, sends it to the backend API for processing, and receives LaTeX expressions as a response.

- **LaTeX Rendering** (`renderLatexToCanvas`)
  - Adds LaTeX-formatted expressions on the canvas after receiving results from the backend.
  - Uses MathJax for rendering mathematical expressions dynamically.

- **Reset Canvas** (`resetCanvas`)
  - Clears the canvas and resets the app state.

---

### Libraries Used

- **React**: For building the component-based UI.
- **Tailwind CSS**: For styling the app with utility-first CSS classes.
- **MathJax**: For rendering LaTeX expressions in the browser.
- **axios**: For API calls to the backend.
- **react-draggable**: Allows LaTeX elements to be moved on the canvas.

## Usage Notes

1. **Interactivity**: Drawing lines on the canvas starts in white but updates to the selected color.
2. **Responsiveness**: The app adapts to screen size, with dropdown color selection for mobile.

---

Feel free to customize and extend this code!
