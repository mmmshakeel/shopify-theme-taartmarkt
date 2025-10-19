# Implementation Plan

- [x] 1. Set up core template structure and theme settings
  - Create the coming-soon.json template file with basic structure
  - Add coming soon page settings group to settings_schema.json with all configuration options
  - Create basic liquid template structure for the coming soon section
  - _Requirements: 1.1, 5.1, 5.2_

- [x] 2. Implement coming soon page section with responsive design
  - Create sections/coming-soon-page.liquid with block-based structure similar to email-signup-banner
  - Implement responsive CSS styling with mobile-first approach
  - Add background image support with overlay opacity controls
  - Add content positioning options (9-position grid system)
  - _Requirements: 1.2, 1.3, 1.4_

- [x] 3. Implement HubSpot form integration block
  - Create HubSpot form block type in the coming soon section schema
  - Add embed code rendering with proper sanitization and error handling
  - Implement fallback messaging for invalid or missing embed codes
  - Style the HubSpot form container to match theme design
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5_

- [x] 4. Implement baker registration button functionality
  - Create baker registration block type with configurable button text and URL
  - Add Google Form URL validation and button state management
  - Implement button styling consistent with theme design system
  - Add target="_blank" and proper accessibility attributes for external link
  - _Requirements: 3.1, 3.2, 3.3, 3.4_

- [ ] 5. Implement password page access link
  - Create password link block type with configurable link text
  - Add subtle styling for discreet placement in the layout
  - Implement proper routing to Shopify's native password page (/password)
  - _Requirements: 4.1, 4.2, 4.3, 4.4_

- [x] 6. Implement coming soon mode activation system
  - Create liquid logic to detect coming soon mode setting in layout/theme.liquid
  - Add template redirection mechanism to redirect to coming soon page when enabled
  - Implement bypass logic for admin users and password page access
  - Add proper meta tags and SEO considerations for coming soon state
  - _Requirements: 1.1, 4.2, 4.3_

- [ ] 7. Add error handling and validation improvements
  - Implement better error handling for HubSpot form loading failures
  - Add validation and error states for invalid Google Form URLs
  - Create fallback content for missing or invalid configuration
  - Add user-friendly error messages with setup guidance
  - _Requirements: 2.5, 3.1, 5.1_

- [ ] 8. Add accessibility and performance enhancements
  - Implement proper heading hierarchy and semantic HTML structure
  - Add ARIA labels and descriptions for all interactive elements
  - Ensure keyboard navigation works for all buttons and links
  - Add screen reader support for form states and error messages
  - Implement lazy loading for background images
  - _Requirements: 1.4, 2.1, 3.4, 4.4_