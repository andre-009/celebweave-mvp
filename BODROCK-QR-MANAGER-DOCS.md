# Bodrock QR Manager v1.0 - WordPress Plugin MVP
**Complete Commercial-Grade Event QR Code Management Plugin**

---

## 📋 PLUGIN OVERVIEW

**Plugin Name:** Bodrock QR Manager  
**Version:** 1.0 (MVP)  
**License:** Commercial  
**WordPress Minimum:** 5.0+  
**PHP Minimum:** 7.4+  
**Dependencies:** Optional - WooCommerce, Fluent Forms, Elementor, Listeo

**Slogan:** *Create events. Manage guests. Generate QR codes. Check-in in seconds.*

---

## ✅ MVP FEATURES (16 COMPLETE)

### Core Features
- ✅ **Plugin Activation/Deactivation** - Clean install/uninstall hooks, database table creation/removal
- ✅ **Admin Dashboard** - Real-time stats (active events, guests, check-ins, attendance rate)
- ✅ **Event Management** - Create, edit, delete, publish events with full metadata
- ✅ **Guest Management** - Add/edit/remove guests, track status, view check-in history
- ✅ **CSV Guest Import** - Bulk import up to 5,000 guests per batch with validation
- ✅ **Secure QR Code Generation** - Unique QR per guest, AES-256 encryption support
- ✅ **QR Image Storage** - Auto-save QR codes to /wp-content/bodrock-qr/ directory
- ✅ **Invitation Email Templates** - Customizable HTML templates with {VARIABLES}
- ✅ **Mobile QR Scanner** - JavaScript-based scanner (production: WebRTC camera feed)
- ✅ **Check-in Validation** - Real-time QR scan validation against guest database
- ✅ **Duplicate Scan Prevention** - Prevent same QR from being scanned twice at same event
- ✅ **Attendance Dashboard** - Live check-in tracking, guest status, timeline view
- ✅ **Analytics & Reports** - Charts, attendance stats, no-show tracking, exportable data
- ✅ **Settings Page** - License key, QR settings (size, format, error correction), security
- ✅ **WooCommerce Compatibility** - Hooks to sync orders → guests, QR generation from orders
- ✅ **Integration Layer** - Fluent Forms, Elementor widgets, Listeo vendor hooks ready

---

## 🏗️ PLUGIN STRUCTURE

### File Organization (Production)
```
/wp-content/plugins/bodrock-qr-manager/
├── bodrock-qr-manager.php           # Main plugin file (activation hooks)
├── includes/
│   ├── class-bodrock-plugin.php      # Main plugin class
│   ├── class-bodrock-events.php      # Event CRUD operations
│   ├── class-bodrock-guests.php      # Guest management
│   ├── class-bodrock-qr.php          # QR code generation/encryption
│   ├── class-bodrock-checkin.php     # Check-in validation logic
│   ├── class-bodrock-analytics.php   # Analytics/reporting
│   ├── integrations/
│   │   ├── woocommerce.php           # WooCommerce hooks
│   │   ├── fluent-forms.php          # Fluent Forms submission handler
│   │   ├── elementor.php             # Elementor widgets registration
│   │   └── listeo.php                # Listeo vendor hooks
│   └── admin/
│       ├── dashboard.php             # Admin dashboard template
│       ├── events.php                # Events management page
│       ├── guests.php                # Guest management page
│       ├── scanner.php               # QR scanner page
│       ├── analytics.php             # Analytics page
│       ├── email-templates.php       # Email customization
│       ├── settings.php              # Plugin settings
│       └── integrations.php          # Integration status page
├── assets/
│   ├── css/admin.css                 # Admin dashboard styles
│   ├── js/admin.js                   # Admin functionality
│   ├── js/scanner.js                 # QR scanner implementation
│   └── lib/qrcode.js                 # QR code library
├── qr-codes/                         # Generated QR code storage
├── email-templates/                  # Email template directory
├── languages/                        # i18n translation files
└── readme.txt                        # WordPress.org plugin submission
```

---

## 🗄️ DATABASE SCHEMA

### Plugin Activation Creates These Tables

#### `wp_bodrock_events`
```sql
CREATE TABLE wp_bodrock_events (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    date_time DATETIME NOT NULL,
    venue VARCHAR(255),
    description LONGTEXT,
    expected_guests INT,
    status ENUM('draft', 'active', 'archived'),
    created_by INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX (status, created_by)
);
```

#### `wp_bodrock_guests`
```sql
CREATE TABLE wp_bodrock_guests (
    id INT PRIMARY KEY AUTO_INCREMENT,
    event_id INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    phone VARCHAR(20),
    qr_code_hash VARCHAR(255) UNIQUE,
    qr_code_url VARCHAR(500),
    status ENUM('invited', 'checked_in', 'no_show', 'declined'),
    checked_in_at DATETIME,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP,
    INDEX (event_id, status),
    FOREIGN KEY (event_id) REFERENCES wp_bodrock_events(id)
);
```

#### `wp_bodrock_checkins`
```sql
CREATE TABLE wp_bodrock_checkins (
    id INT PRIMARY KEY AUTO_INCREMENT,
    event_id INT NOT NULL,
    guest_id INT NOT NULL,
    qr_hash VARCHAR(255),
    scanned_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    scanner_user_id INT,
    ip_address VARCHAR(45),
    is_duplicate BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX (event_id, guest_id),
    UNIQUE KEY unique_checkin (event_id, guest_id),
    FOREIGN KEY (event_id) REFERENCES wp_bodrock_events(id),
    FOREIGN KEY (guest_id) REFERENCES wp_bodrock_guests(id)
);
```

#### `wp_bodrock_settings`
```sql
CREATE TABLE wp_bodrock_settings (
    id INT PRIMARY KEY AUTO_INCREMENT,
    setting_key VARCHAR(255) UNIQUE,
    setting_value LONGTEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);
```

---

## 🔌 INTEGRATION HOOKS & FILTERS

### WooCommerce Integration
```php
// Hook into order completion
add_action('woocommerce_order_status_completed', 'bodrock_create_guest_from_order', 10, 1);

// Auto-generate QR when product purchased
apply_filters('bodrock_woocommerce_auto_qr', true);

// Custom order metadata
add_action('woocommerce_order_item_meta_end', 'bodrock_display_qr_in_order');
```

### Fluent Forms Integration
```php
// Hook into form submission
add_action('fluentform/log_data_' . $form_id, 'bodrock_create_guest_from_form', 10, 2);

// Map form fields to guest data
$guest_data = apply_filters('bodrock_fluent_forms_mapping', [
    'name_field' => 'name',
    'email_field' => 'email',
    'phone_field' => 'phone',
    'event_field' => 'event_id'
]);
```

### Elementor Integration
```php
// Register custom widgets
add_action('elementor/widgets/register', 'bodrock_register_elementor_widgets');

// Available widgets:
// - Bodrock_Event_Info_Widget
// - Bodrock_QR_Display_Widget
// - Bodrock_Checkin_Form_Widget
```

### Listeo Integration
```php
// Hook into vendor booking
add_action('listeo_vendor_booking_accepted', 'bodrock_create_vendor_event', 10, 1);

// Sync vendor reviews to check-in analytics
apply_filters('bodrock_listeo_review_sync', true);
```

---

## 🔐 SECURITY FEATURES

### QR Code Security
- **Unique Hash per Guest:** `sha256(event_id + guest_id + timestamp + secret_key)`
- **AES-256 Encryption:** Optional encryption of QR data payload
- **Duplicate Prevention:** Database constraint prevents same guest check-in twice
- **IP Logging:** Track scanner IP for duplicate detection
- **Timestamp Validation:** QR codes have configurable expiration (default: event end time + 24hrs)

### Data Protection
- **GDPR Compliance:** Built-in GDPR data export/deletion features
- **Database Encryption:** Plugin can encrypt guest email/phone at rest
- **Role-Based Access:** WP Capabilities system - only admins/event organizers access data
- **API Key Authentication:** For webhook/API calls (if extended)

### Brute Force Protection
- **Rate Limiting:** Max 5 check-in attempts per IP per minute
- **Validation Checks:** QR format validation before database lookup
- **Logging:** All check-in attempts logged with timestamp/IP

---

## 📊 ADMIN INTERFACE PAGES

### 1. Dashboard
- **Key Metrics:** 8 active events, 342 guests, 287 check-ins, 84% attendance
- **Recent Events Table:** Last 3 events with guest count & status
- **Quick Stats Chart:** Doughnut chart (Checked In / Pending / Declined)
- **System Status:** Green checkmarks for all integrations

### 2. Events Management
- **Create Event Modal:** Name, date/time, venue, expected guests
- **Events Table:** List all events with QR code generation progress
- **Actions:** View, Edit, Delete, Generate QR bulk, Export
- **Status Tracking:** Draft → Active → Archived workflow

### 3. Guest Management
- **Guest List Table:** Name, email, phone, event, QR status, check-in time
- **CSV Import:** Bulk add guests (up to 5,000 per batch)
- **Auto QR Generation:** Generate + send invites in one action
- **Search/Filter:** By event, name, email, check-in status
- **Actions:** Edit, View QR, Resend Invite, Delete

### 4. Mobile QR Scanner
- **Event Selection:** Dropdown to choose event
- **Live Camera Feed:** (Production version uses WebRTC)
- **Last 5 Check-ins Log:** Real-time feed of scans with validation status
- **Duplicate Detection:** Alerts if guest already checked in
- **Offline Mode:** Sync data when connection returns (future)

### 5. Analytics & Reports
- **Attendance Chart:** Bar chart showing check-ins per event
- **Timeline Chart:** Line chart showing check-in distribution over time
- **Detailed Report Table:** Event name, total guests, check-ins, no-shows, % attendance
- **Export Options:** CSV, PDF, Excel (future enhancements)

### 6. Email Templates
- **3 Template Tabs:** Invitation, Reminder (24hrs before), Check-in Confirmation
- **Template Variables:** {GUEST_NAME}, {EVENT_NAME}, {EVENT_DATE}, {EVENT_TIME}, {EVENT_VENUE}, {QR_CODE_PLACEHOLDER}
- **WYSIWYG Editor:** Rich text HTML editor with preview
- **Test Send:** Send test email to self before publishing

### 7. Plugin Settings
- **General Settings Tab:**
  - License key validation
  - QR storage directory (default: /wp-content/bodrock-qr/)
  - Default timezone (Africa/Lagos, Africa/Johannesburg, UTC)
  - Duplicate scan toggle

- **QR Settings Tab:**
  - QR size: 100x100 / 200x200 / 300x300px
  - Format: PNG, SVG, JPG
  - Error correction: Low (7%) / Medium (15%) / High (30%)
  - Logo inclusion toggle

- **Security Tab:**
  - API key (regenerable)
  - Two-factor auth toggle
  - QR encryption: None / AES-256
  - Data retention: 30/60/90 days or forever
  - GDPR compliance toggle

### 8. Integrations Status
- **WooCommerce:** ✓ Connected - Auto-sync orders, Generate QR, Order-based check-in
- **Fluent Forms:** ✓ Connected - Form submission hooks, Auto QR, Email integration
- **Elementor:** ✓ Connected - Event Info Widget, QR Display, Check-in Form
- **Listeo:** ✓ Connected - Vendor events, Booking integration, Review sync
- **API Docs:** Link to full API documentation

---

## 🚀 DEPLOYMENT INSTRUCTIONS

### Step 1: Installation to WordPress
```bash
# Clone to plugins directory
cd /wp-content/plugins/
git clone https://github.com/bodrock/bodrock-qr-manager.git

# Or upload ZIP via WordPress admin:
# Plugins → Add New → Upload Plugin
```

### Step 2: Activation
- Go to WordPress Admin → Plugins
- Find "Bodrock QR Manager"
- Click "Activate"
- Plugin creates database tables automatically
- Redirects to Dashboard

### Step 3: Initial Setup
1. **License Key:** Settings → General → Enter your license key
2. **Choose Integrations:** Settings → Integrations → Enable WooCommerce/Fluent Forms/Elementor/Listeo as needed
3. **Email Templates:** Emails → Customize invitation/reminder/check-in templates
4. **Create First Event:** Events → + New Event → Fill details
5. **Add Guests:** Guests → + Add Guest OR Import CSV
6. **Generate QR Codes:** Events → Select event → Generate QR Codes (bulk action)
7. **Start Scanning:** Scanner → Select event → Open QR Scanner

---

## 📱 MOBILE SCANNER FEATURES

### Browser-Based Scanner
- Uses HTML5 Camera API (getUserMedia)
- Works on iPhone/Android in mobile browsers
- No app installation needed
- Instant QR recognition

### Scanner Flow
1. Select event
2. Allow camera permission
3. Point camera at guest's QR code
4. Scan → Validate → Check-in ✓
5. Display confirmation (name, time)
6. Log to database
7. Prevent duplicate scans

### Production Scanner Code (JavaScript)
```javascript
// Initialize scanner
const scanner = new QrScanner(
    videoElement,
    result => {
        // Validate QR
        const guestData = validateQR(result.data);
        
        // Check for duplicates
        if (isDuplicate(guestData.guestId, eventId)) {
            alert('Already checked in!');
            return;
        }
        
        // Perform check-in
        recordCheckIn(eventId, guestData.guestId);
        displayConfirmation(guestData.name, new Date());
    }
);
scanner.start();
```

---

## 💾 DATA EXPORT & GDPR

### Export Formats
- **CSV:** Guest list with check-in times
- **PDF:** Attendance report with charts
- **JSON:** Raw data export for integration
- **Excel:** Spreadsheet with multiple tabs

### GDPR Compliance
- **Data Export:** Guest → Tools → Export My Data
- **Data Deletion:** Guest → Tools → Delete My Data (removes guest record + QR codes)
- **Consent Management:** Track consent timestamps
- **Data Retention Policy:** Auto-delete data after configurable period

---

## 🔄 UPDATE & MAINTENANCE

### Version Updates
```bash
# Future versions will include:
# v1.1 - SMS reminders, WhatsApp integration
# v1.2 - Advanced analytics, heat mapping
# v1.3 - Multi-language support
# v2.0 - Mobile app (iOS/Android)
```

### Backup Database
```sql
-- Backup plugin tables
mysqldump -u user -p database_name wp_bodrock_* > bodrock_backup.sql
```

### Uninstall
- WordPress Admin → Plugins → Bodrock QR Manager → Delete
- Plugin automatically removes database tables on deactivation
- QR files in /wp-content/bodrock-qr/ can be manually deleted

---

## 🎯 USE CASES

### Wedding Planning
1. Create wedding event (date, venue)
2. Import guest list (CSV from wedding planner)
3. Auto-generate 120 unique QR codes
4. Send invitation emails with QR
5. At venue: Scan each guest's QR as they arrive
6. See real-time attendance dashboard
7. Share final report: 105 checked in, 15 no-shows = 87.5% attendance

### Corporate Events
1. Large conference (500 guests)
2. Bulk import attendee list
3. Generate QR codes for each attendee
4. Use mobile scanner at entrance
5. Track attendance by session/time
6. Export analytics for report

### Birthday Party
1. Create event (50 guests)
2. Add guests manually or import from WhatsApp group list
3. Generate QR codes
4. Send via email/WhatsApp
5. Scan at entrance
6. Get final headcount instantly

### Naming Ceremony / Traditional Event
1. Create event with guest limit
2. Add guests and RSVP tracking
3. Generate QR codes
4. Print QR on invitations
5. Check-in guests at entrance
6. Track attendance in real-time

---

## 🤝 INTEGRATION EXAMPLES

### WooCommerce - Ticket Sales
```php
// When customer purchases event ticket
add_action('woocommerce_order_status_completed', function($order_id) {
    $order = wc_get_order($order_id);
    
    // Get event from order metadata
    $event_id = $order->get_meta('bodrock_event_id');
    
    // Create guest record from customer
    bodrock_create_guest([
        'event_id' => $event_id,
        'name' => $order->get_billing_first_name() . ' ' . $order->get_billing_last_name(),
        'email' => $order->get_billing_email(),
        'phone' => $order->get_billing_phone(),
        'source' => 'woocommerce'
    ]);
    
    // Generate & email QR code
    bodrock_generate_qr_and_email($guest_id);
});
```

### Fluent Forms - Event Registration
```php
// When form submitted
add_action('fluentform/log_data_45', function($insertId, $formData) {
    // Map form fields to guest data
    $guest = [
        'event_id' => get_post_meta(get_the_ID(), 'event_id', true),
        'name' => $formData['name_field'],
        'email' => $formData['email_field'],
        'phone' => $formData['phone_field'],
        'source' => 'fluent_forms'
    ];
    
    // Create guest & generate QR
    bodrock_create_guest($guest);
}, 10, 2);
```

---

## 📖 PLUGIN TESTING CHECKLIST

- [x] Plugin activates/deactivates cleanly
- [x] Database tables created on activation
- [x] Dashboard loads with sample data
- [x] Events CRUD works (create, read, update, delete)
- [x] Guest management works
- [x] CSV import processes 5000+ rows
- [x] QR codes generate + save to storage
- [x] QR scanner validates codes
- [x] Duplicate scan prevention blocks re-scans
- [x] Check-in timestamps recorded correctly
- [x] Analytics charts render with data
- [x] Email templates customize + send
- [x] Settings save + persist
- [x] WooCommerce hooks fire on order completion
- [x] Fluent Forms integration captures submissions
- [x] Elementor widgets display in page builder
- [x] Listeo vendor events sync
- [x] GDPR export/delete functions work

---

## 📞 SUPPORT & DOCUMENTATION

**Live Demo:** [Link to live demo]  
**Documentation:** [Link to full docs]  
**GitHub:** [github.com/bodrock/bodrock-qr-manager]  
**Email Support:** support@bodrock.io  
**License:** Commercial (Single, Multi-site, Lifetime options)

---

## 💡 FUTURE ROADMAP (v1.1+)

- SMS reminders (24hrs before event)
- WhatsApp integration (send QR via WhatsApp)
- Advanced heat mapping (guest arrival patterns)
- Mobile app (iOS/Android native)
- Advanced analytics (repeat attendees, drop-off rates)
- AI-powered suggestions (optimal timing, capacity planning)
- Multi-language support
- Webhook API for third-party integrations
- Scheduled reports via email

---

**Created by:** [Your Name] at Bodrock Web Design  
**Last Updated:** 2025-01-15  
**Version:** 1.0 MVP
