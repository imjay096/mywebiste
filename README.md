<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DMI Consolidated Pricelist & Sales Dashboard</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            margin: 15px; 
            background-color: #f4f7f6; 
            color: #333;
        }
        .header { 
            text-align: center; 
            background-color: #ffffff; 
            padding: 15px; 
            border-radius: 8px; 
            box-shadow: 0 4px 6px rgba(0,0,0,0.05); 
            margin-bottom: 15px; 
        }
        .header h1 { margin: 0; color: #004080; font-size: 22px; }
        .header p { margin: 3px 0; font-size: 13px; color: #555; }
        .developer-tag {
            font-size: 12px;
            color: #004080;
            font-weight: bold;
            margin-top: 6px;
        }

        /* Navigation Tabs */
        .nav-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }
        .tab-btn {
            padding: 10px 20px;
            font-size: 14px;
            font-weight: bold;
            border: none;
            background-color: #e0e0e0;
            color: #333;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.3s;
        }
        .tab-btn.active {
            background-color: #004080;
            color: #fff;
        }

        .tab-content { display: none; }
        .tab-content.active { display: block; }

        /* Dashboard Metric Cards */
        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        .metric-card {
            background: #fff;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.08);
            border-left: 5px solid #004080;
            text-align: center;
        }
        .metric-card.today { border-left-color: #28a745; }
        .metric-card.mago { border-left-color: #17a2b8; }
        .metric-card.yago { border-left-color: #ffc107; }
        .metric-card h4 { margin: 0; color: #666; font-size: 13px; text-transform: uppercase; }
        .metric-card .amount { font-size: 22px; font-weight: bold; color: #004080; margin-top: 8px; }

        .search-container { 
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px; 
            flex-wrap: wrap;
        }
        #searchInput { 
            width: 100%; 
            max-width: 400px; 
            padding: 10px 15px; 
            font-size: 15px; 
            border: 2px solid #ccc; 
            border-radius: 30px; 
            outline: none; 
        }
        #searchInput:focus { border-color: #004080; }

        .sort-select {
            padding: 9px 12px;
            font-size: 13px;
            border: 2px solid #ccc;
            border-radius: 20px;
            background-color: #fff;
            outline: none;
            font-weight: bold;
            cursor: pointer;
        }

        .table-container {
            overflow-x: auto;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            max-height: 65vh;
            background: #fff;
        }
        table { 
            width: 100%; 
            border-collapse: collapse; 
            background-color: #ffffff; 
        }
        th, td { 
            padding: 10px 12px; 
            text-align: center; 
            border-bottom: 1px solid #eeeeee; 
            vertical-align: middle;
            font-size: 14px;
        }
        th { 
            background-color: #004080; 
            color: #ffffff; 
            position: sticky; 
            top: 0; 
            z-index: 10;
            white-space: nowrap;
        }
        th.sortable {
            cursor: pointer;
            user-select: none;
        }
        th.sortable:hover {
            background-color: #002b55;
        }
        tr:hover { background-color: #f0f8ff; }
        .product-image { 
            width: 50px; 
            height: 50px; 
            object-fit: cover; 
            border-radius: 6px; 
            border: 1px solid #ddd;
            cursor: pointer;
        }
        .desc-col { text-align: left; font-weight: 600; }

        /* Action Column Layout Styles */
        .action-box {
            display: flex;
            flex-direction: column;
            gap: 6px;
            align-items: center;
            justify-content: center;
            min-width: 210px;
        }
        .action-controls {
            display: flex;
            align-items: center;
            gap: 5px;
            background: #f8f9fa;
            padding: 5px 8px;
            border-radius: 6px;
            border: 1px solid #e2e8f0;
            width: 100%;
            box-sizing: border-box;
            justify-content: center;
        }
        .cs-config-box {
            display: none;
            font-size: 11px;
            color: #004080;
            align-items: center;
            justify-content: center;
            gap: 6px;
            background: #eef5fc;
            padding: 4px 8px;
            border-radius: 4px;
            border: 1px solid #b8daff;
            width: 100%;
            box-sizing: border-box;
        }

        /* Buttons */
        .btn {
            padding: 6px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            font-size: 13px;
        }
        .btn-primary { background-color: #004080; color: white; }
        .btn-success { background-color: #28a745; color: white; }
        .btn-warning { background-color: #ffc107; color: #333; }
        .btn-danger { background-color: #dc3545; color: white; }
        .btn-info { background-color: #17a2b8; color: white; }
        .btn-secondary { background-color: #6c757d; color: white; }

        /* Badges */
        .badge {
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
            display: inline-block;
        }
        .badge-success { background-color: #d4edda; color: #155724; }
        .badge-danger { background-color: #f8d7da; color: #721c24; }

        /* Customer Cards */
        .customer-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 15px;
        }
        .customer-card {
            background: #fff;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            border-left: 5px solid #004080;
        }
        .customer-card h3 { margin: 0 0 5px 0; color: #004080; }
        .customer-card p { margin: 3px 0; font-size: 13px; color: #555; }

        /* Floating Cart Button */
        .cart-float {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: #28a745;
            color: white;
            padding: 12px 20px;
            border-radius: 30px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            cursor: pointer;
            font-weight: bold;
            z-index: 100;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Modal Styles */
        .modal {
            display: none; 
            position: fixed; 
            z-index: 1000; 
            left: 0; top: 0;
            width: 100%; height: 100%; 
            background-color: rgba(0, 0, 0, 0.6); 
            justify-content: center;
            align-items: center;
        }
        .modal-body {
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 650px;
            max-height: 85vh;
            overflow-y: auto;
            position: relative;
        }
        .close-btn {
            position: absolute;
            top: 10px; right: 15px;
            font-size: 24px; font-weight: bold;
            cursor: pointer; color: #888;
        }
        .form-group {
            margin-bottom: 12px;
            text-align: left;
        }
        .form-group label { display: block; font-weight: bold; margin-bottom: 4px; font-size: 13px; }
        .form-group input, .form-group select {
            width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px; box-sizing: border-box;
        }
        .summary-box {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 6px;
            margin-top: 10px;
            font-size: 14px;
        }
        .time-breakdown {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 10px;
            margin: 15px 0;
            text-align: center;
        }
        .time-box {
            background: #eef5fc;
            padding: 10px;
            border-radius: 6px;
            border: 1px solid #b8daff;
        }
        .time-box div { font-size: 11px; color: #555; text-transform: uppercase; font-weight: bold; }
        .time-box span { font-size: 15px; font-weight: bold; color: #004080; }
        .history-item {
            border-bottom: 1px dashed #ccc;
            padding: 10px 0;
        }
        .qty-input {
            width: 55px;
            text-align: center;
            font-weight: bold;
            padding: 4px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        footer {
            text-align: center;
            margin-top: 25px;
            padding: 12px;
            font-size: 12px;
            color: #666;
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        /* Print Media Styles */
        @media print {
            body * {
                visibility: hidden;
            }
            #printableReceiptModal, #printableReceiptModal * {
                visibility: visible;
            }
            #printableReceiptModal {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                margin: 0;
                padding: 0;
                background: #fff;
            }
            .no-print {
                display: none !important;
            }
        }
    </style>
</head>
<body>

<div class="header">
    <h1>DMI Pricelist & Sales Management System</h1>
    <p><strong>DYON MARKETING INCORPORATED</strong></p>
    <p>1149 Teodoro San Luis St. Pandacan Manila | Tel: 8528-1484</p>
    <p class="developer-tag">Developer: IMJAY ROSALES</p>
</div>

<!-- Navigation Tabs -->
<div class="nav-tabs">
    <button class="tab-btn active" onclick="switchTab('pricelist')">📋 Pricelist & Order</button>
    <button class="tab-btn" onclick="switchTab('customers')">👥 Customers Directory</button>
    <button class="tab-btn" onclick="switchTab('dashboard')">📊 Sales Dashboard</button>
</div>

<!-- TAB 1: PRICELIST & CATALOG -->
<div id="pricelistTab" class="tab-content active">
    <div class="search-container">
        <input type="text" id="searchInput" onkeyup="searchTable()" placeholder="🔍 Search product by Code or Description...">
        <select id="productSortSelect" class="sort-select" onchange="sortProductsBySelect(this.value)">
            <option value="">Sort Items By...</option>
            <option value="nameAsc">Name (A-Z)</option>
            <option value="nameDesc">Name (Z-A)</option>
            <option value="codeAsc">Code (Ascending)</option>
        </select>
        <button class="btn btn-success" onclick="openAddProductModal()">+ Add New Product</button>
    </div>

    <div style="margin-bottom: 10px; background: #e8f4fd; padding: 10px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center;">
        <div>
            <strong>Selected Customer:</strong> 
            <span id="selectedCustomerName" style="color: #004080; font-weight: bold;">Please select from Customers Directory</span>
        </div>
        <button class="btn btn-primary" onclick="switchTab('customers')">Select / Change</button>
    </div>

    <div class="table-container">
        <table id="priceTable">
            <thead>
                <tr>
                    <th>Picture</th>
                    <th>Item Code</th>
                    <th class="sortable" onclick="toggleSortProducts('name')" title="Click to sort by Name">
                        Product Description <span id="sortNameIcon">⇅</span>
                    </th>
                    <th>Old Price</th>
                    <th>List Price (PC)</th>
                    <th>SRP</th>
                    <th>Action (Select UOM)</th>
                </tr>
            </thead>
            <tbody id="tableBody"></tbody>
        </table>
    </div>
</div>

<!-- TAB 2: CUSTOMERS DIRECTORY -->
<div id="customersTab" class="tab-content">
    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; flex-wrap: wrap; gap: 10px;">
        <h2>Customers Directory</h2>
        <div style="display: flex; gap: 10px; align-items: center; flex-wrap: wrap;">
            <label style="font-weight: bold; font-size: 13px;">Sort Directory:</label>
            <select id="customerSortSelect" class="sort-select" onchange="renderCustomers()">
                <option value="nameAsc">Customer Name (A - Z)</option>
                <option value="nameDesc">Customer Name (Z - A)</option>
                <option value="storeAsc">Store Name (A - Z)</option>
                <option value="storeDesc">Store Name (Z - A)</option>
                <option value="dateDesc">Date Added (Newest First)</option>
                <option value="dateAsc">Date Added (Oldest First)</option>
            </select>
            <button class="btn btn-success" onclick="openCustomerModal()">+ Add New Customer</button>
        </div>
    </div>

    <div id="customerGrid" class="customer-grid"></div>
</div>

<!-- TAB 3: SALES DASHBOARD -->
<div id="dashboardTab" class="tab-content">
    <h2>Sales Performance & Customer Status</h2>
    
    <!-- Top Metrics -->
    <div class="metrics-grid">
        <div class="metric-card today">
            <h4>Sales Today</h4>
            <div class="amount" id="metricToday">₱0.00</div>
        </div>
        <div class="metric-card">
            <h4>Sales (This Month)</h4>
            <div class="amount" id="metricThisMonth">₱0.00</div>
        </div>
        <div class="metric-card mago">
            <h4>MAGO (Month Ago Sales)</h4>
            <div class="amount" id="metricMAGO">₱0.00</div>
        </div>
        <div class="metric-card yago">
            <h4>YAGO (Year Ago Sales)</h4>
            <div class="amount" id="metricYAGO">₱0.00</div>
        </div>
    </div>

    <h3>Customer Purchase Status (This Month)</h3>
    <div class="table-container">
        <table>
            <thead>
                <tr>
                    <th>Store Name</th>
                    <th>Customer Name</th>
                    <th>Contact</th>
                    <th>Status (This Month)</th>
                    <th>Sales (This Month)</th>
                    <th>Last Order Date</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody id="dashboardCustomerBody"></tbody>
        </table>
    </div>
</div>

<!-- Floating Cart Button -->
<div id="cartFloat" class="cart-float" onclick="openCartModal()" style="display: none;">
    🛒 Order Cart (<span id="cartCount">0</span>)
</div>

<!-- MODAL: ADD/EDIT PRODUCT -->
<div id="addProductModal" class="modal">
    <div class="modal-body">
        <span class="close-btn" onclick="closeModal('addProductModal')">&times;</span>
        <h3 id="addProductModalTitle">Add New Product</h3>
        <form id="productForm" onsubmit="saveProduct(event)">
            <div class="form-group">
                <label>Item Code *</label>
                <input type="text" id="prodCode" required placeholder="e.g. 05999">
            </div>
            <div class="form-group">
                <label>Product Description *</label>
                <input type="text" id="prodName" required placeholder="e.g. Premium Beef Franks 500g">
            </div>
            <div class="form-group">
                <label>Old Price (₱)</label>
                <input type="number" step="0.01" id="prodOldPrice" placeholder="e.g. 120.00">
            </div>
            <div class="form-group">
                <label>List Price (₱) *</label>
                <input type="number" step="0.01" id="prodListPrice" required placeholder="e.g. 115.00">
            </div>
            <div class="form-group">
                <label>SRP (₱)</label>
                <input type="number" step="0.01" id="prodSRP" placeholder="e.g. 125.00">
            </div>
            <button type="submit" class="btn btn-primary" style="width: 100%; margin-top: 10px;">Save Product</button>
        </form>
    </div>
</div>

<!-- MODAL: ADD / EDIT CUSTOMER -->
<div id="customerModal" class="modal">
    <div class="modal-body">
        <span class="close-btn" onclick="closeModal('customerModal')">&times;</span>
        <h3 id="customerModalTitle">Add / Edit Customer</h3>
        <form id="customerForm" onsubmit="saveCustomer(event)">
            <input type="hidden" id="custId">
            <div class="form-group">
                <label>Customer Name *</label>
                <input type="text" id="custName" required placeholder="e.g. Juan Dela Cruz">
            </div>
            <div class="form-group">
                <label>Store Name *</label>
                <input type="text" id="custStore" required placeholder="e.g. Dela Cruz Store">
            </div>
            <div class="form-group">
                <label>Address *</label>
                <input type="text" id="custAddress" required placeholder="e.g. 123 Main St., Manila">
            </div>
            <div class="form-group">
                <label>Contact Number *</label>
                <input type="text" id="custContact" required placeholder="e.g. 09171234567">
            </div>
            <button type="submit" class="btn btn-primary" style="width: 100%; margin-top: 10px;">Save Customer Details</button>
        </form>
    </div>
</div>

<!-- MODAL: CART & CHECKOUT -->
<div id="cartModal" class="modal">
    <div class="modal-body">
        <span class="close-btn" onclick="closeModal('cartModal')">&times;</span>
        <h3 id="cartModalTitle">Confirm Order</h3>
        <p><strong>Customer:</strong> <span id="cartCustName">-</span></p>
        
        <div id="cartItemsList" style="max-height: 250px; overflow-y: auto; margin-bottom: 15px; border-top: 1px solid #ddd;"></div>

        <div class="form-group">
            <label>Select Discount Option:</label>
            <select id="discountSelect" onchange="updateCartCalculations()">
                <option value="none">No Discount (0%)</option>
                <option value="3percent">Less 3% Discount (-3%)</option>
                <option value="3plus2">Less 3% + Less 2% Discount (-3% + -2%)</option>
            </select>
        </div>

        <div class="summary-box">
            <div style="display: flex; justify-content: space-between;">
                <span>Subtotal:</span> <span id="summarySubtotal">₱0.00</span>
            </div>
            <div style="display: flex; justify-content: space-between; color: #d9534f;">
                <span>Discount:</span> <span id="summaryDiscount">-₱0.00</span>
            </div>
            <hr>
            <div style="display: flex; justify-content: space-between; font-weight: bold; font-size: 16px; color: #28a745;">
                <span>Total Amount:</span> <span id="summaryTotal">₱0.00</span>
            </div>
        </div>

        <div style="display: flex; gap: 10px; margin-top: 15px;">
            <button class="btn btn-primary" onclick="browseMoreItems()" style="flex: 1; padding: 10px;">+ Add More Items</button>
            <button class="btn btn-danger" onclick="cancelOrder()" style="flex: 1; padding: 10px;">Cancel Edit</button>
        </div>
        <button class="btn btn-success" onclick="submitOrder()" style="width: 100%; margin-top: 10px; padding: 10px;">Finalize & Save Order</button>
    </div>
</div>

<!-- MODAL: CUSTOMER DETAILS & HISTORY -->
<div id="historyModal" class="modal">
    <div class="modal-body">
        <span class="close-btn" onclick="closeModal('historyModal')">&times;</span>
        <h3 style="margin-bottom: 5px;">Customer Detail & History</h3>
        <p style="margin: 0;"><strong>Store:</strong> <span id="historyCustStore">-</span></p>
        <p style="margin: 0; font-size: 13px; color: #555;"><strong>Owner:</strong> <span id="historyCustName">-</span> | <strong>Contact:</strong> <span id="historyCustContact">-</span></p>

        <!-- Spending Breakdown -->
        <div class="time-breakdown">
            <div class="time-box">
                <div>This Week</div>
                <span id="spentThisWeek">₱0.00</span>
            </div>
            <div class="time-box">
                <div>This Month</div>
                <span id="spentThisMonth">₱0.00</span>
            </div>
            <div class="time-box">
                <div>This Year</div>
                <span id="spentThisYear">₱0.00</span>
            </div>
        </div>

        <h4 style="margin-top: 15px; margin-bottom: 5px; color: #004080;">Items Purchased Summary</h4>
        <div class="table-container" style="max-height: 150px; margin-bottom: 15px;">
            <table>
                <thead>
                    <tr>
                        <th>Code</th>
                        <th>Item Description</th>
                        <th>Total Qty</th>
                        <th>Total Amount</th>
                    </tr>
                </thead>
                <tbody id="historyItemSummaryBody"></tbody>
            </table>
        </div>

        <h4 style="margin-bottom: 5px; color: #004080;">Order Transaction History</h4>
        <div id="historyList" style="max-height: 250px; overflow-y: auto; border: 1px solid #eee; padding: 10px; border-radius: 6px;"></div>
    </div>
</div>

<!-- MODAL: PRINT ORDER RECEIPT -->
<div id="printReceiptModal" class="modal">
    <div class="modal-body" style="max-width: 700px;">
        <span class="close-btn no-print" onclick="closeModal('printReceiptModal')">&times;</span>
        
        <div id="printableReceiptModal">
            <div id="printReceiptContent"></div>
        </div>

        <div class="no-print" style="margin-top: 20px; display: flex; justify-content: flex-end; gap: 10px;">
            <button class="btn btn-secondary" onclick="closeModal('printReceiptModal')">Close</button>
            <button class="btn btn-primary" onclick="executePrint()">🖨️ Print Now</button>
        </div>
    </div>
</div>

<!-- MODAL: IMAGE FULL SIZE -->
<div id="imageModal" class="modal" onclick="closeModal('imageModal')">
    <span class="close-btn" style="color:#fff;" onclick="closeModal('imageModal')">&times;</span>
    <img id="imgFullSize" style="max-width: 90%; max-height: 85vh; border-radius: 8px;" onclick="event.stopPropagation()">
</div>

<footer>
    Designed & Developed by <strong>IMJAY ROSALES</strong> | DMI Sales Management System
</footer>

<script>
    // Placeholder default products
    const defaultProducts = [
        ["04368", "Beef Franks Classic Regular 250g", "63.00", "65.00", ""]
    ];

    // State Variables
    let products = JSON.parse(localStorage.getItem('dmi_products')) || defaultProducts;
    let customers = JSON.parse(localStorage.getItem('dmi_customers')) || [];
    let orders = JSON.parse(localStorage.getItem('dmi_orders')) || [];
    let activeCustomer = null;
    let cart = [];
    let editingOrderId = null;
    let editingProductCode = null;
    let currentItemSort = 'none'; // 'nameAsc', 'nameDesc', 'codeAsc'

    // Initialize Page
    document.addEventListener("DOMContentLoaded", () => {
        renderProducts();
        renderCustomers();
        renderDashboard();
    });

    // Date Helpers
    function parseOrderDate(o) {
        if (!o) return new Date(0);
        if (o.timestamp) {
            const d = new Date(o.timestamp);
            if (!isNaN(d.getTime())) return d;
        }
        if (o.dateFormatted) {
            const d = new Date(o.dateFormatted);
            if (!isNaN(d.getTime())) return d;
        }
        return new Date(0);
    }

    function isToday(dateObj) {
        if (!dateObj || isNaN(dateObj.getTime())) return false;
        const now = new Date();
        return dateObj.getDate() === now.getDate() &&
               dateObj.getMonth() === now.getMonth() &&
               dateObj.getFullYear() === now.getFullYear();
    }

    function isThisWeek(dateObj) {
        if (!dateObj || isNaN(dateObj.getTime())) return false;
        const now = new Date();
        const startOfWeek = new Date(now);
        startOfWeek.setHours(0, 0, 0, 0);
        startOfWeek.setDate(now.getDate() - now.getDay());
        const endOfWeek = new Date(startOfWeek);
        endOfWeek.setDate(endOfWeek.getDate() + 7);
        return dateObj >= startOfWeek && dateObj < endOfWeek;
    }

    function isThisMonth(dateObj) {
        if (!dateObj || isNaN(dateObj.getTime())) return false;
        const now = new Date();
        return dateObj.getFullYear() === now.getFullYear() && dateObj.getMonth() === now.getMonth();
    }

    function isThisYear(dateObj) {
        if (!dateObj || isNaN(dateObj.getTime())) return false;
        const now = new Date();
        return dateObj.getFullYear() === now.getFullYear();
    }

    function isMAGO(dateObj) {
        if (!dateObj || isNaN(dateObj.getTime())) return false;
        const now = new Date();
        let targetMonth = now.getMonth() - 1;
        let targetYear = now.getFullYear();
        if (targetMonth < 0) {
            targetMonth = 11;
            targetYear -= 1;
        }
        return dateObj.getFullYear() === targetYear && dateObj.getMonth() === targetMonth;
    }

    function isYAGO(dateObj) {
        if (!dateObj || isNaN(dateObj.getTime())) return false;
        const now = new Date();
        return dateObj.getFullYear() === (now.getFullYear() - 1) && dateObj.getMonth() === now.getMonth();
    }

    // Switch Tabs
    function switchTab(tabName) {
        document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
        document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));

        if(tabName === 'pricelist') {
            document.querySelectorAll('.tab-btn')[0].classList.add('active');
            document.getElementById('pricelistTab').classList.add('active');
        } else if(tabName === 'customers') {
            document.querySelectorAll('.tab-btn')[1].classList.add('active');
            document.getElementById('customersTab').classList.add('active');
        } else if(tabName === 'dashboard') {
            document.querySelectorAll('.tab-btn')[2].classList.add('active');
            document.getElementById('dashboardTab').classList.add('active');
            renderDashboard();
        }
    }

    // PRODUCT MANAGEMENT & SORTING
    function toggleSortProducts(type) {
        if (type === 'name') {
            if (currentItemSort === 'nameAsc') {
                currentItemSort = 'nameDesc';
            } else {
                currentItemSort = 'nameAsc';
            }
        }
        document.getElementById('productSortSelect').value = currentItemSort;
        sortAndRenderProducts();
    }

    function sortProductsBySelect(val) {
        currentItemSort = val;
        sortAndRenderProducts();
    }

    function sortAndRenderProducts() {
        if (currentItemSort === 'nameAsc') {
            products.sort((a, b) => (a[1] || '').localeCompare(b[1] || ''));
        } else if (currentItemSort === 'nameDesc') {
            products.sort((a, b) => (b[1] || '').localeCompare(a[1] || ''));
        } else if (currentItemSort === 'codeAsc') {
            products.sort((a, b) => (a[0] || '').localeCompare(b[0] || ''));
        }
        
        const iconSpan = document.getElementById('sortNameIcon');
        if (iconSpan) {
            if (currentItemSort === 'nameAsc') iconSpan.innerText = ' 🔼';
            else if (currentItemSort === 'nameDesc') iconSpan.innerText = ' 🔽';
            else iconSpan.innerText = ' ⇅';
        }

        renderProducts();
    }

    function openAddProductModal() {
        editingProductCode = null;
        document.getElementById('productForm').reset();
        document.getElementById('prodCode').disabled = false;
        document.getElementById('addProductModalTitle').innerText = 'Add New Product';
        document.getElementById('addProductModal').style.display = 'flex';
    }

    function editProduct(code) {
        const prod = products.find(p => p[0] === code);
        if(prod) {
            editingProductCode = code;
            document.getElementById('prodCode').value = prod[0];
            document.getElementById('prodCode').disabled = true;
            document.getElementById('prodName').value = prod[1];
            document.getElementById('prodOldPrice').value = prod[2] ? prod[2].replace(/,/g, '') : '';
            document.getElementById('prodListPrice').value = prod[3] ? prod[3].replace(/,/g, '') : '';
            document.getElementById('prodSRP').value = prod[4] ? prod[4].replace(/,/g, '') : '';
            
            document.getElementById('addProductModalTitle').innerText = 'Edit Product Price / Details';
            document.getElementById('addProductModal').style.display = 'flex';
        }
    }

    function saveProduct(e) {
        e.preventDefault();
        const code = document.getElementById('prodCode').value.trim();
        const name = document.getElementById('prodName').value.trim();
        const oldP = document.getElementById('prodOldPrice').value.trim();
        const listP = document.getElementById('prodListPrice').value.trim();
        const srp = document.getElementById('prodSRP').value.trim();

        if(editingProductCode) {
            const index = products.findIndex(p => p[0] === editingProductCode);
            if(index !== -1) {
                products[index][1] = name;
                products[index][2] = oldP;
                products[index][3] = listP;
                products[index][4] = srp;
            }
            alert("Product updated successfully!");
        } else {
            if(products.find(p => p[0] === code)) {
                alert("Error: Item Code already exists!");
                return;
            }
            products.push([code, name, oldP, listP, srp]);
            alert("New product added!");
        }
        
        localStorage.setItem('dmi_products', JSON.stringify(products));
        sortAndRenderProducts();
        closeModal('addProductModal');
    }

    // Update UOM Display in Pricelist
    function updateUOM(code, basePrice) {
        const uom = document.getElementById(`uom-${code}`).value;
        const configDiv = document.getElementById(`cs-config-${code}`);
        const pcsInput = document.getElementById(`pcs-${code}`);
        const preview = document.getElementById(`preview-${code}`);
        
        if(uom === 'cs') {
            configDiv.style.display = 'flex';
            let pcs = parseInt(pcsInput.value) || 1;
            preview.innerText = '₱' + (basePrice * pcs).toLocaleString('en-US', {minimumFractionDigits: 2});
        } else {
            configDiv.style.display = 'none';
        }
    }

    // Render Pricelist
    function renderProducts() {
        const tbody = document.getElementById('tableBody');
        tbody.innerHTML = '';
        products.forEach(item => {
            let tr = document.createElement('tr');
            let oldPriceStr = item[2] ? '₱' + parseFloat(item[2].replace(/,/g, '')).toFixed(2) : '-';
            let newPriceVal = item[3] ? parseFloat(item[3].replace(/,/g, '')) : 0;
            let newPriceStr = item[3] ? '₱' + newPriceVal.toFixed(2) : '-';
            let srpStr = item[4] ? '₱' + parseFloat(item[4].replace(/,/g, '')).toFixed(2) : '-';

            tr.innerHTML = `
                <td>
                    <img src="images/${item[0]}.jpg" 
                         onerror="this.onerror=null; this.src='https://via.placeholder.com/50?text=No+Img';" 
                         alt="Pic" class="product-image" onclick="openImageModal('images/${item[0]}.jpg')">
                </td>
                <td><strong>${item[0]}</strong></td>
                <td class="desc-col">${item[1]}</td>
                <td style="color: #777;">${oldPriceStr}</td>
                <td style="color: #d9534f; font-weight: bold;">${newPriceStr}</td>
                <td style="color: #5cb85c; font-weight: bold;">${srpStr}</td>
                <td>
                    <div class="action-box">
                        <button class="btn btn-warning" style="font-size: 11px; width: 100%; padding: 4px;" onclick="editProduct('${item[0]}')">✏️ Edit Price/Detail</button>
                        <div class="action-controls">
                            <input type="number" id="qty-${item[0]}" value="1" min="1" class="qty-input" title="Quantity">
                            <select id="uom-${item[0]}" onchange="updateUOM('${item[0]}', ${newPriceVal})" style="padding: 4px; border: 1px solid #ccc; border-radius: 4px; font-weight: bold;">
                                <option value="pc">PC</option>
                                <option value="cs">CS</option>
                            </select>
                            <button class="btn btn-primary" onclick="addToCartUOM('${item[0]}', '${escapeHtml(item[1])}', ${newPriceVal})">+ Add</button>
                        </div>
                        <div id="cs-config-${item[0]}" class="cs-config-box">
                            <span>Pcs/CS:</span>
                            <input type="number" id="pcs-${item[0]}" value="12" min="1" class="qty-input" style="width: 42px;" onchange="updateUOM('${item[0]}', ${newPriceVal})">
                            <span id="preview-${item[0]}" style="color: #d9534f; font-weight: bold;">₱${(newPriceVal * 12).toFixed(2)}</span>
                        </div>
                    </div>
                </td>
            `;
            tbody.appendChild(tr);
        });
    }

    function escapeHtml(str) { return str.replace(/'/g, "\\'"); }

    // CUSTOMERS DIRECTORY MANAGEMENT & SORTING
    function renderCustomers() {
        const grid = document.getElementById('customerGrid');
        grid.innerHTML = '';

        if(customers.length === 0) {
            grid.innerHTML = '<p style="grid-column: 1/-1; text-align: center; color: #777;">No registered customers yet.</p>';
            return;
        }

        const sortVal = document.getElementById('customerSortSelect') ? document.getElementById('customerSortSelect').value : 'nameAsc';
        let sortedCust = [...customers];

        sortedCust.sort((a, b) => {
            if (sortVal === 'nameAsc') {
                return (a.name || '').localeCompare(b.name || '');
            } else if (sortVal === 'nameDesc') {
                return (b.name || '').localeCompare(a.name || '');
            } else if (sortVal === 'storeAsc') {
                return (a.storeName || '').localeCompare(b.storeName || '');
            } else if (sortVal === 'storeDesc') {
                return (b.storeName || '').localeCompare(a.storeName || '');
            } else if (sortVal === 'dateDesc') {
                let timeA = parseInt(String(a.id).replace('CUST-', '')) || 0;
                let timeB = parseInt(String(b.id).replace('CUST-', '')) || 0;
                return timeB - timeA;
            } else if (sortVal === 'dateAsc') {
                let timeA = parseInt(String(a.id).replace('CUST-', '')) || 0;
                let timeB = parseInt(String(b.id).replace('CUST-', '')) || 0;
                return timeA - timeB;
            }
            return 0;
        });

        sortedCust.forEach(c => {
            const card = document.createElement('div');
            card.className = 'customer-card';
            card.innerHTML = `
                <h3>${c.storeName}</h3>
                <p><strong>Owner:</strong> ${c.name}</p>
                <p><strong>Address:</strong> ${c.address}</p>
                <p><strong>Contact:</strong> ${c.contact}</p>
                <div style="margin-top: 10px; display: flex; gap: 5px; flex-wrap: wrap;">
                    <button class="btn btn-success" onclick="selectCustomerForOrder('${c.id}')">🛒 Order</button>
                    <button class="btn btn-info" onclick="viewCustomerHistory('${c.id}')">📜 History</button>
                    <button class="btn btn-warning" onclick="editCustomer('${c.id}')">✏ Edit</button>
                    <button class="btn btn-danger" onclick="deleteCustomer('${c.id}')">🗑️</button>
                </div>
            `;
            grid.appendChild(card);
        });
    }

    function openCustomerModal(id = null) {
        document.getElementById('customerForm').reset();
        document.getElementById('custId').value = '';
        
        if (id) {
            const cust = customers.find(c => String(c.id) === String(id));
            if(cust) {
                document.getElementById('customerModalTitle').innerText = 'Edit Customer Details';
                document.getElementById('custId').value = cust.id;
                document.getElementById('custName').value = cust.name;
                document.getElementById('custStore').value = cust.storeName;
                document.getElementById('custAddress').value = cust.address;
                document.getElementById('custContact').value = cust.contact;
            }
        } else {
            document.getElementById('customerModalTitle').innerText = 'Add New Customer';
        }

        document.getElementById('customerModal').style.display = 'flex';
    }

    function editCustomer(id) {
        openCustomerModal(id);
    }

    function saveCustomer(e) {
        e.preventDefault();
        const id = document.getElementById('custId').value;
        const name = document.getElementById('custName').value.trim();
        const storeName = document.getElementById('custStore').value.trim();
        const address = document.getElementById('custAddress').value.trim();
        const contact = document.getElementById('custContact').value.trim();

        if (id) {
            const index = customers.findIndex(c => String(c.id) === String(id));
            if (index !== -1) {
                customers[index].name = name;
                customers[index].storeName = storeName;
                customers[index].address = address;
                customers[index].contact = contact;
            }
        } else {
            customers.push({ id: 'CUST-' + Date.now(), name, storeName, address, contact });
        }

        localStorage.setItem('dmi_customers', JSON.stringify(customers));
        renderCustomers();
        closeModal('customerModal');
        alert(id ? 'Customer updated successfully!' : 'New customer added!');
    }

    function deleteCustomer(id) {
        if(confirm('Are you sure you want to delete this customer?')) {
            customers = customers.filter(c => String(c.id) !== String(id));
            localStorage.setItem('dmi_customers', JSON.stringify(customers));
            renderCustomers();
            if(activeCustomer && String(activeCustomer.id) === String(id)) {
                activeCustomer = null;
                document.getElementById('selectedCustomerName').innerText = 'Please select from Customers Directory';
            }
        }
    }

    // ORDERING & CART
    function selectCustomerForOrder(id) {
        activeCustomer = customers.find(c => String(c.id) === String(id));
        if (activeCustomer) {
            document.getElementById('selectedCustomerName').innerText = `${activeCustomer.storeName} (${activeCustomer.name})`;
            switchTab('pricelist');
        }
    }

    function addToCartUOM(code, name, basePrice) {
        if(!activeCustomer) {
            alert('Please select a Customer first before adding items to order.');
            switchTab('customers');
            return;
        }

        const qtyInput = document.getElementById(`qty-${code}`);
        const uom = document.getElementById(`uom-${code}`).value;
        const pcsInput = document.getElementById(`pcs-${code}`);
        
        let qtyToAdd = parseInt(qtyInput.value, 10);
        if (isNaN(qtyToAdd) || qtyToAdd <= 0) qtyToAdd = 1;

        let finalPrice = basePrice;
        let itemName = name;
        let cartKey = code;
        let pcsPerCase = 1;

        if (uom === 'cs') {
            let pcs = parseInt(pcsInput.value, 10) || 1;
            pcsPerCase = pcs;
            finalPrice = basePrice * pcs;
            itemName = `${name} [CASE: ${pcs}pcs]`;
            cartKey = `${code}-CS-${pcs}`; 
        } else {
            itemName = `${name} [PC]`;
            cartKey = `${code}-PC`;
        }

        let existing = cart.find(item => item.cartKey === cartKey);
        if(existing) {
            existing.qty += qtyToAdd;
        } else {
            cart.push({ cartKey, code, name: itemName, price: finalPrice, qty: qtyToAdd, uom: uom, pcsPerCase: pcsPerCase });
        }
        
        if (qtyInput) qtyInput.value = 1;
        updateCartFloat();
    }

    function updateCartFloat() {
        const float = document.getElementById('cartFloat');
        const countSpan = document.getElementById('cartCount');
        let totalItems = cart.reduce((sum, i) => sum + i.qty, 0);

        if(totalItems > 0) {
            float.style.display = 'flex';
            countSpan.innerText = totalItems;
        } else {
            float.style.display = 'none';
        }
    }

    function openCartModal() {
        if(!activeCustomer || cart.length === 0) return;

        document.getElementById('cartModalTitle').innerText = editingOrderId ? 'Edit Order Details' : 'Confirm Order';
        document.getElementById('cartCustName').innerText = `${activeCustomer.storeName} (${activeCustomer.name})`;
        renderCartList();
        updateCartCalculations();
        document.getElementById('cartModal').style.display = 'flex';
    }

    function renderCartList() {
        const container = document.getElementById('cartItemsList');
        container.innerHTML = '';
        cart.forEach((item, idx) => {
            let uomSelect = `
                <select onchange="changeCartItemUOM(${idx}, this.value)" style="padding: 2px; font-size: 11px; border-radius: 3px; border: 1px solid #ccc;">
                    <option value="pc" ${item.uom === 'pc' ? 'selected' : ''}>PC</option>
                    <option value="cs" ${item.uom === 'cs' ? 'selected' : ''}>CS</option>
                </select>
            `;
            
            let csConfig = `
                <span style="display: ${item.uom === 'cs' ? 'inline' : 'none'};">
                    <input type="number" value="${item.pcsPerCase || extractPcs(item.name)}" onchange="updateCartItemCS(${idx}, this.value)" style="width: 40px; font-size: 11px; padding: 2px;" title="Pcs per Case"> pcs
                </span>
            `;

            const div = document.createElement('div');
            div.style.cssText = 'display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid #eee;';
            div.innerHTML = `
                <div style="flex: 2; text-align: left;">
                    <div style="font-weight: bold; font-size: 13px;">${item.name}</div>
                    <div style="font-size: 12px; color: #777; margin-top: 4px; display: flex; align-items: center; gap: 5px;">
                        ₱${item.price.toFixed(2)} / 
                        ${uomSelect}
                        ${csConfig}
                    </div>
                </div>
                <div style="display: flex; align-items: center; gap: 4px;">
                    <button class="btn btn-warning" onclick="changeQty(${idx}, -1)" style="padding: 2px 8px; font-weight: bold;">-</button>
                    <input type="number" min="1" value="${item.qty}" onchange="setQtyDirect(${idx}, this.value)" class="qty-input">
                    <button class="btn btn-warning" onclick="changeQty(${idx}, 1)" style="padding: 2px 8px; font-weight: bold;">+</button>
                </div>
                <div style="flex: 1; text-align: right; font-weight: bold;">
                    ₱${(item.price * item.qty).toFixed(2)}
                </div>
            `;
            container.appendChild(div);
        });
    }

    function extractPcs(name) {
        if (!name) return 12;
        let match = name.match(/\[CASE: (\d+)pcs\]/i);
        return match ? parseInt(match[1]) : 12;
    }

    function changeCartItemUOM(index, newUOM) {
        let item = cart[index];
        let prod = products.find(p => p[0] === item.code);
        let basePrice = prod ? parseFloat(prod[3].replace(/,/g, '')) : (item.price / (item.uom === 'cs' ? (item.pcsPerCase || extractPcs(item.name)) : 1));
        let baseName = prod ? prod[1] : item.name.replace(/ \[(PC\vert{}CASE).*\]/g, '');

        if (newUOM === 'cs') {
            let pcs = item.pcsPerCase || extractPcs(item.name) || 12;
            item.uom = 'cs';
            item.price = basePrice * pcs;
            item.name = `${baseName} [CASE: ${pcs}pcs]`;
            item.pcsPerCase = pcs;
            item.cartKey = `${item.code}-CS-${pcs}`;
        } else {
            item.uom = 'pc';
            item.price = basePrice;
            item.name = `${baseName} [PC]`;
            item.cartKey = `${item.code}-PC`;
        }
        renderCartList();
        updateCartCalculations();
    }

    function updateCartItemCS(index, pcsStr) {
        let pcs = parseInt(pcsStr) || 1;
        let item = cart[index];
        let prod = products.find(p => p[0] === item.code);
        let basePrice = prod ? parseFloat(prod[3].replace(/,/g, '')) : (item.price / (item.pcsPerCase || extractPcs(item.name) || 1));
        let baseName = prod ? prod[1] : item.name.replace(/ \[(PC\vert{}CASE).*\]/g, '');

        item.pcsPerCase = pcs;
        item.uom = 'cs';
        item.price = basePrice * pcs;
        item.name = `${baseName} [CASE: ${pcs}pcs]`;
        item.cartKey = `${item.code}-CS-${pcs}`;

        renderCartList();
        updateCartCalculations();
    }

    function browseMoreItems() {
        document.getElementById('cartModal').style.display = 'none';
        switchTab('pricelist');
    }

    function cancelOrder() {
        editingOrderId = null;
        cart = [];
        updateCartFloat();
        document.getElementById('cartModal').style.display = 'none';
    }

    function changeQty(index, delta) {
        cart[index].qty += delta;
        if(cart[index].qty <= 0) cart.splice(index, 1);
        renderCartList();
        updateCartCalculations();
        updateCartFloat();
        if(cart.length === 0) closeModal('cartModal');
    }

    function setQtyDirect(index, val) {
        let newQty = parseInt(val, 10);
        if(isNaN(newQty) || newQty <= 0) newQty = 1;
        cart[index].qty = newQty;
        renderCartList();
        updateCartCalculations();
        updateCartFloat();
    }

    function updateCartCalculations() {
        let subtotal = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
        let discountType = document.getElementById('discountSelect').value;
        let finalTotal = subtotal;
        let discountAmount = 0;

        if(discountType === '3percent') {
            discountAmount = subtotal * 0.03;
            finalTotal = subtotal - discountAmount;
        } else if(discountType === '3plus2') {
            let afterFirstDisc = subtotal * 0.97;
            finalTotal = afterFirstDisc * 0.98;
            discountAmount = subtotal - finalTotal;
        }

        document.getElementById('summarySubtotal').innerText = '₱' + subtotal.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('summaryDiscount').innerText = '-₱' + discountAmount.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('summaryTotal').innerText = '₱' + finalTotal.toLocaleString('en-US', {minimumFractionDigits: 2});
    }

    function submitOrder() {
        let subtotal = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
        let discountType = document.getElementById('discountSelect').value;
        let finalTotal = subtotal;
        let discountAmount = 0;

        if(discountType === '3percent') {
            discountAmount = subtotal * 0.03;
            finalTotal = subtotal - discountAmount;
        } else if(discountType === '3plus2') {
            let afterFirstDisc = subtotal * 0.97;
            finalTotal = afterFirstDisc * 0.98;
            discountAmount = subtotal - finalTotal;
        }

        const now = new Date();
        
        if (editingOrderId) {
            const orderIndex = orders.findIndex(o => o.orderId === editingOrderId);
            if (orderIndex !== -1) {
                orders[orderIndex].items = [...cart];
                orders[orderIndex].subtotal = subtotal;
                orders[orderIndex].discountType = discountType;
                orders[orderIndex].discountAmount = discountAmount;
                orders[orderIndex].totalAmount = finalTotal;
            }
            editingOrderId = null;
            alert('Order successfully updated!');
        } else {
            const newOrder = {
                orderId: 'ORD-' + Date.now(),
                customerId: activeCustomer.id,
                timestamp: now.toISOString(),
                dateFormatted: now.toLocaleString('en-US'),
                items: [...cart],
                subtotal: subtotal,
                discountType: discountType,
                discountAmount: discountAmount,
                totalAmount: finalTotal
            };
            orders.push(newOrder);
            alert('Order successfully finalized and saved!');
        }

        localStorage.setItem('dmi_orders', JSON.stringify(orders));
        cart = [];
        updateCartFloat();
        closeModal('cartModal');
        
        renderDashboard();
    }

    // ORDER HISTORY & PRINT FUNCTION
    function viewCustomerHistory(custId) {
        const cust = customers.find(c => String(c.id) === String(custId));
        if(!cust) return;

        document.getElementById('historyCustStore').innerText = cust.storeName || '-';
        document.getElementById('historyCustName').innerText = cust.name || '-';
        document.getElementById('historyCustContact').innerText = cust.contact || '-';

        const custOrders = orders.filter(o => String(o.customerId) === String(custId));

        let weekTotal = 0, monthTotal = 0, yearTotal = 0;
        let itemAggMap = {};

        custOrders.forEach(o => {
            const orderDate = parseOrderDate(o);
            const total = parseFloat(o.totalAmount || 0);

            if(isThisWeek(orderDate)) weekTotal += total;
            if(isThisMonth(orderDate)) monthTotal += total;
            if(isThisYear(orderDate)) yearTotal += total;

            (o.items || []).forEach(item => {
                const itemCode = item.code || 'UNKNOWN';
                const itemQty = parseInt(item.qty || 0, 10);
                const itemPrice = parseFloat(item.price || 0);

                if(!itemAggMap[itemCode]) itemAggMap[itemCode] = { name: item.name || itemCode, qty: 0, amount: 0 };
                itemAggMap[itemCode].qty += itemQty;
                itemAggMap[itemCode].amount += (itemPrice * itemQty);
            });
        });

        document.getElementById('spentThisWeek').innerText = '₱' + weekTotal.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('spentThisMonth').innerText = '₱' + monthTotal.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('spentThisYear').innerText = '₱' + yearTotal.toLocaleString('en-US', {minimumFractionDigits: 2});

        const itemBody = document.getElementById('historyItemSummaryBody');
        itemBody.innerHTML = '';
        const itemKeys = Object.keys(itemAggMap);

        if(itemKeys.length === 0) {
            itemBody.innerHTML = '<tr><td colspan="4" style="color: #777;">No item history recorded yet.</td></tr>';
        } else {
            itemKeys.forEach(code => {
                const i = itemAggMap[code];
                let tr = document.createElement('tr');
                tr.innerHTML = `
                    <td><strong>${code}</strong></td>
                    <td style="text-align: left;">${i.name}</td>
                    <td>${i.qty}</td>
                    <td style="font-weight: bold; color: #28a745;">₱${i.amount.toLocaleString('en-US', {minimumFractionDigits: 2})}</td>
                `;
                itemBody.appendChild(tr);
            });
        }

        const listDiv = document.getElementById('historyList');
        listDiv.innerHTML = '';

        if(custOrders.length === 0) {
            listDiv.innerHTML = '<p style="text-align: center; color: #777;">No previous orders found for this customer.</p>';
        } else {
            const sortedOrders = [...custOrders].sort((a, b) => parseOrderDate(b) - parseOrderDate(a));
            sortedOrders.forEach(o => {
                let discLabel = 'No Discount (0%)';
                if(o.discountType === '3percent') discLabel = 'Less 3%';
                if(o.discountType === '3plus2') discLabel = 'Less 3% + Less 2%';

                let itemsHtml = (o.items || []).map(i => {
                    const price = parseFloat(i.price || 0);
                    const qty = parseInt(i.qty || 0, 10);
                    const uomStr = i.uom ? i.uom.toUpperCase() : (i.name.includes('[CASE') ? 'CS' : 'PC');
                    return `<div style="font-size: 12px; color: #555;">• <strong>[${i.code}]</strong> ${i.name} (${qty} ${uomStr}) - ₱${(price * qty).toLocaleString('en-US', {minimumFractionDigits: 2})}</div>`;
                }).join('');

                const div = document.createElement('div');
                div.className = 'history-item';
                div.innerHTML = `
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <div style="font-weight: bold; color: #004080; font-size: 13px;">${o.dateFormatted || parseOrderDate(o).toLocaleString('en-US')}</div>
                        <div style="display: flex; gap: 4px;">
                            <button class="btn btn-info" style="font-size: 11px; padding: 4px 8px;" onclick="printOrder('${o.orderId}')">🖨️ Print</button>
                            <button class="btn btn-warning" style="font-size: 11px; padding: 4px 8px;" onclick="loadOrderForEdit('${o.orderId}')">✏ Edit</button>
                            <button class="btn btn-danger" style="font-size: 11px; padding: 4px 8px;" onclick="voidOrder('${o.orderId}', '${custId}')">🗑️️ Void</button>
                        </div>
                    </div>
                    <div style="margin: 5px 0;">${itemsHtml}</div>
                    <div style="font-size: 12px; color: #777;">
                        Subtotal: ₱${parseFloat(o.subtotal || 0).toLocaleString('en-US', {minimumFractionDigits: 2})} | Discount (${discLabel}): -₱${parseFloat(o.discountAmount || 0).toLocaleString('en-US', {minimumFractionDigits: 2})}
                    </div>
                    <div style="font-weight: bold; color: #28a745; font-size: 14px; margin-top: 3px;">
                        Total Paid: ₱${parseFloat(o.totalAmount || 0).toLocaleString('en-US', {minimumFractionDigits: 2})}
                    </div>
                `;
                listDiv.appendChild(div);
            });
        }

        document.getElementById('historyModal').style.display = 'flex';
    }

    // PRINT RECEIPT FUNCTION (IN-PAGE RECEIPT VIEW WITH UOM)
    function printOrder(orderId) {
        const order = orders.find(o => o.orderId === orderId);
        if(!order) return;
        
        const cust = customers.find(c => String(c.id) === String(order.customerId));
        if(!cust) return;

        let itemsHtml = (order.items || []).map(i => {
            const price = parseFloat(i.price || 0);
            const qty = parseInt(i.qty || 0, 10);
            const uomStr = i.uom ? i.uom.toUpperCase() : (i.name.includes('[CASE') ? 'CS' : 'PC');
            const amount = price * qty;
            return `
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; font-weight: bold; text-align: center;">${i.code}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; text-align: left;">${i.name}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; text-align: center; font-weight: bold;">${qty} ${uomStr}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; text-align: right;">₱${price.toFixed(2)}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; text-align: right; font-weight: bold;">₱${amount.toFixed(2)}</td>
                </tr>
            `;
        }).join('');

        let discLabel = 'No Discount (0%)';
        if(order.discountType === '3percent') discLabel = 'Less 3%';
        if(order.discountType === '3plus2') discLabel = 'Less 3% + Less 2%';

        const receiptContainer = document.getElementById('printReceiptContent');
        receiptContainer.innerHTML = `
            <div style="text-align: center; border-bottom: 2px solid #004080; padding-bottom: 10px; margin-bottom: 15px;">
                <h2 style="margin: 0; color: #004080; font-size: 22px;">DYON MARKETING INCORPORATED</h2>
                <p style="margin: 3px 0; font-size: 13px; color: #555;">1149 Teodoro San Luis St. Pandacan Manila | Tel: 8528-1484</p>
                <h3 style="margin: 10px 0 0 0; color: #333; text-transform: uppercase; letter-spacing: 1px;">Order Slip / Receipt</h3>
            </div>
            
            <div style="display: flex; justify-content: space-between; margin-bottom: 15px; font-size: 13px; background: #f8f9fa; padding: 10px; border-radius: 6px; border: 1px solid #e9ecef;">
                <div>
                    <p style="margin: 3px 0;"><strong>Customer Name:</strong> ${cust.name}</p>
                    <p style="margin: 3px 0;"><strong>Store Name:</strong> ${cust.storeName}</p>
                    <p style="margin: 3px 0;"><strong>Address:</strong> ${cust.address}</p>
                    <p style="margin: 3px 0;"><strong>Contact No:</strong> ${cust.contact}</p>
                </div>
                <div style="text-align: right;">
                    <p style="margin: 3px 0;"><strong>Date:</strong> ${order.dateFormatted || parseOrderDate(order).toLocaleString('en-US')}</p>
                </div>
            </div>

            <table style="width: 100%; border-collapse: collapse; margin-bottom: 15px; font-size: 13px;">
                <thead>
                    <tr style="background-color: #004080; color: white;">
                        <th style="padding: 8px; border: 1px solid #004080;">Item Code</th>
                        <th style="padding: 8px; border: 1px solid #004080; text-align: left;">Description</th>
                        <th style="padding: 8px; border: 1px solid #004080; text-align: center;">Qty (UOM)</th>
                        <th style="padding: 8px; border: 1px solid #004080; text-align: right;">Unit Price</th>
                        <th style="padding: 8px; border: 1px solid #004080; text-align: right;">Total</th>
                    </tr>
                </thead>
                <tbody>
                    ${itemsHtml}
                </tbody>
            </table>

            <div style="text-align: right; font-size: 13px; background: #f8f9fa; padding: 10px; border-radius: 6px; border: 1px solid #e9ecef;">
                <p style="margin: 3px 0;">Subtotal: <strong>₱${parseFloat(order.subtotal || 0).toLocaleString('en-US', {minimumFractionDigits: 2})}</strong></p>
                <p style="margin: 3px 0; color: #d9534f;">Discount (${discLabel}): <strong>-₱${parseFloat(order.discountAmount || 0).toLocaleString('en-US', {minimumFractionDigits: 2})}</strong></p>
                <h3 style="margin: 8px 0 0 0; color: #28a745; font-size: 16px;">Grand Total: ₱${parseFloat(order.totalAmount || 0).toLocaleString('en-US', {minimumFractionDigits: 2})}</h3>
            </div>

            <div style="text-align: center; margin-top: 25px; font-size: 12px; color: #777;">
                <p style="margin: 0;">Thank you for your business!</p>
            </div>
        `;

        document.getElementById('printReceiptModal').style.display = 'flex';
    }

    function executePrint() {
        window.print();
    }

    function loadOrderForEdit(orderId) {
        const orderToEdit = orders.find(o => o.orderId === orderId);
        if(!orderToEdit) return;

        editingOrderId = orderId;
        cart = JSON.parse(JSON.stringify(orderToEdit.items));

        cart.forEach(i => {
            if (!i.pcsPerCase) {
                i.pcsPerCase = extractPcs(i.name);
            }
            if (!i.uom) {
                i.uom = i.name.includes('[CASE') ? 'cs' : 'pc';
            }
        });

        selectCustomerForOrder(orderToEdit.customerId);
        
        document.getElementById('discountSelect').value = orderToEdit.discountType || 'none';
        
        closeModal('historyModal');
        updateCartFloat();
        openCartModal();
    }

    function voidOrder(orderId, custId) {
        if(confirm("Are you sure you want to VOID and delete this order?")) {
            orders = orders.filter(o => o.orderId !== orderId);
            localStorage.setItem('dmi_orders', JSON.stringify(orders));
            viewCustomerHistory(custId);
            renderDashboard();
        }
    }

    // DASHBOARD & SEARCH
    function renderDashboard() {
        let totalToday = 0, totalThisMonth = 0, totalMAGO = 0, totalYAGO = 0;

        orders.forEach(o => {
            const orderDate = parseOrderDate(o);
            const total = parseFloat(o.totalAmount || 0);

            if(isToday(orderDate)) totalToday += total;
            if(isThisMonth(orderDate)) totalThisMonth += total;
            if(isMAGO(orderDate)) totalMAGO += total;
            if(isYAGO(orderDate)) totalYAGO += total;
        });

        document.getElementById('metricToday').innerText = '₱' + totalToday.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('metricThisMonth').innerText = '₱' + totalThisMonth.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('metricMAGO').innerText = '₱' + totalMAGO.toLocaleString('en-US', {minimumFractionDigits: 2});
        document.getElementById('metricYAGO').innerText = '₱' + totalYAGO.toLocaleString('en-US', {minimumFractionDigits: 2});

        const tbody = document.getElementById('dashboardCustomerBody');
        tbody.innerHTML = '';

        if(customers.length === 0) {
            tbody.innerHTML = '<tr><td colspan="7" style="color: #777;">No customers found.</td></tr>';
            return;
        }

        customers.forEach(c => {
            const custOrders = orders.filter(o => String(o.customerId) === String(c.id));
            const monthOrders = custOrders.filter(o => isThisMonth(parseOrderDate(o)));
            
            let hasPurchased = monthOrders.length > 0;
            let monthSpent = monthOrders.reduce((sum, o) => sum + parseFloat(o.totalAmount || 0), 0);

            let lastOrderDateStr = 'Never';
            if(custOrders.length > 0) {
                const sorted = [...custOrders].sort((a, b) => parseOrderDate(b) - parseOrderDate(a));
                lastOrderDateStr = sorted[0].dateFormatted || parseOrderDate(sorted[0]).toLocaleString('en-US');
            }

            let tr = document.createElement('tr');
            tr.innerHTML = `
                <td><strong>${c.storeName}</strong></td>
                <td>${c.name}</td>
                <td>${c.contact}</td>
                <td>
                    ${hasPurchased ? '<span class="badge badge-success">Purchased</span>' : '<span class="badge badge-danger">No Purchase</span>'}
                </td>
                <td style="font-weight: bold; color: ${hasPurchased ? '#28a745' : '#777'};">
                    ₱${monthSpent.toLocaleString('en-US', {minimumFractionDigits: 2})}
                </td>
                <td style="font-size: 12px; color: #555;">${lastOrderDateStr}</td>
                <td>
                    <button class="btn btn-info" onclick="viewCustomerHistory('${c.id}')">View Detail</button>
                </td>
            `;
            tbody.appendChild(tr);
        });
    }

    function searchTable() {
        let input = document.getElementById("searchInput");
        let filter = input.value.toUpperCase();
        let table = document.getElementById("priceTable");
        let tr = table.getElementsByTagName("tr");

        for (let i = 1; i < tr.length; i++) {
            let tdCode = tr[i].getElementsByTagName("td")[1];
            let tdDesc = tr[i].getElementsByTagName("td")[2];
            
            if (tdCode || tdDesc) {
                let codeValue = tdCode.textContent || tdCode.innerText;
                let descValue = tdDesc.textContent || tdDesc.innerText;
                
                if (codeValue.toUpperCase().indexOf(filter) > -1 || descValue.toUpperCase().indexOf(filter) > -1) {
                    tr[i].style.display = "";
                } else {
                    tr[i].style.display = "none";
                }
            }
        }
    }

    function closeModal(modalId) {
        document.getElementById(modalId).style.display = 'none';
    }

    function openImageModal(src) {
        document.getElementById('imgFullSize').src = src;
        document.getElementById('imageModal').style.display = 'flex';
    }
</script>
</body>
</html>
