# sarafanola1-sudo.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario WIN</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 500px;
            margin: auto;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        .logo {
            text-align: center;
            margin-bottom: 15px;
        }
        .logo img {
            width: 150px;
        }
        h2 {
            text-align: center;
            color: #ff6b00;
        }
        label {
            font-weight: bold;
            margin-top: 10px;
            display: block;
        }
        input, select, textarea, button {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border-radius: 5px;
            border: 1px solid #ccc;
            font-size: 14px;
        }
        .checkbox-group {
            display: flex;
            flex-direction: column;
            margin-top: 10px;
        }
        .checkbox-group label {
            font-weight: normal;
        }
        button {
            background-color: #ff6b00;
            color: #fff;
            border: none;
            cursor: pointer;
            font-weight: bold;
        }
        button:hover {
            background-color: #e65c00;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">
            <img src="https://www.win.pe/assets/img/logo.png" alt="WIN Logo">
        </div>
        <h2>Registro para Instalación WIN</h2>
        <form id="winForm">
            <label for="nombre">Nombre completo:</label>
            <input type="text" id="nombre" name="nombre" required>

            <label for="dni">Número de DNI:</label>
            <input type="text" id="dni" name="dni" pattern="[0-9]{8}" title="Debe tener 8 dígitos" required>

            <label for="correo">Correo electrónico:</label>
            <input type="email" id="correo" name="correo" required>

            <label for="celular">Número de celular:</label>
            <input type="text" id="celular" name="celular" pattern="[0-9]{9}" title="Debe tener 9 dígitos" required>

            <label for="direccion">Dirección exacta:</label>
            <textarea id="direccion" name="direccion" rows="3" required></textarea>

            <label for="ubicacion">Ubicación:</label>
            <select id="ubicacion" name="ubicacion" required>
                <option value="">Selecciona tu ubicación</option>
                <option value="Lima">Lima</option>
                <option value="Provincia de Lima">Provincia de Lima</option>
                <option value="Provincia">Provincia</option>
            </select>

            <div id="subUbicacion" class="hidden">
                <label for="detalleUbicacion">Selecciona tu ciudad:</label>
                <select id="detalleUbicacion" name="detalleUbicacion">
                    <option value="">Selecciona una opción</option>
                </select>
            </div>

            <label for="plan">¿Qué plan buscas?</label>
            <select id="plan" name="plan" required>
                <option value="">Selecciona un plan</option>
                <option value="Solo Internet">Solo Internet</option>
                <option value="Internet + Cable">Internet + Cable</option>
            </select>

            <label>¿Desea adicionales?</label>
            <div class="checkbox-group">
                <label><input type="checkbox" name="adicional" value="Teléfono"> Teléfono</label>
                <label><input type="checkbox" name="adicional" value="Mesh"> Mesh</label>
                <label><input type="checkbox" name="adicional" value="IPTV"> IPTV</label>
                <label><input type="checkbox" name="adicional" value="Winbox"> Winbox</label>
            </div>

            <button type="submit">Enviar por WhatsApp</button>
        </form>
    </div>

    <script>
        const ubicacion = document.getElementById("ubicacion");
        const subUbicacionDiv = document.getElementById("subUbicacion");
        const detalleUbicacion = document.getElementById("detalleUbicacion");

        ubicacion.addEventListener("change", function() {
            const valor = ubicacion.value;
            detalleUbicacion.innerHTML = '<option value="">Selecciona una opción</option>';
            
            if (valor === "Provincia de Lima") {
                subUbicacionDiv.classList.remove("hidden");
                ["Barranca", "Huacho", "Hualmay", "Huaral"].forEach(ciudad => {
                    const option = document.createElement("option");
                    option.value = ciudad;
                    option.textContent = ciudad;
                    detalleUbicacion.appendChild(option);
                });
            } else if (valor === "Provincia") {
                subUbicacionDiv.classList.remove("hidden");
                ["Arequipa", "Chiclayo", "Chimbote", "Cusco", "Huancayo", "Piura", "Trujillo"].forEach(ciudad => {
                    const option = document.createElement("option");
                    option.value = ciudad;
                    option.textContent = ciudad;
                    detalleUbicacion.appendChild(option);
                });
            } else {
                subUbicacionDiv.classList.add("hidden");
            }
        });

        document.getElementById("winForm").addEventListener("submit", function(event){
            event.preventDefault();
            const nombre = document.getElementById("nombre").value;
            const dni = document.getElementById("dni").value;
            const correo = document.getElementById("correo").value;
            const celular = document.getElementById("celular").value;
            const direccion = document.getElementById("direccion").value;
            const ubicacionVal = ubicacion.value;
            const subUbicacionVal = detalleUbicacion.value ? ` - ${detalleUbicacion.value}` : "";
            const plan = document.getElementById("plan").value;

            const adicionales = Array.from(document.querySelectorAll('input[name="adicional"]:checked'))
                                    .map(el => el.value)
                                    .join(", ") || "Ninguno";

            const mensaje = `📋 *Solicitud de Instalación WIN*%0A%0A` +
                            `👤 Nombre: ${nombre}%0A` +
                            `🆔 DNI: ${dni}%0A` +
                            `📧 Correo: ${correo}%0A` +
                            `📱 Celular: ${celular}%0A` +
                            `🏠 Dirección: ${direccion}%0A` +
                            `📍 Ubicación: ${ubicacionVal}${subUbicacionVal}%0A` +
                            `📦 Plan: ${plan}%0A` +
                            `➕ Adicionales: ${adicionales}`;

            window.open(`https://wa.me/51993254668?text=${mensaje}`, "_blank");
        });
    </script>
</body>
</html>
