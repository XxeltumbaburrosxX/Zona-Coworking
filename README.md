<!DOCTYPE html>
<html>
<head>
    <title>Calculadora de Presupuesto</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background-color: #fff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 400px;
        }

        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #34495e;
        }

        select, input, button {
            width: calc(100% - 22px);
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            box-sizing: border-box;
            font-size: 16px;
        }

        button {
            background-color: #f39c12;
            color: white;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #e67e22;
        }

        #resultado {
            margin-top: 20px;
            padding: 15px;
            background-color: #ecf0f1;
            border-radius: 5px;
            color: #333;
            font-weight: bold;
            text-align: center;
        }

        select {
            appearance: none;
            background-image: url('data:image/svg+xml;utf8,<svg fill="black" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 4 5"><path fill="currentColor" d="M2 0L0 2h4zm0 5L0 3h4z"/></svg>');
            background-repeat: no-repeat;
            background-position-x: 95%;
            background-position-y: center;
            background-size: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculadora de Presupuesto</h1>

        <label for="espacio">Seleccione el espacio:</label>
        <select id="espacio">
            <option value="salon_pequeno">Salón Pequeño (10 USD/hora)</option>
            <option value="salon_grande">Salón Grande (20 USD/hora)</option>
        </select>

        <label for="horas">Cantidad de horas:</label>
        <input type="number" id="horas" value="1">

        <button onclick="calcularPresupuesto()">Calcular Presupuesto</button>

        <div id="resultado"></div>
    </div>

    <script>
        async function calcularPresupuesto() {
            const espacio = document.getElementById("espacio").value;
            const horas = parseInt(document.getElementById("horas").value);

            let precioUSD = espacio === "salon_pequeno" ? 10 * horas : 20 * horas;

            const tasaCambio = await obtenerTasaCambio();
            const precioBS = precioUSD * tasaCambio;

            document.getElementById("resultado").innerHTML = `
                Precio en USD: ${precioUSD.toFixed(2)} USD<br>
                Precio en BS: ${precioBS.toFixed(2)} BS
            `;
        }

        async function obtenerTasaCambio() {
            try {
                const response = await fetch("https://api.exchangerate-api.com/v4/latest/USD");
                const data = await response.json();
                return data.rates.VES;
            } catch (error) {
                console.error("Error al obtener la tasa de cambio:", error);
                return 0;
            }
        }
    </script>
</body>
</html>
