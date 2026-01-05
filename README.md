FabLogic HG

A Dual-Backend ERP & Quoting Engine for Custom Fabrication.

📸 Demo

💼 The Challenge

In the custom hose and gasket industry, "off-the-shelf" software fails. Inventory is complex (inches vs. feet), and pricing requires intricate calculations (nesting circular gaskets on rectangular sheets to minimize waste).

My client needed a system that could handle these complex manufacturing variables while syncing financial data with both QuickBooks Online and QuickBooks Desktop.

🛠️ The Solution

I engineered FabLogic HG, a standalone Python desktop application (PySide6) that serves as a "sidecar" to QuickBooks. It bridges the gap between raw inventory data and manufacturing logic.

Dual-Backend Strategy: Implemented an Abstract Base Class architecture to allow the app to seamlessly switch between QuickBooks Online (REST API) and Desktop (COM/XML) without changing the core business logic.

Algorithmic Optimization: Developed a custom "Greedy Best-Fit" nesting algorithm. It calculates how many gaskets fit on a sheet, accounting for "slugs" (waste material from inner cutouts) to maximize yield and accurately price scrap.

Dynamic Assembly Builder: Created a "Hose Builder" module that automatically calculates labor, fitting costs, and hose length.

🚀 Technical Highlights (The "Secret Sauce")

Abstracting Legacy Integrations

One of the biggest hurdles was supporting both modern web APIs and legacy desktop COM interfaces simultaneously. I used the Strategy Pattern to create a unified BackendInterface. This allows the UI to call standard methods like push_invoice(), while the underlying adapter handles the specific complexities of XML parsing (for Desktop) or JSON payloads (for Online).

Key Code Snippet

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


🏗️ Architecture & Tech Stack

This application was built to be scalable and maintainable using the following technologies:

Category

Technologies

Core

Python 3.11, PySide6 (Qt)

Data

SQLite (Local Caching), Pandas (Data Manipulation)

Integrations

Intuit OAuth (QBO), pywin32 (QBD COM Interface)

Key Logic

Custom 2D Bin Packing Algorithm (Nesting)

System Data Flow

graph TD;
    UI[PySide6 Frontend] --> Controller;
    Controller --> BackendInterface{Backend Interface};
    BackendInterface -->|REST API| QBO[QuickBooks Online];
    BackendInterface -->|COM/XML| QBD[QuickBooks Desktop];
    Controller --> Algo[Nesting Algorithm];
    Algo --> DB[(SQLite Cache)];


🔐 Licensing & Access

FabLogic HG is a proprietary commercial product.

Source Code: Closed Source (Available via licensing agreement)

Binaries: Windows 10/11 Executable (.exe)

Customization: Available for white-labeling or feature additions.

To request a demo or discuss integration with your existing ERP workflow, please contact me directly.

📬 Contact

Aaron - Operations Manager turned Python Developer

View Portfolio Website | Connect on LinkedIn
