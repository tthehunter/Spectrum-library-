<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#0066cc">
  <title>Spectrum Library Manager</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
  <style>
    /* ===== CORE STYLES ===== */
    :root {
      --primary: #0066cc;
      --primary-dark: #0055b3;
      --success: #28a745;
      --danger: #dc3545;
      --warning: #ffc107;
      --info: #17a2b8;
      --light: #f8f9fa;
      --dark: #343a40;
      --gray: #6c757d;
      --border: #dee2e6;
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
      line-height: 1.6;
      color: #333;
      background-color: #f5f7fa;
      padding: 0;
      margin: 0;
      min-height: 100vh;
    }
    
    .container {
      width: 100%;
      max-width: 800px;
      margin: 0 auto;
      padding: 15px;
    }
    
    /* ===== HEADER ===== */
    header {
      background: var(--primary);
      color: white;
      padding: 15px 0;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      position: sticky;
      top: 0;
      z-index: 100;
    }
    
    .header-content {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 15px;
    }
    
    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .logo-icon {
      font-size: 1.5rem;
    }
    
    .logo-text {
      font-size: 1.2rem;
      font-weight: 600;
    }
    
    .menu-toggle {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
      display: none;
    }
    
    /* ===== TABS ===== */
    .tabs {
      display: flex;
      margin: 15px 0;
      border-bottom: 1px solid var(--border);
      overflow-x: auto;
    }
    
    .tab {
      padding: 12px 20px;
      cursor: pointer;
      background: #e9ecef;
      border: 1px solid var(--border);
      border-bottom: none;
      border-radius: 5px 5px 0 0;
      margin-right: 5px;
      white-space: nowrap;
      font-weight: 500;
      transition: all 0.2s;
    }
    
    .tab.active {
      background: white;
      border-bottom: 2px solid white;
      font-weight: 600;
      color: var(--primary);
      box-shadow: 0 -2px 5px rgba(0,0,0,0.05);
    }
    
    .tab:hover:not(.active) {
      background: #f1f3f5;
    }
    
    .tab-content {
      display: none;
      animation: fadeIn 0.3s;
      background: white;
      border-radius: 0 0 5px 5px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.05);
      padding: 20px;
      margin-bottom: 20px;
    }
    
    .tab-content.active {
      display: block;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    
    /* ===== FORM STYLES ===== */
    .form-section {
      margin-bottom: 25px;
    }
    
    .section-title {
      font-size: 1.1rem;
      font-weight: 600;
      color: var(--primary);
      margin-bottom: 15px;
      padding-bottom: 8px;
      border-bottom: 1px solid var(--border);
    }
    
    .form-row {
      display: flex;
      flex-wrap: wrap;
      margin: 0 -10px;
    }
    
    .form-group {
      flex: 1 0 220px;
      padding: 0 10px;
      margin-bottom: 15px;
    }
    
    label {
      display: block;
      margin-bottom: 6px;
      font-weight: 500;
      color: var(--dark);
    }
    
    input, select, textarea {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid var(--border);
      border-radius: 4px;
      font-size: 16px;
      transition: border-color 0.2s;
    }
    
    input:focus, select:focus, textarea:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 2px rgba(0, 102, 204, 0.2);
    }
    
    .checkbox-group {
      display: flex;
      align-items: center;
      gap: 8px;
      margin: 10px 0;
    }
    
    .checkbox-label {
      cursor: pointer;
      user-select: none;
    }
    
    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 10px 16px;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      font-weight: 500;
      cursor: pointer;
      transition: all 0.2s;
      text-align: center;
    }
    
    .btn-primary {
      background: var(--primary);
      color: white;
    }
    
    .btn-primary:hover {
      background: var(--primary-dark);
      transform: translateY(-1px);
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    
    .btn-success {
      background: var(--success);
      color: white;
    }
    
    .btn-danger {
      background: var(--danger);
      color: white;
    }
    
    .btn-block {
      width: 100%;
    }
    
    .btn-icon {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 24px;
      height: 24px;
    }
    
    /* ===== RECEIPT STYLES ===== */
    .receipt-container {
      max-width: 800px;
      margin: 0 auto;
      background: white;
      padding: 30px;
      border: 1px solid #e0e0e0;
      box-shadow: 0 2px 10px rgba(0,0,0,0.05);
      position: relative;
    }
    
    .receipt-header {
      text-align: center;
      margin-bottom: 25px;
      padding-bottom: 15px;
      border-bottom: 1px solid #eee;
    }
    
    .receipt-title {
      font-size: 24px;
      font-weight: 700;
      color: var(--primary);
      margin: 10px 0;
    }
    
    .receipt-subtitle {
      color: var(--gray);
      font-size: 16px;
    }
    
    .receipt-id {
      text-align: right;
      font-weight: 600;
      color: var(--dark);
      margin-bottom: 20px;
    }
    
    .receipt-details {
      margin-bottom: 25px;
    }
    
    .detail-row {
      display: flex;
      padding: 8px 0;
      border-bottom: 1px solid #f0f0f0;
    }
    
    .detail-label {
      flex: 0 0 180px;
      font-weight: 500;
      color: var(--dark);
    }
    
    .detail-value {
      flex: 1;
    }
    
    .fee-table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
    }
    
    .fee-table th {
      background: var(--primary);
      color: white;
      text-align: left;
      padding: 10px 15px;
    }
    
    .fee-table td {
      padding: 10px 15px;
      border-bottom: 1px solid #f0f0f0;
    }
    
    .fee-table tr:nth-child(even) {
      background: #f8f9fa;
    }
    
    .total-row {
      font-weight: 700;
      background: #e9f7fe !important;
    }
    
    .receipt-footer {
      margin-top: 30px;
      padding-top: 20px;
      border-top: 1px solid #eee;
      text-align: center;
      color: var(--gray);
      font-size: 14px;
    }
    
    .verification-note {
      margin-top: 15px;
      padding: 10px;
      background: #e9f7fe;
      border-radius: 4px;
      font-size: 14px;
    }
    
    /* ===== VERIFICATION STYLES ===== */
    .verification-box {
      border: 1px solid var(--border);
      border-radius: 5px;
      padding: 20px;
      background: white;
    }
    
    .verification-input {
      display: flex;
      gap: 10px;
      margin-bottom: 15px;
    }
    
    .verification-result {
      margin-top: 20px;
      padding: 15px;
      border-radius: 4px;
    }
    
    .valid-receipt {
      background: #d4edda;
      color: #155724;
      border: 1px solid #c3e6cb;
    }
    
    .invalid-receipt {
      background: #f8d7da;
      color: #721c24;
      border: 1px solid #f5c6cb;
    }
    
    /* ===== DATA MANAGEMENT STYLES ===== */
    .data-controls {
      display: flex;
      gap: 10px;
      margin-bottom: 15px;
      flex-wrap: wrap;
    }
    
    .search-box {
      flex: 1;
      min-width: 200px;
    }
    
    .records-summary {
      background: #e9f7fe;
      padding: 10px 15px;
      border-radius: 4px;
      margin-bottom: 15px;
      font-size: 14px;
    }
    
    .records-table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }
    
    .records-table th {
      background: var(--primary);
      color: white;
      text-align: left;
      padding: 10px 15px;
    }
    
    .records-table td {
      padding: 10px 15px;
      border-bottom: 1px solid var(--border);
    }
    
    .records-table tr:nth-child(even) {
      background: #f8f9fa;
    }
    
    .action-buttons {
      display: flex;
      gap: 8px;
      min-width: 120px;
    }
    
    .pagination {
      display: flex;
      justify-content: center;
      gap: 5px;
      margin-top: 20px;
    }
    
    .page-item {
      padding: 5px 10px;
      border: 1px solid var(--border);
      border-radius: 3px;
      cursor: pointer;
    }
    
    .page-item.active {
      background: var(--primary);
      color: white;
      border-color: var(--primary);
    }
    
    .page-item:hover:not(.active) {
      background: #f1f3f5;
    }
    
    /* ===== SETTINGS STYLES ===== */
    .settings-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 20px;
    }
    
    .setting-card {
      border: 1px solid var(--border);
      border-radius: 5px;
      padding: 20px;
      background: white;
      box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }
    
    .setting-title {
      font-size: 1.1rem;
      font-weight: 600;
      margin-bottom: 15px;
      color: var(--primary);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    
    .fee-structure-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 15px;
      margin-top: 15px;
    }
    
    .fee-item {
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 15px;
      background: #f8f9fa;
    }
    
    .fee-title {
      font-weight: 600;
      margin-bottom: 10px;
      color: var(--dark);
    }
    
    .fee-options {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    
    .fee-option {
      display: flex;
      justify-content: space-between;
      padding: 5px 0;
    }
    
    /* ===== NOTIFICATION STYLES ===== */
    .notification {
      position: fixed;
      top: 20px;
      right: 20px;
      padding: 15px 20px;
      border-radius: 5px;
      color: white;
      font-weight: 500;
      box-shadow: 0 3px 10px rgba(0,0,0,0.2);
      z-index: 1000;
      transform: translateX(120%);
      transition: transform 0.3s ease-out;
      max-width: 300px;
    }
    
    .notification.show {
      transform: translateX(0);
    }
    
    .notification.success {
      background: var(--success);
    }
    
    .notification.error {
      background: var(--danger);
    }
    
    .notification.warning {
      background: var(--warning);
    }
    
    .notification.info {
      background: var(--info);
    }
    
    /* ===== HELP MODAL ===== */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.5);
      z-index: 1000;
      align-items: center;
      justify-content: center;
    }
    
    .modal.show {
      display: flex;
    }
    
    .modal-content {
      background: white;
      width: 90%;
      max-width: 600px;
      border-radius: 8px;
      overflow: hidden;
      max-height: 90vh;
      display: flex;
      flex-direction: column;
    }
    
    .modal-header {
      padding: 15px 20px;
      background: var(--primary);
      color: white;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .modal-title {
      font-size: 1.2rem;
      font-weight: 600;
    }
    
    .modal-close {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
      width: 30px;
      height: 30px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 50%;
    }
    
    .modal-body {
      padding: 20px;
      overflow-y: auto;
      flex: 1;
    }
    
    .modal-footer {
      padding: 15px 20px;
      border-top: 1px solid var(--border);
      display: flex;
      justify-content: flex-end;
    }
    
    /* ===== RESPONSIVE ===== */
    @media (max-width: 768px) {
      .form-row {
        flex-direction: column;
      }
      
      .form-group {
        flex: 1 0 100%;
      }
      
      .detail-label {
        flex: 0 0 140px;
      }
      
      .tabs {
        overflow-x: auto;
      }
      
      .tab {
        padding: 10px 15px;
        font-size: 0.9rem;
      }
      
      .receipt-container {
        padding: 20px 15px;
      }
      
      .menu-toggle {
        display: block;
      }
      
      .mobile-menu {
        display: none;
        position: absolute;
        top: 100%;
        right: 15px;
        background: white;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        border-radius: 5px;
        z-index: 100;
      }
      
      .mobile-menu.show {
        display: block;
      }
      
      .mobile-menu-item {
        padding: 12px 20px;
        display: block;
        color: var(--dark);
        text-decoration: none;
      }
      
      .mobile-menu-item:hover {
        background: #f1f3f5;
      }
    }
  </style>
</head>
<body>
  <header>
    <div class="header-content">
      <div class="logo">
        <span class="logo-icon">📚</span>
        <span class="logo-text">Spectrum Library Manager</span>
      </div>
      <button class="menu-toggle" id="menuToggle">☰</button>
      <div class="mobile-menu" id="mobileMenu">
        <a href="#" class="mobile-menu-item" onclick="showTab('generateTab'); return false;">Generate Receipt</a>
        <a href="#" class="mobile-menu-item" onclick="showTab('verifyTab'); return false;">Verify Receipt</a>
        <a href="#" class="mobile-menu-item" onclick="showTab('dataTab'); return false;">Manage Data</a>
        <a href="#" class="mobile-menu-item" onclick="showTab('settingsTab'); return false;">Settings</a>
        <a href="#" class="mobile-menu-item" onclick="openHelpModal(); return false;">Help</a>
      </div>
    </div>
  </header>

  <div class="container">
    <!-- TAB NAVIGATION -->
    <div class="tabs">
      <div class="tab active" data-tab="generateTab" onclick="showTab('generateTab')">Generate Receipt</div>
      <div class="tab" data-tab="verifyTab" onclick="showTab('verifyTab')">Verify Receipt</div>
      <div class="tab" data-tab="dataTab" onclick="showTab('dataTab')">Manage Data</div>
      <div class="tab" data-tab="settingsTab" onclick="showTab('settingsTab')">Settings</div>
    </div>
    
    <!-- GENERATE TAB -->
    <div id="generateTab" class="tab-content active">
      <div class="form-section">
        <h2 class="section-title">Student Information</h2>
        <div class="form-row">
          <div class="form-group">
            <label for="name">Student Name <span style="color: var(--danger);">*</span></label>
            <input type="text" id="name" placeholder="Enter full name" required>
          </div>
          <div class="form-group">
            <label for="father">Father's Name <span style="color: var(--danger);">*</span></label>
            <input type="text" id="father" placeholder="Enter father's name" required>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label for="aadhaar">Aadhaar Number <span style="color: var(--danger);">*</span></label>
            <input type="text" id="aadhaar" placeholder="Enter 12-digit Aadhaar" maxlength="12" required>
          </div>
          <div class="form-group">
            <label for="contact">Contact Number</label>
            <input type="tel" id="contact" placeholder="Enter phone number" maxlength="10">
          </div>
        </div>
      </div>
      
      <div class="form-section">
        <h2 class="section-title">Fee Structure</h2>
        <div class="form-row">
          <div class="form-group">
            <label for="plan">Select Plan <span style="color: var(--danger);">*</span></label>
            <select id="plan" required>
              <option value="">-- Select Plan --</option>
              <option value="morning">Morning (6am to 2pm) - ₹500</option>
              <option value="evening">Evening (2pm to 10pm) - ₹700</option>
              <option value="full">Full Day (12hrs) - ₹900</option>
              <option value="24hrs">24 Hours - ₹1000</option>
              <option value="night">Night Shift - ₹350</option>
            </select>
          </div>
          <div class="form-group">
            <label for="months">Months Paid <span style="color: var(--danger);">*</span></label>
            <input type="number" id="months" placeholder="1-12" min="1" max="12" value="1" required>
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <div class="checkbox-group">
              <input type="checkbox" id="locker">
              <label for="locker" class="checkbox-label">Include Locker (+₹100)</label>
            </div>
          </div>
          <div class="form-group">
            <div class="checkbox-group">
              <input type="checkbox" id="seat">
              <label for="seat" class="checkbox-label">Seat Reservation (+₹50)</label>
            </div>
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label>Calculated Fee</label>
            <div style="background: #e9f7fe; padding: 12px; border-radius: 4px; margin-top: 5px;">
              <div style="display: flex; justify-content: space-between; font-weight: 500;">
                <span>Base Fee:</span>
                <span id="baseFeeDisplay">₹0</span>
              </div>
              <div style="display: flex; justify-content: space-between; margin-top: 5px;">
                <span>Locker (+₹100):</span>
                <span id="lockerFeeDisplay">₹0</span>
              </div>
              <div style="display: flex; justify-content: space-between; margin-top: 5px;">
                <span>Seat (+₹50):</span>
                <span id="seatFeeDisplay">₹0</span>
              </div>
              <div style="display: flex; justify-content: space-between; margin-top: 10px; font-weight: 700; border-top: 1px solid #ddd; padding-top: 8px;">
                <span>Total Fee:</span>
                <span id="totalFeeDisplay">₹0</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <div class="form-section">
        <button class="btn btn-primary btn-block" onclick="generateReceipt()">
          <span class="btn-icon">✓</span> Generate Receipt
        </button>
      </div>
      
      <div id="receiptOutput" class="receipt-container" style="display: none;">
        <div class="receipt-header">
          <h1 class="receipt-title">SPECTRUM LIBRARY</h1>
          <p class="receipt-subtitle">Official Fee Receipt</p>
        </div>
        
        <div class="receipt-id">
          <strong>Receipt ID:</strong> <span id="receiptId">SPL-2024-001</span>
        </div>
        
        <div class="receipt-details">
          <div class="detail-row">
            <div class="detail-label">Date & Time:</div>
            <div class="detail-value" id="receiptDateTime">14/08/2024 15:30</div>
          </div>
          <div class="detail-row">
            <div class="detail-label">Student Name:</div>
            <div class="detail-value" id="studentName">John Doe</div>
          </div>
          <div class="detail-row">
            <div class="detail-label">Father's Name:</div>
            <div class="detail-value" id="fatherName">Jane Doe</div>
          </div>
          <div class="detail-row">
            <div class="detail-label">Aadhaar Number:</div>
            <div class="detail-value" id="aadhaarNo">XXXX-XXXX-1234</div>
          </div>
          <div class="detail-row">
            <div class="detail-label">Contact Number:</div>
            <div class="detail-value" id="contactNo">XXXXXX5678</div>
          </div>
        </div>
        
        <table class="fee-table">
          <thead>
            <tr>
              <th>Description</th>
              <th>Amount (₹)</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td id="planDescription">Morning (6am to 2pm)</td>
              <td id="baseFeeValue">500</td>
            </tr>
            <tr id="lockerRow" style="display: none;">
              <td>Locker Facility</td>
              <td>100</td>
            </tr>
            <tr id="seatRow" style="display: none;">
              <td>Seat Reservation</td>
              <td>50</td>
            </tr>
            <tr class="total-row">
              <td>Monthly Total</td>
              <td id="monthlyTotal">650</td>
            </tr>
            <tr class="total-row">
              <td><strong>Total for <span id="monthsPaid">1</span> Month(s)</strong></td>
              <td><strong id="finalTotal">650</strong></td>
            </tr>
          </tbody>
        </table>
        
        <div class="receipt-footer">
          <p>Thank you for being a valued member of Spectrum Library</p>
          <p>This is a computer-generated receipt and does not require a signature</p>
          <div class="verification-note">
            ✅ Valid receipt. Verify at: spectrumlibrary.in/verify?id=<span id="verifyId">SPL-2024-001</span>
          </div>
        </div>
      </div>
      
      <div id="receiptActions" style="display: none; margin-top: 20px; text-align: center;">
        <div class="data-controls" style="justify-content: center;">
          <button class="btn btn-primary" onclick="downloadPDF()">
            <span class="btn-icon">↓</span> Download PDF
          </button>
          <button class="btn btn-success" onclick="printReceipt()">
            <span class="btn-icon">🖨️</span> Print Receipt
          </button>
          <button class="btn btn-info" onclick="resetForm()">
            <span class="btn-icon">↺</span> New Receipt
          </button>
        </div>
      </div>
    </div>
    
    <!-- VERIFY TAB -->
    <div id="verifyTab" class="tab-content">
      <div class="verification-box">
        <h2 class="section-title">Receipt Verification</h2>
        <p>Enter a receipt ID to check its validity and view details</p>
        
        <div class="verification-input">
          <input type="text" id="verifyInput" placeholder="Enter Receipt ID (e.g., SPL-2024-123)" maxlength="15">
          <button class="btn btn-primary" onclick="verifyReceipt()">Verify</button>
        </div>
        
        <div id="verificationResult"></div>
      </div>
    </div>
    
    <!-- DATA MANAGEMENT TAB -->
    <div id="dataTab" class="tab-content">
      <div class="form-section">
        <h2 class="section-title">Receipt Records</h2>
        
        <div class="data-controls">
          <div class="search-box">
            <input type="text" id="searchInput" placeholder="Search by name, ID, or Aadhaar..." oninput="filterRecords()">
          </div>
          <button class="btn btn-success" onclick="exportToCSV()">
            <span class="btn-icon">↓</span> Export CSV
          </button>
          <button class="btn btn-info" onclick="backupData()">
            <span class="btn-icon">💾</span> Backup Data
          </button>
          <button class="btn btn-warning" onclick="restoreData()">
            <span class="btn-icon">🔄</span> Restore Data
          </button>
        </div>
        
        <div class="records-summary" id="recordsSummary">
          Showing <strong>0</strong> of <strong>0</strong> records
        </div>
        
        <table class="records-table">
          <thead>
            <tr>
              <th>Date</th>
              <th>Receipt ID</th>
              <th>Student</th>
              <th>Aadhaar</th>
              <th>Plan</th>
              <th>Amount</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody id="recordsTableBody">
            <!-- Records will be populated here -->
          </tbody>
        </table>
        
        <div class="pagination" id="paginationControls">
          <!-- Pagination will be populated here -->
        </div>
      </div>
    </div>
    
    <!-- SETTINGS TAB -->
    <div id="settingsTab" class="tab-content">
      <div class="form-section">
        <h2 class="section-title">Library Settings</h2>
        
        <div class="settings-grid">
          <div class="setting-card">
            <h3 class="setting-title">📚 Library Information</h3>
            <div class="form-group">
              <label for="libraryName">Library Name</label>
              <input type="text" id="libraryName" value="Spectrum Library">
            </div>
            <div class="form-group">
              <label for="libraryAddress">Address</label>
              <textarea id="libraryAddress" rows="3">123 Main Street, Library Building, City, State - 123456</textarea>
            </div>
            <div class="form-group">
              <label for="libraryContact">Contact Information</label>
              <input type="text" id="libraryContact" value="+91 98765 43210">
            </div>
            <button class="btn btn-primary" onclick="saveLibrarySettings()">Save Settings</button>
          </div>
          
          <div class="setting-card">
            <h3 class="setting-title">💰 Fee Structure</h3>
            <p>Adjust fee amounts as needed:</p>
            
            <div class="fee-structure-grid">
              <div class="fee-item">
                <div class="fee-title">Morning (6am-2pm)</div>
                <div class="fee-options">
                  <div class="fee-option">
                    <span>Base Fee:</span>
                    <input type="number" id="morningBase" value="500" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>With Locker:</span>
                    <input type="number" id="morningLocker" value="600" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>Seat Reservation:</span>
                    <input type="number" id="morningSeat" value="550" min="0" style="width: 70px; text-align: right;">
                  </div>
                </div>
              </div>
              
              <div class="fee-item">
                <div class="fee-title">Evening (2pm-10pm)</div>
                <div class="fee-options">
                  <div class="fee-option">
                    <span>Base Fee:</span>
                    <input type="number" id="eveningBase" value="700" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>With Locker:</span>
                    <input type="number" id="eveningLocker" value="800" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>Seat Reservation:</span>
                    <input type="number" id="eveningSeat" value="750" min="0" style="width: 70px; text-align: right;">
                  </div>
                </div>
              </div>
              
              <div class="fee-item">
                <div class="fee-title">Full Day (12hrs)</div>
                <div class="fee-options">
                  <div class="fee-option">
                    <span>Base Fee:</span>
                    <input type="number" id="fullBase" value="900" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>With Locker:</span>
                    <input type="number" id="fullLocker" value="1000" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>Seat Reservation:</span>
                    <input type="number" id="fullSeat" value="950" min="0" style="width: 70px; text-align: right;">
                  </div>
                </div>
              </div>
              
              <div class="fee-item">
                <div class="fee-title">24 Hours</div>
                <div class="fee-options">
                  <div class="fee-option">
                    <span>Base Fee:</span>
                    <input type="number" id="allDayBase" value="1000" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>With Locker:</span>
                    <input type="number" id="allDayLocker" value="1100" min="0" style="width: 70px; text-align: right;">
                  </div>
                  <div class="fee-option">
                    <span>Seat Reservation:</span>
                    <input type="number" id="allDaySeat" value="1050" min="0" style="width: 70px; text-align: right;">
                  </div>
                </div>
              </div>
              
              <div class="fee-item">
                <div class="fee-title">Night Shift</div>
                <div class="fee-options">
                  <div class="fee-option">
                    <span>Base Fee:</span>
                    <input type="number" id="nightBase" value="350" min="0" style="width: 70px; text-align: right;">
                  </div>
                </div>
              </div>
            </div>
            
            <button class="btn btn-primary" onclick="saveFeeStructure()" style="margin-top: 15px;">
              Save Fee Structure
            </button>
          </div>
          
          <div class="setting-card">
            <h3 class="setting-title">⚙️ Application Settings</h3>
            
            <div class="form-group">
              <label for="languageSelect">Language</label>
              <select id="languageSelect">
                <option value="en">English</option>
                <option value="hi">हिंदी (Hindi)</option>
              </select>
            </div>
            
            <div class="form-group">
              <label>
                <input type="checkbox" id="autoPrint" checked>
                Auto-print after receipt generation
              </label>
            </div>
            
            <div class="form-group">
              <label>
                <input type="checkbox" id="requireAadhaar" checked>
                Require Aadhaar number
              </label>
            </div>
            
            <div class="form-group">
              <label>
                <input type="checkbox" id="enableSeatReservation" checked>
                Enable Seat Reservation option
              </label>
            </div>
            
            <div class="form-group">
              <label>
                <input type="checkbox" id="enableLocker" checked>
                Enable Locker option
              </label>
            </div>
            
            <button class="btn btn-primary" onclick="saveAppSettings()">
              Save Application Settings
            </button>
          </div>
          
          <div class="setting-card">
            <h3 class="setting-title">📊 Audit Trail</h3>
            <p>Track all actions performed in the system:</p>
            
            <div id="auditTrail" style="max-height: 300px; overflow-y: auto; border: 1px solid var(--border); border-radius: 4px; padding: 10px; background: #f8f9fa;">
              <!-- Audit entries will appear here -->
            </div>
            
            <button class="btn btn-danger" onclick="clearAuditTrail()" style="margin-top: 10px;">
              Clear Audit Trail
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  <!-- NOTIFICATION SYSTEM -->
  <div class="notification" id="notification"></div>
  
  <!-- HELP MODAL -->
  <div class="modal" id="helpModal">
    <div class="modal-content">
      <div class="modal-header">
        <h3 class="modal-title">Library Manager Help</h3>
        <button class="modal-close" onclick="closeHelpModal()">&times;</button>
      </div>
      <div class="modal-body">
        <h4>Generating a Receipt</h4>
        <p>1. Fill in the student information (name, father's name, Aadhaar)</p>
        <p>2. Select a plan and enter the number of months</p>
        <p>3. Add optional services (locker, seat reservation)</p>
        <p>4. Click "Generate Receipt" to create the receipt</p>
        <p>5. Use the action buttons to download, print, or create a new receipt</p>
        
        <h4 style="margin-top: 20px;">Verifying a Receipt</h4>
        <p>1. Go to the "Verify Receipt" tab</p>
        <p>2. Enter the receipt ID (e.g., SPL-2024-123)</p>
        <p>3. Click "Verify" to see the receipt details</p>
        
        <h4 style="margin-top: 20px;">Managing Data</h4>
        <p>1. View all receipts in the "Manage Data" tab</p>
        <p>2. Search for specific receipts using the search box</p>
        <p>3. Export data to CSV for backup or reporting</p>
        <p>4. Backup and restore your entire database</p>
        
        <h4 style="margin-top: 20px;">Customizing Settings</h4>
        <p>1. Update your library information in the "Library Information" section</p>
        <p>2. Adjust fee amounts as needed in the "Fee Structure" section</p>
        <p>3. Configure application behavior in "Application Settings"</p>
        <p>4. View the audit trail to track all system actions</p>
      </div>
      <div class="modal-footer">
        <button class="btn btn-primary" onclick="closeHelpModal()">Close</button>
      </div>
    </div>
  </div>

  <script>
    // ===== CORE SYSTEM =====
    const APP_VERSION = '1.0.0';
    const DB_NAME = 'spectrumLibraryDB';
    const DEFAULT_FEE_STRUCTURE = {
      morning: { base: 500, locker: 600, seat: 550 },
      evening: { base: 700, locker: 800, seat: 750 },
      full: { base: 900, locker: 1000, seat: 950 },
      '24hrs': { base: 1000, locker: 1100, seat: 1050 },
      night: { base: 350 }
    };
    
    // Initialize the app
    document.addEventListener('DOMContentLoaded', function() {
      initApp();
      setupEventListeners();
      renderInitialData();
    });
    
    function initApp() {
      // Initialize database if not exists
      if (!localStorage.getItem(DB_NAME)) {
        const initialData = {
          receipts: [],
          settings: {
            library: {
              name: 'Spectrum Library',
              address: '123 Main Street, Library Building, City, State - 123456',
              contact: '+91 98765 43210'
            },
            feeStructure: DEFAULT_FEE_STRUCTURE,
            app: {
              language: 'en',
              autoPrint: true,
              requireAadhaar: true,
              enableSeatReservation: true,
              enableLocker: true
            }
          },
          auditTrail: []
        };
        localStorage.setItem(DB_NAME, JSON.stringify(initialData));
        addAuditEntry('System initialized with default settings');
      }
      
      // Load settings
      loadSettings();
    }
    
    function setupEventListeners() {
      // Menu toggle for mobile
      document.getElementById('menuToggle').addEventListener('click', function() {
        document.getElementById('mobileMenu').classList.toggle('show');
      });
      
      // Close mobile menu when clicking outside
      document.addEventListener('click', function(event) {
        const menuToggle = document.getElementById('menuToggle');
        const mobileMenu = document.getElementById('mobileMenu');
        
        if (!menuToggle.contains(event.target) && !mobileMenu.contains(event.target)) {
          mobileMenu.classList.remove('show');
        }
      });
      
      // Plan selection changes
      document.getElementById('plan').addEventListener('change', updateFeeCalculation);
      
      // Checkbox changes
      document.getElementById('locker').addEventListener('change', updateFeeCalculation);
      document.getElementById('seat').addEventListener('change', updateFeeCalculation);
      
      // Months input change
      document.getElementById('months').addEventListener('input', updateFeeCalculation);
      
      // Language change
      document.getElementById('languageSelect').addEventListener('change', function() {
        const settings = getSettings();
        settings.app.language = this.value;
        saveSettings(settings);
        showNotification('Language updated successfully', 'success');
      });
      
      // Initialize fee calculation
      updateFeeCalculation();
    }
    
    function renderInitialData() {
      // Load settings into UI
      loadSettingsIntoUI();
      
      // Render data tab
      renderDataTab();
    }
    
    // ===== DATABASE MANAGEMENT =====
    function getDatabase() {
      return JSON.parse(localStorage.getItem(DB_NAME)) || {
        receipts: [],
        settings: {
          library: {
            name: 'Spectrum Library',
            address: '123 Main Street, Library Building, City, State - 123456',
            contact: '+91 98765 43210'
          },
          feeStructure: DEFAULT_FEE_STRUCTURE,
          app: {
            language: 'en',
            autoPrint: true,
            requireAadhaar: true,
            enableSeatReservation: true,
            enableLocker: true
          }
        },
        auditTrail: []
      };
    }
    
    function saveDatabase(db) {
      localStorage.setItem(DB_NAME, JSON.stringify(db));
    }
    
    function getSettings() {
      const db = getDatabase();
      return db.settings;
    }
    
    function saveSettings(settings) {
      const db = getDatabase();
      db.settings = settings;
      saveDatabase(db);
    }
    
    function addAuditEntry(action) {
      const db = getDatabase();
      const timestamp = new Date().toLocaleString('en-IN');
      db.auditTrail.unshift({
        timestamp: timestamp,
        action: action
      });
      
      // Keep only the last 100 entries
      if (db.auditTrail.length > 100) {
        db.auditTrail = db.auditTrail.slice(0, 100);
      }
      
      saveDatabase(db);
      renderAuditTrail();
    }
    
    // ===== TAB MANAGEMENT =====
    function showTab(tabId) {
      // Hide all tabs
      document.querySelectorAll('.tab-content').forEach(tab => {
        tab.classList.remove('active');
      });
      
      // Deactivate all tabs
      document.querySelectorAll('.tab').forEach(tab => {
        tab.classList.remove('active');
      });
      
      // Show selected tab
      document.getElementById(tabId).classList.add('active');
      
      // Activate tab button that matches the tabId
      const tabButtons = document.querySelectorAll('.tab');
      tabButtons.forEach(tabButton => {
        if (tabButton.getAttribute('data-tab') === tabId) {
          tabButton.classList.add('active');
        }
      });
      
      // Close mobile menu if open
      document.getElementById('mobileMenu').classList.remove('show');
      
      // Special handling for data tab
      if (tabId === 'dataTab') {
        renderDataTab();
      }
    }
    
    // ===== RECEIPT GENERATION =====
    function updateFeeCalculation() {
      const plan = document.getElementById('plan').value;
      const locker = document.getElementById('locker').checked;
      const seat = document.getElementById('seat').checked;
      const months = parseInt(document.getElementById('months').value) || 1;
      
      if (!plan) {
        document.getElementById('baseFeeDisplay').textContent = '₹0';
        document.getElementById('lockerFeeDisplay').textContent = '₹0';
        document.getElementById('seatFeeDisplay').textContent = '₹0';
        document.getElementById('totalFeeDisplay').textContent = '₹0';
        return;
      }
      
      const settings = getSettings();
      const feeStructure = settings.feeStructure[plan];
      
      let baseFee = 0;
      let lockerFee = 0;
      let seatFee = 0;
      
      if (feeStructure) {
        baseFee = feeStructure.base;
        
        if (locker) {
          lockerFee = feeStructure.locker ? (feeStructure.locker - feeStructure.base) : 100;
        }
        
        if (seat) {
          seatFee = feeStructure.seat ? (feeStructure.seat - feeStructure.base) : 50;
        }
      }
      
      const total = (baseFee + lockerFee + seatFee) * months;
      
      // Update display
      document.getElementById('baseFeeDisplay').textContent = `₹${baseFee}`;
      document.getElementById('lockerFeeDisplay').textContent = locker ? `₹${lockerFee}` : '₹0';
      document.getElementById('seatFeeDisplay').textContent = seat ? `₹${seatFee}` : '₹0';
      document.getElementById('totalFeeDisplay').textContent = `₹${total}`;
      
      // Update receipt preview if visible
      if (document.getElementById('receiptOutput').style.display !== 'none') {
        document.getElementById('baseFeeValue').textContent = baseFee;
        document.getElementById('monthlyTotal').textContent = baseFee + lockerFee + seatFee;
        document.getElementById('finalTotal').textContent = total;
        document.getElementById('monthsPaid').textContent = months;
        
        // Update locker and seat rows visibility
        document.getElementById('lockerRow').style.display = locker ? 'table-row' : 'none';
        document.getElementById('seatRow').style.display = seat ? 'table-row' : 'none';
      }
    }
    
    function generateReceipt() {
      const name = document.getElementById('name').value.trim();
      const father = document.getElementById('father').value.trim();
      const aadhaar = document.getElementById('aadhaar').value.trim();
      const contact = document.getElementById('contact').value.trim();
      const plan = document.getElementById('plan').value;
      const locker = document.getElementById('locker').checked;
      const seat = document.getElementById('seat').checked;
      const months = parseInt(document.getElementById('months').value) || 1;
      
      // Validation
      if (!name) {
        showNotification('Please enter student name', 'error');
        return;
      }
      
      if (!father) {
        showNotification('Please enter father\'s name', 'error');
        return;
      }
      
      const settings = getSettings();
      if (settings.app.requireAadhaar && (!aadhaar || aadhaar.length !== 12)) {
        showNotification('Please enter a valid 12-digit Aadhaar number', 'error');
        return;
      }
      
      if (!plan) {
        showNotification('Please select a plan', 'error');
        return;
      }
      
      if (months < 1 || months > 12) {
        showNotification('Please enter valid months (1-12)', 'error');
        return;
      }
      
      // Get fee structure
      const feeStructure = settings.feeStructure[plan];
      
      if (!feeStructure) {
        showNotification('Invalid plan selected', 'error');
        return;
      }
      
      // Calculate fees
      let baseFee = feeStructure.base;
      let lockerFee = 0;
      let seatFee = 0;
      
      if (locker && feeStructure.locker) {
        lockerFee = feeStructure.locker - feeStructure.base;
      } else if (locker) {
        lockerFee = 100; // Default locker fee
      }
      
      if (seat && feeStructure.seat) {
        seatFee = feeStructure.seat - feeStructure.base;
      } else if (seat) {
        seatFee = 50; // Default seat fee
      }
      
      const monthlyTotal = baseFee + lockerFee + seatFee;
      const total = monthlyTotal * months;
      
      // Generate unique ID
      const date = new Date();
      const receiptId = `SPL-${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}${String(date.getDate()).padStart(2, '0')}-${String(Math.floor(100 + Math.random() * 900))}`;
      
      // Create receipt data
      const receiptData = {
        id: receiptId,
        date: date.toLocaleDateString('en-IN'),
        time: date.toLocaleTimeString('en-IN', { hour: '2-digit', minute: '2-digit' }),
        name: name,
        father: father,
        aadhaar: aadhaar,
        contact: contact,
        plan: plan,
        locker: locker,
        seat: seat,
        months: months,
        baseFee: baseFee,
        lockerFee: lockerFee,
        seatFee: seatFee,
        monthlyTotal: monthlyTotal,
        total: total
      };
      
      // Save to database
      const db = getDatabase();
      db.receipts.unshift(receiptData); // Add to beginning of array
      saveDatabase(db);
      
      // Add to audit trail
      addAuditEntry(`Generated receipt #${receiptId} for ${name}`);
      
      // Fill receipt
      document.getElementById('receiptId').textContent = receiptId;
      document.getElementById('verifyId').textContent = receiptId;
      document.getElementById('receiptDateTime').textContent = `${date.toLocaleDateString('en-IN')} ${date.toLocaleTimeString('en-IN', { hour: '2-digit', minute: '2-digit' })}`;
      document.getElementById('studentName').textContent = name;
      document.getElementById('fatherName').textContent = father;
      document.getElementById('aadhaarNo').textContent = aadhaar ? `${aadhaar.substring(0, 4)}-XXXX-${aadhaar.substring(8)}` : 'Not provided';
      document.getElementById('contactNo').textContent = contact ? `XXXXXX${contact.substring(6)}` : 'Not provided';
      
      // Set plan description
      let planDescription = '';
      switch(plan) {
        case 'morning': planDescription = 'Morning (6am to 2pm)'; break;
        case 'evening': planDescription = 'Evening (2pm to 10pm)'; break;
        case 'full': planDescription = 'Full Day (12hrs)'; break;
        case '24hrs': planDescription = '24 Hours'; break;
        case 'night': planDescription = 'Night Shift'; break;
        default: planDescription = 'Unknown Plan';
      }
      document.getElementById('planDescription').textContent = planDescription;
      
      // Update fee display
      document.getElementById('baseFeeValue').textContent = baseFee;
      document.getElementById('monthlyTotal').textContent = monthlyTotal;
      document.getElementById('finalTotal').textContent = total;
      document.getElementById('monthsPaid').textContent = months;
      
      // Update locker and seat rows visibility
      document.getElementById('lockerRow').style.display = locker ? 'table-row' : 'none';
      document.getElementById('seatRow').style.display = seat ? 'table-row' : 'none';
      
      // Show receipt and actions
      document.getElementById('receiptOutput').style.display = 'block';
      document.getElementById('receiptActions').style.display = 'block';
      
      // Auto-print if enabled
      if (settings.app.autoPrint) {
        setTimeout(printReceipt, 500);
      }
      
      showNotification('Receipt generated successfully!', 'success');
    }
    
    function downloadPDF() {
      const { jsPDF } = window.jspdf;
      
      // Get receipt data
      const receiptId = document.getElementById('receiptId').textContent;
      const receiptDateTime = document.getElementById('receiptDateTime').textContent;
      const studentName = document.getElementById('studentName').textContent;
      const fatherName = document.getElementById('fatherName').textContent;
      const aadhaarNo = document.getElementById('aadhaarNo').textContent;
      const contactNo = document.getElementById('contactNo').textContent;
      const planDescription = document.getElementById('planDescription').textContent;
      const baseFeeValue = parseInt(document.getElementById('baseFeeValue').textContent);
      const monthlyTotal = parseInt(document.getElementById('monthlyTotal').textContent);
      const finalTotal = parseInt(document.getElementById('finalTotal').textContent);
      const monthsPaid = document.getElementById('monthsPaid').textContent;
      const lockerVisible = document.getElementById('lockerRow').style.display !== 'none';
      const seatVisible = document.getElementById('seatRow').style.display !== 'none';
      
      // Create PDF
      const doc = new jsPDF();
      
      // Add library logo (simple text version for now)
      doc.setFontSize(20);
      doc.setTextColor(0, 102, 204);
      doc.text('SPECTRUM LIBRARY', 105, 20, null, null, 'center');
      
      doc.setFontSize(16);
      doc.setTextColor(0, 0, 0);
      doc.text('Official Fee Receipt', 105, 30, null, null, 'center');
      
      // Add horizontal line
      doc.setDrawColor(0, 102, 204);
      doc.setLineWidth(0.5);
      doc.line(20, 35, 190, 35);
      
      // Receipt ID
      doc.setFontSize(12);
      doc.setTextColor(100, 100, 100);
      doc.text(`Receipt ID: ${receiptId}`, 20, 45);
      doc.text(`Date & Time: ${receiptDateTime}`, 140, 45, null, null, 'right');
      
      // Student details
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.text('Student Information', 20, 55);
      
      doc.setFontSize(10);
      doc.text(`Name: ${studentName}`, 20, 62);
      doc.text(`Father's Name: ${fatherName}`, 20, 68);
      doc.text(`Aadhaar: ${aadhaarNo}`, 20, 74);
      if (contactNo !== 'Not provided') {
        doc.text(`Contact: ${contactNo}`, 20, 80);
      }
      
      // Fee details
      doc.setFontSize(12);
      doc.text('Fee Details', 20, 90);
      
      // Create table for fee details
      const feeData = [
        ['Description', 'Amount (₹)'],
        [planDescription, baseFeeValue],
      ];
      
      if (lockerVisible) {
        feeData.push(['Locker Facility', '100']);
      }
      
      if (seatVisible) {
        feeData.push(['Seat Reservation', '50']);
      }
      
      feeData.push(['Monthly Total', monthlyTotal]);
      feeData.push([`Total for ${monthsPaid} Month(s)`, finalTotal]);
      
      doc.autoTable({
        head: [feeData[0]],
        body: feeData.slice(1),
        startY: 95,
        theme: 'striped',
        styles: { fontSize: 10 },
        headStyles: { fillColor: [0, 102, 204] }
      });
      
      // Footer
      const finalY = doc.lastAutoTable.finalY + 10;
      doc.setFontSize(10);
      doc.setTextColor(100, 100, 100);
      doc.text('Thank you for being a valued member of Spectrum Library', 105, finalY + 5, null, null, 'center');
      doc.text('This is a computer-generated receipt and does not require a signature', 105, finalY + 10, null, null, 'center');
      
      doc.text(`Verify at: spectrumlibrary.in/verify?id=${receiptId}`, 105, finalY + 20, null, null, 'center');
      
      // Save the PDF
      doc.save(`Spectrum_Receipt_${receiptId}.pdf`);
      
      addAuditEntry(`Downloaded PDF for receipt #${receiptId}`);
      showNotification('PDF receipt downloaded successfully', 'success');
    }
    
    function printReceipt() {
      const receiptDiv = document.getElementById('receiptOutput');
      const originalDisplay = receiptDiv.style.display;
      
      // Show all elements that might be hidden
      receiptDiv.style.display = 'block';
      
      // Create a new window for printing
      const printWindow = window.open('', '_blank');
      printWindow.document.write(`
        <html>
          <head>
            <title>Print Receipt</title>
            <style>
              body { font-family: Arial, sans-serif; padding: 20px; }
              .receipt-container { max-width: 800px; margin: 0 auto; background: white; padding: 30px; border: 1px solid #e0e0e0; }
              .receipt-header { text-align: center; margin-bottom: 25px; padding-bottom: 15px; border-bottom: 1px solid #eee; }
              .receipt-title { font-size: 24px; font-weight: 700; color: #0066cc; margin: 10px 0; }
              .receipt-subtitle { color: #6c757d; font-size: 16px; }
              .receipt-id { text-align: right; font-weight: 600; color: #343a40; margin-bottom: 20px; }
              .receipt-details { margin-bottom: 25px; }
              .detail-row { display: flex; padding: 8px 0; border-bottom: 1px solid #f0f0f0; }
              .detail-label { flex: 0 0 180px; font-weight: 500; color: #343a40; }
              .detail-value { flex: 1; }
              .fee-table { width: 100%; border-collapse: collapse; margin: 20px 0; }
              .fee-table th { background: #0066cc; color: white; text-align: left; padding: 10px 15px; }
              .fee-table td { padding: 10px 15px; border-bottom: 1px solid #f0f0f0; }
              .fee-table tr:nth-child(even) { background: #f8f9fa; }
              .total-row { font-weight: 700; background: #e9f7fe !important; }
              .receipt-footer { margin-top: 30px; padding-top: 20px; border-top: 1px solid #eee; text-align: center; color: #6c757d; font-size: 14px; }
            </style>
          </head>
          <body>
            ${receiptDiv.innerHTML}
          </body>
        </html>
      `);
      
      printWindow.document.close();
      
      // Print after a short delay to ensure content is loaded
      printWindow.addEventListener('load', function() {
        printWindow.print();
        setTimeout(function() {
          printWindow.close();
          receiptDiv.style.display = originalDisplay;
        }, 500);
      });
      
      addAuditEntry(`Printed receipt #${document.getElementById('receiptId').textContent}`);
    }
    
    function resetForm() {
      // Reset form fields
      document.getElementById('name').value = '';
      document.getElementById('father').value = '';
      document.getElementById('aadhaar').value = '';
      document.getElementById('contact').value = '';
      document.getElementById('plan').value = '';
      document.getElementById('locker').checked = false;
      document.getElementById('seat').checked = false;
      document.getElementById('months').value = '1';
      
      // Reset fee calculation
      updateFeeCalculation();
      
      // Hide receipt and actions
      document.getElementById('receiptOutput').style.display = 'none';
      document.getElementById('receiptActions').style.display = 'none';
      
      // Focus on name field
      document.getElementById('name').focus();
      
      showNotification('Form reset successfully', 'info');
    }
    
    // ===== VERIFICATION SYSTEM =====
    function verifyReceipt() {
      const receiptId = document.getElementById('verifyInput').value.trim().toUpperCase();
      const resultDiv = document.getElementById('verificationResult');
      
      if (!receiptId) {
        resultDiv.innerHTML = `
          <div class="invalid-receipt">
            <p>Please enter a receipt ID</p>
          </div>
        `;
        return;
      }
      
      // Get all receipts
      const db = getDatabase();
      const receipt = db.receipts.find(r => r.id.toUpperCase() === receiptId);
      
      if (receipt) {
        // Format plan name
        let planName = '';
        switch(receipt.plan) {
          case 'morning': planName = 'Morning (6am to 2pm)'; break;
          case 'evening': planName = 'Evening (2pm to 10pm)'; break;
          case 'full': planName = 'Full Day (12hrs)'; break;
          case '24hrs': planName = '24 Hours'; break;
          case 'night': planName = 'Night Shift'; break;
          default: planName = receipt.plan;
        }
        
        resultDiv.innerHTML = `
          <div class="valid-receipt">
            <h3 style="margin-top: 0;">✅ VALID RECEIPT</h3>
            <p><strong>ID:</strong> ${receipt.id}</p>
            <p><strong>Date:</strong> ${receipt.date} ${receipt.time}</p>
            <p><strong>Student:</strong> ${receipt.name}</p>
            <p><strong>Father:</strong> ${receipt.father}</p>
            <p><strong>Plan:</strong> ${planName}</p>
            <p><strong>Months:</strong> ${receipt.months}</p>
            <p><strong>Total Fee:</strong> ₹${receipt.total}</p>
            <p><strong>Aadhaar:</strong> ${receipt.aadhaar ? `${receipt.aadhaar.substring(0, 4)}-XXXX-${receipt.aadhaar.substring(8)}` : 'Not provided'}</p>
            ${receipt.locker ? '<p><strong>Locker:</strong> Included (+₹100)</p>' : ''}
            ${receipt.seat ? '<p><strong>Seat:</strong> Reserved (+₹50)</p>' : ''}
            <div style="margin-top: 15px; text-align: center;">
              <button class="btn btn-primary" onclick="generateVerificationPDF('${receipt.id}')">
                <span class="btn-icon">↓</span> Download Verification
              </button>
            </div>
          </div>
        `;
        
        addAuditEntry(`Verified receipt #${receiptId}`);
        showNotification('Receipt verified successfully', 'success');
      } else {
        resultDiv.innerHTML = `
          <div class="invalid-receipt">
            <h3 style="margin-top: 0;">❌ INVALID RECEIPT</h3>
            <p>No record found for ID: <strong>${receiptId}</strong></p>
            <p>Check if the ID is entered correctly or contact library staff.</p>
          </div>
        `;
        
        addAuditEntry(`Failed verification attempt for ID: ${receiptId}`);
        showNotification('Receipt not found', 'error');
      }
    }
    
    function generateVerificationPDF(receiptId) {
      const db = getDatabase();
      const receipt = db.receipts.find(r => r.id === receiptId);
      
      if (!receipt) {
        showNotification('Receipt not found', 'error');
        return;
      }
      
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      
      // Add content
      doc.setFontSize(20);
      doc.setTextColor(0, 102, 204);
      doc.text('SPECTRUM LIBRARY', 105, 20, null, null, 'center');
      
      doc.setFontSize(16);
      doc.setTextColor(0, 0, 0);
      doc.text('Receipt Verification', 105, 30, null, null, 'center');
      
      // Add horizontal line
      doc.setDrawColor(0, 102, 204);
      doc.setLineWidth(0.5);
      doc.line(20, 35, 190, 35);
      
      // Verification details
      doc.setFontSize(12);
      doc.text('Verification Details', 20, 45);
      
      doc.setFontSize(10);
      doc.text(`Receipt ID: ${receipt.id}`, 20, 52);
      doc.text(`Date: ${receipt.date} ${receipt.time}`, 20, 58);
      doc.text(`Student: ${receipt.name}`, 20, 64);
      doc.text(`Father: ${receipt.father}`, 20, 70);
      doc.text(`Plan: ${getPlanName(receipt.plan)}`, 20, 76);
      doc.text(`Months: ${receipt.months}`, 20, 82);
      doc.text(`Total Fee: ₹${receipt.total}`, 20, 88);
      
      // Verification status
      doc.setFontSize(14);
      doc.setTextColor(40, 167, 69);
      doc.text('VERIFICATION STATUS: VALID', 20, 100);
      
      doc.setTextColor(0, 0, 0);
      doc.setFontSize(10);
      doc.text('This receipt has been verified against the Spectrum Library database.', 20, 108);
      doc.text('You may contact library staff for any questions or concerns.', 20, 114);
      
      // Footer
      doc.setFontSize(8);
      doc.setTextColor(100, 100, 100);
      doc.text(`Verified on: ${new Date().toLocaleDateString('en-IN')} ${new Date().toLocaleTimeString('en-IN')}`, 20, 125);
      doc.text(`Verification ID: VRF-${new Date().getTime()}`, 20, 130);
      
      // Save the PDF
      doc.save(`Receipt_Verification_${receipt.id}.pdf`);
      
      addAuditEntry(`Generated verification PDF for receipt #${receipt.id}`);
      showNotification('Verification PDF downloaded successfully', 'success');
    }
    
    function getPlanName(planCode) {
      switch(planCode) {
        case 'morning': return 'Morning (6am to 2pm)';
        case 'evening': return 'Evening (2pm to 10pm)';
        case 'full': return 'Full Day (12hrs)';
        case '24hrs': return '24 Hours';
        case 'night': return 'Night Shift';
        default: return planCode;
      }
    }
    
    // ===== DATA MANAGEMENT =====
    let currentPage = 1;
    const RECORDS_PER_PAGE = 10;
    
    function renderDataTab() {
      const db = getDatabase();
      const searchQuery = document.getElementById('searchInput').value.toLowerCase();
      
      // Filter records
      let filteredRecords = [...db.receipts];
      if (searchQuery) {
        filteredRecords = filteredRecords.filter(receipt => 
          receipt.id.toLowerCase().includes(searchQuery) ||
          receipt.name.toLowerCase().includes(searchQuery) ||
          (receipt.aadhaar && receipt.aadhaar.toLowerCase().includes(searchQuery))
        );
      }
      
      // Update records summary
      document.getElementById('recordsSummary').innerHTML = 
        `Showing <strong>${Math.min((currentPage - 1) * RECORDS_PER_PAGE + 1, filteredRecords.length)}</strong> to 
         <strong>${Math.min(currentPage * RECORDS_PER_PAGE, filteredRecords.length)}</strong> of 
         <strong>${filteredRecords.length}</strong> records`;
      
      // Calculate pagination
      const totalPages = Math.ceil(filteredRecords.length / RECORDS_PER_PAGE);
      const startIndex = (currentPage - 1) * RECORDS_PER_PAGE;
      const endIndex = Math.min(startIndex + RECORDS_PER_PAGE, filteredRecords.length);
      
      // Render records table
      const tableBody = document.getElementById('recordsTableBody');
      tableBody.innerHTML = '';
      
      for (let i = startIndex; i < endIndex; i++) {
        const receipt = filteredRecords[i];
        const row = document.createElement('tr');
        
        row.innerHTML = `
          <td>${receipt.date}</td>
          <td>${receipt.id}</td>
          <td>${receipt.name}</td>
          <td>${receipt.aadhaar ? `${receipt.aadhaar.substring(0, 4)}-XXXX-${receipt.aadhaar.substring(8)}` : 'N/A'}</td>
          <td>${getPlanName(receipt.plan)}</td>
          <td>₹${receipt.total}</td>
          <td class="action-buttons">
            <button class="btn btn-info" title="View" onclick="viewReceipt('${receipt.id}')">
              <span class="btn-icon">👁️</span>
            </button>
            <button class="btn btn-success" title="Download PDF" onclick="downloadReceiptPDF('${receipt.id}')">
              <span class="btn-icon">↓</span>
            </button>
            <button class="btn btn-danger" title="Delete" onclick="deleteReceipt('${receipt.id}')">
              <span class="btn-icon">🗑️</span>
            </button>
          </td>
        `;
        
        tableBody.appendChild(row);
      }
      
      // Render pagination
      renderPagination(totalPages);
    }
    
    function renderPagination(totalPages) {
      const paginationDiv = document.getElementById('paginationControls');
      paginationDiv.innerHTML = '';
      
      if (totalPages <= 1) return;
      
      // Previous button
      const prevBtn = document.createElement('div');
      prevBtn.className = 'page-item';
      prevBtn.innerHTML = '&laquo;';
      prevBtn.onclick = () => {
        if (currentPage > 1) {
          currentPage--;
          renderDataTab();
        }
      };
      paginationDiv.appendChild(prevBtn);
      
      // Page numbers
      const startPage = Math.max(1, currentPage - 2);
      const endPage = Math.min(totalPages, startPage + 4);
      
      for (let i = startPage; i <= endPage; i++) {
        const pageItem = document.createElement('div');
        pageItem.className = `page-item ${i === currentPage ? 'active' : ''}`;
        pageItem.textContent = i;
        pageItem.onclick = () => {
          currentPage = i;
          renderDataTab();
        };
        paginationDiv.appendChild(pageItem);
      }
      
      // Next button
      const nextBtn = document.createElement('div');
      nextBtn.className = 'page-item';
      nextBtn.innerHTML = '&raquo;';
      nextBtn.onclick = () => {
        if (currentPage < totalPages) {
          currentPage++;
          renderDataTab();
        }
      };
      paginationDiv.appendChild(nextBtn);
    }
    
    function filterRecords() {
      currentPage = 1;
      renderDataTab();
    }
    
    function viewReceipt(receiptId) {
      const db = getDatabase();
      const receipt = db.receipts.find(r => r.id === receiptId);
      
      if (!receipt) {
        showNotification('Receipt not found', 'error');
        return;
      }
      
      // Switch to generate tab
      showTab('generateTab');
      
      // Fill form with receipt data
      document.getElementById('name').value = receipt.name;
      document.getElementById('father').value = receipt.father;
      document.getElementById('aadhaar').value = receipt.aadhaar;
      document.getElementById('contact').value = receipt.contact;
      document.getElementById('plan').value = receipt.plan;
      document.getElementById('locker').checked = receipt.locker;
      document.getElementById('seat').checked = receipt.seat;
      document.getElementById('months').value = receipt.months;
      
      // Update fee calculation
      updateFeeCalculation();
      
      // Show receipt
      document.getElementById('receiptOutput').style.display = 'block';
      document.getElementById('receiptActions').style.display = 'block';
      
      // Fill receipt details
      document.getElementById('receiptId').textContent = receipt.id;
      document.getElementById('verifyId').textContent = receipt.id;
      document.getElementById('receiptDateTime').textContent = `${receipt.date} ${receipt.time}`;
      document.getElementById('studentName').textContent = receipt.name;
      document.getElementById('fatherName').textContent = receipt.father;
      document.getElementById('aadhaarNo').textContent = receipt.aadhaar ? `${receipt.aadhaar.substring(0, 4)}-XXXX-${receipt.aadhaar.substring(8)}` : 'Not provided';
      document.getElementById('contactNo').textContent = receipt.contact ? `XXXXXX${receipt.contact.substring(6)}` : 'Not provided';
      
      // Set plan description
      let planDescription = '';
      switch(receipt.plan) {
        case 'morning': planDescription = 'Morning (6am to 2pm)'; break;
        case 'evening': planDescription = 'Evening (2pm to 10pm)'; break;
        case 'full': planDescription = 'Full Day (12hrs)'; break;
        case '24hrs': planDescription = '24 Hours'; break;
        case 'night': planDescription = 'Night Shift'; break;
        default: planDescription = 'Unknown Plan';
      }
      document.getElementById('planDescription').textContent = planDescription;
      
      // Update fee display
      document.getElementById('baseFeeValue').textContent = receipt.baseFee;
      document.getElementById('monthlyTotal').textContent = receipt.monthlyTotal;
      document.getElementById('finalTotal').textContent = receipt.total;
      document.getElementById('monthsPaid').textContent = receipt.months;
      
      // Update locker and seat rows visibility
      document.getElementById('lockerRow').style.display = receipt.locker ? 'table-row' : 'none';
      document.getElementById('seatRow').style.display = receipt.seat ? 'table-row' : 'none';
      
      showNotification('Receipt loaded successfully', 'success');
    }
    
    function downloadReceiptPDF(receiptId) {
      const db = getDatabase();
      const receipt = db.receipts.find(r => r.id === receiptId);
      
      if (!receipt) {
        showNotification('Receipt not found', 'error');
        return;
      }
      
      // Switch to generate tab and fill data
      viewReceipt(receiptId);
      
      // Generate PDF after a short delay to ensure UI updates
      setTimeout(() => {
        downloadPDF();
      }, 300);
    }
    
    function deleteReceipt(receiptId) {
      if (!confirm('Are you sure you want to delete this receipt? This action cannot be undone.')) {
        return;
      }
      
      const db = getDatabase();
      const initialCount = db.receipts.length;
      
      db.receipts = db.receipts.filter(r => r.id !== receiptId);
      
      if (db.receipts.length < initialCount) {
        saveDatabase(db);
        addAuditEntry(`Deleted receipt #${receiptId}`);
        showNotification('Receipt deleted successfully', 'success');
        renderDataTab();
      } else {
        showNotification('Receipt not found', 'error');
      }
    }
    
    function exportToCSV() {
      const db = getDatabase();
      
      // CSV header
      const headers = ['ID', 'Date', 'Time', 'Student', 'Father', 'Aadhaar', 'Contact', 'Plan', 'Months', 'Base Fee', 'Locker', 'Seat', 'Total'];
      let csv = [headers.join(',')];
      
      // Add data rows
      db.receipts.forEach(receipt => {
        const row = [
          `"${receipt.id}"`,
          `"${receipt.date}"`,
          `"${receipt.time}"`,
          `"${receipt.name}"`,
          `"${receipt.father}"`,
          `"${receipt.aadhaar}"`,
          `"${receipt.contact}"`,
          `"${getPlanName(receipt.plan)}"`,
          receipt.months,
          receipt.baseFee,
          receipt.locker ? 'Yes' : 'No',
          receipt.seat ? 'Yes' : 'No',
          receipt.total
        ];
        csv.push(row.join(','));
      });
      
      // Create and download CSV
      const csvData = csv.join('\n');
      const blob = new Blob([csvData], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      
      const a = document.createElement('a');
      a.href = url;
      a.download = `Spectrum_Library_Receipts_${new Date().toISOString().slice(0, 10)}.csv`;
      document.body.appendChild(a);
      a.click();
      
      setTimeout(() => {
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
        addAuditEntry('Exported receipts to CSV');
        showNotification('CSV exported successfully', 'success');
      }, 100);
    }
    
    function backupData() {
      const db = getDatabase();
      const backup = {
        version: APP_VERSION,
        timestamp: new Date().toISOString(),
        data: db
      };
      
      const backupData = JSON.stringify(backup, null, 2);
      const blob = new Blob([backupData], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      
      const a = document.createElement('a');
      a.href = url;
      a.download = `spectrum_library_backup_${new Date().toISOString().slice(0, 10)}.json`;
      document.body.appendChild(a);
      a.click();
      
      setTimeout(() => {
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
        addAuditEntry('Created data backup');
        showNotification('Data backup created successfully', 'success');
      }, 100);
    }
    
    function restoreData() {
      if (!confirm('Are you sure you want to restore data? This will replace your current data.')) {
        return;
      }
      
      // Create file input
      const fileInput = document.createElement('input');
      fileInput.type = 'file';
      fileInput.accept = '.json';
      
      fileInput.onchange = function(event) {
        const file = event.target.files[0];
        if (!file) return;
        
        const reader = new FileReader();
        reader.onload = function(e) {
          try {
            const backup = JSON.parse(e.target.result);
            
            // Validate backup format
            if (!backup.version || !backup.timestamp || !backup.data) {
              throw new Error('Invalid backup format');
            }
            
            // Restore data
            localStorage.setItem(DB_NAME, JSON.stringify(backup.data));
            addAuditEntry('Restored data from backup');
            
            // Refresh UI
            loadSettings();
            renderInitialData();
            
            showNotification('Data restored successfully', 'success');
          } catch (error) {
            console.error('Restore error:', error);
            showNotification('Failed to restore data: Invalid backup file', 'error');
          }
        };
        reader.readAsText(file);
      };
      
      fileInput.click();
    }
    
    // ===== SETTINGS MANAGEMENT =====
    function loadSettings() {
      const settings = getSettings();
      
      // Load library settings
      document.getElementById('libraryName').value = settings.library.name;
      document.getElementById('libraryAddress').value = settings.library.address;
      document.getElementById('libraryContact').value = settings.library.contact;
      
      // Load fee structure
      const fs = settings.feeStructure;
      document.getElementById('morningBase').value = fs.morning.base;
      document.getElementById('morningLocker').value = fs.morning.locker || (fs.morning.base + 100);
      document.getElementById('morningSeat').value = fs.morning.seat || (fs.morning.base + 50);
      
      document.getElementById('eveningBase').value = fs.evening.base;
      document.getElementById('eveningLocker').value = fs.evening.locker || (fs.evening.base + 100);
      document.getElementById('eveningSeat').value = fs.evening.seat || (fs.evening.base + 50);
      
      document.getElementById('fullBase').value = fs.full.base;
      document.getElementById('fullLocker').value = fs.full.locker || (fs.full.base + 100);
      document.getElementById('fullSeat').value = fs.full.seat || (fs.full.base + 50);
      
      document.getElementById('allDayBase').value = fs['24hrs'].base;
      document.getElementById('allDayLocker').value = fs['24hrs'].locker || (fs['24hrs'].base + 100);
      document.getElementById('allDaySeat').value = fs['24hrs'].seat || (fs['24hrs'].base + 50);
      
      document.getElementById('nightBase').value = fs.night.base;
      
      // Load app settings
      document.getElementById('languageSelect').value = settings.app.language;
      document.getElementById('autoPrint').checked = settings.app.autoPrint;
      document.getElementById('requireAadhaar').checked = settings.app.requireAadhaar;
      document.getElementById('enableSeatReservation').checked = settings.app.enableSeatReservation;
      document.getElementById('enableLocker').checked = settings.app.enableLocker;
      
      // Update UI based on settings
      updateSettingsUI();
    }
    
    function updateSettingsUI() {
      const settings = getSettings();
      
      // Toggle seat reservation UI
      const seatRows = document.querySelectorAll('#seat, .seat-option');
      seatRows.forEach(el => {
        el.style.display = settings.app.enableSeatReservation ? 'block' : 'none';
      });
      
      // Toggle locker UI
      const lockerRows = document.querySelectorAll('#locker, .locker-option');
      lockerRows.forEach(el => {
        el.style.display = settings.app.enableLocker ? 'block' : 'none';
      });
      
      // Toggle Aadhaar requirement
      const aadhaarField = document.querySelector('#aadhaar');
      if (aadhaarField) {
        if (settings.app.requireAadhaar) {
          aadhaarField.required = true;
          aadhaarField.placeholder = 'Enter 12-digit Aadhaar';
        } else {
          aadhaarField.required = false;
          aadhaarField.placeholder = 'Enter Aadhaar (optional)';
        }
      }
    }
    
    function loadSettingsIntoUI() {
      loadSettings();
      updateSettingsUI();
      renderAuditTrail();
    }
    
    function saveLibrarySettings() {
      const settings = getSettings();
      
      settings.library = {
        name: document.getElementById('libraryName').value,
        address: document.getElementById('libraryAddress').value,
        contact: document.getElementById('libraryContact').value
      };
      
      saveSettings(settings);
      addAuditEntry('Updated library settings');
      showNotification('Library settings saved successfully', 'success');
    }
    
    function saveFeeStructure() {
      const settings = getSettings();
      
      settings.feeStructure = {
        morning: {
          base: parseInt(document.getElementById('morningBase').value) || 500,
          locker: parseInt(document.getElementById('morningLocker').value) || 600,
          seat: parseInt(document.getElementById('morningSeat').value) || 550
        },
        evening: {
          base: parseInt(document.getElementById('eveningBase').value) || 700,
          locker: parseInt(document.getElementById('eveningLocker').value) || 800,
          seat: parseInt(document.getElementById('eveningSeat').value) || 750
        },
        full: {
          base: parseInt(document.getElementById('fullBase').value) || 900,
          locker: parseInt(document.getElementById('fullLocker').value) || 1000,
          seat: parseInt(document.getElementById('fullSeat').value) || 950
        },
        '24hrs': {
          base: parseInt(document.getElementById('allDayBase').value) || 1000,
          locker: parseInt(document.getElementById('allDayLocker').value) || 1100,
          seat: parseInt(document.getElementById('allDaySeat').value) || 1050
        },
        night: {
          base: parseInt(document.getElementById('nightBase').value) || 350
        }
      };
      
      saveSettings(settings);
      addAuditEntry('Updated fee structure');
      showNotification('Fee structure saved successfully', 'success');
    }
    
    function saveAppSettings() {
      const settings = getSettings();
      
      settings.app = {
        language: document.getElementById('languageSelect').value,
        autoPrint: document.getElementById('autoPrint').checked,
        requireAadhaar: document.getElementById('requireAadhaar').checked,
        enableSeatReservation: document.getElementById('enableSeatReservation').checked,
        enableLocker: document.getElementById('enableLocker').checked
      };
      
      saveSettings(settings);
      updateSettingsUI();
      addAuditEntry('Updated application settings');
      showNotification('Application settings saved successfully', 'success');
    }
    
    function renderAuditTrail() {
      const db = getDatabase();
      const auditTrailDiv = document.getElementById('auditTrail');
      
      auditTrailDiv.innerHTML = '';
      
      if (db.auditTrail.length === 0) {
        auditTrailDiv.innerHTML = '<p>No audit entries yet</p>';
        return;
      }
      
      db.auditTrail.forEach(entry => {
        const entryDiv = document.createElement('div');
        entryDiv.style.padding = '8px 0';
        entryDiv.style.borderBottom = '1px solid var(--border)';
        
        entryDiv.innerHTML = `
          <div style="font-weight: 500; margin-bottom: 3px;">${entry.timestamp}</div>
          <div>${entry.action}</div>
        `;
        
        auditTrailDiv.appendChild(entryDiv);
      });
    }
    
    function clearAuditTrail() {
      if (!confirm('Are you sure you want to clear the audit trail? This action cannot be undone.')) {
        return;
      }
      
      const db = getDatabase();
      db.auditTrail = [];
      saveDatabase(db);
      
      renderAuditTrail();
      addAuditEntry('Cleared audit trail');
      showNotification('Audit trail cleared successfully', 'success');
    }
    
    // ===== UI UTILITIES =====
    function showNotification(message, type = 'info') {
      const notification = document.getElementById('notification');
      notification.textContent = message;
      notification.className = `notification ${type} show`;
      
      setTimeout(() => {
        notification.classList.remove('show');
      }, 3000);
    }
    
    function openHelpModal() {
      document.getElementById('helpModal').classList.add('show');
    }
    
    function closeHelpModal() {
      document.getElementById('helpModal').classList.remove('show');
    }
  </script>
</body>
</html>
