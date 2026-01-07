# server


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

