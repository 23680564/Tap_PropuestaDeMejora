# Tap Propuesta De Mejora

## Descripción General

Se implementó una mejora al sistema POS_TAP desarrollado con Flet para incorporar un control de acceso por roles y una vista exclusiva para clientes. Esta mejora permite que cada tipo de usuario acceda únicamente a las funcionalidades que le corresponden.

El sistema ahora distingue entre:

- Administrador
- Servidor
- Mesero
- Cliente

### Objetivo de la Mejora

El objetivo principal fue simular un entorno real de restaurante, donde:

- El personal interno puede administrar ventas, gastos e historial.
- El cliente puede consultar el menú digital con imágenes y precios.
- Se evita que el cliente interactúe con funciones administrativas.


### Funcionalidades Implementadas

1. Pantalla Inicial de Selección de Rol

Al ejecutar la aplicación, se muestra una pantalla de inicio con cuatro opciones:

- Administrador
- Servidor
- Mesero
- Cliente 

Dependiendo de la opción seleccionada, el sistema carga una interfaz diferente.

### 2. Acceso Completo para Personal Interno

Los roles:

- Administrador
- Servidor
- Mesero

comparten acceso al sistema completo, incluyendo los siguientes módulos:

- Ventas
- Gastos
- Dashboard
- Historial
- Cerrar Día

### 3. Menú Digital para Clientes

Se desarrolló una vista independiente llamada `MenuClienteView`, diseñada exclusivamente para clientes.

Características:
- Muestra los platillos disponibles.
- Presenta imágenes de cada platillo.
- Muestra el precio.
- No permite agregar, editar ni eliminar productos.
- No muestra la orden actual ni el total.

Esta vista es únicamente visual.

### 4. Integración de Imágenes

Se agregó una carpeta `assets/` en la raíz del proyecto para almacenar las imágenes de los platillos.

Ejemplo de archivos:

`mole_poblano.jpg`

`enchiladas_verdes.jpg`

`chilaquiles_rojos.jpg`

`pozole_rojo.png`

`tlayuda_oaxaquena.jpeg`

`papa_asada.jpg`

`picada_verde.jpeg`

En `main.py` se configuró:

`page.assets_dir = "assets"`

### 5. Botón de Cerrar Sesión

Se añadió un botón en la interfaz principal que permite regresar a la pantalla de selección de roles sin cerrar la aplicación.

### Estructura del proyecto

```
POS_TAP2/
│
├── main.py
│
├── core/
│   └── data_manager.py
│
├── views/
│   ├── ventas_view.py
│   ├── gastos_view.py
│   ├── dashboard_view.py
│   ├── historial_view.py
│   ├── cierre_dia_view.py
│   └── menu_cliente_view.py
│
└── assets/
    ├── mole_poblano.jpg
    ├── enchiladas_verdes.jpg
    ├── chilaquiles_rojos.jpg
    ├── pozole_rojo.jpg
    ├── tlayuda_oaxaquena.jpg
    ├── papa_asada.jpg
    └── picada_verde.jpg

```



### Archivos Modificados

`main.py`

Se modificó para:

- Mostrar la pantalla inicial de selección de rol.
- Cargar el sistema completo para personal interno.
- Cargar el menú digital para clientes.
- Configurar la carpeta de recursos (assets).
- Incorporar el botón de cerrar sesión.

`views/menu_cliente_view.py`

**Nuevo archivo que contiene la vista visual del menú para clientes.**

### Flujo de Uso
- El usuario ejecuta `main.py.`
- Se muestra la pantalla de selección de rol.
- #### Si selecciona:
- Administrador, Servidor o Mesero → se abre el sistema completo.
- Cliente → se abre el menú digital.
El cliente consulta los platillos y comunica su pedido al mesero.


### Platillos de Ejemplo

El menú digital incluye los siguientes productos:

Platillo	           Precio

Mole Poblano	       $45.00

Enchiladas Verdes	   $35.00

Chilaquiles Rojos     $30.00

Pozole Rojo   	      $50.00

Tlayuda Oaxaqueña   	$55.00

Papa Asada  	        $100.00

Picada Verde   	$25.00

### Tecnologías Utilizadas
- Python
- Flet
- Arquitectura basada en vistas
- Manejo de recursos estáticos (imágenes)

### Beneficios de la Implementación
- Control de acceso por roles.
- Separación entre funciones administrativas y consulta del cliente.
- Interfaz visual más atractiva.
- Mejora de la experiencia del usuario.
- Código modular y escalable.

### Posibles Mejoras Futuras
- Autenticación con usuario y contraseña.
- Generación automática de códigos QR por mesa.
- Realización de pedidos desde el dispositivo del cliente.
- Integración con base de datos.
- Gestión de disponibilidad de platillos.

  ### Codigos actualizados e implementados
  
  `main.py`

  `menu_clientes_view.py`
  
  ### MAIN

```
import flet as ft
from flet.controls.material.icons import Icons

from core.data_manager import DataManager
from views.ventas_view import VentasView
from views.gastos_view import GastosView
from views.dashboard_view import DashboardView
from views.historial_view import HistorialView
from views.cierre_dia_view import CierreDiaView
from views.menu_cliente_view import MenuClienteView


def main(page: ft.Page):
    # Configuración general
    page.title = "POS_TAP - Taller Flet"
    page.theme_mode = ft.ThemeMode.DARK
    page.bgcolor = "#0f172a"
    page.padding = 0
    page.assets_dir = "assets"
    page.window.width = 1200
    page.window.height = 800

    # Instancia del manejador de datos
    dm = DataManager()

    # =========================================================
    # INTERFAZ PRINCIPAL (Administrador / Servidor / Mesero)
    # =========================================================
    def mostrar_sistema_completo():
        page.clean()

        content_area = ft.Container(
            expand=True,
            bgcolor="#0f172a"
        )

        def change_route(e):
            idx = e.control.selected_index
            content_area.content = None

            if idx == 0:
                content_area.content = VentasView(page, dm)
            elif idx == 1:
                content_area.content = GastosView(page, dm)
            elif idx == 2:
                content_area.content = DashboardView(page, dm)
            elif idx == 3:
                content_area.content = HistorialView(page, dm)
            elif idx == 4:
                content_area.content = CierreDiaView(page, dm)

            page.update()

        # Barra lateral de navegación
        sidebar = ft.NavigationRail(
            selected_index=0,
            label_type=ft.NavigationRailLabelType.ALL,
            min_width=100,
            bgcolor="#1e293b",
            on_change=change_route,
            destinations=[
                ft.NavigationRailDestination(
                    icon=Icons.SHOPPING_CART,
                    label="Ventas"
                ),
                ft.NavigationRailDestination(
                    icon=Icons.PAYMENT,
                    label="Gastos"
                ),
                ft.NavigationRailDestination(
                    icon=Icons.ANALYTICS,
                    label="Dashboard"
                ),
                ft.NavigationRailDestination(
                    icon=Icons.HISTORY,
                    label="Historial"
                ),
                ft.NavigationRailDestination(
                    icon=Icons.NIGHTLIGHT,
                    label="Cerrar Día"
                ),
            ]
        )

        # Vista inicial
        content_area.content = VentasView(page, dm)

        # Botón para cerrar sesión
        logout_button = ft.IconButton(
            icon=Icons.LOGOUT,
            icon_color="white",
            tooltip="Cerrar sesión",
            on_click=lambda e: mostrar_login()
        )

        # Interfaz principal
        page.add(
            ft.Column(
                [
                    # Encabezado superior
                    ft.Container(
                        content=ft.Row(
                            [
                                ft.Text(
                                    "POS_TAP",
                                    size=24,
                                    weight=ft.FontWeight.BOLD,
                                    color="white"
                                ),
                                ft.Container(expand=True),
                                logout_button
                            ]
                        ),
                        padding=20,
                        bgcolor="#1e293b"
                    ),

                    # Área principal
                    ft.Row(
                        [
                            sidebar,
                            ft.VerticalDivider(
                                width=1,
                                color="#334155"
                            ),
                            content_area
                        ],
                        expand=True
                    )
                ],
                expand=True
            )
        )

        page.update()

    # =========================================================
    # MENÚ SOLO VISUAL PARA CLIENTE
    # =========================================================
    def mostrar_menu_cliente():
        page.clean()

        page.add(
            ft.Column(
                [
                    # Encabezado
                    ft.Container(
                        content=ft.Text(
                            "Menú Digital",
                            size=30,
                            weight=ft.FontWeight.BOLD,
                            color="white"
                        ),
                        padding=20,
                        alignment=ft.Alignment(0, 0),
                        bgcolor="#1e293b"
                    ),

                    # Misma vista de ventas, pero en modo solo lectura
                    MenuClienteView(page, dm),

                    # Botón para regresar
                    ft.Container(
                        content=ft.ElevatedButton(
                            "Regresar",
                            icon=Icons.ARROW_BACK,
                            on_click=lambda e: mostrar_login()
                        ),
                        padding=20,
                        alignment=ft.Alignment(0, 0)
                    )
                ],
                expand=True
            )
        )

        page.update()

    # =========================================================
    # PANTALLA DE SELECCIÓN DE ROL
    # =========================================================
    def mostrar_login():
        page.clean()

        def seleccionar_rol(rol):
            if rol in ["Administrador", "Servidor", "Mesero"]:
                mostrar_sistema_completo()
            elif rol == "Cliente":
                mostrar_menu_cliente()

        page.add(
            ft.Container(
                content=ft.Column(
                    [
                        ft.Icon(
                            Icons.RESTAURANT_MENU,
                            size=100,
                            color="#38bdf8"
                        ),

                        ft.Text(
                            "Punto de venta",
                            size=40,
                            weight=ft.FontWeight.BOLD,
                            color="white"
                        ),

                        ft.Text(
                            "¿Quién desea iniciar sesión?",
                            size=20,
                            color="#cbd5e1"
                        ),

                        ft.Divider(
                            height=30,
                            color="transparent"
                        ),

                        ft.ElevatedButton(
                            "Administrador",
                            width=300,
                            height=50,
                            on_click=lambda e: seleccionar_rol("Administrador")
                        ),

                        ft.ElevatedButton(
                            "Servidor",
                            width=300,
                            height=50,
                            on_click=lambda e: seleccionar_rol("Servidor")
                        ),

                        ft.ElevatedButton(
                            "Mesero",
                            width=300,
                            height=50,
                            on_click=lambda e: seleccionar_rol("Mesero")
                        ),

                        ft.ElevatedButton(
                            "Cliente",
                            width=300,
                            height=50,
                            on_click=lambda e: seleccionar_rol("Cliente")
                        ),
                    ],
                    horizontal_alignment=ft.CrossAxisAlignment.CENTER,
                    alignment=ft.MainAxisAlignment.CENTER,
                    spacing=15
                ),
                expand=True,
                alignment=ft.Alignment(0, 0)
            )
        )

        page.update()

    # Mostrar pantalla inicial
    mostrar_login()


if __name__ == "__main__":
    ft.run(main)

```

### Menu_Clientes_view

```
# views/menu_cliente_view.py
#
# Vista del menú para clientes con imágenes de los platillos.

import flet as ft


def MenuClienteView(page: ft.Page, dm):
    # Lista de platillos con sus imágenes
    productos = [
        {
            "nombre": "Mole Poblano",
            "precio": 45.0,
            "imagen": "mole_poblano.jpg",
        },
        {
            "nombre": "Enchiladas Verdes",
            "precio": 35.0,
            "imagen": "enchiladas_verdes.jpg",
        },
        {
            "nombre": "Chilaquiles Rojos",
            "precio": 30.0,
            "imagen": "chilaquiles_rojos.jpg",
        },
        {
            "nombre": "Pozole Rojo",
            "precio": 50.0,
            "imagen": "pozole_rojo.png",
        },
        {
            "nombre": "Tlayuda Oaxaqueña",
            "precio": 55.0,
            "imagen": "tlayuda_oaxaquena.jpeg",
        },
        {
            "nombre": "Papa Asada",
            "precio": 100.0,
            "imagen": "papa_asada.jpg",
        },
        {
            "nombre": "Picada Verde",
            "precio": 25.0,
            "imagen": "picada_verde.jpeg",
        },
    ]

    cards = []

    for producto in productos:
        cards.append(
            ft.Container(
                width=250,
                bgcolor="#1e293b",
                border_radius=12,
                padding=10,
                content=ft.Column(
                    [
                        # Imagen del platillo
                        ft.Image(
                            src=producto["imagen"],
                            width=230,
                            height=150,
                            fit="cover",  # Compatible con cualquier versión
                            border_radius=10,
                        ),

                        # Nombre del platillo
                        ft.Text(
                            producto["nombre"],
                            size=18,
                            weight=ft.FontWeight.BOLD,
                            color="white",
                            text_align=ft.TextAlign.CENTER,
                        ),

                        # Precio
                        ft.Text(
                            f"${producto['precio']:.2f}",
                            size=22,
                            color="#38bdf8",
                            text_align=ft.TextAlign.CENTER,
                        ),
                    ],
                    horizontal_alignment=ft.CrossAxisAlignment.CENTER,
                    spacing=10,
                ),
            )
        )

    return ft.Container(
        expand=True,
        padding=20,
        content=ft.Column(
            [
                ft.Text(
                    "Consulte nuestro menú y realice su pedido con el mesero.",
                    size=18,
                    color="#cbd5e1",
                    text_align=ft.TextAlign.CENTER,
                ),

                ft.Divider(
                    height=20,
                    color="transparent",
                ),

                ft.Row(
                    cards,
                    wrap=True,
                    spacing=20,
                    run_spacing=20,
                    alignment=ft.MainAxisAlignment.CENTER,
                ),
            ],
            scroll=ft.ScrollMode.AUTO,
            horizontal_alignment=ft.CrossAxisAlignment.CENTER,
        ),
    )

```
