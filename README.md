[Google Maps Email Extractor](https://apify.com/logical_scrapers/google-maps-email-extractor?fpr=data)

# Google Maps Contact & Social Media Extractor

Extract business contact information including email addresses and social media profiles from Google Maps listings. This actor helps you build comprehensive contact databases for lead generation, market research, and business outreach.

## Purpose

This actor helps you:

- Find businesses in specific locations
- Extract comprehensive business details
- Discover contact information including email addresses
- Build targeted contact lists efficiently

## Features

- **Targeted Search**: Search businesses by type and location
- **Rich Data Extraction**:

- Business name and category
- Rating and number of reviews
- Complete address
- Phone numbers
- Website URLs
- Operating hours
- Business status
- Booking links
- Google Maps URL
- **Contact Information Discovery**:

- Email addresses from websites
- Social media profiles:

- Facebook
- Instagram
- Twitter/X
- LinkedIn
- YouTube
- TikTok
- Pinterest
- Smart contact page analysis
- **Performance Optimized**:

- Parallel processing
- Duplicate prevention
- Smart rate limiting
- Memory efficient

## Input Configuration

```
{
    "searchTerms": ["restaurants"],     // List of business types or keywords to search for
    "location": "New York",           // Location to search within (e.g., "New York, NY")
    "maxResults": 100                 // Maximum number of results to collect (default: 10)
}
```

### Input Parameters

| Field | Type | Description |
| --- | --- | --- |
| `searchTerms` | array | List of business types or keywords to search for |
| `location` | string | Location to search within (e.g., "New York, NY") |
| `maxResults` | number | Maximum number of results to collect (default: 10) |

## Output Format

The actor saves results to the default dataset in the following format:

```
{
    name: string | null;              // Business name
    category: string | null;          // Business category
    rating: number | null;            // Rating (0-5)
    totalReviews: number | null;      // Number of reviews
    address: string | null;           // Full address
    phone: string | null;             // Phone number
    website: string | null;           // Website URL
    businessStatus: string | null;    // Operating status
    hours: {                          // Operating hours
        open: string;
        close: string;
    } | null;
    bookingLinks: string | null;      // Booking/reservation links
    googleMapsUrl: string;            // Google Maps listing URL
    emails: string[];                 // Discovered email addresses
    socialLinks: {                    // Social media profiles
        facebook?: string;            // Facebook profile URL
        instagram?: string;           // Instagram profile URL
        twitter?: string;             // Twitter/X profile URL
        linkedin?: string;            // LinkedIn company/profile URL
        youtube?: string;             // YouTube channel URL
        tiktok?: string;             // TikTok profile URL
        pinterest?: string;           // Pinterest profile URL
    };
}
```

## Performance Optimization

- Uses browser pooling
- Implements smart delays
- Efficiently manages memory usage
- Handles pagination automatically

## Tips

1. Start with a small `maxResults` to test the output
2. Use specific search terms for better targeting
3. Include country/city in location for precise targeting
4. Monitor memory usage for large extractions

## Support

- For technical questions, contact the developer through email at [coredev.dan@gmail.com](mailto:coredev.dan@gmail.com)
- For issues and feature requests, create an issue in the Isuues section