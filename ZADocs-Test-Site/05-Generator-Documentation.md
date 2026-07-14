# ZADocs Test Site — Document Generator Documentation

**Purpose:** Technical specification for the document generator system

**Architecture:** Mu-plugin + PHPWord + Elementor HTML widgets

---

## System Overview

```
User Flow:
1. User visits template page (e.g., /employment-contract-template/)
2. Clicks "Preview & Download" button
3. Lands on generator page (e.g., /employment-contract-preview/)
4. Sees document preview with fillable fields [Full Name], [Company], etc.
5. Clicks on fields to edit (contenteditable or input overlays)
6. Clicks "Download DOCX" → AJAX request to mu-plugin
7. PHPWord generates DOCX with user's data
8. File downloads to user's device
9. OR clicks "Print" → Browser print dialog with custom CSS
```

---

## File Structure

```
/wp-content/
├── mu-plugins/
│   └── zadocs-generator.php (main generator logic)
├── zadocs-templates/
│   ├── employment-contract.docx
│   ├── lease-agreement.docx
│   ├── invoice-template.docx
│   └── ... (101 templates)
└── uploads/
    └── zadocs-generated/ (temporary generated files)
        └── [timestamp]_[slug].docx (auto-deleted after 1 hour)
```

---

## Mu-Plugin: zadocs-generator.php

### Core Functions

```php
<?php
/**
 * ZADocs Document Generator
 * Generates DOCX files from templates with user-filled data
 * 
 * Location: /wp-content/mu-plugins/zadocs-generator.php
 * Version: 1.0.0
 */

// Prevent direct access
if (!defined('ABSPATH')) exit;

/**
 * Register shortcode for generator preview
 * Usage: [zadocs_generator template="employment-contract"]
 */
function zadocs_generator_shortcode($atts) {
    $atts = shortcode_atts([
        'template' => ''
    ], $atts);
    
    if (empty($atts['template'])) {
        return '<p>Error: Template not specified</p>';
    }
    
    $template_path = WP_CONTENT_DIR . '/zadocs-templates/' . $atts['template'] . '.docx';
    
    if (!file_exists($template_path)) {
        return '<p>Template not found</p>';
    }
    
    // Load template and extract fields
    $fields = zadocs_extract_fields($template_path);
    
    // Generate preview HTML
    ob_start();
    include __DIR__ . '/generator-preview.php';
    return ob_get_clean();
}
add_shortcode('zadocs_generator', 'zadocs_generator_shortcode');

/**
 * Extract placeholder fields from DOCX template
 * Pattern: [Full Name], [Company], [Date], etc.
 */
function zadocs_extract_fields($template_path) {
    // Use ZipArchive to read DOCX (DOCX is a ZIP file)
    $zip = new ZipArchive;
    $zip->open($template_path);
    
    // Read document.xml
    $content = $zip->getFromName('word/document.xml');
    $zip->close();
    
    // Extract all [Placeholder] patterns
    preg_match_all('/\[([^\]]+)\]/', $content, $matches);
    
    // Return unique fields
    return array_unique($matches[1]);
}

/**
 * Generate DOCX with user data
 * Called via AJAX
 */
function zadocs_generate_docx() {
    // Verify nonce
    check_ajax_referer('zadocs_generator', 'nonce');
    
    $template = sanitize_text_field($_POST['template']);
    $data = $_POST['data']; // Array of field => value
    
    $template_path = WP_CONTENT_DIR . '/zadocs-templates/' . $template . '.docx';
    
    if (!file_exists($template_path)) {
        wp_send_json_error(['message' => 'Template not found']);
    }
    
    // Use PHPWord to load and modify template
    require_once ABSPATH . 'wp-includes/class-phpword.php';
    
    $phpWord = \PhpWord\IOFactory::load($template_path, 'Word2007');
    
    // Replace placeholders
    foreach ($phpWord->getSections() as $section) {
        foreach ($section->getElements() as $element) {
            if (method_exists($element, 'getText')) {
                $text = $element->getText();
                foreach ($data as $field => $value) {
                    $text = str_replace('[' . $field . ']', $value, $text);
                }
                $element->setText($text);
            }
        }
    }
    
    // Generate filename
    $filename = sanitize_title($template) . '_' . time() . '.docx';
    $output_path = WP_CONTENT_DIR . '/uploads/zadocs-generated/' . $filename;
    
    // Save generated file
    $objWriter = \PhpWord\IOFactory::createWriter($phpWord, 'Word2007');
    $objWriter->save($output_path);
    
    // Return download URL
    wp_send_json_success([
        'url' => content_url('uploads/zadocs-generated/' . $filename)
    ]);
}
add_action('wp_ajax_zadocs_generate_docx', 'zadocs_generate_docx');
add_action('wp_ajax_nopriv_zadocs_generate_docx', 'zadocs_generate_docx');

/**
 * Serve generated file for download
 */
function zadocs_serve_generated_file() {
    $filename = sanitize_text_field($_GET['file']);
    $file_path = WP_CONTENT_DIR . '/uploads/zadocs-generated/' . $filename;
    
    if (!file_exists($file_path)) {
        wp_die('File not found');
    }
    
    // Force download
    header('Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document');
    header('Content-Disposition: attachment; filename="' . basename($file_path) . '"');
    header('Content-Length: ' . filesize($file_path));
    readfile($file_path);
    exit;
}
add_action('init', 'zadocs_serve_generated_file');
```

---

## Generator Preview Template

**File:** `/wp-content/mu-plugins/generator-preview.php`

```php
<?php
/**
 * Generator Preview HTML
 * Displays document with editable fields
 */

$fields = $fields; // From shortcode
$template = $atts['template'];
?>

<div class="zadocs-generator" data-template="<?php echo esc_attr($template); ?>">
    
    <!-- Control Panel -->
    <div class="generator-controls">
        <h3>Document Controls</h3>
        
        <button class="btn-download" onclick="zadocsDownload()">
            📄 Download DOCX
        </button>
        
        <button class="btn-print" onclick="window.print()">
            🖨️ Print Document
        </button>
        
        <button class="btn-reset" onclick="zadocsReset()">
            🔄 Reset
        </button>
        
        <a href="/<?php echo get_post_field('post_name', get_the_ID()); ?>/" class="btn-browse">
            ← Browse More Templates
        </a>
    </div>
    
    <!-- Document Preview -->
    <div class="doc-preview" id="docPreview">
        <!-- This will be populated via JavaScript -->
        <!-- For now, show loading state -->
        <div class="loading">Loading document preview...</div>
    </div>
    
</div>

<script>
// Field data
const zadocsFields = <?php echo json_encode($fields); ?>;
const zadocsTemplate = '<?php echo esc_js($template); ?>';

// Initialize preview
document.addEventListener('DOMContentLoaded', function() {
    loadPreview();
});

// Load document preview
function loadPreview() {
    // Fetch template content (stored as HTML in template post)
    fetch('/wp-json/zadocs/v1/template/' + zadocsTemplate)
        .then(res => res.json())
        .then(data => {
            document.getElementById('docPreview').innerHTML = data.content;
            makeFieldsEditable();
        });
}

// Make [Placeholder] fields editable
function makeFieldsEditable() {
    const placeholders = document.querySelectorAll('.placeholder');
    placeholders.forEach(el => {
        el.contentEditable = true;
        el.classList.add('doc-field');
        el.title = 'Click to edit';
    });
}

// Collect user data
function collectData() {
    const data = {};
    const fields = document.querySelectorAll('.doc-field');
    fields.forEach(field => {
        const fieldName = field.getAttribute('data-field');
        data[fieldName] = field.innerText;
    });
    return data;
}

// Download DOCX
function zadocsDownload() {
    const data = collectData();
    
    fetch('/wp-admin/admin-ajax.php', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: new URLSearchParams({
            action: 'zadocs_generate_docx',
            nonce: '<?php echo wp_create_nonce('zadocs_generator'); ?>',
            template: zadocsTemplate,
            data: JSON.stringify(data)
        })
    })
    .then(res => res.json())
    .then(response => {
        if (response.success) {
            // Trigger download
            const a = document.createElement('a');
            a.href = response.data.url;
            a.download = zadocsTemplate + '.docx';
            a.click();
        } else {
            alert('Error: ' + response.data.message);
        }
    });
}

// Reset fields
function zadocsReset() {
    const fields = document.querySelectorAll('.doc-field');
    fields.forEach(field => {
        field.innerText = '[' + field.getAttribute('data-field') + ']';
    });
}
</script>

<style>
.zadocs-generator {
    max-width: 1200px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 300px 1fr;
    gap: 32px;
}

.generator-controls {
    background: #F7F9FC;
    padding: 24px;
    border-radius: 8px;
    height: fit-content;
    position: sticky;
    top: 100px;
}

.generator-controls h3 {
    margin-bottom: 16px;
}

.generator-controls button,
.generator-controls a {
    display: block;
    width: 100%;
    margin-bottom: 12px;
    padding: 12px;
    border-radius: 6px;
    text-align: center;
    text-decoration: none;
    font-weight: 600;
}

.btn-download {
    background: #0057B8;
    color: #FFFFFF;
    border: none;
}

.btn-print {
    background: #28a745;
    color: #FFFFFF;
    border: none;
}

.btn-reset {
    background: #6c757d;
    color: #FFFFFF;
    border: none;
}

.btn-browse {
    background: transparent;
    color: #0057B8;
    border: 2px solid #0057B8;
}

.doc-preview {
    background: #FFFFFF;
    border: 1px solid #E2E8F0;
    border-radius: 8px;
    padding: 40px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    min-height: 800px;
}

.doc-field {
    background: #F0F4F8;
    padding: 4px 8px;
    border-radius: 4px;
    color: #0057B8;
    font-weight: 600;
    cursor: text;
}

.doc-field:focus {
    outline: 2px solid #0057B8;
    background: #E8F0F8;
}

@media (max-width: 767px) {
    .zadocs-generator {
        grid-template-columns: 1fr;
    }
    
    .generator-controls {
        position: relative;
        top: 0;
    }
}
</style>
```

---

## Template Creation Guide

### Step 1: Create DOCX Template

1. Open Microsoft Word (or LibreOffice Writer)
2. Create document with placeholders: `[Full Name]`, `[Company]`, `[Date]`
3. Save as `.docx` format
4. Upload to `/wp-content/zadocs-templates/`

### Step 2: Create WordPress Post

1. WordPress Admin → Posts → Add New
2. Title: "Employment Contract Template"
3. Content: SEO content (overview, when to use, FAQs)
4. Custom Field: `docx_file_url` = `/wp-content/zadocs-templates/employment-contract.docx`
5. Publish

### Step 3: Create Generator Page

1. WordPress Admin → Pages → Add New
2. Title: "Employment Contract Preview"
3. Edit with Elementor
4. Add HTML Widget
5. Insert shortcode: `[zadocs_generator template="employment-contract"]`
6. Publish

### Step 4: Link Template to Generator

In template post content:
- "Preview & Download" button links to: `/employment-contract-preview/`

---

## PHPWord Installation

**Method 1: Composer (Recommended)**

```bash
cd /path/to/wordpress
composer require phpoffice/phpword
```

**Method 2: Manual Upload**

1. Download PHPWord from: https://github.com/PHPOffice/PHPWord/releases
2. Extract to: `/wp-includes/class-phpword.php` (autoload)
3. Or use PHAR file

**Method 3: Mu-Plugin Bundled**

1. Download PHPWord
2. Place in: `/wp-content/mu-plugins/phpword/`
3. Update require path in zadocs-generator.php

---

## REST API Endpoint

**File:** `/wp-content/mu-plugins/zadocs-rest-api.php`

```php
<?php
/**
 * REST API for template content
 */

add_action('rest_api_init', function() {
    register_rest_route('zadocs/v1', '/template/(?P<slug>[a-zA-Z0-9-]+)', [
        'methods' => 'GET',
        'callback' => 'zadocs_get_template_content',
        'permission_callback' => '__return_true'
    ]);
});

function zadocs_get_template_content($request) {
    $slug = $request['slug'];
    
    $post = get_page_by_path($slug, OBJECT, 'post');
    
    if (!$post) {
        return new WP_Error('not_found', 'Template not found', ['status' => 404]);
    }
    
    return [
        'title' => $post->post_title,
        'content' => apply_filters('the_content', $post->post_content),
        'fields' => zadocs_extract_fields(WP_CONTENT_DIR . '/zadocs-templates/' . $slug . '.docx')
    ];
}
```

---

## Security Considerations

### Nonce Verification

All AJAX requests must include nonce:

```javascript
nonce: '<?php echo wp_create_nonce('zadocs_generator'); ?>'
```

### File Validation

```php
// Validate template path
$template = basename($template); // Prevent directory traversal
$template_path = WP_CONTENT_DIR . '/zadocs-templates/' . $template . '.docx';

// Verify file exists and is readable
if (!file_exists($template_path) || !is_readable($template_path)) {
    wp_die('Invalid template');
}
```

### Sanitization

```php
// Sanitize all user input
$data = array_map('sanitize_text_field', $_POST['data']);
$filename = sanitize_title($template) . '_' . time() . '.docx';
```

### File Cleanup

```php
// Auto-delete generated files after 1 hour
function zadocs_cleanup_old_files() {
    $dir = WP_CONTENT_DIR . '/uploads/zadocs-generated/';
    $files = glob($dir . '*.docx');
    $now = time();
    
    foreach ($files as $file) {
        if ($now - filemtime($file) > 3600) {
            unlink($file);
        }
    }
}
add_action('wp_scheduled_delete', 'zadocs_cleanup_old_files');
```

---

## Testing Checklist

### Functional Tests

- [ ] Template loads correctly
- [ ] Fields are editable
- [ ] Download generates DOCX
- [ ] Downloaded DOCX has correct data
- [ ] Print works with custom CSS
- [ ] Reset clears all fields
- [ ] "Browse More Templates" links correctly

### Security Tests

- [ ] Nonce validation works
- [ ] File paths are sanitized
- [ ] No directory traversal possible
- [ ] Generated files are cleaned up

### Performance Tests

- [ ] Generator loads in <2 seconds
- [ ] Download generates in <3 seconds
- [ ] No memory leaks
- [ ] Works with 100+ concurrent users

---

## Troubleshooting

### Issue: Fields Not Editable

**Solution:**
- Check JavaScript console for errors
- Verify `.placeholder` class exists in preview HTML
- Ensure `makeFieldsEditable()` is called

### Issue: Download Fails

**Solution:**
- Check AJAX response in Network tab
- Verify PHPWord is loaded
- Check file permissions on `/uploads/zadocs-generated/`

### Issue: Template Not Found

**Solution:**
- Verify template file exists in `/wp-content/zadocs-templates/`
- Check filename matches shortcode parameter
- Verify file permissions (readable by web server)

---

**Document Version:** 1.0.0  
**Last Updated:** June 10, 2026  
**Next Review:** After generator implementation
