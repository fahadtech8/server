Wordpress, PHP project, aur Landing Page ke liye complete setup aur steps neeche diye gaye hain. Isme aapke request ke mutabiq Multiple Licenses ka logic bhi add kar diya gaya hai taaki aap ek hi file se saari sites manage kar sakein.

1. GitHub Setup (controller.json)
Sabse pehle GitHub par apni repository mein controller.json banayein aur usme multiple licenses ka array rakhein.

Format:



JSON


{
  "license_keys": ["MY-SECURE", "FAHAD-786", "WP-KEY-99"],
  "title": "Invalid License",
  "message": "Aapka license mismatch detected. Please admin se contact karein."
}




2. PHP Project Setup (PlayTube ya Core PHP)
Ise apne PHP project ki sabse main file (jaise index.php) mein sabse upar paste karein.

Code:

PHP




<?php
// --- ENCRYPTED MULTI-LICENSE SECURITY START ---
function _verify_sys_access() {
    // 1. GitHub URL (Encrypted)
    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    
    // 2. Local License Key (Encrypted: "MY-SECURE")
    $_k = base64_decode("TVktU0VDVVJF"); 

    $_o = array("http" => array("header" => "User-Agent: PT-Shield\r\n", "timeout" => 7));
    $_r = @file_get_contents($_u . '?v=' . time(), false, stream_context_create($_o));

    if ($_r === FALSE) {
        die("<div style='background:#000;color:#fff;height:100vh;display:flex;align-items:center;justify-content:center;font-family:sans-serif;'>
            <div style='text-align:center;'><h1>Security Check</h1><p>Connecting...</p></div>
        </div>");
    }

    $_d = json_decode($_r, true);

    // Multi-License Logic
    if (!isset($_d['license_keys']) || !in_array($_k, $_d['license_keys'])) {
        $_t = $_d['title'] ?? "Invalid License";
        $_m = $_d['message'] ?? "License mismatch detected.";

        die("<div style='position:fixed;inset:0;background:rgba(0,0,0,0.98);color:white;display:flex;align-items:center;justify-content:center;text-align:center;font-family:sans-serif;z-index:9999999;'>
            <div style='padding:40px;border-radius:20px;background:#000;border:1px solid #333;box-shadow:0 10px 50px rgba(255,0,0,0.3);max-width:400px;'>
                <div style='width:50px;height:50px;background:#ff3333;border-radius:50%;margin:0 auto 20px;'></div>
                <h1 style='font-size:24px;color:#ff3333;margin:0;'>$_t</h1>
                <p style='font-size:15px;color:#888;margin-top:15px;line-height:1.6;'>$_m</p>
            </div>
        </div>");
    }
}
_verify_sys_access();
?>


3. Wordpress Setup
Ise apne Wordpress Theme ki functions.php file mein sabse niche paste karein.


Code:

PHP

<?php
add_action('init', 'wp_remote_shield_lock');
function wp_remote_shield_lock() {
    if (is_admin()) return; // Admin dashboard access rahega

    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    $_k = base64_decode("TVktU0VDVVJF"); // Change key per site

    $response = wp_remote_get($_u . '?v=' . time());
    if (is_wp_error($response)) return;

    $data = json_decode(wp_remote_retrieve_body($response), true);

    if (!isset($data['license_keys']) || !in_array($_k, $data['license_keys'])) {
        wp_die("<h1 style='color:red;'>".$data['title']."</h1><p>".$data['message']."</p>");
    }
}
4. Landing Page Setup (HTML/JS)
Ise apne landing page ki file mein </body> se theek pehle paste karein.

Code:

HTML

<div id="secure_layer" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.9); z-index:9999999; color:white; align-items:center; justify-content:center; text-align:center;">
    <div style="padding:40px; background:#000; border:1px solid #444; border-radius:20px; max-width:400px;">
        <h1 id="t_header" style="color:#ff3333;"></h1>
        <p id="t_body" style="color:#bbb;"></p>
    </div>
</div>

<script>
(function() {
    const _0x_u = "aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u";
    const _0x_lk = "TVktU0VDVVJF"; 

    async function checkLicense() {
        const layer = document.getElementById('secure_layer');
        const scene = document.querySelector('.scene');
        try {
            const r = await fetch(atob(_0x_u) + '?v=' + Date.now());
            const d = await r.json();
            const isValid = d.license_keys && d.license_keys.includes(atob(_0x_lk));

            if (!isValid) {
                document.getElementById('t_header').innerText = d.title;
                document.getElementById('t_body').innerText = d.message;
                layer.style.display = 'flex';
                if(scene) scene.style.display = 'none';
            } else {
                layer.style.display = 'none';
                if(scene) scene.style.display = 'flex';
            }
        } catch (e) {
            console.log("Server error");
        }
    }
    checkLicense();
    setInterval(checkLicense, 10000); 
})();
</script>


# Kahan add karna hai (Steps):
GitHub: controller.json banayein aur Raw link copy karein.

PHP Site: index.php ki Line 2 par code paste karein (theek <?php ke niche).

Wordpress: Theme Editor mein ja kar functions.php ke bilkul end mein add karein.

Landing Page: HTML file ke end mein </body> se theek pehle paste karein.
