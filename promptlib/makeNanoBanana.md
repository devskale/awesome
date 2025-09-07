# NanoBanana Web UI Prompt

## Product Definition

### High Level
Build a simple web-based user interface called "NanoBanana" that allows users to upload an image, either select a prompt from a library of prompts or write their own custom prompt, and send both to Google Gemini (NanoBanana) API to generate a new image, which is then displayed in the UI.

### Goal Setting
- Provide an intuitive interface for image processing with AI
- Integrate seamlessly with Google Gemini API for image generation
- Allow easy selection from a predefined prompt library or custom prompt entry
- Display results immediately after processing

## Design System

### Colors
- Primary: #4CAF50 (Green for action buttons)
- Secondary: #2196F3 (Blue for links and accents)
- Background: #f5f5f5 (Light gray)
- Text: #333333 (Dark gray)
- Error: #f44336 (Red)

### Typography
- Font Family: 'Roboto', sans-serif
- Headings: 24px bold
- Body: 16px normal
- Small: 14px normal

### Structure
- Container: Max width 800px, centered
- Padding: 20px
- Border radius: 8px for cards
- Box shadow: 0 2px 4px rgba(0,0,0,0.1)

## IA & UX Flow

### Navigation Design
- Single page application
- No navigation menu needed

### Page Layout
- Header with title
- Main content area divided into:
  - Image upload section
  - Prompt selection section (dropdown from library)
  - Custom prompt section (textarea for user input)
  - Submit button
  - Results display section

## Functional Screens

### Key Workflows
- User Onboarding: Simple instructions on how to use
- Core functionality:
  1. Upload image
  2. Select prompt from library or enter custom prompt
  3. Click submit
  4. View result
- Error handling: Display API errors or invalid inputs

## Components & Reuse

### System design elements
- Buttons: Rounded corners, hover effects
- Forms: Styled inputs with focus states
- Icons: Use simple SVG icons for upload, submit
- Cards: For sections with subtle shadows

## Animations

### Transitions
- Smooth fade-in for results
- Loading spinner during API call

### UX Polish
- Hover effects on buttons
- Progress indicator during processing

## Responsive Rules

### Cross Device Behaviour
- Mobile: Stack sections vertically, full width
- Tablet: Maintain layout with adjusted spacing
- Desktop: Centered layout with max width

## Engineering Stack

### Build Instructions
- HTML5 for structure
- CSS3 for styling
- Vanilla JavaScript for functionality
- Google Generative AI SDK for API integration

### Deployment Guidelines
- Static hosting (GitHub Pages, Netlify)
- No backend required for basic functionality
- API key handling: Load from environment or user input (not secure for production)

### Implementation Details

1. **HTML Structure**:
   - Header with title "NanoBanana Image Processor"
   - File input for image upload
   - Select dropdown for prompts from library
   - Textarea for custom prompt input
   - Button to submit
   - Div for displaying result image
   - Loading indicator

2. **CSS Styling**:
   - Responsive design
   - Clean, modern look
   - Consistent spacing and colors

3. **JavaScript Functionality**:
   - Load prompts from /promptlib directory (fetch .md files)
   - Populate select with prompt options
   - Handle file upload and preview
   - Allow user to enter custom prompt in textarea
   - Prioritize custom prompt if entered, otherwise use selected library prompt
   - Send image and prompt (custom or selected) to Gemini API
   - Display generated image

4. **API Integration**:
   - Use @google/generative-ai SDK
   - Model: Gemini 1.5 Flash (assuming 2.5 Flash Image maps to this)
   - API key: Load from .env file or prompt user
   - Send multipart request with image and text prompt
   - Handle response and display generated image

5. **Prompt Handling**:
   - Fetch all .md files from /promptlib/
   - Populate select dropdown with filenames as options
   - Use file content as prompt text when selected
   - Provide textarea for custom prompt input
   - Logic: If custom prompt textarea has content, use that; otherwise use selected library prompt
   - Handle CORS if necessary (may need local server)

6. **Error Handling**:
   - Validate image upload (type, size)
   - Check API response for errors
   - Display user-friendly error messages

7. **Security Considerations**:
   - API key should not be hardcoded
   - For demo purposes, use environment variable
   - In production, implement server-side proxy

Create a complete, functional web application that meets all these specifications. Ensure the code is clean, well-commented, and follows best practices.