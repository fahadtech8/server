1. GitHub Configuration (controller.json)
Sabse pehle GitHub par apni file ko is format mein update karein. Isme har license key ke samne uski expiry date (YYYY-MM-DD) likhein.

JSON


{
  "licenses": {
    "MY-SECURE": "2026-12-31",
    "FAHAD-786": "2025-06-01",
    "WP-KEY-99": "2025-01-15"
  },
  "title": "License Alert",
  "msg_expired": "Aapka license expire ho gaya hai. Please renew karein.",
  "msg_invalid": "Invalid License! Please admin se contact karein."
}



2. Landing Page (HTML/JS)
Kahan lagayein: HTML file mein </body> tag se theek pehle paste karein.

HTML




<div id="secure_layer" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.95); z-index:9999999; color:white; align-items:center; justify-content:center; text-align:center; font-family:sans-serif;">
    <div style="padding:40px; background:#000; border:1px solid #444; border-radius:20px; max-width:400px; box-shadow: 0 10px 50px rgba(255,0,0,0.3);">
        <h1 id="t_header" style="color:#ff3333; margin:0;"></h1>
        <p id="t_body" style="color:#bbb; margin-top:15px;"></p>
    </div>
</div>

<script>
(function() {
    const _0x_u = "aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u";
    const _0x_lk = "TVktU0VDVVJF"; 

    async function checkTimeLicense() {
        const layer = document.getElementById('secure_layer');
        const scene = document.querySelector('.scene');
        try {
            const r = await fetch(atob(_0x_u) + '?v=' + Date.now());
            const d = await r.json();
            
            const localKey = atob(_0x_lk);
            const today = new Date().toISOString().split('T')[0];
            const expiryDate = d.licenses[localKey];

            const isExpired = expiryDate && today > expiryDate;
            const isInvalid = !expiryDate;

            if (isInvalid || isExpired) {
                document.getElementById('t_header').innerText = d.title;
                document.getElementById('t_body').innerText = isExpired ? d.msg_expired : d.msg_invalid;
                layer.style.display = 'flex';
                if(scene) scene.style.display = 'none';
            } else {
                layer.style.display = 'none';
                if(scene) scene.style.display = 'flex';
            }
        } catch (e) { console.log("Connection Error"); }
    }
    checkTimeLicense();
    setInterval(checkTimeLicense, 15000); 
})();
</script>





3. PHP Project / Laravel Project
Kahan lagayein: Laravel mein public/index.php mein require __DIR__.'/../vendor/autoload.php'; ke theek niche paste karein. Normal PHP mein index.php ke bilkul top par paste karein.






PHP

<?php
// --- ENCRYPTED TIME-BASED SECURITY START ---
function _verify_sys_with_time() {
    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    $_k = base64_decode("TVktU0VDVVJF"); 

    $_o = array("http" => array("header" => "User-Agent: Security-Shield\r\n", "timeout" => 7));
    $_r = @file_get_contents($_u . '?v=' . time(), false, stream_context_create($_o));

    if ($_r === FALSE) {
        die("<div style='background:#000;color:#fff;height:100vh;display:flex;align-items:center;justify-content:center;font-family:sans-serif;'><h1>Security Check Offline</h1></div>");
    }

    $_d = json_decode($_r, true);
    $today = date("Y-m-d");

    $is_valid = isset($_d['licenses'][$_k]);
    $is_expired = ($is_valid && $today > $_d['licenses'][$_k]);

    if (!$is_valid || $is_expired) {
        $_t = $_d['title'] ?? "Access Denied";
        $_m = $is_expired ? ($_d['msg_expired'] ?? "Expired") : ($_d['msg_invalid'] ?? "Invalid");

        die("<div style='position:fixed;inset:0;background:rgba(0,0,0,0.98);color:white;display:flex;align-items:center;justify-content:center;text-align:center;font-family:sans-serif;z-index:9999999;'>
            <div style='padding:40px;border-radius:20px;background:#000;border:1px solid #333;box-shadow:0 10px 50px rgba(255,0,0,0.3);max-width:400px;'>
                <div style='width:50px;height:50px;background:#ff3333;border-radius:50%;margin:0 auto 20px;'></div>
                <h1 style='font-size:24px;color:#ff3333;margin:0;'>$_t</h1>
                <p style='font-size:15px;color:#888;margin-top:15px;line-height:1.6;'>$_m</p>
            </div>
        </div>");
    }
}
_verify_sys_with_time();
// --- ENCRYPTED SECURITY END ---





4. WordPress Project
Kahan lagayein: Apne WordPress theme ki functions.php file mein sabse niche paste karein.




PHP

<?php
// --- WORDPRESS REMOTE TIME SECURITY ---
add_action('init', 'wp_time_license_shield');
function wp_time_license_shield() {
    if (is_admin()) return;

    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    $_k = base64_decode("TVktU0VDVVJF"); 

    $response = wp_remote_get($_u . '?v=' . time());
    if (is_wp_error($response)) return;

    $data = json_decode(wp_remote_retrieve_body($response), true);
    $today = date("Y-m-d");

    $is_valid = isset($data['licenses'][$_k]);
    $is_expired = ($is_valid && $today > $data['licenses'][$_k]);

    if (!$is_valid || $is_expired) {
        $msg = $is_expired ? $data['msg_expired'] : $data['msg_invalid'];
        wp_die("<h1 style='color:red;'>".$data['title']."</h1><p>$msg</p>", "License Check", array('response' => 403));
    }
}




Aap ye saare codes ek Notepad file mein save kar sakte hain. Jab bhi aapko license manage karna ho, bas GitHub ki JSON file mein changes karein.
