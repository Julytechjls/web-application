Salary Calculator

This program saves as a salary calculator.

Step 1: Set Up Your Environment


1. **Install Python**: Make sure Python is installed on your machine. You can download it from [python.org](https://www.python.org/).

2. **Install Flask**: Use pip to install Flask.
   
3.** pip install Flask


Step 2: Create the Project Structure


Create a new directory for your project and navigate into it. Within this directory, create the following structure:

`
salary_calculator/
│
├── app.py
├── templates/
│   └── form.html
├── result.html


 Step 3: Create the Flask Application

Create a file named `app.py` and add the following code to set up a basic Flask application:

from flask import Flask, render_template, request
from clases import Empleado, Gerente
 
app = Flask(__name__)
 
# Ruta para el formulario de entrada de datos
@app.route('/')
def formulario():
    return render_template('formulario.html')
 
# Ruta para procesar los datos del formulario
@app.route('/calcular', methods=['POST'])
def calcular():
    nombre = request.form['nombre']
    edad = int(request.form['edad'])
    salario_base = float(request.form['salario_base'])
    tipo_empleado = request.form['tipo_empleado']
 
    if tipo_empleado == 'gerente':
        bono = float(request.form['bono'])
        empleado = Gerente(nombre, edad, salario_base, bono)
    else:
        empleado = Empleado(nombre, edad, salario_base)
 
    salario_mensual = empleado.calcular_salario_mensual()
    return render_template('resultado.html', nombre=nombre, salario=salario_mensual)
 
if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')



### Step 4: Create the HTML Template

Create a file named `form.html` in the `templates` directory with the following content:

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Salario</title>
</head>
<body>
    <h1>Calculadora de Salario</h1>
    <form action="/calcular" method="post">
        <label for="nombre">Nombre:</label><br>
        <input type="text" id="nombre" name="nombre"><br>
        <label for="edad">Edad:</label><br>
        <input type="number" id="edad" name="edad"><br>
        <label for="salario_base">Salario Base:</label><br>
        <input type="number" id="salario_base" name="salario_base"><br>
        <label for="tipo_empleado">Tipo de Empleado:</label><br>
        <select id="tipo_empleado" name="tipo_empleado">
            <option value="empleado">Empleado</option>
            <option value="gerente">Gerente</option>
        </select><br>
        <label for="bono">Bono (solo para gerentes):</label><br>
        <input type="number" id="bono" name="bono" step="any" disabled><br>
        <input type="submit" value="Calcular">
    </form>
    <script>
        document.getElementById('tipo_empleado').addEventListener('change', function() {
            var bonoInput = document.getElementById('bono');
            if (this.value === 'gerente') {
                bonoInput.disabled = false;
            } else {
                bonoInput.disabled = true;
            }
        });
    </script>
</body>
</html>


Create a file named `result.html` in the `templates` directory with the following content:

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Resultado</title>
</head>
<body>
    <h1>Resultado</h1>
    <p>El salario de {{ nombre }} es: ${{ salario }}</p>
    <a href="/">Volver al formulario</a>
</body>
</html>



### Step 5: Create a file named ´clases.py´ same directory with app.py and add this content: 

class Empleado:
    def __init__(self, nombre, edad, salario_anual):
        self._nombre = nombre
        self._edad = edad
        self._salario_anual = salario_anual
 
    @property
    def nombre(self):
        return self._nombre
 
    @nombre.setter
    def nombre(self, nuevo_nombre):
        self._nombre = nuevo_nombre
 
    @property
    def edad(self):
        return self._edad
 
    @edad.setter
    def edad(self, nueva_edad):
        self._edad = nueva_edad
 
    @property
    def salario_anual(self):
        return self._salario_anual
 
    @salario_anual.setter
    def salario_anual(self, nuevo_salario):
        self._salario_anual = nuevo_salario
 
    def calcular_salario_mensual(self):
        return self._salario_anual / 12
 
class Gerente(Empleado):
    def __init__(self, nombre, edad, salario_anual, bono):
        super().__init__(nombre, edad, salario_anual)
        self._bono = bono
 
    @property
    def bono(self):
        return self._bono
 
    @bono.setter
    def bono(self, nuevo_bono):
        self._bono = nuevo_bono
 
    def calcular_salario_mensual(self):
        salario_base_mensual = super().calcular_salario_mensual()
        return salario_base_mensual + self._bono / 12


### Step 6: Run the Application

Navigate to your project directory and run the Flask application:

python app.py


Your application should now be running at `http://(ip):5000/`. Open this URL in your web browser to see the salary calculator interface.

### Step 7: Testing

Test the application by entering different hourly rates and hours worked to ensure the salary calculation works correctly. Validate the error handling by entering non-numeric values.

