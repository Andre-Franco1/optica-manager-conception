# Briefing — Optical Store Management System (Optica Manager)
    
    A chain of optical stores needs to manage its daily operations, including client registration, product inventory, sales processing, and invoice generation.
    The system must also support multiple branches (stores), each with its own separate stock and user access.
    This first version will implement an MVP (Minimum Viable Product) focused on the most essential features — client management, sales, inventory, and invoices — while maintaining a scalable structure for future expansions such as accounting, reports, and marketing integrations.


## Basic Information:
    - The system will serve two optical store branches, each identified by a unique TenantId.
    - Each branch has its own inventory and users.
    - The system will be operated by a single employee per store, who performs all daily tasks (sales, stock updates, etc.).
    - Every sale must automatically update the stock quantities of the products sold.
    - The system must ensure data consistency between physical and digital stock — discrepancies should be minimized by encouraging the operator to register all received products before they are sold.
    - In the future, the system may integrate with suppliers for automatic stock updates via XML import.

## Core Features:
    - Client Management (CRUD): Register, edit, and list clients with personal information and purchase history.
    - Sales Management (CRUD): Register product sales, associate them with a client, and automatically update stock and financial data.
    - User Management (CRUD): Manage system users and define permissions by role.
    - Stock Management (CRUD): Register products, manage quantities, and update stock based on sales and deliveries.
    - Invoice Generation: Generate electronic invoices (NF-e / NFC-e) for each sale.

## Stock 
    - Each product (e.g., lenses, frames, contact lenses, accessories) is registered with:
        - name
        - brand
        - model
        - color
        - purchase price
        - sale price
        - quantity in stock
        - category
        - store (TenantId)
    - Every time a sale is made:
        - The quantity of each product sold is automatically deducted from the corresponding store’s stock.
    - If the operator tries to sell a product that is not in the stock or has zero quantity, the system must block the sale.
    - Stock should also support manual adjustments for inventory corrections or damaged/lost products.

## Sales Process:
    - A sale includes:
        - Client (existing or new)
        - List of sold products (each product may include a frame, lenses, or both)
        - Date and time
        - Payment method (cash, card, PIX, installment)
        - Total amount
        - Invoice (generated after confirmation)
    - After the sale is saved:
        - The stock is updated automatically.
        - An Invoice and a PDF Service Order are generated.
        - The PDF Service Order must include:
            - Customer details (name, CPF/CNPJ, phone, address)
            - Product details (frames, lenses, accessories, prices, and discounts)
            - Payment method and sale status
            - Sale date and expected delivery date
            - Store and salesperson identification
        - The PDF file must be saved and linked to the corresponding sale record in the system.
    - In case of returns or exchanges, the system must:
        - Allow the sale to be marked as Returned or Exchanged.
        - Restore the product quantity to stock in the case of a return.
        - Create a new sale record in the case of an exchange.

## Multi-Tenant Logic:
    - Each store (branch) operates under a unique TenantId.
    - All CRUD operations (clients, sales, stock, users) are filtered by TenantId.
    - Administrators may have access to all stores, while store operators only access their own branch data.

## User Types:
    - Operator User:
        - Can register clients.
        - Can manage sales.
        - Can manage stock for their branch.
        - Can generate invoices.
    - Administrator:
        - Has all operator permissions.
        - Can manage users and permissions.
        - Can view data from all branches.
        - Can configure store settings (Tenant registration, business info, invoice settings).

## Maintenance (CRUDs):
    - Clients:
        - Fields: name, phone, date of birth.
        - View: purchase history and invoices
    - Products:
        - Fields: name, brand, model, category, color, quantity, purchase price, sale price, TenantId.
        - Option to import data via XML.
    - Sales:
        - Fields: client, products, date/time, payment method, total, TenantId.
    - Users:
        - Fields: name, email, password, phone, role, TenantId.
    - Invoices (Nf-e/NFC-e):
        - Auto-generated upon sale confirmation.
        - Fields: sale ID, date, status, XML link, PDF link.

## Future Enhancements (Beyond MVP):
    - Inventory transfer between stores.
    - Vendor performance reports and sales analytics.