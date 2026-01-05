# FabLogic HG
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/GUI-PySide6%20(Qt)-41cd52?style=flat&logo=qt&logoColor=white)](https://doc.qt.io/qtforpython/)
[![Industry](https://img.shields.io/badge/Industry-Hose%20%26%20Gasket-orange?style=flat)](https://github.com/Bearded1derer/FabLogic-Showcase)

**Specialized Manufacturing Intelligence for the Hose & Gasket Industry.**

---

## 🏭 The Problem: Generic ERPs Don't Cut It
In the custom hose and gasket industry, "off-the-shelf" software fails.
* **Inventory Mismatch:** You buy sheet rubber in **pounds** or **rolls** but sell it in **square inches** or **custom shapes**.
* **The "Scrap" Trap:** Standard software cannot account for the waste material left over after cutting circular gaskets from a rectangular sheet.
* **Complex Assemblies:** Quoting a hose assembly requires tracking the hose, the fittings, the labor, and the crimp specs simultaneously.

**FabLogic HG** bridges the gap. It is a proprietary "sidecar" application that adds specialized manufacturing logic to your existing QuickBooks environment.

## ⚡ Key Capabilities

### 1. Smart Gasket Nesting (Yield Optimization)
Stop guessing your material costs. FabLogic HG uses a custom **Greedy Best-Fit algorithm** to virtually "nest" circular gaskets onto your specific sheet sizes before you quote.
* **Optimized Yield:** Calculates the exact number of gaskets per sheet.
* **Slug Management:** Accounts for usable "slugs" (waste material from inner cutouts) to maximize future profit.
* **Accurate Costing:** Automatically distributes the cost of waste material into the unit price.

### 2. The Hose Assembly Builder
A dedicated module for building complex hose assemblies in seconds.
* **Auto-Calculations:** Input the ID, pressure rating, and length; the system automatically selects the correct fittings and crimp specs.
* **Labor Tracking:** Applies standard labor times based on the assembly complexity (crimping vs. welding).

### 3. Dual-Backend QuickBooks Bridge
FabLogic HG is built on a "Dual-Backend" architecture, meaning it works seamlessly with both:
* **QuickBooks Desktop (Enterprise):** Direct COM/XML integration for legacy stability.
* **QuickBooks Online:** Modern OAuth2 REST API integration for cloud flexibility.

*Your business logic stays the same, even if you migrate your accounting software.*

---

## 📸 System Demo
> *[Insert your demo GIF here]*

---

## 🛠️ Technical Architecture
Built for reliability on the shop floor and flexibility in the back office.

| Layer | Technology Stack |
| :--- | :--- |
| **Core Application** | Python 3.11, PySide6 (Qt) |
| **Data & Caching** | SQLite (Local speed), Pandas (Data manipulation) |
| **Legacy Bridge** | `pywin32` (COM Interface for QB Desktop) |
| **Cloud Bridge** | Intuit OAuth2 (REST API for QB Online) |
| **Algorithms** | Custom 2D Bin Packing (Nesting) |

### The "Secret Sauce": Abstract Strategy Pattern
One of the biggest technical challenges was supporting both modern web APIs and legacy desktop COM interfaces simultaneously.

I used the **Strategy Pattern** to create a unified `BackendInterface`. This allows the UI to call standard methods like `push_invoice()`, while the underlying adapter handles the specific complexities of XML parsing (for Desktop) or JSON payloads (for Online).

```python
# The Greedy Best-Fit Nesting Strategy
def calculate_layout(sheet_width, sheet_height, items, spacing):
    # Sort items by OD descending to optimize packing (Largest first)
    sorted_items = sorted(items, key=lambda x: x['od'], reverse=True)
    
    placed_items = []
    
    for item in sorted_items:
        # Check if item fits in an existing "Slug" (waste material) 
        # before using a fresh sheet to maximize yield.
        best_slug_idx = -1
        for i, slug in enumerate(slugs):
            if slug['size'] >= item['od'] + spacing:
                # ... Match logic ...
