# Design Document

## Overview

The coming soon page will be implemented as a custom Shopify template that replaces the default index page when the site is in "under construction" mode. The design leverages Shopify's existing email-signup-banner section as a foundation but extends it with additional functionality for HubSpot integration, baker registration, and password page access.

The solution will be implemented using Shopify's native template system, ensuring compatibility with the Dawn theme architecture while providing easy customization through the theme editor.

## Architecture

### Template Structure
- **Custom Template**: `templates/coming-soon.json` - A new template that will be used instead of the index template
- **Custom Section**: `sections/coming-soon-page.liquid` - A specialized section based on email-signup-banner but with extended functionality
- **Theme Settings**: Extended settings in `config/settings_schema.json` for configuration
- **Assets**: Custom CSS and JavaScript files for enhanced functionality

### Activation Mechanism
The coming soon page will be activated through a theme setting that allows the site owner to:
1. Enable/disable coming soon mode
2. Choose which template to redirect to (coming-soon.json)
3. Configure all related settings through the theme editor

### Integration Points
- **HubSpot Forms**: Embedded via iframe or JavaScript widget
- **Google Forms**: Direct link integration with configurable URL
- **Shopify Password Page**: Native Shopify password protection system
- **Theme Customizer**: All settings accessible through Shopify's theme editor

## Components and Interfaces

### 1. Coming Soon Section (`sections/coming-soon-page.liquid`)

**Purpose**: Main content area for the coming soon page

**Key Features**:
- Responsive hero section with background image support
- Customizable heading and description text
- HubSpot form integration area
- Baker registration button
- Discreet password page link
- Mobile-optimized layout

**Configuration Options**:
- Background image with overlay opacity control
- Content positioning (9 positions: top/middle/bottom + left/center/right)
- Color scheme selection
- Text content customization
- Button styling options

### 2. HubSpot Integration Component

**Implementation Approach**: 
- **Option A**: Iframe embed (simpler, more reliable)
- **Option B**: JavaScript widget (more integrated appearance)

**Configuration**:
- HubSpot form embed code input in theme settings
- Form styling to match theme design
- Success/error message handling
- GDPR compliance considerations

### 3. Baker Registration Component

**Implementation**:
- Styled button with configurable text
- Opens Google Form in new tab/window
- Configurable Google Form URL through theme settings
- Button styling consistent with theme design

### 4. Password Page Access Component

**Implementation**:
- Small, discreet text link
- Positioned subtly (footer area or small text)
- Links to `/password` route (Shopify's native password page)
- Configurable link text

### 5. Theme Settings Extension

**New Settings Group**: "Coming Soon Page"
- Enable/disable coming soon mode
- HubSpot form embed code
- Google Form URL for baker registration
- Custom text content (heading, description, button text)
- Background image selection
- Color scheme selection
- Layout positioning options

## Data Models

### Theme Settings Schema Extension

```json
{
  "name": "Coming Soon Page",
  "settings": [
    {
      "type": "checkbox",
      "id": "coming_soon_enabled",
      "label": "Enable Coming Soon Mode",
      "default": false
    },
    {
      "type": "text",
      "id": "coming_soon_heading",
      "label": "Main Heading",
      "default": "We're Coming Soon!"
    },
    {
      "type": "richtext",
      "id": "coming_soon_description",
      "label": "Description Text",
      "default": "<p>We're working hard to bring you something amazing. Stay tuned!</p>"
    },
    {
      "type": "textarea",
      "id": "hubspot_embed_code",
      "label": "HubSpot Form Embed Code",
      "info": "Paste your HubSpot form embed code here"
    },
    {
      "type": "url",
      "id": "baker_registration_url",
      "label": "Baker Registration Google Form URL"
    },
    {
      "type": "text",
      "id": "baker_button_text",
      "label": "Baker Registration Button Text",
      "default": "Baker Registration"
    },
    {
      "type": "text",
      "id": "password_link_text",
      "label": "Password Page Link Text",
      "default": "Site Access"
    },
    {
      "type": "image_picker",
      "id": "coming_soon_background",
      "label": "Background Image"
    }
  ]
}
```

### Section Schema Structure

The coming-soon-page section will have a flexible block-based structure similar to the email-signup-banner:

- **Heading Block**: Customizable main heading
- **Description Block**: Rich text description
- **HubSpot Form Block**: Container for HubSpot integration
- **Baker Registration Block**: Button for Google Form
- **Password Link Block**: Discreet access link

## Error Handling

### HubSpot Integration Errors
- **Invalid Embed Code**: Display fallback message with contact information
- **Form Load Failure**: Graceful degradation to email contact
- **Submission Errors**: Clear error messaging with retry options

### Google Form Integration Errors
- **Invalid URL**: Button disabled with error message in theme editor
- **Form Unavailable**: Fallback to alternative contact method

### General Error Scenarios
- **Missing Configuration**: Default placeholder content with setup instructions
- **Asset Loading Failures**: Progressive enhancement approach
- **Mobile Compatibility**: Responsive design with mobile-first approach

## Testing Strategy

### Functional Testing
1. **Coming Soon Mode Activation**
   - Verify template switching works correctly
   - Test theme setting toggle functionality
   - Confirm password page access remains available

2. **HubSpot Integration Testing**
   - Test form embedding with various HubSpot form types
   - Verify form submission and success handling
   - Test error scenarios and fallback behavior

3. **Baker Registration Testing**
   - Verify Google Form link opens correctly
   - Test button functionality across browsers
   - Confirm new tab/window behavior

4. **Password Page Access Testing**
   - Verify link leads to correct password page
   - Test password functionality remains intact
   - Confirm link visibility and accessibility

### Cross-Browser Testing
- Chrome, Firefox, Safari, Edge
- Mobile browsers (iOS Safari, Chrome Mobile)
- Test form functionality across all browsers

### Responsive Design Testing
- Mobile devices (320px - 768px)
- Tablets (768px - 1024px)
- Desktop (1024px+)
- Test all interactive elements at different screen sizes

### Performance Testing
- Page load speed with HubSpot integration
- Image optimization for background images
- JavaScript loading and execution timing

### Accessibility Testing
- Keyboard navigation for all interactive elements
- Screen reader compatibility
- Color contrast compliance
- Focus management and ARIA labels

### Integration Testing
- HubSpot form submission end-to-end
- Google Form accessibility and functionality
- Shopify password page integration
- Theme customizer setting updates

## Implementation Phases

### Phase 1: Core Template and Section
- Create coming-soon.json template
- Build basic coming-soon-page.liquid section
- Implement theme settings integration
- Add basic styling and responsive design

### Phase 2: HubSpot Integration
- Implement HubSpot form embedding
- Add form styling and error handling
- Test submission functionality
- Add success/error messaging

### Phase 3: Additional Features
- Implement baker registration button
- Add password page link
- Enhance styling and animations
- Add advanced customization options

### Phase 4: Testing and Optimization
- Comprehensive cross-browser testing
- Performance optimization
- Accessibility improvements
- Documentation and user guide creation