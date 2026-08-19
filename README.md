# SADA KALO FASHION — Inventory MVC v2.1 Final

## স্ট্রাকচার

```
public_html/
├── Core/
│   ├── Bootstrap.php
│   ├── AuthKernel.php
│   └── Csrf.php
├── inventory/
│   └── uploads/          ← ছবি (DB path: /inventory/uploads/xxx.jpg)
└── Inventory/
    ├── Helper/
    ├── Dashboard/        ✅
    ├── POS/              ✅
    ├── ProductAdd/       ✅
    ├── ItemsList/        ✅
    ├── SalesHistory/     ✅
    ├── Return/           ✅
    ├── OutOfStock/       ✅
    ├── Category/         ✅
    ├── AdminControl/     ✅ admin-only
    ├── AdminReturn/      ✅ admin-only
    ├── DailyActivity/    ✅ admin-only
    ├── Unsold/           ✅ admin-only
    └── Notification/     placeholder
```

## লজিক সারাংশ

| মডিউল | মূল লজিক |
|--------|----------|
| **POS** | ট্রানজেকশন + `FOR UPDATE` + `pieces >= qty` অ্যাটমিক ডিডাক্ট; ইউজার min price (buy+cost); অ্যাডমিন ওভাররাইড |
| **ProductAdd** | কোড অটো SKF-XXXX; upload → `/inventory/uploads/`; sell ≥ buy+cost |
| **ItemsList** | এডিট + `product_edit_history`; sell ≥ buy+cost; admin only edit |
| **Return** | pending রিকোয়েস্ট; already-returned qty হিসাব |
| **AdminReturn** | approve → stock +pieces; reject; approved delete ব্লক |
| **SalesHistory** | admin price edit → item profit + sale totals recalculate |
| **AdminControl** | stock add/set + FOR UPDATE; price update |
| **OutOfStock** | pieces = 0 |
| **Unsold** | pieces > 0 এবং last sale/created ≥ 15/30/45 দিন |
| **DailyActivity** | তারিখ অনুযায়ী add/sale/return সামারি |

## নিরাপত্তা

- সব মডিউল: `Core/Bootstrap.php` → session + PDO `$conn` + AuthKernel + CSRF
- Admin modules: 403 gate
- POS/Return/ProductAdd mutations: CSRF verify
- স্টক: negative prevent

## $current ভ্যালু

dashboard · productadd · itemslist · pos · saleshistory · return · outofstock · category · admincontrol · adminreturn · dailyactivity · unsold

---
SADA KALO FASHION · Inventory MVC v2.1
