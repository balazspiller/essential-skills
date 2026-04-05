---
name: reddit-poster
description: Create and submit posts to Reddit using old.reddit.com with CSS selectors. Use when asked to post to Reddit, submit a link or text post to a subreddit, or create a new Reddit submission. Reliable browser-based posting using CSS selectors for form fields.
---

# Reddit Poster

Create and submit posts to Reddit subreddits using old.reddit.com interface with CSS selectors. Reliable, stable form filling for both link and text posts.

## Why Use This

- **CSS Selectors**: Stable and reliable (e.g., `textarea[name="title"]`) vs changing ARIA refs
- **old.reddit.com**: Simpler HTML structure, no heavy JavaScript, more automation-friendly
- **Browser-based**: Uses existing Reddit session (you must be logged in)
- **Tested**: Proven working workflow

## When to Use

Use this skill when:
- User asks to "post to Reddit"
- User wants to "submit to r/subreddit"
- User asks to "create a Reddit post"
- User has a link or content to share on Reddit

## Prerequisites

- Must be logged into Reddit in browser
- Browser profile must persist login session
- Use old.reddit.com (not www.reddit.com)

## CSS Selectors (Use These!)

**CRITICAL:** Always use CSS selectors, not ARIA refs. The refs change constantly.

### Text Post Form
```
textarea[name="title"]    - Title field (required)
textarea[name="text"]     - Body text field (optional)
button[name="submit"]     - Submit button
```

### Link Post Form
```
textarea[name="title"]    - Title field (required)
input[name="url"]          - URL field (required)
button[name="submit"]     - Submit button
```

### Tabs
```
a[href="#text"]           - Text post tab
a[href="#link"]            - Link post tab
```

## Posting Workflow

### Step 1: Navigate to Submit Page

```bash
browser open "https://old.reddit.com/r/{subreddit}/submit"
```

### Step 2: Select Post Type (if needed)

For text posts, click the text tab:
```bash
browser act --kind click --selector 'a[href="#text"]'
```

For link posts, click the link tab:
```bash
browser act --kind click --selector 'a[href="#link"]'
```

### Step 3: Fill Form Using CSS Selectors

**For Text Posts:**
```bash
# Fill title
browser act --kind type --selector 'textarea[name="title"]' --text "Your Post Title"

# Fill body (optional)
browser act --kind type --selector 'textarea[name="text"]' --text "Your post content here..."
```

**For Link Posts:**
```bash
# Fill URL
browser act --kind type --selector 'input[name="url"]' --text "https://example.com/article"

# Fill title
browser act --kind type --selector 'textarea[name="title"]' --text "Your Post Title"
```

### Step 4: Submit

```bash
browser act --kind click --selector 'button[name="submit"]'
```

### Step 5: Verify

Navigate to user profile to confirm post was created:
```bash
browser open "https://old.reddit.com/user/{username}/submitted/"
```

Or check the subreddit:
```bash
browser open "https://old.reddit.com/r/{subreddit}/new"
```

## Complete Example: Text Post

```bash
# Navigate to submit page
browser open "https://old.reddit.com/r/EmDashCMS/submit"

# Click text tab (if not already selected)
browser act --kind click --selector 'a[href="#text"]'

# Fill title
browser act --kind type --selector 'textarea[name="title"]' --text "First thoughts on EmDash"

# Fill body text
browser act --kind type --selector 'textarea[name="text"]' --text "Brian Coords takes EmDash for a spin...

Key lessons:
• Frontend freedom
• Better content modeling
• Agent skills

https://example.com/article"

# Submit
browser act --kind click --selector 'button[name="submit"]'

# Verify
browser open "https://old.reddit.com/user/YourUsername/submitted/"
```

## Complete Example: Link Post

```bash
# Navigate to submit page
browser open "https://old.reddit.com/r/EmDashCMS/submit"

# Click link tab (if needed)
browser act --kind click --selector 'a[href="#link"]'

# Fill URL
browser act --kind type --selector 'input[name="url"]' --text "https://example.com/article"

# Fill title
browser act --kind type --selector 'textarea[name="title"]' --text "Interesting article about EmDash"

# Submit
browser act --kind click --selector 'button[name="submit"]'
```

## Important Notes

### Title Requirements
- Maximum 300 characters
- Required field
- Cannot start with "vote up if" (intergalactic law violation)

### Rate Limiting
- Reddit has anti-spam measures
- Wait between submissions if posting multiple times
- Check user profile if post doesn't appear immediately

### Common Issues

**Why ARIA refs don't work:**
- ARIA refs (e.g., `ref=e50`) change every page load
- CSS selectors (e.g., `textarea[name="title"]`) are stable
- **Always use CSS selectors**

**Post not appearing after submission:**
1. Wait 30-60 seconds for processing
2. Check user profile: `/user/{username}/submitted/`
3. Check subreddit /new page: `/r/{subreddit}/new`
4. Verify form was filled correctly before submitting

## Editing Posts

To edit an existing post:

### Step 1: Navigate to Post
```bash
browser open "https://old.reddit.com/r/{subreddit}/comments/{post_id}/{slug}/"
```

### Step 2: Click Edit Link
Use JavaScript to click the edit button:
```bash
browser act --kind evaluate --fn "() => { document.querySelector('a[href*="edit"]').click(); }"
```

Or use CSS selector:
```bash
browser act --kind click --selector 'a[onclick*="edit"]'
```

### Step 3: Update Content Using JavaScript
The most reliable method is using JavaScript to set the textarea value:
```bash
browser act --kind evaluate --fn "() => { document.querySelector('textarea[name=\"text\"]').value = 'Your updated text here...'; return 'updated'; }"
```

**Note:** Use actual line breaks (`\n`) for formatting:
```javascript
"Line 1\n\nLine 2\n* bullet 1\n* bullet 2"
```

### Step 4: Save Changes
```bash
browser act --kind evaluate --fn "() => { document.querySelector('button[type=\"submit\"]').click(); return 'saved'; }"
```

### Complete Edit Example
```bash
# Navigate to post
browser open "https://old.reddit.com/r/EmDashCMS/comments/1scd5iu/first_thoughts_on_emdash/"

# Click edit
browser act --kind evaluate --fn "() => { document.querySelector('a[href*="edit"]').click(); }"

# Update content with proper formatting
browser act --kind evaluate --fn "() => { document.querySelector('textarea[name=\"text\"]').value = 'Brian Coords takes EmDash for a spin...\n\nKey lessons:\n* Point 1\n* Point 2\n* Point 3\n\nMore text...'; return 'updated'; }"

# Save
browser act --kind evaluate --fn "() => { document.querySelector('button[type=\"submit\"]').click(); return 'saved'; }"
```

## Integration with Reddit Data Extractor

Combine with `reddit-data-extractor` skill for full Reddit workflow:

1. **Monitor** subreddit with extractor
2. **Analyze** posts for context
3. **Create** reply or new post with poster
4. **Edit** posts as needed
5. **Verify** changes were saved

---

**Key Takeaways:**
1. **CSS selectors** are stable and reliable
2. **JavaScript evaluate** works best for editing (setting textarea values)
3. **ARIA refs** change constantly - avoid them
4. Use `\n` for line breaks in text content
