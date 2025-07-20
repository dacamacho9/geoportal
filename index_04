<script>
  const map = L.map('map').setView([-0.22985, -78.52495], 14);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  const url = "https://wtuhbbmapoindpuaywxo.supabase.co";
  const key = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Ind0dWhiYm1hcG9pbmRwdWF5d3hvIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTI3Mjc5NTEsImV4cCI6MjA2ODMwMzk1MX0.1vH0zSrQ5EHHz1p_SS3gtzyDGTBpzHBOjq5ART7Uc8k";

  const capas = {};

  async function toggleLayer(checkbox) {
    const nombre = checkbox.value;

    if (checkbox.checked) {
      try {
        const res = await fetch(`${url}/rest/v1/${nombre}?select=*&limit=10000`, {
          headers: {
            apikey: key,
            Authorization: `Bearer ${key}`
          }
        });

        if (!res.ok) {
          alert(`❌ Error al consultar la capa ${nombre}: ${res.statusText}`);
          return;
        }

        const datos = await res.json();

        const geojson = {
          type: "FeatureCollection",
          features: datos.map(f => {
            const { geom, ...props } = f;
            return {
              type: "Feature",
              properties: props,
              geometry: geom
            };
          })
        };

        capas[nombre] = L.geoJSON(geojson, {
          onEachFeature: (f, layer) => {
            let contenido = "<strong>Información:</strong><br>";
            for (const [key, value] of Object.entries(f.properties)) {
              contenido += `<strong>${key}</strong>: ${value}<br>`;
            }
            layer.bindPopup(contenido);
          }
        }).addTo(map);

        if (capas[nombre].getLayers().length > 0) {
          map.fitBounds(capas[nombre].getBounds());
        } else {
          alert(`⚠️ La capa ${nombre} no contiene geometrías visibles.`);
        }
      } catch (err) {
        console.error("Error en toggleLayer:", err);
        alert("❌ Error inesperado al cargar la capa.");
      }
    } else {
      if (capas[nombre]) {
        map.removeLayer(capas[nombre]);
        delete capas[nombre];
      }
    }
  }
</script>
