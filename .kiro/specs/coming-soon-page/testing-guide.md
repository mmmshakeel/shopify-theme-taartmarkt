# Local Testing Guide

This guide provides step-by-step instructions for manually testing each task locally using Shopify CLI and theme development tools.

## Prerequisites

Before starting, ensure you have:
- Shopify CLI installed (`npm install -g @shopify/cli @shopify/theme`)
- A Shopify development store or partner account
- Local development environment set up

## Setup Commands

```bash
# Connect to your Shopify store (run once)
shopify theme dev --store=your-store-name.myshopify.com

# This will start a local development server, typically at:
# http://127.0.0.1:9292
```

## Task-by-Task Testing Instructions

### Task 1: Set up core template structure and theme settings

**What to test:**
- Template file creation
- Theme settings appear in customizer
- Basic structure renders

**Testing steps:**
1. Start local development: `shopify theme dev`
2. Open your local development URL in browser
3. Go to Shopify Admin → Online Store → Themes → Customize
4. Look for "Coming Soon Page" section in theme settings
5. Verify all settings fields appear (heading, description, toggles, etc.)
6. Check that coming-soon.json template exists in templates folder

**Expected results:**
- New settings section visible in theme customizer
- Template file created without errors
- No console errors in browser developer tools

---

### Task 2: Implement coming soon page section with responsive design

**What to test:**
- Section renders properly
- Responsive design works
- Background images display
- Content positioning works

**Testing steps:**
1. In theme customizer, navigate to the coming soon template
2. Add the "Coming Soon Page" section
3. Upload a background image in settings
4. Test different content positions (top-left, center, bottom-right, etc.)
5. Test on different screen sizes:
   - Desktop: Resize browser window to 1200px+ width
   - Tablet: Resize to 768px-1024px width  
   - Mobile: Resize to 320px-767px width
6. Check image overlay opacity controls

**Expected results:**
- Section displays with proper styling
- Background image shows with overlay
- Content repositions correctly based on settings
- Layout is responsive across all screen sizes
- No layout breaks or overflow issues

---

### Task 3: Implement HubSpot form integration block

**What to test:**
- HubSpot embed code renders
- Form styling matches theme
- Error handling for invalid codes

**Testing steps:**
1. Get a test HubSpot form embed code (create free HubSpot account if needed)
2. In theme customizer, add HubSpot form block to coming soon section
3. Paste embed code in the settings field
4. Save and preview the page
5. Test form submission with test data
6. Test with invalid embed code (paste random text)
7. Test with empty embed code field

**Expected results:**
- Valid HubSpot form displays and functions
- Form styling integrates well with theme
- Invalid embed code shows fallback message
- Empty field shows placeholder or setup instructions
- Form submissions work (check HubSpot dashboard)

---

### Task 4: Implement baker registration button functionality

**What to test:**
- Button displays and links correctly
- Opens in new tab
- Button styling matches theme

**Testing steps:**
1. Create a test Google Form (or use placeholder URL)
2. In theme customizer, add baker registration block
3. Enter Google Form URL in settings
4. Customize button text
5. Save and test the button click
6. Verify it opens in new tab/window
7. Test with invalid URL
8. Test with empty URL field

**Expected results:**
- Button displays with custom text
- Clicking opens Google Form in new tab
- Original page remains open
- Button is properly styled and accessible
- Invalid URLs show error state or disable button
- Empty URL field shows setup message

---

### Task 5: Implement password page access link

**What to test:**
- Link appears and functions
- Routes to password page correctly
- Styling is subtle but visible

**Testing steps:**
1. In theme customizer, add password link block
2. Customize link text if desired
3. Save and locate the link on the page
4. Click the link to verify it goes to `/password`
5. Test that password page still functions normally
6. Return to coming soon page and verify link placement

**Expected results:**
- Link is visible but subtle in design
- Clicking navigates to Shopify password page
- Password page functionality remains intact
- Link text is customizable
- Link has proper accessibility attributes

---

### Task 6: Add theme setting controls and validation

**What to test:**
- Settings update preview in real-time
- Validation works for URLs and codes
- Default values load correctly

**Testing steps:**
1. Open theme customizer with coming soon page
2. Change various settings and observe real-time preview updates:
   - Heading text
   - Background image
   - Content position
   - Color scheme
3. Test URL validation by entering invalid URLs
4. Test embed code validation with malformed code
5. Clear all settings and verify defaults load

**Expected results:**
- Changes appear immediately in preview
- Invalid URLs show validation errors
- Invalid embed codes show warnings
- Default values populate when fields are empty
- No JavaScript errors in console

---

### Task 7: Implement coming soon mode activation system

**What to test:**
- Coming soon mode toggle works
- Template switching functions
- Admin access remains available

**Testing steps:**
1. In theme customizer, find "Enable Coming Soon Mode" toggle
2. Enable the toggle and save
3. Visit your store's main URL (not the preview URL)
4. Verify coming soon page displays instead of normal homepage
5. Test that `/password` still works for admin access
6. Disable coming soon mode and verify normal site returns

**Expected results:**
- Toggle controls whether coming soon page shows
- Main site URL shows coming soon when enabled
- Password page access remains functional
- Normal site returns when disabled
- No redirect loops or errors

---

### Task 8: Add comprehensive error handling and fallbacks

**What to test:**
- Error states display properly
- Fallback content shows when needed
- User-friendly error messages

**Testing steps:**
1. Test various error scenarios:
   - Remove HubSpot embed code while form block is active
   - Enter invalid Google Form URL
   - Remove background image
   - Clear all text content
2. Verify each error shows appropriate fallback
3. Check that error messages are helpful, not technical
4. Test recovery by fixing the errors

**Expected results:**
- Missing content shows helpful placeholder messages
- Invalid configurations display clear error states
- Fallback content maintains page functionality
- Error messages guide users to fix issues
- Page remains functional even with configuration errors

---

### Task 9: Optimize performance and add loading states

**What to test:**
- Page loads quickly
- Images load efficiently
- Loading states appear appropriately

**Testing steps:**
1. Test page load speed using browser dev tools:
   - Open Network tab in Chrome DevTools
   - Reload the coming soon page
   - Check load times and resource sizes
2. Test with slow network (throttle to "Slow 3G" in DevTools)
3. Observe loading states for HubSpot form
4. Test image loading with large background images

**Expected results:**
- Page loads in under 3 seconds on normal connection
- Images are optimized and load progressively
- Loading states provide visual feedback
- Page remains usable during loading
- No render-blocking resources delay initial paint

---

### Task 10: Add accessibility features and ARIA labels

**What to test:**
- Keyboard navigation works
- Screen reader compatibility
- Focus management

**Testing steps:**
1. Test keyboard navigation:
   - Use Tab key to navigate through all interactive elements
   - Use Enter/Space to activate buttons and links
   - Ensure focus is visible on all elements
2. Test with screen reader (or browser accessibility tools):
   - Use Chrome DevTools Accessibility panel
   - Check that all elements have proper labels
   - Verify heading hierarchy is logical
3. Test color contrast using accessibility tools

**Expected results:**
- All interactive elements are keyboard accessible
- Focus indicators are clearly visible
- Screen readers can navigate and understand content
- Color contrast meets WCAG guidelines
- Heading structure is semantic and logical

---

### Task 11: Create comprehensive test coverage

**What to test:**
- Cross-browser compatibility
- Different device sizes
- All interactive features

**Testing steps:**
1. Test in multiple browsers:
   - Chrome, Firefox, Safari, Edge
   - Mobile browsers (iOS Safari, Chrome Mobile)
2. Test on actual devices if possible:
   - Phone, tablet, desktop
3. Test all interactive elements in each browser:
   - HubSpot form submission
   - Baker registration button
   - Password page link
   - Theme customizer settings

**Expected results:**
- Consistent appearance across all browsers
- All functionality works in each browser
- Responsive design works on real devices
- No browser-specific bugs or layout issues
- Forms and links function properly everywhere

---

### Task 12: Add final styling and theme integration

**What to test:**
- Visual consistency with theme
- Smooth animations
- Color scheme integration

**Testing steps:**
1. Compare coming soon page styling with main theme:
   - Typography consistency
   - Color scheme matching
   - Button and form styling alignment
2. Test different color schemes in theme customizer
3. Test animations and transitions:
   - Hover effects on buttons
   - Form focus states
   - Loading animations
4. Test dark mode if theme supports it

**Expected results:**
- Coming soon page looks like part of the main theme
- All color schemes work properly
- Animations are smooth and purposeful
- Typography and spacing match theme standards
- Dark mode works if applicable

## General Testing Tips

### Browser Developer Tools
- Always keep DevTools open during testing
- Monitor Console for JavaScript errors
- Use Network tab to check resource loading
- Use Responsive Design Mode for mobile testing

### Shopify CLI Commands
```bash
# Push changes to development theme
shopify theme push --development

# Pull latest changes
shopify theme pull --development

# Check for theme issues
shopify theme check
```

### Common Issues to Watch For
- **CORS errors**: HubSpot forms may have cross-origin issues
- **Mixed content**: HTTPS/HTTP conflicts with external resources
- **Mobile layout**: Text or buttons too small on mobile
- **Loading performance**: Large images slowing page load
- **Theme conflicts**: Custom CSS conflicting with theme styles

### Testing Checklist for Each Task
- [ ] No console errors
- [ ] Responsive design works
- [ ] Accessibility is maintained
- [ ] Performance is acceptable
- [ ] Cross-browser compatibility
- [ ] Error states handled gracefully
- [ ] Settings save and load correctly
- [ ] Visual consistency with theme