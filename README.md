# 🎬 MovieApp — TFC DAM 2025

> Trabajo de Fin de Ciclo — Desarrollo de Aplicaciones Multiplataforma (DAM) · 2025

Aplicación Android nativa para explorar películas populares, construida con **Kotlin**, **Jetpack Compose** y arquitectura **multi-módulo Clean Architecture + MVVM**.

---

## 📖 Descripción

MovieApp consume la API pública de **The Movie Database (TMDB)** para mostrar un catálogo de películas populares con soporte offline. Los datos se almacenan localmente mediante **Room**, de forma que la app sigue siendo funcional aunque no haya conexión a internet. Incluye paginación automática, caché inteligente y gestión de errores.

### Funcionalidades principales

- 🎥 **Listado de películas populares** con scroll infinito y paginación
- 🔍 **Detalle de película** con carátula, descripción, popularidad, fecha de estreno y puntuación
- 💾 **Caché offline** — los datos se guardan en Room y se sirven desde local si la API falla
- 🔄 **Sincronización inteligente** — detecta la última página cargada y evita peticiones redundantes
- 🗑️ **Limpieza de base de datos** con registro de la última vez que se borró
- ✨ **Animaciones Lottie** en las pantallas de carga

---

## 🚀 Estado del proyecto

![Estado](https://img.shields.io/badge/estado-en%20desarrollo-yellow)
![Plataforma](https://img.shields.io/badge/plataforma-Android-green)
![Lenguaje](https://img.shields.io/badge/lenguaje-Kotlin-blueviolet)
![UI](https://img.shields.io/badge/UI-Jetpack%20Compose-blue)
![BD](https://img.shields.io/badge/base%20de%20datos-Room-orange)
![API](https://img.shields.io/badge/API-TMDB-teal)

> ⚠️ El proyecto se encuentra actualmente **en desarrollo**. Algunas funcionalidades pueden estar incompletas o sujetas a cambios.

---

## 🛠️ Tecnologías y librerías

| Tecnología / Librería | Uso |
|---|---|
| **Kotlin** | Lenguaje principal |
| **Jetpack Compose + Material 3** | UI declarativa |
| **Room** | Base de datos local (SQLite) |
| **Retrofit 2 + OkHttp** | Llamadas a la API REST de TMDB |
| **Hilt (Dagger)** | Inyección de dependencias |
| **ViewModel + StateFlow** | Gestión del estado (MVVM) |
| **Coil** | Carga asíncrona de imágenes |
| **Lottie** | Animaciones en pantallas de carga |
| **Gson** | Serialización / deserialización JSON |
| **Secrets Gradle Plugin** | Gestión segura de la API Key |
| **KSP** | Procesador de anotaciones (Room, Hilt) |

---

## 🗂️ Estructura del proyecto

El proyecto sigue una arquitectura **multi-módulo** organizada en capas:

```
MovieApp/
├── app/                              # Módulo principal (entry point)
│   └── src/main/
│       ├── MainActivity.kt
│       ├── MovieDBApplication.kt
│       └── features/home/ui/        # Pantalla Home con navegación
│           ├── FragmentHome.kt
│           ├── HomeViewModel.kt
│           └── MainViewModel.kt
│
├── features/
│   └── movies/
│       ├── domain/                  # Capa de dominio (lógica de negocio)
│       │   ├── model/MovieModel.kt
│       │   ├── repository/MovieRepository.kt
│       │   └── use_case/
│       │       ├── GetMoviesUseCase.kt
│       │       ├── GetDetailMoviesUseCase.kt
│       │       └── DeleteDatabaseUseCase.kt
│       │
│       ├── data/                    # Capa de datos
│       │   ├── database/
│       │   │   ├── MoviesDatabase.kt
│       │   │   ├── dao/MovieDAO.kt
│       │   │   ├── dao/MoviePageDAO.kt
│       │   │   └── entities/MovieEntity.kt
│       │   ├── dataSource/
│       │   │   ├── MoviesLocalDataSource.kt
│       │   │   └── MoviesNetworkDataSource.kt
│       │   ├── dto/MovieDTO.kt
│       │   ├── mapper/MovieMapper.kt
│       │   ├── network/MoviesService.kt
│       │   ├── implementation/MovieRepositoryImpl.kt
│       │   └── di/                  # Módulos Hilt
│       │
│       └── ui/                      # Capa de presentación (Compose)
│           ├── screens/
│           │   ├── MovieListScreen.kt
│           │   ├── MovieDetailScreen.kt
│           │   ├── LoadingFullScreen.kt
│           │   └── LoadingPartialScreen.kt
│           ├── fragment/
│           │   ├── MovieFragment.kt
│           │   └── MovieDetailFragment.kt
│           ├── adapter/MoviesAdapter.kt
│           └── view_model/
│               ├── MovieViewModel.kt
│               └── MovieDetailViewModel.kt
│
└── lib/                             # Módulos de librería compartidos
    ├── common/                      # Utilidades comunes
    ├── common_data/                 # Retrofit, OkHttp, ApiKeyInterceptor
    └── common_ui/                   # BaseFragment, ErrorFragment, Extensions
```

---

## 🏛️ Arquitectura

La app implementa **Clean Architecture** con el patrón **MVVM** y separación en módulos Gradle independientes:

```
UI (Compose Screens / Fragments)
         ↕
    ViewModel  ←── StateFlow
         ↕
    Use Cases  (dominio)
         ↕
    Repository (interfaz)
         ↕
  ┌──────────────────────┐
  │  NetworkDataSource   │  ← Retrofit + TMDB API
  │  LocalDataSource     │  ← Room Database
  └──────────────────────┘
```

La estrategia de datos prioriza Room como fuente de verdad: si hay datos en caché para la página solicitada, se sirven directamente sin llamar a la red. Si la API falla, se devuelven los datos locales disponibles.

---

## ⚙️ Requisitos previos

- **Android Studio** Hedgehog (2023.1.1) o superior
- **JDK 11** o superior
- **Android SDK** API 26 (Android 8.0) mínimo — `targetSdk 35`
- **Cuenta en TMDB** para obtener una API Key → [https://www.themoviedb.org](https://www.themoviedb.org)

---

## 🗄️ Base de datos local (Room)

La base de datos `MoviesDatabase` contiene dos tablas:

| Tabla | Descripción |
|---|---|
| `movies_entities` | Almacena los datos de cada película (id, título, descripción, carátula, popularidad, etc.) |
| `movie_page_entities` | Registra la paginación: última página cargada, total de páginas y timestamp del último borrado |

---

## 📄 Licencia

Este proyecto ha sido desarrollado con fines académicos como **Trabajo de Fin de Ciclo** del ciclo formativo de grado superior en **Desarrollo de Aplicaciones Multiplataforma (DAM)**.

```
© 2025 Hugo Ramilo Marcote — Todos los derechos reservados.
Uso educativo y personal. No se permite la distribución comercial sin autorización.
```

---

## 👤 Autor

**Hugo Ramilo Marcote**

- 🎓 CFGS Desarrollo de Aplicaciones Multiplataforma (DAM)
- 🐙 GitHub: [@HugoRamiloMarcote](https://github.com/HugoRamiloMarcote)

---

<p align="center">
  Desarrollado con ❤️ como TFC · DAM · 2025 · Powered by <a href="https://www.themoviedb.org">TMDB</a>
</p>
