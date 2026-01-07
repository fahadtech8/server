#  for landing page 


# start controller.json 

{
  "license_key": "MY-SECURE",
  "title": "Invalid License",
  "message": "Aapka license key match nahi kar raha. Please admin se contact karein."
}

# end controller.json 


# start form landing 

<script>
(function() {
    // 1. GitHub Raw URL (Encrypted)
    const _0x_u = "aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u";
    
    // 2. Local License Key (Iska matlab hai "MY-SECURE")
    const _0x_lk = "TVktU0VDVVJF"; 

    async function checkLicense() {
        const layer = document.getElementById('secure_layer');
        const scene = document.querySelector('.scene');

        try {
            const r = await fetch(atob(_0x_u) + '?v=' + Date.now());
            if (!r.ok) throw new Error();
            
            const d = await r.json();

            // Automatic Logic: Agar keys match nahi karti to True (Lock), agar karti hain to False (Unlock)
            const isMismatch = (d.license_key !== atob(_0x_lk));

            if (isMismatch) {
                // Agar license alag alag hain
                document.getElementById('t_header').innerText = d.title || "License Error";
                document.getElementById('t_body').innerText = d.message || "License mismatch detected.";
                
                layer.style.display = 'flex';
                if(scene) scene.style.display = 'none';
            } else {
                // Agar license dono taraf sahi hai
                layer.style.display = 'none';
                if(scene) scene.style.display = 'flex';
            }
        } catch (err) {
            // Agar internet band hai ya file nahi mili
            layer.style.display = 'flex';
            document.getElementById('t_header').innerText = "Security Check";
            document.getElementById('t_body').innerText = "Connecting to license server...";
        }
    }

    checkLicense();
    setInterval(checkLicense, 10000); 
})();
</script>


# End form landing 



# start for  php page 

<?php
// --- ENCRYPTED REMOTE SECURITY START ---
function _verify_sys_access() {
    // 1. GitHub URL (Encrypted) - "https://fahadtech8.github.io/server/controller.json"
    $_u = base64_decode("aHR0cHM6Ly9mYWhhZHRlY2g4LmdpdGh1Yi5pby9zZXJ2ZXIvY29udHJvbGxlci5qc29u");
    
    // 2. Local License Key (Encrypted) - "MY-SECURE"
    $_k = base64_decode("TVktU0VDVVJF"); 

    // 3. Encrypted System Phrases
    $_e1 = base64_decode("U2VjdXJpdHkgQ2hlY2s="); // Security Check
    $_e2 = base64_decode("Q29ubmVjdGluZyB0byBsaWNlbnNlIHNlcnZlci4uLg=="); // Connecting to license server...

    $_o = array("http" => array("header" => "User-Agent: PT-Shield\r\n", "timeout" => 7));
    $_r = @file_get_contents($_u . '?v=' . time(), false, stream_context_create($_o));

    if ($_r === FALSE) {
        die("<div style='background:#000;color:#fff;height:100vh;display:flex;align-items:center;justify-content:center;font-family:sans-serif;'>
            <div style='text-align:center;'><h1>$_e1</h1><p>$_e2</p></div>
        </div>");
    }

    $_d = json_decode($_r, true);

    // Automatic Logic Check
    if (!isset($_d['license_key']) || $_d['license_key'] !== $_k) {
        $_t = $_d['title'] ?? base64_decode("SW52YWxpZCBMaWNlbnNl"); // Invalid License
        $_m = $_d['message'] ?? base64_decode("TGljZW5zZSBtaXNtYXRjaCBkZXRlY3RlZC4="); // License mismatch detected.

        die("<div style='position:fixed;inset:0;background:rgba(0,0,0,0.98);color:white;display:flex;align-items:center;justify-content:center;text-align:center;font-family:sans-serif;z-index:9999999;'>
            <div style='padding:40px;border-radius:20px;background:#000;border:1px solid #333;box-shadow:0 10px 50px rgba(255,0,0,0.3);max-width:400px;'>
                <div style='width:50px;height:50px;background:#ff3333;border-radius:50%;margin:0 auto 20px;box-shadow:0 0 15px #f00;'></div>
                <h1 style='font-size:24px;color:#ff3333;margin:0;'>$_t</h1>
                <p style='font-size:15px;color:#888;margin-top:15px;line-height:1.6;'>$_m</p>
            </div>
        </div>");
    }
}
_verify_sys_access();
// --- ENCRYPTED REMOTE SECURITY END ---


# end for  php page 
