# 完整DB Schema與說明

## 📊 系統架構概覽

### 核心模組
- **用戶管理**: 用戶、賣家、會員等級、積分
- **商品管理**: 商品、分類、評論
- **訂單管理**: 訂單、訂單明細、付款、退貨
- **物流倉儲**: 倉庫、庫存、供應商、配送
- **數據分析**: 銷售、庫存、物流績效分析

## 🗃️ 核心資料表

### 用戶相關 (5張表)
| 表名 | 主要欄位 | 說明 |
|------|---------|------|
| **users** | user_id, username, email | 用戶基本資料 |
| **sellers** | seller_id, user_id, store_name | 賣家店舖資料 |
| **membership_levels** | level_id, level_name, discount_rate | 會員等級 |
| **points** | point_id, user_id, points_earned | 積分記錄 |
| **login_logs** | log_id, user_id, login_time | 登入記錄 |

### 商品相關 (3張表)
| 表名 | 主要欄位 | 說明 |
|------|---------|------|
| **products** | product_id, product_name, price, seller_id | 商品資料 |
| **categories** | category_id, category_name | 商品分類 |
| **reviews** | review_id, product_id, user_id, rating | 商品評論 |

### 訂單相關 (5張表)
| 表名 | 主要欄位 | 說明 |
|------|---------|------|
| **orders** | order_id, customer_id, total_amount | 訂單主檔 |
| **order_items** | order_item_id, order_id, product_id | 訂單明細 |
| **payments** | payment_id, order_id, amount | 付款記錄 |
| **returns_refunds** | return_id, order_id, refund_amount | 退貨退款 |
| **coupons** | coupon_id, coupon_code, discount_amount | 優惠券 |

### 物流相關 (8張表)
| 表名 | 主要欄位 | 說明 |
|------|---------|------|
| **warehouses** | warehouse_id, warehouse_name, capacity | 倉庫資料 |
| **inventory** | inventory_id, warehouse_id, product_id | 庫存管理 |
| **suppliers** | supplier_id, supplier_name, rating | 供應商 |
| **shipments** | shipment_id, order_id, tracking_number | 物流配送 |

## ⚙️ 技術規格

### 資料類型標準
```sql
-- ID欄位統一: INT UNSIGNED AUTO_INCREMENT
-- 金額欄位: DECIMAL(10,2) 
-- 時間欄位: TIMESTAMP
-- 文字欄位: VARCHAR(255) / TEXT
-- 布林欄位: BOOLEAN
```

### 關鍵約束
- **主鍵**: 所有表都有自增主鍵
- **外鍵**: 嚴格參照完整性
- **唯一鍵**: email, username, sku 等
- **檢查約束**: 評分 1-5、折扣率 0-100

## 🔒 安全特性

### 資料驗證
- **Email格式驗證** + XSS防護
- **密碼雜湊** (bcrypt)
- **輸入過濾** (HTML escape)
- **SQL注入防護**

### 業務邏輯約束
- 登入失敗次數限制 (< 5次)
- 庫存數量 ≥ 0
- 訂單金額 > 0
- 評分範圍 1-5

## 📈 效能考量

### 索引策略
- 主鍵自動索引
- 外鍵欄位索引
- 查詢熱點欄位 (email, sku, order_date)
- 複合索引 (user_id + created_at)

### 資料分析表
- 預先計算統計資料
- 按日期分割大型表
- 定期清理過期記錄

## 總結

✅ **21張核心表** 涵蓋完整電商功能  
✅ **統一資料規範** 確保一致性  
✅ **完整安全防護** 防止常見攻擊  
✅ **高效能設計** 支援大量併發