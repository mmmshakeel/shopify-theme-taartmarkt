# Coming Soon Mode Activation System

## Overview

The coming soon mode activation system has been implemented in `layout/theme.liquid` to automatically redirect visitors to a coming soon page when the site is under construction, while allowing authorized access for administrators and specific use cases.

## How It Works

### 1. Detection Logic

The system checks the `settings.coming_soon_enabled` setting to determine if coming soon mode is active. This setting can be configured in the Shopify theme editor under "Coming Soon Page" settings.

### 2. Bypass Conditions

The system automatically bypasses coming soon mode for:

- **Password page access**: `/password` routes and password templates
- **Admin access**: When in Shopify design mode or preview mode
- **Admin users**: Customers tagged with 'admin' or 'staff'
- **API endpoints**: `/api/` and `/webhooks/` routes
- **Admin panel**: `/admin` routes
- **Cart/Checkout**: Direct cart and checkout access
- **Already on coming soon page**: Prevents redirect loops

### 3. Redirection Mechanism

When coming soon mode is active and no bypass conditions are met:

- **JavaScript redirection**: Primary method for modern browsers
- **Meta refresh fallback**: For browsers with JavaScript disabled
- **Automatic page detection**: Looks for pages with 'coming-soon' handle or template

### 4. SEO and Meta Tags

When in coming soon mode, the system automatically:

- Sets appropriate meta tags (noindex, nofollow)
- Adds Open Graph and Twitter Card meta tags
- Includes structured data for search engines
- Uses coming soon specific title and description

### 5. Template Integration

The system works with:

- `templates/coming-soon.json`: Main coming soon template
- `templates/page.coming-soon.json`: Page-based coming soon template
- Automatic fallback to `/pages/coming-soon` if no specific page is found

## Configuration

### Theme Settings

Configure the following in Shopify theme editor:

- `coming_soon_enabled`: Enable/disable coming soon mode
- `coming_soon_heading`: Main heading text
- `coming_soon_description`: Description text
- `coming_soon_background`: Background image
- `coming_soon_overlay_opacity`: Background overlay opacity
- `coming_soon_color_scheme`: Color scheme selection

### Debug Mode

When in Shopify design mode, the system logs debug information to the browser console:

- Coming soon mode status
- Bypass conditions status
- Current template and request path

## Testing

### Enable Coming Soon Mode

1. Go to Shopify Admin → Online Store → Themes
2. Click "Customize" on your active theme
3. Navigate to "Theme settings" → "Coming Soon Page"
4. Check "Enable Coming Soon Mode"
5. Configure other settings as needed
6. Save changes

### Test Bypass Conditions

- **Admin access**: Access site while logged into Shopify admin
- **Password page**: Visit `/password` directly
- **Design mode**: Access through theme editor preview

### Verify Redirection

1. Enable coming soon mode
2. Open site in incognito/private browsing mode
3. Verify redirection to coming soon page
4. Check that bypass conditions work correctly

## Troubleshooting

### Common Issues

1. **Redirect loop**: Check that coming soon page exists and is accessible
2. **No redirection**: Verify `coming_soon_enabled` setting is true
3. **Admin can't access**: Check bypass conditions and admin detection

### Debug Steps

1. Enable design mode to see console logs
2. Check browser network tab for redirect responses
3. Verify coming soon page template exists
4. Test with different user types (admin vs. regular visitor)

## Security Considerations

- Password page access is always preserved
- Admin panel access is never blocked
- API endpoints remain accessible for integrations
- Cart/checkout functionality is preserved for existing sessions