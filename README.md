<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Калькулятор 3D-печати (PETG) и Приёмка ПВЗ</title>
  <style>
    :root {
      --primary: #2563eb;
      --primary-hover: #1d4ed8;
      --bg: #f8fafc;
      --card-bg: #ffffff;
      --text: #1e293b;
      --text-muted: #64748b;
      --border: #e2e8f0;
      --success: #16a34a;
      --danger: #dc2626;
      --warning: #d97706;
    }

    * {
      box-sizing: border-box;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    body {
      margin: 0;
      padding: 20px;
      background-color: var(--bg);
      color: var(--text);
      line-height: 1.5;
    }

    .container {
      max-width: 1120px;
      margin: 0 auto;
    }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;
      flex-wrap: wrap;
      gap: 12px;
    }

    h1 {
      margin: 0;
      font-size: 24px;
      font-weight: 700;
      cursor: default;
      user-select: none;
    }

    .tabs {
      display: flex;
      gap: 8px;
      align-items: center;
    }

    .tab-btn {
      padding: 10px 18px;
      border: 1px solid var(--border);
      background: var(--card-bg);
      border-radius: 8px;
      font-weight: 600;
      font-size: 14px;
      cursor: pointer;
      transition: all 0.2s;
      color: var(--text);
    }

    .tab-btn.active {
      background: var(--primary);
      color: #fff;
      border-color: var(--primary);
    }

    #tab-pvz {
      display: none;
      background: #fef3c7;
      border-color: #f59e0b;
      color: #92400e;
    }

    #tab-pvz.active {
      background: #d97706;
      color: #fff;
      border-color: #d97706;
    }

    #btn-lock-pvz {
      display: none;
      padding: 10px 14px;
      border: 1px solid #fecaca;
      background: #fef2f2;
      color: var(--danger);
      border-radius: 8px;
      font-weight: 600;
      font-size: 13px;
      cursor: pointer;
      transition: all 0.2s;
    }

    #btn-lock-pvz:hover {
      background: #fee2e2;
    }

    .grid {
      display: grid;
      grid-template-columns: 1.15fr 0.85fr;
      gap: 24px;
    }

    @media (max-width: 860px) {
      .grid {
        grid-template-columns: 1fr;
      }
    }

    .card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 24px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.06);
      border: 1px solid var(--border);
    }

    .card h2 {
      margin-top: 0;
      font-size: 18px;
      margin-bottom: 16px;
      padding-bottom: 12px;
      border-bottom: 1px solid var(--border);
    }

    .form-group {
      margin-bottom: 16px;
    }

    .form-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    @media (max-width: 540px) {
      .form-row {
        grid-template-columns: 1fr;
      }
    }

    label {
      display: block;
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 6px;
      color: var(--text);
    }

    input[type="text"],
    input[type="tel"],
    input[type="email"],
    input[type="number"],
    select,
    textarea {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 14px;
      background: #fff;
      color: var(--text);
    }

    input:focus, select:focus, textarea:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.12);
    }

    .file-drop {
      border: 2px dashed #cbd5e1;
      border-radius: 10px;
      padding: 20px;
      text-align: center;
      cursor: pointer;
      transition: all 0.2s;
      background: #f8fafc;
    }

    .file-drop:hover, .file-drop.dragover {
      border-color: var(--primary);
      background: #eff6ff;
    }

    .file-drop input {
      display: none;
    }

    .color-palette {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 8px;
    }

    .color-swatch {
      width: 34px;
      height: 34px;
      border-radius: 50%;
      border: 2px solid #cbd5e1;
      cursor: pointer;
      position: relative;
      transition: transform 0.15s, box-shadow 0.15s;
    }

    .color-swatch:hover {
      transform: scale(1.08);
    }

    .color-swatch.selected {
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.3);
      transform: scale(1.1);
    }

    .slicer-box {
      background: #f8fafc;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 14px;
      margin-bottom: 16px;
    }

    .slicer-title {
      font-size: 13px;
      font-weight: 700;
      margin-bottom: 10px;
      color: var(--text);
    }

    .contact-box {
      background: #f8fafc;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 14px;
      margin-bottom: 16px;
    }

    .contact-title {
      font-size: 13px;
      font-weight: 700;
      margin-bottom: 10px;
      color: var(--text);
    }

    .viewer-container {
      width: 100%;
      height: 290px;
      background: #0f172a;
      border-radius: 10px;
      position: relative;
      overflow: hidden;
      margin-bottom: 14px;
      border: 1px solid var(--border);
    }

    #viewer-canvas {
      width: 100%;
      height: 100%;
      display: block;
    }

    .viewer-hint {
      position: absolute;
      bottom: 8px;
      left: 10px;
      right: 10px;
      text-align: center;
      font-size: 11px;
      color: rgba(255, 255, 255, 0.78);
      background: rgba(15, 23, 42, 0.65);
      padding: 4px 8px;
      border-radius: 6px;
      pointer-events: none;
    }

    .viewer-placeholder {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      color: #94a3b8;
      font-size: 13px;
      text-align: center;
      padding: 16px;
      pointer-events: none;
    }

    .stats-box {
      background: #f1f5f9;
      border-radius: 10px;
      padding: 14px;
      margin-bottom: 16px;
    }

    .stat-row {
      display: flex;
      justify-content: space-between;
      font-size: 13px;
      padding: 5px 0;
      border-bottom: 1px dashed #cbd5e1;
    }

    .stat-row:last-child {
      border-bottom: none;
    }

    .stat-val {
      font-weight: 600;
    }

    .total-price-box {
      background: #eff6ff;
      border: 1px solid #bfdbfe;
      border-radius: 10px;
      padding: 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
    }

    .total-price-box span:first-child {
      font-weight: 600;
      font-size: 15px;
    }

    .total-price {
      font-size: 24px;
      font-weight: 800;
      color: var(--primary);
    }

    .btn {
      width: 100%;
      padding: 12px 18px;
      border: none;
      border-radius: 8px;
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
      transition: background 0.2s;
    }

    .btn-primary {
      background: var(--primary);
      color: #fff;
    }

    .btn-primary:hover {
      background: var(--primary-hover);
    }

    .btn-success {
      background: var(--success);
      color: #fff;
    }

    .btn-success:hover {
      background: #15803d;
    }

    .btn-outline {
      background: #fff;
      border: 1px solid var(--border);
      color: var(--text);
    }

    .btn-outline:hover {
      background: #f1f5f9;
    }

    .btn-download-stl {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      margin-top: 8px;
      padding: 6px 12px;
      font-size: 12px;
      font-weight: 600;
      color: var(--primary);
      background: #eff6ff;
      border: 1px solid #bfdbfe;
      border-radius: 6px;
      cursor: pointer;
      transition: all 0.15s;
    }

    .btn-download-stl:hover {
      background: #dbeafe;
    }

    .client-order-success {
      display: none;
      margin-top: 18px;
      padding: 18px;
      border: 2px solid #86efac;
      background: #f0fdf4;
      border-radius: 10px;
      text-align: center;
    }

    .client-order-success h3 {
      margin: 0 0 8px 0;
      color: #166534;
      font-size: 16px;
    }

    .qr-wrapper {
      background: #fff;
      padding: 14px;
      border-radius: 10px;
      display: inline-block;
      border: 1px solid var(--border);
      margin: 10px 0;
    }

    .qr-wrapper img,
    .qr-wrapper canvas {
      display: block;
      max-width: 100%;
      height: auto;
      margin: 0 auto;
    }

    .qr-payload-preview {
      text-align: left;
      background: #fff;
      border: 1px solid #cbd5e1;
      border-radius: 8px;
      padding: 10px 12px;
      font-size: 12px;
      color: var(--text);
      white-space: pre-wrap;
      margin-top: 10px;
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
    }

    .pvz-section {
      display: none;
    }

    .pvz-section.active {
      display: block;
    }

    .order-item {
      padding: 12px;
      border: 1px solid var(--border);
      border-radius: 8px;
      margin-bottom: 10px;
      cursor: pointer;
      transition: all 0.15s;
      background: #fff;
    }

    .order-item:hover {
      border-color: var(--primary);
      background: #f8fafc;
    }

    .order-item.selected {
      border-color: var(--primary);
      background: #eff6ff;
    }

    .order-header {
      display: flex;
      justify-content: space-between;
      font-weight: 700;
      font-size: 14px;
      margin-bottom: 4px;
    }

    .badge {
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 11px;
      font-weight: 600;
    }

    .badge-new {
      background: #dbeafe;
      color: #1e40af;
    }

    .badge-accepted {
      background: #dcfce7;
      color: #166534;
    }

