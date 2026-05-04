import streamlit as st

def convertir_temperatura(valor, unidad_origen, unidad_destino):
    """
    Función para convertir temperaturas entre Celsius, Fahrenheit y Kelvin.
    """
    # Si las unidades son iguales, no hay necesidad de convertir
    if unidad_origen == unidad_destino:
        return valor
    
    # Primero, convertimos la unidad de origen a Celsius para estandarizar
    celsius = 0.0
    if unidad_origen == "Fahrenheit":
        celsius = (valor - 32) * 5.0 / 9.0
    elif unidad_origen == "Kelvin":
        celsius = valor - 273.15
    else: # Si ya es Celsius
        celsius = valor

    # Luego, convertimos de Celsius a la unidad de destino deseada
    if unidad_destino == "Fahrenheit":
        return (celsius * 9.0 / 5.0) + 32
    elif unidad_destino == "Kelvin":
        return celsius + 273.15
    else: # Si el destino es Celsius
        return celsius

# --- Configuración de la página ---
st.set_page_config(page_title="Conversor de Temperatura", page_icon="🌡️", layout="centered")

# --- Interfaz de usuario (UI) ---
st.title("🌡️ Conversor de Temperatura")
st.markdown("Esta aplicación convierte temperaturas entre **Celsius (°C)**, **Fahrenheit (°F)** y **Kelvin (K)**.")

# Crear columnas para organizar los inputs
col1, col2 = st.columns(2)

with col1:
    st.subheader("Origen")
    unidad_origen = st.selectbox(
        "Convertir de:", 
        ["Celsius", "Fahrenheit", "Kelvin"],
        key="origen"
    )
    valor_entrada = st.number_input("Ingresa el valor:", value=0.0, format="%.2f")

with col2:
    st.subheader("Destino")
    unidad_destino = st.selectbox(
        "Convertir a:", 
        ["Celsius", "Fahrenheit", "Kelvin"],
        key="destino"
    )

# Añadir un botón para ejecutar la conversión
st.markdown("---")
if st.button("🔄 Calcular Conversión", use_container_width=True):
    # Validaciones especiales (el cero absoluto)
    es_valido = True
    if unidad_origen == "Celsius" and valor_entrada < -273.15:
        st.error("❌ El valor no puede ser inferior al cero absoluto (-273.15 °C).")
        es_valido = False
    elif unidad_origen == "Fahrenheit" and valor_entrada < -459.67:
        st.error("❌ El valor no puede ser inferior al cero absoluto (-459.67 °F).")
        es_valido = False
    elif unidad_origen == "Kelvin" and valor_entrada < 0:
        st.error("❌ El valor no puede ser inferior al cero absoluto (0 K).")
        es_valido = False

    if es_valido:
        # Calcular el resultado
        resultado = convertir_temperatura(valor_entrada, unidad_origen, unidad_destino)
        
        # Mostrar el resultado de forma destacada
        st.success(f"### Resultado: {valor_entrada:.2f} {unidad_origen} = {resultado:.2f} {unidad_destino}")