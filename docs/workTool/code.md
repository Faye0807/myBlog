<html>
<style>
    .mac {
        width:10px;
        height:10px;
        border-radius:5px;
        float:left;
        margin:10px 0 0 5px;
    }
    .b1 {
        background:#E0443E;
        margin-left: 10px;
    }
    .b2 { background:#DEA123; }
    .b3 { background:#1AAB29; }
    .warpper{
        background:#121212;
        border-radius:5px;
        /* width:400px; */
    }
</style>
<div class="warpper">
    <div class="mac b1"></div>
    <div class="mac b2"></div>
    <div class="mac b3"></div>
<div>
<br>
</html>

```
├── models
│   ├── common
│   ├── commonResult
│   ├── flexAccess
│   │   └── home
│   ├── steadyWealth
│   │   ├── holdShares
│   │   └── productDetail
│   └── trade
│       ├── redeem
│       └── subscribe
├── pages
│   ├── commonResult
│   ├── plus
│   │   ├── holdShares
│   │   └── home
│   ├── steadyWealth
│   │   ├── holdShares
│   │   ├── productDetail
└── └── trade
        ├── redeem
        └── subscribe



 ```

旧版
 ```
├── models
│   ├── common
│   ├── flexAccess
│   │   ├── commonResult
│   │   ├── home
│   │   ├── redeem
│   │   └── subscribe
│   └── steadyWealth
│       ├── holdShares
│       ├── productDetail
├── pages
│   ├── openAccount
│   ├── plus
│   │   ├── commonResult
│   │   ├── home
│   │   ├── redeem
│   │   ├── subscribe
└── └── steadyWealth
        ├── holdShares
        └── productDetail
```