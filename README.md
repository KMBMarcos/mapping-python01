# 🗺️ Visualizador de Mapas con ArcGIS Online

Una herramienta ligera y fácil de usar para visualizar mapas interactivos utilizando la API de ArcGIS Online desde **Python** y **Jupyter Notebook**. Esta aplicación permite explorar y visualizar datos geoespaciales de manera intuitiva y eficiente.

## 📋 Descripción

Esta herramienta proporciona una interfaz simple y moderna para visualizar mapas a través de la API de ArcGIS Online. Permite cargar y visualizar capas de mapas, realizar búsquedas de ubicaciones, y explorar datos geoespaciales de forma interactiva.

## ✨ Características

- 🗺️ Visualización de mapas interactivos
- 🔍 Búsqueda de ubicaciones
- 📍 Marcadores y capas personalizadas
- 🎨 Interfaz de usuario moderna y responsive
- ⚡ Integración con ArcGIS Online API
- 📱 Compatible con dispositivos móviles

## 🚀 Inicio Rápido

### Requisitos Previos

- Python 3.9+ instalado
- Conda Environment
- Jupyter Notebook o JupyterLab
- Conexión a Internet
- (Opcional) Cuenta de ArcGIS Online / ArcGIS Developer para acceder a servicios privados

### Instalación

1. Clona o descarga este repositorio:

```bash
git clone [url-del-repositorio]
cd mapping-python01
```

2. Crea y activa un entorno virtual en conda:

```bash
conda create -n <NAME_ENVIRONMENT> python=3.10
conda activate <NAME_ENVIRONMENT>
```

3. Instala las dependencias:

```bash
 
conda install -c esri arcgis jupyter pandas numpy
```

4. Inicia Jupyter:

```bash
jupyter notebook
# o
jupyter lab
```

5. Abre el cuaderno principal (por ejemplo `notebooks/01_intro_arcgis.ipynb`) desde la interfaz de Jupyter.

## 📖 Uso

1. Abre el notebook que quieras ejecutar en Jupyter (por ejemplo en la carpeta `notebooks/`).
2. Configura tus credenciales (API Key o usuario/contraseña de ArcGIS Online) según se describe en la sección de configuración.
3. Ejecuta las celdas en orden para:
   - **Autenticarte** contra ArcGIS Online
   - **Cargar un mapa base interactivo**
   - **Añadir capas y servicios** (feature layers, mapas web, etc.)
   - **Explorar y consultar datos geoespaciales**

## 🛠️ Tecnologías Utilizadas

- **ArcGIS API for Python** (ArcGIS Maps SDK for Python): Para la conexión con ArcGIS Online y el manejo de datos GIS
- **Jupyter Notebook / JupyterLab**: Entorno interactivo para ejecutar el código y visualizar los mapas
- **Python 3**: Lógica de negocio y procesamiento de datos

## 📁 Estructura del Proyecto

```
mapping-python01/
│
├── requirements.txt            # Dependencias Python del proyecto
├── .env.example                # Variables de entorno de ejemplo (NO subir .env real)
├── config.py                   # Configuración (API key, portal, etc.)
├── notebooks/                  # Cuadernos Jupyter principales
│   ├── 01_intro_arcgis.ipynb   # Introducción y mapa base
│   ├── 02_capas_vectoriales.ipynb
│   └── 03_consultas_y_filtros.ipynb
├── src/                        # (Opcional) módulos Python reutilizables
│   ├── services/               # Clientes/servicios (ArcGIS, geocoding, etc.)
│   └── utils/                  # Utilidades (helpers, validaciones)
└── README.md                   # Este archivo
```

## ⚙️ Configuración

### API Key de ArcGIS Online

Si necesitas usar servicios privados o aumentar los límites de uso, puedes configurar una API Key:

1. Obtén tu API Key desde [ArcGIS Developers](https://developers.arcgis.com/)
2. Configura la clave en el archivo de configuración correspondiente

### Configuración de Cursor IDE y Skills

Este proyecto está pensado para aprovechar las **skills de Cursor IDE** y facilitar el trabajo con GIS desde Python.

- En este repositorio encontrarás una skill en:  
  `.cursor/skills/gis-python-geomatics-geology/SKILL.md`

#### Usar esta skill en Cursor

1. Abre este proyecto con **Cursor**.
2. Asegúrate de que la carpeta `.cursor/skills/` está incluida en el proyecto (ya debería estar en el repositorio).
3. En Cursor, usa el chat del asistente y referencia la skill, por ejemplo:
   - «Usa mi skill GIS definida en `SKILL.md` para ayudarme con este notebook.»
4. (Opcional) Crea tus propias skills duplicando la carpeta existente:
   - Copia `gis-python-geomatics-geology` con otro nombre dentro de `.cursor/skills/`.
   - Ajusta el contenido de `SKILL.md` para tu nuevo caso de uso.

De esta forma, cualquier persona que clone el repositorio podrá reutilizar tu configuración de skills en Cursor o definir las suyas propias.

## 📝 Ejemplos de Uso

### Listado de notebooks de ejemplo

- **Geocodificación y visualización de resultados** (`geocode.ipynb`):  
  Permite introducir direcciones o lugares, geocodificarlos usando ArcGIS Online y representar los puntos resultantes sobre un mapa.
  
- **Visualización básica de mapas** (`map_visualizer.ipynb`):  
  Muestra cómo autenticarse contra ArcGIS Online, cargar un mapa base y visualizarlo de forma interactiva dentro de Jupyter.(EN PROGRESO)

### Autenticación básica en un notebook

```python
from arcgis.gis import GIS

# Uso de API Key
gis = GIS(api_key="TU_API_KEY_AQUI")

# o autenticación con usuario/contraseña
# gis = GIS("https://www.arcgis.com", "usuario", "contraseña")
```

### Cargar un mapa base y mostrarlo en Jupyter

```python
from arcgis.mapping import WebMap
from arcgis.gis import GIS

gis = GIS(api_key="TU_API_KEY_AQUI")

webmap_dict = {
    "baseMap": {
        "baseMapLayers": [{
            "id": "World_Topo_Map",
            "layerType": "ArcGISTiledMapServiceLayer",
            "url": "https://services.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer"
        }],
        "title": "Topographic"
    }
}

webmap = WebMap(webmap_dict)
webmap
```

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo `LICENSE` para más detalles.

## 🔗 Recursos Útiles

- [Documentación de ArcGIS Maps SDK for Python](https://developers.arcgis.com/python/latest/)
- [ArcGIS Online](https://www.arcgis.com/)
- [Guía de Inicio Rápido de ArcGIS](https://developers.arcgis.com/documentation/)

## 📧 Contacto

Para preguntas o sugerencias, por favor abre un issue en el repositorio.

---

**Nota**: Esta herramienta utiliza la API pública de ArcGIS Online. Para uso comercial o de alto volumen, considera obtener una licencia apropiada.
