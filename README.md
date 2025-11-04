# Proyecto-poligonUS
Sistema Recomendador Básico de Películas
19/09/2025: Se creó un grupo de Whatsapp "Proyecto 3: Recomendador básico de productos o películas" para que todos los integrantes pudieramos hablar del proyecto, dar ideas y entrar a desarrollar el trabajo.
24/09/2025 el compañero Henry proporcionó un dataset que encontró sobre peliculas de diferentes generos, por otro lado el compañero Herminson tambíen presentó otra propuesta que al mirarlas todos los del equipo encontramos que eran muy similares, después de hacer cerificaciones se tomó como base principal continuar con la propuesta de Henry y la documentación desarrollada por Herminson y las compañeras Sandra, Lesly y Kimberly se dejaron como base de explicación de lo que se esta creando.
25/092025: en runión virtual del equipo de definio que una propuesta sería la escogida y que se apoyaria con la información de la otra para generar el pdf ecxplicativo, se repartieron tareas a cada integrante del equipo y nos quedamos de reunir algunos de los integrantes para mirar que ideas nuevas teniamos y que tareas eran necesarias que se hicieran para que se lograra el objetivo.
25/09/2025: se hace el registro en Basecamp de todos los integrantes del equipo y se verifican los chats.
02/10/2025: se llega a un acuerdo para que Henry y Herminson se reunieran a mirar el tema de codigos y hacer pruebaas para encontrar que falla o que hace falta y asignar tareas al equipo.
03/10/2025: Reunión de creación, instalación, verificación y ejecución del recomendador de peliculas, se hicieron intentos en los dos pcs de los compañeros mencionados y se tuvo dificultad para que el streamlit se mantuviera activo cada que se hiciera el ingreso a la página esta se caía después de una consulta, se acabó el tiempo y se retomará luego para corregir, se logró sacar varias tareas que se le entregaran a otros integrantes para que las desarrollen (Se requiere hacer una mejor traducción en el Dataset de películas del inglés al español de la columna title_es, teniendo en cuenta que muchas películas en ingles pueden tener un nombre diferente en español, Agregar una columna que contenga el rating de las películas (modificar para que salgan las mas reconocidas en un top), Revisar la documentación del proyecto y corregir posibles errores, Tener en cuenta en el Dataset que hay películas que pertenecen a varios géneros a la vez, Hacer el montaje en Knime)
28/10/2025: nos ha quedado complicado reunirnos, se escribe correo a los integrantes del equipo entreguemos lo que tenemos pendiente.
31/10/2025: Henry al contar con mayor conocimiento en codigos organizó el error que se nos venía presentando y nos informo que hizo lo siguiente para corregirlo: En la versión inicial  el proyecto fue elaborado para que funcionara localmente con sus archivos alojados en una carpeta dentro del disco C. la base de datos o dataset estaba montado en una hoja electrónica de excel y el código fuente fue programado en python para que funcione 
bajo el navegador web por medio de Streamlit, la estructura del directorio y su funcionamiento se observan en la siguiente imagen. <img width="1447" height="584" alt="image" src="https://github.com/user-attachments/assets/06f615dd-d350-4879-b708-654cfd1e1308" />

En la ejecución del programa se abre en el navegador y permite hacer búsquedas por Nombre, en inglés o español o por uno o varios géneros. al pasar el mouse sobre las sugerencias de las películas se puede observar el rating  promedio otorgado por varios usuarios y a su vez permite hacer clic en en la ventanita de "Ver ficha" para que abra una sinopsis de la película en "https://www.themoviedb.org/movie  " que incluye la posibilidad de ver un pequeño trailer. <img width="1539" height="694" alt="image" src="https://github.com/user-attachments/assets/511679db-2199-470b-bb66-e7e5462a2c29" />

MONTAJE A UN REPOSITORIO DE GITHUB.
Se debe crear una cuenta en GitHub (https://github.com/ )
1. Se sube a un repositorio de GitHub , y se hace la prueba de ejecución pero saca muchos errores por el tamaño del dataset en excel.  muestra de los errores <img width="1680" height="1050" alt="image" src="https://github.com/user-attachments/assets/883e526e-2b58-4f32-84ba-b676ef32a7b5" />
Se soluciona manejando el dataset como  .csv   y  así figura el dataset montado en el repositorio, el archivo requirements.txt es un archivo que hay que crear
y sirve para indicarle a https://share.streamlit.io/  que componentes trae el código del programa principal en este caso  movies.py
<img width="1448" height="626" alt="image" src="https://github.com/user-attachments/assets/9231b824-bef9-4a30-a9e6-d6881d689546" />
CREACION DEL LINK DE EJECUCION DE LA APP CON  STREAMLIT.IO ( https://share.streamlit.io/ )

<img width="1609" height="975" alt="image" src="https://github.com/user-attachments/assets/7fbde949-04ee-407e-bcf0-89f2afbf7937" />

Después de hacer clic en DEPLOY   se genera el link para compartir.  este link abre la app en cualquier navegador.
https://cineapp-imn4hyuhc7fl3kx9pzp4ke.streamlit.app/
adjunto los 3 archivos finales de la app subida a GitHub

**01/11/2025: se generó el código final**

import streamlit as st
import pandas as pd
import requests

# Intentar leer desde GitHub directamente
try:
    csv_url = "https://raw.githubusercontent.com/henrymunoz/cineapp/main/movies_buscador_IN_ESP.csv"
    df = pd.read_csv(csv_url, sep=';', encoding='latin-1')
    st.success("✅ Datos cargados correctamente desde GitHub")
except Exception as e:
    st.error("❌ No se pudo cargar el archivo CSV desde GitHub.")
    st.write("Detalles del error:", e)
    st.stop()


# ===== CONFIG =====
st.set_page_config(page_title="🎬 CineMatch - Recomendador", layout="wide")

# ===== ESTILOS =====
page_bg = """
<style>
    .stApp {
        background-color: #0d47a1;
        color: white;
    }
    label, .stSelectbox label, .stMultiSelect label, .stTextInput label {
        color: white !important;
        font-weight: bold;
    }
    div.stButton > button {
        color: black !important;
        font-weight: bold;
    }
    .centered-message {
        text-align: center;
        font-size: 20px;
        font-weight: bold;
        margin-top: 40px;
        color: white;
    }
    /* Tarjeta de película */
    .movie-card {
        position: relative;
        overflow: hidden;
        border-radius: 12px;
        margin-bottom: 20px;
    }
    .movie-card img {
        border-radius: 12px;
        transition: transform 0.3s ease;
    }
    .movie-card:hover img {
        transform: scale(1.05);
    }
    /* Overlay */
    .overlay {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0,0,0,0.6);
        display: flex;
        align-items: center;
        justify-content: center;
        opacity: 0;
        transition: opacity 0.3s ease;
        border-radius: 12px;
    }
    .movie-card:hover .overlay {
        opacity: 1;
    }
    .overlay a {
        background: #ffcc00;
        padding: 10px 18px;
        border-radius: 8px;
        text-decoration: none;
        color: black;
        font-weight: bold;
        font-size: 14px;
    }
    .overlay a:hover {
        background: #ffb300;
    }
    /* Rating */
    .rating-badge {
        position: absolute;
        bottom: 8px;
        right: 8px;
        padding: 6px 10px;
        border-radius: 10px;
        font-weight: bold;
        font-size: 13px;
        display: flex;
        align-items: center;
        gap: 4px;
        color: white;
    }
    .rating-good {
        background: rgba(0, 150, 0, 0.8);
    }
    .rating-medium {
        background: rgba(255, 170, 0, 0.8);
    }
    .rating-bad {
        background: rgba(200, 0, 0, 0.8);
    }
</style>
"""
st.markdown(page_bg, unsafe_allow_html=True)

# ===== CARGAR DATA =====
#df = pd.read_excel("movies_buscador_IN_ESP.xlsx")  SE ELIMINA BUSQUEDA EN EXCEL
#df = pd.read_csv("movies_buscador_IN_ESP.csv", sep=';', encoding='utf-8', engine='python')
df = pd.read_csv("movies_buscador_IN_ESP.csv", sep=';', encoding='latin-1', engine='python')
df = df.dropna(axis=1, how='all') # Elimina columnas vacias


st.title("🍿 CineMatch - Tu recomendador de películas")
st.markdown("Busca películas por título (inglés o español) o explora por género 🎥")

# Botón refrescar alineado a la derecha
col1, col2 = st.columns([5, 1])
with col2:
    if st.button("🔄 Refrescar búsqueda"):
        st.rerun()

# Buscador
title_search = st.text_input("🔎 Buscar por título (en inglés o español):")

# Filtro por género
all_genres = pd.unique(df[['genres','genres.1','genres.2','genres.3','genres.4']].values.ravel('K'))
all_genres = [g for g in all_genres if pd.notna(g)]
selected_genres = st.multiselect("🎭 Filtrar por género:", sorted(all_genres))

# ===== FILTROS =====
filtered_df = df.copy()

if title_search:
    filtered_df = filtered_df[
        filtered_df['title'].str.contains(title_search, case=False, na=False) |
        filtered_df['title_es'].str.contains(title_search, case=False, na=False)
    ]

if selected_genres:
    filtered_df = filtered_df[
        filtered_df[['genres','genres.1','genres.2','genres.3','genres.4']]
        .apply(lambda row: any(g in row.values for g in selected_genres), axis=1)
    ]

# ===== MOSTRAR RESULTADOS =====
if not title_search and not selected_genres:
    st.markdown('<p class="centered-message">👉 Empieza buscando una película o selecciona un género.</p>', unsafe_allow_html=True)
else:
    if filtered_df.empty:
        st.warning("No se encontraron películas con esos filtros 😢")
    else:
        cols = st.columns(6)

       
        for i, (_, row) in enumerate(filtered_df.iterrows()):
            tmdb_link = row["LINK TMDBLD"]
            tmdb_id = tmdb_link.split("/")[-1] if tmdb_link else None
            rating = row.get("rating", None)

         
            # ===== CLASIFICAR RATING =====
            if pd.isna(rating):
                rating_html = '<div class="rating-badge rating-medium">⭐ N/A</div>'
            elif rating >= 4:
                rating_html = f'<div class="rating-badge rating-good">🟢 {rating:.1f}</div>'
            elif rating >= 2.5:
                rating_html = f'<div class="rating-badge rating-medium">🟡 {rating:.1f}</div>'
            else:
                rating_html = f'<div class="rating-badge rating-bad">🔴 {rating:.1f}</div>'

            # ===== POSTER =====
            poster_url = None
            if tmdb_id:
                try:
                    response = requests.get(
                        f"https://api.themoviedb.org/3/movie/{tmdb_id}",
                        params={"api_key": "9f0bd3de6d61891223e72ec93de05459", "language": "es-ES"}
                    )
                    if response.status_code == 200:
                        data = response.json()
                        poster_path = data.get("poster_path")
                        if poster_path:
                            poster_url = f"https://image.tmdb.org/t/p/w300{poster_path}"
                except:
                    pass

            with cols[i % 6]:
                if poster_url:
                    st.markdown(
                        f"""
                        <div class="movie-card">
                            <img src="{poster_url}" width="100%">
                            {rating_html}
                            <div class="overlay">
                                <a href="{tmdb_link}" target="_blank">Ver ficha</a>
                            </div>
                        </div>
                        """,
                        unsafe_allow_html=True
                    )
                else:
                    st.write("🎞️ Sin póster disponible")

                    

