<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, initial-scale=1.0">

  <title>Asistencia Lafayette</title>

  <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 25px;
      background: #f4f4f4;
    }

    h1 {
      margin-bottom: 5px;
    }

    p {
      color: #555;
    }

    button {
      width: 100%;
      max-width: 400px;
      padding: 18px;
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
      border: 0;
      border-radius: 10px;
      background: #111827;
      color: white;
    }

    #reader {
      width: 100%;
      max-width: 500px;
      margin: 25px auto;
    }

    #resultado {
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
    }
  </style>
</head>

<body>

  <h1>Colegio Lafayette</h1>
  <p>Prueba del lector QR</p>

  <button id="boton" onclick="abrirCamara()">
    📷 ABRIR CÁMARA
  </button>

  <div id="reader"></div>

  <div id="resultado"></div>

  <script>
    let lector = null;

    async function abrirCamara() {

      const boton = document.getElementById("boton");

      boton.textContent = "ABRIENDO CÁMARA...";

      try {

        const camaras = await Html5Qrcode.getCameras();

        if (!camaras || camaras.length === 0) {
          throw new Error("No se encontraron cámaras.");
        }

        let camara = camaras[camaras.length - 1].id;

        for (const item of camaras) {

          const nombre =
            String(item.label || "").toLowerCase();

          if (
            nombre.includes("back") ||
            nombre.includes("rear") ||
            nombre.includes("trasera")
          ) {
            camara = item.id;
          }
        }

        lector = new Html5Qrcode("reader");

        await lector.start(
          camara,
          {
            fps: 10,
            qrbox: {
              width: 250,
              height: 250
            }
          },
          codigoLeido,
          function() {}
        );

        boton.textContent = "✓ CÁMARA ACTIVA";
        boton.disabled = true;

      } catch (error) {

        boton.textContent = "📷 ABRIR CÁMARA";

        alert(
          "No se pudo abrir la cámara.\n\n" +
          (error.message || error)
        );
      }
    }


    function codigoLeido(texto) {

      document.getElementById("resultado").innerHTML =
        "✓ QR DETECTADO<br><br>" +
        texto;
    }
  </script>

</body>
</html>
