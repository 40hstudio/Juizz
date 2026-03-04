# Shopify × Webflow Integration Documentation
> Shopify is the source of truth. Shopyflow syncs to Webflow.

## Table of Contents
- [Phase 1: Logic Flow & Product Setup](#phase-1-logic-flow--product-setup)
- [Phase 2: Creating a New Product in Shopify](#phase-2-creating-a-new-product-in-shopify)
- [Phase 3: Manual Workflow in Webflow CMS](#phase-3-manual-workflow-in-webflow-cms)

---

## Phase 1: Logic Flow & Product Setup

### 🔄 Logic Flow

This project uses a three-layer architecture:

| Layer | Role | Responsibility |
|---|---|---|
| **Shopify** | Source of Truth | Core product data: pricing, inventory, images, basic descriptions |
| **Shopyflow** | Bridge | Detects Shopify changes and syncs them to Webflow |
| **Webflow** | Frontend/CMS | Displays products and stores advanced content (filters/specs) |

**Key rule:** If a field is synced from Shopify, do not edit it in Webflow—Shopyflow will overwrite it on the next sync.

---

### 🛍️ Product Setup (Synced Fields)

When a product is created or updated in Shopify, Shopyflow automatically syncs it to the Webflow Products Collection.

#### Shopify → Webflow Field Mapping

| Shopify Field | → | Webflow CMS | Notes |
|---|---|---|---|
| **Title** | → | Products → Product Name | Display name |
| **Description** | → | Products → Rich Text Product Description | Rich text content |
| **Media** | → | Products → Product Image/Gallery | Images/gallery |
| **Product Type** | → | Product Type Collection | Used for categorization logic |
| **Vendor** | → | Vendors Collection | Brand |
| **Tags** | → | Tag Collection | Internal organization & syncing logic |
| **Variants** | → | Product Options | Größe / Farbe / Rahmenform |
| **Price** | → | Products → Display Price | Live-synced pricing |
| **Collection** | → | Collection (Subcategory) mapping | **Subcategories** (multiple allowed) |

> **Collection = Subcategory (multiple allowed):** Assign one or more Shopify **Collections** to a product to define its **Subcategory/Subcategories**. This lets one product appear in multiple subcategory views on the storefront.

#### Webflow CMS Collections Managed by Shopyflow
- **Products**
- **Vendors**
- **Product Options**
- **Tag**
- **Product Type**
- **Collection**

---

## Phase 2: Creating a New Product in Shopify

### ✅ Shopify Product Fields (What to Fill In)

When creating a new product in Shopify, fill in all relevant fields so the product syncs and displays correctly.

| Field | Description |
|---|---|
| **Title** | Product name |
| **Description** | Product description (rich text) |
| **Media** | Images/videos |
| **Category** | Closest Shopify category (required for metafields like color hex) |
| **Type** | **Must be one of:** E-Bike / E-Cargobike / Kinderrad |
| **Vendor** | Brand |
| **Collection** | **Required.** Used as **Subcategory**; multiple collections allowed |
| **Tags** | Internal organization & syncing logic |
| **Variants** | Größe / Farbe / Rahmenform |
| **Price** | Price and optional Compare-at price |
| **Quantity** | Inventory available per variant |

---

### 🗂️ Type Fields — Top-Level Types (3 Options Only)

The **Type** field is used to select the top-level product group. The user must choose **one** of:

- **E-Bike**
- **E-Cargobike**
- **Kinderrad**

After selecting one of these, Shopify **Smart Collections** handle the mapping into the correct collections.

> **Important:** Do not create custom Type values outside these three options, otherwise Smart Collection mapping may fail.

---

### 🗂️ Collection Fields — Subcategories (Multiple Allowed)

Shopify **Collections** are used as the product���s **Subcategories**.

- A product can belong to **multiple** Collections.
- This allows a product to appear in multiple subcategory pages/filters.

Example:
- Collection: `E-Gravelbike`
- Collection: `Sale`

---

### 📂 Category Fields (Metafields Requirement)

Choose the closest matching Shopify **Category**.

This is important because the Category controls which **metafields** are available—especially the **color metafield** used to provide a **hex code** for the Farbe/Color variant.

> If the product Category is incorrect (or missing), the color variant may not have a hex code and color swatches may not render correctly on the product detail page.

---

### 🏢 Vendor Fields

Vendor = product brand. This syncs to Webflow’s **Vendors** collection.

---

### 🎛️ Variants Fields

This project uses three variants:

| Variant | Meaning | Notes |
|---|---|---|
| **Größe** | Size | Standard option |
| **Farbe** | Color | Must be connected to Category metafields for hex code |
| **Rahmenform** | Frame shape | Standard option |

---

### 💰 Price Fields (Sale Logic)

To show a product as on sale:
- Set **Compare-at price** higher than **Price**.

Example:
```
Price:          €1,299
Compare-at:     €1,599
```

---

### 📦 Quantity Fields

Set inventory per variant using the **Available** quantity.

---

### ✅ Product Creation Checklist

- [ ] Title filled in
- [ ] Description written
- [ ] Media uploaded
- [ ] Category selected (metafields / color hex)
- [ ] Type set to one of: E-Bike / E-Cargobike / Kinderrad
- [ ] Vendor entered
- [ ] Collection(s) added (Subcategory; multiple allowed)
- [ ] Tags added
- [ ] Variants set up (Größe, Farbe, Rahmenform)
- [ ] Price set (+ Compare-at if on sale)
- [ ] Quantity set

---

## Phase 3: Manual Workflow in Webflow CMS

### Overview

Some content is not managed in Shopify and must be entered manually in Webflow CMS:

1. **Product Filters** (Option Filters CMS)
2. **Product Technical Specs** (Product CMS fields)

---

### 🔽 Product Filters (Option Filters CMS)

Filters are controlled by a Webflow CMS collection called **Option Filters**.

For each variant value you want users to filter by, create an Option Filters CMS item.

#### Option Filters Fields

| Field | Description | Example |
|---|---|---|
| **Filter Name** | Display name of the option | `Red` |
| **Filter Group** | Group type | `Color` / `Size` / `Frame` |
| **Filter Color** | Swatch color (Color group only) | `#FF0000` |

> **Important:** Filter Name should match the variant value naming to avoid mismatches.

---

### 🔧 Product Technical Specs (Product CMS)

Technical specs are entered on the **Product CMS** item in Webflow. If a spec field is empty, the corresponding section is hidden on the frontend.

#### Technical Specs Fields
- **Motor**
- **Bremsen**
- **Lenksystem**
- **Reifen & Rahmengröße**
- **Weitere Details**

#### Steps
1. Open the product in Webflow **Products** CMS
2. Fill in any relevant technical fields
3. Leave irrelevant fields blank (they will be hidden)
4. Publish

---

### ✅ Phase 3 Checklist

**Option Filters**
- [ ] Add filter entries for Color / Size / Frame values as needed
- [ ] Ensure Filter Name matches variant naming

**Technical Specs**
- [ ] Fill in relevant specs on the Product CMS item
- [ ] Publish changes
