<div align="center">

#  FRAPPE PRINT FORMAT
#  ULTIMATE GUIDE

### *The comprehensive handbook for building Print Formats in Frappe / ERPNext*

<p>
  <img src="https://img.shields.io/badge/Frappe-Framework-2490E3?style=for-the-badge&logo=frappe&logoColor=white" alt="Frappe Framework">
  <img src="https://img.shields.io/badge/Jinja-Templates-B41706?style=for-the-badge&logo=jinja&logoColor=white" alt="Jinja">
  <img src="https://img.shields.io/badge/Bootstrap-5.1-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white" alt="Bootstrap">
  <img src="https://img.shields.io/badge/License-MIT-00D26A?style=for-the-badge" alt="License">
</p>

<p>
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black" alt="JavaScript">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" alt="HTML5">
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white" alt="CSS3">
</p>

---

</div>

<br>

## 📑 Table of Contents

<table>
  <tr>
    <td align="center" width="14%">
      <a href="#1-print-format-basics">
        <img src="https://img.shields.io/badge/01-Basics-E34F26?style=for-the-badge" alt="Basics"/><br>
        <sub><b>Structure & Types</b></sub>
      </a>
    </td>
    <td align="center" width="14%">
      <a href="#2-jinja-context--variables">
        <img src="https://img.shields.io/badge/02-Jinja-B41706?style=for-the-badge" alt="Jinja"/><br>
        <sub><b>Context & Variables</b></sub>
      </a>
    </td>
    <td align="center" width="14%">
      <a href="#3-css-rules--print-media">
        <img src="https://img.shields.io/badge/03-CSS-1572B6?style=for-the-badge" alt="CSS"/><br>
        <sub><b>Print Media</b></sub>
      </a>
    </td>
    <td align="center" width="14%">
      <a href="#4-child-tables--loops">
        <img src="https://img.shields.io/badge/04-Tables-27AE60?style=for-the-badge" alt="Tables"/><br>
        <sub><b>Child Tables</b></sub>
      </a>
    </td>
    <td align="center" width="14%">
      <a href="#5-common-mistakes">
        <img src="https://img.shields.io/badge/05-Mistakes-E74C3C?style=for-the-badge" alt="Mistakes"/><br>
        <sub><b>Pitfalls</b></sub>
      </a>
    </td>
    <td align="center" width="14%">
      <a href="#6-full-invoice-example">
        <img src="https://img.shields.io/badge/06-Example-9B59B6?style=for-the-badge" alt="Example"/><br>
        <sub><b>Full Invoice</b></sub>
      </a>
    </td>
    <td align="center" width="14%">
      <a href="#7-advanced-pro-tips">
        <img src="https://img.shields.io/badge/07-Pro_Tips-FF6B6B?style=for-the-badge" alt="Tips"/><br>
        <sub><b>Advanced</b></sub>
      </a>
    </td>
  </tr>
</table>

<br>

---

<div id="1-print-format-basics">

<br>

## 🖨️ 1) Print Format Basics

<img src="https://img.shields.io/badge/Frappe-Print_Format_Doctype-2490E3?style=flat-square&logo=frappe&logoColor=white" alt="Print Format">

A **Print Format** in Frappe/ERPNext controls how documents look when printed or exported as PDF. It's attached to a specific DocType (e.g., Sales Invoice, Purchase Order).

### Print Format Fields

| Field | Content | Notes |
|-------|---------|-------|
| **DocType** | Target doctype | e.g., `Sales Invoice` |
| **HTML** | Jinja HTML Template | Main layout & content |
| **CSS** | Scoped styles | No `<style>` tags needed |
| **Custom Format** | `Yes / No` | Enable to unlock HTML editor |
| **Standard** | `Yes / No` | System default format |
| **Print Format Type** | `Jinja / JS` | Use **Jinja** for most cases |

### Two Types of Print Formats

```
┌─────────────────────────────────────────────────────────┐
│  TYPE 1 — Standard Format (Auto-Generated)              │
│  • Frappe auto-renders all fields                       │
│  • Limited customization                                │
│  • Good for quick internal use                          │
├─────────────────────────────────────────────────────────┤
│  TYPE 2 — Custom Format (HTML + Jinja)         ✅ Best  │
│  • Full control over layout, branding, structure        │
│  • Uses Jinja2 templating engine                        │
│  • Supports CSS and Bootstrap                           │
│  • Recommended for client-facing documents              │
└─────────────────────────────────────────────────────────┘
```

### How to Create

1. Go to: `Settings > Print Format > New`
2. Set **DocType** (e.g., `Sales Invoice`)
3. Enable **Custom Format = Yes**
4. Write your **HTML** using Jinja variables
5. Add your **CSS** in the CSS field
6. Save → Preview via the print icon on any document

> 💡 **Pro Tip:** Use `Print Preview` (Ctrl+P) from any document to test your format live.

</div>

---

<div id="2-jinja-context--variables">

<br>

## 🧩 2) Jinja Context & Variables

<img src="https://img.shields.io/badge/Jinja2-Context_Variables-B41706?style=flat-square&logo=jinja&logoColor=white" alt="Jinja Context">

Frappe automatically passes several variables into the print format context.

### Core Variables

| Variable | Type | Description |
|----------|------|-------------|
| `doc` | Object | The main document (e.g., Sales Invoice) |
| `doc.name` | String | Document ID (e.g., `SINV-0001`) |
| `doc.items` | List | Child table rows |
| `frappe` | Module | Frappe utilities |
| `_` | Function | Translation function |

### Accessing Document Fields

```html
<!-- Basic field -->
<p>{{ doc.customer }}</p>

<!-- Date formatting -->
<p>{{ doc.posting_date }}</p>
<p>{{ frappe.format_date(doc.posting_date) }}</p>

<!-- Currency formatting -->
<p>{{ frappe.format_value(doc.grand_total, {"fieldtype": "Currency"}) }}</p>

<!-- Safe output (escape HTML) -->
<p>{{ doc.remarks | e }}</p>

<!-- Default fallback -->
<p>{{ doc.customer_name or "N/A" }}</p>
```

### Company & Letter Head Info

```html
<!-- Company details -->
<h2>{{ doc.company }}</h2>
<p>{{ doc.company_address }}</p>

<!-- Using frappe.get_doc for linked data -->
{% set company = frappe.get_doc("Company", doc.company) %}
<p>{{ company.phone_no }}</p>
<p>{{ company.email }}</p>
```

### Linked Document Data

```html
<!-- Fetch linked document -->
{% set customer = frappe.get_doc("Customer", doc.customer) %}
<p>{{ customer.customer_name }}</p>

<!-- Get value shorthand -->
{% set tax_id = frappe.db.get_value("Customer", doc.customer, "tax_id") %}
<p>Tax ID: {{ tax_id }}</p>
```

### Conditional Display

```html
<!-- Show block only if field has value -->
{% if doc.po_no %}
<p>PO Number: {{ doc.po_no }}</p>
{% endif %}

<!-- If/else -->
{% if doc.status == "Paid" %}
  <span class="badge-paid">✓ PAID</span>
{% else %}
  <span class="badge-due">PAYMENT DUE</span>
{% endif %}

<!-- Check permission or role -->
{% if frappe.session.user != "Guest" %}
  <p>Internal Note: {{ doc.internal_note }}</p>
{% endif %}
```

### Useful Frappe Filters

| Filter | Example | Output |
|--------|---------|--------|
| `\| e` | `{{ doc.note \| e }}` | HTML-escaped string |
| `\| abs_url` | `{{ "/files/logo.png" \| abs_url }}` | Absolute URL |
| `\| currency` | `{{ doc.total \| currency }}` | Formatted currency |
| `\| len` | `{{ doc.items \| len }}` | List length |
| `\| upper` | `{{ doc.name \| upper }}` | Uppercase string |

</div>

---

<div id="3-css-rules--print-media">

<br>

## 🎨 3) CSS Rules & Print Media

<img src="https://img.shields.io/badge/CSS3-Print_Optimized-1572B6?style=flat-square&logo=css3&logoColor=white" alt="Print CSS">

> ⚠️ **WARNING:** Never use global selectors. Always scope styles to your wrapper class.

### Scoping Pattern

```css
/* ❌ FORBIDDEN — Breaks system UI */
* { box-sizing: border-box; }
body { font-size: 14px; }
table { width: 100%; }

/* ✅ CORRECT — Scoped to your wrapper */
.invoice-wrapper * { box-sizing: border-box; }
.invoice-wrapper table { width: 100%; }
```

### Print Media Essentials

```css
/* Print-specific styles */
@media print {
    .invoice-wrapper {
        width: 210mm;          /* A4 width */
        margin: 0;
        padding: 10mm;
    }
    
    /* Hide unwanted UI elements */
    .no-print { display: none !important; }
    
    /* Prevent table row breaks across pages */
    .invoice-wrapper tr { page-break-inside: avoid; }
    
    /* Force page break */
    .page-break { page-break-after: always; }
    
    /* Keep header on each page */
    thead { display: table-header-group; }
    tfoot { display: table-footer-group; }
}
```

### Page Size Settings

```css
/* A4 Page */
@page {
    size: A4 portrait;
    margin: 15mm 20mm;
}

/* US Letter */
@page {
    size: letter portrait;
    margin: 1in;
}

/* Landscape */
@page {
    size: A4 landscape;
    margin: 15mm;
}
```

### Available CSS Variables

| Variable | Purpose |
|----------|---------|
| `--primary-color` | Brand/accent color |
| `--text-color` | Default text color |
| `--border-color` | Table/divider borders |
| `--card-bg` | Section backgrounds |

</div>

---

<div id="4-child-tables--loops">

<br>

## 📋 4) Child Tables & Loops

<img src="https://img.shields.io/badge/Jinja2-Child_Tables-B41706?style=flat-square&logo=jinja&logoColor=white" alt="Child Tables">

Most Frappe documents have child tables (e.g., `items`, `taxes`, `payment_schedule`).

### Basic Items Loop

```html
<table class="items-table">
    <thead>
        <tr>
            <th>#</th>
            <th>Item</th>
            <th>Description</th>
            <th>Qty</th>
            <th>Rate</th>
            <th>Amount</th>
        </tr>
    </thead>
    <tbody>
        {% for item in doc.items %}
        <tr>
            <td>{{ loop.index }}</td>
            <td>{{ item.item_code }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.qty }} {{ item.uom }}</td>
            <td>{{ item.rate }}</td>
            <td>{{ item.amount }}</td>
        </tr>
        {% endfor %}
    </tbody>
</table>
```

### Loop Helpers

```html
{% for item in doc.items %}
    {{ loop.index }}       {# Current index (1-based) #}
    {{ loop.index0 }}      {# Current index (0-based) #}
    {{ loop.first }}       {# True if first iteration #}
    {{ loop.last }}        {# True if last iteration #}
    {{ loop.length }}      {# Total number of items #}
{% endfor %}
```

### Taxes & Charges Table

```html
{% for tax in doc.taxes %}
<tr>
    <td>{{ tax.description }}</td>
    <td>{{ tax.rate }}%</td>
    <td>{{ tax.tax_amount }}</td>
</tr>
{% endfor %}
```

### Conditional Rows

```html
{% for item in doc.items %}
<tr {% if item.is_free_item %}class="free-item"{% endif %}>
    <td>{{ item.item_name }}</td>
    <td>
        {% if item.is_free_item %}
        <span>FREE</span>
        {% else %}
        {{ item.rate }}
        {% endif %}
    </td>
</tr>
{% endfor %}
```

### Totals Summary

```html
<table class="totals-table">
    <tr>
        <td>Net Total</td>
        <td>{{ doc.net_total }}</td>
    </tr>
    {% for tax in doc.taxes %}
    <tr>
        <td>{{ tax.description }} ({{ tax.rate }}%)</td>
        <td>{{ tax.tax_amount }}</td>
    </tr>
    {% endfor %}
    <tr class="grand-total">
        <td><strong>Grand Total</strong></td>
        <td><strong>{{ doc.grand_total }} {{ doc.currency }}</strong></td>
    </tr>
</table>
```

</div>

---

<div id="5-common-mistakes">

<br>

## 🚫 5) Common Mistakes

<img src="https://img.shields.io/badge/⚠️-Avoid_These-E74C3C?style=flat-square" alt="Warning">

### ❌ Not Scoping CSS

```css
/* ❌ BAD — Can break ERPNext desk */
table { border-collapse: collapse; }
td { padding: 8px; }

/* ✅ GOOD — Scoped to print wrapper */
.print-wrapper table { border-collapse: collapse; }
.print-wrapper td { padding: 8px; }
```

### ❌ Missing Null Checks

```html
<!-- ❌ BAD — Crashes if field is empty -->
{{ doc.custom_field.upper() }}

<!-- ✅ GOOD — Safe with fallback -->
{{ (doc.custom_field or "") | upper }}
{% if doc.custom_field %}{{ doc.custom_field }}{% endif %}
```

### ❌ Hardcoded Currency Symbol

```html
<!-- ❌ BAD — Doesn't respect multi-currency -->
<td>$ {{ doc.grand_total }}</td>

<!-- ✅ GOOD — Uses document currency -->
<td>{{ doc.currency }} {{ doc.grand_total }}</td>
<td>{{ frappe.format_value(doc.grand_total, {"fieldtype": "Currency"}) }}</td>
```

### ❌ Image Not Showing

```html
<!-- ❌ BAD — Relative path may break in PDF -->
<img src="/files/logo.png">

<!-- ✅ GOOD — Use abs_url for PDF rendering -->
<img src="{{ "/files/logo.png" | abs_url }}">

<!-- ✅ BETTER — Use company letterhead logo -->
{% set company = frappe.get_doc("Company", doc.company) %}
{% if company.company_logo %}
<img src="{{ company.company_logo | abs_url }}">
{% endif %}
```

### ❌ Table Breaking Across Pages

```css
/* ❌ BAD — Rows split mid-content when printing */
.print-wrapper tr { }

/* ✅ GOOD — Prevent row breaks */
@media print {
    .print-wrapper tr { page-break-inside: avoid; }
    .print-wrapper thead { display: table-header-group; }
}
```

### 📋 Quick Reference

| ❌ Mistake | ✅ Solution |
|-----------|-----------|
| Global CSS selectors | Always use wrapper class prefix |
| No null checks | Use `or ""` or `{% if %}` guards |
| Hardcoded currency | Use `doc.currency` dynamically |
| Relative image paths | Use `\| abs_url` filter |
| Rows breaking on print | Add `page-break-inside: avoid` |
| `<html>`,`<head>` tags | Not needed — Frappe handles the layout |

</div>

---

<div id="6-full-invoice-example">

<br>

## 🏆 6) Full Invoice Example

<img src="https://img.shields.io/badge/Example-Sales_Invoice-27AE60?style=flat-square" alt="Example">

### 📄 HTML

```html
<div class="invoice-wrapper">

    <!-- ===== HEADER ===== -->
    <div class="invoice-header">
        <div class="company-info">
            {% set company = frappe.get_doc("Company", doc.company) %}
            {% if company.company_logo %}
            <img src="{{ company.company_logo | abs_url }}" class="company-logo" alt="Logo">
            {% endif %}
            <h2>{{ doc.company }}</h2>
            <p>{{ company.company_description or "" }}</p>
        </div>
        <div class="invoice-meta">
            <h1>TAX INVOICE</h1>
            <table class="meta-table">
                <tr>
                    <td>Invoice No:</td>
                    <td><strong>{{ doc.name }}</strong></td>
                </tr>
                <tr>
                    <td>Date:</td>
                    <td>{{ frappe.format_date(doc.posting_date) }}</td>
                </tr>
                <tr>
                    <td>Due Date:</td>
                    <td>{{ frappe.format_date(doc.due_date) }}</td>
                </tr>
                {% if doc.po_no %}
                <tr>
                    <td>PO No:</td>
                    <td>{{ doc.po_no }}</td>
                </tr>
                {% endif %}
            </table>
        </div>
    </div>

    <hr class="divider">

    <!-- ===== BILL TO / SHIP TO ===== -->
    <div class="address-section">
        <div class="bill-to">
            <h4>Bill To</h4>
            <p><strong>{{ doc.customer_name }}</strong></p>
            <p>{{ doc.address_display or "" }}</p>
            {% if doc.contact_display %}
            <p>Contact: {{ doc.contact_display }}</p>
            {% endif %}
        </div>
        {% if doc.shipping_address_name %}
        <div class="ship-to">
            <h4>Ship To</h4>
            <p>{{ doc.shipping_address or "" }}</p>
        </div>
        {% endif %}
    </div>

    <!-- ===== ITEMS TABLE ===== -->
    <table class="items-table">
        <thead>
            <tr>
                <th class="col-no">#</th>
                <th class="col-item">Item</th>
                <th class="col-desc">Description</th>
                <th class="col-qty">Qty</th>
                <th class="col-uom">UOM</th>
                <th class="col-rate">Rate</th>
                <th class="col-amount">Amount</th>
            </tr>
        </thead>
        <tbody>
            {% for item in doc.items %}
            <tr>
                <td>{{ loop.index }}</td>
                <td>{{ item.item_code }}</td>
                <td>{{ item.description or item.item_name }}</td>
                <td>{{ item.qty }}</td>
                <td>{{ item.uom }}</td>
                <td>{{ item.rate }}</td>
                <td>{{ item.amount }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>

    <!-- ===== TOTALS ===== -->
    <div class="totals-section">
        <div class="spacer"></div>
        <table class="totals-table">
            <tr>
                <td>Net Total</td>
                <td>{{ doc.currency }} {{ doc.net_total }}</td>
            </tr>
            {% for tax in doc.taxes %}
            {% if tax.tax_amount != 0 %}
            <tr>
                <td>{{ tax.description }}</td>
                <td>{{ doc.currency }} {{ tax.tax_amount }}</td>
            </tr>
            {% endif %}
            {% endfor %}
            {% if doc.discount_amount %}
            <tr class="discount-row">
                <td>Discount</td>
                <td>- {{ doc.currency }} {{ doc.discount_amount }}</td>
            </tr>
            {% endif %}
            <tr class="grand-total-row">
                <td><strong>Grand Total</strong></td>
                <td><strong>{{ doc.currency }} {{ doc.grand_total }}</strong></td>
            </tr>
            {% if doc.outstanding_amount %}
            <tr class="outstanding-row">
                <td>Outstanding</td>
                <td>{{ doc.currency }} {{ doc.outstanding_amount }}</td>
            </tr>
            {% endif %}
        </table>
    </div>

    <!-- ===== AMOUNT IN WORDS ===== -->
    {% if doc.in_words %}
    <div class="in-words">
        <p><em>Amount in Words: <strong>{{ doc.in_words }}</strong></em></p>
    </div>
    {% endif %}

    <!-- ===== TERMS ===== -->
    {% if doc.terms %}
    <div class="terms-section">
        <h4>Terms & Conditions</h4>
        <p>{{ doc.terms }}</p>
    </div>
    {% endif %}

    <!-- ===== FOOTER ===== -->
    <div class="invoice-footer">
        <div class="signature-box">
            <p>Authorized Signature</p>
            <div class="signature-line"></div>
            <p>{{ doc.company }}</p>
        </div>
        <div class="thank-you">
            <p>Thank you for your business!</p>
        </div>
    </div>

</div>
```

### 🎨 CSS

```css
/* ===== WRAPPER ===== */
.invoice-wrapper {
    font-family: Arial, sans-serif;
    font-size: 13px;
    color: #333;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

/* ===== HEADER ===== */
.invoice-wrapper .invoice-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 20px;
}

.invoice-wrapper .company-logo {
    max-height: 70px;
    max-width: 180px;
    margin-bottom: 8px;
}

.invoice-wrapper .company-info h2 {
    font-size: 18px;
    margin: 0 0 4px 0;
    color: #1a1a2e;
}

.invoice-wrapper .invoice-meta h1 {
    font-size: 22px;
    font-weight: 700;
    color: #2490E3;
    text-align: right;
    margin: 0 0 10px 0;
}

.invoice-wrapper .meta-table td {
    padding: 3px 8px;
    font-size: 12px;
}

.invoice-wrapper .meta-table td:first-child {
    color: #666;
    white-space: nowrap;
}

/* ===== DIVIDER ===== */
.invoice-wrapper .divider {
    border: none;
    border-top: 2px solid #2490E3;
    margin: 15px 0;
}

/* ===== ADDRESS SECTION ===== */
.invoice-wrapper .address-section {
    display: flex;
    gap: 40px;
    margin: 20px 0;
}

.invoice-wrapper .bill-to,
.invoice-wrapper .ship-to {
    flex: 1;
    background: #f8f9fa;
    padding: 12px 16px;
    border-radius: 6px;
    border-left: 3px solid #2490E3;
}

.invoice-wrapper .bill-to h4,
.invoice-wrapper .ship-to h4 {
    margin: 0 0 8px 0;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: #2490E3;
}

.invoice-wrapper .bill-to p,
.invoice-wrapper .ship-to p {
    margin: 2px 0;
    font-size: 12px;
}

/* ===== ITEMS TABLE ===== */
.invoice-wrapper .items-table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
}

.invoice-wrapper .items-table thead tr {
    background: #2490E3;
    color: white;
}

.invoice-wrapper .items-table th {
    padding: 10px 12px;
    text-align: left;
    font-size: 12px;
    font-weight: 600;
}

.invoice-wrapper .items-table td {
    padding: 9px 12px;
    border-bottom: 1px solid #e8e8e8;
    font-size: 12px;
    vertical-align: top;
}

.invoice-wrapper .items-table tbody tr:nth-child(even) {
    background: #f9f9f9;
}

.invoice-wrapper .items-table .col-no   { width: 4%; }
.invoice-wrapper .items-table .col-item { width: 15%; }
.invoice-wrapper .items-table .col-desc { width: 30%; }
.invoice-wrapper .items-table .col-qty  { width: 8%; text-align: center; }
.invoice-wrapper .items-table .col-uom  { width: 8%; }
.invoice-wrapper .items-table .col-rate { width: 15%; text-align: right; }
.invoice-wrapper .items-table .col-amount { width: 15%; text-align: right; }

/* ===== TOTALS ===== */
.invoice-wrapper .totals-section {
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
}

.invoice-wrapper .spacer { flex: 1; }

.invoice-wrapper .totals-table {
    width: 280px;
    border-collapse: collapse;
}

.invoice-wrapper .totals-table td {
    padding: 6px 10px;
    font-size: 13px;
}

.invoice-wrapper .totals-table td:last-child {
    text-align: right;
}

.invoice-wrapper .discount-row td {
    color: #e74c3c;
}

.invoice-wrapper .grand-total-row {
    background: #2490E3;
    color: white;
    font-size: 14px;
}

.invoice-wrapper .outstanding-row {
    border-top: 2px solid #e74c3c;
    color: #e74c3c;
}

/* ===== IN WORDS ===== */
.invoice-wrapper .in-words {
    margin: 16px 0;
    padding: 10px 14px;
    background: #f0f7ff;
    border-radius: 4px;
    font-size: 12px;
    border: 1px solid #c8dff7;
}

/* ===== TERMS ===== */
.invoice-wrapper .terms-section {
    margin-top: 20px;
    padding: 12px;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    font-size: 12px;
}

.invoice-wrapper .terms-section h4 {
    margin: 0 0 6px 0;
    font-size: 12px;
    text-transform: uppercase;
    color: #555;
}

/* ===== FOOTER ===== */
.invoice-wrapper .invoice-footer {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    margin-top: 40px;
    padding-top: 20px;
    border-top: 1px solid #ddd;
}

.invoice-wrapper .signature-box {
    text-align: center;
    font-size: 12px;
}

.invoice-wrapper .signature-line {
    width: 160px;
    border-top: 1px solid #333;
    margin: 30px auto 6px auto;
}

.invoice-wrapper .thank-you {
    font-size: 13px;
    color: #2490E3;
    font-style: italic;
}

/* ===== PRINT MEDIA ===== */
@page {
    size: A4 portrait;
    margin: 12mm 15mm;
}

@media print {
    .invoice-wrapper tr { page-break-inside: avoid; }
    .invoice-wrapper thead { display: table-header-group; }
    .invoice-wrapper tfoot { display: table-footer-group; }
    .no-print { display: none !important; }
}
```

</div>

---

<div id="7-advanced-pro-tips">

<br>

## 🚀 7) Advanced Pro Tips

<img src="https://img.shields.io/badge/Advanced-Expert_Level-FF6B6B?style=flat-square" alt="Advanced">

<details>
<summary><b>🌍 Translations / i18n</b></summary>
<br>

```html
<!-- Jinja translation in print format -->
<h1>{{ _("Tax Invoice") }}</h1>
<p>{{ _("Bill To") }}</p>
<th>{{ _("Description") }}</th>
```

> 📌 Enable "Allow Print for Draft" and translations in DocType settings.

</details>

<details>
<summary><b>🖼 Letterhead Integration</b></summary>
<br>

```html
<!-- Use Frappe's built-in Letterhead -->
{% if letter_head %}
<div class="letterhead">
    {{ letter_head }}
</div>
{% endif %}

<!-- Or manually load company logo -->
{% set company = frappe.get_doc("Company", doc.company) %}
{% if company.company_logo %}
<img src="{{ company.company_logo | abs_url }}" style="max-height: 80px;">
{% endif %}
```

</details>

<details>
<summary><b>📊 Calculations in Template</b></summary>
<br>

```html
<!-- Sum a child table column -->
{% set total_qty = doc.items | sum(attribute='qty') %}
<p>Total Quantity: {{ total_qty }}</p>

<!-- Count rows -->
<p>Total Line Items: {{ doc.items | length }}</p>

<!-- Filter items in loop -->
{% for item in doc.items if item.item_group == "Services" %}
<tr><td>{{ item.item_name }}</td></tr>
{% endfor %}
```

</details>

<details>
<summary><b>🏷 Barcode & QR Code</b></summary>
<br>

```html
<!-- QR Code using Google Charts API -->
<img src="https://chart.googleapis.com/chart?chs=150x150&cht=qr&chl={{ doc.name | urlencode }}" 
     alt="QR Code">

<!-- Or use Frappe's built-in barcode field -->
<div class="barcode">
    {{ frappe.barcode(doc.name, "Code128") }}
</div>
```

</details>

<details>
<summary><b>📄 Multi-Page / Page Breaks</b></summary>
<br>

```html
<!-- Force a page break between sections -->
<div class="page-break"></div>
```

```css
.invoice-wrapper .page-break {
    page-break-after: always;
    break-after: page;
}

/* Keep section together — no break inside */
.invoice-wrapper .keep-together {
    page-break-inside: avoid;
}
```

</details>

<details>
<summary><b>🔢 Number Formatting</b></summary>
<br>

```html
<!-- Format as currency (respects system settings) -->
{{ frappe.format_value(doc.grand_total, {"fieldtype": "Currency"}) }}

<!-- Format with specific currency -->
{{ frappe.format_value(doc.grand_total, {"fieldtype": "Currency", "currency": doc.currency}) }}

<!-- Float precision -->
{{ "%.2f" | format(doc.qty) }}

<!-- Percentage -->
{{ doc.tax_rate }}%
```

</details>

<details>
<summary><b>🗓 Date & Time Formatting</b></summary>
<br>

```html
<!-- Default system date format -->
{{ frappe.format_date(doc.posting_date) }}

<!-- Custom format -->
{{ frappe.format_date(doc.posting_date, "dd-MM-yyyy") }}

<!-- Date + Time -->
{{ frappe.format_datetime(doc.modified) }}

<!-- Relative (e.g., "2 days ago") -->
{{ frappe.utils.pretty_date(doc.creation) }}
```

</details>

<details>
<summary><b>🛠 Debugging Print Formats</b></summary>
<br>

```html
<!-- Dump all available doc fields (REMOVE before production!) -->
<pre style="font-size:10px; background:#f5f5f5; padding:10px;">
{{ doc | tojson(indent=2) }}
</pre>

<!-- Check specific variable -->
{% if doc.custom_field is defined %}
    <p>Field value: {{ doc.custom_field }}</p>
{% else %}
    <p style="color:red">Field not found!</p>
{% endif %}
```

```python
# Python — check what's in print format context via bench console
import frappe
doc = frappe.get_doc("Sales Invoice", "SINV-0001")
print(doc.as_dict().keys())
```

</details>

<details>
<summary><b>🔐 Permissions & Visibility</b></summary>
<br>

```html
<!-- Show internal info only to logged-in users -->
{% if frappe.session.user != "Guest" %}
<p>Internal Cost: {{ doc.landed_cost_voucher_amount }}</p>
{% endif %}

<!-- Role-based visibility -->
{% if "Accounts Manager" in frappe.get_roles() %}
<p>Profit Margin: {{ doc.profit_margin }}%</p>
{% endif %}
```

</details>

</div>

---

<br>

## 👨‍💻 Developer

<div align="center">

### Mohamed Ayman
**Frappe / ERPNext Developer**

</div>