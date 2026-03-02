# Shopify × Webflow Integration Documentation
> Powered by Shopyflow

---

## Table of Contents

- [Phase 1: Logic Flow & Product Setup](#phase-1-logic-flow--product-setup)
- [Phase 2: Creating a New Product in Shopify](#phase-2-creating-a-new-product-in-shopify)
- [Phase 3: Manual Workflow in Webflow CMS](#phase-3-manual-workflow-in-webflow-cms)

---

## Phase 1: Logic Flow & Product Setup

### 🔄 Logic Flow

The entire system is built on a **three-layer architecture** where each layer has a clear and distinct role:

| Layer | Role | Responsibility |
|---|---|---|
| **Shopify** | Source of Truth | Stores and manages all core product data |
| **Shopyflow** | The Bridge | Detects Shopify changes and syncs to Webflow |
| **Webflow** | The Frontend | Displays data and handles advanced content |

> **Key Principle:** Shopify is always the single source of truth. Any product data that needs to be updated — pricing, inventory, images, or descriptions — must be edited in Shopify first. Shopyflow will automatically propagate those changes to Webflow.

---

### 🛍️ Product Setup

#### How It Works

When a product is **created or updated in Shopify**, Shopyflow automatically detects the change and syncs the data to the corresponding Webflow CMS collections. **No manual entry is required in Webflow** for these core fields.

---

#### Field Mapping Reference

The following fields are managed in Shopify and mapped to specific Webflow CMS Collections:

| Shopify Field | → | Webflow CMS Collection | Notes |
|---|---|---|---|
| **Title** | → | Products → Product Name | Primary display name |
| **Description** | → | Products → Rich Text Description | Supports formatted content |
| **Media** | → | Products → Image / Gallery | All product images |
| **Product Type** | → | Product Type Collection | Used for categorization |
| **Vendor** | → | Vendors Collection | Brand information |
| **Tags** | → | Tag Collection | Internal organization & sync logic |
| **Variants** | → | Product Options | Color, Size, Frame, etc. |
| **Price** | → | Products → Display Price | Live-synced pricing |

---

#### Webflow CMS Collections Managed by Shopyflow

Once synced, Shopyflow populates the following collections in Webflow CMS:

- 📦 **Products**
- 🏷️ **Vendors**
- 🎨 **Product Options**
- 🔖 **Tag**
- 📂 **Product Type**
- 🗂️ **Collection**

---

#### ⚠️ Important Rules

1. **Never edit synced fields directly in Webflow.** Changes will be overwritten on the next sync.
2. **All pricing and inventory updates** must be made in Shopify.
3. **Advanced or custom content** (e.g., detailed editorial descriptions, SEO fields, custom layouts) can be managed directly in Webflow as they are outside the sync scope.

---

## Phase 2: Creating a New Product in Shopify

### 📋 Shopify Product Fields Overview

When creating a new product in Shopify, **all fields must be filled out completely** to ensure the product is detailed, accurate, and syncs correctly to Webflow via Shopyflow.

Here's a summary of every field you need to complete:

| Field | Description |
|---|---|
| **Title** | The product name displayed on the storefront |
| **Description** | Detailed product information (supports rich text) |
| **Media** | Product images and/or videos |
| **Category** | Shopify's standard product category (affects metafields) |
| **Type** | Custom product type used for Smart Collections |
| **Vendor** | The brand of the product |
| **Tags** | Used for filtering, organization, and sync logic |
| **Variant** | Product options: Größe, Farbe, Rahmenform |
| **Price** | Regular price (and Compare Price for sale items) |

---

### 🗂️ Type Fields — Smart Collections

The **Type** field powers Shopify's **Smart Collection** logic. When you enter a product type, Shopify **automatically assigns the product** to the matching collection — no manual collection management needed.

#### Smart Collection Mapping

| Collection | Product Types |
|---|---|
| 🚲 **E-Bike** | E-Gravelbike, E-Citybike, E-Faltrad, E-Tourenbike, E-Mountainbike, E-SUB |
| 👶 **Kinderräder** | Vendor = Pyro |
| 📦 **E-Cargobike** | E-Longtail, E-Longjohn, E-Trike |

> **How it works:** Simply type the product type (e.g., `E-Gravelbike`) into the Type field, and Shopify will automatically place the product into the correct Smart Collection (e.g., **E-Bike**).

---

### 📂 Category Fields

You must **choose the closest matching Shopify standard category** for the product.

> ⚠️ **This is critical.** The category unlocks Shopify's **metafield system**. Without the correct category selected, the **Farbe (colour) variant will not have a hex code** — meaning customers won't see the colour swatch on the product detail page.

---

### 🏢 Vendor Fields

The **Vendor** field represents the **brand of the product**. This is synced to the Webflow **Vendors Collection** via Shopyflow.

---

### 🎛️ Variants Fields

This project uses **3 variant types** for each product:

| Variant | Description | Special Requirement |
|---|---|---|
| **Größe** | Size of the product | Standard input |
| **Farbe** | Colour of the product | ⚠️ Must be connected to **Metafields** for hex code |
| **Rahmenform** | Frame shape/type | Standard input |

> **Farbe & Metafields:** In order for the colour hex code to appear on the product detail page, the **Farbe variant must be linked to the colour metafield**. This connection is only available when the correct **Category** has been selected (see above).

---

### 💰 Price Fields

Pricing is set at the **variant level**, giving you per-variant price control.

| Field | Usage |
|---|---|
| **Price** | The current selling price |
| **Compare Price** | The original price before the sale |

> **Sale Logic:** To mark a product as on sale, set the **Compare Price higher than the Price**. Shopify (and Webflow via Shopyflow) will automatically recognize and display this as a discounted product.

**Example:**
```
Price:          €1,299
Compare Price:  €1,599  ← Shows as "Was €1,599, Now €1,299"
```

---

### 📦 Quantity Fields

Stock quantity is also managed at the **variant level**.

| Field | Description |
|---|---|
| **Available** | The number of units available for each variant |

> Set the quantity per variant to ensure accurate inventory tracking and to prevent overselling.

---

### ✅ Product Creation Checklist

Before publishing a new product, make sure all of the following are completed:

- [ ] Title filled in
- [ ] Description written
- [ ] Media uploaded
- [ ] **Category selected** (required for metafields & colour hex codes)
- [ ] Type entered (for Smart Collection assignment)
- [ ] Vendor entered
- [ ] Tags added
- [ ] All 3 variants set up (Größe, Farbe, Rahmenform)
- [ ] **Farbe variant connected to colour metafield**
- [ ] Price set per variant
- [ ] Compare Price set (if product is on sale)
- [ ] Quantity set per variant

---

## Phase 3: Manual Workflow in Webflow CMS

### Overview

While Shopify (via Shopyflow) handles all core product data automatically, there are certain fields that require **manual input directly in Webflow CMS**. This phase covers those two key areas: **Product Filters** and **Product Technical Specs**.

---

### 🔽 Product Filters

Product filters allow customers to narrow down products on the storefront by **Frame, Color, and Size**. These are managed through a dedicated Webflow CMS collection called **Option Filters**.

#### How It Works

For every variant value you've set up in Shopify, a corresponding entry must be **manually created in the Option Filters CMS** in Webflow. This is what powers the filter UI on the frontend.

#### Option Filters CMS — Field Reference

| Field | Description | Example |
|---|---|---|
| **Filter Name** | The display name of the filter option | `Red` |
| **Filter Group** | The variant group this filter belongs to | `Color` |
| **Filter Color** | The visual color swatch (for Color group only) | `#FF0000` (Red) |

#### Filter Groups Available

| Group | Purpose |
|---|---|
| 🖼️ **Frame** | Filters products by Rahmenform (frame shape) |
| 🎨 **Color** | Filters products by Farbe (colour), shows color swatch |
| 📐 **Size** | Filters products by Größe (size) |

#### Step-by-Step: Adding a Filter Entry

For every new variant value added in Shopify, follow these steps in Webflow CMS:

1. Open the **Option Filters** CMS collection in Webflow
2. Create a **new entry**
3. Fill in the fields:
   - **Filter Name** → Match the variant value (e.g., `Red`)
   - **Filter Group** → Select the correct group (e.g., `Color`)
   - **Filter Color** → Set the matching color value (e.g., `#FF0000`)
4. **Publish** the entry

> ⚠️ **Important:** The Filter Name must **exactly match** the variant value set in Shopify. Any mismatch will cause the filter to not connect correctly with the product.

---

### 🔧 Product Technical Specs

Technical specifications provide customers with detailed hardware and component information about each product. These fields are managed directly in the **Product CMS** in Webflow.

#### How It Works

Each product in the Webflow Product CMS has dedicated technical spec fields. These fields are **hidden on the frontend by default** — they will only appear on the product detail page **when content has been entered**.

> ✅ **No content = Hidden section.** You don't need to worry about empty fields showing up on the page.

#### Technical Spec Fields

| Field | Description |
|---|---|
| ⚡ **Motor** | Motor type, wattage, and specifications |
| 🛑 **Bremsen** | Brake system details |
| 🔧 **Lenksystem** | Steering system information |
| 🔵 **Reifen & Rahmengröße** | Tire size and frame dimensions |
| 📋 **Weitere Details** | Any additional technical information |

#### Step-by-Step: Adding Technical Specs

1. Open the **Products** CMS collection in Webflow
2. Find and open the **product you want to update**
3. Scroll to the **Technical Specs section**
4. Fill in only the fields that are **relevant to the product**
5. Leave unused fields **blank** — they will automatically be hidden
6. **Save and publish** the item

---

### 📝 Phase 3 Manual Workflow Checklist

After syncing a product from Shopify via Shopyflow, complete the following in Webflow CMS:

**Option Filters**
- [ ] Add a Filter entry for each **Color** variant (with hex code)
- [ ] Add a Filter entry for each **Size** variant
- [ ] Add a Filter entry for each **Frame** variant
- [ ] Verify Filter Names match Shopify variant values exactly

**Technical Specs**
- [ ] Open the product in the Products CMS
- [ ] Fill in **Motor** details (if applicable)
- [ ] Fill in **Bremsen** details (if applicable)
- [ ] Fill in **Lenksystem** details (if applicable)
- [ ] Fill in **Reifen & Rahmengröße** details (if applicable)
- [ ] Fill in **Weitere Details** (if applicable)
- [ ] Publish the updated product
